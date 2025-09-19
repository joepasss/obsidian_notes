**laptop**
[power management guide gentoo wiki](https://wiki.gentoo.org/wiki/Power_management/Guide)

```
emerge -avq sys-power/tlp
rc-update add tlp default
rc-service tlp start
```

**git**

```
emerge -avq dev-vcs/git

git config --global user.email "<joe@some.org>"
git config --global user.name "<joe>"
```

**xorg**
[xorg gentoo wiki](https://wiki.gentoo.org/wiki/Xorg)
[non-root xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg)
[xorg guide](https://wiki.gentoo.org/wiki/Xorg/Guide)
[xorg hardware acceleartion](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide)

``` bash
### make.conf 수정
USE="${USE} X elogind"

emerge -avq --changed-use --update --deep @world

### elogind
rc-update add elogind boot
/etc/init.d/elogind start

### video group 에 추가, 로그아웃 필요함
gpasswd -a $USER video

### group 확인
groups

### xorg-server emerge
emerge -avq x11-base/xorg-server
```

**i3, kitty**

``` bash
emerge -avq x11-wm/i3 x11-terms/kitty

# $HOME/.xinitrc
#!/bin/bash

exec dbus-launch --exit-with session i3
```

### dpi 설정 (xdpi)

```bash
### ~/.Xresources
Xft.dpi: 192 # 96의 배수

```

`.Xresources` 작성 이후, `xrdb -merge ~/.Xresources`