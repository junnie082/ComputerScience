# 3-way handshaking (연결 과정)

## [[네트워크] TCP 프로토콜 연결/종료 과정](https://velog.io/@ragnarok_code/Network-TCP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%97%B0%EA%B2%B0%EC%A2%85%EB%A3%8C-%EA%B3%BC%EC%A0%95)

#### 2024.05.26.(일)

## 3-way handshaking (연결 과정)

![5](/assets/images/2024-05-26/5.png)

### 3-way handshake 란?

- TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결을 설정(Connection Establish)하는 과정
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
- 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

A 프로세스(Client)가 B 프로세스(Server)에 연결을 요청

#### a. A -> B: SYN

- 접속 요청 프로세스 A가 연결 요청 메세지 전송 (SYN)
- 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태 - B: LISTEN, A: CLOSED

#### b. B -> A: SYN + ACK

- 접속 요청을 받은 프로세스 B가 요청을 수락했으며, 접속 요청 프로세스인 A도 포트를 열어 달라는 메세지 전송 (SYN + ACK)
- 수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태 - B: SYN_RCV, A: CLOSED

#### c. A -> B: ACK

- PORT 상태 - B: SYN_RCV, A: ESTABLISHED
- 마지막으로 접속 요청 프로세스 A가 수락 확인을 보내 연결을 맺음 (ACK)
- 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
- PORT 상태 - B: ESTABLISEHD, A: ESTABLISHED

## 4-way handshaking (연결 해제 과정)

![6](/assets/images/2024-05-26/6.png)

### 4-way handshake란?

- TCP의 연결을 해제(Connection Termination)하는 과정

A 프로세스(Client)가 B 프로세스(Server)에 연결 해제를 요청

#### a. A -> B: FIN

- 프로세스 A가 연결을 종료하겠다는 FIN 플래그를 전송
- 프로세스 B가 FIN 플래그로 응답하기 전까지 연결을 계속 유지

#### b. B -> A: ACK

- 프로세스 B는 일단 확인 메세지를 보내고 자신의 통신이 끝날 때까지 기다린다. (이 상태가 TIME_WAIT 상태)
- 수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 그리고 자신이 전송할 데이터가 남아있다면 이어서 계속 전송한다.

#### c. B -> A: FIN

- 프로세스 B가 통신이 끝났으면 연결 종료 요청에 합의한다는 의미로 프로세스 A에게 FIN 플래그를 전송

#### d. A -> B: ACK

- 프로세스 A는 확인했다는 메세지를 전송

```
포트(PORT) 상태 정보

* CLOSED: 포트가 닫힌 상태
* LISTEN: 포트가 열린 상태로 연결 요청 대기 중
* SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
* ESTABLISHED: 포트 연결 상태
```

```
Flag Bit

* URG: 긴급 위치 필드 유효 여부 설정
* ACK: 응답 유효 여부 설정. 최초의 SYN 패킷 이후 모든 패킷은 ACK 플래그 설정 필요. 데이터를 잘 받았으면 긍정 응답으로 ACK(=SYN + 1) 전송
* PSH: 수신측에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때
* RST: 연결 리셋 응답 혹은 유효하지 않은 세그먼트 응답
* SYN: 연결 설정 요청. 양쪽이 보낸 최초 패킷에만 SYN 플래그 설정
* FIN: 연결 종료 의사 표시
```

```
플래그 정보

TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.

* 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.

```

```
SYN(Synchronize Sequence Number) / 0000010

* 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
```

```
ACK (Acknowledgement) / 0100000

* 응답확인. 패킷을 받았다는 것을 의미한다.
* Acknowledgement Number 필드가 유효한지를 나타낸다.
* 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
```

```
FIN(Finish) / 0000001

* 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더이상 전송할 데이터가 없음을 의미한다.
```

### Q1: TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유?

- Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있기 때문에 일단 FIN에 대한 ACK만 보내고, 데이터를 모두 전송한 후에 자신도 FIN 메세지를 보내기 때문이다.

### Q2: 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?

- 이러한 현상에 대비하여 Client는 Server로부터 FIN 플래그를 수신하더라도 일정시간(Default 240sec)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. (TIME_WAIT 과정)

### Q3: 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?

- Connection을 맺을 때 사용하는 포트(Port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다. 서버측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순차적인 Number가 전송된다면 이전의 Connection으로부터 오는 패킷으로 인식할 수 있다. 이런 문제가 발생할 가능성을 줄이기 위해서 난수로 ISN을 설정한다.
