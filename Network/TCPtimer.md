# TCP - Timer

## [[컴퓨터 네트워크]TCP-Timer](https://velog.io/@hsshin0602/%EC%BB%B4%ED%93%A8%ED%84%B0-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCP-Timer)

#### 2024.05.26.(일)

## TCP Timer

TCP 프로토콜에서는 회선 연결의 신뢰성을 확보하기 위해 4개의 타이머를 활용한다. retransmission timer, persistance tiner, keepalive timer, time-waited timer가 있다.

### 재전송(Retransmission) 타이머

송신측은 매 세그먼트를 전송할 때마다 재전송 타이머 가동한다. 정해진 시간(RTO, Retransmission Timeout) 내 수신 확인응답(ACK)이 안되면 재전송을 하게되고 여기서, RTO 값은 고정된 것이 아니라 조정 가능하다.

### RTT(Round Trip Time, 왕복 시간)

패킷망(인터넷) 상에서, 송신지부터 목적지 호스트까지, 패킷이 왕복하는 데 걸리는 시간을 의미한다. IP 패킷 왕복시간은 ping 명령어를 사용하여 RTT(왕복시간) 및 TLL(IP 패킷수명) 수치를 알아볼 수 있다.

### Measured RTT (RTTm)

TCP 세그먼트들이 보내면 그 세그먼트에 대한 ack 수신까지 걸리는 실제 시간이다. (측정값) 네트워크 가변성 때문에 RTT 측정값은 그때마다 달라진다. 동시에 여러 세그먼트에 대한 RTTm을 측정하지 않는다.

### Smoothed RTT (RTTs)

RTTm은 변동이 너무 커서 RTO로 사용하기 부적절하다. 이 문제를 해결하기 위해 이전 RTTs와 측정된 RTTm이 필요하다.

```
첫번째 측정시, RTTs = RTTm
RTTs = (1-α)RTTs + αRTTs
일반적으로, α=1/8 (0.125) 즉, 7/8 RTTs(기존값) + 1/8 RTTm(측정값)
```

### RTT deviation (RTTd)

RTTs를 더 보완해서 RTO를 구하기 위해 사용하는 것으로 편차를 의미한다. 타임아웃 되어서 실제 재전송 발생시에, RTO 계산 방법이다.

```
첫번째 측정시 RTTd = RTTm/2
RTTd = (1-b)RTTd + b|RTTs - RTTm|
일반적으로 b=1/4 (0.25) 즉, 3/4 기존값 + 1/4 (new 평균값 - new 측정값)
```

### Retransmission Timeout (RTO)

재전송 타이머로 재전송까지 걸리는 시간을 의미한다. RTO의 초깃값으로는 system에서 default값을 할당한다.

```
RTO = RTTs + 4RTTd
```

### Karn's algorithm & Exponential Backoff

**_Karn의 알고리즘_**은 RTO안에 ACK가 도착하지 않으면 해당 세그먼트에 대한 재전송이 발생하는데 이때 재전송된 세그먼트를 RTT 측정 대상에서 제외하는 것을 말한다. 왕복 시간 추정은 새로 전송하는 데이터 세그먼트만을 RTT 측정 대상으로 삼는 것을 의미한다.

**_지수 백오프_**는 세그먼트 재전송이 발생했을 때, 기존에 계산된 RTO를 혼잡제어를 위해 점차 두 배씩 늘려가는 방식을 의미한다.

### persistence Timer

영속 타이머(persistence timer)는 윈도우 크기 결정을 위한 타이머이다. 앞에서 설명했다 싶이 Receiver가 rwnd = 0 을 보내 **_Sender가 데이터 송신을 멈춘 상황에 persistence timer가 작동_**한다. persistence timer가 time out이 되었는데도 상대방에게서 아무런 반응이 없으면, Sender는 **_1 바이트 길이의 데이터 (probe segment)_**를 상대방에게 보내본다.

데이터를 보내면 해당 데이터에 대한 Ack이 도착한다. rwnd가 여전히 0이면 상대방 수신버퍼가 아직 비워지지 않았다는 의미이고 rwnd = k이면 중간에 Receiver가 보낸 Ack이 유실되었을 가능성이 존재한다.

### Keepalive Timer

연결 유지를 위한 타이머로 이미 설정된 연결이 오랫동안 휴지 상태에 있지 않도록 하기 위해 사용된다.

### Time-waited Timer

TCP 연결 종료 후에 이 기간 동안만 연결을 유지하기 위해 구동되는 타이머를 의미한다. 이전 연결 종료 전의 어떤 패킷이 늦게 중복 지연 도착하게 되는 것을 방지한다.
