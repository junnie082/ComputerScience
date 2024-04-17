# Network 6

# Chapter 06 멀티캐스트 라우팅

#### 2024.04.18.(목)

### 6.1 소개

- 현재의 인터넷은 유니캐스트만 있는 것이 아님
- 멀티캐스트 통신이 급증하는 추세임
- Unicasting, Multicasting, and Broadcasting 기본 개념
- 멀티캐스트 라우팅 프로토콜 소개

### 6.1.1 유니캐스팅(Unicasting)

- 하나의 소스, 하나의 목적지
- One-to-One
- 경로상의 라우터는 한 출구 인터페이스로 하나의 패킷만 포워딩

![34](/assets/images/2024-04-18/34.png)

### 6.1.3. 브로드캐스팅(Broadcasting)

- One-to-all 통신
- 하나의 호스트가 네트워크의 모든 호스트에게 전달
- 인터넷 수준 전체에서는 제공하기 어려움
- 부분적 브로드캐스팅은 가능
- 일부 피어-투-피어(peer-to-peer) 응용에서는 모든 피어에게 엑세스 하기 위해 사용하는 경우가 있음
- 일부 도메인 (지역 or AS)에서 멀티캐스팅을 브로드캐스팅으로 해결하는 경우도 있음

### 6.2 멀티캐스트 기초

- 멀티캐스트 주소
- 멀티캐스트 그룹 정보 수집
- 멀티캐스트 최적 트리

### 6.2.1 멀티캐스트 주소

- 1 sender, many receiver
  - 가끔 수천에서 수백만(전세계)
- 모든 수신자를 패킷에 넣을 수 없음
  - IP 패킷의 수신주소는 오직 1
- 멀티캐스트 주소가 필요
  - 멀티캐스트 주소는 하나가 아닌 수신자 그룹의 주소
- 멀티캐스트 주소는 그룹의 식별자

![35](/assets/images/2024-04-18/35.png)

### 6.2.2 데이터링크계층에서의 전달

- 멀티캐스트에서, 인터넷 수준에서의 전달은 네트워크 계층의 그룹 주소를 이용하여 전달
- 멀티캐스트 패킷을 프레임에 넣어서 전달하기 위해 데이터링크 계층에서도 멀티캐스트 주소가 필요
- 유니캐스트의 경우에는 ARP를 이용하여 MAC 주소를 얻지만, 멀티캐스트 주소의 경우에는, 하부 데이터링크 계층의 지원여부에 따라 ARP를 통해 멀티캐스트를 위한 MAC(physical, 물리) 주소 획득이 안될 수 있음. (IP주소는 논리(logical, 주소))

![36](/assets/images/2024-04-18/36.png)

- 멀티캐스트 IP 주소 232.43.14.7를 위한 이더넷의 멀티캐스트 주소는?

Solution
2 단계로 해결:
a. IP 주소의 왼쪽 23비트 추출: 2B:0E:07
b. 앞쪽의 이더넷의 멀티캐스트 주소 붙임: 01:00:5E:0

> > 01:00:5E:2B:0E:07

- 멀티캐스트 IP 주소 228.43.14.7를 위한 이더넷의 멀티캐스트 주소는?
  Solution
  2 단계로 해결:
  a. IP 주소의 왼쪽 23비트 추출: 2B:0E:07
  b. 앞쪽에 이더넷의 멀티캐스트 주소 붙임: 01:00:5E:0
  => 결과는 01:00:5E:2B:0E:07
  => 앞의 예제 결과와 같다.
  => IP가 달라도 MAC 주소는 같다 (5 bit 무시 때문)
  => 다른 그룹으로 전달 가능. 상위계층에서 해결 (무시)

### 터널링 (Turnneling)

- 대부분의 WAN은 멀티캐스트를 지원하지 않음
  - 터널링(Tunneling)으로 해결
- 터널링은 멀티캐트 피킷을 유니캐스팅 패킷으로 캡슐화하여 전송

![37](/assets/images/2024-04-18/37.png)

### 6.2.3 그룹 정보 모으기 (IGMP)

유니캐스트와 멀티캐스트에서 포워딩 테이블 생성 2단계:

1. 라우터는 자신에게 연결된 목적지를 인식

- 유니캐스트의 경우, 자동으로 알게 됨
- 멀티캐스트는 멤버들을 알게 하는 방안이 필요 => IGMP

2. 각 라우터는 위 단계에서 얻은 정보를 다른 모든 라우터에게 전달하여, 어떤 목적지가 어느 라우터에 연결되었는지를 알려줌

- 유니캐스트의 경우 네트워크 프리픽스를 다른 라우터에게 전달
- 멀티캐스트의 경우 존재하는 그룹의 정보를 다른 라우터에게 전달

![38](/assets/images/2024-04-18/38.png)

### 6.2.4 멀티캐스트 포워딩

유니캐스트와 다른 점

1. 전달 경로 수

- 유니캐스트의 경우 하나의 목적지 (한방향으로만 전달)
- 멀티캐스트의 경우 하나 이상의 목적지 (여러 방향으로 전달)

2. 포워딩 결정 시 참조 정보

- 유니캐스트의 경우 목적지 주소만 보고 결정
- 멀티캐스트의 경우 소스, 목적지 주소 둘 다 보고 결정

![39](/assets/images/2024-04-18/39.png)

### 6.2.5 멀티캐스트의 2가지 접근방법

- 멀티캐스트 라우팅에서도, 유니캐스트 라우팅과 같이, 소스에서 목적지까지 최적 경로 트리 필요
- 멀티캐스트 라우팅에서는 목적지 주소뿐만 아니라, 소스의 주소도 고려
- 2가지 접근법
  - 소스 기반 트리(source-based tree)라우팅
  - 그룹 공유 트리(group-based tree)라우팅

### 멀티캐스트 트리 종류

- 소스 기반 트리(source-based tree)
  - 그룹(m)당 소스 개수(n) 만큼의 트리: m\*n개 트리
  - 각 소스를 루트로 하는 트리
- 그룹 공유 트리(group-shared tree)
  - 그룹(m)당 1개의 트리: m개
  - Designated(Core, Rendezvous-point) 라우터를 루트로 하는 트리
  - 멀티캐스트를 보내려는 소스가, 유니캐스트로 랑데뷰포인트로 전달(터널링) 후, 멀티캐스팅
  - 코어 라우터 선정 알고리즘 필요

### 6.3 Intra-domain Protocols

- Source-based tree approach
  - RIP 확장: DVMRP
  - OSPF 확장: MOSPF
- Independent protocol
  - 소스기반이나 그룹공유기반 방식 네트워크 모두에 사용 가능
  - 점점 많이 사용되고 있음
  - PIM-SM, PIM-SM

### 6.3.1 DVMRP

- Distance Vector Multicast Routing Protocol (DVMRP)는 유니캐스트 라우팅 프로토콜인 Routing Information Protocol (RIP)의 확장

- Source-based tree approach

- 멀티캐스트 트리 생성 3단계 알고리즘
  - RPF(Reverse Path Forwarding)
  - RPB(Reverse Path Broadcasting)
  - RPM(Reverse Path Multicasting)

### 멀티캐스트 트리 생성 3단계 알고리즘

- Reverse Path Forwarding

  - 멀티캐스트의 소스가 있는 방향으로부터 온 패킷만을 포워딩
  - 유니캐스트 라우팅 테이블 참조
  - 루프현상 제거

- Reverse Path Broadcasting

  - 여러 네트워크에 연결된 네트워크로는 복수개의 패킷 전달
  - 부모 노드를 정해 하나만 수신하게 함

- Reverse Path Multicasting

* IGMP 통해 모은 정보를 기초로, 멤버가 없으면 메세지 교환을 통해 Prune(leaving, 나중에 다시 Join 가능)

![40](/assets/images/2024-04-18/40.png)

![41](/assets/images/2024-04-18/41.png)

### 6.3.2 Multicast Link State (MOSPF)

- Open Shortest Path First(OSPF)의 확장
- Source-based tree approach
- 각 라우터에 있는 그룹 멤버 정보를 통해서 트리 생성
  - 그룹내의 각 소스를 루트로 하는 트리 생성
  - 라우터 자신을 루트로 하는 서브트리 생성
  - 멤버가 없는 경로를 Prune
    - 각 라우터는 IGMP를 통해 멤버 정보를 만들어, 플러딩을 통해 모든 라우터에게 정보 전달
  - 트리를 이용해 멀티캐스트 데이터 포워딩

![18](/assets/images/2024-04-18/18.png)

### 6.3.3 PIM (Protocol Independent Multicast)

- PIM은 유니캐스트 라우팅 프로토콜 종류와 관계없이 사용가능한 멀티캐스트 프로토콜
  - 멀티캐스트 패킷을 포워딩하기 위해 유니캐스트 라우팅 테이블을 이용하지만, 어떻게 생성되었는지는 무관
- 2가지의 모드
  - Dense mode: 그룹의 액티브 멤버가 많을 때
  - Sparse mode: 소수 라우터만이 액티브 멤버를 가질 때

### PIM-DM

- 소스기반 트리의 DVMRP와 비슷하지만 단순화 하였음
  - RPF와 RPM만 사용
- 2 Step

* 멀티미디어 패킷 수신시 RPF 알고리즘 사용하여 검사
  - 수신 방향 검사
* 수신 방향 맞으면 처음에는 Broadcasting, 나중에 Prune 이후는 멀티캐스팅

![44](/assets/images/2024-04-18/44.png)

### PIM-SM

- 그룹 공유 트리
- Designated Router (Core Router, 랑데뷰 포인터)
- 2 Step:
  - 소스는 유니캐스트 터널링으로 멀티캐스트 패킷을 랑데뷰포인터로
  - 코어 라우터는 공유트리로 포워딩
- 그룹마다 공유트리, 랑데뷰포인터 한 개씩 필요

![45](/assets/images/2024-04-18/45.png)

### 6.4 Interdomain Routing Protocols

- AS(Autonomous System) 간
- MBGP(Multicast Border Gateway Protocol)
  - BGP의 확장
- BGMP(Border Gateway Multicast Protocol)

### 6.5 IGMP

- Internet Group Management Protocol
- 그룹 멤버 정보를 획득에 사용
- ICMP와 같이 IP 패킷에 캡슐화 되어 전달

### 6.5.1 IGMPv3 메세지

- Query 메세지

  - 라우터가 네트워크 내의 모든 호스트에게 주기적으로 보냄
  - 특정 그룹의 멤버가 있는지 확인
    - General query message: 모든 멀티태스크 그룹 멤버정보 원함
    - Group-specific query message: 특정 그룹 멤버정보 원함
    - Source-and-group-specific query message

- Report 메세지
  - 호스트가 query 메세지에 대한 응답
  - 그룹 주소 레코드의 리스트 포함

### 6.5.3 Encapsulation

- IP 패킷으로 캡슐화
  - protocol 필드값 2
- TTL 필드값 1: 현재 네트워크 내만 전파
- 메세지별 IP 패킷의 목적지 주소값
