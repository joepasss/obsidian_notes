### samba 설치
``` bash
sudo emerge -avq samba
```

### config 파일 수정

``` bash
# smb.conf.default 파일 복사해서 smb.conf.default 파일 생성
sudo cp /etc/samba/smb.conf.default /etc/samba/smb.conf

# 공유할 폴더 생성
mkdir -p /home/SOME_FOLDER_NAME

# 폴더 소유자, 그룹 지정
sudo chown USER_NAME:GROUP /home/SOME_FOLDER_NAME
```

``` conf
# samba config 수정
[global]
	workgroup = WORKGROUP
	server string = samba server
	netbios name = gentoo-server
	security = user
	map to guest = Bad user
	dns proxy = no

[shared]
	path = MY_SHARED_PATH
	browsable = yes
	writable = yes
	guest ok = no
	read only = no
	force user = MY_USER

[private]
	path = MY_PRIVATE_PATH
	browsable = no
	writable = yes
	valid users = MY_USER
```

### samba 서비스 시작 & user 추가하기

``` bash
# samba service 자동 시작 & service 시작
sudo rc-update add samba default
sudo rc-service samba start

# samba user 추가
sudo useradd -m smbuser
sudo smbpasswd -a MY_USER_NAME
sudo smbpasswd -e smbuser
```

### samba 접속하기

``` bash
# ip addr 명령어로 ip 주소 가져오기
ip addr | grep MY_NETWORK

# 이후 finder -> 이동 -> 서버에 연결 탭에서
smb://MY_IP_ADDR
```