### `rust & rustup emerge`

``` bash
# rust 설치
sudo emerge -avq dev-lang/rust # 오래걸림

eselect rust list
sudo eselect rust set NUMBER

# rustup unmask
sudo touch /etc/portage/package.accept_keywords/rustup
echo "dev-util/rustup ~amd64" | sudo tee -a /etc/portage/package.accept_keywords/rustup > /dev/null

# rustup 설치
sudo emerge -avq dev-util/rustup

# rust symlink 생성
rustup-init-gentoo --symlink

# rust 최신 버전으로 update
rustup update stable
```

### `neovim` 설정



