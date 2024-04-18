# Network 7

# Chapter 07 IPv6 (차세대 IP)

#### 2024.04.18.(목)

### 7.1 IPv6 주소

- IPv6로의 전환의 주된 이유는 주소공간 부족
- IPv6의 거대한 주소 공간은 주소 고갈의 문제 해결
- IPv6 조소는 128 비트 (16 바이트 (옥텟))
  - IPv4의 4배 길이

### 7.1.1 표현

- 128 비트를 컴퓨터는 쉽게 저장하지만, 인간은 다루기 쉽지 않다
- 바이너리 표기법과 16진 표기법 (Coloned Hexadecimal)

> > FEF6:BA98:7654:3210:ADEF:BBFF:2922:FF00

### 7.1.2 주소공간

- IPv6의 주소 개수는 2^128 개
- 주소 고갈 문제 해결
- IPv4 주소(43억 개)의 2^96배 (10^28배)
- 3.4 x 10^34개

### 22.1.3 주소 공간 할당

- IPv6의 주소공간은 가변길이의 여러 블럭으로 분할하고, 각 공간은 특수 목적으로 사용

- 대부분의 공간은 아직 미할당
  - 미래 사용 위해 남겨둠

![46](/assets/images/2024-04-18/46.png)

![47](/assets/images/2024-04-18/47.png)

- 한 기관이 2000:1456:2474/48을 할당. 이 기관의 첫 번째와 두 번째 서브넷의 블록에 대한 CIDR 표기는?

Solution
상위 64비트 중에 48비트가 프리픽스이므로, 나머지 16비트를 이용하여 서브넷을 할당하므로, 첫번째와 두번째의 서브넷 식별자는 0001(16)과 0002(16).
따라서, 각 서브넷 블록은 2000:1456:2474:0000/64 과 2000:1456:2474:0001/64

- EUI 물리주소가 (F5-A9-23-EF-07-14-7A-D2)16 인 경우에 인터페이스 ID는?

Solution
첫번째 옥텟의 7번째 비트를 1로 변경하고 콜론으로 표기하면 EUI-64 형식의 인터페이스 ID가 된다.
결과는 F7A9:23EF:0714:7AD2

- 이더넷의 물리주소가 (F5-A9-23-14-7A-D2)16에 대한 인터페이스 ID는?

Solution
첫번째 옥텟의 7번째 비트를 1로하고, 중간에 FFFE16 두 옥텟을 추가하고, 콜론으로 표시하면
F7A9:23FF:FE14:7AD2

- 한 기관에 할당된 블록이 2000:1456:2474/48
  3번째 서브넷에 있는 컴퓨터의 IEEE 물리주소가 (F5-A9-23-13-7A-D2)16인 경우의 IPv6 주소는?

Solution
인터페이스의 인터페이스 ID는 F7A9:23FF:FE14:7AD2
글로벌 프리픽스 2000:1456:2474/48
3번째 서브넷 (0003)16
이상을 반영하면

![48](/assets/images/2024-04-18/48.png)

### 7.1.4 자동설정

- IPv6의 흥미로운 특징 중의 하나가 호스트 주소의 자동설정 기능
- IPv4에서는 수동설정, 혹은 DHCP에 의한 자동설정
- DHCP는 IPv6에서도 주소할당에 사용
- IPv6에서는 호스트가 스스로 주소설정 가능

![49](/assets/images/2024-04-18/49.png)

### 7.1.5 Renumbering

- 서비스 제공자를 변경하는 경우에, 인터페이스 ID는 동일하고, 네트워크 프리픽스와 서브넷 ID만 변경하면 됨
- 네트워크 프리픅스와 서브넷 ID는 라우터가 주기적으로 메세지를 통하여 알리고, 각 호스트는 자신의 주소를 이 값으로 변경한다

### 7.2 IPv6 프로토콜

- IPv6 패킷 포맷
  - 고정헤더
  - 확장헤더
- 메세지 유형

### 7.1.1 패킷 포맷

- 40 바이트 기본 헤더
- 유료부하 (payload)

* 확장 헤더 (Optional) + 데이터 (상위계층)
* 최대 65535 바이트

![50](/assets/images/2024-04-18/50.png)

### 7.2.2 확장헤더

- 6가지 확장헤더
  - IPv4에서는 옵션
- hop-by-hop option
- source routing
- Fragmentation
- Authentication
- encrypted security payload
- destination option

![51](/assets/images/2024-04-18/51.png)

### 7.3 ICMPv6 프로토콜

- ICMPv6는 ICMPv4 보다 복잡
  - ARP, IGMP 기능 흡수
- 새로운 메세지 추가됨

![52](/assets/images/2024-04-18/52.png)

### 7.3.1 Error-Reporting Messages

- 보고되는 4가지 에러 종류

  - destination unreachable
  - packet too big
  - time exceeded
  - parameter problems

- Source-quenched 메세지는 제외됨
  - IPv4에서 혼잡제어 용도
  - IPv6에서는 헤더의 priority, flow label 필드를 활용하여 혼잡제어

### 7.3.2 Informational Messages

- 2 종류
  - echo request 메세지
  - Echo reply 메세지
- 인터넷에서 통신 가능 상태 체크

### 7.3.3 Neighbor-Discovery 메세지

- 2개의 새로운 프로토콜 지원
  - Neighbor-Discovery(ND) protocol
  - Inverse-Neighbor-Discovery(IND) protocol

### 7.3.4 Group Membership Message

- IPv에서 그룹 통신 관리를 다루는 프로토콜은 IGMPv3 프로토콜
- IPv6에서는 Multicast Listener Delivery 프로토콜이 담당
  - MLDv1이 IGMPv2의 IPv6 버전
  - MLDv2가 IGMPv3의 IPv6 버전
- MLDv2의 2 메세지 유형
  - membership-query 메세지
  - membership-report 메세지

### 7.4 IPv4에서 IPv6로의 전환

- 전환은 IPv4와 IPv6 시스템 사이에 어떠한 문제도 발생하지 않도록 매끄럽게 (smooth)

### 7.4.1 3가지 전환 전략

- 듀얼 스택 (Dual stack)
- 터널링 (Tunneling)
- 헤더 변환 (Header translation)

![53](/assets/images/2024-04-18/53.png)

### 7.4.2 IP 주소의 사용

- 전환기간 동안에는 IPv4와 IPv6 두 방식의 주소 모두 사용
- 전환 완료시 IPv4 주소는 없어질 것
- DNS 서버가 호스트 이름으로부터 두 방식의 주소 모두를 매핑해줄 것이고, 전세계의 모든 호스트가 IPV6로 전환되면 IPv4 디렉토리는 없어질 것임
