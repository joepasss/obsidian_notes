### 유저의 keypress 입력 받아오기

``` c
#include <unistd.h>

int main() {
	char c;
	// 표준 출력으로부터 한 글자씩 계속 읽다가 더 이상 데이터가 없다면 (EOF) read 함수가 0을 반환하고 프로그램은 루프를 종료함
	while (read(STDIN_FILENO, &c, 1) == 1) {};
	return 0;
}
```

`read()` 함수
``` c
ssize_t read(int fd, void *buf, size_t count);
```

* `int fd` 읽을 파일의 디스크럽터, `STDIN_FILENO`는 표준 입력을 나타내는 파일 디스크럽터, `fd`의 `0`이 전달됨
* `buf` 데이터를 저장할 메모리 공간, `char` 타입 변수 `c`의 주소를 전달하여 읽은 문자를 `c`에 저장함
*  `count` 읽고자 하는 데이터의 크기(바이트) 수, 1을 전달하여 한 번에 1바이트를 읽음
* 이후 읽은 바이트 수를 반환 `0`반환시, 파일의 끝을 의미 (`EOF`) 더 이상 읽을 데이터가 없음을 나타내고 `-1` 반환시 읽기 오류가 발생했음을 나타냄

`STDIN_FILENO`
* 표준 입력을 가리키는 파일 디스트럽터, 항상 값이 0임
* `UNIX` 계열 시스템에서 프로그램이 시작될 때 다음과 같은 파일 디스크럽터가 열려 있음
	* `STDIN_FILENO`: `0` 표준 입력 (키보드 입력)
	* `STDOUT_FILENO`: `1` 표준 출력 (터미널 출력)
	* `STDERR_FILENO`: `2` 표준 에러 출력 (에러 메시지 출력)

**프로그램을 종료하려면,**
`CTRL-D` 를 이용해서, read()함수에 EOF를 전달해 주는 방법이 있고,  `CTRL-C`를 이용해서, process 종료 시그널을 보내는 방법으로 종료가 가능함

**`canonical mode(cooked mode)`**
`cannoical mode` 에서는 사용자의 키보드 입력이 `enter` 키를 통해서만 전달이 되는데, 지금 만드려고 하는 text-editor에서는 맞지 않음 그래서 `raw mode`로 변환하는게 필요함

**특정 키(q)를 눌러서 프로그램을 종료하기**
 `raw mode`에 진입하기 전에, 특정 키를 눌러서 프로그램을 종료하는 방법을 먼저 implement 함
 
``` c
#include <unistd.h>

int main() {
  char c;
  while (read(STDIN_FILENO, &c, 1) == 1 && c != 'q') {
  };
  return 0;
}
```

이렇게 작성한 후, `q`키 + `enter` 를 누르면 프로그램이 종료되는걸 볼 수 있음

### `raw mode` 진입

``` c
#include <stdlib.h>
#include <termios.h>
#include <unistd.h>

struct termios orig_termios;

void disableRawMode() {
  // 기존 설정으로 되돌림 (orig_termios 에 저장된 설정 적용)
  tcsetattr(STDIN_FILENO, TCSAFLUSH, &orig_termios);
}

// raw mode 로 전환
void enableRawMode() {
  // 현재 터미널의 속성을 가져와서 orig_termios 에 저장
  tcgetattr(STDIN_FILENO, &orig_termios);
  // 프로그램이 종료될 때 disableRawMode 함수 호출
  atexit(disableRawMode);

  // 새로운 raw 구조체 생성
  struct termios raw = orig_termios;
  // ECHO: 키 입력을 터미널에 표시하지 않도록 함
  // ICANNON: 캐노니컬 모드 해제 (Enter 키 누르기 전 까지 입력을 버퍼링 하지 않고, 바로 처리함)
  raw.c_lflag &= ~(ECHO | ICANON);

  // 변경된 설정 적용
  tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
}

int main() {
  enableRawMode();

  char c;
  while (read(STDIN_FILENO, &c, 1) == 1 && c != 'q') {
  };
  return 0;
}
```

* **`termios` 구조체**
	* 터미널 인터페이스를 제어하는데 사용되는 구조체
	* `c_iflag`(input flag)
		* 입력 모드에서 동작을 제어하는 플래그
		* `IGNBRK`: 브레이크 신호 무시
		* `ICRNL`: CR(`\r`)을 NR(`\n`)으로 변경
		* `IXON`: XON/XOFF 흐름 제어를 활성화
		* `BRKINT`: 브레이크 신호가 입력되면, 프로그램을 인터럽트
	* `c_oflag` (output flag)
		* 출력 모드에서 동작을 제어하는 플래그
		* `OPOST`: 출력을 처리할 때, 터미널의 기본 처리 규칙 적용
	* `c_cflag` (control flag)
		* 제어 모드에서 동작을 제어하는 플래그, 데이터 전송 속도, 패리티 비트, 문자 크기 등 제어
		* `CS8`: 8비트 문자 사용
		* `CLOCAL`: 모뎀 제어 신호를 무시
		* `CREAD`: 입력을 활성화
		* `PARENB`: 패리티 비트를 활성화
	* `c_lflag` (local flag)
		* 로컬 모드에서 터미널의 동작을 제어하는 플래그, 캐노니컬 모드, 에코, 시그널 처리 등을 제어
		* `ECHO`: 키 입력이 터미널에 표시되도록 함
		* `ICANNON`: 캐노니컬 모드 활성화
		* `ISIG`: SIGINT, SIGQUIT 등 시그널 허용
	* `c_cc` (control characters)
		* 특수 제어 문자를 정의하는 배열, 파일 끝(`EOF`), 중단(`INTR`), 중지(`SUSP`) 같은 제어 문자를 설정 가능
		* `VEOF`: `EOF` 제어 문자 (`CTRL + D`)
		* `VINTR`: 인터럽트 제어 문자 (`CTRL + C`)
		* `VSUSP`: 중지 제어 문자 (`CTRL + Z`)
	* `c_ispeed` (input speed)
		* 입력 속도를 설정
	* `c_ospeed` (output speed)
		* 출력 속도를 설정

### keypress 표시하기

```c
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
#include <termios.h>
#include <unistd.h>
struct termios orig_termios;

void disableRawMode() { … }

void enableRawMode() { … }

int main() {
  enableRawMode();
  char c;
  while (read(STDIN_FILENO, &c, 1) == 1 && c != 'q') {
    // print 불가능한 문자일 경우 (제어 문자일 경우)
    if (iscntrl(c)) {
      printf("%d\n", c);
    // print 가능한 문자일 경우
    } else {
      printf("%d ('%c')\n", c, c);
    }
  }
  return 0;
}
```

* `iscntrl`
	* `int iscntrl(int ch);`
	* 주어진 문자가 제어 문자가 아니면 0을 반환, 제어 문자일 경우 0이 아닌 값을 반환함
	* 제어 문자
		* 인쇄되지 않는 문자
		* 화면에 출력되지는 않지만, 터미널이나 텍스트 처리에서 특수한 기능을 수행하는 문자
		* `\n`, `\r` 등

[ASCII Table](https://www.asciitable.com/)
ASCII 기준 32번 부터 126번 까지는 printable 한 문자

**특수 키**
* 화살표 키, page up, page down, home, end
	* 3개의 4바이트로 터미널에 전달됨
	* 일반적으로 27, `[`, 하나 또는 두 개의 다른 문자 포함 (이스케이프 시퀀스)
* ESC 키
	* ASCII 코드 27로 전달됨
	* 이스케이프 시퀀스의 시작을 나타낼 수 있음
*  Backspace 키
	* ASCII 코드 127로 터미널에 전달됨
	* DEL 문자를 나타냄
	* 마지막 입력 문자를 지우는 기능을 수행
* DELETE 키
	* 4바이트의 이스케이프 시퀀스로 전달
	* `27` `[` `3` `~`
	* 첫 번째 바이트는 27(ESC) 이고 뒤따르는 값이 Delete 를 구분함
* ENTER 키
	* ASCII 코드 10으로 전달됨(newline)
	* `\n`으로 나타나며, 줄을 바꾸는 역할을 함
* Ctrl 키 조합
	* Ctrl 키와 A-Z 키를 조합하면 문자가 1~26의 코드에 매핑됨

### `Ctrl-C` `Ctrl-z` `Ctrl-s` `Ctrl-q` `Ctrl-v` 비활성화

* `Ctrl-C`
	* `SIGINT (Interrupt)` 시그널
	* 현재 실행 중인 프로그램을 중단함
* `Ctrl-Z`
	* `SIGSTP (Stop)` 시그널
	* 현재 실행 중인 프로그램을 일시 중지함
* `Ctrl-S`
	* `XOFF`
	* 터미널의 출력을 일시 정지함
* `Ctrl-Q`
	* `XON`
	* 중단된 터미널의 출력을 재개함
* `Ctrl-V`
	* `IEXTEN`
	* 리터럴 문자 입력, 다음에 입력하는 문자를 리터럴로 처리함 (특수 키나 제어 문자가 입력될 때 이를 문자 그대로 전달되게 함)

``` c
void enableRawMode() {
  tcgetattr(STDIN_FILENO, &orig_termios);
  atexit(disableRawMode);

  struct termios raw = orig_termios;
  // Disable Ctrl-S, Ctrl-Q
  raw.c_iflag &= ~(IXON);

  // Disable Ctrl-C, Ctrl-Z (ISIG)
  // Disable Ctrl-V (IEXTEN)
  raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);

  tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
}
```

### `Ctrl-M` fix

`Enter` 키를 입력하면 `10` 이 출력되는데, 지금 프로그램에서는 `Ctrl-M`, `Ctrl-J` 도 `10`이 출력된다 `Ctrl-M` 은 알파벳의 13번째 문자이므로 `13`출력을 기대해 볼 수 있는데 `10`이 출력되는 상황

`13`은 `carriage return` `13, \r` 출력으로, 현재 상황에서는 `newline (10, \n)` 으로 interuppt 되는 상황 이를 고치기 위해 `ICRNL` 플래그를 비활성화 할 필요가 있다

``` c
void enableRawMode() {
  tcgetattr(STDIN_FILENO, &orig_termios);
  atexit(disableRawMode);

  struct termios raw = orig_termios;
  // ICRNL (CR(\r) 을 NR(\n)으로 변경하는 플래그) 비활성화
  // CTRL-M 이 13으로 처리 (Enter 키도 13으로 처리됨)
  raw.c_iflag &= ~(ICRNL | IXON);
  raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);

  tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
}
```

**`Carriage Return (CR, \r)`**
* 입력을 줄의 처음으로 이동시키는 동작.
* 화면의 커서를 행의 맨 앞으로 이동
	* Window 에서는 CR 과 NL을 모두 사용해서 줄을 바꿈 (`\r\n`)
	* Unix 에서는 NL(`\n`)만 사용해서 줄을 바꿈

`ICRNL`
* 입력된 CR을 NL로 변환하여 줄 바꿈을 자동으로 처리하게 만드는 기능

### `output processing` 비활성화
`\n`을 출력하기 전, `\r\n`으로  변경해서 출력하는 상황인데, `OPOST`플래그를 이용해서 이 기능을 비활성화 할 수 있음

``` c
void enableRawMode() {
  tcgetattr(STDIN_FILENO, &orig_termios);
  atexit(disableRawMode);

  struct termios raw = orig_termios;
  raw.c_iflag &= ~(ICRNL | IXON);
  // OPST 플래그 비활성화
  raw.c_oflag &= ~(OPOST);
  raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);

  tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
}

int main() {
  enableRawMode();

  char c;
  while (read(STDIN_FILENO, &c, 1) == 1 && c != 'q') {
    if (iscntrl(c)) {
      //\r 추가해서 CR, OPST 플래그를 비활성화 했기 때문에 수동으로 처리해줘야 함
      printf("%d\r\n", c);
    } else {
      // \r 추가
      printf("%d ('%c')\r\n", c, c);
    }
  };
  return 0;
}
```

### `BRKINT`, `INPCK`, `ISTRIP`, `CS8` 플래그 설정

**`BRKINT`**
* `CTRL-C`등의 break condition 일 때, `SIGNET` 신호를 프로그램에 보냄
**`INPCK`**
* 패리티 체크 기능
**`ISTRIP`**
* 입력을 8비트만 받고, 나머지는 0으로 초기화함
**`CS8`**
* 플래그가 아니라 bit mask
* bitwise-OR (`|`)을 이용해서 character size를 바이트당 8비트로 고정함 (UTF-8 등 지원, 현대 시스템에서는 8비트(1바이트)가 표준이기 때문에 `CS8` 사용)

``` c
void enableRawMode() {
  tcgetattr(STDIN_FILENO, &orig_termios);
  atexit(disableRawMode);

  struct termios raw = orig_termios;
  // BRKINT, INPCK, ISTRIP
  raw.c_iflag &= ~(BRKINT | ICRNL | INPCK | ISTRIP | IXON);
  raw.c_oflag &= ~(OPOST);

  // CS8
  raw.c_cflag |= (CS8);
  raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);

  tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
}
```

### `read()` timeout 설정

```c
void enableRawMode() {
  tcgetattr(STDIN_FILENO, &orig_termios);
  atexit(disableRawMode);

  struct termios raw = orig_termios;
  raw.c_iflag &= ~(BRKINT | ICRNL | INPCK | ISTRIP | IXON);
  raw.c_oflag &= ~(OPOST);
  raw.c_cflag |= (CS8);
  raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);

  // control flag 에서 VMIN 을 사용해서 read() 함수가 return 할 수 있는 최소 바이트를 설정
  raw.c_cc[VMIN] = 0;
  // VTIME 을 사용해서 read() 가 return되기 전 기다리는 최대 시간 (timeout) 설정 (1/10 second, 100 milisecond)
  raw.c_cc[VTIME] = 1;

  tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
}


int main() {
  enableRawMode();

  while (1) {
    char c = '\0';
    // 아직 read 함수의 값을 사용하지 않기 때문에 (void) 로 함수 타입캐스팅함
    (void)read(STDIN_FILENO, &c, 1);

    if (iscntrl(c)) {
      printf("%d\r\n", c);
    } else {
      printf("%d ('%c')\r\n", c, c);
    }

    if (c == 'q')
      break;
  }

  return 0;
}
```

### `error handling`

``` c
#include <ctype.h>
// errno 헤더 추가
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <termios.h>
#include <unistd.h>

struct termios orig_termios;

// die 함수 작성
void die(const char *s) {
  perror(s);
  exit(1);
}

void disableRawMode() {
  // tcsetattr 함수 실패시 die 함수 호출
  if (tcsetattr(STDIN_FILENO, TCSAFLUSH, &orig_termios)) {
    die("tcsetattr");
  }
}

void enableRawMode() {
  // tcgetattr 함수 실패시 die 함수 호출
  if (tcgetattr(STDIN_FILENO, &orig_termios)) {
    die("tcgetattr");
  }
  atexit(disableRawMode);

  struct termios raw = orig_termios;
  raw.c_iflag &= ~(BRKINT | ICRNL | INPCK | ISTRIP | IXON);
  raw.c_oflag &= ~(OPOST);
  raw.c_cflag |= (CS8);
  raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);

  raw.c_cc[VMIN] = 0;
  raw.c_cc[VTIME] = 1;

  // tcsetattr 함수 실패시 die 함수 호출
  if (tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw)) {
    die("tcsetattr");
  };
}

int main() {
  enableRawMode();

  while (1) {
    char c = '\0';

    // read 함수 실패 && errno 가 EAGAIN 이 아닐때 die 함수 호출
    if (read(STDIN_FILENO, &c, 1) == -1 && errno != EAGAIN) {
      die("read");
    };

    if (iscntrl(c)) {
      printf("%d\r\n", c);
    } else {
      printf("%d ('%c')\r\n", c, c);
    }

    if (c == 'q')
      break;
  }

  return 0;
}
```