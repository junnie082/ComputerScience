# Network 5

# Chapter 05 유니캐스트 라우팅(2)

#### 2024.04.18.(목)

### 4.3 유니캐스트 라우팅 프로토콜

- 인터넷에서 공통으로 사용되는 3 프로토콜: RIP, OSPF, BGP
- Routing Information Protocol (RIP)
  - based on the distance-vector algorithm
- Open Shortest Path First (OSPF)
  - based on the link-state algorithm
- Border Gateway Protocol (BGP)
  - based on the path-vector algorithm

### 4.3.1 인터넷 구조

- 인터넷은 하나의 백본을 가진 트리구조에서, 서로 다른 사설 회사에 의해 운영되는 다수의 백본을 가진 구조로 바뀌어 왔음

- 현재 인터넷 구조는 그림 4.14과 같은 형태를 가지고 있음

![18](/assets/images/2024-04-18/18.png)

### 4.3.2 RIP

- Routing Information Protocol(RIP)는 가장 널리 사용되는 거리-벡터 라우팅 알고리즘 기반의 도메인내(intradomain) 라우팅 프로토콜

- RIP는 초기에 Xerox Network System(XNS)에 사용됐지만, UNIX 버전의 Berkeley Software Distribution(BSD)에 사용된 이후로, 널리 사용됨

![19](/assets/images/2024-04-18/19.png)

![20](/assets/images/2024-04-18/20.png)

- 다음 그림은 AS(autonomous system) 내에서 RIP 동작의 좀더 실제적인 예이다. 부팅 이후에 처음 만들어지는 포워딩 테이블을 볼 수 있을 것이다. 이후, 업데이트 메세지를 교환한 이후에 변경되는 변화를 볼 수 있다. 최종적으로 더 이상의 변화가 없는 안정된 포워딩 테이블을 볼 수 있다.

![21](/assets/images/2024-04-18/21.png)

![22](/assets/images/2024-04-18/22.png)

### 4.3.3 Open Shortest Path First

- Open Shortest Path First(OSPF)도 RIP와 같이, 도메인내 라우팅 프로토콜
- 링크 스트레이트 라우팅 프로토콜 기반
- OSPF은 공개(open) 프로토콜
  - 규격이 문서로 공개되어 있음

![23](/assets/images/2024-04-18/23.png)

![24](/assets/images/2024-04-18/24.png)

![25](/assets/images/2024-04-18/25.png)

![26](/assets/images/2024-04-18/26.png)

### 4.3.4 BGP

- Border Gateway Protocol version 4(BGP4)는 인터넷에서 사용되고 있는 도메인간(interdomain) 라우팅 프로토콜
- BGP4는 path-vector 알고리즘 기반
  - 네트워크 도달 가능성 정보 제공에 맞춰짐
- eBGP(external BGP): AS의 경계 라우터에서 작동
  - 다른 AS와 정보 교환
- iBGP(internal BGP): 모든 라우터에서 작동
  - AS내 모든 라우터와 정보 교환

![27](/assets/images/2024-04-18/27.png)

![28](/assets/images/2024-04-18/28.png)

![29](/assets/images/2024-04-18/29.png)

![30](/assets/images/2024-04-18/30.png)

![31](/assets/images/2024-04-18/31.png)

![32](/assets/images/2024-04-18/32.png)
