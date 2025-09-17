
### linux sound processing

```mermaid
graph LR
  program["`**program**`"]
  server["`**server**
    pipewire, pulseaudio, jack ...`"]
  driver["`**driver**
    alsa`"]
  sound_card["`**sound card**`"]
  program --> server
  server --> driver
  driver --> sound_card
```

### sound card & driver(`alsa`) 준비

**kernel 에서 필요한 firmware 추가**

`cd /usr/src/linux` 폴더로 이동 후 `sudo make menuconfig`
`lspci` 명령으로 sound card 종류 확인 가능함

```
Device Drivers --->
    <*> Sound card support
        <*> Advanced Linux Sound Architecture --->
            [*] PCI sound devices  --->
                Select the driver for your audio controller. (lspci 로 확인 가능)
                HD-Audio  --->
                   Select a codec or enable all and let the generic parse choose the right one:
                   [*] Build Realtek HD-audio codec support
                   [*] ...
                   [*] Build Silicon Labs 3054 HD-modem codec support
                   [*] Enable generic HD-audio codec parser
            [*]   Sound Proc FS Support
            [*]     Verbose procfs contents
            [*] USB sound devices  --->
                Must have as some cards are presented as USB devices.
                [*] USB Audio/MIDI driver
General setup --->
    [*] System V IPC
```

8개 이상의 sound output이 있을 경우, (GPU카드가 가지고 있는 hdmi 포트 포함) `Max number of sound cards` 늘려야함

```
Device Drivers --->
    <*> Sound card support
        <*> Advanced Linux Sound Architecture --->
            [*] Dynamic device file minor numbers
            (32) Max number of sound cards
```

이후 kernel build
```bash
### /usr/src/linux dir 에서, installkernel USE Flag 에 grub, dracut 있다고 가정함
# clean
sudo make clean

# make
sudo make

# 모듈 install
sudo make modules_install

# kernel install
sudo make install
```

**alsa 설치**

global use case 추가
``` bash
esue -E alsa
```

`euse` 없는 경우 `sudo emerge -avq app-portage/gentoolkit`으로 설치 가능함

``` bash
### global use case 수동으로 추가

# make.conf 열기
sudo vim /etc/portage/make.conf

# USE flag에 alsa 추가
USE="alsa"
```

use flag 수정했으므로 `@world set` 업데이트

```bash
sudo emerge -avq --changed-use --deep @world
```

`alsa-utils` emerge

```bash
sudo emerge -avq media-sound/alsa-utils
```

### server (`pipewire`) 설치

**`media-sound/pulseaudio-daemon` 설치되어 있는지 확인하고 삭제**

``` bash
# 패키지 존재여부 확인
qlist -IRv | grep -i pulseaudio-daemon

# 있다면 제거
sudo emerge -avqW media-sound/pulseaudio-daemon
sudo emerge -avqc

# -c 옵션(--depclean)으로 삭제되지 않는 경우 강제 삭제 (--unmerge, -C)
sudo emerge -avqC media-sound/pulseaudio-daemon
```

**선행 디펜던시 설치, global use flag 설정**

```bash
# media-libs/libpulse (선행 디펜던시) 존재여부 확인
qlist -IRv | grep -i libpulse

# 없으면 설치
sudo emerge -avq media-libs/libpulse

# global use flag 설정 (pulseaudio 추가, elogind 추가)
sudo euse -E pulseaudio elogind

# pipewire use flag 설정
sudo touch /etc/portage/package.use/pipewire
echo "media-video/pipewire sound-server dbus pipewire-alsa roc" | sudo tee -a /etc/portage/package.use/pipewire > /dev/null

# pulseaudio-daemon 제외하기
sudo touch /etc/portage/package.use/pulseaudio
echo "media-sound/pulseaudio -daemon" | sudo tee -a /etc/portage/package.use/pulseaudio > /dev/null

sudo emerge --deselect media-sound/pulseaudio-daemon

# @world set update
sudo emerge -avq --update --changed-use --deep @world media-video/pipewire

# depclean
sudo emerge -avqc
```

### `pipewire` 설정

`pipewire.conf` 복사
`sudo mkdir /etc/pipewire/; sudo cp /usr/share/pipewire/pipewire.conf /etc/pipewire/pipewire.conf`

`.bashrc` 에 `Hyprland` 실행 alias 작성

```bash
# .bashrc
# ... more configuraitons ...

alias Hyprland="dbus-launch --exit-with-session Hyprland -c MY_CONFIG_PATH"

# ... more configurations ...
```

`hyprland.conf` 에 `exec-once` 추가

```
# hyprland.conf
# ... more configurations ...

exec-once = gentoo-pipewire-launcher

# ... more configurations ...
```

추가한 뒤 `Hyprland` 재실행 해야함
