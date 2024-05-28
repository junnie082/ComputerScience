# 응용계층(Application Layer)

## [[컴퓨터망] 17. 응용계층(Application Layer)에 대해 알아보자](https://velog.io/@yooniverseis/17.-%EC%9D%91%EC%9A%A9%EA%B3%84%EC%B8%B5Application-layer%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

#### 2024.05.27.(월)

## 1. Client-server paradigm

네트워크를 구성하는 목적은 사용자에게 서비스를 제공하기 위해서다.

local site의 user가 remote site computer의 서비스를 제공받으려면 어떻게 해야할까?

인터넷으로 연결된 local computer와 remote computer에서 제공받을/혹은 제공할 프로그램을 실행해야 한다.

1. local machine에서 다른 응용 프로그램에 서비스를 요청하는 컴퓨터를 client, remote machine에서 요청에 응답하는 컴퓨터를 server라고 한다. 다시말해 요청과 응답은 분리되어 있다.

2. One-to-many: 서버는 서비스를 요청하는 여러개의 client에게 응답할 수 있다.

3. 서버 프로그램은 적절한 sw만 있으면 어떤 컴퓨터든 가능, 서버는 계속 돌아가야 된다.

4. 클라이언트는 언제든 원하는 시간에 서비스를 요청할 수 있지만, 서버는 클라이언트의 요청이 언제 올지 모르기 때문에 매시간 돌아가야 한다.

### Server와 Client

#### Server

- remote machine에서 실행된다
- client에 서비스를 제공
- client의 request를 받을 수 있는 문을 open
- request가 있기 전까지는 실행되지 않는다
- 한번 실행되면 계속 실행되면서 client의 요청을 받아야 한다

* request가 오면 응답한다

#### Client

- local machine에서 서버에 서비스를 요청하는 프로그램
- user에 의해 시작되고 서비스가 완료되면 종료
- IP 주소와 port 번호로 remote host에 접근한다

### Server types

서버의 타입은 다음과 같이 4종류가 있는데,
UDP는 Connectionless iterative,
TCP와 SCTP는 Connection-oriented concurrent 서버다

### Connectionless iterative server

UDP를 사용하는 서버는 iterative하다.
이 말은 서버는 한번에 하나의 request를 처리한다.
server는 UDP에서 datagram을 받아서, request 처리하고, reponse를 UDP를 통해 client에 전송한다.
이 경우 server는 다른 datagram을 신경쓰지 않는다. 다른 datagrams는 큐(queue)에 저장되어 처리되길 기다린다.

### Connection-oriented concurrent server

TCP나 SCTP를 사용하는 서버는 concurrent하다.
이 말은 **_서버는 한번에 여러 클라이언트를 처리할 수 있다_**는 말이다.
client의 request는 **_byte_** 단위의 **_stream_**이다.

이 서버는 하나의 port만 사용하면 안된다. 왜냐하면 각각의 클라이언트와의 연결이 port를 필요로 하기 때문에 동시에 여러 포트를 써야한다.

하지만 서버는 well-known port를 써야 하는데 이건 하나뿐이다!
이를 해결하기 위해 일단 client가 well-known port로 접근을 요청하면,
임시적인 다른 포트로 할당을 하고 well-known과의 연결을 해제한다.

서버는 여러개의 클라이언트를 상대하기 위해, 부모 프로세스를 카피하여 여러개의 child process를 생성한다.
각 connection별로 queue를 가지고 있으며, client로부터 온 segment는 적절한 큐에 저장된다.

### Socket Interfaces

client의 process는 어떻게 server의 process와 통신할까?

> > 하나의 interface는 instructions의 집합이다. 이 instructions는 두 개의 entities 사이의 상호작용을 위해 설계되어 있다.

몇몇의 interface는 통신을 위해 설계되어 있다.

> > :socket interface, transport layer interface(TLI), Stream

### Relation between the operating system and the TCP/IP suite

| Application layer |
| Socket interface |
| Transport layer |
| Network layer |
| Data link layer |
| Physical layer |

Operating System - Transport layer, Network layer, Data link layer, Physical layer

socket interface는 instruction의 집합으로, OS와 application layer 사이에 위치해있다.

### Concepts of sockets

application과 OS가 통신하기 위해서는 OS가 소켓을 만들어야 한다.
OS가 소켓을 생성하고 나면, application은 data를 받기 위해 해당 소켓에 연결한다. 통신을 위해서는 client와 server의 각 OS 소켓이 연결되어야 한다.

### Socket data structure

```C
struct socket
{
    int family;
    int type;
    int protocol;
    socketaddr local;
    socektaddr remote;
}
```

- Family: 프로토콜 그룹을 정의한다. (IPv4, IPv6, UNIX domain protocols 등등) IPv4인 경우 IF_INET, IPv6인 경우 IF_INET6으로 정의된다.

- Type: 소켓 타입을 정의한다. TCP일 경우 SOCK_STREAM, UDP일 경우 SPCK_DGRAM, SCTP일 경우 SOCL_SEQPACKET, application이 IP 서비스를 바로 사용하는 경우 SOCK_RAW

- Protocol: interface를 사용하는 프로토콜을 정의한다. TCP/IP protocol suite를 위해서 0으로 설정됨

- Local socket address: local socket address를 정의한다. IP번호와 port number로 구성되어 있다.

- Remote socket address: remote socket address를 정의한다.

여러가지 소켓 타입(TCP, UDP, SCTP)

### Communication using UDP

Connectionless iterative communication using UDP

- Server Process

1. 서버 프로세스가 먼저 실행되어야 한다.
2. 서버에서 socket 함수로 **_소켓을 생성_**한다.
3. bind 함수를 실행하여 소켓에 well-known port와 IP 주소를 할당한다.
4. 그 다음 recvfrom 함수는 datagram이 도착할 때까지 block 된다.
5. datagram이 도착하면, recvfrom 함수가 unblock 되어 client의 주소와 주소 길이를 알아낸 후 process에 전달한다.
6. process는 client의 주소와 주소 길이를 받은 후, **_요청을 처리_**한다.
7. 반환할 결과값을 sendto 함수를 통해 client에게 보낸다.
8. 클라이언트의 요청에 응답하기 위해 위의 4~7 과정을 반복한다.

- Client Process

1. socket 함수를 호출하여 서버와 통신할 소켓을 생성한다.
2. sendto 함수에 서버의 주소와 데이터가 담긴 버퍼의 위치를 넘겨준다.
3. recvfrom 함수는 서버에서 응답이 오기 전까지 block 된다.
4. 응답이 도착한 경우 recvfrom은 unblock되고, UDP가 client process에 data를 전달해준다.

#### Communication using TCP

Flow diagram for connection-oriented, concurrent communication

child process가 생성된 후 할일을 다 하고 destroyed 되지 않으면 zombie process가 된다. 얘는 공간을 차지하기 때문에 시스템의 성능에 영향을 줄 수 있다.

#### Variable used in echo client and echo server using TCP

## 2. Peer-to-peer paradigm

P2P 서비스는 두 대의 peer computer가 각각 서비스를 제공하고 받는 것이다. 서버와 클라이언트의 구분이 없다.
