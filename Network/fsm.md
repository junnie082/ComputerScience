# FSM과 State 패턴

## [FSM과 State 패턴](https://ozt88.tistory.com/8)

#### 2024.05.26.(일)

## FSM

- FSM은 Finite State Machine을 의미한다.

## Finite State Machine

- 유한 상태 기계란?

  - 유한 상태 기계는 자신이 취할 수 있는 유한한 갯수의 상태들을 가진다.
  - 그리고 그 중에서 반드시 하나의 상태만 취한다.
  - 현재 상태는 특정 조건이 되면 다른 상태로 변할 수 있다.
  - 유한 상태 기계는 가능한 상태들의 집합과 각 상태들의 전이 조건으로 정의될 수 있다.
  - 상태들의 노드와 그 노드들을 연결하는 조건의 엣지로 표현할 수 있다 (그래프).

- 전구의 예

  - 전구는 ON/OFF 두 가지 상태를 갖는다.
  - 전구는 반드시 둘 중 하나의 상태만 취한다.
  - 각 상태는 특정 조건(스위치 올림/내림)에 따라 변한다.
  - 전구는 다음과 같이 정의할 수 있다.
    - ON: 스위치가 올라가면 OFF로 전이
    - OFF: 스위치가 내려가면 ON으로 전이

- 유한 상태 기계를 왜 쓰는가?
  - 가능한 상태들을 명확히 규정할 수 있다.
  - 상태 중복을 피할 수 있다.
  - 전이들을 명확하게 규정할 수 있다.
  - 따라서 기계의 동작을 분명하게 규정할 수 있다.
  - 프로그래밍에서 FSM에 기반한 객체를 만든다면, 안정적인 작동을 보장할 수 있다.

## FSM 구현

- 기본적으로 IF, SWITCH 등의 조건 분기로 나누어 작동하게 할 수 있다.

```C
typedef enum{ON, OFF} BurbState;
typedef enum{ON, OFF} SwitchState;

class Burb
{
public:
    void Transition(SwitchState switchState)
    {
        switch(m_State)
        {
            case ON:
                if (switchState == OFF)
                    m_State = OFF;
                break;
            case OFF:
                if (switchState == ON)
                    m_State = ON;
                break;
        }
    }
private:
    BurbState m_State;
}
```
