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

**dotfile clone**
미리 작성해놓은 dotfiles 가 있다면 본인의 레포지토리 복사
`git clone $MY_REPO_URL`
`git clone git@github.com:joepasss/dotfiles.git`

**symlink 생성**
보통의 config 파일은 `~/.config` 폴더 또는 홈 디렉터리에 저장되므로, `~/.config` 폴더 내부에 dotfile의 symlink를 생성해 주면 된다

```bash
##### home dir 에 있다고 가정
### tmux
ln -s ~/dotfiles/tmux/.tmux.conf ~/

# tpm 설치
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

# tmux 를 연 후 prefix키 (Ctrl + s) + I 로 설치 진행 가능

# gitmux 설치
sudo emerge -avq dev-lang/go
go install github.com/arl/gitmux@latest
sudo ln -s ~/go/bin/gitmux /usr/bin

# gitmux config 파일 심링크 생성
ln -s ~/dotfiles/gitmux/.gitmux.conf ~/

### .bashrc
# 기존 bashrc 삭제
rm -rf ~/.bashrc
ln -s ~/dotfiles/bash/.bashrc ~/

# bash gitstatus 설치
git clone --depth=1 https://github.com/romkatv/gitstatus.git ~/gitstatus

# .bashrc 확인 후 source ~/gitstatus/gitstatus.prompt.sh 가 없다면 추가해줌
echo 'source ~/gitstatus/gitstatus.prompt.sh' >> ~/.bashrc

```
### hyprland & kitty
**use flag에 wayland 추가**
`sudo vim /etc/portage/make.conf`
```
# /etc/portage/make.conf

# .... some configs ...

USE="wayland gles2"

# .... more configs ...
```


**hyprland & kitty package emerge**
`sudo emerge -avq hyprland kitty`

이후 `Hyprland -c MY_CONFIG_PATH` 또는 `Hyprland` (디폴트 config 사용)으로 실행 가능

**kitty config 심링크 생성**
```bash
mkdir ~/.config
ln -s ~/dotfiles/kitty/ ~/.config
```

### firefox & obsidan
**gentoolkit 설치**
`sudo emerge -avq gentoolkit`

**firefox use flag 설정**
`equery uses firefox` 명령으로 사용가능한 use 플래그 목록 출력 가능함
`sudo vim /etc/portage/package.use/firefox` 에 추가한 뒤

`sudo emerge -avq firefox`(오래걸림)
이후 config 변경해야한다 하면 `etc-update` 명령어로 업데이트 가능함

**r7l overlay 추가**
`sudo emerge -avq app-eselect/eselect-repository`
`mkdir -p /etc/portage/repos.conf`

`sudo eselect repository list | grep -i r7l` 후 존재하면
`sudo eselect repository enable r7l` 명령으로 enable 해 준뒤,
`sudo emaint sync --repo r7l` 명령어로 emerge 에 sync 해 줌

**obsidan unmask (수동)**
`sudo vim /etc/portage/pacakge.accept_keywords/obsidian` 로 파일 생성 & 열기
버전 확인 후
```
# /etc/portage/package.accept_keywords/obsidian
=app-text/obsidian-1.5.12 ~amd64
```

추가해 준 다음 `sudo emerge -avq app-text/obsidan`

**자동 unmask**
`sudo emerge -avq --autounmask-write --autounmask app-text/obsidian` 이후 `sudo etc-update` 실행해주면 자동으로 `accept_keyworld/zz_autounmask` 파일에 unmask 작성 해 준다

### 한글입력 & 폰트 설치
**한글 폰트 & 이모지 폰트**
 `sudo emerge -avq noto-emoji alee-fonts noto-cjk fontawesome`

**nerd font**
`xarblu-overlay` 추가
```bash
# xarblu-overlay 있는지 확인
sudo eselect repository list | grep -i xarblu-overlay

# 없으면
sudo eselect respository add xarblu-overlay

# 있으면
sudo eselect respository enable xarblu-overlay

# overlay 추가 후 sync
sudo emaint sync -r xarblu-overlay
```

`media-fonts/nerd-fonts` 설치
```bash
# ~amd64 ~x86 mask 되어 있으므로, /etc/portage/package.accept_keywords/nerd-fonts 생성 후, unmask
sudo touch /etc/portage/package.accept_keywords/nerd-fonts

echo "=media-fonts/nerd-fonts-3.2.1::xarblu-overlay ~amd64 ~x86" | sudo tee -a /etc/portage/package.accept_keywords/nerd-fonts > /dev/null

# 확인
cat /etc/portage/package.accept_keywords/nerd-fonts

# USE flag 확인
equery uses nerd-fonts

# 사용할 폰트를 USE flag에 추가
sudo touch /etc/portage/package.use/nerd-fonts

echo "media-fonts/nerd-fonts d2coding firacode firamono hack jetbrainmono X" | sudo tee -a /etc/portage/package.use/nerd-fonts > /dev/null

# nerd-font 설치
sudo emerge -avq nerd-fonts
```

**한글 입력기 설치 & 설정 (kime)**
``` bash
# repository add
sudo eselect repository add riey git https://github.com/Riey/overlay
sudo eselect repository enable riey

# repository sync
sudo emaint sync -r riey

# kime emerge
sudo emerge -avq kime

# default config 파일 복사
mkdir ~/.config/kime/
cp -i /usr/share/doc/kime/default_config.yaml ~/.config/kime/config.yaml

# 작성해놓은 config 파일이 있다면
ln -s ~/dotfiles/kime/ ~/.config/

# hyprland env 작성
# hyprland.conf 파일에서
env = XDG_CURRENT_DESKTOP,Hyprland
env = INPUT_METHOD,kime
env = XMODIFIERS,@im=kime
env = GTK_IM_MODULE,kime
env = QT_IM_MODULE,kime

exec-once = kime
```

### notification daemon(dunst) & status bar (waybar) & app launcher (tofi) 설치

``` bash
# waybar use flag 설정
sudo touch /etc/portage/package.use/waybar
echo "gui-apps/waybar logind network pulseaudio tray udev upower wifi sndio experimental libinput popups" | sudo tee -a /etc/portage/package.use/waybar > /dev/null

# waybar config 추가
ln -s ~/dotfiles/waybar/ ~/.config/

# tofi unmask
sudo touch /etc/portage/package.accept_keywords/tofi
echo "=gui-apps/tofi-0.9.1::guru ~amd64" | sudo tee -a /etc/portage/package.accept_keywords/tofi > /dev/null

# tofi config 추가
ln -s ~/dotfiles/tofi/ ~/.config/

# emerge
sudo emerge -avq dunst waybar tofi

# (wpa supplicnat 디펜던시 update 필요한 경우)
sudo etc-update
sudo emerge -avq dunst waybar tofi
```

###  배경화면 변경 (swww)

``` bash
# lz4 emerge
sudo emerge -avq app-arch/lz4

# 아무 dir 에서
git clone git@github.com:LGFae/swww.git

# ssh 설정 안한 경우
git clone https://github.com/LGFae/swww.git

cd swww
cargo build --release

# build 된 바이너리 파일 /usr/bin/ 디렉터리로 이동
sudo mv ./target/release/swww /usr/bin
sudo mv ./target/release/swww-daemon /usr/bin

# hyprland config에 swww 추가
.... configs ....

exec-once = swww-daemon

.... more configs ....

# 배경화면 설정 (hyprland 연 상태에서 가능함)
swww img <path/to/img>
```