# Segment

## [[Network] Segment, 세그먼트](https://velog.io/@nnnyeong/Network-Segment-%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8)

#### 2024.05.26.(일)

## 세그먼트 구조

결론부터 말하자면 TCP 세그먼트는 세그먼트 헤더와 실제 데이터로 구성되어 있다!

`TCP SEGMENT = TCP 헤더 (세그먼트 헤더) + 데이터`

TCP 프로토콜은 데이터 스트림으로부터 데이터를 받아들이고 이것을 일정 단위로 분할한 뒤 TCP 헤더를 덧붙여 TCP 세그먼트를 생성하고, 이렇게 만들어진 TCP 세그먼트는 IP 데이터 그램에 캡슐화 되어 상대방에게 보내지게 된다.

```
IP 데이터그램이란?

IP에서 사용하는 패킷을 말한다, = IP PDU
```

```
패킷이란?

컴퓨터 네트워크가 전달하는 데이터의 형식화된 블록이다.
```

즉, 정리하자면

**_Segment = TCP 프로토콜 데이터 유닛(PDU)_**를 의미! & **_데이터그램 = IP PDU_** 인 것이다!

## TCP Segment 구조

![1](/assets/images/2024-05-26/1.png)

위에서 설명한 TCP 세그먼트의 구조를 그림으로 살펴보자!

실제 보내려는 **_데이터는 보라색 Data 영역_**에 담겨지고 어디서 보내는지, 어디로 보내는지, 몇번째인지 등등의 정보들은 모두 하늘색 영역인 헤더 영역에 담겨지게 된다!

각 영역에 어떤 정보가 담기는지 보자면,

#### Source Port

- 송신포트
- 16 비트 할당되어 2의 16승까지 표현 가능
- 세그먼트를 보내는 송신측의 포트 번호(로컬 포트)

#### Destination Port

- 수신포트
- 16 비트 할당되어 2의 16승까지 표현 가능
- 목적지 포트, 수신측의 포트 번호

#### Sequence Number

- 32 비트 할당, 송신 바이트 흐름의 위치를 가리킴

수신자는 쪼개진 세그먼트의 순서를 파악하여 올바른 순서로 데이터를 재조립하게 된다. 송신자가 최초로 데이터를 전송할 때는 이 번호를 랜덤한 수로 초기화, 이후 **_자신이 보낼 데이터의 1byte 당 시퀀스 번호를 1씩 증가_**시켜 데이터의 순서를 표현한다.

#### Acknowledgement Number

- 32 비트 할당
- ACK 플래그가 설정된 경우 acknowledgement number 필드 값은 수신자가 예상하는 다음 시퀀스 번호를 의미한다!
- 승인 번호는 `다음에 보내줘야 하는 데이터의 시작점`을 의미!

ex) 1MB 짜리 데이터를 100bytes 씩 보낼 때

송신자는 처음 100bytes 만큼만 데이터를 전송하면서 시퀀스 번호를 0으로 초기화한다. 시퀀스 번호는 1byte당 1씩 증가하기 때문에 100byte의 데이터를 전송하면 이후 전송 받아야할 시퀀스 번호를 100이 될 것이다!

#### Data Offset

- 4 bit 할당
- 전체 세그먼트 중에서 헤더가 아닌 데이터가 시작되는 위치가 어디서부터인지가 표시됨
- option 필드의 길이가 고정되어 있지 않기 때문에 필요한 필드!

data offset을 표기할 때는 32비트 워드 단위를 사용하며, 32비트 체계에서 1워드 = 4bytes를 의미하므로 data offset 필드의 값에 4를 곱하면 세그먼트에서 헤더를 제외한 실제 데이터의 시작 위치를 알 수 있게 된다!

4bit가 할당되었으므로 표현할 수 있는 값의 범위를 0000~1111, 즉 0~15 word 이므로 기본적으로 0~60bytes의 오프셋까지 표현할 수 있다. 하지만 옵션 필드를 제외한 나머지 필드는 필수로 존재해야 하기 때문에 최소값은 20bytes (5 word)로 고정되어 있다!

#### Reversed

- 3 bit 할당
- 미래에 사용하기 위해 남겨둔 예비 필드
- 모두 0으로 채워져야 함

#### Flag

- 1 bit 씩 할당된 9개의 비트 플래그
- 이 플래그들은 현재 세그먼트의 속성을 나타냄
- 기존에는 6개의 플래그만을 사용했지만, 혼잡 제어 기능의 향상을 위해 reversed 필드를 사용해 NS, CWR, ECE 플래그가 추가

| URG | Urgent Pointer (긴급 포인터) 필드에 값이 채워져 있음을 알리는 플래그, 이 포인터가 가리키는 긴급한 데이터는 높게 처리되어 먼저 처리된다, 요즘엔 잘 사용되지 않음 |
| ACK | Acknowledgement(승인 번호) 필드에 값이 채워져 있음을 알리는 플래그, 이 플래그가 0이라면 승인 번호 필드 자체가 무시됨 |
| PSH | Push 플래그, 수신측에게 이 데이터를 최대한 빠르게 응용 프로그램에게 전달해달라는 플래그이다. 이 플래그가 0이라면 수신측은 자신의 버퍼가 다 채워질 때까지 기다림, 1이라면 이 세그먼트 이후에 더 이상 연결된 세그먼트가 없음을 의미 |
| RST | Reset 플래그, 이미 연결이 확립되어 ESTABLISHED 상태인 상대방에게 연결을 강제로 리셋해달라는 요청을 의미 |
| SYN | Synchronize 플래그, 상대방과 연결을 생성할 때, 시퀀스 번호의 동기화를 맞추기 위한 세그먼트 |
| FIN | Finish 플래그, 상대방과 연결을 종료하고 싶다는 요청을 의미 |

#### Window Size

- 한번에 전송할 수 있는 데이터의 양을 의미하는 값을 담음
- 2의 16승 만큼의 값 표현 가능
- 단위는 byte -> 윈도우의 최대 크기는 64 byte

#### CheckSum

- 데이터 송신 중에 발생할 수 있는 오류를 검출하기 위한 값
- 송신측이 보낸 데이터에 뭔가 변조가 있었음을 알 수 있음

TCP의 체크섬은 전송할 데이터를 16 bits 씩 나누어 차례대로 더해가는 방법으로 생성

#### Urgent Pointer

- 긴급포인터
- UPG 플래그가 1이라면 수신측은 이 포인터가 가리키고 있는 데이터를 우선적으로 처리함

#### Options

- TCP의 기능을 확장할 때 사용하는 필드들
- 이 필드의 크기는 고정되어 있지 않고 가변적
- 때문에 수신측이 어디까지가 헤더이고 어디서부터 데이터인지 알기 위해 데이터 오프셋 필드를 사용함

데이터 오프셋 필드 20~60 bytes의 값을 표현할 수 있는데, 아무런 옵션도 사용하지 않은 헤더의 길이 (source port 부터 urgent pointer)가 20bytes 이고 옵션을 모두 사용했을 때 옵션 필드의 최대 길이가 40bytes이기 때문이다.

만약 데이터 오프셋 필드의 값이 5, 즉 20bytes 보다 크지만 TCP 옵션을 하나도 사용하고 있지 않다면, 초과한 bytes 만큼 이 필드를 0으로 채워줘야 수신측이 헤더의 크기를 올바르게 측정할 수 있다.
