## live booting disk 만들기 (gentoo live gui)

**live boot iso 다운로드**
[gentoo downloads](https://www.gentoo.org/downloads/) 에서 브라우저를 통해 설치 하거나 wget, curl 등의 tool로 host pc 에 설치
``` bash
# wget 이용

# 원하는 폴더로 이동 후 설치
cd $HOME/Downloads; \
wget https://distfiles.gentoo.org/releases/amd64/autobuilds/20240830T150404Z/livegui-amd64-20240830T150404Z.iso
```

**booting 디스크 초기화 & bootable 하게 만들기**
``` bash
# 디스크 식별
lsblk

# 디스크 초기화
sudo wipefs --all --force /dev/MY_DISK

# iso 파일 복사
sudo dd if=$HOME/Downloads/My_ISO of=/dev/MY_DISK status=progress
```

## HOST system 준비

**하드웨어 requirement 확인**
```bash
# cpu 최소 4코어 이상
nproc

# 램 8기가 이상
free -h
```

**software requirement 확인**
``` bash
cat > version-check.sh << "EOF"
#!/bin/bash
# A script to list version numbers of critical development tools

# If you have tools installed in other directories, adjust PATH here AND
# in ~lfs/.bashrc (section 4.4) as well.

LC_ALL=C 
PATH=/usr/bin:/bin

bail() { echo "FATAL: $1"; exit 1; }
grep --version > /dev/null 2> /dev/null || bail "grep does not work"
sed '' /dev/null || bail "sed does not work"
sort   /dev/null || bail "sort does not work"

ver_check()
{
   if ! type -p $2 &>/dev/null
   then 
     echo "ERROR: Cannot find $2 ($1)"; return 1; 
   fi
   v=$($2 --version 2>&1 | grep -E -o '[0-9]+\.[0-9\.]+[a-z]*' | head -n1)
   if printf '%s\n' $3 $v | sort --version-sort --check &>/dev/null
   then 
     printf "OK:    %-9s %-6s >= $3\n" "$1" "$v"; return 0;
   else 
     printf "ERROR: %-9s is TOO OLD ($3 or later required)\n" "$1"; 
     return 1; 
   fi
}

ver_kernel()
{
   kver=$(uname -r | grep -E -o '^[0-9\.]+')
   if printf '%s\n' $1 $kver | sort --version-sort --check &>/dev/null
   then 
     printf "OK:    Linux Kernel $kver >= $1\n"; return 0;
   else 
     printf "ERROR: Linux Kernel ($kver) is TOO OLD ($1 or later required)\n" "$kver"; 
     return 1; 
   fi
}

# Coreutils first because --version-sort needs Coreutils >= 7.0
ver_check Coreutils      sort     8.1 || bail "Coreutils too old, stop"
ver_check Bash           bash     3.2
ver_check Binutils       ld       2.13.1
ver_check Bison          bison    2.7
ver_check Diffutils      diff     2.8.1
ver_check Findutils      find     4.2.31
ver_check Gawk           gawk     4.0.1
ver_check GCC            gcc      5.2
ver_check "GCC (C++)"    g++      5.2
ver_check Grep           grep     2.5.1a
ver_check Gzip           gzip     1.3.12
ver_check M4             m4       1.4.10
ver_check Make           make     4.0
ver_check Patch          patch    2.5.4
ver_check Perl           perl     5.8.8
ver_check Python         python3  3.4
ver_check Sed            sed      4.1.5
ver_check Tar            tar      1.22
ver_check Texinfo        texi2any 5.0
ver_check Xz             xz       5.0.0
ver_kernel 4.19

if mount | grep -q 'devpts on /dev/pts' && [ -e /dev/ptmx ]
then echo "OK:    Linux Kernel supports UNIX 98 PTY";
else echo "ERROR: Linux Kernel does NOT support UNIX 98 PTY"; fi

alias_check() {
   if $1 --version 2>&1 | grep -qi $2
   then printf "OK:    %-4s is $2\n" "$1";
   else printf "ERROR: %-4s is NOT $2\n" "$1"; fi
}
echo "Aliases:"
alias_check awk GNU
alias_check yacc Bison
alias_check sh Bash

echo "Compiler check:"
if printf "int main(){}" | g++ -x c++ -
then echo "OK:    g++ works";
else echo "ERROR: g++ does NOT work"; fi
rm -f a.out

# cpu core 확인
if [ "$(nproc)" = "" ]; then
   echo "ERROR: nproc is not available or it produces empty output"
else
   echo "OK: nproc reports $(nproc) logical cores are available"
fi
EOF

bash version-check.sh
```

### disk partition (uefi)

``` 
### partion

/dev/MY_DISK
	--- /boot/efi (EXT2,  200Mib)
	--- /boot/efi (FAT32, 200Mib)
	--- swap      (swap,  hibernation 사용시 ram 용량 이상으로)
	--- /root     (EXT4,  나머지 용량 설정, 최소 10G)
```

`cfdisk /dev/MY_DISK`로 파티션을 생성한 후 포멧 진행

``` bash
mkfs.vfat -F32 /dev/MY_EFI_PARTITION
mkfs.ext2      /dev/MY_BOOT_PARTITION
mkfs.ext4 -j   /dev/MY_ROOT_PARTITION

# swap 사용예정일 경우
mkswap /dev/MY_SWAP_PARTITION
```

포멧, 파티션 결과 확인하기
``` bash
# 파티션 결과 확인
lsblk

# lsblk 출력 결과
livecd ~ # lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0   3.2G  1 loop /run/rootfsbase
sda      8:0    0 238.5G  0 disk 
├─sda1   8:1    0   200M  0 part 
├─sda2   8:2    0   200M  0 part 
├─sda3   8:3    0    16G  0 part [SWAP]
└─sda4   8:4    0 222.1G  0 part 
sdb      8:16   1  14.6G  0 disk 
├─sdb1   8:17   1   254K  0 part 
├─sdb2   8:18   1   2.8M  0 part 
├─sdb3   8:19   1   3.3G  0 part /run/initramfs/live
└─sdb4   8:20   1   300K  0 part 

# 포멧 결과 확인
blkid /dev/sda*

# blkid 출력 결과
blkid /dev/sda*
/dev/sda: PTUUID="2b470c43-1b7c-4ea8-adda-0c0ce6970ea6" PTTYPE="gpt"
/dev/sda1: UUID="4a9e4b5d-dc7c-402b-bdb9-c27ff5c2484e" BLOCK_SIZE="1024" TYPE="ext2" PARTUUID="4808f671-3983-48b2-a33d-304aa7159e9f"
/dev/sda2: UUID="6CEA-E140" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="619ebccf-6ce1-4cfe-95a0-d88061d3963a"
/dev/sda3: UUID="67a0fc49-1472-4ce4-9511-24b567e2bbb4" TYPE="swap" PARTUUID="fa712d79-42f0-44b4-8a03-ca3a0ad86948"
/dev/sda4: UUID="91a0db94-0a5c-4efb-9f70-ae8995ac557b" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="7e287db8-533c-46be-a045-016028f83f88"
```
### $LFS variable 지정하기

``` bash
export LFS=/mnt/lfs
```

**host 운영체제의 .bashrc 에 직접 지정하는 방법**
``` bash
echo "export LFS=/mnt/lfs" | tee -a $HOME/.bashrc > /dev/null; source .bashrc

### vim 또는 nano 로 직접 추가해도 된다
# echo $LFS 로 확인 가능함
```

### 파티션 마운트

``` bash
# 마운트될 폴더 생성
mkdir -pv $LFS

# root 파티션 마운트
mount -v -t ext4 /dev/MY_ROOT_PARTITION $LFS

# swap 사용중인 경우
swapon /dev/MY_SWAP_PARTION

# 마운트 되었는지 확인
lsblk
```

## 패키지, 패치 source 설치

[lfs downloads (stable)](https://www.linuxfromscratch.org/lfs/downloads/stable/)

``` bash
# $LFS/source 폴더 생성
mkdir -v $LFS/sources

# 디렉터리를 sticky(여러 유저가 쓰기 권한을 가지고 있을 때, 소유자만이 삭제가 가능함)
chmod -v a+wt $LFS/sources

# 디렉터리 권한 확인 (출력이 drwxrwxrwt 인 경우 정상)
ls -al $LFS | grep sources

# wget-list 설치
wget https://www.linuxfromscratch.org/lfs/downloads/stable/wget-list

wget --input-file=wget-list --continue --directory-prefix=$LFS/sources

# md5sum (파일 무결성 확인, 해쉬값 생성하고 검증해 주는 유틸리티)
wget https://www.linuxfromscratch.org/lfs/downloads/stable/md5sums --directory-prefix=$LFS/sources

pushd $LFS/sources
	md5sum -c md5sums
popd

# owner를 root로 변경
chown root:root $LFS/sources/*
```

### 디렉터리 생성

``` bash
# root 아닐경우
su root

mkdir -pv $LFS/{etc,var} $LFS/usr/{bin,lib,sbin}

# usr 디렉터리에 심링크 생성
for i in bin lib sbin; do
	ln -sv usr/$i $LFS/$i
done

# x86_64인 경우 lib64 dir 생성
case $(uname -m) in
	x86_64) mkdir -pv $LFS/lib64 ;;
esac

# cross-compiler dir 생성
mkdir -pv $LFS/tools

# 정상적으로 생성되었는지 확인
ls $LFS  # etc, var, bin, lib, sbin, tools 있어야함 (bin, lib, sbin은 usr/{bin, lib, sbin}의 심링크)
ls $LFS/usr # bin, lib, sbin 있어야함
```

### lfs 유저 생성

``` bash
# root 가 아니면
su root

# root 로 로그인 한 상태에서
groupadd lfs
useradd -s /bin/bash -g lfs -m -k /dev/null lfs
passwd lfs

# $LFS access 허용
chown -v lfs $LFS/{usr{,/*},lib,var,etc,bin,sbin,tools}
case $(uname -m) in
  x86_64) chown -v lfs $LFS/lib64 ;;
esac

# lfs로 로그인
su - lfs
```

### lfs 유저 환경 설정 (.bash_profile, .bashrc 수정)

``` bash
# ~/.bash_profile
cat > ~/.bash_profile << "EOF"
exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
EOF

# ~/.bashrc
cat > ~/.bashrc << "EOF"
set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
LFS_TGT=$(uname -m)-lfs-linux-gnu
PATH=/usr/bin
if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
PATH=$LFS/tools/bin:$PATH
CONFIG_SITE=$LFS/usr/share/config.site
export LFS LC_ALL LFS_TGT PATH CONFIG_SITE
export MAKEFLAGS=j$(nproc)
EOF

source ~/.bash_profile
```

## LFS Cross toolchain & temporary tools build

### cross-toolchain compile

**Binutils Pass 1**

```bash
cd $LFS/sources
tar -xf binutils-2.43.1.tar.xz
cd binutils-2.43.1

mkdir -v build
cd build

### configure
../configure --prefix=$LFS/tools \
             --with-sysroot=$LFS \
             --target=$LFS_TGT   \
             --disable-nls       \
             --enable-gprofng=no \
             --disable-werror    \
             --enable-new-dtags  \
             --enable-default-hash-style=gnu

### build & install
make
make install
```

**GCC Pass 1**

``` bash
cd $LFS/sources
tar -xf gcc-14.2.0.tar.xz
cd gcc-14.2.0

tar -xf ../mpfr-4.2.1.tar.xz
mv -v mpfr-4.2.1 mpfr
tar -xf ../gmp-6.3.0.tar.xz
mv -v gmp-6.3.0 gmp
tar -xf ../mpc-1.3.1.tar.gz
mv -v mpc-1.3.1 mpc

### x86_64
case $(uname -m) in
  x86_64)
    sed -e '/m64=/s/lib64/lib/' \
        -i.orig gcc/config/i386/t-linux64
 ;;
esac

mkdir -v build
cd build

### configure
../configure                  \
    --target=$LFS_TGT         \
    --prefix=$LFS/tools       \
    --with-glibc-version=2.40 \
    --with-sysroot=$LFS       \
    --with-newlib             \
    --without-headers         \
    --enable-default-pie      \
    --enable-default-ssp      \
    --disable-nls             \
    --disable-shared          \
    --disable-multilib        \
    --disable-threads         \
    --disable-libatomic       \
    --disable-libgomp         \
    --disable-libquadmath     \
    --disable-libssp          \
    --disable-libvtv          \
    --disable-libstdcxx       \
    --enable-languages=c,c++

### build & install
make
make install

### create system header(limits.h)
cd $LFS/sources/gcc-14.2.0
cat gcc/limitx.h gcc/glimits.h gcc/limity.h > \
  `dirname $($LFS_TGT-gcc -print-libgcc-file-name)`/include/limits.h
```

**Linux API Headers**

```bash
cd $LFS/sources
tar -xf linux-6.10.5.tar.xz
cd linux-6.10.5

### 커널 빌드 환경 정리(이전 빌드 있다면 제거, 설정 파일 있다면 제거, 기타 생성 파일 제거)
# make clean 으로 제거되지 않는 파일(.config 등)까지 정리해줌
# 커널 소스 트리를 완전히 초기화 해 주는 역할
make mrproper

# 커널 헤더 추출 & ./usr에 추출된 헤더 옮기기
make headers
find usr/include -type f ! -name '*.h' -delete
cp -rv usr/include $LFS/usr
```

**Glibc**

``` bash
cd $LFS/sources
tar -xf glibc-2.40.tar.xz
cd glibc-2.40

### LSB(Linux Standard Base) 규격 준수를 위한 심링크 생성
case $(uname -m) in
    i?86)   ln -sfv ld-linux.so.2 $LFS/lib/ld-lsb.so.3
    ;;
    x86_64) ln -sfv ../lib/ld-linux-x86-64.so.2 $LFS/lib64
            ln -sfv ../lib/ld-linux-x86-64.so.2 $LFS/lib64/ld-lsb-x86-64.so.3
    ;;
esac

### patch 적용 (runtime data 저장하는 위치를 /var/db 에서 FHS-compliant 위치로 바꿔줌)
patch -Np1 -i ../glibc-2.40-fhs-1.patch

mkdir -v build
cd build

### configure
../configure                             \
      --prefix=/usr                      \
      --host=$LFS_TGT                    \
      --build=$(../scripts/config.guess) \
      --enable-kernel=4.19               \
      --with-headers=$LFS/usr/include    \
      --disable-nscd                     \
      libc_cv_slibdir=/usr/lib

### build & install
make
make DESTDIR=$LFS install

### ldd 스크립트에 hardcoded 된 path 수정
sed '/RTLDLIST=/s@/usr@@g' -i $LFS/usr/bin/ldd
```

**!!Sanity Check!!**

``` bash
echo 'int main(){}' | $LFS_TGT-gcc -xc -
readelf -l a.out | grep ld-linux

### 정상적일때 출력
[Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]

### test 파일 삭제
rm -v a.out
```

**Libstdc++**

``` bash
cd $LFS/sources/gcc-14.2.0

### 이전 build 삭제
rm -rv build
mkdir -v build
cd build

### configure
../libstdc++-v3/configure           \
    --host=$LFS_TGT                 \
    --build=$(../config.guess)      \
    --prefix=/usr                   \
    --disable-multilib              \
    --disable-nls                   \
    --disable-libstdcxx-pch         \
    --with-gxx-include-dir=/tools/$LFS_TGT/include/c++/14.2.0

### build & install
make
make DESTDIR=$LFS install

### libtool archive file 삭제
rm -v $LFS/usr/lib/lib{stdc++{,exp,fs},supc++}.la
```
### cross compiling temporary tools

**M4**

``` bash
cd $LFS/sources
tar -xf m4-1.4.19.tar.xz
cd m4-1.4.19

### configure
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**Ncurses**

``` bash
cd $LFS/sources
tar -xf ncurses-6.5.tar.gz
cd ncurses-6.5

### gawk check
sed -i s/mawk// configure

### tic build
mkdir build
pushd build
  ../configure
  make -C include
  make -C progs tic
popd

### configure
./configure --prefix=/usr                \
            --host=$LFS_TGT              \
            --build=$(./config.guess)    \
            --mandir=/usr/share/man      \
            --with-manpage-format=normal \
            --with-shared                \
            --without-normal             \
            --with-cxx-shared            \
            --without-debug              \
            --without-ada                \
            --disable-stripping

### build & install
make
make DESTDIR=$LFS TIC_PATH=$(pwd)/build/progs/tic install
ln -sv libncursesw.so $LFS/usr/lib/libncurses.so
sed -e 's/^#if.*XOPEN.*$/#if 1/' \
    -i $LFS/usr/include/curses.h
```

**bash**

``` bash
cd $LFS/sources
tar -xf bash-5.2.32.tar.gz
cd bash-5.2.32

### configure
./configure --prefix=/usr                      \
            --build=$(sh support/config.guess) \
            --host=$LFS_TGT                    \
            --without-bash-malloc              \
            bash_cv_strtold_broken=no

### build & install
make
make DESTDIR=$LFS install
ln -sv bash $LFS/bin/sh
```

**coreutils**

``` bash
cd $LFS/sources
tar -xf coreutils-9.5.tar.xz
cd coreutils-9.5

### configure
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess) \
            --enable-install-program=hostname \
            --enable-no-install-program=kill,uptime

### build & install
make
make DESTDIR=$LFS install

### move programs
mv -v $LFS/usr/bin/chroot              $LFS/usr/sbin
mkdir -pv $LFS/usr/share/man/man8
mv -v $LFS/usr/share/man/man1/chroot.1 $LFS/usr/share/man/man8/chroot.8
sed -i 's/"1"/"8"/'                    $LFS/usr/share/man/man8/chroot.8
```

**diffutils**

``` bash
cd $LFS/sources
tar -xf diffutils-3.10.tar.xz
cd diffutils-3.10

### configure
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(./build-aux/config.guess)

### make & install
make
make DESTDIR=$LFS install
```

**file**

``` bash
cd $LFS/sources
tar -xf file-5.45.tar.gz
cd file-5.45

### 임시 file command 생성
mkdir build
pushd build
  ../configure --disable-bzlib      \
               --disable-libseccomp \
               --disable-xzlib      \
               --disable-zlib
  make
popd

### configure
./configure --prefix=/usr --host=$LFS_TGT --build=$(./config.guess)

### make & install
make FILE_COMPILE=$(pwd)/build/src/file
make DESTDIR=$LFS install

### remove libtool archive file
rm -v $LFS/usr/lib/libmagic.la
```

**findutils**

``` bash
cd $LFS/sources
tar -xf findutils-4.10.0.tar.xz
cd findutils-4.10.0

### configure
./configure --prefix=/usr                   \
            --localstatedir=/var/lib/locate \
            --host=$LFS_TGT                 \
            --build=$(build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**gawk**

``` bash
cd $LFS/sources
tar -xf gawk-5.3.0.tar.xz
cd gawk-5.3.0

### 필요없는 파일 설치되지 않게
sed -i 's/extras//' Makefile.in

### configure
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**grep**

``` bash
cd $LFS/sources
tar -xf grep-3.11.tar.xz
cd grep-3.11

### configure
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(./build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**gzip**

``` bash
cd $LFS/sources
tar -xf gzip-1.13.tar.xz
cd gzip-1.13

### configure
./configure --prefix=/usr --host=$LFS_TGT

### build & install
make
make DESTDIR=$LFS install
```

**make**

``` bash
cd $LFS/sources
tar -xf make-4.4.1.tar.gz
cd make-4.4.1

### configure
./configure --prefix=/usr   \
            --without-guile \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**patch**

``` bash
cd $LFS/sources
tar -xf patch-2.7.6.tar.xz
cd patch-2.7.6

### configure
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**sed**

``` bash
cd $LFS/sources
tar -xf sed-4.9.tar.xz
cd sed-4.9

### configure
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(./build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**tar**

``` bash
cd $LFS/sources
tar -xf tar-1.35.tar.xz
cd tar-1.35

### configure
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess)

### build & install
make
make DESTDIR=$LFS install
```

**xz**

``` bash
cd $LFS/sources
tar -xf xz-5.6.2.tar.xz
cd xz-5.6.2

### configure
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess) \
            --disable-static                  \
            --docdir=/usr/share/doc/xz-5.6.2

### build & install
make
make DESTDIR=$LFS install

### remove libtool archive file
rm -v $LFS/usr/lib/liblzma.la
```

**binutils Pass2**

``` bash
cd $LFS/sources/binutils-2.43.1

### libtool 사용하지 않는 패키지들 수정
# add_dir 변수를 삭제해서 빌드된 바이너리가 호스트 시스템의 라이브러리 경로를 참조하는걸 막음
sed '6009s/$add_dir//' -i ltmain.sh

### build dir 재생성
rm -rf ./build
mkdir -v build
cd build

### configure
../configure                   \
    --prefix=/usr              \
    --build=$(../config.guess) \
    --host=$LFS_TGT            \
    --disable-nls              \
    --enable-shared            \
    --enable-gprofng=no        \
    --disable-werror           \
    --enable-64-bit-bfd        \
    --enable-new-dtags         \
    --enable-default-hash-style=gnu

### build & install
make
make DESTDIR=$LFS install

### remove libtool archive files
rm -v $LFS/usr/lib/lib{bfd,ctf,ctf-nobfd,opcodes,sframe}.{a,la}
```

**gcc Pass2**

``` bash
cd $LFS/sources/gcc-14.2.0

### 이전 빌드 삭제
rm -rf mpfr
rm -rf gmp
rm -rf mpc
rm -rf build

### mpfr, gmp, mpc unpack
tar -xf ../mpfr-4.2.1.tar.xz
mv -v mpfr-4.2.1 mpfr
tar -xf ../gmp-6.3.0.tar.xz
mv -v gmp-6.3.0 gmp
tar -xf ../mpc-1.3.1.tar.gz
mv -v mpc-1.3.1 mpc

### x86_64 lib 폴더 이름 변경 (64bit-lib -> lib)
case $(uname -m) in
  x86_64)
    sed -e '/m64=/s/lib64/lib/' \
        -i.orig gcc/config/i386/t-linux64
  ;;
esac

### libgcc building rule 변경
sed '/thread_header =/s/@.*@/gthr-posix.h/' \
    -i libgcc/Makefile.in libstdc++-v3/include/Makefile.in

### build dir 재생성
mkdir -v build
cd build

### configure
../configure                                       \
    --build=$(../config.guess)                     \
    --host=$LFS_TGT                                \
    --target=$LFS_TGT                              \
    LDFLAGS_FOR_TARGET=-L$PWD/$LFS_TGT/libgcc      \
    --prefix=/usr                                  \
    --with-build-sysroot=$LFS                      \
    --enable-default-pie                           \
    --enable-default-ssp                           \
    --disable-nls                                  \
    --disable-multilib                             \
    --disable-libatomic                            \
    --disable-libgomp                              \
    --disable-libquadmath                          \
    --disable-libsanitizer                         \
    --disable-libssp                               \
    --disable-libvtv                               \
    --enable-languages=c,c++

### build & install
make
make DESTDIR=$LFS install

### utility 심링크 생성 (cc -> gcc)
ln -sv gcc $LFS/usr/bin/cc
```

## entering chroot env

**ownership 변경**

``` bash
### sudo - lfs 로 lfs 유저에 로그인한 상황이라면, root 로 변경
exit

### 유저 확인
whoami

### $LFS 변수 확인
echo "$LFS"

### $LFS 는 호스트 시스템에서만 존재하는 lfs 유저가 가지고 있는데 $LFS의 ownership 을 root 로 변경
chown --from lfs -R root:root $LFS/{usr,lib,var,etc,bin,sbin,tools}
case $(uname -m) in
  x86_64) chown --from lfs -R root:root $LFS/lib64 ;;
esac
```

**VFS(Virtual File System) 생성**

```bash
### vfs (/dev, /proc, /sys, /run) 생성
mkdir -pv $LFS/{dev,proc,sys,run}

### /dev mount & populating
mount -v --bind /dev $LFS/dev

### 나머지 VFS mount
mount -vt devpts devpts -o gid=5,mode=0620 $LFS/dev/pts
mount -vt proc proc $LFS/proc
mount -vt sysfs sysfs $LFS/sys
mount -vt tmpfs tmpfs $LFS/run

### shm(공유 메모리 파일 시스템) 설정
# /dev/shm 이 심볼릭 링크인지 확인하고, 심볼릭 링크라면 실제 경로로 디렉터리 생성, 아닌 경우 tmpfs 파일 시스템을 마운트 하여 시스템 메모리에서 공간을 할당해 줌
if [ -h $LFS/dev/shm ]; then
  install -v -d -m 1777 $LFS$(realpath /dev/shm)
else
  mount -vt tmpfs -o nosuid,nodev tmpfs $LFS/dev/shm
fi
```

**chroot 진입**

```
chroot "$LFS" /usr/bin/env -i   \
    HOME=/root                  \
    TERM="$TERM"                \
    PS1='(lfs chroot) \u:\w\$ ' \
    PATH=/usr/bin:/usr/sbin     \
    MAKEFLAGS="-j_`$(nproc)`_"      \
    TESTSUITEFLAGS="-j_`$(nproc)`_" \
    /bin/bash --login

# I have no name! 이 출력되는경우 정상적으로 진입된것 (/etc/passwd 가 아직 만들어지지 않았을)
```

**디렉터리 생성**

``` bash
### root-level 디렉터리 생성
mkdir -pv /{boot,home,mnt,opt,srv}

### subdirectories 생성
mkdir -pv /etc/{opt,sysconfig}
mkdir -pv /lib/firmware
mkdir -pv /media/{floppy,cdrom}
mkdir -pv /usr/{,local/}{include,src}
mkdir -pv /usr/lib/locale
mkdir -pv /usr/local/{bin,lib,sbin}
mkdir -pv /usr/{,local/}share/{color,dict,doc,info,locale,man}
mkdir -pv /usr/{,local/}share/{misc,terminfo,zoneinfo}
mkdir -pv /usr/{,local/}share/man/man{1..8}
mkdir -pv /var/{cache,local,log,mail,opt,spool}
mkdir -pv /var/lib/{color,misc,locate}

### /run /var/run, /run/lock /var/lock 연결
ln -sfv /run /var/run
ln -sfv /run/lock /var/lock

## 권한 설정
install -dv -m 0750 /root
install -dv -m 1777 /tmp /var/tmp
```

**permission mode**

0755:

    소유자(owner): 7 = 4(읽기) + 2(쓰기) + 1(실행) → rwx
    그룹(group): 5 = 4(읽기) + 1(실행) → r-x
    기타 사용자(others): 5 = 4(읽기) + 1(실행) → r-x

0750:

    소유자: rwx
    그룹: r-x
    기타 사용자: --- (권한 없음)

1777:

    sticky bit가 설정된 경우로, 모든 사용자에게 rwx 권한이 있지만, sticky bit(1)가 설정되어 각 사용자는 자신의 파일만 삭제할 수 있습니다. 주로 /tmp 같은 임시 디렉터리에서 사용됩니다.

**필수 파일, symlink 생성**

``` bash
### mtab symlink 생성
ln -sv /proc/self/mounts /etc/mtab

### /etc/hosts 생성
cat > /etc/hosts << EOF
127.0.0.1  localhost $(hostname)
::1        localhost
EOF

### /etc/passwd 생성
cat > /etc/passwd << "EOF"
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/dev/null:/usr/bin/false
daemon:x:6:6:Daemon User:/dev/null:/usr/bin/false
messagebus:x:18:18:D-Bus Message Daemon User:/run/dbus:/usr/bin/false
uuidd:x:80:80:UUID Generation Daemon User:/dev/null:/usr/bin/false
nobody:x:65534:65534:Unprivileged User:/dev/null:/usr/bin/false
EOF

### /etc/group 생성
cat > /etc/group << "EOF"
root:x:0:
bin:x:1:daemon
sys:x:2:
kmem:x:3:
tape:x:4:
tty:x:5:
daemon:x:6:
floppy:x:7:
disk:x:8:
lp:x:9:
dialout:x:10:
audio:x:11:
video:x:12:
utmp:x:13:
cdrom:x:15:
adm:x:16:
messagebus:x:18:
input:x:24:
mail:x:34:
kvm:x:61:
uuidd:x:80:
wheel:x:97:
users:x:999:
nogroup:x:65534:
EOF

### locale 생성
localedef -i C -f UTF-8 C.UTF-8

### 임시 유저 생성
echo "tester:x:101:101::/home/tester:/bin/bash" >> /etc/passwd
echo "tester:x:101:" >> /etc/group
install -o tester -d /home/tester

### start new shell
exec /usr/bin/bash --login

### login, agetty, init programs 를 위한 log file init
touch /var/log/{btmp,lastlog,faillog,wtmp}
chgrp -v utmp /var/log/lastlog
chmod -v 664  /var/log/lastlog
chmod -v 600  /var/log/btmp
```

### 추가 cross compiling temporary tools build

**Gettext**

```bash
### chmod 환경에서, root로 로그인했다고 가정
cd /sources
tar -xf gettext-0.22.5.tar.xz
cd gettext-0.22.5

### configure
./configure --disable-shared

### build
make

### install msgfmt msgmerge xgettext
cp -v gettext-tools/src/{msgfmt,msgmerge,xgettext} /usr/bin
```

**Bison**

``` bash
### chmod 환경에서, root로 로그인했다고 가정
cd /sources
tar -xf bison-3.8.2.tar.xz
cd bison-3.8.2

### configure
./configure --prefix=/usr \
            --docdir=/usr/share/doc/bison-3.8.2

### build & install
make
make install
```

**Perl**

``` bash
### chmod 환경에서, root로 로그인했다고 가정
cd /sources
tar -xf perl-5.40.0.tar.xz
cd perl-5.40.0

### configure
sh Configure -des                                         \
             -D prefix=/usr                               \
             -D vendorprefix=/usr                         \
             -D useshrplib                                \
             -D privlib=/usr/lib/perl5/5.40/core_perl     \
             -D archlib=/usr/lib/perl5/5.40/core_perl     \
             -D sitelib=/usr/lib/perl5/5.40/site_perl     \
             -D sitearch=/usr/lib/perl5/5.40/site_perl    \
             -D vendorlib=/usr/lib/perl5/5.40/vendor_perl \
             -D vendorarch=/usr/lib/perl5/5.40/vendor_perl

### make & make install
make
make install
```

**Python**

``` bash
### chmod 환경에서, root 로 로그인했다고 가정
cd /sources
tar -xf Python-3.12.5.tar.xz
cd Python-3.12.5

### configure
./configure --prefix=/usr   \
            --enable-shared \
            --without-ensurepip

### build & install
make
make isntall
```

**Texinfo**

``` bash
### chmod 환경에서, root로 로그인했다고 가정
cd /sources
tar -xf texinfo-7.1.tar.xz
cd texinfo-7.1

### configure
./configure --prefix=/usr

### build & install
make
make install
```

**Util-linux**

```bash
### chmod 환경에서, root로 로그인했다고 가정
cd /sources
tar -xf util-linux-2.40.2.tar.xz
cd util-linux-2.40.2

### hwclock dir 생성
mkdir -pv /var/lib/hwclock

### configure
./configure --libdir=/usr/lib     \
            --runstatedir=/run    \
            --disable-chfn-chsh   \
            --disable-login       \
            --disable-nologin     \
            --disable-su          \
            --disable-setpriv     \
            --disable-runuser     \
            --disable-pylibmount  \
            --disable-static      \
            --disable-liblastlog2 \
            --without-python      \
            ADJTIME_PATH=/var/lib/hwclock/adjtime \
            --docdir=/usr/share/doc/util-linux-2.40.2

### build & install
make
make install
```


### 임시 파일 정리 & backup 생성

**임시 파일 정리**

``` bash
### documentation file remove
rm -rf /usr/share/{info,man,doc}/*

### .la 파일 (libltdl 에서만 사용) 삭제
find /usr/{lib,libexec} -name \*.la -delete

### /tools dir 삭제
rm -rf /tools
```

**백업**

```bash
### chroot 환경 나가기
exit

### VFS unmount
mountpoint -q $LFS/dev/shm && umount $LFS/dev/shm
umount $LFS/dev/pts
umount $LFS/{sys,proc,run,dev}

### backup archive 생성
cd $LFS
tar -cJpf $HOME/lfs-temp-tools-12.2.tar.xz .
```

**복원 하는 방법**

``` bash
cd $LFS

# rm -rf 하기 전에 꼭 현재 디렉터리 어디에 있는지 확인

####
rm -rf ./*
####

# 직접 rm -rf 명령보다는 if문으로 두번 체크
if [ $(pwd) = "/mnt/lfs" ]; then
	rm -rf ./*
fi

tar -xpf $HOME/lfs-temp-tools-12.2.tar.xz
```

**VFS re-mount**

``` bash
mount -v --bind /dev $LFS/dev

mount -vt devpts devpts -o gid=5,mode=0620 $LFS/dev/pts
mount -vt proc proc $LFS/proc
mount -vt sysfs sysfs $LFS/sys
mount -vt tmpfs tmpfs $LFS/run

if [ -h $LFS/dev/shm ]; then
  install -v -d -m 1777 $LFS$(realpath /dev/shm)
else
  mount -vt tmpfs -o nosuid,nodev tmpfs $LFS/dev/shm
fi
```

**chroot 재진입**

```bash
chroot "$LFS" /usr/bin/env -i   \
    HOME=/root                  \
    TERM="$TERM"                \
    PS1='(lfs chroot) \u:\w\$ ' \
    PATH=/usr/bin:/usr/sbin     \
    MAKEFLAGS="-j$(nproc)"      \
    TESTSUITEFLAGS="-j$(nproc)" \
    /bin/bash --login
```

## building LFS system

### install basic system software

**설치할 프로그램 목록**

| NAME                 | DESC                                           |
| -------------------- | ---------------------------------------------- |
| Man-pages            | 메뉴얼 페이지 제공                                     |
| Iana-Etc             | 네트워크 관련 서비스 포트와 프로토콜 데이터를 관리하는 파일 모음           |
| Glibc                | 표준 C라이브러리                                      |
| Zlib                 | 데이터 압축을 위한 라이브러리                               |
| Bzip2                | 고압축률의 데이터 압축 도구                                |
| Xz                   | LZMA 알고리즘을 사용하는 압축 도구 및 라이브러리                  |
| Lz4                  | 매우 빠른 압축 및 압축 해제 라이브러리                         |
| Zstd                 | 빠른 압축과 높은 압축률을 제공하는 압축 도구                      |
| File                 | 파일의 타입을 확인하는 도구                                |
| Readline             | 명령어 입력 시 키보드 입력을 처리하는 라이브러리                    |
| M4                   | 매크로 프로세서                                       |
| Bc                   | 임의의 정밀도를 가진 수학 연산을 제공하는 계산기 언어                 |
| Flex                 | 텍스트 패턴을 분석하는 프로그램(lexer) 생성 도구                 |
| Tcl                  | 간단하고 효율적인 스크립트 언어                              |
| Expect               | 자동화된 대화형 프로그램을 작성할 수 있게 해 주는 도구                |
| DejaGNU              | GNU 소프트웨어 테스트 도구                               |
| Pkgconf              | 패키지 설정을 위한 유틸리티, 패키지 의존성 관리                    |
| Binutils             | 바이너리 파일을 처리하는 다양한 도구 모음                        |
| GMP                  | 고정 및 부동 소숫점 연산을 지원하는 수학 라이브러리                  |
| MPFR                 | 부동소수점 수학 연산을 지원하는 라이브러리                        |
| MPC                  | 복소수 연산을 지원하는 라이브러리                             |
| Attr                 | 파일에 확장 속성을 설정 및 관리하는 도구                        |
| Acl                  | 파일에 대한 엑세스 제어 목록(ACL)을 설정하고 관리                 |
| Libcap               | 리눅스 파일 및 프로세스에 대한 권한 설정을 관리하는 라이브러리            |
| Libxcrypt            | 암호화 라이브러리, 암호 해시 함수를 제공                        |
| Shadow               | 사용자 계정 및 비밀번호 관리                               |
| GCC                  | GNU Compiler Collection, C, C++ 기타 언어를 위한 컴파일러 |
| Ncurses              | 터미널에서 GUI처럼 동작하는 프로그램을 만들기 위한 라이브러리            |
| Sed                  | 스트림 편집기                                        |
| Psmisc               | 프로세스 관리와 관련된 유틸리티 모음                           |
| Gettext              | 프로그램에서 다국어 지원을 추가하는 도구                         |
| Bison                | 문법 분석기를 생성하는 도구                                |
| Grep                 | 텍스트 파일에서 문자열을 검색하는 도구                          |
| Bash                 | Bourne Again SHell                             |
| Libtool              | 여러 플랫폼 간의 라이브러리 관리를 단순화하는 도구                   |
| GDBM                 | GNU 데이터베이스 관리 라이브러리                            |
| Gperf                | 최적의 해시 함수를 생성하는 도구                             |
| Expat                | XML 파싱을 위한 라이브러리                               |
| Inetutils            | 네트워크 유틸리티 모음(ping, ftp, telnet 등)              |
| Less                 | 긴 파일을 스크롤하며 볼 수 있게 해줌                          |
| Perl                 | 스크립트 언어                                        |
| XML::Parser          | XML 파일을 처리하기 위한 Perl 모듈                        |
| Inittool             | 국제화 관련 파일을 처리하는 도구                             |
| Autoconf             | 소프트웨어 패키지의 컴파일 설정을 자동으로 해 주는 도구                |
| Automake             | Makefile을 자동으로 생성하는 도구                         |
| OpenSSL              | SSL/TLS 및 암호화 기능을 제공하는 라이브러리                   |
| Kmod                 | 커널 모듈을 관리하는 도구                                 |
| Libelf from Elfutils | ELF 형식의 바이너리 파일을 처리하는 라이브러리                    |
| Libffi               | 외부 함수 호출을 지원하는 라이브러리                           |
| Python               | 파이썬                                            |
| Flit-Core            | 파이썬 패키징 도구                                     |
| Wheel                | 파이썬 패키지 포맷                                     |
| SetupTools           | 파이썬 패키지를 설치하고 관리하는 도구                          |
| Ninja                | Make 보다 빠른 빌드 시스템                              |
| Meson                | 고성능 빌드 시스템                                     |
| Coreutils            | 리눅스의 기본 유틸리티 모음(ls, cp, mv 등)                  |
| Check                | 유닛 테스트 프레임워크                                   |
| Diffutils            | 파일 간 차이를 비교하는 도구                               |
| Gawk                 | 텍스트 패턴 스캐닝 및 처리 언어                             |
| Findutils            | 파일을 검색하는 도구 모음                                 |
| Groff                | 문서 형식화 도구                                      |
| GRUB                 | 부트로더                                           |
| Gzip                 | 파일 압축 도구                                       |
| IPRoute2             | 네트워크 구성 및 제어 도구 모음                             |
| Kbd                  | 키보드 매핑을 관리하는 도구                                |
| Libpipeline          | 파이프라인 처리 도구                                    |
| Make                 | 소프트웨어 빌드 도구                                    |
| Patch                | 패치 파일 적용하는 도구                                  |
| Tar                  | 파일 아카이빙 도구                                     |
| Texinfo              | 문서화 도구                                         |
| Vim                  | 텍스트 편집기                                        |
| MarkupSafe           | 템플릿 렌더링에서 안전한 문자열 처리를 위한 파이썬 모듈                |
| Jinja                | 파이썬의 템플릿 엔진                                    |
| Udev from Systemd    | 장치 관리 데몬                                       |
| Man-DB               | 메뉴얼 페이지 데이터베이스 관리 도구                           |
| Procps-ng            | 시스템 프로세스 및 자원 모니터링                             |
| Util-linux           | 여러 유틸리티가 포함된 필수 시스템 도구 모음                      |
| E2fsprogs            | Ext2/3/4 파일 시스템을 위한 유틸리티                       |
| Sysklogd             | 시스템 로그 데몬                                      |
| sysVinit             | 전통적인 System V 방식의 init 시스템                     |

**man-pages (1/80)**

``` bash
cd /sources
tar -xf man-pages-6.9.1.tar.xz
cd man-pages-6.9.1

### password hashing function 관련 man page 제거
rm -v man3/crypt*

### install man page
make prefix=/usr install
```

**Iana-Etc(2/80)**

``` bash
cd /sources
tar -xf iana-etc-20240806.tar.gz
cd iana-etc-20240806

### copy files
cp services protocols /etc
```

**Glibc(3/80)**

```bash
cd /sources/glibc-2.40

### patch 적용
patch -Np1 -i ../glibc-2.40-fhs-1.patch

### build 재생성
rm -rf ./build
mkdir -v ./build
cd build

### configure
echo "rootsbindir=/usr/sbin" > configparms

../configure --prefix=/usr                            \
             --disable-werror                         \
             --enable-kernel=4.19                     \
             --enable-stack-protector=strong          \
             --disable-nscd                           \
             libc_cv_slibdir=/usr/lib

### compile
make

### check
make check

### id.so.conf 생성
touch /etc/ld.so.conf

### skip outdated sanity check
sed '/test-installation/s@$(PERL)@echo not running@' -i ../Makefile

### install
make install

### fix hardcoded path
sed '/RTLDLIST=/s@/usr@@g' -i /usr/bin/ldd

### add locale
make localedata/install-locales

localedef -i C -f UTF-8 C.UTF-8
localedef -i ja_JP -f SHIFT_JIS ja_JP.SJIS 2> /dev/null || true

### Glibc configuration
### nsswitch.conf 생성
cat > /etc/nsswitch.conf << "EOF"
# Begin /etc/nsswitch.conf

passwd: files
group: files
shadow: files

hosts: files dns
networks: files

protocols: files
services: files
ethers: files
rpc: files

# End /etc/nsswitch.conf
EOF

### Time Zone Data 추가
tar -xf ../../tzdata2024a.tar.gz

ZONEINFO=/usr/share/zoneinfo
mkdir -pv $ZONEINFO/{posix,right}

for tz in etcetera southamerica northamerica europe africa antarctica  \
          asia australasia backward; do
    zic -L /dev/null   -d $ZONEINFO       ${tz}
    zic -L /dev/null   -d $ZONEINFO/posix ${tz}
    zic -L leapseconds -d $ZONEINFO/right ${tz}
done

cp -v zone.tab zone1970.tab iso3166.tab $ZONEINFO
zic -d $ZONEINFO -p America/New_York
unset ZONEINFO

### timezone 설정
tzselect

### localtime 파일 생성
ln -sfv /usr/share/zoneinfo/Asia/Seoul /etc/localtime

### dynamic loader 설정
cat > /etc/ld.so.conf << "EOF"
# Begin /etc/ld.so.conf
/usr/local/lib
/opt/lib

# Add an include directory
include /etc/ld.so.conf.d/*.conf

EOF

mkdir -pv /etc/ld.so.conf.d
```

**Zlib (4/80)**

```bash
cd /sources
tar -xf zlib-1.3.1.tar.gz
cd zlib-1.3.1

### configure
./configure --prefix=/usr

### build & install
make
make check
make install

### 필요 없는 정적 라이브러리 제거
rm -fv /usr/lib/libz.a
```

**Bzip2 (5/80)**

```bash
cd /sources
tar -xf bzip2-1.0.8.tar.gz
cd bzip2-1.0.8

### patch 적용 (도큐먼트 설치해줌)
patch -Np1 -i ../bzip2-1.0.8-install_docs-1.patch

### symlink 수정
sed -i 's@\(ln -s -f \)$(PREFIX)/bin/@\1@' Makefile

### man page 설치 경로 수정
sed -i "s@(PREFIX)/man@(PREFIX)/share/man@g" Makefile

### Bzip2 컴파일 준비
make -f Makefile-libbz2_so
make clean

### build & install
make
make PREFIX=/usr install

cp -av libbz2.so.* /usr/lib
ln -sv libbz2.so.1.0.8 /usr/lib/libbz2.so

cp -v bzip2-shared /usr/bin/bzip2
for i in /usr/bin/{bzcat,bunzip2}; do
  ln -sfv bzip2 $i
done

### 필요 없는 정적 라이브러리 제거
rm -fv /usr/lib/libbz2.a
```

**Xz (6/80)**

```bash
cd /sources/xz-5.6.2

### configure
./configure --prefix=/usr    \
            --disable-static \
            --docdir=/usr/share/doc/xz-5.6.2

### build & install
make
make check
make install
```

**Lz4 (7/80)**

``` bash
cd /sources
tar -xf lz4-1.10.0.tar.gz
cd lz4-1.10.0

### build & install
make BUILD_STATIC=no PREFIX=/usr
make -j1 check
make BUILD_STATIC=no PREFIX=/usr install
```

**Zstd (8/80)**

``` bash
cd /sources
tar -xf zstd-1.5.6.tar.gz
cd zstd-1.5.6

### build & install
make prefix=/usr
make check
make prefix=/usr install

### 필요 없는 정적 파일 삭제
rm -v /usr/lib/libzstd.a
```

**File (9/80)**

``` bash
cd /sources/file-5.45

### configure
./configure --prefix=/usr

### build & install
make
make check
make install
```

**Readline (10/80)**

```bash
cd /sources
tar -xf readline-8.2.13.tar.gz
cd readline-8.2.13

### <libname>.old resolve
sed -i '/MV.*old/d' Makefile.in
sed -i '/{OLDSUFF}/c:' support/shlib-install

### rpath 하드코딩 방지
sed -i 's/-Wl,-rpath,[^ ]*//' support/shobj-conf

### configure
./configure --prefix=/usr    \
            --disable-static \
            --with-curses    \
            --docdir=/usr/share/doc/readline-8.2.13

### build & install
make SHLIB_LIBS="-lncursesw"
make SHLIB_LIBS="-lncursesw" install
```

**M4 (11/80)**

```bash
cd /sources/m4-1.4.19

### configure
./configure --prefix=/usr

### build & install
make
make check
make install
```

**Bc (12/80)**

```bash
cd /sources
tar -xf bc-6.7.6.tar.xz
cd bc-6.7.6

### configure
CC=gcc ./configure --prefix=/usr -G -O3 -r

### build & install
make
make test
make install
```

**Flex (13/80)**

``` bash
cd /sources
tar -xf flex-2.6.4.tar.gz
cd flex-2.6.4

### configure
./configure --prefix=/usr \
            --docdir=/usr/share/doc/flex-2.6.4 \
            --disable-static

### build & install
make
make check
make install

### lex -> flex 심링크 생성
ln -sv flex   /usr/bin/lex
ln -sv flex.1 /usr/share/man/man1/lex.1
```

**Tcl (14/80)**

``` bash
cd /sources
tar -xf tcl8.6.14-src.tar.gz
cd tcl8.6.14

### configure
SRCDIR=$(pwd)
cd unix
./configure --prefix=/usr           \
            --mandir=/usr/share/man \
            --disable-rpath

### build
make

sed -e "s|$SRCDIR/unix|/usr/lib|" \
    -e "s|$SRCDIR|/usr/include|"  \
    -i tclConfig.sh

sed -e "s|$SRCDIR/unix/pkgs/tdbc1.1.7|/usr/lib/tdbc1.1.7|" \
    -e "s|$SRCDIR/pkgs/tdbc1.1.7/generic|/usr/include|"    \
    -e "s|$SRCDIR/pkgs/tdbc1.1.7/library|/usr/lib/tcl8.6|" \
    -e "s|$SRCDIR/pkgs/tdbc1.1.7|/usr/include|"            \
    -i pkgs/tdbc1.1.7/tdbcConfig.sh

sed -e "s|$SRCDIR/unix/pkgs/itcl4.2.4|/usr/lib/itcl4.2.4|" \
    -e "s|$SRCDIR/pkgs/itcl4.2.4/generic|/usr/include|"    \
    -e "s|$SRCDIR/pkgs/itcl4.2.4|/usr/include|"            \
    -i pkgs/itcl4.2.4/itclConfig.sh

unset SRCDIR

make test
make install

### 설치된 라이브러리 권한 부여
chmod -v u+w /usr/lib/libtcl8.6.so

### Tcl 헤더 install
make install-private-headers

### 심링크 생성
ln -sfv tclsh8.6 /usr/bin/tclsh

### man page 이름변경 (Perl man page 와 conflict)
mv /usr/share/man/man3/{Thread,Tcl_Thread}.3
```

**Expect (15/80)**

```bash
cd /sources
tar -xf expect5.45.4.tar.gz
cd expect5.45.4

### PTY check
python3 -c 'from pty import spawn; spawn(["echo", "ok"])'
# ok 가 출력되지 않는다면, chroot 환경에서 나간 뒤, VFS 마운트 다시 확인해야함

### patch 적용
patch -Np1 -i ../expect-5.45.4-gcc14-1.patch

### configure
./configure --prefix=/usr           \
            --with-tcl=/usr/lib     \
            --enable-shared         \
            --disable-rpath         \
            --mandir=/usr/share/man \
            --with-tclinclude=/usr/include

### build & install
make
make test
make install
ln -svf expect5.45.4/libexpect5.45.4.so /usr/lib
```

**DejaGNU (16/80)**

``` bash
cd /sources
tar -xf dejagnu-1.6.3.tar.gz
cd dejagnu-1.6.3

### build dir 생성
mkdir -v build
cd build

### configure
../configure --prefix=/usr
makeinfo --html --no-split -o doc/dejagnu.html ../doc/dejagnu.texi
makeinfo --plaintext       -o doc/dejagnu.txt  ../doc/dejagnu.texi

### test
make check

### install
make install
install -v -dm755  /usr/share/doc/dejagnu-1.6.3
install -v -m644   doc/dejagnu.{html,txt} /usr/share/doc/dejagnu-1.6.3
```

**Pkgconf (17/80)**

``` bash
cd /sources
tar -xf pkgconf-2.3.0.tar.xz
cd pkgconf-2.3.0

### configure
./configure --prefix=/usr              \
            --disable-static           \
            --docdir=/usr/share/doc/pkgconf-2.3.0

### build & install
make
make install

### original Pkg-config 을 위한 심링크 생성
ln -sv pkgconf   /usr/bin/pkg-config
ln -sv pkgconf.1 /usr/share/man/man1/pkg-config.1
```

**Binutils (18/80)**

``` bash
cd /sources/binutils-2.43.1

### 이전에 생성한 build dir 삭제 & 재생성
rm -rf ./build
mkdir -v build
cd build

### configure
../configure --prefix=/usr       \
             --sysconfdir=/etc   \
             --enable-gold       \
             --enable-ld=default \
             --enable-plugins    \
             --enable-shared     \
             --disable-werror    \
             --enable-64-bit-bfd \
             --enable-new-dtags  \
             --with-system-zlib  \
             --enable-default-hash-style=gnu

### build & install
make tooldir=/usr
make -k check

### test 결과 확인
grep '^FAIL:' $(find -name '*.log')

make tooldir=/usr install

### 필요 없는 정적 라이브러리 삭제
rm -fv /usr/lib/lib{bfd,ctf,ctf-nobfd,gprofng,opcodes,sframe}.a
```

**GMP (19/80)**

``` bash
cd /sources
tar -xf gmp-6.3.0.tar.xz
cd gmp-6.3.0

### configure
./configure --prefix=/usr    \
            --enable-cxx     \
            --disable-static \
            --docdir=/usr/share/doc/gmp-6.3.0

### build & install
make
make html
make check 2>&1 | tee gmp-check-log

awk '/# PASS:/{total+=$3} ; END{print total}' gmp-check-log
# 199개 이상 테스트 통과해야함

make install
make install-html
```

**MPFR (20/80)**

```bash
cd /sources
tar -xf mpfr-4.2.1.tar.xz
cd mpfr-4.2.1

### configure
./configure --prefix=/usr        \
            --disable-static     \
            --enable-thread-safe \
            --docdir=/usr/share/doc/mpfr-4.2.1

### build & install
make
make html
make check
make install
make install-html
```

**Mpc (21/80)**

```bash
cd /sources
tar -xf mpc-1.3.1.tar.gz
cd mpfr-1.3.1

### configure
./configure --prefix=/usr    \
            --disable-static \
            --docdir=/usr/share/doc/mpc-1.3.1

### build & install
make
make html
make check
make install
make install-html
```

**Attr (22/80)** 

``` bash
cd /sources
tar -xf attr-2.5.2.tar.gz
cd attr-2.5.2

### configure
./configure --prefix=/usr     \
            --disable-static  \
            --sysconfdir=/etc \
            --docdir=/usr/share/doc/attr-2.5.2

### build & install
make
make check
make install
```

**Acl(23/80)**

``` bash
cd /sources
tar -xf acl-2.3.2.tar.xz
cd acl-2.3.2

### configure
./configure --prefix=/usr         \
            --disable-static      \
            --docdir=/usr/share/doc/acl-2.3.2

### build & install
make
make install

##### Coreutils 패키지가 설치된 후 make check 가능함
```

**Libcap (24/80)**

``` bash
cd /sources
tar -xf libcap-2.70.tar.xz
cd libcap-2.70

### static library 설치 목록에서 제거
sed -i '/install -m.*STA/d' libcap/Makefile

### build $ install
make prefix=/usr lib=lib
make test
make prefix=/usr lib=lib install
```

**Libxcrypt (25/80)**

``` bash
cd /sources
tar -xf libxcrypt-4.4.36.tar.xz
cd libxcrypt-4.4.36

### configure
./configure --prefix=/usr                \
            --enable-hashes=strong,glibc \
            --enable-obsolete-api=no     \
            --disable-static             \
            --disable-failure-tokens

### build & install
make
make check
make install
```

**Shadow (26/80)**

``` bash
cd /sources
tar -xf shadow-4.16.0.tar.xz
cd shadow-4.16.0

### disable groups program
sed -i 's/groups$(EXEEXT) //' src/Makefile.in
find man -name Makefile.in -exec sed -i 's/groups\.1 / /'   {} \;
find man -name Makefile.in -exec sed -i 's/getspnam\.3 / /' {} \;
find man -name Makefile.in -exec sed -i 's/passwd\.5 / /'   {} \;

### YESCRYPT 사용
sed -e 's:#ENCRYPT_METHOD DES:ENCRYPT_METHOD YESCRYPT:' \
    -e 's:/var/spool/mail:/var/mail:'                   \
    -e '/PATH=/{s@/sbin:@@;s@/bin:@@}'                  \
    -i etc/login.defs

### passwd 생성
touch /usr/bin/passwd

### configure
./configure --sysconfdir=/etc   \
            --disable-static    \
            --with-{b,yes}crypt \
            --without-libbsd    \
            --with-group-name-max-length=32

### build & install
make
make exec_prefix=/usr install
make -C man install-man

### configuring shadow

# enable shadowed passwords
pwconv

# enable shadowed group passwd
grpconv

mkdir -p /etc/default
useradd -D --gid 999

sed -i '/MAIL/s/yes/no/' /etc/default/useradd

# root passwd 설정
passwd root
```

**gcc (27/80)**

```bash
cd /sources/gcc-14.2.0

### lib64 이름변경
case $(uname -m) in
  x86_64)
    sed -e '/m64=/s/lib64/lib/' \
        -i.orig gcc/config/i386/t-linux64
  ;;
esac

### build dir 재생성
rm -rf ./build
mkdir -v build
cd build

### configure
../configure --prefix=/usr            \
             LD=ld                    \
             --enable-languages=c,c++ \
             --enable-default-pie     \
             --enable-default-ssp     \
             --enable-host-pie        \
             --disable-multilib       \
             --disable-bootstrap      \
             --disable-fixincludes    \
             --with-system-zlib

### build
make

### gcc limit 해제
ulimit -s -H unlimited

### test failures fix
sed -e '/cpython/d'               -i ../gcc/testsuite/gcc.dg/plugin/plugin.exp
sed -e 's/no-pic /&-no-pie /'     -i ../gcc/testsuite/gcc.target/i386/pr113689-1.c
sed -e 's/300000/(1|300000)/'     -i ../libgomp/testsuite/libgomp.c-c++-common/pr109062.c
sed -e 's/{ target nonpic } //' \
    -e '/GOTPCREL/d'              -i ../gcc/testsuite/gcc.target/i386/fentryname3.c

### tester 유저 이용, 테스트
chown -R tester .
su tester -c "PATH=$PATH make -k check"

2### 테스트 결과 확인
../contrib/test_summary

### install
make install

### tester 에서 root 로 ownership 변경
chown -v -R root:root \
    /usr/lib/gcc/$(gcc -dumpmachine)/14.2.0/include{,-fixed}

### 심링크 생성
ln -svr /usr/bin/cpp /usr/lib

### cc -> gcc 심링크 생성
ln -sv gcc.1 /usr/share/man/man1/cc.1

### LTO 심링크
ln -sfv ../../libexec/gcc/$(gcc -dumpmachine)/14.2.0/liblto_plugin.so \
        /usr/lib/bfd-plugins/

### sanity check
echo 'int main(){}' > dummy.c
cc dummy.c -v -Wl,--verbose &> dummy.log
readelf -l a.out | grep ': /lib'

# 에러가 없어야함, [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2] 이게 마지막 출력이어야함

grep -E -o '/usr/lib.*/S?crt[1in].*succeeded' dummy.log

# /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.0/../../../../lib/Scrt1.o succeeded
# /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.0/../../../../lib/crti.o succeeded
# /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.0/../../../../lib/crtn.o succeeded

grep -B4 '^ /usr/include' dummy.log

#include <...> search starts here:
# /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.0/include
# /usr/local/include
# /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.0/include-fixed
# /usr/include

grep 'SEARCH.*/usr/lib' dummy.log |sed 's|; |\n|g'

# SEARCH_DIR("/usr/x86_64-pc-linux-gnu/lib64")
# SEARCH_DIR("/usr/local/lib64")
# SEARCH_DIR("/lib64")
# SEARCH_DIR("/usr/lib64")
# SEARCH_DIR("/usr/x86_64-pc-linux-gnu/lib")
# SEARCH_DIR("/usr/local/lib")
# SEARCH_DIR("/lib")
# SEARCH_DIR("/usr/lib");

grep "/lib.*/libc.so.6 " dummy.log

# attempt to open /usr/lib/libc.so.6 succeeded

grep found dummy.log

# found ld-linux-x86-64.so.2 at /usr/lib/ld-linux-x86-64.so.2

### 확인 완료 후,
rm -v dummy.c a.out dummy.log

### 파일 이동
mkdir -pv /usr/share/gdb/auto-load/usr/lib
mv -v /usr/lib/*gdb.py /usr/share/gdb/auto-load/usr/lib
```2