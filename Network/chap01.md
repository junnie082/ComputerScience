# Network 1

# Chapter 01 OSI 7계층과 TCP/IP

#### 2024.04.17.(수)

### 1. 분산 컴퓨팅

컴퓨터 A 컴퓨터 B
Process <-> Process (통신 필요)

Protocol < - 네트워크 - > Protocol

- process: 실행 중인 프로그램
- program: 실행 가능한 파일

### 2. OSI 7 계층

| 7계층 | Application Layer | 응용 계층 |
| 6계층 | Presentation Layer | 표현 계층 |
| 5계층 | Session Layer | 세션 계층 |
| 4계층 | Transport Layer | 전송(수송) 계층 |
| 3계층 | Network Layer | 네트워크 계층 |
| 2계층 | Data Link Layer | 데이터링크 계층 |
| 1계층 | Physical Layer | 물리 계층 |

1. 응용계층 <- 응용계층 프로토콜 -> 응용계층
2. 표현계층 <- 프레젠테이션 프로토콜 -> 표현계층
3. 세션계층 <- 세션계층 프로토콜 -> 세션계층
4. 전송계층 <- 전송계층 프로토콜 -> 전송계층
5. 네트워크 계층 <- 네트워크 -> <- 라우터 -> 네트워크 계층
6. 데이터링크 계층 <- 데이터링크 -> <- 브릿지 -> 데이터링크 계층
7. 물리계층 <- 물리계층 -> <- 리피터 -> 물리계층

### 3. 계층별 핵심 기능

| 응용 계층 | 다양한 네트워크 서비스 제공 (이메일, 파일 전송) |
| 표현 계층 | 데이터의 표현 방식 정의 (ASCII, 압축, 암호화) |
| 세션 계층 | 응용 프로세스 간의 연결 관리 |
| 수송 계층 | 종단간(end-to-end) 신뢰성있는 정보 전달 |
| 네트워크 계층 | 네트워크를 통한 데이터 전달 |
| 데이터링크 계층 | 인접시스템간(neighbor)의 신뢰성있는 정보 전달 |
| 물리 계층 | 물리 매체를 통한 0과 1의 비트 전송 |

#### < 물리 계층 >

- 물리 매체를 통한 0 또는 1의 비트 데이터 전달

ex: 10100110 -> _-_-_----_

- 물리 매체: 전선, 광, 공기(무선)

- 전기적 정의: 전압, 주파수, 인코딩방식(NRZ, Manchester, ...)

- 기계적 정의: 커넥터, 25핀/9핀, male/female

- 기능적 정의: +/-/접지(GND), Tx/Rx, Clock, DTR/DSR

#### < Data Link 계층 >

- 인접 시스템 간(neightbor) 신뢰성 있는 정보 전달

- 데이터 링크(data link): 데이터가 흐르는 시스템 간의 물리적 연결

- 데이터를 바이트 단위 혹은 프레임으로 구성하여 전달

- 신뢰성 보장 위한 기능: 연결 제어, 오류 제어, 흐름 제어
  - 연결 제어: 연결을 맺고 데이터를 전달, 이후 연결 종료
  - 오류 제어: 오류 검출, 오류 정정
  - 흐름 제어: 데이터 전달 속도가 너무 빠르거나 느리지 않게 조절

#### < 네트워크 계층 >

- 네트워크를 통한 데이터 전달 기능

- 시스템의 수가 적고 가까이 있다면, 하나의 네트워크로 구성하여 모든 컴퓨터들이 직접 데이터 교환

- 컴퓨터의 수가 많고 지리적으로 떨어져 있으므로, 지역별로 네트워크를 구성하고, 이것들을 서로 연결
  (Internet: 전 세계의 모든 네트워크를 연결한 네트워크)

- 어드레싱(Addressing): 각 시스템에 주소 부여

- 라우팅(Routing): 목적지로 갈 수 있도록 경로를 찾아주는 것

#### < 수송 계층 >

- 종단 시스템 간 신뢰성 있는 정보 전달

  - 구간별로 데이터링크 계층에서 담당하지만, 종단간의 추가적 보장이 필요

- 종단 시스템 - 네트워크 - 종단 시스템

- 신뢰성 보장: 연결 제어, 오류 제어, 흐름 제어
  - 네트워크의 상태도 고려한 흐름 제어 필요

#### < 세션 계층 >

- 응용 프로세스 간의 연결 제어

- 세션(session): 응용 프로세스 간의 연결

- 세션의 수립, 관리, 해제

#### < 표현 계층 >

- 데이터의 표현 계층(Syntax)

- 데이터의 형식, 암호화, 압축

- ASCII/binary, jpg/gif, mpg/avi

#### < 응용 계층 >

- 응용 프로세스에게 네트워크 서비스 제공

- 네트워크 서비스
  - 웹 데이터 전달(HTTP)
  - 파일 전송(FTP)
  - 이메일(SMTP)
  - 원격 접속(Telnet)
  - 네트워크 관리(SNMP)

### 4. 데이터 전달

Process -> Layer 7 -> Layer 6 -> Layer 5 -> Layer 4 -> Layer 3 -> Layer 2 -> Layer 1 -> 네트워크 -> Layer 1 -> Layer 2 -> Layer 3 -> Layer 4 -> Layer 5 -> Layer 6 -> Layer 7 -> Process

#### 캡슐화

![1](/assets/images/2024-04-17/1.png)

### TCP/IP 프로토콜 데이터 단위

Application program: Message -> TCP header | Application data: TCP segment -> IP header | TCP header | Application data: IP packet -> Eternet header | IP header | TCP header | Application data | Ethernet footer: Ethernet frame

### 프로토콜 유형

- 연결 (Connection)

* 연결 지향(Connection-oriented) protocol
  - Connection establishment phase
  - Data transfer phase
  - Disconnect phase
* 비연결(Connectionless) protocol
* data transfer phase

- 프로토콜 종류

  - TCP/IP 프로토콜
  - OSI 프로토콜

- 거리/기술
  - WAN/MAN/LAN/PAN/BAN

### 5. TCP/IP Protocol Stack

![2](/assets/images/2024-04-17/2.png)

### < Web Server & Client >

![3](/assets/images/2024-04-17/3.png)

### 6. LAN Protocol

- Layer 2

  - LLC Sublayer: Logical Link Control - 연결 제어, 오류 제어, 흐름 제어
  - MAC Sublayer: Media Access Control - 프레임 단위 송.수신, 충돌 방지

- Layer 1
  - Physical: 물리 매체 접속

### LAN Protocol Standard

- IEEE 802 표준
- IEEE 802.3: CSMA/CD
- IEEE 802.11: Wireless LAN
- IEEE 802.15: Wireless PAN
- IEEE 802.2: LLC(Logical Link Control)
- IEEE 802.1: Management, 상위 계층 접속, Spanning Tree, VLAN

### 이더넷 (Ethernet) Protocol

- MAC Protocol + Physical Protocol
- CSMA/CD (Carrier Sense Multiple Access/Collision Detect)
- Ethernet: 10Mbps
- FastEthernet: 1000Mbps
- GigaEthernet: 1Gbps (1000Mbps)

### Ethernet Protocol

![4](/assets/images/2024-04-17/4.png)

### 7. 네트워크 장비

![5](/assets/images/2024-04-17/5.png)

1. physical: repeater
2. data link: bridge
3. network: router
4. transport: L4 switch
5. session: gateway
6. presentation: gateway
7. application: gateway

### (1) Repeater

![6](/assets/images/2024-04-17/6.png)

### (2) Bridge

![7](/assets/images/2024-04-17/7.png)

- MAC 주소를 보고 포워딩/블럭킹

  - Flat -> 소규모 네트워크

- MAC 주소 테이블

  - DA, 포트, 시간정보
  - 프레임의 송신지 주소를 이용해 만듦

- Promiscuous Mode 동작

  - 모든 프레임 수신

- 목적지 정보 없을 때

  - Flooding (수신포트 제외 모든 포트로 전송)

- 브리지 종류
  - Transparent Bridge
  - Source Routing Bridge

### (3) Router

![8](/assets/images/2024-04-17/8.png)

![9](/assets/images/2024-04-17/9.png)

### (4) Gateway

- Gateway
  - 7계층 장비(Layer 1-7)
  - 프로토콜 중계
  - OSI Gateway(TCP/IP <-> OSI)
  - SNA Gateway(TCP/IP <-> SNA)

### (5) Switch

- 고속 (하드웨어화), 다수의 포트
- L2 Switch: MAC 주소. 다수 포트
- L3 Switch: IP 주소. 고속 스위칭
- L4 Switch: 포트 번호 이용 트래픽 제어. 로드 밸런싱
- L7 Switch: 로드밸런싱, QoS 지원. 보안, 바이러스/웜 차단

### (6) Hub

- 초창기에는 멀티포트 Repeater 지칭

  - 단순 플러딩
  - 충돌 시 모든 포트에 영향

- 많은 포트가 있는 장치
  - Multiport device

### (7) Other Network Devices

- Proxy

  - Layer 7
  - Web Proxy (보안, 캐싱), E-mail Proxy

- LAN Card

  - L1, L2

- 공유기

  - Static Routing 지원. NAT 기능
  - 스위치 내장
  - 유/무선 인터페이스
  - DCHP 서버 내장

- Transceiver
  - MAU (Medium Access Unit)

### Web Proxy

![10](/assets/images/2024-04-17/10.png)
