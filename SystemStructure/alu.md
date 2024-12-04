# ALU

#### 2024.12.03.(화)

### ALU (Arithmetic and Logic Unit)

`산술 논리 연산 장치(ALU, Arithmetic and Logic Unit)`은 CPU 내의 실질적인 연산을 담당한다. 산술연산과 논리연산을 모두 ALU가 담당하는데, 연산을 할 때에는 ALU가 위의 레지스터를 참조하여 연산한다. ALU의 연산은 다른 언어와 연산에서 발생하는 예외처리에서 차이를 보인다. 예를 들어, 다른 언어에서는 `오버플로우(Overflow)`에 대해서는 별다른 조치를 취하지 않고 변수가 담고 있는 값의 비정상적인 변경으로 파악해야 하는 반면, ALU는 OF(Overflow Flag)를 켜서 오버플로우가 발생했다는 사실을 알린다. 이러한 플래그로는 ZF(Zero Flag), OF(Overflow Flag), CF(Carry Flag), SF(Sign Flag) 등이 있다.

## [[컴퓨터구조] ALU](https://velog.io/@shdbtjd8/%EC%BB%B4%ED%93%A8%ED%84%B0%EA%B5%AC%EC%A1%B0-ALU)

### 들어가며

ALU는 "Arithmetic Logic Unit"의 약자로, 컴퓨터의 중요한 구성 요소 중 하나이다.
ALU는 CPU(Central Processing Unit)의 핵심 부분 중 하나로서 산술 및 논리 연산을 수행하는 장치이다. 주요 역할은 다음과 같다:

- 산술 연산: ALU는 덧셈, 뺄셈, 곱셈, 나눗셈과 같은 산술 연산을 처리한다. 이러한 연산은 CPU가 숫자를 계산하고 수학적 연산을 수행하는 데 필요하다.

- 논리 연산: ALU는 논리 연산자(AND, OR, NOT, XOR 등)를 사용하여 비트수준의 논리 연산을 수행한다. 이는 조건을 평가하고 논리 게이트를 사용하여 프로그램의 제어 흐름을 조작하는 데 사용된다.

`ALU` 산술 연산, 논리 연산을 도와주는 연산기이다. 이전까지 알아본 1 bit full adder와 다른 서킷들을 합쳐서 구성되어 있으며 점차적으로 진화된 형태를 띤다.

### 2진 연산 방식

우리가 10진수 연산을 할 때 연산을 어떻게 할까? 위 그림을 보면은 첫째 자리수 7과 4를 더해서 11이 나오지만 한 자리로는 표현할 수 없어서 1을 다음자리로 넘기고 있다. 이렇게 다음 자리로 올라가는 수를 우리는 올림수라 하고 영어로는 `Carry out`이라고 한다. 둘째 자리수 입장에서는 2와 5를 더하면 되는데 올림수가 올라왔고 올라온 수를 `Carry in`이라고 한다. 똑같은 올림수인데 어느 시점에서 바라보냐에 따라서 용어가 다르다.

2진 연산도 마찬가지이다. 하나의자리에서 표현할 수 있는 수는 0과 1이다. 그래서

> 1 + 1 = 2

는 2진수에서는 표현하지 못하는 방법이고 1을 다음 자리로 올려줘야 한다. 마치 10지수에서 한 자리로 10을 표현하지 못해서 1을 다음 자리로 올리듯이 말이다.

### Overflow

덧셈기와 뺄셈기에서 오버플로우는 언제 발생하는지 알아보자.
먼저 `Overflow`란 현재 가지고 있는 비트로는 연산 결과를 표현할 수 없을 때 발생하는 현상이다.
예를 들어 Signed 3비트 체계에서는 표현할 수 있는 수가 `-4 ~ 3` 까지이다. 그런데 2 + 2를 하면 어떨까? 4는 표현할 수 없는 수이고 2비트로 나타내면 100이다. 하지만 Signed 3비트 체계에서 100은 -4이다. 즉, 4는 표현할 수 없는 수이고이런 경우에 연산 결과로 오버플로우가 발생했다고 한다.

덧셈기에서 오버플로우는 `서로 같은 부호를 더했을 때` 발생할 수 있다.
뺄셈기에서 오버플로우는 `서로 다른 부호를 뺄 때` 발생할 수 있다.

우리는 여기서 조금 더 Overflow에 대한 일반화된 규칙을 만들어낼 수 있다.

### Rule of thumb

| Operation | Operand A | Operand B | Result including overflow |
| A + B | >= 0 | >= 0 | < 0 |
| A + B | < 0 | < 0 | >= 0 |
| A - B | >= 0 | < 0 | < 0 |
| A - B | < 0 | >= 0 | >= 0 |

N 비트 덧셈 혹은 뺄셈을 할 때 `마지막 ALU의 Carry in과 Carry out이 다르다면` 우리는 그때 Overflow가 발생했다고 볼 수 있다.

> 요약. 마지막 ALU의 Carry-in 과 Carry-out이 다르다면 Overflow가 발생

### ALU

ALU는 단순히 덧셈, 뺄셈처럼 산술연산만 가능한 게 아니라 `논리 연산`도 가능하다. AND, OR, NOT, NAND, NOR처럼 말이다. 어떻게 ALU가 이렇게 다양한 연산을 할 수 있을 지를 점차적으로 알아보자. 핵심 키워드는 `MUX`이다.

먼저 AND, OR 연산을 하는 서킷이다. a랑 b랑 AND 연산의 결과를 MUX의 0과 매핑을 하고 OR 연산을 할 때는 1과 매핑을 한다. 그럴 시에 MUX에 0을 인가하면은 AND 연산을, 1을 인가하면은 OR 연산을 수행한다.

## [컴퓨터 구조 - 4.4. 데이터패스의 단순한 구현](https://vansoft1215.tistory.com/3)

### 1. ALU 제어

1. ALU 제어 신호의 종류 6가지

- MIPS는 제어입력 4개를 사용하는 다음 6개 조합을 정의하고 있다.

| ALU control lines | Function |
| 0000 | AND |
| 0001 | OR |
| 0010 | add |
| 0110 | sub |
| 0111 | slt |
| 1100 | NOR |

- ALU는 명령어 종류에 따라 5가지(초록색) 기능 중 하나를 수행하게 됨.

2. 명령어별 ALU 제어 신호

- ALUop 제어 (2bit) + 기능 코드 (6bit) = ALU 제어 입력 (4bit)

| Instruction OPcode (지시어 opcode) | ALU OP (명령어 제어 필드) | Instruction Operation (연산자 종류) | Funct field (기능 필드) | Desired ALU action (필요한 ALU 연산) | ALU control input (들어오는 ALU 제어신호) |
| lw | 00 | lw | xxxxxx | add | 0010 |
| sw | 00 | sw | xxxxxx | add | 0010 |
| beq | 01 | beq | xxxxxx | sub | 0110 |

---

| R-type | 10 | add | 100000 | add | 0010 |
| R-type | 10 | sub | 100010 | sub | 0110 |
| R-type | 10 | AND | 100100 | AND | 0000 |
| R-type | 10 | OR | 100101 | OR | 0001 |
| R-type | 10 | slt | 101010 | slt | 0111 |

3. 4bit ALU 제어신호를 위한 진리표

- 진리표: 입력이 가질 수 있는 모든 값을 나열하고, 각 경우의 출력값을 보이는 논리 연산 표현방법

* 입력된 명령어에 따른, 두 가지 입력 필드에 따라(ALUop, Func field), 4bit ALU 제어 값이 어떻게 실행되는지를 보여준다.

| ALUop(제어필드) | Funct Field | Operation(발생하는 ALU 제어신호) | 비고 |
| 00 | xxxxxx | 0010 | 둘다 0이면 sw. lw|
| x1 | xxxxxx | 0110 | op값 1인건, beq 밖에 없음
| 1x | xx0000 | 0010 | add |
| 1x | xx0010 | 0110 | sub |
| 1x | xx0100 | 0000 | AND |
| 1x | xx0101 | 0001 | OR |
| 1x | xx1010 | 0111 | slt |

- 이 진리표가 만들어지면, 최적화후 게이트로 변환작업을 기계적으로 수행한다.

2. 주 제어 유닛의 설계

- 명령어 필드를 데이터패스에 연결하는 방법을 이해하기.

a. R-Type Instruction

b. Load or Store Instruction

2. 7개 제어신호의 기능(=제어선의 기능)

| 신호 이름 | 인가되지 않은 경우(0) | 인가된 경우(1) |
| RegDst | 명령어 rt 필드(20:16)가 Write reg로 입력 | 명령어 rd 필드(15:11)가 write reg로 입력 |
| Regwrite | 아무일도 발생하지 X | write register 입력이 지정하는 레지스터에 write Data 입력 값을 쓴다. |
| ALUSrc | 레지스터 파일의 두 번째 출력(Read Data2) 가 ALU의 두 번째 피연산자가 된다. | 명령어의 하위 16bit가 부호확장되어 ALU의 두 번째 피연산자가 된다. |
| PCSrc | PC+4가 새로운 PC 값이 된다. | 분기 목적지 주소가 새로운 PC가 된다. |
| MemRead | 아무일도 발생하지 X | Address 입력이 지정하는 데이터 메모리 내용을 Read Data 출력으로 내보낸다. |
| MemWrite | 아무일도 발생하지 X | Address 입력이 지정하는 데이터의 메모리 내용을 Write Data 입력값으로 바꾼다. |
| MemtoReg | ALU 출력이 레지스터의 write data 입력이 된다. | 데이터 메모리 출력이 레지스터의 Write Data 입력이 된다. |

3. 제어유닛 (Control Unit)

- 제어신호(Control)은 제어신호 중 하나를 제외한(PCSrc 제외: 제어유닛으로부터 직접 나오는 값이 아니고, 만들어야 하는 신호임), 나머지 모두를 명령어의 OPCode 필드만 보고 결정할 수 있다.

1. 제어 유닛의 입력
   : 명령어의 6bit OPCode 필드

2. 제어 유닛의 출력
   : 멀티플렉서를 제어하는데 쓰이는 3개의 1bit 신호 (RegDst, ALUSrc, MemtoReg)
   : 레지스터 파일과 데이터 메모리에서 읽고 쓰는 것을 제어하기 위한 3개의 신호 (RegWrite, MemRead, MemWrite)
   : 분기할지 말지를 판단하는 데에 쓰이는 1bit 신호(Branch)
   : ALU를 위한 2bit 제어신호 (ALUop)

3. AND 게이트
   : 분기 제어신호와 ALU의 zero 출력을 결합하는 데 사용되는 AND 게이트 출력은, PC 다음 값을 선택하는 데 쓰인다.

4. 데이터페이스의 동작 (명령어에 따른 데이터패스와 제어신호)

1) R형식 명령어의 실행과정

1. 명령어를 명령어 메모리에서 가져오고, PC 값을 증가시킨다.
2. 두 레지스터(Read register 1,2)를 레지스터 파일로부터 읽는다. 이 단계에서 주 제어유닛이 제어선들의 값들을 계산한다.
3. ALU는 읽어들인 값들에 대해 연산을 하는데, 기능코드(명령어의 funct 필드인 비트 5:0)를 사용하여 ALU 제어신호를 만든다.
4. ALU 결과값이 레지스터 파일의 Write reg에 기록된다.

- 목적지 레지스터는 명령어의 비트 15:11을 이용해 선택한다.

- R형식 명령어의 제어신호

| RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | Memwrite | Branch | ALUop |
| 1 | 0 | 0 | 1 | 0 | 0 | 0 | 10 |

2. 적재(lw) 명령어의 실행과정

1) 명령어를 명령어 메모리에서 가져오고 PC 값을 증가시킨다.
2) 레지스터(Read Register)의 값을 레지스터 파일로부터 읽어 값을 계산한다.
3) ALU는 레지스터 파일에서 읽어들인 Read Register의 주소값과, 명령어의 하위 16bit(OFFSET) 부호확장한 값과의, 합을 구한다.
4) ALU 계산결과(합)을 메모리 접근을 위한 주소로 사용해 메모리를 읽는다.
5) 메모리 유닛에서 가져온 데이터를 레지스터 파일의 Write Register[20:16]에 기록한다.

- 적재(lw) 명령어의 제어신호

| RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | Memwrite | Branch | ALUop |
| 0 | 1 | 1 | 1 | 1 | 0 | 0 | 00 |

3. 저장(sw) 명령어의 실행과정

1) 명령어를 명령어 메모리에서 가져오고 PC 값을 증가시킨다.
2) 레지스터(Read Register)의 값을 레지스터 파일로부터 읽어 값을 계산한다.
3) ALU는 레지스터 파일에서 읽어들인 Read Register의 주소값과, 명령어의 하위 16bit(OFFSET) 부호확장한 값과의, 합ㅇ르 구한다.
4) ALU 계산 결과(합)을 메모리 접근을 위한 주소로 사용해, 데이터 메모리에 쓰기.

- 저장(sw) 명령어의 제어신호

| RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | Memwrite | Branch | ALUop |
| X | 1 | X | 0 | 0 | 1 | 0 | 00 |

4. 분기(beq)명령어의 실행과정

1) 명령어를 명령어 메모리에서 가져오고 PC값을 증가시킨다.
2) 두 레지스터(Read Register 1,2)의 값을 레지스터 파일로부터 읽고, 해당 값을 계산한다.
3) ALU는 레지스터 파일에서 읽어들인 값들에 대해 뺄셈을 한다. 그리고 명령어의 하위 16bit(OFFSET)를 부호확장한 후 2bit 자리이동한 값에다, PC+4를 더한다. 결과 값이 분기 목적지 주소이다.
4) 어떤 덧셈기의 값을 PC에 저장할지, ALU의 zero 출력을 이용하여 판단한다.

- 분기(beq) 명령어의 제어신호

| RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | Memwrite | Branch | ALUop |
| X | O | X | O | O | O | 1 | O1 |

5. 점프(jump)명령어의 실행과정

- jump 는 직접, 워드 주소를 사용한다.
- 즉, 새로운 멀티플렉서와 제어신호가 필요하다.

- 점프(jump) 명령어의 제어신호

| RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | Memwrite | Branch | ALUop | jump |
| X | X | X | O | O | O | X | XX | 1 |

4. 제어 유닛의 완성

1) 각 명령어의 제어 신호 종합

- opcode의 이진수 인코딩을 이용해 각 출력의 진리표를 만듦.

5. 단일 사이클 구현은 오늘날 왜 사용되지 않는가?

- 단일 사이클 설계가 올바르게 작동된다 하더라도, 비효율성 때문에 현대에서 잘 사용되지 않는다.

* 클럭 사이클이 모든 명령어에 대해 같은 길이를 가져야 함.
* 클럭 사이클은 가장 긴 경로로 결정 (lw-5개 유닛을 차례로 사용)
* CPI 값은 1이지만, 단일 사이클구현은 클럭사이클이 너무 길기 때문에 전체 성능이 좋지 않음.

- 이후, `파이프라이닝`을 통해, 처리율(효율성)이 높은 기술을 구현하게 된다.
