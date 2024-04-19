### 디스크 정리

`lsblk` 명령으로 디스크 확인

`dd if=/dev/zero of=/dev/MY_DISK status=progress bs=4M` 또는 `wipefs -all --force /dev/MY_DISK` 명령으로 디스크 삭제

`cfdisk /dev/MY_DISK` 명령으로 파티션 진행

 **swap 사이즈 table**

|  RAM SIZE  | Suspend Support  | Hibernation Support |
| :--------: | :--------------: | :-----------------: |
|   2GB 이하   |     2 * RAM      |       3 * RAM       |
| 2GB ~ 8GB  |       RAM        |       2 * RAM       |
| 8GB ~ 64GB | 8GB min ~ 16 max |      1.5 * RAM      |
|  64GB 이상   |      8 min       |  Hibernation 추천 안함  |

**파티션 진행**
gpt 레이블 선택 후
EFI sysmtem (boot) 1G
Linux swap 16G (테이블 보고 결정)
Linux filesystem 나머지

```bash
#lsblk 출력

sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0     1G  0 part
├─sda2   8:2    0    16G  0 part
└─sda3   8:3    0 221.5G  0 part
```

**파티션 포맷**
`mkfs.vfat -F32 /dev/EFI_BOOT_PARTITION`
`mkfs.ext4 -j /dev/FILE_SYSTEM_PARTITION`

**스왑 파티션 생성 + 적용**
`mkswap /dev/SWAP_PARTITION`
`swapon /dev/SWAP_PARTITION`

```bash
# file -sL /dev/sda* 출력
/dev/sda:  DOS/MBR boot sector; partition 1 : ID=0xee, start-CHS (0x0,0,2), end-CHS (0x3ff,255,63), startsector 1, 500118191 sectors, extended partition table (last)
/dev/sda1: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "mkfs.fat", sectors/cluster 8, Media descriptor 0xf8, sectors/track 63, heads 255, hidden sectors 2048, sectors 2097144 (volumes > 32 MB), FAT (32 bit), sectors/FAT 2048, serial number 0x5b15f620, unlabeled
/dev/sda2: Linux swap file, 4k page size, little endian, version 1, size 4194303 pages, 0 bad pages, no label, UUID=74ac9d43-9bf7-44e8-8b7d-fcaf52623eb8
/dev/sda3: Linux rev 1.0 ext4 filesystem data, UUID=a7adc58c-9dcd-44da-8ea8-24fb597ce62b (extents) (64bit) (large files) (huge files)
```

```bash
# lsblk 출력
sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0     1G  0 part
├─sda2   8:2    0    16G  0 part [SWAP]
└─sda3   8:3    0 221.5G  0 part
```

**파일 파티션 mount 하기**
파일 파티션이 마운트될 폴더 생성
`mkdir --parents /mnt/gentoo`

파일 파티션 마운트
`mount /dev/MY_FILE_PARTITION /mnt/gentoo`

### gentoo stage 파일 설치

파일 파티션이 마운트 되어 있는 폴더(/mnt/gentoo)로 이동
`cd /mnt/gentoo`

`links https://www.gentoo.org/downloads` 로 들어가서 다운로드 받거나 `wget GENTOO_STAGE_FILE_LINK` 로 다운로드 받을 수 있음

**다운로드 받은 압축 파일 압축 해제**
`tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner`

**compile options 설정**
`vi /mnt/gentoo/etc/portage/make.conf`

```
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j4 -l5"

# NOTE: This stage was built with the bindist Use flag enabled

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C.utf8
```

COMMON_FLAGS="-march=MYCPU_NAME" 추가

```bash
# -march 가능한 목록
nocona core2 nehalem corei7 westmere sandybridge corei7-avx ivybridge
core-avx-i haswell core-avx2 broadwell skylake skylake-avx512 cannonlake
icelake-client rocketlake icelake-server cascadelake tigerlake cooperlake sapphirerapids emeraldrapids alderlake raptorlake meteorlake graniterapids graniterapids-d bonnell atom silvermont slm goldmont goldmont-plus
tremont gracemont sierraforest grandridge knl knm x86-64 x86-64-v2 x86-64-v3
x86-64-v4 eden-x2 nano nano-1000 nano-2000 nano-3000 nano-x2 eden-x4 nano-x4 lujiazui k8 k8-sse3 opteron opteron-sse3 athlon64 athlon64-sse3 athlon-fx amdfam10 barcelona bdver1 bdver2 bdver3 bdver4 znver1 znver2 znver3 znver4 btver1 btver2 native
```

MAKEOPTS="-j4 -l5"
-j = ram의 1/2, cpu 코어 수를 넘으면 안됨
-l = cpu 코어 수 보다 살짝 크거나 같아야함

`cat /proc/cpuinfo`명령으로 cpu 확인 가능함

### chrooting

**DNS info 복사**
`cp --dereference /etc/resolv.conf /mnt/gentoo/etc/`

**chroot**
`arch-chroot /mnt/gentoo`

또는(arch-chroot 가 없는 경우)

```
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
mount --bind /run /mnt/gentoo/run
mount --make-slave /mnt/gentoo/run

# non-gentoo install media 사용하는 경우
test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm 

# chmod
chmod 1777 /dev/shm /run/shm
```

**profile 가져오기**
`source /etc/profile; export PS1="(chroot) ${PS1}"`

**boot partition mount**
`mkdir /efi`
`mount /dev/MY_BOOT_PARTITION /efi`

``` bash
# lsblk 출력
sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0     1G  0 part /efi
├─sda2   8:2    0    16G  0 part [SWAP]
└─sda3   8:3    0 221.5G  0 part /
```

### Portage 설정

**gentoo ebuild repository sync에 필요한 파일 가져오기**
`mkdir --parents /etc/portage/repos.conf`
`cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf`

``` bash
# cat /etc/portage/repos.conf/gentoo.conf 출력

[DEFAULT]
main-repo = gentoo

[gentoo]
location = /var/db/repos/gentoo
sync-type = rsync
sync-uri = rsync://rsync.gentoo.org/gentoo-portage
auto-sync = yes
sync-rsync-verify-jobs = 1
sync-rsync-verify-metamanifest = yes
sync-rsync-verify-max-age = 3
sync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc
sync-openpgp-keyserver = hkps://keys.gentoo.org
sync-openpgp-key-refresh-retry-count = 40
sync-openpgp-key-refresh-retry-overall-timeout = 1200
sync-openpgp-key-refresh-retry-delay-exp-base = 2
sync-openpgp-key-refresh-retry-delay-max = 60
sync-openpgp-key-refresh-retry-delay-mult = 4
sync-webrsync-verify-signature = yes
```

**gentoo ebuild repository snapshot 설치**
`emerge-webrsync`

**mirror 추가**
`emerge --ask --verbose --oneshot app-portage/mirrorselect`
`mirrorselect -i -o >> /etc/portage/make.conf` (스페이스 바로 체크가능)

**프로파일 선택**
`eselect profile list` 로 프로파일 리스트 확인하고 `eselect profile set NUMBER` 로 선택 가능

**cpu flag 설정**
`emerge --ask --oneshot app-portage/cpuid2cpuflags`
`echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpu-flags`

**gpu flag 설정**
gpu 카드 확인을 위해 lspci emerge `emerge -avq sys-apps/pciutils`
emerge 완료 후 `lspci | grep -i VGA` 명령으로 확인 가능

`nano /etc/poratage/make.conf` 에 VIDEO_CARD="" 추가해줌

*엔비디아 그래픽 카드의 경우 설치가 모두 완료된 후 설정하는 것을 추천*

**ACCEPT_LICENSE** 수정

@GPL-COMPATIBLE  
@FSF-APPROVED  
@OSI-APPROVED  
@MISC-FREE  
@FREE-SOFTWARE  
@FSF-APPROVED-OTHER  
@MISC-FREE-DOCS  
@FREE-DOCUMENTS.  
@FREE  
@BINARY-REDISTRIBUTABLE  
@EULA
등등등 추가 가능
전체 다 가능하게 하려면 " * " 옵션 주면 됨

`nano /etc/poratage/make.conf` 에 ACCEPT_LICENSE="" 추가해줌

`portageq envvar ACCEPT_LICENSE` 로 적용 여부 확인 가능

``` bash
# cat /etc/portage/make.conf 출력

# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j4 -l5"
VIDEO_CARD="intel"
ACCEPT_LICENSE="*"

# NOTE: This stage was built with the bindist Use flag enabled

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C.utf8

....
```

### world set 업데이트

``` bash
# 오래걸림
emerge --quiet --ask --verbose --update --deep --newuse @world
```

이후 사용할 app 설치
`emerge -avq vim tmux sys-apps/lshw`

**eslect editor 설정**
`eselect editor list`로 가능한 텍스트 에디터 확인
`eselect editor set NUMBER` 로 변경 가능

### timezone 설정

`echo "MY_TIMEZONE01/MY_TIMEZONE02" > /etc/timezone`
`emerge --config sys-libs/timezone-data`

`ls -l /usr/share/zoneinfo` 명령어로 timezone list 확인 가능

### locale 설정

`vim /etc/locale.gen`
``` bash
# cat /etc/locale.gen 출력

# /etc/locale.gen: list all of the locales you want to have on your system.
# See the locale.gen(5) man page for more details.
#
# The format of each line:
# <locale name> <charset>
#
# Where <locale name> starts with a name as found in /usr/share/i18n/locales/.
# It must be unique in the file as it is used as the key to locale variables.
# For non-default encodings, the <charset> is typically appended.
#
# Where <charset> is a charset located in /usr/share/i18n/charmaps/ (sans any
# suffix like ".gz").
#
# All blank lines and lines starting with # are ignored.
#
# For the default list of supported combinations, see the file:
# /usr/share/i18n/SUPPORTED
#
# Whenever glibc is emerged, the locales listed here will be automatically
# rebuilt for you.  After updating this file, you can simply run `locale-gen`
# yourself instead of re-emerging glibc.

#en_US ISO-8859-1
en_US.UTF-8 UTF-8
#ja_JP.EUC-JP EUC-JP
#ja_JP.UTF-8 UTF-8
#ja_JP EUC-JP
#en_HK ISO-8859-1
#en_PH ISO-8859-1
#de_DE ISO-8859-1
#de_DE@euro ISO-8859-15
#es_MX ISO-8859-1
#fa_IR UTF-8
#fr_FR ISO-8859-1
#fr_FR@euro ISO-8859-15
#it_IT ISO-8859-1
```

`locale-gen` 명령으로 로케일 생성

로케일 선택
`eselect locale list`
`eselect locale set NUMBER`

업데이트
`env-update && source /etc/profile && export PS1="(chroot) ${PS1}"`

### firmware 설치

`emerge -avq sys-kernel/linux-firmware`

**SOF(Sound Open Firmware)**
intel에서 나온 Smart Sound Technology (SST)를 대체하기 위해 나옴
10세대 이상, Apollo Lake 등에 적용 가능 [호환 가능한 CPU 목록 확인하기](https://thesofproject.github.io/latest/platforms/index.html)

**Microcode**
cpu의 firmware = microcode
amd의 microcode 는 linux-firmware 패키지를 통해 설치 가능
intel은 `emerge -avq sys-firmware/intel-microcode` 를 통해 설치 가능


### kernel configuration

**Kernel source 설치**
`emerge -avq sys-kernel/installkernel sys-kernel/gentoo-sources`

**커널 (gentoo-sources) 심링크 생성**
`eselect kernel list`
`eselect kernel set NUMBER`

**심링크 생성 확인**
`ls -l /usr/src/linux`

``` bash
# ls -l /usr/src/linux 출력
lrwxrwxrwx 1 root root 19 Apr  4 00:00 /usr/src/linux -> linux-6.6.21-gentoo
```

**kernel configuration**
`cd /usr/src/linux`
`make menuconfig`

**kernel build**
(`/usr/src/linux` dir 에서)
`make`
`make modules_install`
`make install`


**installkernel bootloader 설정변경 자동화**
installkernel use flag 에 사용할 bootloader 추가
`vim /etc/portage/package.use/installkernel`

``` 
# /etc/portage/package.use/installkernel

sys-kernel/installkernel grub
```

**initramfs 생성**
installkernel use flag에 dracut 추가 (kernel build시 자동으로 initramfs 생성해줌)
`vim /etc/portage/package.use/installkernel`

``` 
# /etc/portage/package.use/installkernel

sys-kernel/installkernel dracut grub
```

`emerge --ask --verbose --update --changed-use --deep @world` 또는 `emerge -avq sys-kernel/dracut` 명령으로 수동 설치

`dracut --kver=MY_KERNEL_VERSION` 명령으로 initramfs 생성 가능함 (`ls /boot` 또는 `eslect kernel list`)로 확인 가능

```bash
# ls /boot 출력
config-6.6.21-gentoo  initramfs-6.6.21-gentoo.img  System.map-6.6.21-gentoo  vmlinuz-6.6.21-gentoo

# eselect kernel list 출력
Available kernel symlink targets:
  [1]   linux-6.6.21-gentoo *

# 확인한 버전으로 initramfs 생성
dracut --kver=6.6.21-gentoo
```

**부트로더 설정**
`ls /sys/firmware/efi`로 `efivar`존재여부 확인
`efivar` 있으면 uefi 사용 가능 `/etc/portage/make.conf`파일에 `GRUB_PLATFORMS="efi-64"` 추가

```bash
# cat /etc/portage/make.conf 출력
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j4 -l5"
VIDEO_CARD="intel"
ACCEPT_LICENSE="*"
GRUB_PLATFORMS="efi-64"

# ....
```

`efibootmgr`, `grub` 설치
`emerge -avq sys-boot/efibootmgr sys-boot/grub`

`grub` 설치
UEFI 의 경우 (`efivar` 존재)
`grub-install --efi-directory=/MY_BOOT_PARTITION_MOUNT_POINT --bootloader-id=MY_OS_NAME`

DOS / legacy (`efivar` 없음)
`grub-install /dev/MY_BOOT_PARTITION --bootloader-id=MY_OS_NAME`

`grub.cfg`생성
`grub-mkconfig -o /boot/grub/grub.cfg`

### 시스템 설정

**`fstab` 작성**
`emerge --ask --oneshot sys-fs/genfstab`
`genfstab -U / >> /etc/fstab`

**`hostname`, `passwd` 설정**
`echo MY_HOSTNAME > /etc/hostname`

`/etc/hosts` `127.0.0.1 MY_HOSTNAME` 추가해줌

```bash
#### cat /etc/hostname 출력
gentoo

#### cat /etc/hosts 출력
# /etc/hosts: Local Host Database
#
# This file describes a number of aliases-to-address mappings for the for
# local hosts that share this file.
#
# The format of lines in this file is:
#
# IP_ADDRESS    canonical_hostname      [aliases...]
#
#The fields can be separated by any number of spaces or tabs.
#
# In the presence of the domain name service or NIS, this file may not be
# consulted at all; see /etc/host.conf for the resolution order.
#

# IPv4 and IPv6 localhost aliases
127.0.0.1       gentoo
127.0.0.1       localhost
::1             localhost

#
# Imaginary network.
#10.0.0.2               myname
#10.0.0.3               myfriend
#
# According to RFC 1918, you can use the following IP networks for private
# nets which will never be connected to the Internet:
#
#       10.0.0.0        -   10.255.255.255
#       172.16.0.0      -   172.31.255.255
#       192.168.0.0     -   192.168.255.255
#
# In case you want to be able to connect directly to the Internet (i.e. not
# behind a NAT, ADSL router, etc...), you need real official assigned
# numbers.  Do not try to invent your own network numbers but instead get one
# from your network provider (if any) or from your regional registry (ARIN,
# APNIC, LACNIC, RIPE NCC, or AfriNIC.)
#
```

`passwd`명령으로 패스워드 추가 가능


**user 추가**
`emerge -avq app-admin/sudo`명령으로 sudo emerge

```bash
# user 추가
useradd -m -g users -G wheel -s /bin/bash SOMEUSERNAME

# user password 추가
passwd SOMEUSERNAME
```

**`visudo` 파일 수정**
`EDITOR=vim visudo` 으로 연 다음 `%wheel ALL=(ALL:ALL) ALL` 라인을 찾아서 코멘트 해제 해줌

```bash
##
## User privilege specification
##
root ALL=(ALL:ALL) ALL

## Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL:ALL) ALL

## Same thing without a password
# %wheel ALL=(ALL:ALL) NOPASSWD: ALL

## Uncomment to allow members of group sudo to execute any command
# %sudo ALL=(ALL:ALL) ALL
```

**필수 패키지 설치**
`emerge -avq sysklogd chrony networkmanager wireless-tools`

```bash
# sysklogd (system logger) 설정
rc-update add sysklogd default

# chrony (ntp) 설정
rc-update add chronyd default

# network manager 설정
rc-service NetworkManager start
rc-update add NetworkManager default
```

### mount 해제, reboot

**chroot 환경 exit**
`exit`

**umount**
```bash
# root dir 로 이동
cd ~

# umount
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount -R /mnt/gentoo

# umount 확인
lsblk

# lsblk 출력
sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0     1G  0 part
├─sda2   8:2    0    16G  0 part [SWAP]
└─sda3   8:3    0 221.5G  0 part

# 확인 완료되면, reboot
reboot
```