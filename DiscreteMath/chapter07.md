# Chapter 07

#### 2024.06.04.(화)

## 7.1 함수의 개념

```
정의 7-1 함수(function: f: A -> B)

집합 A에서 집합 B로 가는 관계가 성립할 때, 집합 A의 원소 a에 대하여 집합 B의 원소 b 하나가 대응되는 관계
```

```
정의 7-2 원상(preimage), 상(image), 정의역(domain), 공역(codomain), 치역(range)

집합 A에서 집합 B로 가는 함수 f: A -> B에 대하여,
(a) 원상: 집합 B의 원소 b와 대응하는 집합 A의 원소 a
(b) 상: 집합 A의 원소 a에 대응하는 집합 B의 원소 b
(c) 정의역(dom(f)): 원상의 집합, 집합 A
(d) 공역(codom(f)): 상이 포함된 집합, 집합 B
(e) 치역(ran(f)): 상의 집합,
```

```
정리 7-1 관계와 함수의 차이

| 집합 A에서 집합 B로의 관계 | 집합 A에서 집합 B로의 함수 |
| 집합 A(정의역)의 어떤 원소는 집합 B(공역)의 원소와 전혀 대응하지 않거나 하나 이상의 원소와 대응할 수 있다. | 집합 A(정의역)의 모든 원소는 집합 B(공역)의 원소 하나와 반드시 대응해야 한다. |
```

- 집합 A = {a,b,c}에서 집합 B = {1,2,3} 으로 가는 관계
  - f1 = {(a,2), (b,1), (c,1)}
    - f1은 함수
      - dom(f1) = A / codom(f1) = B / ran(f1) = {1,2}
  - f2 = {(a,2), (a,3), (c,1), (c,2), (c,3)}
    - f2는 함수 아님
      - 관계 f2의 정의역 A원소 중 b는 대응하는 공역 원소가 없음
      - 관계 f2의 정의역 A원소 중 a,c가 두 개 이상의 공역 원소와 대응

## 7.2 함수의 성질

### 단사 함수, 전사 함수, 전단사 함수

```
정의 7-3 단사 함수, 전사 함수, 전단사 함수

f를 A에서 B로의 함수라 하자. 즉 f: A -> B이다.

(1) 단사 함수: 모든 x, y ( A에 대해서 x != y이면 f(x) != f(y)인 함수이다. (x1, y1) ( f이고 (x2, y) ( f이면 x1 = x2 이다.

(2) 전사 함수: f(A) = B인 함수이다. 즉, Ran(f) = B이다. 다시 말하면 모든 y ( B, 어떤 x ( A, y = f(x) 이다.

(3) 전단사 함수: 단사 함수이면서 전사 함수인 함수를 말한다.
```

`단사 함수`

- 모든 x, y ( A에 대해서 x != y이면 f(x) != f(y)

- 서로 다른 x,y ( A에 대해서 이미지가 같은 값으로 대응되는지를 확인하면 된다.

- (x1, y) ( f이고 (x2, y) ( f 이면 x1 = x2

`전사 함수`

- 전사 함수는 치역과 공변역이 같아지는 함수이다.

- 모든 y ( B, 어떤 x ( A, y = f(x)

`전단사 함수`

- 전단사 함수는 전사 함수도 되고 단사 함수도 되는 경우이다.
  - 조건1) 정의역의 원소 개수 = 공역의 원소 개수
  - 조건2) 서로 다른 정의역 원소가 서로 다른 공역 원소와 대응해야 함

### 7.2.3 전단사함수

- 집합 A = {a,b,c}와 집합 B = {1,2,3}에 대한 함수

  - f1: A -> B일 때, f1 = {(a,2), (b,1), (c,3)}

    - a,b ( A에 대하여 a != b이고 f1(a) = 2 != 1 = f1(b)
    - a,c ( A에 대하여 a != c이고 f1(a) = 2 != 3 = f1(c)
    - b,c ( A에 대하여 b != c이고 f1(b) = 1 != 3 = f1(c)

    따라서 함수 f1은 단사함수

    - f1(a) = 2이므로 2 ( B는 a ( A와 대응
    - f1(b) = 1이므로 1 ( B는 b ( A와 대응
    - f1(c) = 3이므로 3 ( B는 c ( A와 대응
      따라서 함수 f1은 전사함수

따라서 f1은 전단사함수

- 집합 A = {a,b,c}와 집합 B = {1,2,3}에 대한 함수
  - f2: A -> B일 때, f2 = {(a,2), (b,1), (c,1)}
    - a,b ( A에 대하여 a != b이고 f2(a) = 2 != 1 = f2(b)
    - a,c ( A에 대하여 a != c이고 f2(a) = 2 != 1 = f2(c)
    - b,c ( A에 대하여 b != c이고 f2(b) = 1 = 1 = f2(c)
      따라서 함수 f2은 단수함수 아님
    - f2(a) = 2이므로 2 ( B는 a ( A와 대응
    - f2(b) = f2(c) = 1이므로 1 ( B는 b,c ( A와 대응
      따라서 함수 f2는 전사함수 아님

따라서 함수 f2는 전단사함수 아님

## 7.3 합성함수

```
정의 7-6 합성함수(composite function g * f)

두 함수 f: A -> B와 g: B -> C가 있을 때, 집합 A의 각 원소를 집합 C의 원소에 대응하는 함수

g * f = (g * f)(x) = g(f(x)), x ( A
```

- 집합 A = {1,2,3,4}, B = {a,b,c}, C = {w,x,y,z}에 대한 함수

f: A -> B f = {(1,a), (2,b), (3,b), (4,c)}
g: B -> C g = {(a,x), (b,w), (c,a)}
h: C -> A h = {(w,4), (x,2), (y,1), (z,3)}

- 가능한 합성함수들: g _ f = g(f(x)), h _ g = h(g(x)), f \* h = f(h(x))
  - g(f(1)) = g(a) = x
  - g(f(2)) = g(b) = w
  - g(f(3)) = g(b) = w
  - g(f(4)) = g(c) = z

따라서 g \* f = {(1,x), (2,w), (3,w), (4,z)}

dom(g _ f) = A, codom(g _ f) = C,ran(g \* f) = {w,x,z}

- 교환법칙이 성립하지 않는 합성함수

집합 A = {1,2,3}에 대한 함수

f: A -> A f = {(1,3), (2,1), (3,2)}
g: A -> A g = {(1,1), (2,3), (3,3)}

g _ f = {(1,3), (2,1), (3,3)}
f _ f = {(1,3), (2,2), (3,2)}

- 합성함수의 결합법칙

집합 A = {1,2,3,4}, B = {a,b,c,d,e}, C = {x,y,z}, D = {11,12,13,14}에 대한 함수

f: A -> B f = {(1,c), (2,a), (3,e), (4,b)}
g: B -> C g = {(a,x), (b,y), (c,y), (d,z), (e,z)}
h: C -> A h = {(x,12), (y,11), (z,14)}

### 7.3.1 합성함수의 성질

```
정리 7-2 합성함수의 성질

집합 A, B, C에 대한 함수 f: A -> B, g: B -> C가 있을 때, 합성함수 g * f의 성질은 다음과 같다.

(a) f와 g가 단사함수이면, g * f도 단사함수이다.
(b) f와 g가 전사함수이면, g * f도 전사함수이다.
(c) f와 g가 전단사함수이면, g * f도 전단사함수이다.
(d) g * f가 단사함수이면, f도 단사함수이다.
(e) g * f가 전사함수이면, g도 전사함수이다.
(f) g * f가 전단사함수이면, f는 단사함수이고 g는 전사함수이다.
```

## 7.4 함수의 종류

### 7.4.1 항등함수

```
정의 7-7 항등함수(identity function: IA)

집합 A에 대한 함수 f: A -> A가 f(a) = a로 정의되는 관계
```

- 함수의 정의역, 공역, 치역 모두 상등
- 단사함수, 전사함수, 전단사함수

* 집합에 대한 함수
  - f1 = x의 경우, f1 = {(-1,-1), (0,0), (1,1)}
    - 항등함수
  - f2 = |x|의 경우, f2 = {(-1, 1), (0,0), (1,1)}
    - 항등함수 아님

```
정리 7-3 항등함수와의 합성

함수 f: A -> B가 있고 집합 A에 대한 항등함수가 IA, 집합 B에 대한 항등함수 IB일 때,

f * IA = IB * f = f
```

```
증명

a ( A이고 b ( B일 때 f(a) = b라고 가정하자. 항등함수 IA의 경우 IA(a) = a, 항등함수 IB의 경우는 IB(b) = b이다. 따라서 다음이 성립한다.

f * IA = f(IA(a)) = f(a) = b
IB * f = IB(f(a)) = IB(b) = b

그러므로 f * IA = IB * f = f가 성립한다.
```

- 집합 A = {1,2,3}에서 집합 B = {a,b,c,d}로의 함수

f = {(1,c), (2,a), (3,d)}

- IA = {(1,1), (2,2), (3,3)}, IB = {(a,a), (b,b), (c,c), (d,d)}
- f \* IA = f(IA(x))에 대하여

  - f \* IA = f(IA(1)) = f(1) = c
  - f \* IA = f(IA(2)) = f(2) = a
  - f \* IA = f(IA(3)) = f(3) = d

- IB \* f = IB(f(x))에 대하여
  - IB \* f = IB(f(1)) = IB(c) = c
  - IB \* f = IB(f(2)) = IB(a) = a
  - IB \* f = IB(f(3)) = IB(d) = d

따라서 f _ IA = IB _ f = f

### 7.4.2 역함수

```
정의 7-8 역함수(inverse function: f^(-1))

전단사함수 f: A -> B에 대해 B -> A로 대응되는 관계. a ( A, b ( B에 대해 f(a) = b일 때, f^(-1)(b) = a

(f(a): 가역함수, f^(-1)(b): 역함수)

* 가역함수(invertible function)
: 전단사함수로 역함수가 존재하는 함수
```

- 가역함수 f의 정의역 = 역함수 f^-1의 공역
- 가역함수 f의 공역 = 역함수 f^-1의 정의역
- 단사함수이거나 전사함수인 경우, 역함수를 구할 수 없음

* f1: {1,2,3} -> {a,b,c,d} f1 = {(1,b), (2,c), (3,d)}

  - 단사함수
  - f1^-1의 정의역 원소 a와 대응하는 f1^-1의 정의역 원소가 2개 존재

* f: {1,2,3} -> {a,b,c} f = {(1,a), (2,b), (3,c)}
  - 전단사함수
  - f^-1 = {(a,1), (b,2), (c,3)}

```
정리 7-4 항등함수와 역함수의 관계

전단사함수 f: A -> B에 대하여 다음이 성립한다.

(a) f^-1 * f = IA
(b) f * f^-1 = IB
```

```
증명

a ( A, b ( B에 대해 f(a) = b이면 함수 f는 가역함수(전단사함수)이므로 f^-1(b) = a이다.

(a) (f^-1 * f)(a) = f^-1(f(a)) = f^-1(b) = a
합성함수 f^-1 * f의 최초 입력인 a ( A와 출력 a는 같은 원소이므로, f^-1 * f는 집합 A에 대한 항등함수 IA이다.

따라서, f^-1 * f = IA

(b) (f * f^-1)(b) = f(f^-1(b)) = f(a) = b
합성함수 f * f^-1의 최초 입력인 b ( B와 출력 b는 같은 원소이므로, f * f^-1는 집합 B에 대한 항등함수 IB이다.

따라서, f * f^-1 = IB
```

```
정리 7-5 합성함수의 역함수

전단사함수 f: A -> B, g: B -> C에 대하여 다음이 성립한다.

(g * f)^-1 = f^-1 * g^-1
```

```
증명

(g * f)^-1는 합성함수 g*f의 역함수이므로 [정리 7-4(a)]에 의해 (g*f)^-1 * (g*f) = IA가 성립한다. 여기에 (g*f)^-1 대신 f^-1 * g^-1를 대입하여 같은 결과가 나오는지 확인한다.

(g * f)^-1 * (g * f) = (f^-1 * g^-1) * (g * f)
                     = f^-1 * (g^-1 * g) * f (결합법칙에 의하여)
                     = f^-1 * (IB * f)  ([정리 7-4(a)]에 의하여)
                     = f^-1 * f ([정리 7-3]에 의하여)
                     = IA
```

따라서 (g _ f)^-1 = f^-1 _ g^-1가 성립한다.

### 7.4.3 상수함수

```
정의 7-9 상수함수(constant function)

함수 f: A -> B에서 집합 A의 모든 원소가 집합 B의 원소 하나에만 대응하는 관계

모든 a ( A, 어떤 b ( B에 대해 f(a) = b
```

- 상수함수의 공역의 원소 수가 n개일 때
  - 정의역, 공역의 원소 수가 모두 n > 1이면
    - 단사함수도, 전사함수도 아님
  - 정의역 원소가 1개, 공역의 원소 수가 n > 1이면
    - 단사함수이지만 전사함수 아님
  - 정의역의 원소 수가 n > 1, 공역 원소가 1개이면
    - 단사함수는 아니고 전사함수임
  - 정의역, 공역 원소가 모두 1개이면
    - 전단사함수

### 7.4.4 특성함수

```
정의 7-10 특성함수(characteristic function: fA)

전체집합 U의 부분집합인 A에 대하여 다음과 같은 출력을 갖는 함수

fA(x) = {1, x ( A일 때
        0, x !( A일 때}
```

### 7.4.5 바닥함수와 천정함수

```
정의 7-11 바닥함수(floor function: [x]) / 최대정수함수(greatest integer function)

x ( R에 대해 x보다 작거나 같은 정수 중 가장 큰 정수를 구하는 함수

[x] = n <-> n <= x < n+1, n ( Z
```

```
정의 7-12 천정함수(celling function: [x]) / 최소정수함수(least integer function)

x ( R에 대해 x보다 크거나 같은 정수 중 가장 적은 정수를 구하는 함수

[x] = n <-> n-1 < x <= n, n ( Z
```

## 7.5 해시 함수

### Hash Functions - Definition

- Definition

  - A hash function H maps a message M (a bitstrings) of arbitrary length to short fixed-length bitstrings T (e.g. 256 bits) that serves as fingerprint for M:

    - H: F\*2 -> Ft2
    - H(M) = T

  - Hash functions provide integrity and efficiency.

### Hash Functions - Simple Example

- Simple toy example

  - Input m is decimal integer

    - View as string of (three) digits
    - For example, M = 218 -> m1 = 2, m2 = 1, m3 = 8

  - Sum hash:

    - hsum(m) = (m1 + m2 + m3) mod 10

  - Exercise: hsum(218) = ?

  - Sum hash:
    - hsum(m) = (sum of mis) mod 10
  - Exercise: hsum(218) = 1

```
Note that this hash function is just a toy example.
Not secure!
Why?
```

- Simple toy example - attack 1

Digest forgery  
hsum(M || 10 - sum(mi) || X)

- Simple toy example - attack 2

Message forgery
hsum(M' || 10 - sum(mi') || 10 - sum(mi))

```
Now we know that this hash function is not secure!
Then, what do we consider to make secure hash functions?
```

### Hash Function - Security

- Preimage resistance (Weaker Notion)

  - Given H(x), find x' such that H(x') = H(x)
    - Generic complexity: about 2^t trials

- Second preimage resistance (Weaker Notion)

  - Given x, find x' != x such that H(x) = H(x')
    - Generic complexity: about 2^t trials

- Collision resistance (Strongest Notion)
  - Distinct x and x' with H(x) = H(x')
    - Generic complexity: about 2^(t/2) trials due to the birthday paradox

### Cryptographic Hash Function

- Uniformly distributed

#### Hash Function - Security

- Preimage resistance: One-way function

  - Given H(x) for random x, it is hard to find x, or any x' s.t. H(x') = H(x)

- Second preimage resistance

  - Given random x, it is hard to find x' such that
    H(x') = H(x)
    - Hard to find collision with a specific random x

- Collision resistance

  - It is hard to find (x, x') such that H(x') = H(x)
    - Note: attacker can always try inputs randomly till finding collisions

- 생일 문제 (Birthday Problem), 생일 역설 (Birthday Paradox)

  - 사람이 임의로 모였을 때 그 중에 생일이 같은 두 명이 존재할 확률을 구하는 문제
    - 생일의 가능한 가짓수는 365개이므로 366명 이상의 사람이 모인다면 비둘기집 원리에 따라 생일이 같은 1쌍(두 명)이 반드시 존재.
    - 그렇다면, 생일이 같은 두 사람이 있을 확률이 1/2일 이상이 되려면, 몇 명의 모이면 될까?
      - 직관: (확률론적 가정) 생일이 366가지이므로 임의의 두 사람의 생일이 같을 확률은 1/366이고, 따라서 366명쯤은 모여야 생일이 같은 경우가 있을 것이라고 생각
      - 실제로는 23명만 모여도 생일이 같은 두 사람이 있을 확률이 1/2(50%)를 넘고, 57명이 모이면 99%를 넘음.
  - 활용: 생일이 같은 두 사람을 찾는 것과 비슷하게, 해시 충돌값 모든 입력값을 계산하지 않아도 충분히 높은 확률로 해시 충돌을 찾을 수 있음.
  - 즉, 가능한 데이터의 개수가 2^n일 때, 2^(n/2) 정도의 데이터가 있으면 일치하는 데이터가 존재할 확률이 1/2보다 큼
  - [생일이 같을 확률] = 1 - [생일이 다를 확률]
  - 일반화: n 명의 사람이 있을 때 그 중 생일이 같은 사람이 둘 이상 있을 확률을 p(n)이라고 한다면, 반대로 모든 사람의 생일이 다를 확률 p'(n)이 된다. 먼저 p'(n)을 구해보면, 두 번째 사람의 생일은 첫 번째 사람과 다르고, 세 번째 사람의 생일은 첫 번째와 두 번째 모두와 달라야 한다.

  - 즉, 50명만 모이면 그 가운데 2명 이상의 생일이 같을 확률이 97%이고, 100명이 모이면 거의 1에 가까워진다는 것을 알 수 있음
  - 가능한 데이터의 개수가 2^n일 때, 2^(n/2) 정도의 데이터가 있으면 일치하는 데이터가 존재할 확률이 1/2보다 큼.

### Hash Function - Applications

- Collision - Resistance and Software Distribution

  - Developer in Seoul develops large software m
  - Repository in Pusan obtains copy of m
  - User in Ulsan wants to obtain m - securely and efficiently
    - Don't send m from Seoul both Pusan and Ulsan
  - How?

  - Repository in Pusan downloads software m from developer in Seoul
  - User download from (nearby) repository; receives m'
    - Is m' = m? User should validate! How?
  - User securely downloads h(m) directly from developer
    - Digest h(m) is hort - much less overhead than downloading m
  - User Validate: h(m) = h(m') -> m = m'
