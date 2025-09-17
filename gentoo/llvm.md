arch-chroot 이후에, 또는 일반 gcc 기반 환경에서

```bash
# /etc/portage/env/compiler-clang
COMMON_FLAGS="-march=native -02 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"

CC="clang"
CXX="clang++"

LDFLAGS="-fuse-ld=ldd -rtlib=compiler-rt -unwindlib=libunwind -Wl, --as-needed"
```

``` bash
# /etc/portage/package.use/clang
llvm-core/clang-common default-compiler-rt default-lld llvm-libunwind
```

``` bash
# GCC 로 bootstrap compiling
emerge -avq llvm-core/clang llvm-core/llvm llvm-runtimes/compiler-rt llvm-runtimes/libunwind llvm-core/lld
```

clang, cmake verify
``` bash
which clang
clang --version

which cmake
cmake --version

# 없으면 env upate
env-update && ./etc/profile

# 그래도 안되면 profile 변경 후, @world update
eselect profile list
eselect profile set #
emerge -avq --update --deep --newuse @world
```

이후, bootstrap용 프로그램 빌드 성공했으면

```bash
# /etc/portage/package.env/cc
llvm-core/clang compiler-clang
llvm-core/llvm compiler-clang
llvm-runtimes/libcxx compiler-clang
llvm-runtimes/libcxxabi compiler-clang
llvm-runtimes/compiler-rt compiler-clang
llvm-runtimes/compiler-rt-sanitizers compiler-clang
llvm-runtimes/libunwind compiler-clang
llvm-core/lld compiler-clang
```

toolchain rebuild
``` bash
emerge -avq llvm-core/clang llvm-core/llvm llvm-runtimes/libcxx llvm-runtimes/libcxxabi llvm-runtimes/compiler-rt llvm-runtimes/compiler-rt-sanitizers llvm-runtimes/libunwind llvm-core/lld
```

이후 `eselect profile` 설정 한 뒤에, `emerge -avq --update --deep --newuse @world` 실행.

CONFIG_EXTRA_FIRMWARE="amdgpu/psp_13_0_4_ta.bin amdgpu/psp_13_0_4_toc.bin amdgpu/dcn_3_1_4_dmcub.bin amdgpu/gc_11_0_1_imu.bin amdgpu/gc_11_0_1_me.bin amdgpu/gc_11_0_1_mec.bin amdgpu/gc_11_0_1_mes1.bin amdgpu/gc_11_0_1_mes_2.bin amdgpu/gc_11_0_1_mes.bin amdgpu/gc_11_0_1_pfp.bin amdgpu/gc_11_0_1_rlc.bin amdgpu/vcn_4_0_2.bin"