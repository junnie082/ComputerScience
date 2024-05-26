# SCTP (Stream Control Transmission Protocol)란 무엇일까

[[컴퓨터망]16. SCTP(Stream Control Transmission Protocol)란 무엇일까](https://velog.io/@yooniverseis/16.-SCTPStream-Control-Transmission-Protocol%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

#### 2024.05.26.(일)

## 1. Intro

STCP란, Stream Control Transmission Protocol 의 약어로 메세지 기반의 transport layer protocol 이다.

![16](/assets/images/2024-05-26/16.png)

UDP의 메세지 스트리밍 특성과 TCP의 연결형 및 신뢰성 제공을 조합한 프로토콜이다.

## 2. SCTP Services

STCP는 application layer의 process에 다음과 같은 서비스들을 제공한다.

- Process-to-Process Communication
- Multiple Streams
- Multihoming
- Full-Duplex Communication
- Connection-Oriented Service
- Reliable Service

![17](/assets/images/2024-05-26/17.png)

> > SCTP 프로토콜은 multiple stream이 가능하다.

> > STCP 프로토콜은 각 호스트에 multiple IP address를 할당할 수 있다.

## 3. SCTP features

이전에 공부했던 TCP와 비교하여 SCTP의 특성을 비교해보자

### Transmission Sequence Number (TSN)

SCTP에서, data chunk들은 TSN을 사용하여 번호가 붙여진다.

### Stream Identifier (SI)

서로 다른 스트림을 SCTP는 SI를 사용한다.

### Stream Sequence Number (SSN)

같은 스트림 내에 있는 서로 다른 data chunks를 구분하기 위해 SSN을 사용한다.

## 4. TCP vs SCTP

TCP와 SCTP의 차이점

> > TCP has segments, SCTP has packets.

> > SCTP에서 control 정보와 데이터는 다른 chunks로 분리되어 전달된다.

Data chunk는 세 개의 identifier로 식별된다.

`TSN, SI, SSN` 이렇게 세가지

SCTP에서, acknowledgment number는 data chunk를 받았음을 알리기 위해 사용된다.
control chunk는 다른 control chunk에 의해 알려짐.

## 5. Packet format

data/control chunk와 packet의 포맷을 살펴보자.

### SCTP packet format

> > SCTP 패킷에서, control chunks는 data chunks 이전에 온다.

> > Chunk는 32 bit (즉, 4바이트) 내

> > SCTP 패킷의 padding byte는 길이 필드에 포함되지 않는다.

### Data chunk

하나의 data chunk는 하나의 메세지 이상을 포함하는 데이터를 전송하지 못한다. 그러나 메세지는 몇 개의 chunks로 나눠질 수 있다.

> > data chunk의 data field는 적어도 1바이트 이상의 데이터를 운반해야 한다. 즉, lengh field는 17보다 작을 수 없다.

### INIT chunk

> > INT chunk가 운반되는 패킷에는 다른 chunk는 운반 불가능하다.

### INT ACK chunk

> > 위와 마찬가지로 INT ACK chunk를 운반하는 패킷에는 다른 chunk 운반 불가능

### COOKIE ECHO chunk

### COOKIE ACK

### SACK chunk

### HEARTBEAT and HEARTBEAT ACK chunk

### SHUTDOWN chunk

### ERROR chunk

### ABORT chunk

## 5. an SCTP association

TCP와 SCTP의 공통점!
바로 연결지향(Connection-oriented) 프로토콜이다.

SCTP에서 connection은 **_association_** 이라고 불린다.
multihoming 기능을 강조하는 용어이다.

TCP에서와는 다르게 four-way handshaking으로 association을 진행

### Data Transfer

위에서 INIT/INIT ACK 패킷은 다른 chunk를 전송할 수 없다고 했다.
COOKIE ECHO/COOKIE ACK chunk는 data chunk를 전송할 수 있다.

SCTP에서 data chunk는 TSN을 소모한다.
받고 ack를 전송해야하는 건 data chunk 뿐이다.

SCTP에서 ack는 축적된 TSN이다.
The TSN of the last data chunk received in order

### Association Termination

### Association Abortion

## 6. State transition diagram

![18](/assets/images/2024-05-26/18.png)
