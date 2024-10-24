``` c
void editorAppendRow(char *s, size_t len) {....}

void editorRowInsertChar(erow *row, int at, int c) {
  // 삽입 위치가 음수이거나, 행의 길이보다 큰 경우, 삽입 위치를 행의 끝 (row->size)로 조정
  if (at < 0 || at > row->size) {
    at = row->size;
  }

  // 기존의 row->chars 배열의 크기를 1만큼 늘리기 위해 메모리 재할당
  row->chars = realloc(row->chars, row->size + 2);
  // 삽입 위치에서부터 오른쪽으로 문자를 밀어내기 위해 메모리 블록을 이동시킴
  // &row->chars[at + 1] 삽입할 위치 다음부터 데이터를 이동
  // &row->chars[at] 삽입 위치에서 기존 문자 참조
  // ow->size - at + 1 이동해야할 문자의 길이를 나타냄, '\0' 을 포함시키기 위해서 1을 더해줌
  // 이 작업을 통해 at 위치 이후의 문자들이 오른쪽으로 한 칸씩 밀림
  memmove(&row->chars[at + 1], &row->chars[at], row->size - at + 1);
  // 행의 길이를 1 증가시킴
  row->size++;
  // at 위치에 새로 삽입할 문자를 저장
  row->chars[at] = c;

  // 행 update
  editorUpdateRow(row);
}
```

**`memmove`**
* `void *memmove(void *dest, const void *src, size_t n);`
* `dest`: 데이터를 복사할 대상 메모리의 시작 주소
* `src` 데이터를 복사할 원본 메모리의 시작 주소
* `n` 복사할 바이트 수
* `dest` 포인터를 반환함

```c
/*** editor operations ***/

void editorInsertChar(int c) {
  // 커서가 마지막줄에 있는 경우, 한 개의 row를 추가해줌
  if (E.cy == E.numrows) {
    editorAppendRow("", 0);
  }

  editorRowInsertChar(&E.row[E.cy], E.cx, c);
  E.cx++;
}

void editorProcessKeyPress() {
  int c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case HOME_KEY:
    E.cx = 0;
    break;

  case END_KEY:
    if (E.cy < E.numrows) {
      E.cx = E.row[E.cy].size;
    }
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
    if (c == PAGE_UP) {
      E.cy = E.rowoff;
    } else if (c == PAGE_DOWN) {
      E.cy = E.rowoff + E.screenrows - 1;

      if (E.cy > E.numrows) {
        E.cy = E.numrows;
      }
    }

    int times = E.screenrows;
    while (times--) {
      editorMoveCursor(c == PAGE_UP ? ARROW_UP : ARROW_DOWN);
    }

    break;
  }

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;

  // default state에서 editorInsertChar 함수 호출
  default:
    editorInsertChar(c);
    break;
  }
}
```

### 특수 문자 삽입 제한

`Backspace` 키나 `Enter` 키 등의 특수문자가 character로서 입력되지 않도록 제한

``` c
enum editorKey {
  // backspace 키 추가
  BACKSPACE = 127,
  ARROW_LEFT = 1000,
  ARROW_RIGHT,
  ARROW_UP,
  ARROW_DOWN,
  DEL_KEY,
  HOME_KEY,
  END_KEY,
  PAGE_UP,
  PAGE_DOWN
};

void editorProcessKeyPress() {
  int c = editorReadKey();

  switch (c) {
  // enter key
  case '\r':
    /* TODO */
    break;

  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case HOME_KEY:
    E.cx = 0;
    break;

  case END_KEY:
    if (E.cy < E.numrows) {
      E.cx = E.row[E.cy].size;
    }
    break;

  // backspce, delete key
  // Ctrl-H = 8, ASCII 코드에 따라 BS(Backspce)
  case BACKSPACE:
  case CTRL_KEY('h'):
  case DEL_KEY:
    /* TODO */
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
    if (c == PAGE_UP) {
      E.cy = E.rowoff;
    } else if (c == PAGE_DOWN) {
      E.cy = E.rowoff + E.screenrows - 1;

      if (E.cy > E.numrows) {
        E.cy = E.numrows;
      }
    }

    int times = E.screenrows;
    while (times--) {
      editorMoveCursor(c == PAGE_UP ? ARROW_UP : ARROW_DOWN);
    }

    break;
  }

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;

  // Ctrl-l (refresh)
  // Esc
  case CTRL('l'):
  case '\x1b':
    break;

  default:
    editorInsertChar(c);
    break;
  }
}
```

### 디스크에 저장

``` c
/*** file i/o ***/

// 편집기의 각 행을 하나의 큰 문자열로 변환
char *editorRowsToString(int *buflen) {
  int totlen = 0;
  int j;

  // 전체 길이 계산
  for (j = 0; j < E.numrows; j++) {
    // 각 행의 크기에 줄바꿈 문자를 더해서 총 길이를 구함
    totlen += E.row[j].size + 1;
  }

  *buflen = totlen;

  // 전체 문자열을 저장할 버퍼를 동적으로 할당
  char *buf = malloc(totlen);
  char *p = buf;

  // 문자열 복사
  for (j = 0; j < E.numrows; j++) {
    // 각 행의 내용을 복사
    memcpy(p, E.row[j].chars, E.row[j].size);
    p += E.row[j].size;
    // 줄바꿈 문자 추가
    *p = '\n';
    p++;
  }

  return buf;
}

// 현재 편집 중인 파일을 디스크에 저장하는 기능
void editorSave() {
  // 파일 이름이 없는 경우 return
  if (E.filename == NULL)
    return;

  int len;
  char *buf = editorRowsToString(&len);

  int fd = open(E.filename, O_RDWR | O_CREAT, 0644);
  ftruncate(fd, len);
  write(fd, buf, len);
  close(fd);
  free(buf);
}
```

**`CTRL-S` 입력되었을 때, `editorSave()` 함수 호출**

``` c
void editorProcessKeyPress() {
  int c = editorReadKey();

  switch (c) {
  case '\r':
    /* TODO */
    break;

  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  // ctrl + s 키 눌렀을 때, editorSave() 함수 호출
  case CTRL_KEY('s'):
    editorSave();
    break;

  case HOME_KEY:
    E.cx = 0;
    break;

  case END_KEY:
    if (E.cy < E.numrows) {
      E.cx = E.row[E.cy].size;
    }
    break;

  case BACKSPACE:
  case CTRL_KEY('h'):
  case DEL_KEY:
    /* TODO */
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
    if (c == PAGE_UP) {
      E.cy = E.rowoff;
    } else if (c == PAGE_DOWN) {
      E.cy = E.rowoff + E.screenrows - 1;

      if (E.cy > E.numrows) {
        E.cy = E.numrows;
      }
    }

    int times = E.screenrows;
    while (times--) {
      editorMoveCursor(c == PAGE_UP ? ARROW_UP : ARROW_DOWN);
    }

    break;
  }

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;

  case CTRL('l'):
  case '\x1b':
    break;

  default:
    editorInsertChar(c);
    break;
  }
}
```

**`editorSave()` 함수 에러 핸들링**

``` c
void editorSave() {
  if (E.filename == NULL)
    return;

  int len;
  char *buf = editorRowsToString(&len);

  int fd = open(E.filename, O_RDWR | O_CREAT, 0644);

  if (fd != -1) {
    if (ftruncate(fd, len) != -1) {
      if (write(fd, buf, len) == len) {
        close(fd);
        free(buf);
        return;
      }
    }

    close(fd);
  }

  free(buf);
}
```

**`editorSetStatusMessage()` 함수를 이용해서, save 성공했는지 아닌지 status 표시**

``` c
struct editorConfig E;

/*** prototypes ***/

// 프로토타입 추가
void editorSetStatusMessage(const char *fmt, ...);

void editorSave() {
  if (E.filename == NULL)
    return;

  int len;
  char *buf = editorRowsToString(&len);

  int fd = open(E.filename, O_RDWR | O_CREAT, 0644);

  if (fd != -1) {
    if (ftruncate(fd, len) != -1) {
      if (write(fd, buf, len) == len) {
        close(fd);
        free(buf);

        // 성공시 메시지 출력
        editorSetStatusMessage("%d bytes written to disk", len);
        return;
      }
    }

    close(fd);
  }

  free(buf);
  // 실패시 메시지 출력
  editorSetStatusMessage("Can't save! I/O error: %s", strerror(errno));
}

int main(int argc, char *argv[]) {
  enableRawMode();
  initEditor();

  if (argc >= 2) {
    editorOpen(argv[1]);
  }

  // help 메시지 추가
  editorSetStatusMessage("HELP: Ctrl-S = save | Ctrl-Q = quit");

  while (1) {
    editorRefreshScreen();
    editorProcessKeyPress();
  }

  return 0;
}
```

### 파일이 수정되었는지 확인하는 flag (dirty flag)

``` c
// modified statusmessage 추가
void editorDrawStatusBar(struct abuf *ab) {
  abAppend(ab, "\x1b[7m", 4);

  char status[80], rstatus[80];

  int len = snprintf(status, sizeof(status), "%.20s - %d lines %s",
                     E.filename ? E.filename : "[No Name]", E.numrows,
                     E.dirty ? "(modified)" : "");

  int rlen = snprintf(rstatus, sizeof(rstatus), "%d/%d", E.cy + 1, E.numrows);

  if (len > E.screencols) {
    len = E.screencols;
  }

  abAppend(ab, status, len);

  while (len < E.screencols) {
    if (E.screencols - len == rlen) {
      abAppend(ab, rstatus, rlen);
      break;
    } else {
      abAppend(ab, " ", 1);
      len++;
    }
  }

  abAppend(ab, "\x1b[m", 3);
  abAppend(ab, "\r\n", 2);
}

/*** row operations ***/

void editorAppendRow(char *s, size_t len) {
  E.row = realloc(E.row, sizeof(erow) * (E.numrows + 1));

  int at = E.numrows;
  E.row[at].size = len;
  E.row[at].chars = malloc(len + 1);
  memcpy(E.row[at].chars, s, len);
  E.row[at].chars[len] = '\0';

  E.row[at].rsize = 0;
  E.row[at].render = NULL;
  editorUpdateRow(&E.row[at]);

  E.numrows++;
  // dirty 값 증가
  E.dirty++;
}

void editorRowInsertChar(erow *row, int at, int c) {
  if (at < 0 || at > row->size) {
    at = row->size;
  }

  row->chars = realloc(row->chars, row->size + 2);
  memmove(&row->chars[at + 1], &row->chars[at], row->size - at + 1);
  row->size++;
  row->chars[at] = c;

  editorUpdateRow(row);
  // dirty 값 증가
  E.dirty++;
}

/*** editor operations ***/

/*** file i/o ***/
void editorOpen(char *filename) {
  free(E.filename);
  E.filename = strdup(filename);

  FILE *fp = fopen(filename, "r");
  if (!fp) {
    die("fopen");
  }

  char *line = NULL;
  size_t linecap = 0;
  ssize_t linelen;

  while ((linelen = getline(&line, &linecap, fp)) != -1) {
    while (linelen > 0 &&
           (line[linelen - 1] == '\n' || line[linelen - 1] == '\r')) {
      linelen--;
    }

    editorAppendRow(line, linelen);
  }

  free(line);
  fclose(fp);
  // dirty 값 초기화
  E.dirty = 0;
}

void editorSave() {
  if (E.filename == NULL)
    return;

  int len;
  char *buf = editorRowsToString(&len);

  int fd = open(E.filename, O_RDWR | O_CREAT, 0644);

  if (fd != -1) {
    if (ftruncate(fd, len) != -1) {
      if (write(fd, buf, len) == len) {
        close(fd);
        free(buf);

        // dirty 값 초기화
        E.dirty = 0;

        editorSetStatusMessage("%d bytes written to disk", len);
        return;
      }
    }

    close(fd);
  }

  free(buf);
  editorSetStatusMessage("Can't save! I/O error: %s", strerror(errno));
}
```

### 종료하기 전, confirmation

``` c
/*** define ***/
#define KILO_QUIT_TIMES 3

void editorProcessKeyPress() {
  static int quit_times = KILO_QUIT_TIMES;

  int c = editorReadKey();

  switch (c) {
  case '\r':
    /* TODO */
    break;

  case CTRL_KEY('q'):
    // 파일이 수정되었을 경우 confirmation 수행
    if (E.dirty && quit_times > 0) {
      editorSetStatusMessage("Warning!!! File has unsaved changes. "
                             "Press Ctrl-Q %d more times to quit.",
                             quit_times);
      quit_times--;
      return;
    }
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case CTRL_KEY('s'):
    editorSave();
    break;

  case HOME_KEY:
    E.cx = 0;
    break;

  case END_KEY:
    if (E.cy < E.numrows) {
      E.cx = E.row[E.cy].size;
    }
    break;

  case BACKSPACE:
  case CTRL_KEY('h'):
  case DEL_KEY:
    /* TODO */
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
    if (c == PAGE_UP) {
      E.cy = E.rowoff;
    } else if (c == PAGE_DOWN) {
      E.cy = E.rowoff + E.screenrows - 1;

      if (E.cy > E.numrows) {
        E.cy = E.numrows;
      }
    }

    int times = E.screenrows;
    while (times--) {
      editorMoveCursor(c == PAGE_UP ? ARROW_UP : ARROW_DOWN);
    }

    break;
  }

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;

  case CTRL('l'):
  case '\x1b':
    break;

  default:
    editorInsertChar(c);
    break;
  }

  // quit_times 초기화
  quit_times = KILO_QUIT_TIMES;
}
```

### delete charactor (`backspace`)구현

``` c
void editorRowDelChar(erow *row, int at) {
  if (at < 0 || at >= row->size)
    return;

  // row->chars[at] 에 있는 문자 삭제
  // at 위치의 문자를 제거하기 위해 그 뒤에 있는 모든 ㅁ누자를 한 칸씩 앞으로 옮김
  // abcde 이고 at이 2라면, chars[2]에는 문자 'c'가 있는데, 'c'를 덮어씌우기 위해, 'd', 'e'를 한 칸씩 앞으로 이동시켜, 'abde'문자열을 만듦
  memmove(&row->chars[at], &row->chars[at + 1], row->size - at);
  row->size--;
  editorUpdateRow(row);
  E.dirty++;
}

/*** editor operations ***/
void editorDelChar() {
  if (E.cy == E.numrows)
    return;

  erow *row = &E.row[E.cy];

  if (E.cx > 0) {
    editorRowDelChar(row, E.cx - 1);
    E.cx--;
  }
}


void editorProcessKeyPress() {
  static int quit_times = KILO_QUIT_TIMES;

  int c = editorReadKey();

  switch (c) {
  case '\r':
    /* TODO */
    break;

  case CTRL_KEY('q'):
    if (E.dirty && quit_times > 0) {
      editorSetStatusMessage("Warning!!! File has unsaved changes. "
                             "Press Ctrl-Q %d more times to quit.",
                             quit_times);
      quit_times--;
      return;
    }
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case CTRL_KEY('s'):
    editorSave();
    break;

  case HOME_KEY:
    E.cx = 0;
    break;

  case END_KEY:
    if (E.cy < E.numrows) {
      E.cx = E.row[E.cy].size;
    }
    break;

  case BACKSPACE:
  case CTRL_KEY('h'):
  case DEL_KEY:
    // DEL키 입력시 editorMoveCursor 로 커서 이동 후, editorDelChar 호출
    if (c == DEL_KEY)
      editorMoveCursor(ARROW_RIGHT);
    editorDelChar();
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
    if (c == PAGE_UP) {
      E.cy = E.rowoff;
    } else if (c == PAGE_DOWN) {
      E.cy = E.rowoff + E.screenrows - 1;

      if (E.cy > E.numrows) {
        E.cy = E.numrows;
      }
    }

    int times = E.screenrows;
    while (times--) {
      editorMoveCursor(c == PAGE_UP ? ARROW_UP : ARROW_DOWN);
    }

    break;
  }

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;

  case CTRL('l'):
  case '\x1b':
    break;

  default:
    editorInsertChar(c);
    break;
  }

  quit_times = KILO_QUIT_TIMES;
}
```

**줄 끝에서 `backspace` key 입력 처리**

``` c
// 메모리 누수 방지
void editorFreeRow(erow *row) {
  free(row->render);
  free(row->chars);
}

// 특정 위치의 행을 삭제하는 기능을 담당
void editorDelRow(int at) {
  // 인덱스 범위 검사 수행 
  // at 이 0보다 작거나, row보다 크거나 같으면, return
  if (at < 0 || at >= E.numrows)
    return;

  // 동적할당된 메모리 free
  editorFreeRow(&E.row[at]);
  // 삭제된 행 이후의 모든 행을 한 칸씩 옮김
  memmove(&E.row[at], &E.row[at + 1], sizeof(erow) * (E.numrows - at - 1));
  // numrow 감소
  E.numrows--;
  // Dirty값 증가
  E.dirty++;
}

// 특정 행(row)에 문자열을 추가하는 기능
void editorRowAppendString(erow *row, char *s, size_t len) {
  // chars 배열에 새로운 문자열을 추가하기 위해 메모리 재할당
  row->chars = realloc(row->chars, row->size + len + 1);
  // 문자열 복사 (새로 추가할 문자열 s를 행의 기존 문자열 뒤에 붙임)
  memcpy(&row->chars[row->size], s, len);
  row->size += len;
  row->chars[row->size] = '\0';
  editorUpdateRow(row);
  E.dirty++;
}
```

**커서가 줄 끝에 있지 않을때, 처리**

``` c
void editorDelChar() {
  // 커서가 줄 끝에 있을 때 return
  if (E.cy == E.numrows)
    return;
  // 커서가 첫 번째 줄 처음에 위치할 때 return
  if (E.cx == 0 && E.cy == 0)
    return;

  erow *row = &E.row[E.cy];

  // 커서가 첫 번째가 아닐 때, editorRowDelChar 호출해서 char 삭제
  if (E.cx > 0) {
    editorRowDelChar(row, E.cx - 1);
    E.cx--;
  // 커서가 첫 번째에 있을 때, row 삭제한 후, 커서 이동
  } else {
    E.cx = E.row[E.cy - 1].size;
    editorRowAppendString(&E.row[E.cy - 1], row->chars, row->size);
    editorDelRow(E.cy);
    E.cy--;
  }
}
```

### `Enter` key

**`editorAppendRow` 함수 수정**

``` c
// editorAppendRow 함수 수정
void editorInsertRow(int at, char *s, size_t len) {
  // at validate
  // at이 0보다 작거나, at이 numrows 보다 큰 경우 Return
  // 0보다 작으면, 파일의 가장 처음에 있다는 뜻
  // numrows 보다 크다면, 배열 크기보다 큰 인덱스에 접근하는 것
  if (at < 0 || at > E.numrows)
    return;

  // 배열 크기 재할당
  E.row = realloc(E.row, sizeof(erow) * (E.numrows + 1));
  // at 위치 이후의 모든 행을 한 칸씩 뒤로 이동시킴, 삽입하려는 위치에 빈 자리를 확보
  memmove(&E.row[at + 1], &E.row[at], sizeof(erow) * (E.numrows - at));

  E.row[at].size = len;
  E.row[at].chars = malloc(len + 1);
  memcpy(E.row[at].chars, s, len);
  E.row[at].chars[len] = '\0';

  E.row[at].rsize = 0;
  E.row[at].render = NULL;
  editorUpdateRow(&E.row[at]);

  E.numrows++;
  E.dirty++;
}

/*** editor operations ***/

void editorInsertChar(int c) {
  if (E.cy == E.numrows) {
    editorInsertRow(E.numrows, "", 0);
  }

  editorRowInsertChar(&E.row[E.cy], E.cx, c);
  E.cx++;
}

/*** file i/o ***/
void editorOpen(char *filename) {
  free(E.filename);
  E.filename = strdup(filename);

  FILE *fp = fopen(filename, "r");
  if (!fp) {
    die("fopen");
  }

  char *line = NULL;
  size_t linecap = 0;
  ssize_t linelen;

  while ((linelen = getline(&line, &linecap, fp)) != -1) {
    while (linelen > 0 &&
           (line[linelen - 1] == '\n' || line[linelen - 1] == '\r')) {
      linelen--;
    }

    editorInsertRow(E.numrows, line, linelen);
  }

  free(line);
  fclose(fp);
  E.dirty = 0;
}
```

**`newline` 생성**

``` c
/*** editor operations ***/
void editorInsertChar(int c) {....}

void editorInsertNewline() {
  if (E.cx == 0) {
    editorInsertRow(E.cy, "", 0);
  } else {
    erow *row = &E.row[E.cy];
    editorInsertRow(E.cy + 1, &row->chars[E.cx], row->size - E.cx);
    row = &E.row[E.cy];
    row->size = E.cx;
    row->chars[row->size] = '\0';
    editorUpdateRow(row);
  }

  E.cy++;
  E.cx = 0;
}

void editorProcessKeyPress() {
  static int quit_times = KILO_QUIT_TIMES;

  int c = editorReadKey();

  switch (c) {
  case '\r':
    // 함수 호출
    editorInsertNewline();
    break;

  case CTRL_KEY('q'):
    if (E.dirty && quit_times > 0) {
      editorSetStatusMessage("Warning!!! File has unsaved changes. "
                             "Press Ctrl-Q %d more times to quit.",
                             quit_times);
      quit_times--;
      return;
    }
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case CTRL_KEY('s'):
    editorSave();
    break;

  case HOME_KEY:
    E.cx = 0;
    break;

  case END_KEY:
    if (E.cy < E.numrows) {
      E.cx = E.row[E.cy].size;
    }
    break;

  case BACKSPACE:
  case CTRL_KEY('h'):
  case DEL_KEY:
    if (c == DEL_KEY)
      editorMoveCursor(ARROW_RIGHT);
    editorDelChar();
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
    if (c == PAGE_UP) {
      E.cy = E.rowoff;
    } else if (c == PAGE_DOWN) {
      E.cy = E.rowoff + E.screenrows - 1;

      if (E.cy > E.numrows) {
        E.cy = E.numrows;
      }
    }

    int times = E.screenrows;
    while (times--) {
      editorMoveCursor(c == PAGE_UP ? ARROW_UP : ARROW_DOWN);
    }

    break;
  }

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;

  case CTRL('l'):
  case '\x1b':
    break;

  default:
    editorInsertChar(c);
    break;
  }

  quit_times = KILO_QUIT_TIMES;
}
```

### 다른 이름으로 저장하기

**새로운 파일을 저장할 때, 문자열을 입력받는 함수 작성**

``` c
/*** prototypes ***/

void editorRefreshScreen();

/*** input ***/

char *editorPrompt(char *prompt) {
  // 입력받을 문자열의 크기 지정
  size_t bufsize = 128;
  // *buf 는 malloc 을 사용해서, 128바이트의 동적 메모리를 할당
  // 사용자 입력을 저장할 문자열
  char *buf = malloc(bufsize);

  // 현재 입력된 문자열의 길이
  size_t buflen = 0;
  // buf를 빈 문자열로 초기화
  buf[0] = '\0';

  while (1) {
    // 상태 바에, 입력 프롬프트와 현재 입력된 문자열(buf)를 표시함
    // prompt는 호출 시 넘겨주는 포멧 문자열임
    editorSetStatusMessage(prompt, buf);
    // 화면 새로고침
    editorRefreshScreen();

    // 사용자가 입력한 키를 받아옴
    int c = editorReadKey();
    // Enter key 입력 시
    if (c == '\r') {
      // 현재 입력된 문자열이 0이 아닐때,
      if (buflen != 0) {
        // 상태 바를 비워서 더 이상 프롬프트가 표시되지 않게 함
        editorSetStatusMessage("");
        // 입력된 문자열 반환하고 함수 종료
        return buf;
      }
    // 사용자가 입력한 키가 제어 문자가 아니고, c 값이 128 미만일 경우 (일반 문자열인 경우)
    } else if (!iscntrl(c) && c < 128) {
      // 문자의 개수가 buflen 을 초과할 때, realloc()을 사용해서 buf 크기 조정
      if (buflen == bufsize - 1) {
        // buf size 두 배 증가시킴
        bufsize *= 2;
        // realloc 으로 버퍼 재할당
        buf = realloc(buf, bufsize);
      }

      // 새로운 문자를 buf에 추가,
      // buflen 을 증가시킴
      buf[buflen++] = c;
      // buf 끝에 \0을 추가해서 c 스타일의 문자열의 끝을 표시함
      buf[buflen] = '\0';
    }
  }
}
```

**`E.filename` 값이 `NULL`일 때, prompt 띄우기**

```c
/*** prototypes ***/

void editorSetStatusMessage(const char *fmt, ...);
void editorRefreshScreen();
// prototype 추가
char *editorPrompt(char *prompt);

/*** File i/o ***/

void editorSave() {
  if (E.filename == NULL) {
    E.filename = editorPrompt("Save as: %s");
  }

  int len;
  char *buf = editorRowsToString(&len);

  int fd = open(E.filename, O_RDWR | O_CREAT, 0644);

  if (fd != -1) {
    if (ftruncate(fd, len) != -1) {
      if (write(fd, buf, len) == len) {
        close(fd);
        free(buf);

        E.dirty = 0;

        editorSetStatusMessage("%d bytes written to disk", len);
        return;
      }
    }

    close(fd);
  }

  free(buf);
  editorSetStatusMessage("Can't save! I/O error: %s", strerror(errno));
}
```

**`prompt` 에서 `esc` 키 입력받았을 때, input prompt 제거**

``` c
/*** file i/o ***/

void editorSave() {
  if (E.filename == NULL) {
    E.filename = editorPrompt("Save as: %s (ESC to cancel)");

    if (E.filename == NULL) {
      editorSetStatusMessage("save aborted");
      return;
    }
  }

  int len;
  char *buf = editorRowsToString(&len);

  int fd = open(E.filename, O_RDWR | O_CREAT, 0644);

  if (fd != -1) {
    if (ftruncate(fd, len) != -1) {
      if (write(fd, buf, len) == len) {
        close(fd);
        free(buf);

        E.dirty = 0;

        editorSetStatusMessage("%d bytes written to disk", len);
        return;
      }
    }

    close(fd);
  }

  free(buf);
  editorSetStatusMessage("Can't save! I/O error: %s", strerror(errno));
}

/*** input ***/

char *editorPrompt(char *prompt) {
  size_t bufsize = 128;
  char *buf = malloc(bufsize);

  size_t buflen = 0;
  buf[0] = '\0';

  while (1) {
    editorSetStatusMessage(prompt, buf);
    editorRefreshScreen();

    int c = editorReadKey();

    // esc 키 입력일때, input prompt cancel
    if (c == '\x1b') {
      editorSetStatusMessage("");
      free(buf);
      return NULL;
    } else if (c == '\r') {
      if (buflen != 0) {
        editorSetStatusMessage("");
        return buf;
      }
    } else if (!iscntrl(c) && c < 128) {
      if (buflen == bufsize - 1) {
        bufsize *= 2;
        buf = realloc(buf, bufsize);
      }

      buf[buflen++] = c;
      buf[buflen] = '\0';
    }
  }
}
```

**`prompt` 에서 `Backspce(CTRL-H, Delete)` 키 입력 받기**

``` c
/*** input ***/

char *editorPrompt(char *prompt) {
  size_t bufsize = 128;
  char *buf = malloc(bufsize);

  size_t buflen = 0;
  buf[0] = '\0';

  while (1) {
    editorSetStatusMessage(prompt, buf);
    editorRefreshScreen();

    int c = editorReadKey();

    // backspace 입력 받았을 때, buflen 이 0이 아닌지 확인하고, 0이 아닐 경우 buflen 의 길이를 1 줄인다음 맨 끝 char 을 '\0'으로 대체
    if (c == DEL_KEY || c == CTRL_KEY('h') || c == BACKSPACE) {
      if (buflen != 0)
        buf[--buflen] = '\0';
    } else if (c == '\x1b') {
      editorSetStatusMessage("");
      free(buf);
      return NULL;
    } else if (c == '\r') {
      if (buflen != 0) {
        editorSetStatusMessage("");
        return buf;
      }
    } else if (!iscntrl(c) && c < 128) {
      if (buflen == bufsize - 1) {
        bufsize *= 2;
        buf = realloc(buf, bufsize);
      }

      buf[buflen++] = c;
      buf[buflen] = '\0';
    }
  }
}

```

