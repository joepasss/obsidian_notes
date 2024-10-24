[build your own text editor](https://viewsourcecode.org/snaptoken/kilo/index.html)

### c compiler 준비

**shell.nix 작성**
``` nix
{ pkgs ? import <nixpkgs> {} }:

pkgs.mkShell {
	buildInputs = [
		pkgs.gcc
	];
	shellHook = ''
		bash
	'';
}
```

`nix-shell` 명령으로 nix shell 환경에 진입 가능, `gcc --version` 으로 `gcc` 가 정상 설치 되었는지 확인 가능함. `make` 를 사용해서 build 할 예정이기 때문에 `make --version` 으로 확인

``` bash
### gcc --version check
$ gcc --version
gcc (GCC) 13.3.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

### make version check
$ make --version
GNU Make 4.4.1
Built for x86_64-pc-linux-gnu
Copyright (C) 1988-2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

### build 준비

**`main()` 함수 작성**

```c
### kilo.c
int main() {
	return 0;
}
```

`main()`함수에서 integer값을 운영체제에 return 하게 되는데 `0` 값은 `sucess`를 의미함.
`kilo.c`를 컴파일하기 위해서는 `cc kilo.c -o kilo` 를 쉘에 입력하면 된다 `-o` 옵션은 `output`을 의미함, 이후 build된 파일을 실행하기 위해서는 `./kilo` 명령으로 실행 가능하다

`exit status` 를 체크하려면, `echo $?` 를 프로그램 종료 이후 실행해주면, 값을 확인할 수 있음

### `make` 를 이용한 complile

**`Makefile` 작성**
``` bash
### Makefile 생성
touch Makefile
```

``` Makefile
### [PROJECT_NAME]: [REQUIRED]
### 실행될 파일의 이름, kilo를 빌드하기 위해 필요한 소스 파일 (kilo.c 가 수정되면 kilo를 컴파일)

kilo: kilo.c
	$(CC) kilo.c -o kilo -Wall -Wextra -pedantic -std=C99

# $(CC) make expands to cc by default (gnu gcc 사용한다는 뜻)
# -Wall "all Warnings" 모든 일반적인 경고 활성화
# -Wextra "Waring extra" 추가적인 경고 활성화
# -pedantic 엄격한 표준 준수를 요구, 표준에 어긋나는 경우 에러를 발생
# -std=C99 C99 스탠다드 사용
```

**`build` dir 생성한 다음, `build` 폴더 내부에 컴파일된 파일 저장하기**

``` Makefile
BUILD_DIR = ./build
TARGET = $(BUILD_DIR)/kilo

$(TARGET): kilo.c | $(BUILD_DIR)
	$(CC) kilo.c -o $(TARGET) -Wall -Wextra -pedantic -std=c99

$(BUILD_DIR):
	mkdir -pv $(BUILD_DIR)

clean:
	rm -rf $(BUILD_DIR)
```

