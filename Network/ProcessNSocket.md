# Process와 Socket

[Process와 Socket](https://velog.io/@jeongbeom4693/Process%EC%99%80-Socket)

#### 2024.05.20.(월)

Process와 Socket은 네트워크 사용하는 통신 방법이라고 생각하면 된다.
그렇다면 `네트워크 애플리케이션`은 무엇인가?

## 네트워크 애플리케이션

```
! 네트워크 애플리케이션 개발의 중심은 다른 종단 시스템에서 동작하고 네트워크를 통해 서로 통신하는 프로그램을 작성하는 것이다.
```

예를 들어, 웹 애플리케이션에는 서로 통신하는 서버와 클라이언트로 구별되는 두 프로그램이 있다.

다른 예로는 P2P 파일 공유 시스템에는 파일 공유 커뮤니티에 참여하는 각 호스트에 있는 프로그램을 볼 수 있다.

애플리케이션 구조에 대해서 살펴볼 것인데, 명심해야할 점은 네트워크 구조와는 다르다는 것을 기억해야 한다.

```
>> 네트워크 구조
여기서 말하는 네트워크 구조는 컴퓨터 네트워크 토폴로지를 의미한다기 보다는 ISP(Internet Service Provider)와 IXP의 연결 구조를 의미하는 것으로 생각하면 될 것 같다.
```

애플리케이션 개발자 관점에서 네트워크 구조는 고정되어 있고 애플리케이션에 특정 서비스 집합을 제공한다.

`애플리케이션 구조(application architecture)`는 애플리케이션 개발자에 의해 설계되고 애플리케이션이 다양한 종단 시스템에서 어떻게 조직되어야 하는지를 지시한다.

애플리케이션 구조를 선택함에 있어서 두 가지 우수한 구조가 사용된다.

1. 클라이언트-서버 구조(client-server architecture)
2. P2P 구조

간단하게 보자면, `클라이언트 서버 구조`는 항상 켜져 있는 호스트를 `서버(Server)`라고 부르고, 이 서비스는 `클라이언트(Client)`라는 다른 많은 호스트 요청을 받는 구조를 말한다.

특징으로는 두 가지를 볼 수 있다.

1. 클라이언트-서버 구조에서 클라이언트는 서로 직접적으로 통신하지 않는다.

- 이 말은 즉, 각각의 클라이언트(호스트)가 서버와 통신을 하고 클라이언트와 다른 클라이언트 간에 통신을 직접하지 않는다는 것을 의미한다. (아마도 여기서 클라이언트 간에 통신을 하려면 서버를 거쳐서 하는 것을 의미할 것이다.)

2. 서버가 `고정 IP 주소`라는 잘 알려진 주소를 갖는다.

이러한 특징으로 인해, 무수히 많은 요청에 대비해서 많은 수의 호스트를 갖춘 `데이터 센터(data center)`가 강력한 가상의 서버를 생성하는 데 흔히 사용된다.

`P2P 구조`에서는 항상 켜져 있는 기반구조 서버에 최소로 의존한다. (혹은 전혀 의존하지 않는다.)

대신, `피어(peer)`라는 간헐적으로 연결된 호스트 쌍이 서로 `직접` 통신하도록 한다. 피어는 서비스 제공자가 소유하지 않고, `소비자들이 제어`하는 데스크탑과 랩톱이다.

이 구조의 가장 주목할 만한 특징은 `자가 확장성(self_scalability)`이 있다는 것이다.

예를 들어, P2P 파일 공유 애플리케이션에서 비록 각 피어들이 파일을 요구함으로써 작업 부화를 만들어 내지만 각 피어들은 또한 파일을 다른 피어들에게 `분배`함으로써 그 시스템에 서비스 능력을 추가한다. P2P 구조는 또한 비용 효율적인데 이들은 일반적으로 상당한 서버 기반 구조와 서버 대역폭을 요구하지 않기 때문이다.

그러나 미래 P2P 애플리케이션들은 이들의 고도의 분산 구조 특성으로 인해 보안, 성능, 그리고 신뢰성에 커다란 도전을 맞고 있다.

```
? P2P 방식이 로드밸런싱에 적용될 수 있나? ?

갑자기 든 의문점이었다. 로드밸런싱은 서버의 과부화를 예방하기 위해서 서버가 처리해야할 요청을 여러 대의 서버로 나누어 처리하는 것을 의미한다.

P2P의 경우 클라이언트와 서버의 구분은 존재하지만, 고정적이지 않다. 그렇다면 이 특징을 이용할 수 있지 않을까라는 생각이 들었다.

이 방식이 아직까지 사용되고 있는지는 정확히 모르겠으나, 아직 정형화된 방식이 아니라면 그것은 아마도 보안과 신뢰성의 문제 때문에 채택되지 못한 방식이라고 생각한다.
```

## 프로세스 간의 통신

네트워크 애플리케이션이 무엇인지 살펴 보았다.
이러한 네트워크 애플리케이션을 개발하기 전에 여러 종단 시스템에서 실행하는 프로그램이 서로 통신하는 법을 이해해야 한다.
운영체제 용어에서 실제 통신하는 것은 프로그램이 아닌 `프로세스(process)`이다.

```
! 프로세스(process)는 종단 시스템에서 실행되는 프로그램이다.
```

2개의 다른 종단 시스템에서 프로세스는 컴퓨터 네트워크를 통한 `메세지(message)` 교환으로 서로 통신한다.

## 클라이언트와 서버 프로세스

네트워크 애플리케이션은 네트워크에서 서로 메세지를 보내는 두 프로세스로 구성된다.

예를 들어, `웹 애플리케이션`에서 클라이언트 브라우저 프로세스는 웹 서버 프로세스와 메세지를 교환한다.

`P2P` 파일 공유 시스템에서는 한 피어의 프로세스에서 다른 피어의 프로세스로 파일을 전송한다.

이때, 파일을 내려 받는 피어를 `클라이언트`, 파일을 올리는 피어를 `서버`라고 한다.

이러한 특징으로 클라이언트와 서비스를 다음과 같이 정의할 수 있다.

```
* 클라이언트: 두 프로세스 간의 통신 세션에서 통신을 초기화 하는 프로세스 (다른 프로세스와 세션을 시작하려고 접속을 초기화)
* 서버: 세션을 시작하기 위해 접속을 기다리는 프로세스
```

## 프로세스와 컴퓨터 네트워크 사이의 인터페이스

프로세스는 `소켓(socket)`을 통해 네트워크로 메세지를 주고 받는다.

```
! 소켓은 네트워크 애플리케이션이 인터넷에서 만든 프로그래밍 인터페이스로, 애플리케이션과 네트워크 사이의 API(Application Programming Interface)라고도 합니다.
```

- 프로세스: 집(house)
- 소켓: 출입구(door)

애플리케이션 개발자는 소켓의 애플리케이션 계층에 대한 모든 통제권을 갖지만 소켓의 트랜스포트 계층에 대한 통제권은 거의 갖지 못한다.

Transport Layer에 대한 애플리케이션 개발자의 통제는 두 가지이다.

1. 트랜스포트 프로토콜의 선택
2. 최대 버퍼와 최대 세그먼트 크기와 같은 약간의 트랜스포트 계층 매개변수의 설정

```
>> 한 서버에서 열 수 있는 소켓의 수

호스트끼리 통신하기 위해 소켓과 소켓을 연결함으로써 네트워크 커넥션을 맺는다. 그렇다면 소켓 하나에 하나의 소켓을 연결하는 것일까? 그렇다면 서버에서는 몇 개의 연결이 가능할까?
```

## 프로세스 주소 배정

우리가 특정 목적지로 우편 메일을 보내기 위해서는 목적지의 주소를 갖고 있어야 한다. 마찬가지로, 한 호스트의 프로세스가 패킷을 다른 호스트의 프로세스로 보내기 위해서는 수신 프로세스의 주소를 갖고 있어야 한다.

수신 프로세스를 식별하기 위해, 두 가지 정보가 명시될 필요가 있다.

1. `호스트의 주소`
2. 그 목적지 호스트 내의 `수신 프로세스를 명시하는 식별자`

호스트의 식별은 `IP 주소`로 이루어진다.

송신 호스트는 수신 호스트에서 수행되고 있는 수신 프로세스도 식별해야 하는데 한 호스트가 많은 네트워크 응용을 수행할 수 있기 때문이다.

이 말은 즉, 한 호스트에서 통신하는 수가 한 개가 아니라 여러 개가 필요하다는 의미이다.

이것을 위해 목적지 `포트 번호(port number)`가 사용된다.

인기 있는 응용은 `특정 포트 번호를 할당`한다.

예를 들어, 웹 서버 포트 번호는 80번, 메일 서버 포트 번호는 25번으로 식별된다.

이러한 포트 번호를 `잘 알려진 포트 번호(well-known port number)`이라고 한다.
