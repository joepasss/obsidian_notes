*kernel 에서 firmware 모듈 설치했다고 가정함*

``` bash
emerge -avq net-wireless/wpa_supplicant
emerge -avq net-misc/netifrc
```

5G wireless ap 에 접속이 안되는 경우 `tkip` flag 추가

``` bash
touch /etc/portage/package.use/wpa_supplicant
echo "net-wireless/wpa_supplicant" | tee -a /etc/portage/package.use/wpa_supplicant

emerge -avq --changed-use @world
```

**wpa_supplicant configuration 파일 작성**
[wpa_supplicant gentoo wiki](https://wiki.gentoo.org/wiki/Wpa_supplicant)

``` bash
# /etc/wpa_supplicant/wpa_supplicant.conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel

network={
  ssid="SSID"
  psk="PASSWORD"
}
```

**/etc/conf.d/net**
[Netifrc gentoo wiki](https://wiki.gentoo.org/wiki/Netifrc)
[Netifrc documentation](https://gitweb.gentoo.org/proj/netifrc.git/tree/doc/net.example.Linux.in)

```bash
### 인터페이스 이름 확인
ifconfig

....
....

wlp2s0: ...

....
....

### /etc/conf.d/net 파일 작성
modules_wlp2s0="wpa_supplicant"

# DHCP 사용하는 경우
config_wlp2s0="dhcp"

# 고정 ip 사용하는 경우
config_wlp2s0="172.30.1.1/24"            # 사용할 ip 주소
routes_wlp2s0="default via 172.30.1.254" # 라우터 network id (IP), 기본 게이트웨이 주소
dns_servers_wlp2s0="1.1.1.1 8.8.8.8"     # dns servers

preup() {
  # 미리 load 할 firmware 모듈
  modprobe ath11k_pci
  modprobe ath11k

  return 0
}

##### EOF

# /etc/conf.d/net 작성 완료 후, rc-service 생성
sudo ln -s /etc/init.d/net.io /etc/init.d/net.wlp2s0
sudo rc-update add net.wlp2s0 default

# 실행
sudo rc-service net.wlp2s0 start
```

