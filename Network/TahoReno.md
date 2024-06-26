# TCP - Tahoe TCP / Reno TCP

## [TCP - Tahoe TCP / Reno TCP](https://velog.io/@nsh7293/TCP-Taho-TCP-Reno-TCP)

#### 2024.05.26.(일)

Tahoe TCP와 Reno TCP에 대해서 알아보자.

일단 이 두 TCP 모두 TCP의 네트워크 혼잡 제어와 관련해서 제안된 모델이다.
우선, 혼잡 제어와 관련해서 혼잡 정책에 어떠한 것들이 있는지 알아볼 필요가 있다.

혼잡 제어의 방안으로는

- 혼잡 윈도
- 혼잡 감지
- 혼잡 정책

### Tahoe TCP

Tahoe TCP로 알려져 있는 이른 TCP는 두 가지 알고리즘(느린시작, 혼잡회피)을 사용한다.

Tahoe TCP는 혼잡 감지, 타임아웃, 그리고 3개의 중복된 ACK을 발견하기 위해 2개의 신호를 이용한다. 이 버전에서 연결이 설립될 때, TCP는 느린 출발 알고리즘을 시작하고 ssthresh(slow-start thresh)를 의미한다.
변수를 pre-agreed value(사전-동의값, MSS(최대 세그먼트 크기)의 일반적 배수)로 설정하고 1개의 MSS에 cwnd(혼잡 윈도우를 의미)를 둔다.

이 상태에서 전에 말했듯이 ACK가 도착할 때마다 혼잡 윈도 크기는 1씩 증가한다. 이 정책이 매우 적극적이고 전형으로 윈도 크기를 증가시킨다는 것을 알고 있으며, 이것이 혼잡을 초래할지도 모른다.

만약 혼잡이 감지된다면(타임아웃의 발생이나 3개 중복된 ACK가 도착) TCP는 즉시 이 공격적인 증가를 가로막고 재시작하여 새로운 슬롯은 제한된 임계치에 의해 현재 cwnd를 중단하기 위해 알고리즘을 시작하고 현재 혼잡 윈도를 1로 재설정한다. 다시 말하면 TCP는 스크래치(scratch)에서만 재시작되는 것이 아니라 임계치를 어떻게 조절할 것인지 또한 배우게 된다.

만약 임계치까지 혼잡이 감지되지 않으면, TCP는 자신이 원하는 선에 도착함을 알게 된다. 이것은 이 속도로 이어지지 않아야 한다. 혼잡 회피 상태로 움직이고 이 상태는 계속된다. 혼잡 회피 상태에서 혼잡 윈도 크기는 ACK의 크기가 현재 윈도 크기와 같아질 때마다 1씩 증가되어 회부된다. 예를 들어 만약 윈도 크기가 지금 5MSS이면, 윈도 크기가 6MSS가 되기 전에 5개의 ACK를 더 수신해야 한다.

이 상태에서 혼잡 윈도 크기의 상한이 없을 때 혼잡이 탐지되지 않는 한 혼잡 윈도의 가산증가는 데이터 전송 단계의 끝에서 계속된다.

만약 이 상태에서 혼잡이 탐지되면, TCP는 다시 ssthresh의 값을 현재 cwnd 값의 반으로 재설정하고, 다시 느린 출발 상태로 전환한다.

비록, 이 버전의 TCP에서 ssgthresh의 크기가 지속적으로 혼잡 탐지에서 조정될지라도 이것은 전 값보다 더 낮게 설정됨을 의미하지 않는다. 예를 들어, 만약 원래 ssthresh 값이 8MSS이고, TCP가 혼잡 회피 상태이고 cwnd의 값이 20일 때 혼잡이 탐지되면, ssthresh의 새로운 값은 10으로 증가되었음을 알 수 있다.

![14](/assets/images/2024-05-26/14.png)

상단의 자료는 Tahoe TCP의 혼잡 제어를 나타낸 것이다.
TCP는 데이터 전송을 시작하고, ssthresh 변수를 16MSS로 정한다. TCP는 cwnd = 1에서 느린 출발(SS, Slow-Start)을 시작한다. 혼잡 윈도는 기하급수적으로 증가하지만 타임아웃은 세 번째 RTT(임계치에 도달하기 전에) 후에 발생한다. TCP는 네트워크에 혼잡이 있다고 가정한다. 즉시 새로운 ssthresh를 4MSS(현재 8로 설정된 cwnd의 반)로 설정한 다음 cwnd = 1로 설정된 새로운 출발을 시작한다. 혼잡은 기하급수적으로 새로 설정된 임계치까지 증가하게 된다. TCP는 이제 혼잡 회피(CA) 상태로 변하고, 혼잡 윈도는 추가적으로 cwnd = 12MSS에 도달할 때까지 증가한다.
그 순간 3개의 중복된 ACK가 도착하고, 또 다른 혼잡이 암시된다. TCP는 ssthresh 값을 반인 6MSS로 줄이고, 느린 시작 상태가 된다.

cwnd의 지수적 증가는 계속된다. RTT 15인 후에 cwnd 크기는 4이다. 4개의 세그먼트를 보내고 오직 두 ACK를 받은 후에 윈도 크기가 ssthresh(6)에 도달한 TCP는 혼잡 회피 상태에 들어간다. 연결이 RTT 20후 끊어질 때까지 데이터 전송은 현재 혼잡 회피(CA) 상태에서 계속된다.

### Reno TCP

TCP의 새로운 버전인 Reno TCP는 혼잡 제어 FSM에 빠른 회복이라는 상태를 추가한 것이다. 이 버전은 혼잡한 2개 신호인 타임아웃과 각각 다르게 도착하는 3개의 중복된 ACK를 다룬다. 이 버전에서 만약 타임아웃이 일어나면, TCP는 느린 시작 상태(이미 이 상태에 있다면 새로운 라운드를 시작한다.)로 들어간다.

또 한편으로는 만약 3개의 중복된 ACK가 도착하면, TCP는 빠른 회복 상태로 전환하고, 더 중복된 ACK들이 도착하는 한 같은 상태를 유지한다. 빠른 회복 상태는 느린 시작 상태와 혼잡 회피 상태의 중간 형태이다.
이것은 cwnd가 지수적으로 증가하는 느린 출발처럼 행동한다. 그러나 cwnd는 ssthresh에 3MSS(1 대신에)을 더한 값에서 시작한다.

TCP가 빠른 회복 상태에 돌입할 때, 세 가지의 주요 이벤트가 발생할 것이다. 만약 중복 ACK가 계속 도착하게 되면, TCP는 이 상태에서 머무르지만 cwnd는 지수적으로 증가하게 된다. 만약 타임아웃이 발생하면, TCP는 네트워크에 실재적인 혼잡이 있다고 가정하고, 느린 시작 상태로 돌입한다.

만약 새로운 ACK(중복되지 않은)가 도착하면, TCP는 혼잡 회피 상태로 돌입하고, 마치 3개의 중복 ACK들이 나타나지 않았던 것처럼 ssthresh 값으로 cwnd의 크기를 줄여 느린 시작 상태에서 혼잡 회피 상태로 전환한다.

![15](/assets/images/2024-05-26/15.png)

이 자료는 Reno TCP의 혼잡제어 과정을 보여준다.

6MSS(Taho TCP로서 동일한 것)로 감소하지만, 대신 cwnd를 1MSS보다 높은 값(ssthresh + 3 = 9MSS)으로 설정한다. Reno TCP는 빠른 회복 상태로 전환한다. 추가적으로 2개의 중복 ACK가 RTT 15까지 도착하고 cwnd가 지수적으로 증가한다고 가정하고 있다.
그 순간 잃어버린 새로운 세그먼트를 알리기 위해 새로운 ACK(중복되지 않은)가 도착한다.
Reno TCP는 혼잡 회피 상태로 전환하게 된다. 하지만 첫 번째로 빠른 회복 상태를 무시하고 혼잡 윈도를 이전 트랙 뒤로 움직이는 것과 같이 6MSS로 줄인다.

### NewReno TCP

TCP의 가장 최신 버전인 NewReno TCP는 Reno TCP에 최적화를 추가한 것이다.
이 버전에서는 TCP에서 3개의 중복 ACK가 도착할 때, 현재 1개 이상의 세그먼트를 잃었는지에 대해 조사한다.
만약 새로운 ACK가 윈도의 끝을 정의한 후 혼잡을 감지한다면, 오직 하나의 세그먼트만 잃었다고 확신한다.
그러나 만약 ACK 수가 재전송 세그먼트와 윈도의 끝 사이에 정의된다면, ACK로 정의된 세그먼트 또한 잃었을 가능성이 있다.
NewReno TCP는 더 많은 중복된 ACK의 수신을 회피하기 위해 이 세그먼트를 재전송한다.
