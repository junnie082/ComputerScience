# Network 4

# Chapter 04 유니캐스트 라우팅(1)

#### 2024.04.18.(목)

### 4.1 라우팅 소개

- 수없이 많은 호스트와 엄청 많은 라우터가 있는 인터넷에서의 라우팅은 계층적 라우팅으로만 가능

- 다양한 라우팅 알고리즘과 여러 단계의 라우팅 필요

- 인터넷의 라우팅 개념과 알고리즘을 이해하고, 어떻게 인터넷에 적용하는지를 고찰

### 4.1.1 일반적 개념

- 유니캐스트 라우팅에서, 출발지에서 도착지까지 포워딩 테이블을 참조하여 홉 단위로 라우팅 됨

- 출발지 호스트는 자신이 속한 네트워크의 디폴트 라우터에게 패킷을 전달하면 되므로 포워딩 테이블이 불필요

  - 호스트가, 여러 네트워크에 동시에 연결된 경우에는 필요

- 목적지 호스트도 자신이 속한 라우터로부터 패킷을 수신하므로 포워딩 테이블 불필요

  - 여러 네트워크에 동시에 연결된 경우에는, 응답 데이터를 보낼 때 필요

- 인터넷에 연결된 라우터에서 포워딩 테이블이 필요함

### 4.1.2 최소비용(Least-Cost) 라우팅

- 인터넷을 weighted graph로 표시하면, 출발 라우터로부터 도착 라우터로의 가장 좋은 경로는, 둘 사이의 최소 비용 경로

- 즉, 출발 라우터는 목적지로 향하는 여러 가능 경로 중에서 총 비용이 최소가 되는 경로를 선택

![4](/assets/images/2024-04-18/4.png)

![5](/assets/images/2024-04-18/5.png)

### 4.2 라우팅 알고리즘

- 과거 여러 라우팅 알고리즘이 고안됨

- 라우팅 알고리즘들의 차이점

  - 최소비용의 해석
  - 최소 비용 트리를 만드는 방법

- 이 섹션에서는 공통 알고리즘을 논의하고, 인터넷의 라우팅 프로토콜이 이들 중의 한 알고리즘을 어떻게 구현했는지 고찰

### 4.2.1 거리-벡터(Distance-Vector) 라우팅

- 거리-벡터 (DV)
  - 방향과 거리 값
- 최적의 경로 탐색
  - 각 노드는 자신의 이웃 정보를 통해 최소 비용 트리 생성 (단순, 불완전한 정보)
  - 이 정보를 이웃 노드들과 교환하여 업데이트
- 거리-벡터 라우팅에서, 각 라우터는 계속 이웃 노드들에게 자신이 알고있는 (미완) 정보를 끊임없이 교환

### 벨만-포드(Bellman-Ford) 공식

- 소스와 목적지 사이의 최소비용경로 찾는 식
- 소스 x, 목적지 y 사이의 최소비용
  - 중간 노드 a, b, c, ...

> > Dxy = min{(cxa + Day), (cxb + Dby), (cxc + Dcy), ...}

- 업데이트
  - 새로운 경로의 중간 노드 z

> > Dxy = min{Dxy, (cxz + Dzy)}

![6](/assets/images/2024-04-18/6.png)

![7](/assets/images/2024-04-18/7.png)

![8](/assets/images/2024-04-18/8.png)

![9](/assets/images/2024-04-18/9.png)

![10](/assets/images/2024-04-18/10.png)

### 노드의 거리-벡터 라우팅 알고리즘

```C
Distance_Vector_Routing()
{
    // Initialize (create initial vectors for the node)
    D[myself] = 0
    for (y=1 to N)
    {
        if (y is a neighbor)
            D[y] = c[myself][y]
        else
            D[y] = inifinite
    }

    send vector {D[1], D[2], ..., D[N]} to all neightbors
    // Update (improve the vector with the vector received from a neighbor)
    repeat (forever)
    {
        wait (for a vector Dw from a neighbor w or any change in the link)
        for (y=1 to N)
        {
            D[y] = min[D[y], (c[myself][w] + Dw[y])]
            // Bellman-Ford equation
        }
        if (any change in the vector)
            send vector {D[1], D[2], ..., D[N]} to all neighbors
    }
}  // End of Distance Vector
```

![11](/assets/images/2024-04-18/11.png)

### 4.2.2 링크-상태(Link-State) 라우팅

- Link-state
  - 링크(에지, 라우터 간의 연결) 특성
- 에지의 비용이 링크의 상태를 결정
- 저비용의 링크 선호
  - 무한대의 링크 비용: 없거나 고장
- 최소비용트리를 만들려면 전체 네트워크의 지도가 필요
  - 모든 라우터의 링크 비용을 알아야 함
- Link-state database (LSDB)
  - 모든 링크 상태 집합

![12](/assets/images/2024-04-18/12.png)

![13](/assets/images/2024-04-18/13.png)

### 다익스트라(Dijkstra) 알고리즘

- LSDB로부터 최소비용트리 생성

- 3단계
  - 루트 노드 선정(라우터 자신). 루트 노드만 있는 트리로 시작
  - 아직 트리에 없고 연결된 노드를 선택하여 트리에 추가하고, 트리내의 모든 노드까지의 비용 업데이트
    - D[x] (목적지 x까지의 비용), W (추가 노드)
      > > D[x] = min{D[x], (D[w] + c[w][x])}
  - 모든 노드가 포함될 때까지 위, 단계 수행
- 최종적으로, 루트누드에서 모든 노드로의 최소비용트리 생성됨

### Dijkstra's Algorithm

```C
Dijkstras Algorithm()
{
    // Initialization
    Tree = {root} // Tree is made only of the root
    for (y=1 to N) // N is the number of nodes
    {
        if (y is the root)
            D[y] = 0 // D[y] is shortest distance from root to node y
        else if (y is a neighbor)
            D[y] = c[root][y] // c[x][y] is cost between nodes x and y in LSDB
        else
            D[y] = infinite
    }
    // Calculation
    repeat
    {
        find a node w, with D[w] minimum among all nodes not in the Tree
        Tree = Tree union {w} // Add w to tree
        // Update distances for all neighbor of w
        for (every node x, which is neighbor of w and not in the Tree)
        {
            D[x] = min{D[x], (D[w] + c[w][x])}
        }
    } until (all nodes included in the Tree)
} // End of Dijkstra
```

![14](/assets/images/2024-04-18/14.png)

### 4.2.3 경로-벡터 (Path-Vector) 라우팅

- 최소비용이 아닌 다른 정책(policy)을 적용하는 라우팅
  - 특정 노드 제외 경로
- Best Spanning Tree 선정(policy 적용)
- ISP 간의 패킷 경로를 위한 고안
- Distance-vector의 Bellman-ford 알고리즘과 비슷하지만, 최소비용이 아닌 정책을 반영
- 라우팅 테이블 내용은 경로벡터 [x,a,b,..c,d,y]
  - 이웃노드와 교환
