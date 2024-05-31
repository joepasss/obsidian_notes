### git 설치 & config 파일 설정
`sudo emerge -avq dev-vcs/git`

**git config**
github에 ssh key 추가
`ssh-keygen` 으로 key 생성 후 `cat ~/.ssh/id_ed25519.pub` 으로 public key 복사 한 후 [github SSH and GPG keys](https://github.com/settings/keys) 에서 추가해주면 된다

``` bash
# git user.email, user.name 설정
git config --global user.email "you@example.com"
git config --global user.name "YOUR NAME"
```

### make.conf 작성
```bash
VIDEO_CARDS="intel nvidia"
INPUT_DEVICES="libinput"

USE="X dbus elogind -wayland"
```

작성해 준 다음
```bash
sudo emerge -avq --deep --changed-use --update @world
```

가이드 참고해서 그래픽 카드 드라이버 설치
[intel graphic driver guide(gentoo wiki)](https://wiki.gentoo.org/wiki/Intel#Kernel)
[nvidia graphic driver guide(gentoo wiki)](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers)

### xorg-server

``` bash
# xorg-server use flag
sudo touch /etc/portage/package.use/xorg-server
echo "x11-base/xorg-server elogind suid udev xorg"

# emerge
sudo emerge -avq xorg-server

# emerge 이후 profile update
sudo env-update
source /etc/profile
```

### awesome wm 설치

``` bash
# make.conf 에 dbus, elogind, policykit, udisks 추가
USE="X dbus elogind policykit udisks"

# update use
sudo emerge -avq --update --changed-use --deep @world

# awsome wm emerge
# use flags
# dbus
# doc
# gnome
sudo emerge -avq x11-wm/awesome
```


