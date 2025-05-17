### TFTP

``` bash
mkdir /tftproot

# whoami 로 유저, groups로 group확인 가능함
# default는 nobody
chown user:usergroup /tftproot
```

**TFTP 서버 설치**
``` bash
sudo emerge -avq net-ftp/tftp-hpa
```

**`/etc/conf.d/in.tftpd` 설정**
```
# /etc/init.d/in.tftpd

# Path to server files from
# Depending on your application you may have to change this.
# This is commented out to force you to look at the file!
#INTFTPD_PATH="/var/tftp/"
#INTFTPD_PATH="/tftpboot/"
INTFTPD_PATH="/tftproot/"

# For more options, see in.tftpd(8)
# -R 4096:32767 solves problems with ARC firmware, and obsoletes
# the /proc/sys/net/ipv4/ip_local_port_range hack.
# -s causes $INTFTPD_PATH to be the root of the TFTP tree.
# -l is passed by the init script in addition to these options.
INTFTPD_OPTS="-R 4096:32767 -s ${INTFTPD_PATH}"
```

**SERVER TEST**

``` bash
# 파일 생성
touch /tftproot/testfile.txt
echo "HELLO" | tee -a /tftproot/testfile.txt

# 외부 클라이언트에서
tftp MY_SERVER_IP
tftp> get testfile.txt
tftp> quit

# 정상적으로 로드되었는지 확인
ls | grep testfile.txt
cat testfile.txt

# 또는 서버에서 netstat으로 udp 69 포트 열려있는지 확인
# -u : udp 소켓만 출력
# -l : 현재 listening 중인 소켓만 출력
# -n : 포트를 숫자로 표시
# -p : 해당 포트를 사용하는 프로세스 보여줌
sudo netstat -ulnp | grep tftp
```
### SFTP
[gentoo wiki sftp](https://wiki.gentoo.org/wiki/SFTP)

sftp 는 `openssh` 패키지에 포함되어있음, 없다면 `emerge -ave net-misc/openssh` 패키지 설치

``` bash
# 필요한 경우, user 추가
sudo useradd -m -s /bin/bash myuser
sudo passwd myuser

# 이후 각 클라이언트에서 서버 IP로 접근 가능함
```

**Chroot jail + 특정 디렉터리 제한**
