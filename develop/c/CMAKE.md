### CMakeLists.txt
source 가 어디에 있는지, build 폴더가 어디에 있는지 알려줘야함

```bash
## cmake output

$ cmake
Usage

  cmake [options] <path-to-source>
  cmake [options] <path-to-existing-build>
  cmake [options] -S <path-to-source> -B <path-to-build>

Specify a source directory to (re-)generate a build system for it in the
current working directory.  Specify an existing build directory to
re-generate its build system.

Run 'cmake --help' for more information.
```

(root dir 기준)
`cmake -S . -B ./build` 명령을 실행하면, `./build` 디렉터리에 `CMakefiles` `CMakeCache.txt` `Makefile` `cmake_install.cmake` 등 자동으로 생성해 준다

**CMake 최소 버전 명시**
`cmake -version` 명령으로 현재 system 의 cmake version 확인 가능

``` cmake
cmake_minimum_required(VERSION 3.1.1)
```

**project 생성 & add_excutable**

```cmake
cmake_minimum_required(VERSION 3.28.3)

project(CMAKE_STUDY)

add_executable(${PROJECT_NAME} main.c)
```

`cmake -S . -B build/` 명령으로, makefile 등 생성 후 `cd build/; make` 로 build 가능함

**/usr/local/bin/ 디렉터리에 install 하기**
`CMakeLists.txt` 파일에
`install(TARGETS ${PROJECT_NAME} DESTINATION bin)` 추가해 준 뒤

(root dir 기준)
`cmake -S . -B /build; make; sudo make install`

(build dir 기준)
`cmake -S ..; make; sudo make install`

진행해 주면 `/usr/src/bin/` 디렉터리에 install이 가능하고, 쉘에서 바로 실행 가능함

### LIBRARIES
