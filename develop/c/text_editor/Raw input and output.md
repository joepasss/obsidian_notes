 ### `Ctrl-Q` 키로 프로그램 종료하기

``` c
/*** includes ***/

/*** defines ***/
// 입력으로 받은 키(k)에 bitwise and 연산을 사용해서, 하위 5비트만 남기고 나머지는 모두 0으로 변환
#define CTRL_KEY(k) ((k) & 0x1f)
```

`ctrl + A` 를 입력하면 `65(0x41, 1000001)` 값이 출력되는게 아니라, `1(0x01, 0000001)` 값이 출력되는데, 이를 연산해주는 매크로 `0x1f`의 값이 `00011111`이기 때문에, 5개의 비트를 제외하고 모두 0으로 만들 수 있음  

입력된 값이 `17`일때 (`Ctrl-Q`) 프로그램을 종료하도록 수정

``` c
/*** init ***/

int main() {
  enableRawMode();

  while (1) {
    char c = '\0';
    if (read(STDIN_FILENO, &c, 1) == -1 && errno != EAGAIN) {
      die("read");
    };

    if (iscntrl(c)) {
      printf("%d\r\n", c);
    } else {
      printf("%d ('%c')\r\n", c, c);
    }

    // 입력된 값(c)이 17인 경우 프로그램을 종료함
    if (c == CTRL_KEY('q'))
      break;
  }

  return 0;
}
```

### `main` 함수에서 키 입력받는 부분 함수로 분리 & 입력받은 값 출력 제외
``` c
char editorReadKey() {
  int nread;
  char c;

  // read 함수는 실패시 -1 을 반환, 성공시 읽어들인 byte수를 반환 (0은 EOF)
  while ((nread = read(STDIN_FILENO, &c, 1)) != 1) {
    // read 함수가 실패했을때 die() 함수 호출
    if (nread == -1 && errno != EAGAIN) {
      die("read");
    }
  }

  return c;
}
```

``` c
char editorReadKey() {...}

/*** input ***/

void editorProcessKeyPress() {
  // 앞에서 작성한 editorReadKey() 함수를 호출해서 입력된 keypress 받아옴
  char c = editorReadKey();

  switch (c) {
  // 입력된 키가 CTRL-q 일 경우 종료
  case CTRL_KEY('q'):
    exit(0);
    break;
  }
}

/*** init ***/

int main() {
  enableRawMode();

  while (1) {
    // exit 하기 전 까지 반복
    editorProcessKeyPress();
  }

  return 0;
}
```

### 스크린 clear

``` c
/*** output ***/
// 터미널로 출력(STDOUT_FILENO), "\x1b[2J"(4바이트), 4바이트 출력(4)
void editorRefreshScreen() { (void)write(STDOUT_FILENO, "\x1b[2J", 4); }

/*** input ***/
/*** init ***/

int main() {
  enableRawMode();

  while (1) {
    editorRefreshScreen();
    editorProcessKeyPress();
  }
}
```

**터미널 이스케이프 시퀀스`\x1b`**
* `\x1b`는 `ESC`키를 의미 (`26 (0x1b)`으로 출력됨, ASCII 코드 기준)
* `\x1b[<parameters><command>`
* 터미널에서 특수한 작업을 수행하기 위해 사용되는 문자들의 조합
	* 화면 지우기 (`\x1b[2J`) 커서 이동 등 역할을 수행할 수 있음

[VT100](https://vt100.net/docs/vt100-ug/chapter3.html#ED)

### 커서 이동

``` c
/*** output ***/

void editorRefreshScreen() {
  (void)write(STDOUT_FILENO, "\x1b[2J", 4);
  // 커서를 맨 왼쪽 위로 이동함
  (void)write(STDOUT_FILENO, "\x1b[H", 3);
}
```

### 프로그램 종료할 때, screen clear & 커서 이동

``` c
/*** terminal ***/

void die(const char *s) {
  // 에러로 종료시, 스크린 clear & 커서 이동
  (void)write(STDOUT_FILENO, "\x1b[2J", 4);
  (void)write(STDOUT_FILENO, "\x1b[H", 3);

  perror(s);
  exit(1);
}


/*** input ***/

void editorProcessKeyPress() {
  char c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    // 정상적으로 종료시, 스크린 clear & 커서 이동
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;
  }
}
```

### column 에 `~` 그리기

``` c

/*** output ***/

void editorDrawRows() {
  int y;

  // column 이 24개라고 가정함
  for (y = 0; y < 24; y++) {
    // ~ 그리기
    (void)write(STDOUT_FILENO, "~\r\n", 3);
  }
}

void editorRefreshScreen() {
  (void)write(STDOUT_FILENO, "\x1b[2J", 4);
  (void)write(STDOUT_FILENO, "\x1b[H", 3);

  editorDrawRows();

  // 그리고 난 뒤, 커서 이동
  (void)write(STDOUT_FILENO, "\x1b[H", 3);
}
```

### `global state` 작성

터미널의 실제 사이즈를 가져와서 global state에 저장한 뒤 사용할 예정

``` c
/*** data ***/
// 이전에 struct termios orig_termios 를 editorConfig 구조체 내부에 포함시켰음
struct editorConfig {
  struct termios orig_termios;
};

struct editorConfig E;

// orig_termios 를 E.orig_termios 로 변경

void disableRawMode() {
  if (tcsetattr(STDIN_FILENO, TCSAFLUSH, &E.orig_termios)) {
    die("tcsetattr");
  }
}

void enableRawMode() {
  if (tcgetattr(STDIN_FILENO, &E.orig_termios)) {
    die("tcgetattr");
  }
  atexit(disableRawMode);

  struct termios raw = E.orig_termios;
	......
}
```

터미널의 사이즈를 가져오려면, `ioctl()`함수를 호출하면 된다 `ioctl(STDOUT_FILENO, TIOCGWINSZ, &ws)`

```c
#include <sys/ioctl.h>

int getWindowSize(int *rows, int *cols) {
  struct winsize ws;

  // ioctl(시스템 호출)을 사용하여 터미널의 크기를 가져오는데, ws구조체에 저장함
  if (ioctl(STDOUT_FILENO, TIOCGWINSZ, &ws) == -1 || ws.ws_col == 0) {
    return -1;
  } else {
    *cols = ws.ws_col;
    *rows = ws.ws_row;

    return 0;
  }
}
```

**`struct winsize`**

```c
struct winsize {
  unsigned short ws_row;     // 터미널의 row 수
  unsigned short ws_col;     // 터미널의 col 수
  unsigned short ws_xpixel;  // 터미널의 가로 크기 (픽셀 단위, 선택)
  unsigned short ws_ypixel;  // 터미널의 세로 크기 (픽셀 단위, 선택)
}
```

**`ioctl()`
`int ioctl(int fd, unsigned long request, ...);`
* `fd` : 파일 디스크럽터
* `request`: 어떤 작업을 할지 정의하는 명령

**`TIOCBWINSZ`**
* `Terminal IOCtl Get WINdow SiZe`
* 터미널의 윈도우 사이즈를 가져오는 명령

터미널의 사이즈를 가져왔으니, 전역에서 사용할 수 있도록 해 준다

``` c
/*** data ***/
// screenrows, screencols 필드 추가
struct editorConfig {
  int screenrows;
  int screencols;
  struct termios orig_termios;
};

/*** init ***/
// RawMode 진입 후, window size 가져오기
void initEditor() {
  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }
}

int main() {
  enableRawMode();
  initEditor();

  while (1) {
    editorRefreshScreen();
    editorProcessKeyPress();
  }

  return 0;
}
```

전역으로 사용할 수 있는 터미널의 사이즈가 있으므로, `editorDrawRows()` 함수에 하드코딩 해 놨던 row 값 수정

``` c
/*** output ***/

void editorDrawRows() {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    (void)write(STDOUT_FILENO, "~\r\n", 3);
  }
}
```

### `ioctl()` 함수가 작동하지 않을 때 fallback method 생성

커서를 맨 오른쪽 아래에 이동시킨 다음, (`\xb[999C`를 이용해서 최대한 오른쪽으로 이동, `x1b[999B` 를 이용해서 최대한 아래쪽으로 이동) 위치를 읽어와서 터미널의 크기를 추론

``` c
int getWindowSize(int *rows, int *cols) {
  struct winsize ws;

  // 테스트 목적으로 임시로 if 문에 1 추가했음
  if (1 || ioctl(STDOUT_FILENO, TIOCGWINSZ, &ws) == -1 || ws.ws_col == 0) {
    // 커서를 오른쪽 아래로 이동
    if (write(STDOUT_FILENO, "\x1b[999C\x1b[999B", 12) != 12) {
      return -1;
    }

    // 커서 위치를 알아내기 위해서 임시로 call 함
    editorReadKey();
    return -1;
  } else {
    *cols = ws.ws_col;
    *rows = ws.ws_row;

    return 0;
  }
}
```

`n` 커멘드 [device status report](https://vt100.net/docs/vt100-ug/chapter3.html#DSR)를 이용해서 커서의 정보를 가져온 뒤 터미널의 크기를 알아내기

```c
int getCursorPosition(int *rows, int *cols) {
  // 커서 위치를 요청하는 시퀀스 전달
  if (write(STDOUT_FILENO, "\x1b[6n", 4) != 4) {
    return -1;
  }

  printf("\r\n");

  char c;

  // 커서 포지션 출력
  // esc(27) + [ + 17;92R 등으로 출력됨
  while (read(STDIN_FILENO, &c, 1) == 1) {
    if (iscntrl(c)) {
      printf("%d\r\n", c);
    } else {
      printf("%d ('%c')\r\n", c, c);
    }
  }

  editorReadKey();

  return -1;
}

int getWindowSize(int *rows, int *cols) {
  struct winsize ws;

  if (1 || ioctl(STDOUT_FILENO, TIOCGWINSZ, &ws) == -1 || ws.ws_col == 0) {
    if (write(STDOUT_FILENO, "\x1b[999C\x1b[999B", 12) != 12) {
      return -1;
    }
    // return 값 수정 (editorReadKey() 함수 제거, return -1 에서 getCursorPosition 함수 호출로 변경)
    return getCursorPosition(rows, cols);
  } else {
    *cols = ws.ws_col;
    *rows = ws.ws_row;

    return 0;
  }
}
```

출력된 값 `ESC [ <커서의 행 번호>;<커서의 열 번호>R(이스케이프시퀀스 종료 플래그)` 을 parse 해야하므로, 값을 버퍼에 저장 (값이 `R`일 경우 break;)

``` c
int getCursorPosition(int *rows, int *cols) {
  char buf[32];
  unsigned int i = 0;

  if (write(STDOUT_FILENO, "\x1b[6n", 4) != 4) {
    return -1;
  }

  while (i < sizeof(buf) - 1) {
    // 1 바이트씩 읽어서 buf에 저장
    if (read(STDIN_FILENO, &buf[i], 1) != 1) {
      break;
    }

    // 읽은 값이 R 일 경우 break;
    if (buf[i] == 'R') {
      break;
    }

    i++;
  }

  // 문자열의 마지막을 'R'값에서 null 문자로 변경하여 C 문자열로 바꿈
  buf[i] = '\0';

  // buf[0]에 저장된 esc를 제외한 값 출력
  printf("\r\n&buf[1]: '%s'\r\n", &buf[1]);

  editorReadKey();

  return -1;
}
```

`sscanf()`를 이용해서 원하는 숫자 parse

``` c
int getCursorPosition(int *rows, int *cols) {
  char buf[32];
  unsigned int i = 0;

  if (write(STDOUT_FILENO, "\x1b[6n", 4) != 4) {
    return -1;
  }

  while (i < sizeof(buf) - 1) {
    if (read(STDIN_FILENO, &buf[i], 1) != 1) {
      break;
    }

    if (buf[i] == 'R') {
      break;
    }

    i++;
  }

  buf[i] = '\0';

  // buf 의 첫 요소가 esc 시퀀스가 아니거나 두 번째 요소가 '[' 가 아니면 에러
  if (buf[0] != '\x1b' || buf[1] != '[') {
    return -1;
  }

  // sscanf 함수를 이용해서 rows, cols 저장
  if (sscanf(&buf[2], "%d;%d", rows, cols) != 2) {
    return -1;
  }

  return 0;
}
```

**`sscanf()`**
* 문자열에서 데이터를 읽어 변수에 저장하는데 사용됨
* `int sscanf(const char *str, const char *format, ...)`

임시로 넣어놓은 `1` 삭제
``` c
int getWindowSize(int *rows, int *cols) {
  struct winsize ws;

  if (ioctl(STDOUT_FILENO, TIOCGWINSZ, &ws) == -1 || ws.ws_col == 0) {
    if (write(STDOUT_FILENO, "\x1b[999C\x1b[999B", 12) != 12) {
      return -1;
    }
    return getCursorPosition(rows, cols);
  } else {
    *cols = ws.ws_col;
    *rows = ws.ws_row;

    return 0;
  }
}
```

프로그램을 실행해 보면 마지막 라인에 `~`가 없는것을 볼 수 있는데, 마지막 row 일 때, `\r\n` 제거

``` c
void editorDrawRows() {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    // ~ 그리기
    (void)write(STDOUT_FILENO, "~", 1);

    // 마지막 줄이 아닐 때 \r\n
    if (y < E.screenrows - 1) {
      (void)write(STDOUT_FILENO, "\r\n", 22);
    }
  }
}
```

### 최적화 (append buffer)
지금 `write()`를 여러 번 하고 있는데 buffer 에 저장한 다음 한 번에 refresh 를 하게 함

**동적 버퍼 생성**
``` c
int getWindowSize(int *rows, int *cols) {....}

/*** append buffer ***/

struct abuf {
  char *b;
  int len;
};

// constructor 로 작동함
#define ABUF_INIT {NULL, 0}
```

**버퍼에 문자열 추가, 동적으로 할당된 버퍼 free 함수 작성**
```c
#include <string.h>

#define ABUF_INIT {NULL, 0}

// buffer 에 문자열 추가하는 함수
void abAppend(struct abuf *ab, const char *s, int len) {
  // 버퍼 재할당
  char *new = realloc(ab->b, ab->len + len);

  if (new == NULL)
    return;

  // 새로 할당된 버퍼에서 기존 데이터 뒤에 새 데이터를 복사
  memcpy(&new[ab->len], s, len);
  // 새로운 메모리를 ab->b 에 할당하여 버퍼 포인터를 업데이트
  ab->b = new;
  // 버퍼의 길이 증가시킴
  ab->len += len;
}

// 동적으로 할당된 버퍼(ab->b) free
void abFree(struct abuf *ab) { free(ab->b); }
```

**`realloc()`**
* `void *realloc(void *_Nullable ptr, size_t size);`
* `ptr` 이 가리키는 메모리 블록의 크기를 `size` 바이트로 변경함
	* `ptr` 이 `NULL` 일때, `malloc(size)`와 동일하게 동작 (새로운 메모리 할당을 수행)
	* `size`가 `0`일 경우 (그리고 `ptr`이 `NULL`이 아닐 경우), `free(ptr)`과 동일하게 동작 (`ptr`에 할당된 메모리 해제) 비호환성 문제가 있을 수 있기 때문에 `free()`함수를 직접 호출하는게 좋음
	* `ptr`은 이전에 `malloc()` 또는 관련 함수로부터 반환된 포인터여야함, 메모리 블록의 위치가 변경되었다면, 원래의 메모리 블록은 자동으로 `free(ptr)`로 헤제됨

**`memcpy()`**
* `void *memcpy(void dest[restrict .n], const void src[restrict .n], size_t n)`
* `restrict`
	* 포인터 최적화를 위한 힌트
	* 컴파일러에 포인터가 가리키는 메모리 영역이 서로 겹치지 않게 보장해주는 역할
* `n` 바이트를 `src`메모리 영역에서 읽어서, `dest` 메모리 영역으로 복사하는 역할
* 원본 데이터의 메모리 블록을 목적지 메모리 블록으로 그대로 복사하므로, 데이터의 구조나 타입과는 무관하게 사용 가능함.
* 바이트 단위로 메모리 복사를 수행하며, 문자열, 배열, 구조체 등의 복사에 유용

**버퍼를 사용해서 `write()`최적화
``` c
/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    abAppend(ab, "~", 1);

    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}

void editorRefreshScreen() {
  // ab 버퍼 초기화
  struct abuf ab = ABUF_INIT;

  // ab 에 "\x1b[2J", "\x1b[H" 추가
  abAppend(&ab, "\x1b[2J", 4);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);
  
  // ab 에 "\x1b[H" 추가
  abAppend(&ab, "\x1b[H", 3);

  // ab.b (작성하려는 string의 배열), ab.len (길이) 참고해서 write()
  // ab.b가 **`\x1b[2J\x1b[H~\r\n~\r\n~\x1b[H`** 이런형식으로 작성되어있음
  write(STDOUT_FILENO, ab.b, ab.len);
  // ab 버퍼 free
  abFree(&ab);
}
```

### repainting 할 때 커서 숨기기
`\x1b[?25l`으로 커서를 끌 수 있고, `\x1b[?25h`로 커서를 켤 수 있음 [set mode](https://vt100.net/docs/vt100-ug/chapter3.html#SM), [reset mode](https://vt100.net/docs/vt100-ug/chapter3.html#RM), [cursor enable mode](https://vt100.net/docs/vt510-rm/DECTCEM.html)

``` c
void editorRefreshScreen() {
  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[2J", 4);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  abAppend(&ab, "\x1b[H", 3);
  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}
```

### 한 번에 한 줄씩 clear
지금은 `\x1b[2J`이스케이프 시퀀스를 이용해서 화면 전체를 한 번에 지우는데 `x1b[K`를 이용해서 한 줄씩 지울 수 있음 [EL, Erase In Line](https://vt100.net/docs/vt100-ug/chapter3.html#EL)

``` c
/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    abAppend(ab, "~", 1);

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}

void editorRefreshScreen() {
  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  abAppend(&ab, "\x1b[H", 3);
  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}
```

### welcome message

``` c

/*** defines ***/

#define KILO_VERSION "0.0.1"

#define CTRL_KEY(k) ((k) & 0x1f)

/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
    // 화면의 3분의 1 지점일때, 
    if (y == E.screenrows / 3) {
      char welcome[80];
      // welcome 배열에 메시지 작성
      int welcomelen = snprintf(welcome, sizeof(welcome),
                                "Kilo editor --version %s", KILO_VERSION);

      // 메시지가 너무 길 경우 길이 수정
      if (welcomelen > E.screencols) {
        welcomelen = E.screencols;
      }

      abAppend(ab, welcome, welcomelen);
    } else {
      abAppend(ab, "~", 1);
    }

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}
```

**`int sprintf(char *restrict str, const char *restrict format, ...)`**
* 형식(`format`)을 지정하여, 문자열을 버퍼에 저장하는 역할
* 버퍼에 저장된 문자열의 길이(`NULL` 문자 제외)를 반환, 오류 발생시 `-1` 반환

**메시지 중앙 정렬**

``` c
/*** output ***/

void editorDrawRows(struct abuf *ab) {
  int y;

  for (y = 0; y < E.screenrows; y++) {
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

    abAppend(ab, "\x1b[K", 3);
    if (y < E.screenrows - 1) {
      abAppend(ab, "\r\n", 2);
    }
  }
}
```

### 유저 입력 받아서 커서 이동하기

**Cursor drawing**
지금까지는 `\x1b[H` 커멘드를 이용해서 커서를 그렸는데, 버퍼에 커서의 값을 저장한 다음 `\x1b[<y좌표>;<x좌표>H`를 이용해서 커서 drawing

``` c
/*** data ***/

struct editorConfig {
  // x, y 좌표 추가
  int cx, cy;
  int screenrows;
  int screencols;
  struct termios orig_termios;
};

struct editorConfig E;

....

/*** init ***/

void initEditor() {
  // x, y 좌표 0으로 init
  E.cx = 0;
  E.cy = 0;

  if (getWindowSize(&E.screenrows, &E.screencols) == -1) {
    die("getWindowSize");
  }
}

```

```c
void editorRefreshScreen() {
  struct abuf ab = ABUF_INIT;

  abAppend(&ab, "\x1b[?25l", 6);
  abAppend(&ab, "\x1b[H", 3);

  editorDrawRows(&ab);

  char buf[32];
  // snprintf 함수를 이용해서, x좌표, y좌표 buffer에 작성
  snprintf(buf, sizeof(buf), "\x1b[%d;%dH", E.cy + 1, E.cx + 1);
  // 커서 옮기기
  abAppend(&ab, buf, strlen(buf));

  abAppend(&ab, "\x1b[?25h", 6);

  write(STDOUT_FILENO, ab.b, ab.len);
  abFree(&ab);
}
```

**`snprintf()`**
* `int snprintf(char str[restrict .size], size_t size, const char *restrict format, ...);`
* `format`을 이용해서 buffer에 문자열을 저장해주는 역할
* 포멧팅된 결과 문자열의 길이를 반환함, 반환값은 버퍼의 크기를 초과하여 잘렸다고 해도 실제 포멧된 전체 문자열의 길이를 나타냄. 반환값을 통해서 문자열이 잘렸는지 확인할 수 있음

### 키 입력 받아서 커서 이동하기

**`hjkl` 키 입력 받았을때 이동하기**
``` c
/*** input ***/

void editorMoveCursor(char key) {
  switch (key) {
  // 키 입력이 'a' 일 때, cx 감소 (왼쪽으로 이동)
  case 'h':
    E.cx--;
    break;
  // 키 입력이 'd' 일 때, cx 증가 (오른쪽으로 이동)
  case 'l':
    E.cx++;
    break;
  // 키 입력이 'w' 일 때, cy 감소 (위쪽으로 이동)
  case 'k':
    E.cy--;
    break;
  // 키 입력이 's' 일 때, cy 증가 (아래로 이동)
  case 'j':
    E.cy++;
    break;
  }
}

void editorProcessKeyPress() {
  char c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case 'h':
  case 'j':
  case 'k':
  case 'l':
    editorMoveCursor(c);
    break;
  }
}
```

`editorReadKey()` 에서 입력을 받은 후, switch 문에서 `wsad` 키가 감지되면, `editorMoveCursor()`함수를 부른 뒤 loop 종료 loop 종료되었으니 그 다음 loop 돌입 (`editorRefreshScreen()`)

**`''` 와 `""` 차이**
* `''`
	* 문자 상수, 정수형 값으로 표현됨 (97)
* `""`
	* 문자열 상수, 문자 배열로 표현
	* 메모리상의 주소를 가리키는 포인터로 처리됨

**`hjkl` 키와 화살표 키로 커서 이동**

화살표 키는 각각 `\x1b`(이스케이프 시퀀스)와 `[`, `A B C D`값으로 표현되므로,

```c
/*** terminal ***/

void die(const char *s) { … }

void disableRawMode() { … }

void enableRawMode() { … }

char editorReadKey() {
  int nread;
  char c;

  while ((nread = read(STDIN_FILENO, &c, 1)) != 1) {
    if (nread == -1 && errno != EAGAIN) {
      die("read");
    }
  }

  // c가 이스케이프 시퀀스인 경우
  if (c == '\x1b') {
    char seq[3];

    if (read(STDIN_FILENO, &seq[0], 1) != 1) {
      return '\x1b';
    }

    if (read(STDIN_FILENO, &seq[1], 1) != 1) {
      return '\x1b';
    }

    if (seq[0] == '[') {
      switch (seq[1]) {
      case 'A':
        return 'k';
      case 'B':
        return 'j';
      case 'C':
        return 'l';
      case 'D':
        return 'h';
      }
    }

    return '\x1b';
  } else {
    // 이스케이프 시퀀스 아닌 경우 c 반환
    return c;
  }
}
```

**`enum` 작성**

``` c
/*** defines ***/

#define KILO_VERSION "0.0.1"

#define CTRL_KEY(k) ((k) & 0x1f)

enum editorKey {
  ARROW_LEFT = 'h',
  ARROW_RIGHT = 'l',
  ARROW_UP = 'k',
  ARROW_DOWN = 'j'
};

char editorReadKey() {
  int nread;
  char c;

  while ((nread = read(STDIN_FILENO, &c, 1)) != 1) {
    if (nread == -1 && errno != EAGAIN) {
      die("read");
    }
  }

  if (c == '\x1b') {
    char seq[3];

    if (read(STDIN_FILENO, &seq[0], 1) != 1) {
      return '\x1b';
    }

    if (read(STDIN_FILENO, &seq[1], 1) != 1) {
      return '\x1b';
    }

    if (seq[0] == '[') {
      switch (seq[1]) {
      case 'A':
        return ARROW_UP;
      case 'B':
        return ARROW_DOWN;
      case 'C':
        return ARROW_RIGHT;
      case 'D':
        return ARROW_LEFT;
      }
    }

    return '\x1b';
  } else {
    return c;
  }
}

/*** input ***/

void editorMoveCursor(char key) {
  switch (key) {
  case ARROW_LEFT:
    E.cx--;
    break;
  case ARROW_RIGHT:
    E.cx++;
    break;
  case ARROW_UP:
    E.cy--;
    break;
  case ARROW_DOWN:
    E.cy++;
    break;
  }
}

void editorProcessKeyPress() {
  char c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;
  }
}
```

**hjkl 키 겹치지 않도록 다른 값으로 수정**

```c
// 다른 키랑 겹치지 않게 충분히 큰 값 지정함 (1000, 1001, 1002, 1003)
enum editorKey { ARROW_LEFT = 1000, ARROW_RIGHT, ARROW_UP, ARROW_DOWN };

// enum 의 값이 char 에서 int 로 변경되었으므로,
int editorReadKey() {....} // char 에서 int로 변경

/*** input ***/

void editorMoveCursor(int key) {....} // char key 에서 int key 로 변경

void editorProcessKeyPress() {
  // char c 에서 int c 로 변경
  int c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  case ARROW_UP:
  case ARROW_DOWN:
  case ARROW_LEFT:
  case ARROW_RIGHT:
    editorMoveCursor(c);
    break;
  }
}
```

**cursor 가 스크린 밖으로 나가지 않도록 수정**
``` c
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
  case ARROW_DOWN:
    if (E.cy != E.screenrows - 1) {
      E.cy++;
    }
    break;
  }
}
```

### 특수 키 입력 처리

**`Page Up` `Page Down` 키**
`Page Up` 키는 `<ESC>[~5`, `Page Down` 키는 `<ESC>[~6` 이므로, enum 에 값을 추가한 다음 키가 입력되었을때 enum에 들어있는 값으로 처리

``` c
// PAGE_UP, PAGE_DOWN 추가
enum editorKey {
  ARROW_LEFT = 1000,
  ARROW_RIGHT,
  ARROW_UP,
  ARROW_DOWN,
  PAGE_UP,
  PAGE_DOWN
};

int editorReadKey() {
  int nread;
  char c;

  while ((nread = read(STDIN_FILENO, &c, 1)) != 1) {
    if (nread == -1 && errno != EAGAIN) {
      die("read");
    }
  }

  if (c == '\x1b') {
    char seq[3];

    if (read(STDIN_FILENO, &seq[0], 1) != 1) {
      return '\x1b';
    }

    if (read(STDIN_FILENO, &seq[1], 1) != 1) {
      return '\x1b';
    }

    if (seq[0] == '[') {
      if (seq[1] >= '0' && seq[1] <= '9') {
        if (read(STDIN_FILENO, &seq[2], 1) != 1) {
          return '\x1b';
        }

        // 입력된 key 가 \x1b[~5, 6 인 경우 PAGE_UP, PAGE_DOWN return
        if (seq[2] == '~') {
          switch (seq[1]) {
          case '5':
            return PAGE_UP;
          case '6':
            return PAGE_DOWN;
          }
        }
      } else {
        switch (seq[1]) {
        case 'A':
          return ARROW_UP;
        case 'B':
          return ARROW_DOWN;
        case 'C':
          return ARROW_RIGHT;
        case 'D':
          return ARROW_LEFT;
        }
      }
    }

    return '\x1b';
  } else {
    return c;
  }
}
```

**`Page Up` `Page Down` 키 입력되었을때, 커서 이동**

``` c
void editorProcessKeyPress() {
  int c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  // Page Up 키 입력시 커서 최 상단으로 이동
  // Page Down 키 입력시 커서 최 하단으로 이동
  case PAGE_UP:
  case PAGE_DOWN: {
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

**`HOME`, `END` 키 처리

`Home` 키는, `<esc>[1~`, `<esc>[7~`, `<esc>[H`, `<esc>OH` 넷 중 하나로 전달됨
`END`   키는, `<esc>[4~`, `<esc>[8~`, `<esc>[F`, `<esc>OF` 넷 중 하나로 전달됨

``` c
// HOME_KEY, END_KEY 추가
enum editorKey {
  ARROW_LEFT = 1000,
  ARROW_RIGHT,
  ARROW_UP,
  ARROW_DOWN,
  HOME_KEY,
  END_KEY,
  PAGE_UP,
  PAGE_DOWN
};

// HOME_KEY, END_KEY 식별
int editorReadKey() {
  int nread;
  char c;

  while ((nread = read(STDIN_FILENO, &c, 1)) != 1) {
    if (nread == -1 && errno != EAGAIN) {
      die("read");
    }
  }

  if (c == '\x1b') {
    char seq[3];

    if (read(STDIN_FILENO, &seq[0], 1) != 1) {
      return '\x1b';
    }

    if (read(STDIN_FILENO, &seq[1], 1) != 1) {
      return '\x1b';
    }

    if (seq[0] == '[') {
      if (seq[1] >= '0' && seq[1] <= '9') {
        if (read(STDIN_FILENO, &seq[2], 1) != 1) {
          return '\x1b';
        }

        if (seq[2] == '~') {
          switch (seq[1]) {
          case '1':
            return HOME_KEY;
          case '4':
            return END_KEY;
          case '5':
            return PAGE_UP;
          case '6':
            return PAGE_DOWN;
          case '7':
            return HOME_KEY;
          case '8':
            return END_KEY;
          }
        }
      } else {
        switch (seq[1]) {
        case 'A':
          return ARROW_UP;
        case 'B':
          return ARROW_DOWN;
        case 'C':
          return ARROW_RIGHT;
        case 'D':
          return ARROW_LEFT;
        case 'H':
          return HOME_KEY;
        case 'F':
          return END_KEY;
        }
      }
    } else if (seq[0] == 'O') {
      switch (seq[1]) {
      case 'H':
        return HOME_KEY;
      case 'F':
        return END_KEY;
      }
    }

    return '\x1b';
  } else {
    return c;
  }
}
```

**`HOME` `END`눌렀을 때 커서 이동**

``` c
void editorProcessKeyPress() {
  int c = editorReadKey();

  switch (c) {
  case CTRL_KEY('q'):
    (void)write(STDOUT_FILENO, "\x1b[2J", 4);
    (void)write(STDOUT_FILENO, "\x1b[H", 3);

    exit(0);
    break;

  // HOME_KEY 눌렀을 때, 왼쪽 끝으로 이동
  case HOME_KEY:
    E.cx = 0;
    break;

  // END_KEY 눌렀을 때, 오른쪽 끝으로 이동
  case END_KEY:
    E.cx = E.screencols - 1;
    break;

  case PAGE_UP:
  case PAGE_DOWN: {
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

**`DELETE` 키**

`delete` 키는 `<esc>[3~`으로 입력됨

``` c
// enum에 DEL_KEY 추가
enum editorKey {
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

int editorReadKey() {
  int nread;
  char c;

  while ((nread = read(STDIN_FILENO, &c, 1)) != 1) {
    if (nread == -1 && errno != EAGAIN) {
      die("read");
    }
  }

  if (c == '\x1b') {
    char seq[3];

    if (read(STDIN_FILENO, &seq[0], 1) != 1) {
      return '\x1b';
    }

    if (read(STDIN_FILENO, &seq[1], 1) != 1) {
      return '\x1b';
    }

    if (seq[0] == '[') {
      if (seq[1] >= '0' && seq[1] <= '9') {
        if (read(STDIN_FILENO, &seq[2], 1) != 1) {
          return '\x1b';
        }

        if (seq[2] == '~') {
          switch (seq[1]) {
          case '1':
            return HOME_KEY;
          // DEL_KEY 추가
          case '3':
            return DEL_KEY;
          case '4':
            return END_KEY;
          case '5':
            return PAGE_UP;
          case '6':
            return PAGE_DOWN;
          case '7':
            return HOME_KEY;
          case '8':
            return END_KEY;
          }
        }
      } else {
        switch (seq[1]) {
        case 'A':
          return ARROW_UP;
        case 'B':
          return ARROW_DOWN;
        case 'C':
          return ARROW_RIGHT;
        case 'D':
          return ARROW_LEFT;
        case 'H':
          return HOME_KEY;
        case 'F':
          return END_KEY;
        }
      }
    } else if (seq[0] == 'O') {
      switch (seq[1]) {
      case 'H':
        return HOME_KEY;
      case 'F':
        return END_KEY;
      }
    }

    return '\x1b';
  } else {
    return c;
  }
}
```


