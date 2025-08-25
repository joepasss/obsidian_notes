### Kernel configuration

[gentoo wiki, docker](https://wiki.gentoo.org/wiki/Docker)
[gentoo wiki, Home router (NAT configuration)](https://wiki.gentoo.org/wiki/Home_router)

**추가 Kernel 설정**
```
CONFIG_BRIDGE_NETFILTER
CONFIG_IP_NF_RAW
CONFIG_INET_ESP
CONFIG_NF_NAT_TFTP
CONFIG_NF_CONNTRACK_TFTP
CONFIG_SECURITY_APPARMOR

# network
CONFIG_NETFILTER_XT_MATCH_BPF
```

**kernel config 확인**
```bash
# 압축된 커널 설정에서 확인
zgrep <CONFIG_> /proc/config.gz

# 직접 .config 확인
grep <CONFIG_> /usr/src/linux/.config

# module로 커널에 포함된 경우
lsmod | grep <my_module>
```

### Emerge

``` shell
emerge -avq app-containers/docker app-containers/docker-cli
```


**Compatibility Check**
``` shell
# Kernel 설정 확인해주는 스크립트
/usr/share/docker/contrib/check-config.sh
```

### OpenRC 서비스 등록

``` shell
rc-update add docker default
rc-service docker start
```

### user group 추가

``` shell
usermod -aG docker <username>
```

### service 상태 확인

``` bash
docker info
```

``` bash
# 출력 예시
$ docker info
Client:
 Version:    28.0.1
 Context:    default
 Debug Mode: false

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.0.1
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: c507a0257ea6462fbd6f5ba4f5c74facb04021f4
 runc version: 59923ef18c98053ddb1acf23ecba10344056c28e
 init version: de40ad007797e0dcd8b7126f27bb87401d224240
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.12.21-gentoo
 Operating System: Gentoo Linux
 OSType: linux
 Architecture: x86_64
 CPUs: 16
 Total Memory: 30.02GiB
 Name: gentoo
 ID: e8b33b97-6504-42b0-9997-8b69c517afd3
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
```

### NAT 설정 (`nftables`)

[gentoo wiki, nftables](https://wiki.gentoo.org/wiki/Nftables)

**패키지 설치**
```bash
emerge -avq net-firewall/nftables
```

**rc service 등록**
```bash
rc-update add nftables default
```

### DHCP 설정

``` bash
## /etc/dhcpcd.conf
## 고정 ip사용하는 경우에 수정해야함 (docker container 내부에서 외부로 연결 불가능)

### wlp2s0 에서만 고정 ip 사용
interface wlp2s0
	static ip_address=172.30.1.2/24
	static routers=172.30.1.254
	static domain_name_servers=168.126.63.1 168.126.63.2 1.1.1.1 8.8.8.8

denyinterfaces veth*
```

이후 `rc-service` 재시작
```bash
sudo rc-service dhcpcd restart
```