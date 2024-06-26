# Chapter 09

#### 2024.06.04.(화)

## 9.3 트리의 활용

### 9.3.1 최소 스패닝 (신장) 트리

```
정의 스패닝 트리

그래프 G의 부분 그래프 T가 트리이고, T가 G의 모든 정점들을 포함한다면 T를 그래프 G의 스패닝 트리(spanning tree)라고 한다.
```

- 그래프 G의 스패닝 트리는 유일하지 않고 구성하는 방법도 다양하지만, 대표적인 방법으로 너비우선 탐색 방법과 깊이우선 탐색 방법을 사용해서 구성하는 방법이 있다.

- 이렇게 구한 스패닝 트리를 각각 너비우선 스패닝 트리와 깊이우선 스패닝 트리라고 한다.

```
정의 최소 스패닝 트리

임의의 그래프 G가 가중치 그래프라고 할 때, G로부터 생성된 스패닝 트리 가운데 각 간선의 가중치 합이 최소가 되는 것을 최소 스패닝 트리(minimal spanning tree; MST)라고 한다.
```

- 최소 비용 생성 트리의 대표적인 2가지 방법은 `프림(Prim)`의 알고리즘과 `크루스칼(Kruskal)`의 알고리즘임

### 9.3.1 최소 스패닝 (신장) 트리: 프림 알고리즘(Prim algorithm)

- m개의 정점을 갖는 가중치 그래프 G = (V,E)에서 최소 스패닝 트리를 찾기 위한 두 가지 방법

- 프림 방법
  - 그래프 G = (V,E)에서 임의의 점 하나를 선택한 후, (n-1)개의 간선을 하나씩 추가시켜 트리를 만든다
    - 간선을 추가하는 방법
      - 현재까지 만들어진 트리에 연결시킬 때
      - 트리내 점 u와 트리 밖의 점들 v 중 최소 가중치를 가지는 간선 (u,v)를 선택하고 v를 U에 포함시키고 이 간선을 트리에 포함시킨다. (여기서, U는 만들어진 트리에 포함된 노드 집합)
      - 물론 이때, 트리에 간선을 추가할 때, 순회가 되지 않아야 한다.
- [알고리즘] 최소 스패닝 트리 - 프림 알고리즘

```C
void Prim(graph G: set_of_edges T)
// 프림의 알고리즘은 G에 대한 최소 비용 생성 트리를 만든다.
{
    set_of_verticles U;
    vertex u, v;
    T = NULL;
    U = {1};
    while ( U != V)
    {
        let (u, v) be a lowest cost edge such that u is in U and v is in V - U;
        T = T U {(u,v)};
        U = U U {v};
    }
} // Prim
```

### PrimMST 알고리즘 수행 과정

- 임의의 시작점으로 점 c가 선택되었다고 가정하자. 그리고 D[c] = 0으로 초기화시킨다. (여기서, 배열 D[v]는 T(트리)에 있는 점 u와 바깥 점 v를 연결하는 선분의 최소 가중치를 저장하기 위한 원소이다.)

- 시작점 c와 선분으로 연결된 각 점 v에 대해서, D[v]를 각 선분의 가중치로 초기화시키고, 나머지 각 점 w에 대해서, D[w]는 무한대로 초기화시킨다.

- T = {c}로 초기화한다.

- 현재 T에는 점 c만이 있다. 따라서 T에 속하지 않은 각 점 v에 대하여, D[v]가 최소인 점 vmin을 선택한다. D[b] = D[f] = 1로서 최소값이므로 점 b나 점 f 중 택일. 여기서는 점 b를 선택하자. 따라서 점 b와 선분 (c,b)가 T에 추가된다.

- 점 b에 연결된 점 a와 d의 D[a]와 D[b]를 각각 3과 4로 갱신한다.

  - 점 f는 점 b와 선분으로 연결은 되어있으나, 선분 (b,f)의 가중치인 2가 현재 D[f]보다 크므로 D[f]를 갱신 안됨

- T에 속하지 않은 각 점 v에 대하여, vmin 인 점 f를 찾고, 점 f와 선분 (c,f)를 T에 추가시킨다.

- 점 f에 연결된 점 e의 D[e]를 9로 갱신한다. D[d]는 선분 (f,d)의 가중치인 7보다 작기 때문에 갱신 안됨

- 그 다음부터는 점 a와 선분 (b,a), 점 d와 선분 (a,d)가 차례로 T에 추가되고, 최종적으로 점 e와 선분 (a,e)가 추가되면서, 최소 신장 트리 T가 완성된다. T를 리턴하고, 알고리즘을 마친다. 다음의 그림들이 위의 과정을 차례로 보여준다.

- PrimMST 알고리즘이 최종적으로 리턴하는 T에는 왜 사이클이 없을까?
  - 프림 알고리즘은 T 밖에 있는 점을 항상 추가하므로 사이클이 안 만들어진다.

```
1. v개의 정점을 포함하는 그래프 G에서 가중치가 가장 작은 변과 그 변에 연결된 정점을 노드로 선택한다. : 가중치가 같은 변이 2개 이상 있을 경우 임의로 하나를 선택한다.
2. 선택한 노드에 연결된 모든 변들 중 가중치가 가장 작은 변을 선택한다.
: 가중치가 같은 변이 2개 이상 있을 경우 임의로 하나를 선택한다.
: 순환이 형성되는 경우에는 그 변을 선택하지 않는다.
3. 선택한 변이 v-1개가 될 때까지 2의 과정을 반복한다.
```

```
정리 9-5 프림 알고리즘

Prim(v개의 노드를 갖는 가중치 그래프 G) {
    최소신장트리를 구성하는 변의 리스트 T에 smallest(W[vi, vj])에 해당하는 변 ei 추가
    최소신장트리를 구성하는 노드 리스트 N에 리스트 T에 포함된 ㅂ녀과 연결되는 정점들 vi, vj 추가
    while (e <= v-1) {
        if (smallest(W[vi,vj], vi, vj in 리스트 N)) {
            if (not (ei in 리스트 T와 (vi, vj)로 순환 발생)) {
                리스트 T에 선택된 변 (vi, vj) 추가;
                리스트 N에 노드 vi, vj 추가;
                weight += W[vi, vj];
                e++;
            }
        }
    }
}
```

### 9.3.1 최소 스패닝 (신장) 트리: 크루스칼 알고리즘(Kruskal algorithm)

- 크루스칼 방법

  - 아이디어: 가중치를 기준으로 그래프의 모든 간선들을 정렬한 후, 가장 작은 간선을 순서대로 트리에 추가하는데 사이클을 만들지 않을 때에만 그 선분을 추가시킨다.

- 알고리즘 개요
  1. 각 단계에서 가중치가 최소인 간선을 선택
  - 미리 모든 간선을 가중치에 따라 정렬
  2. 선택한 간선 때문에 사이클이 만들어진다면 선택한 간선을 버림
  - 배타적 집합(disjoint set)의 개념을 이용하는 union-find 연산
  3. 받아들여진 간선의 수가 n-1 개가 되면 종료
  - n개의 트리로 구성된 포리스트에서 출발하여 최종 1개의 트리를 생성

```
주어진 가중 그래프 G = (V,E)에서 V = {1,2, ..., n}이라고 하고 T를 연결선의 집합이라고 하자.
1) T를 phi로 놓는다.
2) 연결선의 집합 E를 비용이 적은 순서로 정렬한다.
3) 가장 최소값을 가진 연결선 (u,v)를 차례로 찾아서 (u,v)가 사이클을 이루지 않으면 (u,v)를 T에 포함시킨다.
4) (3)의 과정을 |T| = |V| - 1일 때까지 반복한다.
```

```
T = NULL;
while (T contains less than n-1 edges and E not empty)
{
    choose an edge (v,w) from E of lowest cost;
    delete(v,w) from E;
    if ((v,w) does not create a cycle in T)
        add(v,w) to T;
    else discard (v,w);
}
if (T contains fewer than n-1 edges) printf("no spanning tree\n");
```

- union-find 알고리즘
  - 추가하고자 하는 간선의 양끝 정점이 같은 집합에 속해 있는지를 먼저 검사 필요
  - (두 원소가 다른 집합에 속할 때) 두 집합들의 합집합을 생성하는 `union`
  - 특정 원소가 어떤 집합에 속하는지 알아내는 `find`
  - Kruskal의 MST 알고리즘에서 사이클 검사에 사용

```
1. 노드 수가 v개인 그래프에서 가중치가 가장 작은 변을 차례로 선택한다.
2. 가중치가 같은 변은 모두 선택한다.
3. 노드 사이에 순환이 형성되는 경우 그 변을 선택하지 않는다.
4. 선택된 변이 v-1개가 되면 종료한다.
```

```
정리 9-6 크루스칼 알고리즘

Kruskal(v개의 노드를 갖는 가중치그래프 G) {
    가중치 리스트 W에 그래프 G에 연결된 모든 변의 가중치 W[vi, vj]로 초기화
    최소신장트리를 구성하는 변의 리스트 T 선언
    최소신장트리를 구성하는 노드 리스트 N 선언
    while(e <= v-1) {
        if (smallest(W[vi, vj] in 리스트 W)) {
            if (not(ei in 리스트 T와 (vi, vj)로 순환 발생)) {
                리스트 T에 선택된 변 (vi, vj) 추가;
                리스트 N에 노드 vi, vj 추가;
                가중치 리스트 W에서 W[vi,vj] 삭제;
                weight += W[vi, vj];
                e++;
            }
        }
    }
}
```

### 9.3.2 허프만 알고리즘

- 파일의 각 문자는 일반적으로 고정된 크기의 코드로 표현

  - 파일의 각 문자가 8 bit 아스키 (ASCII) 코드로 저장되면, 그 파일의 bit 수는 8x(파일의 문자 수)

- 이러한 고정된 크기의 코드로 구성된 파일들 저장하거나 전송할 때 파일의 크기를 줄이고, 필요시 원래의 파일로 변환할 수 있으면, 메모리 공간을 효율적으로 사용할 수 있고, 파일 전송 시간을 단축시킬 수 있음.

- 파일 압축 (file compression): 주어진 파일의 크기를 줄이는 방법.

(Ex)
Fixed-Length Code: A(000), C(001), T(010), G(011)
Variable-Length Code: A(0), C(111), T(100), G(101)

- 허프만 (Huffman) 압축 아이디어

  - 파일에 빈번히 나타나는 문자에는 짧은 이진 코드를 할당하고, 드물게 나타나는 문자에는 긴 이진 코드를 할당

- 접두부 특성 (prefix property)

  - 허프만 압축 방법으로 변환시킨 문자 코드들 사이에 존재
  - 각 문자에 할당된 이진 코드는 어떤 다른 문자에 할당된 이진 코드의 접두부 (prefix)가 되지 않는다는 것을 의미한다.
  - Ex)
    - 문자 'a'에 할당된 코드가 '101'이라면, 모든 다른 문자의 코드는 '101'로 시작되지 않음.
    - Any prefix code can be represented by a binary tree.
      - b: 0, a: 10, c: 11
  - If prefix code, the parsing (decoding) is easy

    - 접두부 특성의 장점은 코드와 코드 사이를 구분할 특별한 코드가 필요 없다.
    - 예를 들어, 101#10#1#111#0#... 에서 '#'가 인접한 코드를 구분 짓고 있는데, 허프만 압축에서는 이러한 특별한 코드 없이 파일을 압축하고 해제할 수 있다.

    - (Ex) b: 0, a: 10, c: 11
      - ababcbbbc <-> 1001001100011

- 허프만 압축은 입력 파일에 대해 각 문자의 출현 빈도수 (문자가 파일에 나타나는 횟수)에 기반을 둔 이진트리를 만들어서, 각 문자에 이진 코드를 할당한다.

- 이러한 이진 코드를 `허프만 코드`라고 한다.

- (예제) a,b,c,d,e에 대해 허프만 알고리즘을 적용하여 허프만 코드를 생성하자.

- 입력 파일은 4개의 문자로 되어 있고, 각 문자의 빈도수는 다음과 같다.

A; 450 T: 90 G: 120 C: 270

- 앞의 예제에서 'A'는 '0', 'T'는 '100', 'G'는 '101', 'C'는 '11'의 코드가 각각 할당

10110010001110101010100

101/100/100/0/11/101/0/101/0/100

G T T A C G A G A T

- 할당된 코드드들을 보면, 가장 빈도수가 높은 'A'가 가장 짧은 코드를 가지고, 따라서 루트의 자식이 되어 있고, 빈도수가 낮은 문자는 루트에서 멀리 떨어지게 되어 긴 코드를 가진다.

- 접두부 특성을 가지고 있음

- 파일 압축률: (1,620/7,440) x 100 = 21.8%이며, 원래의 약 1/5 크기로 압축됨.
  - 압축된 파일의 크기의 bit 수: (450 x 1) + (90 x 3) + (120 x 3) + (270 x 2) = 1,620
  - 아스키 코드로 된 파일 크기 bit 수: (450 + 90 + 120 + 270) x 8 = 7,440 (빈도수 A: 450 T: 90 G: 120 C: 270)

### 9.3.3 트리 기타 활용 예

`게임(game)`

- 트리는 체스, 틱택토(tic-tac-toe), 장기, 바둑 등 게임에 있어서의 진행과 전략을 구사할 수 있는 게임 트리로도 활용됨

- tic-tac-toe: 가로, 세로, 대각선으로 연속된 세 개를 놓으면 이기는 게임

- tic-tac-toe 게임트리 탐색 방법
  - 상대방의 수를 고려하여 트리를 만든 뒤 트리의 마지막 노드를 이용하여 게임의 승패를 결정하고, 이 정보를 트리의 루트까지 전파(업데이트) 시킴.

### 9.3.3 트리 기타 활용 예 - 틱택토(tic-tac-toe)

- 9칸의 빈 공간에 'O'와 'X'를 순서를 바꿔가며 한 수씩 두면서 가로나 세로 또는 대각선으로 같은 모양 3개를 연속해서 채우면 이기는 게임
- 왼쪽 그림처럼 처음 빈칸을 채울 수 있는 경우의 수: 9가지
- 다음에 둘 수 있는 경우의 수는 각 수당: 8가지
- 따라서, 게임트리의 크기 = 9!(Factorial) = 9 _ 8 _ 7 ... \* 1 = 362,880
  - 이 수들을 다 두어 본 다음에 첫 수를 결정할 수 있음.
- 탐색트리 가장 위에 위치한 보드가 현재 플레이어의 상태
- 게임이 끝난 상태의 보드들이 저장됨
- 승패 혹은 점수를 계산 가능: 1(승리), 0(무승부), -1(패배)
- 이후, 트리 마지막 줄에서부터 가장 꼭대기까지 승패의 결과를 업데이트 함
  - 승패 정보를 밑에서부터 위로 전파시킬 때 규칙: 최소최대알고리즘(MinMax)

`최소최대 알고리즘(Minmax Algorithm)`

- 바둑과 체스 같은 게임에서는 상대방의 다음에 놓을 수를 미리 예측하고 플레이한다. 일반적으로 내 차례에서는 내게 제일 유리한 수, 상대방 차례에서는 내게 제일 불리한 수가 선택될 것이다.

  - 즉, 게임트리의 가정은 한 플레이어에게 유리한 수는 상대 플레이어에게 불리한 수라는 것이다.

- 이 알고리즘은 위 가정을 고려하여, 다음 차례만이 아니라, 그 이후의 수까지도 바라보면 탐색을 해가는 과정이다. 즉, 최대와 최소를 번갈아가며 선택해 가장 좋은 경우를 선택하는 알고리즘이다.

- 아래 예제는 4수 앞을 예측한 트리 노드이며 0,2,4는 내 차례이며 1,3은 상대방의 차례이다. 최소최대 알고리즘에 의해 다음과 같이 예측한다.

- MinMax 알고리즘의 시간복잡도: exponentially increase

- 이를 보완하기 위해 나온 것이 alpha-beta pruning

- 8-puzzle 게임

- 이 게임은 3x3 크기의 박스 안에서 8개의 격자는 숫자로 표시되어 있고 한 개의 격자는 비어 있다. 이 때, 초기 상태에서 빈 격자에 인접한 숫자를 빈 곳으로 계적으로 움직여서 목표 상태(goal)로 만드는 서양식 게임
