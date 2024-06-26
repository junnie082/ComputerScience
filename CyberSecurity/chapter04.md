# Cyber Security 4

# 제 4 장. 대칭 암호

#### 2024.04.14.(일)

## 제 1 절 문자 암호에서 비트열 암호로

### 1.1 부호화

- 암호화에 컴퓨터 사용이 필수

- 암호화 프로그램도 평문을 비트열로 변경하고 비트열로 된 암호문을 출력

- 부호화(encoding)
  - 문자열을 비트열로 바꾸는 것

### ASCII

- 문자열 midnight 을 다음과 같은 비트열로 부호화

m -> 01101101
i -> 01101001
d -> 01100100
n -> 01101110
i -> 01101001
g -> 01100111
h -> 01101000
t -> 01110100

### 1.2 XOR

- XOR은 배타적 논리합이다.
  - 0 XOR 0 = 0
  - 0 XOR 1 = 1
  - 1 XOR 0 = 1
  - 1 XOR 1 = 0

### 한 비트의 XOR

- XOR은 ⊕ 라는 기호를 써서 표현

- 같은 숫자끼리의 XOR은 반드시 0이 된다.

- A ⊕ B ⊕ B = A (A로 돌아간다)

### 암호화/복호화의 순서와 매우 비슷

- 평문 A를 키 B로 암호화하고, 암호문 A ⊕ B를 얻는다.
- 암호문 A ⊕ B를, 키 B로 복호화해서 평문 A를 얻는다.

### XOR은 그림을 마스크한다

![15](/assets/images/2024-04-14/15.png)

## 제 2 절 일회용 패드-절대 해독 불가능한 암호

### 2.1 일회용 패드란?

- 1회용 패드(one-time pad)
  - 전사공격에서 키공간을 모두 탐색하더라도 해독할 수 없는 암호

### 2.2 일회용 패드의 암호화

![16](/assets/images/2024-04-14/16.png)

![17](/assets/images/2024-04-14/17.png)

### 2.4 일회용 패드는 해독할 수 없다

- 현실적인 시간 내에 해독이 곤란하다는 의미만은 아니다.
- 키 공간 전체를 순식간에 계산할 수 있는 무한대의 계산력을 갖는 컴퓨터로도 일회용 패드는 해독할 수 없다.
- 문자열이 복호화 되었다 하더라도, 그것이 바른 평문인지 아닌지 판정할 수 없다.

### 전사 공격

- 암호문을 복호화해보면 도중에 모든 64비트 패턴이 등장한다

- 그 중에 나타날 수 있는 문자열

  - 규칙적인 문자열

    - aaaaaaaa, abcdefgh, zzzzzzzzz 등

  - 의미 있는 영어 단어

    - midnight, onenight, mistress 등

  - 무의미한 문자열
    - %Ta_AjvX, HY(&JY!z, $@~\*)

- 따라서 어느 것이 바른 평문인지 알 수 없다
  - 즉 어떤 키를 사용하면 바르게 복호화할 수 있는지 알 수 없다

### 전사 공격

- 일회용 패드에서는 키들을 적용하여 얻어진 것이 바른 평문인지 아닌지를 판정하는 것이 불가능하다.

- 그러므로 일회용 패드를 해독할 수 없다.

### 2.5 일회용 패드는 왜 사용되지 않을까?

- 키 배송, 키 보존, 키 재사용, 키 동기화, 키 생성 이슈가 발생

- 일회용 패스의 사용은 키 생성과 배송에 막대한 비용과 노력이 필요

  - 비용과 상관없이 기밀성 최우선 고려하는 경우만 사용
    예) 강대국끼리의 핫라인

- 실제 스트림 암호에 활용

## 스트림 암호와 블록암호

### 블록 암호 기법의 원리

- 스트림 암호
  - 한 번에 1비트 혹은 1바이트의 디지털 데이터 스트림을 암호화하는 방식

![18](/assets/images/2024-04-14/18.png)

- 블록 암호

  - 평문 블록 전체를 가지고 같은 크기의 암호문 블록을 생성
  - 모드를 이용하여 스트림 암호 기법과 동일한 효과

- 두 개 이상의 기본 암호를 연속적으로 수행(치환, 순열 번갈아 수행)

  - 치환 (substitution)
    .평문의 각 원소 또는 원소의 그룹을 다른 원소에 사상
    .혼돈의 원리를 이용
  - 순열(permutation)
    .평문 원소의 순서는 순열의 순서대로 재배치
    .확산의 원리를 이용

- Feistel 암호 방식 (혼돈과 확산: Confusion & Diffusion)

  - Claude Shannon 소개(SHAN49): "매우 이상적인 암호는 암호문에 대한 모든 통계적 정보가 사용된 키와 독립적이어야 한다."
  - 통계적 분석에 기초한 암호 해독 방지

  * 혼돈: 키를 발견하기 어렵게 하기 위해 암호문에 대한 통계 값과 암호 키 값 사이에 관계를 가능한 복잡하게 하는 것

  * 확산: 평문의 통계적 구조가 암호문의 광범위한 통계값에 분산되도록 하여
    - 키를 추론하게 어렵게 하기 위해 평문과 암호문 사이에 통계적인 관계를 가능한 복잡하게 만드는 것

### 3.4 Feistel 암호 구조

![19](/assets/images/2024-04-14/19.png)

### Feistel 네트워크의 매개 변수와 설계 특성

- 블록 크기

  - 64비트가 일반적, 현재 가변적

- 키 크기

  - 64비트 또는 128비트

- 반복 수

  - Feistel 암호 방식은 다중 반복 과정은 보안성을 증가
  - 일반화는 16회

- 서브키 생성 알고리즘

  - 알고리즘이 복잡할수록 암호해독이 더욱 더 어려움

- 반복 함수

  - 함수가 복잡할수록 일반적으로 암호해독이 더욱 더 어려움

- 빠른 소프트웨어 암/복호화

  - 프로그램의 실행 속도가 관심사

- 분석의 용이성

### Feistel 네트워크의 특성

- 원하는 만큼 라운드 수를 늘릴 수 있음

- 라운드 함수 F에 어떤 함수를 사용해도 복호화가 가능

- 암호화와 복호화를 완전히 동일한 구조로 실현할 수 있음

## 제 3 절 DES란?

### 3.1 DES란?

- DES(Data Encryption Standard)는 1977년에 미국의 연방 정보처리 표준 규격(FIPS)으로 채택된 대칭 암호

- 전사 공격으로 해독할 수 있는 수준

- DES는 64비트의 키를 적용하여 64비트의 평문을 64비트의 암호문으로 암호화 시키는 대칭형 블록 암호이다.

- DES 알고리즘에서는 사용하는 함수

  - 대체(substitution)
  - 치환(permutation)

- 대체와 치환은 1949년도에 Claude Shanon이 제시한 혼돈(confusion)과 확산(diffusion)이라는 두 가지 개념에 기반을 두고 있다.

### 3.2 DES의 암호화, 복호화

- 64비트 평문을 64비트 암호문으로 암호화하는 대칭 암호 알고리즘

- `키의 비트 길이는 56비트`

- 64비트 평문(비트열)을 하나의 단위로 모아서 암호화

![20](/assets/images/2024-04-14/20.png)

### 3.3 DES의 구조

![21](/assets/images/2024-04-14/21.png)

![22](/assets/images/2024-04-14/22.png)

![23](/assets/images/2024-04-14/23.png)

![24](/assets/images/2024-04-14/24.png)

![25](/assets/images/2024-04-14/25.png)

![26](/assets/images/2024-04-14/26.png)

![27](/assets/images/2024-04-14/27.png)

- 함수 F(R, K)의 계산
  - 8개의 S-box로 구성
  - 각 S-box는 6비트 입력, 4비트 출력 생성

![28](/assets/images/2024-04-14/28.png)

![29](/assets/images/2024-04-14/29png)

![30](/assets/images/2024-04-14/30.png)

![31](/assets/images/2024-04-14/31.png)

- 쇄도우효과 (Avalanche Effect)
  : 평문이나 키의 작은 변화가 암호문에 대하여 중요한 변화를 일으키는 암호 알고리즘의 중요한 성질

* 예) 해시함수 SHA-1: 1비트만 바꿔도 결과 값이 완전히 달라짐

![32](/assets/images/2024-04-14/32.png)

- 예) DES 쇄도효과: 평문변화
  - 평문이나 키를 1비트 바꿀 때 암호문의 여러 비트가 변함

![33](/assets/images/2024-04-14/33.png)

### 3.5 차분 해독법과 선형 해독법: DES 공격법

- 차분 해독법 (Differential Cryptanalysis)

  - 평문의 일부를 변경하면 암호문이 어떻게 변화하는지를 조사
  - 블록암호를 보면 입력되는 평문이 1비트라도 달라지면 암호문은 전혀 다른 비트 패턴으로 변화
  - 암호문의 변화틀을 조사하여 해독의 실마리를 얻음

- 선형 해독법 (Linear Cryptanalysis)

  - 평문과 암호문 비트를 몇개 정도 XOR 해서 0이 되는 확률을 조사
  - 암호문이 충분히 랜덤하다면
    - 평문과 암호문의 비트를 몇개 XOR한 결과가 0될 확률은 1/2
  - 1/2로부터 크게 벗어난 비트의 갯수를 조사하여 키에 관한 정보를 얻음

- DES는 두 공격법을 통해 해독 가능

- AES기반 현대 블록 암호는 차분/선형 해독법에 대해 안전하도록 설계

## 제 4 절 트리플 DES

### 4.1 트리플 DES란?

- 트리플 DES(Triple-DES)
- DES는 전사공격으로 현실적인 시간 내에 해독
- DES를 대신할 블록 암호가 필요
- 이를 위해 개발된 것이 트리플 DES
- DES보다 강력하도록 DES를 3단 겹치게 한 암호 알고리즘

![34](/assets/images/2024-04-14/34.png)

### 트리플 DES는 DES로도 사용

![35](/assets/images/2024-04-14/35.png)

### 트리플 DES 종류

- DES

  - 모든 키에 같은 비트열을 사용

- DES-EDE2

  - 키1과 키3에 같은 키를 사용하고 키2에 다른 키를 사용
  - EDE는 암호화(Encryption) -> 복호화(Decryption)
    -> 암호화(Encryption) 순서

- DES-EDE3
  - 키1, 키2, 키3을 모두 다른 비트열을 사용

![36](/assets/images/2024-04-14/36.png)

### 트리플 DES 복호화

- 암호화의 역순

- 키3, 키2, 키1의 순으로 복호화 -> 암호화 -> 복호화를 행한다

![37](/assets/images/2024-04-14/37.png)

### 트리플 DES의 현황

- 처리 속도는 빠르지 않음
- 안정성면에서도 풀려 버린 사례가 있음
- 우리나라에서는 3-DES를 표준으로 정하지 않음
- 우리나라 국가표준은 SEED 및 ARIA

## 제 5 절 AES 선정 과정

### 5.1 AES란?

- AES (Advanced Encryption Standard)
  - DES를 대신한 새로운 표준 대칭 암호 알고리즘
  - AES의 후보로서 다수의 대칭 암호 알고리즘을 제안했지만, 그 중에서 Rijndael이라는 대칭 암호 알고리즘이 2000년에 AES로서 선정

### 5.2 AES 선정 과정

- NIST(National Institute of Standard and Technology)에서 공모

- 경쟁방식에 의한 표준화(standardization by competition)

- 조건
  - 제한 없이 무료로 이용
  - ANSI C와 Java에 의한 구현
  - 암호해독에 대한 강도의 평가
  - 암호 알고리즘 설계 규격과 프로그램 공개

### 5.3 AES 최종 후보 및 선정

- 1차 심사 통과: 15개

* CAST256, Crypton, DEAL, DFC, E2, Frog, HPC, LOKI97, Magenta, MARS, RC6, Rijndael, SAFER+, Serpent, Twofish

## 제 6 절 Rijndael

### 6.1 Rijndael이란?

- 벨기에 연구자 Joan Daemen과 Vincent Rijmen이 설계한 블록 암호 알고리즘

- 블록 길이

* 128 비트

- 키의 비트 길이
  - 128비트
  - 192비트
  - 256비트

### 6.2 Rijndael의 암호화와 복호화

- 복수의 라운드(round)로 구성(10~14)

- SPN(Substitution-Permutation Network) 구조 (Feistel 구조 아님)

- SubBytes

- ShiftRows

- MixColumns

- AddRoundKey

![38](/assets/images/2024-04-14/38.png)

![39](/assets/images/2024-04-14/39.png)

### SubByte(바이트 대체)

- SubByte 처리는 1바이트 (0~255중 어떤값)을 인덱스로 하고, 256개의 값을 가지고 치환표(S박스)로부터 1개의 값을 얻음
- 16바이트 중 S박스를 통해 1바이트만 변환 하는 것과 같음

![40](/assets/images/2024-04-14/40.png)

### ShiftRows(행 이동)

- 4바이트 단위로 왼쪽 이동해서 썩는 과정

![41](/assets/images/2024-04-14/41.png)

### MixColumns(열 섞기)

- 4바이트 값을 비트연산을 써서 다른 4바이트 값으로 변환

![42](/assets/images/2024-04-14/42.png)

### AddRoundKey(라운드 키와 XOR)

![43](/assets/images/2024-04-14/43.png)

- Rijndael 암호화 1라운드

  - SubBytes -> ShiftRows -> MixColumns -> AddRoundKey

- 복호화(역순)
  - AddRoundKey -> InvMixColumns -> InvShiftRows -> InvSubBytes
  - AddRoundKey는 동일하게 라운드 키와 XOR
  - 다른 연산은 역처리(Inv~~)

### InvMixColumns(역 열 섞기)

![44](/assets/images/2024-04-14/44.png)

### InvShiftRows(역 행 이동)

![45](/assets/images/2024-04-14/45.png)

### InvSubBytes(역 바이트 대체)

![46](/assets/images/2024-04-14/46.png)

### Rijndael의 해독

- Rijndael 알고리즘의 수학적 구조

  - Rijndael의 수식을 수학적인 조작에 의해 풀 수 있다면, Rijndael을 수학적으로 해독할 수 있을 것이다

- Rijndael에 대한 유효한 공격은 현재로서는 발견되지 않음

### 6.4 어떤 암호를 사용하면 좋은가?

- DES

  - 사용하지 말 것
  - 과거 소프트웨어와의 호환성 유지를 위해 필요

- 트리플 DES

  - 안정성 및 호환성 때문에 미사용이 권고됨
  - AE로 대체

- AES(Rijndael)
  - 고속
  - 다양한 플랫폼
  - 현재까지 안전
  - 사용 권장
  - AES 최종 후보 5개도 사용가능

### 기타 대칭키 암호 알고리즘

- SEED

  - 한국 정보 진흥원과 국내 암호전문가들이 함께 개발한 알고리즘
  - 128 비트 블록 암호, 128 비트 비밀키, 64 비트 라운드 키를 생성, 총 16회 라운드 진행
  - 차후 256 비트의 비밀키를 사용하는 SEED-256이 개발

- ARIA(Academy Research Institute Agency)

  - 128 비트의 블록 암호, Involutional SPN 구조
  - 128, 192, 256 비트의 비밀키 사용
  - 입출력 크기와 사용할 수 있는 비밀키의 크기는 AES와 동일

- HIGHT(HIgh security and light weigHT)

  - 저전력 및 경량화 컴퓨터 환경의 기밀성 제공을 위해 개발된 64비트 블록 암호

- LEA(Light weight Encryption Algorithm)

  - 국가보안 기술 연구소가 개발한 128비트 경량 고속 블록 암호
  - 대용량 데이터를 빠르게 처리하거나 스마트폰 보안, 사물인터넷(IoT) 등 저전력 암호화에 널리 사용

- IDEA(International Data Encryption Algorithm)

  - 스위스 연방 기술 기관에서 개발
  - 128 비트 키, 64 비트 블록 암호로 Feistel과 SPN의 중간 형태

- RC5
  - 미국 Rivest가 개발
  - 비교적 간단한 연산으로 빠른 암/복호화 제공(DES보다 10배 빠름)
  - 32, 64, 128 비트 블록 암호, 0 ~ 2040 비트 키, 라운드 수: 1 ~ 255 회 가변적

Q3. DES에 대한 설명 중 올바른 것은?
1 대칭 암호와 비대칭 암호 모두에 사용된다.
2 블록의 길이가 56비트이다.
3 14 라운드로 이루어졌다.
4 평문블록은 제일 먼저 비트 열의 순서를 재배열하는
초기치환을 거친다.
5 S-상자는 48비트를 입력으로 받아들여 48비트를 출력한다.
