```bash
docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v $XDG_RUNTIME_DIR/$WAYLAND_DISPLAY:$XDG_RUNTIME_DIR/$WAYLAND_DISPLAY \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
    -v $XDG_RUNTIME_DIR:$XDG_RUNTIME_DIR \
    -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
    -v macos-data:$HOME/docker/docker-macos/osx-data \
    -e GENERATE_UNIQUE=true \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom-sonoma.plist' \
    -e SHORTNAME=sonoma \
    sickcodes/docker-osx:latest
```

**`docker run -it`**
* `run` 새 컨테이너 실행
* `-i` 인터렉티브 모드
* `-t` tty 활성화 (터미널 세션을 유지)

**`--device /dev/kvm`**
* 컨테이너에서 `kvm(kernel virtual machine)` 을 사용하도록 허용

`-p 50922:10022`
* 호스트의 50922 포르틀 컨테이너 내부의 10022 포트로 연결

`-v /tmp/.X11-unix:/tmp/.X11-unix`
* X11 소켓을 공유하여 GUI 실행을 가능하게 함
* macOS 에서 GUI를 실행할 수 있도록 호스트와 컨테이너 간에 X11 디스플레이 연결

**`-v $XDG_RUNTIME_DIR/$WAYLAND_DISPLAY:$XDG_RUNTIME_DIR/$WAYLDNA_DISPLAY`**
* Wayland 디스플레이 공유
* `$WAYLAND_DISPLAY`와 `$XDG_RUNTIME_DIR`경로를 컨테이너로 전달
* 컨테이너에서 macOS GUI를 실행할 때, Wayland를 사용하여 렌더링 가능

**`-v /run/user/1000:/run/user/1000**
* `$XDG_RUNTIME_DIR` 를 컨테이너에 마운트하여 사용자 세션 정보 유지

**`-e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR`**
* `$XDG_RUNTIME_DIR` 환경 변수를 컨테이너에 전달 (`/run/user/1000`)

**`-e GENERATE_UNIQUE=true`**
* 새로운 macOS 인스턴스를 생성할 때, UUID 및 기타 정보 자동 생성
* 동일한 설정으로 여러 번 실행해도, 각 인스턴스가 고유한 값을 가짐

**`-e CPU='Haswell-noTSX'`**
* QEMU 에서 mscOS가 Haswell 세대 CPU를 사용하도록 설정
* `noTSX`는 Transactional Synchronization Extensions(TSX) 비활성화

**`-e CPUID_FLAGS='kvm=on,vender=GenuineIntel, +invtsc, vmware-cpuid-freq=on'`**
* CPUID 조작을 통해 macOS가 VM을 감지하지 못하도록 속임
* `kvm=on`: KVM 지원 활성화
* `vender=GenuineIntel`: CPU제조사를 Intel로 설정
* `+invtsc`: Invariant TSC 활성화 (시간 동기화 안정성 향상)
* `vmware-cpuid-freq=on`: VMware 환경처럼 동작하도록 설정

**`-e MASTER_PLIST_URL='...'`**
* `config-custom-sonoma.plist`를 사용하여 macOS VM의 시리얼 넘버, 모델 정보 등을 자동 생성
* `macOS Sonoma`버전의 정보가 담긴 `plist`파일을 다운로드하여 설정

**`-e SHORTNAME=sonoma`**
* macOS 인스턴스의 이름을 sonoma로 지정

**`sickcodes/docker-osx:latest`**
* 사용할 Docker 이미지의 최신 버전을 가져옴