### IP addr

Internet protocol 의 약자

| IPv4(Internet Protocol version 4) | 4바이트 주소 체계  |
| --------------------------------- | ----------- |
| IPv6(Internet Protocol version 6) | 16바이트 주소 체계 |

**네트워크 (IPv4) 클래스**
![[Pasted image 20240619182208.png]]

### 소켓의 구분에 활용되는 PORT 번호

IP는 컴퓨터를 구분하기 위한 용도로 사용되고, PORT는 호스트(컴퓨터)내부의 프로세스들을 구분하기 위한 용도로 사용됨
OS 단에서 PORT에 맞는 소켓에 데이터를 전달해 줌

PORT의 번호는 16비트로 표현됨 (0이상 65535이하)

**well-known port (0부터 1024까지)**
![[Pasted image 20240619182546.png]]

TCP와 UDP는 포트 번호를 공유하지 않기 때문에 중복되어도 상관 없음

### 정리
IP => 네트워크 상에서 호스트(컴퓨터)를 식별
PORT => 운영체제 내부에서 프로세스를 식별

