# Unix Lecture 9

## Interprocess Communication

#### 2023.12.18.(월)

[[운영체제/OS] Interprocess Communication (IPC)란?](https://hidemasa.tistory.com/49)

프로세스들은 independent 일 수도 있고 cooperating 일 수도 있습니다.

cooperating 한 프로세스들은 공유 data를 포함해 프로세스들끼리 영향을 줘요.
정보 공유, 속도 향상, 모듈화의 목적을 가지고 있습니다.

이를 위해 Interprocess Communication (IPC)를 필요로 하는데요, IPC는 두 가지 모델, `Shared memory`와 `Message Passing`이 있습니다.

#### Shared memory 모델

프로세스들이 공유하고 싶은 메모리의 영역이 있을 거에요.
communication에 대한 컨트롤은 OS가 아닌 user 프로세스들이 가지고 있습니다.

가장 중요한 점은 shared memory에 대해 User 프로세스들이 synchronize(동기화)를 허용한다는 것이에요.

shared memory에 대한 IPC는 producer 프로세스와 consumer 프로세스가 있어요.
`생산자-소비자 문제`는 공유 메모리를 사용하여 해결합니다.
동시에 수행하고 공유메모리(버퍼)를 통해 상호통신을 해요.
생산자는 item을 생산하고 이를 버퍼에 저장하면, 소비자는 이 버퍼에 있는 item을 소비해요. 기용한 항목이 있을 때만 소비가 이루어지도록, 생산자/소비자 프로세스는 동기화 되어야 합니다.

#### Message Passing 모델

프로세스들끼리 대화하고 동기화가 이루어지도록 하는 메커니즘이에요.
IPC는 두 가지 동작을 제공해요: `send(message)`와 `receive(message)`
메세지의 사이즈는 고정이거나 가변입니다.
프로세스 P와 Q가 communicate 하고 싶다면, communication link를 만들어 send/receive를 통해 메세지 교환합니다.
이 communication link는 physical하게는 shared memory, hardware bus, network

logical 하게는 direct or indirect, synchronous or asynchronous, automatic or explicit buffering 하게 실행됩니다.

#### Direct Communication (1대 1 통신)

프로세스들은 각자의 이름을 explicity(명확히) 명시합니다.

- send(P, message): P로 메세지를 보내라
- receive(Q, message): Q로부터 메세지를 받아라

한 쌍에는 하나의 링크만 존재하고, 링크는 자동적으로 생성돼요.

#### Indirect Communication (다대다 통신)

메세지들의 mailbox(또는 ports)로부터 send와 receive가 이루어져요.
각 mailbox는 고유의 id를 가지고

- send(A, message): mailbox A로 메세지를 보내라.
- receive(A, message): mailbox A로부터 메세지를 받아라.

[IPC InterProcess Communication 프로세스 간 통신](http://www.ktword.co.kr/test/view/view.php?m_temp1=302)

1. IPC (InterProcess Communication, 프로세스 간 통신)이란?

- 실행 프로세스 간에 통신을 가능케하는 메커니즘

  - 프로세스 간에 서로 통신하는 규칙에 관한 문제

- 호스트의 가종, 운영체제 등이 무엇인지 관계 없이 일정 규칙으로 통신하는 것이 매우 중요함

  - 통상, 두 프로세스 간에 일정 포멧과 순서를 따르는 메세지를 주고 받으며 통신을 하게 됨

- 결국, 멀티태스킹 운영체제 내 또는 네트워크화(Networked)/분산된(Distributed) 컴퓨터들 사이에,
  - 각각 실행되는 프로세스 간에 정보의 교환을 가능케 하는 방법

2. IPC 구현 기법들 구분

- 전통적인 주요 기법 둘

  - 공유 메모리 기법 (Shared Memory)
    - 협력적 프로세스들이 공유 메모리 영역을 통해 데이터 교환
      - 주로, 전역 변수, 공유 변수, 공유 파일을 통해 통신을 이룸
    - 충돌 가능성은 있으나, 일반적으로 더 빠르고 커널 도움이 거의 필요 없음
      - 즉, 고속 통신이 가능하나, 통신 책임이 응용 프로세스에 있고,
      - 운영체제는 단지 공유 메모리만 제공함
  - 메세지 전달 기법 (Message Passing)
    - 협력적 프로세스 간에 메세지 교환
    - 충돌하지 않으나, 적은 양의 데이터 교환 위주
    - 통신 책임 및 수단이 운영체제에 의해 제공됨

- 유닉스 초기 IPC 구현 3가지 기법

  - 공유 메모리 (Shared Memory)
    - 다중 프로세스들이 가상 메모리(Virtual Memory)를 공유
    - 협력 프로세스들 간에 공유되는 메모리 영역이 운영체제에 의해 구축 제공됨
  - 세마포어 (Semaphore): 공유 자료에 대한 접근을 통제하는 일종의 카운터
  - 메세지 큐 (Message Queue)

- 메세지 전달 (Message Passing)에 기반을 둔 기법들
  - 시그널 (Signal)
    - 실시간으로 프로세스에 이벤트 발생을 알리기 위함
  - 세마포어 (Semaphore)
    - 공유 자료에 대한 접근을 통제하는 일종의 카운터
  - 파이프 (Pipe)
    - 2개의 프로세스 입출력을 직렬 연결하여 하나의 공유 페이지를 제공
      - ls | pr (ls를 수행한 표준출력이 pr의 표준입력으로 전달됨)
    - 이름 없는 파이프: 상호 관련 있는 프로세스 간 통신에 사용
    - 이름 있는 파이프(Named Pipe): 장치 파일을 통해, 상호 관련 없는 프로세스 간 통신에 사용
  - 소켓 (Socket)
    - 네트워크에 기반을 둔 IPC 메커니즘
    - 파이프 개념을 네트워크로 확장시킨 것
  - RPC (Remote Procedure Call): 클라이언트-서버 모델을 이용하는 기법
    - 원격지 프로세스 간에 함수의 호출에 기반을 둔 기법
    - 한편, LPC(Local Procedure Call)는 동일 시스템 내에서 호출 기법을 말함
