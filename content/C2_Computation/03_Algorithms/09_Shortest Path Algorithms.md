---
aliases: []
type: Lecture
tags:
  - 3-1/알고리즘
draft: false
date: 2026-05-19
---
1. 그래프의 정의: [[07_Graph (이산수학)]]
2. 그래프의 표현: [[08_Graph (자료구조)]]
3. Graph Algorithms
	- **Graph Traversal Algorithms**
		- [[07_Elementary Graph Algorithms#2.1. 너비 우선 탐색 (BFS, Breadth-First Search)|BFS (Breadth-First Search)]], 인접 리스트 $O(V+E)$
		- [[07_Elementary Graph Algorithms#2.2. 깊이 우선 탐색 (DFS, Depth-First Search)|DFS (Depth-First Search)]], 인접 리스트 $O(V+E)$
	- **MST Algorithms**
		- [[08_MST Algorithms#2.1. Kruskal's Algorithm|Kruskal's Algorithm]], $O(E\lg{E})$
		- [[08_MST Algorithms#2.2. Prim's Algorithm|Prim's Algorithm]], 우선순위 큐 $O(E\lg{V})$
	- **SSSP** (Single-Source Shortest Path Algorithms)
		- (음수 가중치 X) [[09_Shortest Path Algorithms#2.1. Dijkstra's Algorithm|Dijkstra's Algorithm]], 우선순위 큐 $O(E\lg{V})$
		- (음수 가중치 O) [[09_Shortest Path Algorithms#2.2. Bellman-Ford Algorithm|Bellman-Ford Algorithm]], $O(VE)$
	- **APSP** (All-Pairs Shortest Path Algorithms)
		- [[09_Shortest Path Algorithms#4.3. Floyd-Warshall Algorithm|Floyd-Warshall Algorithm]], $O(V^3)$

## 요약
|            알고리즘            |  문제  | 음수 가중치 |     시간 복잡도     |
| :------------------------: | :--: | :----: | :------------: |
|        Bellman-Ford        | SSSP |   허용   |     O(VE)      |
|          Dijkstra          | SSSP |   불가   | O(E + V log V) |
|       Floyd-Warshall       | APSP |   허용   |     Θ(V³)      |
| Matrix-based APSP (faster) | APSP |   허용   |  Θ(V³ log V)   |

## 1. 최단 경로 문제
- **최단 경로 문제**
	- 가중치가 있는 방향 그래프 $G$에서
	- 두 정점 출발점 $s$로부터 목적지 $v$까지 **가중치 합이 최소인 경로**를 찾는 문제이다.

### 1.1. 핵심 속성
- **최적 부분구조(Optimal Substructure)**
	- 최단 경로의 부분 경로도 반드시 최단 경로다.
	- (귀류법으로 증명: 더 짧은 부분 경로가 있다면 전체 경로가 최단이 아니게 됨)

- **삼각 부등식(Triangle Inequality)**
	- δ(u,v) ≤ δ(u,x) + δ(x,v)

- **Relaxation(완화)**
	- 모든 v에 대해 δ(s,v)의 상한값 d[v]를 유지하며 개선
```
Relax(u,v,w):    if d[v] > d[u] + w then d[v] = d[u] + w
```

### 1.2. 최단 경로의 최적 부분구조 증명
#### (1) 정리
- 그래프 $G$에서 정점 $s$로부터 정점 $t$까지의 최단 경로를 $P$라 할 때,
	- $P = P_1 + (u, v) + P_2$ 로 분해할 수 있다.
    - $P_1$: $s$에서 $u$까지의 subpath
    - $(u, v)$: 경로 $P$ 위에 있는 임의의 간선
    - $P_2$: $v$에서 $t$까지의 subpath

#### (2) 명제
- **원래 명제**: subpath $P_1\text{은 } s\text{에서 } u\text{까지의 최단 경로이다.}$
- **그 부정**: subpath $P_1\text{은 } s\text{에서 } u\text{까지의 최단 경로가 아니다.}$

#### (3) 증명 (귀류법)
- subpath $P_1$이 $s$에서 $u$까지의 최단 경로가 아니라고 가정하였으므로,
    - $w(P_1') < w(P_1)$을 만족하는 $s$에서 $u$까지의 경로 $P_1'$이 존재한다.

- $P$의 총 가중치
	- $w(P) = w(P_1) + w(u, v) + w(P_2)$
- 새로운 경로의 총 가중치
	- $w(P') = w(P_1') + w(u, v) + w(P_2)$

- 이제 가중치를 비교하면
    - $w(P') = w(P_1') + w(u, v) + w(P_2) < w(P_1) + w(u, v) + w(P_2) = w(P)$
    - 즉 $w(P') < w(P)$가 성립한다.

- 그러나 이는 $P$가 $s$에서 $t$까지의 최단 경로라는 처음의 전제에 모순된다.
    - 따라서 $P_1$은 $s$에서 $u$까지의 최단 경로여야 한다.
    - 같은 논리로 $P_2$도 $v$에서 $t$까지의 최단 경로임을 증명할 수 있다. $\blacksquare$

## 2. Single-Source Shortest Path
> **하나의 시작 정점**에서 다른 모든 정점까지의 최단 경로

### 2.1. Dijkstra's Algorithm
> **Prim**은 트리 자체를 키우는 데 집중 → 간선 비용 최소화
> **Dijkstra**는 출발점으로부터의 거리를 추적 → 경로 비용 최소화

> **Data Structure**: **우선순위 큐**(Priority Queue)
> **key 값**: 현재 트리와 노드를 잇는 **edge의 가중치** vs. 출발점으로부터의 **누적 거리**

![|200](https://upload.wikimedia.org/wikipedia/commons/5/57/Dijkstra_Animation.gif)

- **특징**
	- 음수 가중치 없을 때 사용
		- *간선 가중치가 양수이므로, dist 최소인 v를 거치지 않고 더 짧은 경로를 만들 수 없음*
	- 그리디 방식

- 두 집합 유지
	- S(확정된 노드)
	- C(나머지 노드)

- $D[v]$가 최소인 노드를 매번 S에 추가하며 인접 노드의 D 값을 갱신

#### (1) Time Complexity
> $O(V)×T_\text{ExtractMin} + O(E)×T_\text{DecreaseKey}$

| 우선순위 큐 구현 |    ExtractMin     |  DecreaseKey   |             ==총 시간==              |
| :-------: | :---------------: | :------------: | :-------------------------------: |
|  ==배열==   |       O(V)        |      O(1)      |             $O(V^2)$              |
|   이진 힙    |      O(lg V)      |    O(lg V)     | $O(V\lg{V}+E\lg{V}) ≈ O(E\lg{V})$ |
|  피보나치 힙   | O(lg V) amortized | O(1) amortized |          $O(V\lg{V+E})$           |

#### (2) Pseudocode
```c
// 인접 행렬 + 배열 기반 구현
Dijkstra(L[1..n, 1..n]) {
    C = {2, 3, ..., n} // O(v): 미확정 노드 집합 초기화
    for (i=2 to n) {   // O(v-1)
	    D[i] = L[1,i]  // O(1)
	}
	// [상한 성질 시작] 초기화 직후부터 항상 D[v] ≥ δ(s,v)
	
    repeat (n-2) times { // O(v-2)
		// 최단 거리 확정 성질 (탐욕적 선택)
        v = C에서 D[v]가 최소인 노드 // O(v): 최솟값 탐색 (배열에서는 C를 선형 탐색)
		// [수렴 성질]
		// 음의 간선 X + 상한 성질 ⇒ 빼는 순간 D[v]=δ(s,v)로 확정되고 영구 유지
        C = C - {v} // O(1)
        for (each w in C) {     // O(v-1) 반복
            D[w] = Min(D[w], D[v]+L[v,w]) // O(1): 누적 거리 비교
            // [상한 유지] Relax는 값을 줄이는 방향뿐 ⇒ D[w] ≥ δ(s,w) 불변
		}
	}
	
    return D // O(1)
}
```

> Compared to Prim: edge 가중치만 비교 vs. 누적 거리 비교

### 2.2. Bellman-Ford Algorithm
![|200](https://upload.wikimedia.org/wikipedia/commons/7/77/Bellman%E2%80%93Ford_algorithm_example.gif)

- **특징**
	- 음수 가중치 허용
	- 음수 사이클 검출 가능

#### (1) Pseudocode
- **Time Complexity**: $O(VE)$
```c
BellmanFord() {
	for each v ∈ V { // O(V): 모든 정점 초기화
		d[v] = ∞
	}
	d[s] = 0 // O(1): 시작 정점 거리 설정
	// [상한 성질]
	// (현재 추정 최단거리) d[v] ≥ δ(s,v) (실제 최단거리)인 상한 성질이 항상 성립한다.
	
	// 이중 루프 합계: **O(V·E)**
	for (i=1 to V-1) { // O(V): V-1 라운드
		// [경로완화 성질]
		// 최단 경로 s→v₁→…→vₖ의 간선을 순서대로 릴렉스하면 d[vₖ]=δ(s,vₖ) 확정되는 경로 완화 성질
		for (each edge (u,v) ∈ E) { // O(E): 모든 간선 순회
			Relax(u, v, w(u,v));    // O(1): 거리 갱신 시도
			// [수렴 성질]
			//             (u,v)가 어떤 최단 경로 위의 간선이고,
			// 호출 전 d[u]=δ(s,u)였다면,
			// 호출 후 d[v]=δ(s,v)가 되어 영구 확정
		}
	}
	
	// V-1 라운드 후에도 더 줄면 간선 수 V개 이상 → 음수 사이클
	for (each edge (u,v) ∈ E) {   // O(E): 모든 간선 재확인
		if (d[v] > d[u]+w(u,v)) { // O(1): 여전히 릴렉스 가능?
			return "no solution"
		}
	}
}

Relax(u, v, w) {
	if (d[v] > d[u] + w) { // 더 짧은 경로를 발견하면
		d[v] = d[u] + w;   // 갱신
	}
}
```

## 3. Minimum Steiner Tree (보조 주제)
- **문제**
	- 그래프 G에서 부분집합 S를 모두 포함하는 최소 비용 연결 부분 그래프(트리) 찾기
	- **NP-complete**

- k=2일 때 → 최단 경로 문제, k=n일 때 → 최소 스패닝 트리 문제

- **응용**: 멀티캐스팅 (한 노드에서 선택된 노드 그룹으로 데이터 전송)

- **TM 알고리즘** (Takahashi & Matsuyama, 1980): 근사 해법
    1. 소스 노드 s만으로 부분트리 T₁ 구성
    2. T_i에 가장 가까운 멤버 노드를 찾아 최소 비용 경로로 연결
    3. 모든 멤버가 포함될 때까지 반복

## 4. All-Pairs Shortest Path (APSP)
> **모든 정점 쌍** 사이의 최단 경로

- 모든 정점 쌍 (u, v)에 대한 최단 경로 거리 행렬 D를 구한다.

### 4.1. 단순 접근법 비교
|            방법            |         시간 복잡도          |
| :----------------------: | :---------------------: |
|   Bellman-Ford를 V번 실행    | O(V²E) = O(V⁴) (밀집 그래프) |
|  Dijkstra를 V번 실행 (이진 힙)  |       O(VE log V)       |
| Dijkstra를 V번 실행 (피보나치 힙) |    O(EV + V² log V)     |
|          **목표**          | $O(V^3)$ (특별한 자료구조 없이)  |

### 4.2. 동적 계획법 기반 행렬 곱셈 방식
- **점화식**
	- $l_{ij}^{(m)} = \text{min}(l_{ik}^{(m-1)} + w_{kj})\quad(1 ≤ k ≤ n)$
	- 최대 m개 간선을 사용하는 i→j 최단 경로의 가중치
	- 행렬 곱셈과 유사한 구조 (min ↔ +, + ↔ •)

- **SLOW-APSP**
	- L⁽¹⁾ = W부터 L⁽ⁿ⁻¹⁾까지 순차 계산 → **Θ(n⁴)**

- **FASTER-APSP**
	- 반복 제곱법(L⁽²ᵐ⁾ = L⁽ᵐ⁾ · L⁽ᵐ⁾)을 사용 → **Θ(n³ log n)**

#### cf. 점화식
- 현재 알고 있는 거리: $dist[i][j]$

- 새로운 중간 정점 `k`를 허용했을 때:
	- `i → j` 직접 가는 게 짧을까?
	- 아니면 `i → k → j`가 짧을까?

- 즉: $dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])$

### 4.3. Floyd-Warshall Algorithm
<iframe src="https://ken-jeong.github.io/pages/floyd_warshall.html" width="100%" height="700"></iframe>

- **핵심 아이디어**
	- 중간 정점을 {1, 2, ..., k}에서만 선택할 수 있다고 제한했을 때의 최단 경로를 점진적으로 확장

- 음수 간선 허용 (단, 음수 사이클은 없어야 함)
- 선행자 행렬 Π를 함께 갱신하면 실제 경로 복원 가능

- **점화식**
$$d_{ij}^{(k)} = \begin{cases} w_{ij} & \text{if } k = 0 \ \min(d_{ij}^{(k-1)},\ d_{ik}^{(k-1)} + d_{kj}^{(k-1)}) & \text{if } k \geq 1 \end{cases}$$

- **Case 1**. k가 경로 p의 중간 정점이 아님
	- → $d_{ij}^{(k)} = d_{ij}^{(k-1)}$

- **Case 2**. k가 중간 정점임
	- → $d_{ij}^{(k)} = d_{ik}^{(k-1)} + d_{kj}^{(k-1)}$

```c
d_ij^(k) = min(d_ij^(k-1), d_ik^(k-1)+d_kj^(k-1))
```

#### (1) Complexity
- **Time Complexity**
	- $O(n^3)$

- *Space Complexity*
	- (각 k마다 행렬 저장 시) $O(n^3)$
	- (in-place 최적화 시) $O(n^2)$

#### (2) Pseudocode
```c
FLOYD_WARSHALL(W) {
    n = rows[W] // O(1)
    D⁽⁰⁾ = W    // O(n²): n×n 행렬 복사
    
    for k = 1 to n:         // 경유 노드, O(n) ┐
        for i = 1 to n:     // 출발 노드, O(n) ├─ 전체 O(n³)
            for j = 1 to n: // 도착 노드, O(n) ┘
                d_ij^(k) = min(d_ij^(k-1), d_ik^(k-1)+d_kj^(k-1)) // O(1)
                // 또는
                dist[i][j] = min(dist[i][j], dist[i][k]+dist[k][j])
    return D⁽ⁿ⁾
}
```
