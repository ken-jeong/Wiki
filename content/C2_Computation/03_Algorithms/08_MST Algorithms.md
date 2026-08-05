---
aliases: []
type: Lecture
tags:
  - 3-1/알고리즘
draft: false
date: 2026-04-28
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

## 1. MST
- [[07_Graph (이산수학)#3.2. 신장 트리]]
- [[07_Graph (이산수학)#3.3. MST]]

### 1.1. MST의 최적 부분구조 증명
#### (1) 정리
- **최적 부분구조** (Optimal Substructure)
	- 어떤 문제의 최적해가 그 부분 문제들의 최적해로 구성되는 성질
	- 이 성질이 성립해야 그리디 알고리즘(Kruskal, Prim)이 올바르게 작동한다.

- 그래프 $G$의 MST $T$에서 임의의 간선 $(u, v)$를 제거하였을 때,
	- $T_1$, $T_2$는 각각 간선 제거 후 얻은 부분 트리이고,
	- $G_1$​, $G_2$​는 각각 $T_1$, $T_2$가 포함하는 정점 집합으로 유도되는 부분 그래프이다.

#### (2) 명제
- 원래 명제: $T_1\text{은 } G_1\text{의 MST이다.}$
- 그 부정: $T_1\text{은 } G_1\text{의 MST가 아니다.}$

#### (3) 증명 (귀류법)
- $T_1$이 $G_1$의 MST가 아니라고 **가정**하였으므로, 
	- $w(T_1') < w(T_1)$을 만족하는 $G_1$의 신장 트리 $T_1'$이 존재한다.

- 새로운 트리 총 가중치 $w(T') = w(T_1') + w(u, v) + w(T_2)$
- $T$의 총 가중치 $w(T) = w(T_1) + w(u,v) + w(T_2)$

- 이제 가중치를 비교하면
	- $w(T') = w(T_1') + w(u, v) + w(T_2) < w(T_1) + w(u, v) + w(T_2) = w(T)$
	- 즉 $w(T') < w(T)$가 성립한다.

- 그러나 이는 $T$가 $G$의 MST라는 처음의 전제에 모순된다.
	- 따라서 $T_1$은 $G_1$의 MST여야 한다.
	- 같은 논리로 $T_2$도 $G_2$의 MST임을 증명할 수 있다. $\blacksquare$

### 1.2. MST 성장 알고리즘
- **핵심 아이디어**
	- 안전한 간선(safe edge)만 추가하여 MST를 점진적으로 구성

- **루프 불변식**
	- 집합 A는 항상 어떤 MST의 부분 집합

- **안전한 간선 찾기 관련 정의**

|         용어         |                  정의                  |
| :----------------: | :----------------------------------: |
|      cut (컷)       |    정점을 두 개의 분리된 집합으로 분할 ($S,V-S$)    |
|   respects (존중)    |       A의 어떤 간선도 컷을 가로지르지 않을 때        |
|    crosses (교차)    |    간선의 한 끝점이 S, 다른 끝점이 V-S에 있을 때     |
| light edge (경량 간선) | 컷을 가로지르는 간선(cross edge) 중 가중치가 최소인 것 |

- **정리**
	- T가 G의 MST이고, A ⊆ T가 서브트리일 때,
	- A와 V-A를 연결하는 최소 가중치 간선 (u, v)는 반드시 T에 속한다.

### 1.3. Cut Property 증명
#### (1) 정리
- **Cut** (컷)
	- 그래프의 노드 집합 V를 두 그룹 **S**와 **V–S**로 나누는 분할 ($S,V-S$)
	- 이 분할 경계를 가로지르는 엣지를 **crossing edge**라고 한다.

- **Light Edge** (라이트 엣지)
	- crossing edge들 중에서 **가중치가 가장 작은 엣지**

- **Cut Property** (컷 속성)
	- 그래프 $G$의 어떤 컷 $S,V-S$에 대해서, light edge $e$는 반드시 MST에 포함된다.
	- *매 단계에서 light edge를 고르는 것이 항상 최적임 - 이 성질이  Kruskal과 Prim 알고리즘의 정당성을 뒷받침한다.*

#### (2) 명제
> 그래프 $G$에서 어떤 cut($S,V-S$)을 가로지르는 간선들 중 가중치가 최소인
- **원래 명제**: light edge $e$를 포함하는 MST가 적어도 하나 존재한다. (Cut Property)
- **그 부정**: light edge $e$를 포함하는 MST가 하나도 존재하지 않는다.

#### (3) 증명 (귀류법)
> 출제

- light edge $e$를 포함하는 $G$의 MST가 하나도 존재하지 않는다고 가정하였으므로,
	- $G$의 임의의 MST $T$는 $e$를 포함하지 않는다.
	- 따라서 $e$를 추가하면 반드시 사이클이 생긴다.

- 간선 $e=(u,v)\quad(u\in S,\quad v\in V\setminus S)$일 때,
	- 사이클이 있으면 cut을 가로지르는 $e$가 아닌 간선 $e' = (u', v')$가 적어도 하나 존재한다.
	- 여기서 $e$가 light edge이므로 정의에 의해 $w(e) \leq w(e')$

- 새로운 트리 $T' = T + {e} - {e'}$를 구성하자.
    - 사이클 위의 간선을 제거하면 스패닝 트리 유지

- 이제 가중치를 비교하면
    - $w(T') = w(T) - w(e') + w(e) \leq w(T)$
    - 즉 $w(T') \leq w(T)$가 성립한다.

- 그런데 $T$가 MST라는 전제로부터 $w(T') \geq w(T)$이므로 $w(T') = w(T)$이고,
	- 따라서 $T'$ 역시 MST이다.
    - 그러나 $T'$는 $e$를 포함하므로, "$e$를 포함하는 MST가 하나도 존재하지 않는다."는 가정에 모순된다.
    - 따라서 light edge $e$를 포함하는 MST가 반드시 존재한다. $\blacksquare$

### 1.4. Growing an MST
```c
GENERIC_MST(G, w) {
	A <- 0
	while A does not form a spanning tree {
		do find an edge (u, v) that is safe for A
		A <- A U {(u, v)}
	}
	return A
}
```

## 2. MST Algorithms
### 2.1. Kruskal's Algorithm
![|200](https://upload.wikimedia.org/wikipedia/commons/b/bb/KruskalDemo.gif)
> 간선 중심 (Edge-based)
> Data Structure: Disjoint Set (Union-Find)

- **동작 원리**
	- 간선을 가중치 오름차순으로 정렬 후,
	- 사이클을 만들지 않는 간선만 순서대로 선택

- **서로소 집합 (Disjoint Set) 활용**
	- `MakeSet(v)`: 단일 원소 집합 생성
	- `FindSet(x)`: x가 속한 집합 반환
	- `Union(Si, Sj)`: 두 집합 합병

#### Time Complexity
- $O(E\lg{E})$ *(간선 정렬이 지배적)*
- Edge의 수가 적은 희소 그래프(Sparse Graph; $E≈O(V)$)에서 효율적이다.

#### Pseudocode
```c
Kruskal() {
    T = ∅ // MST 간선 집합 T 초기화
    
    // O(V): 각 정점을 독립적인 집합으로 초기화
    for each v ∈ V { MakeSet(v) }
    
    // **O(E log E)**: 모든 간선을 가중치 오름차순으로 정렬 & 최소성 보장
    sort E by increasing weight w
    
    // O(V): 정렬된 간선을 하나씩 선택, V-1개 간선이 선택되면 종료
    for each (u, v) ∈ E (sorted) {
	    // 두 정점이 서로 다른 집합이면 사이클이 생기지 않음
        if FindSet(u) ≠ FindSet(v) { // O(E) ≈ O(E·α(V)) & 암시적 cut 정의
	        
            T = T ∪ {(u, v)} // MST에 간선 추가
            Union(FindSet(u), FindSet(v)) // O(V) - 두 집합을 하나로 합침
        }
    }
    return T // 완성된 MST 반환
}
```

- cf. $O(Eα(V))$ (역 아커만 함수는 사실상 4 이하의 상수)

#### Cut Property가 적용되는 방식
- 간선 e = (u, v)를 선택하는 순간, Union-Find 기준으로:
	- S = u가 속한 컴포넌트
	- V-S = 나머지 모든 정점

- 이 cut을 가로지르는 간선 중 아직 처리 안 된 것들은 모두 w(e) 이상
	- → e는 이 cut의 최소 간선
	- → Cut Property에 의해 MST에 포함

### 2.2. Prim's Algorithm
![|200](https://upload.wikimedia.org/wikipedia/commons/9/9b/PrimAlgDemo.gif)
> 정점 중심 (Vertex-based)
> Data Structure: **우선순위 큐**(Priority Queue)

- **동작 원리**
	- 임의의 루트에서 시작하여,
	- 현재 트리와 연결되는 최소 가중치 간선을 반복적으로 선택해 트리를 확장

#### Time Complexity
> $O(V)×T_\text{ExtractMin} + O(E)×T_\text{DecreaseKey}$

| 우선순위 큐 구현 |    ExtractMin     |  DecreaseKey   |            ==총 시간==             |
| :-------: | :---------------: | :------------: | :-----------------------------: |
|    배열     |       O(V)        |      O(1)      |            $O(V^2)$             |
| ==이진 힙==  |      O(lg V)      |    O(lg V)     | $O(V\lg{V}+E\lg{V})≈O(E\lg{V})$ |
|  피보나치 힙   | O(lg V) amortized | O(1) amortized |         $O(V\lg{V+E})$          |

#### Pseudocode
```c
// 이진 힙 기준 $O(V lg V) + O(E lg V)$
Prim(G, w, r) {
	Q = V[G];          // 모든 정점을 우선순위 큐에 삽입: 빌드 힙 O(V) & 명시적 cut 정의
	for (each u ∈ Q) { // O(V): 키(최소 연결 비용)를 무한대로 초기화
		key[u] = ∞;
	}
	
	key[r] = 0;  // O(1): 시작 정점 r의 키를 0으로 설정
	p[r] = NULL; // O(1): 시작 정점 r의 부모 없음
	
	while (Q not empty) {  // O(V)   : 각 정점이 한 번씩 추출됨
		u = ExtractMin(Q); // O(lg V): 힙에서 최솟값 추출 & 최소성 보장
		
		for (each v ∈ Adj[u]) { // u의 차수만큼 반복: 전체 **O(2E)**
			// v가 아직 MST에 포함되지 않았고, 더 작은 가중치 간선이 존재하면
			if ((v ∈ Q) and (w(u, v) < key[v])) { // O(1)			
				p[v] = u; // O(1): v의 부모를 u로 갱신 (MST 간선 기록)
				key[v] = w(u, v); // **O(lg V)**: 힙 감소 연산
			}
		}
	}
}
```

#### Cut Property가 적용되는 방식
- 매 단계마다:
	- S = 현재 MST에 포함된 정점들
	- V-S = 아직 포함되지 않은 정점들

- 이 cut을 가로지르는 간선 중 **최소 가중치 간선**을 선택
	- → Cut Property에 의해 MST에 포함
	- 이를 모든 정점이 포함될 때까지 반복
