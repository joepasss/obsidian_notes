**`vim.h` include**
``` c
// EXTERN 매크로 정의
// 외부 변수나 함수가 정의된다는 것을 알리는 용도로 사용될 수 있음
#define EXTERN

// "vim.h" 헤더 파일 include
#include "vim.h"
```

**`Cygwin` 환경에서 컴파일 될 때 조건부 컴파일 블록**

``` c
// Cygwin 환경에서 컴파일 될 때만 포함되는 조건부 컴파일 블록
// Cygwin에서 자동으로 정의되는 매크로 __CYGWIN__ 사용
#ifdef __CYGWIN__
// cygwin의 버전 정보를 포함하는 헤더 파일
# include <cygwin/version.h>
// cygwin 관련 기능을 제공하는 헤더 파일
# include <sys/cygwin.h>	// for cygwin_conv_to_posix_path() and/or
				// cygwin_conv_path()
// 파일 시스템의 경로 길이 제한을 처리할 때 필요한 상수들이 정의된 헤더 파일
# include <limits.h>
#endif
```

**`MS Windows` 환경에서 컴파일 될 때 조건부 컴파일 블록

``` c
// MS Window 환경에서만 적용됨
// 코드가 windows에서 컴파일 될 때, MSWIN 매크로가 적용되어 있을것
// GUI 기능이 없는 Window 환경이거나, Vim이 DLL로 컴파일 될 때 사용
#if defined(MSWIN) && (!defined(FEAT_GUI_MSWIN) || defined(VIMDLL))
// Window에서 컴파일 될 경우 헤더 포함시킴
# include "iscygpty.h"
#endif
```

**`edit_type` Vim 이 시작될 때, 어떤 종류의 편집 작업을 수행할지 나타내는 상수 정의**

``` c
// Values for edit_type.
#define EDIT_NONE 0  // no edit type yet

// 파일이 arg로 주어졌을 때, 해당 파일을 열고 편집하는 상태
#define EDIT_FILE 1  // file name argument[s] given, use argument list

// 표준 입력에서 파일을 읽어들이는 경우 사용됨
// 파일 이름 대신, 표준 입력으로부터 데이터를 받아 Vim 이 편집할 내용을 가져옴
// 파이프(|)를 사용하여 표준 입력에서 데이터를 읽어와 편집하는 경우에 해당함
#define EDIT_STDIN 2 // read file from stdin

// tag 이름이 인자로 주어졌을 때 사용됨
// 태그 이름을 주면, 해당 태그로 이동한 후 파일을 열 수 있음
#define EDIT_TAG 3   // tag name argument given, use tagname

// Vim이 quickfix 모드로 시작하는 경우
// Quickfix 모드는 컴파일러난 다른 도구의 출력에서 발생한 에러 목록을 보여주고, 에러가 발생한 파일과 줄 번호로 쉽게 이동할 수 있게 해줌
// :make 명령 이후에 발생하는 컴파일 에러를 처라할 때 사용함
#define EDIT_QF 4    // start in quickfix mode
```

**prototypes 정의**

``` c
// UNIX 또는 VMS 가 정의되어 있는 경우(Unix 계열 환경일 때) && NO_VIM_MAIN 이 정의되지 않은 경우에 prototype 정의
#if (defined(UNIX) || defined(VMS)) && !defined(NO_VIM_MAIN)
static int file_owned(char *fname);
#endif
static void mainerr(int, char_u *);
static void early_arg_scan(mparm_T *parmp);
#ifndef NO_VIM_MAIN
static void usage(void);
static void parse_command_name(mparm_T *parmp);
static void command_line_scan(mparm_T *parmp);
static void check_tty(mparm_T *parmp);
static void read_stdin(void);
static void create_windows(mparm_T *parmp);
static void edit_buffers(mparm_T *parmp, char_u *cwd);
static void exe_pre_commands(mparm_T *parmp);
static void exe_commands(mparm_T *parmp);
static void source_startup_scripts(mparm_T *parmp);
static void main_start_gui(void);
static void check_swap_exists_action(void);
# ifdef FEAT_EVAL
static void set_progpath(char_u *argv0);
# endif
#endif
```

**에러 메시지 정의**

``` c
/*
 * Different types of error messages.
 */
static char *(main_errors[]) =
{
    N_("Unknown option argument"),
#define ME_UNKNOWN_OPTION	0
    N_("Too many edit arguments"),
#define ME_TOO_MANY_ARGS	1
    N_("Argument missing after"),
#define ME_ARG_MISSING		2
    N_("Garbage after option argument"),
#define ME_GARBAGE		3
    N_("Too many \"+command\", \"-c command\" or \"--cmd command\" arguments"),
#define ME_EXTRA_CMD		4
    N_("Invalid argument for"),
#define ME_INVALID_ARG		5
};
```