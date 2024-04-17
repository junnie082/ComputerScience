# Network 2

# Chapter 03 네트워크 계층

#### 2024.04.17.(수)

### 2.4 IPv4 주소

- 주소
  - 연결의 양 끝 장치 식별(identify)
- IPv4 주소
  - 32 비트
  - 인터넷에 연결된 통신장치(호스트, 라우터)를 전세계적으로 유일하게 정의하는 32비트의 주소
- IP 주소는 호스트나 라우터의 주소가 아닌, 연결의 끝단 주소
  - 연결이 여러 개인 경우 연결 각각마다 주소가 있음

### 2.4.1 주소 공간

- 프로토콜이 사용하는 전체 주소의 수

- IPv4: 32 bit
  - 2^32 = 4,294,967,296 (43억개)
- IPv6: 128 bit
  - 2^128

### IPv4 주소 표기

- Dotted decimal

> > Binary 10000000 00001011 00000011 00011111
> > Dotted decimal 128.11.3.31

### 주소 계층(Hierarchy)

| 32 bits |
| n bits | (32-n) bits |
| Prefix | Suffix |
| Defines network | Defines connection to the node |

### 2.4.2 Classful Addressing

- Fixed-length prefix
  - 네트워크 부분
  - 호스트 부분
- 다양한 크기의 네트워크를 수용하기 위해, 3개의 고정길이 프리픽스(prefix) 고안
  - n=8, n=16, n=24
- 전체 주소 공간은 5개의 클래스로 분할
  - Class A, B, C, D and E

![21](/assets/images/2024-04-17/21.png)

### 18.4.3 Classless Addressing

- 주소 부족 -> 고갈
- Long-term solution is IPv6
- Short-term solution is IPv4 classless addressing
- A/B/C 클래스 무시
  - 클래스에 따라 네트워크 부분 고정 -> 무시
  - Network Prefix 크기에 따라 네트워크 부분이 달라짐
- CIDR (Classless Inter-domain Routing) 표기

![22](/assets/images/2024-04-17/22.png)

![23](/assets/images/2024-04-17/23.png)

classless 주소에서, 하나의 주소를 가지고, 그 주소가 속한 블록(네트워크)을 알 수가 없다.
예를 들어, 230.8.24.56 주소는 여러 블록에 속할 수 있다.

Prefix length: 16 -> Block: 230.8.0.0 to 230.8.255.255  
Prefix length: 20 -> Block: 230.8.16.0 to 230.8.31.255  
Prefix length: 26 -> Block: 230.8.24.0 to 230.8.24.63  
Prefix length: 27 -> Block: 230.8.24.32 to 230.8.24.63  
Prefix length: 29 -> Block: 230.8.24.56 to 230.8.24.63  
Prefix length: 31 -> Block: 230.8.24.56 to 230.8.24.57

![24](/assets/images/2024-04-17/24.png)

- 한 ISP가 1000개의 주소 블록을 요청했다고 하자.
  1000은 2의 거듭제곱의 수가 아니므로 1024개의 주소를 할당해야 한다.
  prefix 길이는 n=32-log(2)1024 = 22가 된다.
  ISP에 할당하는 하나의 가용 블록의 예는 18.14.12.0/22

- 한 기관이 14.24.74.0/24 블록을 할당 받았다.
  이 기관은 3개의 서브 블록으로 나누어 운영하려 한다. 한 블록은 10개의 주소, 다른 블록은 60개의 주소, 마지막 블록은 120개의 주소가 필요하다. 서브 블록을 설계하라. (VLSM)

Solution: 할당 받은 블록은 2^(32-24)=256개의 주소가 가능하다. 첫 주소는 14.24.74.0/24, 마지막 주소는 14.24.74.255/24.
서브 네트워크는 큰 블록부터 시작해서, 작은 블록을 할당하는 순서로 진행된다.

a. 가장 블록의 주소 개수는 120인데, 이는 2의 제곱수가 아니므로 2의 제곱수인 128 할당.
이 블록의 서브넷마스크는 n1 = 32 - log(2)128 = 25 비트.
이 블록의 첫 주소는 14.24.74.0/25  
마지막 주소는 14.24.74.127/25

b. 다음으로 큰 블록의 주소 개수는 60인데, 2의 제곱수인 64 선택.
이 블록의 서브넷마스크는 n2 = 32 - log(2)64 = 26 비트
이 블록의 첫 주소는 14.24.74.128/26  
이 블록의 마지막 주소는 14.24.74.191/26

c. 마지막 남은 블록의 주소 개수는 10이지만 2의 제곱수인 16을 선택.
이 블록의 서브넷마스크는 n2 = 32 - log(2)16 = 28 비트.
이 블록의 첫 주소는 14.24.74.192/28  
이 블록의 마지막 주소는 14.24.74.208/28

이상의 과정에서 208개의 주소를 사용
48개의 주소가 남았음 -> 나중에 사용을 위해 남겨둠
이 블록의 첫 주소는 14.24.74.208  
마지막 주소는 14.24.74.255  
이 블록의 Prefix는 결정 불가 -> 2~3개의 블록으로 사용 가능

![25](/assets/images/2024-04-17/25.png)

### Address Aggregation

- 작은 주소를 묶어서 하나의 더 큰 주소를 만드는 것
- Address Summarization, Route Summarization, Supernet
- CIDR 전략의 또 다른 큰 잇점

  - 라우팅 테이블을 간소화 -> 빠른 처리, 관리 용이
  - 라우팅 정보 교환 감소 -> 트래픽 절약

- 아래 그림은 ISP에 할당된 하나의 주소 블록을 더 작은 4개의 주소 블록으로 분할해서 사용하는 것을 보였다.
  ISP는 4개의 주소를 모은 하나의 단일 주소를 다른 네트워크에 알린다. 일단 패킷이 이 ISP에 도착하면 ISP는 목적지 주소에 따라 해당하는 작은 블록으로 보내줄 책임이 있다.
  이러한 방식은 마치 우체국에서의 편지/소포 전달과 같은 시스템이다. 특정 국가/지역으로 먼저 보내지고, 그 다음 다시 목적지로 보내진다.

![26](/assets/images/2024-04-17/26.png)

### 2.4.4 DHCP

- 기관에 블록 주소가 할당되면, 관리자는 각각의 호스트나 라우터에 수동으로 주소를 할당할 수 있다.
- 하지만, Dynamic Host Configuration Protocol(DHCP)를 통해 주소할당을 자동으로 할 수 있다.
- DHCP는 클라이언트-서버 패러다임을 이용하는 응용 계층 프로그램으로서, TCP/IP의 네트워크 계층을 도와준다.

![27](/assets/images/2024-04-17/27.png)

![28](/assets/images/2024-04-17/28.png)

### 2.4.5 NAT (Network Address Translation)

- 대부분의 경우, 작은 네트워크에서의 인터넷의 동시 접속은 많지 않음
- 사설(private) 주소와 공인(public) 주소의 매핑을 제공하고, 가상의 사설 주소를 지원하는 시스템이 NAT
- 이 기술은 사이트 내부에서 사설주소를 이용해 내부 통신을 하고, 인터넷의 컴퓨터와 통신을 할 때, 공인주소를 통해 통신을 가능하게 한다.

![29](/assets/images/2024-04-17/29.png)

![30](/assets/images/2024-04-17/30.png)

![31](/assets/images/2024-04-17/31.png)

![32](/assets/images/2024-04-17/32.png)

### 2.5 IP 패킷의 포워딩

- 포워딩에서의 IP 주소의 역할
- 포워딩은 패킷을 목적지로 가는 경로로 위치시키는 것
- 2가지 방안
  - Forwarding based on Destination IP Address
  - Forwarding based on Destination Label

### 2.5.1 Destination Address Forwarding

- Forwarding based on the destination address
  - 전통적 방식(Traditional approach)
  - 현재 대부분의 방식(prevalent today)
- 포워딩 테이블이 필요함
- 호스트가 패킷을 보낼 때나, 라우터가 패킷을 수신하고 포워딩 할 때, 라우팅 테이블을 보고 목적지로 가는 다음 홉을 찾음

![33](/assets/images/2024-04-17/33.png)

![34](/assets/images/2024-04-17/34.png)

![35](/assets/images/2024-04-17/35.png)

### Address Aggregation

- 작은 네트워크 여러 개를 묶어 하나의 큰 네트워크로 표시
- 라우팅 테이블의 수가 적어져서 빠른 탐색 가능

![36](/assets/images/2024-04-17/36.png)

### Longest Mask Matching

- 라우팅 테이블에서 마스크 비트수가 제일 큰 항이 제일 위에 와서 먼저 검사됨

![37](/assets/images/2024-04-17/37.png)

### Hierarchical Routing

- 계층적 라우팅
  - Internet: backbone > 국가 ISP > 지역 ISP > 단말 ISP
  - 전화망: 국가코드, 지역번호
- 네트워크를 계층적으로 배치하면, 포워딩 테이블의 항을 줄일 수 있음

![38](/assets/images/2024-04-17/38.png)

### 2.5.3 Forwarding based on Label

- 1980년대에, IP 라우팅을 스위칭으로 변경하여, 연결지향형의 프로토콜처럼 동작하는 노력 시도

- 연결지향형(connection-oriented)의 네트워크에서(virtual-circui approach), 스위치는 패킷의 레이블(label)을 보고 포워딩

- 라우팅은 라우팅 테이블을 검색

- 스위칭은 테이블을 인덱스처럼 엑세스

  - 탐색없이 바로 엑세스

- 다음 그림은 longest mask algorithm을 이용한 포워딩 테이블 참고
  패킷수신 -> 마스킹 -> 테이블 탐색 -> 다음 홉 정보 획득

![1](/assets/images/2024-04-18/1.png)

- 다음 그림은 레이블을 이용한 스위칭 테이블 참조.
  레이블을 인덱스로 활용하여, 테이블에서 바로 다음 홉 정보 획득.

![2](/assets/images/2024-04-18/2.png)

### MPLS

- Multi-Protocol Label Switching
- MPLS 라우터는 라우터 및 스위치로 동작
  - 라우터로 동작 시, 목적지 주소 기반으로 포워딩
  - 스위치로 동작 시, 레이블에 근거하여 포워딩
- 레이블 스택
  - 계층형 네트워크 지원
  - 스택과 같이 추가 가능

![3](/assets/images/2024-04-18/3.png)

### 2.5.3 라우터

- 라우터: 네트워크 계층에서 사용되는 패킷 스위치
- 라우터는 2가지로 설정 가능
  - 데이터그램 스위치
  - 가상회선 스위치
