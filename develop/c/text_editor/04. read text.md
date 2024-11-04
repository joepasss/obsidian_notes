### 버퍼에 text 저장한 후 표시하기

**텍스트를 저장할 `erow (editor row)` 구조체 선언**
``` c
/*** data ***/

// character data 와 배열의 길이를 저장
typedef struct erow {
  int size;
  char *chars;
} erow;

struct editorConfig {
  int cx, cy;
  int screenrows;
  int screencols;
  int numrows;
  erow row;
  struct termios orig_termios;
};

```

**텍스트 하드코딩 & 출력하기**

```c
#include <sys/types.h>

int getWindowSize(int *rows, int *cols) { .... } 

/*** file i/o ***/

void editorOpen() {
  char *line = "Hello world!";
  ssize_t linelen = 13;

  E.row.size = linelen;
  E.row.chars = malloc(linelen + 1);
  memcpy(E.row.chars, line, linelen);
  E.row.chars[linelen] = '\0';
  E.numrows = 1;
}

/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    if (y >= E.numrows) {
      if (y == E.screenrows / 3) {
        char welcome[80];
        int welcomelen = snprintf(welcome, sizeof(welcome),
                                  "Kilo editor --version %s", KILO_VERSION);

        if (welcomelen > E.screencols) {
          welcomelen = E.screencols;
        }

        int padding = (E.screencols - welcomelen) / 2;

        if (padding) {
          abAppend(ab, "~", 1);
          padding--;
        }

        while (padding--) {
          abAppend(ab, " ", 1);
        }

        abAppend(ab, welcome, welcomelen);
      } else {
        abAppend(ab, "~", 1);
      }
    } else {
      int len = E.row.size;

      if (len > E.screencols)
        len = E.screencols;

      abAppend(ab, E.row.chars, len);
    }

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}

int main() {
  enableRawMode();
  initEditor();
  editorOpen();

  while (1) {
    editorRefreshScreen();
    editorProcessKeyPress();
  }

  return 0;
}
```

### 파일 읽어오기

`fopen()`함수로 파일을 읽어온 다음, `erow`에 저장한 후 출력할 계획

``` c
// getline() 함수 오류 제거
// feature test macro
/*** includes ***/
#define _DEFAULT_SOURCE
#define _BSD_SOURCE
#define _GNU_SOURCE


/*** file i/o ***/

void editorOpen(char *filename) {
  // 읽기 모드로 인자로 전달된 파일을 열기
  FILE *fp = fopen(filename, "r");
  // 파일이 존재하지 않을때 종료
  if (!fp) {
    die("fopen");
  }

  char *line = NULL;
  // 할당된 메모리 크기 초기화
  size_t linecap = 0;
  // 읽어온 문자열의 길이 초기화
  ssize_t linelen;

  // 파일에서 한 줄 읽은뒤, 문자열을 line 버퍼에 저장하고 linecap 에 동적으로 할당된 배열의 크기를 저장함
  linelen = getline(&line, &linecap, fp);

  // getline 함수 성공시
  if (linelen != -1) {
    // 줄 바꿈 문자 '\n' '\r'제거
    while (linelen > 0 &&
           (line[linelen - 1] == '\n' || line[linelen - 1] == '\r')) {
      linelen--;
    }

    // 동적으로 할당된 버퍼의 크기에 비례해서 전역 버퍼 생성
    E.row.size = linelen;
    E.row.chars = malloc(linelen + 1);
    // 전역 버퍼 정보를 이용해서 동적 버퍼 생성
    memcpy(E.row.chars, line, linelen);
    // 맨 끝단에 EOF 추가 (C 문자열로 변환)
    E.row.chars[linelen] = '\0';
    // 한 줄만 읽었기 때문에 numrows = 1 할당
    E.numrows = 1;
  }

  // getline 함수로 할당된 메모리 해제
  free(line);
  // 파일 스트림 제거
  fclose(fp);
}

/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    if (y >= E.numrows) {
      // 입력된 파일이 없을때만 welcome message 출력하도록 변경
      if (E.numrows == 0 && y == E.screenrows / 3) {
        char welcome[80];
        int welcomelen = snprintf(welcome, sizeof(welcome),
                                  "Kilo editor --version %s", KILO_VERSION);

        if (welcomelen > E.screencols) {
          welcomelen = E.screencols;
        }

        int padding = (E.screencols - welcomelen) / 2;

        if (padding) {
          abAppend(ab, "~", 1);
          padding--;
        }

        while (padding--) {
          abAppend(ab, " ", 1);
        }

        abAppend(ab, welcome, welcomelen);
      } else {
        abAppend(ab, "~", 1);
      }
    } else {
      int len = E.row.size;

      if (len > E.screencols)
        len = E.screencols;

      abAppend(ab, E.row.chars, len);
    }

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}

int main(int argc, char *argv[]) {
  enableRawMode();
  initEditor();

  if (argc >= 2) {
    editorOpen(argv[1]);
  }

  while (1) {
    editorRefreshScreen();
    editorProcessKeyPress();
  }

  return 0;
}
```

**`Feature Test Macro`**
* 시스템에서 특정 기능을 활성화시키기 위한 전처리기 지시문
* 지금 사용된 `getline()`함수는 gnu 확장 기능을 사용하므로, `#define _GNU_SOURCE`가 필요
* `#define _DEFAULT_SOURCE`
	* `glibc`라이브러리에서 기본 기능을 활성화하는 매크로
* `#define _BSD_SOURCE`
	* BSD 계열에서 제공하는 기능을 활성화.
	* GNU C 라이브러리에서 BSD 시스템에 존재하는 여러 함수와 상수를 사용할 수 있게 함
	* `glibc` 2.19 이후로  `_DEFAULT_SOURCE`와 통합됨
* `#define _GNU_SOURCE`
	* GNU C 라이브러리의 모든 확장 기능을 사용할 수 있게 함
	* `POSIX`, `BSD`, `SUS` 와 같은 표준 뿐 아니라  GNU 에서 제공하는 모든 추가 기능이 활성화됨

**`FILE`**
* `FILE`구조체는 C 표준 라이브러리에서 파일 입출력을 관리하기 위해 사용되는 구조체
* 일반적으로 파일 디스크럽터, 파일 상태 플래그, 입출력 버퍼, 파일 포인터 위치, 파일 오류 상태, EOF 로 구성되어있음
* `fopen()`, `fgets()`, `fputs()`등 함수에서 사용됨

**`fopen()`**
* `FILE *fopen(const char *restrict pathname, const char *restrict mode);`
* `filename`
	* 열고자 하는 파일의 경로와 이름을 문자열로 전달함
* `mode`
	* 파일을 여는 모드(읽기, 쓰기, 추가 등)을 문자열로 지정

 **파일 열기 모드** (`mode`):

|모드|의미|파일이 존재하지 않을 때|파일이 존재할 때|
|---|---|---|---|
|`"r"`|읽기 전용|오류 반환|파일을 읽기 전용으로 열음|
|`"w"`|쓰기 전용|새 파일을 생성|기존 파일을 덮어씌움|
|`"a"`|추가 전용|새 파일을 생성|파일 끝에서부터 데이터 추가|
|`"r+"`|읽기/쓰기|오류 반환|파일을 읽기/쓰기로 열음|
|`"w+"`|읽기/쓰기|새 파일을 생성|기존 파일을 덮어씌움|
|`"a+"`|읽기/추가|새 파일을 생성|파일 끝에서부터 데이터 추가 가능, 읽기 가능|

**`getline()`**
* `ssize_t getline(char **restrict lineptr, size_t *restrict n, FILE *restrict stream)`
	* 표준 입력이나 파일로부터 한 줄의 텍스트를 읽어오는 함수 (c 표준 라이브러리가 아닌, POSIX에 포함된 함수)
	* `char **lineptr`
		* 읽어온 문자열을 저장할 포인터의 주소 (버퍼)
	* `size_t *n`
		* 현재 `lineptr`에 할당된 메모리 크기, 이 값을 참조하여 크기가 부족할 때, 자동으로 확장함
	* `FILE *stream`
		* 입력을 읽어올 파일 스트림, 파일 포인터(`FILE *fp`)나 표준 입력(`stdin`)전달 가능
* 성공 시, 읽어온 문자열의 길이(바이트 수)를 반환, (`"\n"` 포함)
* 실패 시, `-1`반환 `errno`변수를 통해 원인을 알 수 있음

**`int main(int argc, char *argv[])`**
* `argc`
	* argument count
	* 명령줄 인자의 갯수를 나타내는 변수
	* 최소 값은 1 (프로그램 자체의 이름이 첫 번째 인자로 포함됨)
	* `./program arg1 arg2 arg3` 일때 `argc = 4`
* `argv`
	* argument vector
	* 명령줄 인자들의 배열
	* `argv[0]`은 프로그램의 이름 또는 실행경로를 가리킴, `argv[1]`부터 실제 전달된 인자들이 저장됨

**파일에서 여러 줄 읽어들이기**

``` c
struct editorConfig {
  int cx, cy;
  int screenrows;
  int screencols;
  int numrows;
  erow *row;
  struct termios orig_termios;
};

int getWindowSize (int *rows, int *cols) {....}

/*** row operations ***/

void editorAppendRow(char *s, size_t len) {
  E.row = realloc(E.row, sizeof(erow) * (E.numrows + 1));

  int at = E.numrows;
  E.row[at].size = len;
  E.row[at].chars = malloc(len + 1);
  memcpy(E.row[at].chars, s, len);
  E.row[at].chars[len] = '\0';
  E.numrows++;
}

/*** file i/o ***/

void editorOpen(char *filename) {
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
}

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  E.numrows = 0;
  E.row = NULL;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }
}

```

### vertical scrolling

``` c
struct editorConfig {
  int cx, cy;
  // 유저의 스크롤 상태를 추적할 rowoff 변수 추가
  int rowoff;
  int screenrows;
  int screencols;
  int numrows;
  erow *row;
  struct termios orig_termios;
};

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  // rowoff 변수 초기화 (0 = top of the file)
  E.rowoff = 0;
  E.numrows = 0;
  E.row = NULL;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }
}
```

``` c
/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    // filerow 변수 초기화
    int filerow = y + E.rowoff;

    // filerow 가 실제 줄 수(E.numrows) 보다 크거나 같을 경우 화면에 빈 줄을 표시
    if (filerow >= E.numrows) {
      if (E.numrows == 0 && y == E.screenrows / 3) {
        char welcome[80];
        int welcomelen = snprintf(welcome, sizeof(welcome),
                                  "Kilo editor --version %s", KILO_VERSION);

        if (welcomelen > E.screencols) {
          welcomelen = E.screencols;
        }

        int padding = (E.screencols - welcomelen) / 2;

        if (padding) {
          abAppend(ab, "~", 1);
          padding--;
        }

        while (padding--) {
          abAppend(ab, " ", 1);
        }

        abAppend(ab, welcome, welcomelen);
      } else {
        abAppend(ab, "~", 1);
      }
    } else {
      // 파일의 실제 줄(filerow)을 화면에 출력
      int len = E.row[filerow].size;

      // column 이 부족할 경우 잘라냄
      if (len > E.screencols)
        len = E.screencols;

      // 그리기
      abAppend(ab, E.row[filerow].chars, len);
    }

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}
```

### scroll
**vertical scroll**

cursor 가 화면 밖으로 이동하면, `E.rowoff`변수를 수정해서, scroll 을 구현

```c
/*** output ***/

void editorScroll() {
  // 커서가 visible window 외부에 있는지 확인
  if (E.cy < E.rowoff) {
    // 커서버 visible window 외부에 있다면, rowoff 값을 조정해서, 스크롤
    // E.rowoff = top of screen
    E.rowoff = E.cy;
  }

  if (E.cy >= E.rowoff + E.screenrows) {
    E.rowoff = E.cy - E.screenrows + 1;
  } 
}

void editorRefreshScreen() {
  editorScroll();

  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  char buf[32];
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", E.cy + 1, E.cx + 1);
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}

void editorRefreshScreen() {
  editorScroll();

  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  char buf[32];
  // E.cy 커서 수정
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", (E.cy - E.rowoff) + 1, E.cx + 1);
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}

/*** input ***/

void editorMoveCursor(int key) {
  switch (key) {
  case ARROW_LEFT:
    if (E.cx != 0) {
      E.cx--;
    }
    break;
  case ARROW_RIGHT:
    if (E.cx != E.screencols - 1) {
      E.cx++;
    }
    break;
  case ARROW_UP:
    if (E.cy != 0) {
      E.cy--;
    }
    break;
  // screen bottom (E.screenrows) 초과해서 스크롤 가능하게, (파일의 끝 기준으로 변경)
  case ARROW_DOWN:
    if (E.cy != E.numrows) {
      E.cy++;
    }
    break;
  }
}
```

**horizontal scrolling**

``` c
struct editorConfig {
  int cx, cy;
  int rowoff;
  // column offset 전역 변수 생성
  int coloff;
  int screenrows;
  int screencols;
  int numrows;
  erow *row;
  struct termios orig_termios;
};

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  E.rowoff = 0;
  // coloff 변수 초기화
  E.coloff = 0;
  E.numrows = 0;
  E.row = NULL;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }
}
```

``` c
void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    int filerow = y + E.rowoff;

    if (filerow >= E.numrows) {
      if (E.numrows == 0 && y == E.screenrows / 3) {
        char welcome[80];
        int welcomelen = snprintf(welcome, sizeof(welcome),
                                  "Kilo editor --version %s", KILO_VERSION);

        if (welcomelen > E.screencols) {
          welcomelen = E.screencols;
        }

        int padding = (E.screencols - welcomelen) / 2;

        if (padding) {
          abAppend(ab, "~", 1);
          padding--;
        }

        while (padding--) {
          abAppend(ab, " ", 1);
        }

        abAppend(ab, welcome, welcomelen);
      } else {
        abAppend(ab, "~", 1);
      }
    } else {
      // coloff 만큼 값을 빼줌
      int len = E.row[filerow].size - E.coloff;

      // len 이 음수일 경우 0으로
      if (len < 0) {
        len = 0;
      }

      if (len > E.screencols) {
        len = E.screencols;
      }

      // chars[coloff]를 사용해서 coloff 부터의 문자열만 렌더링
      abAppend(ab, &E.row[filerow].chars[E.coloff], len);
    }

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}
```

``` c
/*** output ***/

void editorScroll() {
  if (E.cy < E.rowoff) {
    E.rowoff = E.cy;
  }

  if (E.cy >= E.rowoff + E.screenrows) {
    E.rowoff = E.cy - E.screenrows + 1;
  }

  // 커서가 화면 왼쪽으로 벗어났을 때, offset값을 수정
  if (E.cx < E.coloff) {
    E.coloff = E.cx;
  }

  // 커서가 화면 오른쪽으로 벗어났을 때, offset 값을 수정
  if (E.cx >= E.coloff + E.screencols) {
    E.coloff = E.cx - E.screencols + 1;
  }
}

void editorRefreshScreen() {
  editorScroll();

  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  char buf[32];
  // 커서 positioin 수정
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", (E.cy - E.rowoff) + 1, (E.cx - E.coloff) + 1);
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}

/*** input ***/

void editorMoveCursor(int key) {
  erow *row = (E.cy >= E.numrows) ? NULL : &E.row[E.cy];

  switch (key) {
  case ARROW_LEFT:
    if (E.cx != 0) {
      E.cx--;
    // 커서가 라인의 처음이고, 이전 row가 존재할 때, 커서를 그 이전 row 맨 끝으로 이동시킴
    } else if (E.cy > 0) {
      E.cy--;
      E.cx = E.row[E.cy].size;
    }
    break;
  // 오른쪽으로 scroll 가능하게 수정
  case ARROW_RIGHT:
  if (row && E.cx < row->size) {
    E.cx++;
  // row 구조체가 NULL 이 아니고, 커서가 row의 끝에 도달했을 때, 커서를 이후 row 첫번째로 이동
  } else if (row && E.cx == row->size) {
    E.cy++;
    E.cx = 0;
  }
  break;
  case ARROW_UP:
    if (E.cy != 0) {
      E.cy--;
    }
    break;
  case ARROW_DOWN:
    if (E.cy != E.numrows) {
      E.cy++;
    }
    break;
  }

  // 커서의 오른쪽 끝 제한
  // 길이가 긴 줄에서 짧은 줄로 이동할 떄, 커서를 그 라인의 끝으로 이동시킴
  row = (E.cy >= E.numrows) ? NULL : &E.row[E.cy];
  int rowlen = row ? row->size : 0;
  if (E.cx > rowlen) {
    E.cx = rowlen;
  }
}
```

### `Tab` 렌더링

```c
/*** data ***/

typedef struct erow {
  int size;
  // render의 문자열 길이를 지정할 field
  int rsize;
  char *chars;
  // tab 문자를 처리하기위한 field
  char *render;
} erow;

/*** row operations ***/

void editorUpdateRow(erow *row) {
  // 기존에 할당된 render 버퍼 해제
  free(row->render);
  // 새로운 메모리 할당
  // 원래 줄의 길이 + 1 (널 종료 문자를 추가하기 위함)
  row->render = malloc(row->size + 1);

  int j;
  int idx = 0;

  for (j = 0; j < row->size; j++) {
    // 원래 줄의 각 문자를 render 문자열에 복사
    row->render[idx++] = row->chars[j];
  }

  // 복사 완료 후, render 문자열의 마지막에 널 종료 문자를 추가해서 c문자열로 변환
  row->render[idx] = '\0';
  row->rsize = idx;
}

void editorAppendRow(char *s, size_t len) {
  E.row = realloc(E.row, sizeof(erow) * (E.numrows + 1));

  int at = E.numrows;
  E.row[at].size = len;
  E.row[at].chars = malloc(len + 1);
  memcpy(E.row[at].chars, s, len);
  E.row[at].chars[len] = '\0';

  // 각 필드 초기화 (rsize, render)
  E.row[at].rsize = 0;
  E.row[at].render = NULL;
  // editorUpdateRow 함수 호출
  editorUpdateRow(&E.row[at]);

  E.numrows++;
}

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    int filerow = y + E.rowoff;

    if (filerow >= E.numrows) {
      if (E.numrows == 0 && y == E.screenrows / 3) {
        char welcome[80];
        int welcomelen = snprintf(welcome, sizeof(welcome),
                                  "Kilo editor --version %s", KILO_VERSION);

        if (welcomelen > E.screencols) {
          welcomelen = E.screencols;
        }

        int padding = (E.screencols - welcomelen) / 2;

        if (padding) {
          abAppend(ab, "~", 1);
          padding--;
        }

        while (padding--) {
          abAppend(ab, " ", 1);
        }

        abAppend(ab, welcome, welcomelen);
      } else {
        abAppend(ab, "~", 1);
      }
    } else {
      // E.row[filerow].size 에서 rsize 로 변환
      int len = E.row[filerow].rsize - E.coloff;

      if (len < 0) {
        len = 0;
      }

      if (len > E.screencols) {
        len = E.screencols;
      }

      // E.row[filerow].chars 에서 render 로 전환
      abAppend(ab, &E.row[filerow].render[E.coloff], len);
    }

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}
```

**`tab` 문자를 공백으로 전환**

``` c
void editorUpdateRow(erow *row) {
  int tabs = 0;
  int j;

  // 탭 문자의 갯수를 셈
  for (j = 0; j < row->size; j++) {
    if (row->chars[j] == '\t') {
      tabs++;
    }
  }

  free(row->render);
  // tab 하나당 7개의 공간이 필요하므로, (\t 를 공백 8개로 변경) 재할당
  row->render = malloc(row->size + tabs * 7 + 1);

  int idx = 0;

  for (j = 0; j < row->size; j++) {
    if (row->chars[j] == '\t') {
      row->render[idx++] = ' ';

      while (idx % 8 != 0) {
        row->render[idx++] = ' ';
      }
    } else {
      row->render[idx++] = row->chars[j];
    }
  }

  row->render[idx] = '\0';
  row->rsize = idx;
}
```

**`tabstop` 글로벌 상수로 지정**

``` c
#define KILO_TAB_STOP 2

/*** row operations ***/

void editorUpdateRow(erow *row) {
  int tabs = 0;
  int j;

  for (j = 0; j < row->size; j++) {
    if (row->chars[j] == '\t') {
      tabs++;
    }
  }

  free(row->render);
  // 7 에서 KILO_TAB_STOP -1 로 변경
  row->render = malloc(row->size + tabs * (KILO_TAB_STOP - 1) + 1);

  int idx = 0;

  for (j = 0; j < row->size; j++) {
    if (row->chars[j] == '\t') {
      row->render[idx++] = ' ';

      // 8에서 KILO_TAB_STOP 으로 변경
      while (idx % KILO_TAB_STOP != 0) {
        row->render[idx++] = ' ';
      }
    } else {
      row->render[idx++] = row->chars[j];
    }
  }

  row->render[idx] = '\0';
  row->rsize = idx;
}
```

### 탭 커서 렌더링

tab과 같은 문자가 화면에서 여러 칸을 차지하는데, 커서의 위치가 이를 반영하지 못하고 있음 렌더링될 위치(E.rx)를 이용해서 tab문자와 같이 여러 칸을 차지하는 문자를 관리

``` c
struct editorConfig {
  int cx, cy;
  // 렌더링 위치 변수 추가
  int rx;
  int rowoff;
  int coloff;
  int screenrows;
  int screencols;
  int numrows;
  erow *row;
  struct termios orig_termios;
};

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  // 변수 초기화
  E.rx = 0;
  E.rowoff = 0;
  E.coloff = 0;
  E.numrows = 0;
  E.row = NULL;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }
}
```

`E.cx`로 렌더링 하던 값을 `E.rx`로 변경

``` c
/*** output ***/

void editorScroll() {
  E.rx = E.cx;

  if (E.cy < E.rowoff) {
    E.rowoff = E.cy;
  }

  if (E.cy >= E.rowoff + E.screenrows) {
    E.rowoff = E.cy - E.screenrows + 1;
  }

  if (E.rx < E.coloff) {
    E.coloff = E.rx;
  }

  if (E.rx >= E.coloff + E.screencols) {
    E.coloff = E.rx - E.screencols + 1;
  }
}


void editorRefreshScreen() {
  editorScroll();

  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  char buf[32];
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", (E.cy - E.rowoff) + 1,
           (E.rx - E.coloff) + 1);
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}
```

`E.rx` 값 계산

``` c
/*** row operations ***/

int editorRowCxToRx(erow *row, int cx) {
  int rx = 0;
  int j;

  for (j = 0; j < cx; j++) {
    // chars[j] 가 탭 문자일때,
    if (row->chars[j] == '\t') {
      // 다음 탭 스탑까지 이동
      rx += (KILO_TAB_STOP - 1) - (rx % KILO_TAB_STOP);
    }

    // 탭 문자가 아닐때, 1 추가
    rx++;
  }

  return rx;
}

/*** output ***/

void editorScroll() {
  // E.rx = 0 으로 초기화
  E.rx = 0;

  if (E.cy < E.numrows) {
    // rx 값 계산
    E.rx = editorRowCxToRx(&E.row[E.cy], E.cx);
  }

  if (E.cy < E.rowoff) {
    E.rowoff = E.cy;
  }

  if (E.cy >= E.rowoff + E.screenrows) {
    E.rowoff = E.cy - E.screenrows + 1;
  }

  if (E.rx < E.coloff) {
    E.coloff = E.rx;
  }

  if (E.rx >= E.coloff + E.screencols) {
    E.coloff = E.rx - E.screencols + 1;
  }
}
```

**`Page Up` `Page Down`키로 scroll**

``` c
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
    E.cx = E.screencols - 1;
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
```

**`END` 키 입력 받았을 때, 현재 row의 끝으로 이동**

``` c
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
  }
}
```

### status bar

`filename`, 현재 위치한 line 위치 등 보여주는 status bar 작성

**아래쪽에 status bar 를 표시할 공간 만들기**

``` c
void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    int filerow = y + E.rowoff;

    if (filerow >= E.numrows) {
      if (E.numrows == 0 && y == E.screenrows / 3) {
        char welcome[80];
        int welcomelen = snprintf(welcome, sizeof(welcome),
                                  "Kilo editor --version %s", KILO_VERSION);

        if (welcomelen > E.screencols) {
          welcomelen = E.screencols;
        }

        int padding = (E.screencols - welcomelen) / 2;

        if (padding) {
          abAppend(ab, "~", 1);
          padding--;
        }

        while (padding--) {
          abAppend(ab, " ", 1);
        }

        abAppend(ab, welcome, welcomelen);
      } else {
        abAppend(ab, "~", 1);
      }
    } else {
      int len = E.row[filerow].rsize - E.coloff;

      if (len < 0) {
        len = 0;
      }

      if (len > E.screencols) {
        len = E.screencols;
      }

      abAppend(ab, &E.row[filerow].render[E.coloff], len);
    }

    abAppend(ab, "\x1b[K", 3);
    // if (y < E.screenrows - 1) 삭제
    abAppend(ab, "\r\n", 2);
  }
}

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  E.rx = 0;
  E.rowoff = 0;
  E.coloff = 0;
  E.numrows = 0;
  E.row = NULL;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }

  // screenrow 1 감소
  E.screenrows -= 1;
}
```

**statusbar background color**

`<ESC>[7m`을 이용해서 색상을 invert 할 수 있다 (`<ESC>[m` 으로 되돌리기 가능)

``` c

void editorDrawStatusBar(struct abuf *ab) {
  // 색상 invert
  abAppend(ab, "\x1b[7m", 4);

  int len = 0;

  while (len < E.screencols) {
    // 빈 문자로 채움
    abAppend(ab, " ", 1);
    len++;
  }

  // 색상 원래대로 되돌리기
  abAppend(ab, "\x1b[m", 3);
}

void editorRefreshScreen() {
  editorScroll();

  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);
  // 함수 호출
  editorDrawStatusBar(&ab);

  char buf[32];
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", (E.cy - E.rowoff) + 1,
           (E.rx - E.coloff) + 1);
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}

```

**`filename` 디스플레이**

``` c
struct editorConfig {
  int cx, cy;
  int rx;
  int rowoff;
  int coloff;
  int screenrows;
  int screencols;
  int numrows;
  erow *row;
  // filename 저장할 변수 추가
  char *filename;
  struct termios orig_termios;
};


/*** file i/o ***/

void editorOpen(char *filename) {
  // 이전에 있던 filename free
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
}

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  E.rx = 0;
  E.rowoff = 0;
  E.coloff = 0;
  E.numrows = 0;
  E.row = NULL;
  // filename 변수 초기화
  E.filename = NULL;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }

  E.screenrows -= 1;
}

```

**`strdup()`**

`char *strdup(const char *s);`
* 새로운 string의 pointer를 반환 (s의 복사본)
* 주어진 문자열을 복사하여 새로 할당된 메모리에 저장한 뒤, 그 복사본에 대한 포인터를 반환하는 함수, 이후 반드시 메모리 해제를 수행해야함
* `malloc()` + `strcpy()` 함수와 동일한 역할을 수행함

**`statusbar` 에 `filename` 그리기**

``` c
void editorDrawStatusBar(struct abuf *ab) {
  abAppend(ab, "\x1b[7m", 4);

  char status[80];
  // snprintf 함수로 status에 filename - 파일 라인 수 출력하고 반환된 값을 이용해서 len 초기화
  int len = snprintf(status, sizeof(status), "%.20s - %d lines",
                     E.filename ? E.filename : "[No Name]", E.numrows);

  if (len > E.screencols) {
    len = E.screencols;
  }

  abAppend(ab, status, len);

  // 남은 자리 공백으로 채움
  while (len < E.screencols) {
    abAppend(ab, " ", 1);
    len++;
  }

  abAppend(ab, "\x1b[m", 3);
}
```

**`statusbar`에 현재 위치하고 있는 라인 그리기**

``` c
void editorDrawStatusBar(struct abuf *ab) {
  abAppend(ab, "\x1b[7m", 4);

  char status[80], rstatus[80];
  int len = snprintf(status, sizeof(status), "%.20s - %d lines",
                     E.filename ? E.filename : "[No Name]", E.numrows);
  int rlen = snprintf(rstatus, sizeof(rstatus), "%d/%d", E.cy + 1, E.numrows);

  if (len > E.screencols) {
    len = E.screencols;
  }

  abAppend(ab, status, len);

  while (len < E.screencols) {
    // 오른쪽에 정렬
    if (E.screencols - len == rlen) {
      abAppend(ab, rstatus, rlen);
      break;
    } else {
      abAppend(ab, " ", 1);
      len++;
    }
  }

  abAppend(ab, "\x1b[m", 3);
}
```

### status message

``` c
#include <time.h>
#include <stdarg.h>

struct editorConfig {
  int cx, cy;
  int rx;
  int rowoff;
  int coloff;
  int screenrows;
  int screencols;
  int numrows;
  erow *row;
  char *filename;
  // 상태 메시지를 저장할 배열
  char statusmsg[80];
  // 상태 메시지가 표시된 시간 기록
  time_t statusmsg_time;
  struct termios orig_termios;
};

void editorSetStatusMessage(const char *fmt, ...) {
  // 가변 인수를 처리하기 위한 변수
  va_list ap;
  va_start(ap, fmt);
  // 포멧 문자열에 맞게 인수를 결합하여 상태 메시지에 저장함
  vsnprintf(E.statusmsg, sizeof(E.statusmsg), fmt, ap);
  va_end(ap);
  // 현재 시간을 기록하여 메시지가 언제 표시되었는지 기록
  E.statusmsg_time = time(NULL);
}

/*** init ***/

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  E.rx = 0;
  E.rowoff = 0;
  E.coloff = 0;
  E.numrows = 0;
  E.row = NULL;
  E.filename = NULL;
  // statusmsg 변수 초기화
  E.statusmsg[0] = '\0';
  E.statusmsg_time = 0;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }

  E.screenrows -= 1;
}


int main(int argc, char *argv[]) {
  enableRawMode();
  initEditor();

  if (argc >= 2) {
    editorOpen(argv[1]);
  }

  editorSetStatusMessage("HELP: Ctrl-Q = quit");

  while (1) {
    editorRefreshScreen();
    editorProcessKeyPress();
  }

  return 0;
}
```

**`va_list`**
* 가변 인수 목록을 처리할 때 사용되는 데이터 타입.
* 함수의 인자 목록에 전달된 가변 인수를 관리하는데 사용되며, 이 변수를 통해 각 인수를 하나씩 접근할 수 있음
**`va_start(ap, fmt)`**
* 가변 인수 처리를 시작할때 사용되는 매크로
* `ap`는 `va_list`변수이고, `fmt`는 고정된 인수 중, 가변 인수 전에 있는 마지막 인수
* `ap`를 초기화하고, 그 이후로 인수 목록에 접근할 수 있도록 설정
**`vsnprintf()`**
* `vsnpritnf(char *str, size_t size, const char *format, va_list ap);`
* 가변 인수 목록을 사 용하여 포멧팅된 문자열을 버퍼에 저장하는 함수
* `str`
	* 포멧팅된 문자열이 저장될 버퍼
* `size`
	* 버퍼의 최대 크기
* `format`
	* 포멧 문자열 `%d %s` 등
* `ap`
	* 가변 인수 목록(`va_list`)
* `printf`처럼 작동하지만, 가변 인수를 처리
**`va_end`**
* 가변 인수 처리가 끝났음을 나타내고, 사용된 `va_list`를 정리하는 매크로
* `va_start`로 시작된 가변 인수 처리가 끝난 뒤에는 반드시 호출해야함

**status message draw**

``` c
void editorDrawStatusBar(struct abuf *ab) {
  abAppend(ab, "\x1b[7m", 4);

  char status[80], rstatus[80];
  int len = snprintf(status, sizeof(status), "%.20s - %d lines",
                     E.filename ? E.filename : "[No Name]", E.numrows);
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
  // 줄바꿈 추가
  abAppend(ab, "\r\n", 2);
}

// message 출력
void editorDrawMessageBar(struct abuf *ab) {
  abAppend(ab, "\x1b[K", 3);

  int msglen = strlen(E.statusmsg);

  if (msglen > E.screencols) {
    msglen = E.screencols;
  }

  if (msglen && time(NULL) - E.statusmsg_time < 5) {
    abAppend(ab, E.statusmsg, msglen);
  }
}

void editorRefreshScreen() {
  editorScroll();

  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);
  editorDrawStatusBar(&ab);
  // message 출력 함수 호출
  editorDrawMessageBar(&ab);

  char buf[32];
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", (E.cy - E.rowoff) + 1,
           (E.rx - E.coloff) + 1);
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}

void initEditor() {
  E.cx = 0;
  E.cy = 0;
  E.rx = 0;
  E.rowoff = 0;
  E.coloff = 0;
  E.numrows = 0;
  E.row = NULL;
  E.filename = NULL;
  E.statusmsg[0] = '\0';
  E.statusmsg_time = 0;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }

  // message 출력될 공간 확보
  E.screenrows -= 2;
}
```