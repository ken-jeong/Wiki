---
aliases: []
type: Lecture
tags:
  - 2-1/자료구조
draft: false
date: 2025-04-22
---
1. 트리의 정의: [[08_Tree (이산수학)]] (트리, 이진 트리, 힙 트리, BST, BBST)
	- 이진 힙 트리를 활용한 [[03_Recurrence & Heap#5. 우선순위 큐|우선순위 큐]]
2. 트리의 구현: [[07_Tree (자료구조)]] (표현, 연산, Traversal)
3. AVL 트리: [[06_BST & AVL Trees]] (BST 연산, AVL 상세)

## 1. 트리의 구현
### 1.1. 트리의 표현
1. **N-링크 표현**
	- 노드가 최대 n개의 자식을 가질 수 있도록 n개의 링크 필드 사용
	- (링크 수 예측이 어려움)

2. **왼쪽 자식-오른쪽 형제 표현**
	- 각 노드가 두 개의 포인터만 가짐
	- **왼쪽 포인터**: 첫 번째 자식 노드
	- **오른쪽 포인터**: 다음 형제 노드

3. **기타 표현 방법**
	- **중첩된 집합**: 노드를 포함 관계로 나타냄
	- **중첩된 괄호**: `(A (B (E)(F)(G(K))) (C(H)) (D(I)(J)))`
	- **들여쓰기(indentation)**: 계층 구조를 들여쓰기로 표현

## 2. 이진 트리의 구현
### 2.1. 이진 트리의 표현
- **배열 표현** (1-based index)
	- 완전 이진 트리를 기준으로 인덱스 번호를 배열 인덱스로 사용
		- parent index(`i/2`)
		- left child index(`2i`)
		- right child index(`2i+1`)
	- 단점: 경사 이진 트리의 경우 빈 칸이 많이 발생 → 메모리 낭비

- **연결 리스트 표현**
	- 노드 구조: left child link + data + right child link
```c
typedef struct TNode {
    TElement data;
    struct TNode* left;
    struct TNode* right;
} TNode;
```

### 2.2. 이진 트리의 순회(Traversal)
- **트리 순회** (Tree Traversal)
	- 트리의 모든 노드를 한 번씩 방문하는 작업이다.
	- 시간 복잡도: $\Theta(n)$

| 방식  |     순회      | 표기  | 특징                       | 주요 용도               |
| :-: | :---------: | :-: | ------------------------ | ------------------- |
| DFS |  Pre-order  | VLR | 루트 노드를 먼저 방문             | 트리 구조 복사, 재귀적 문제 해결 |
| DFS |  In-order   | LVR | 이진 탐색 트리에서 정렬된 순서로 방문    | 이진 탐색 트리에서 값 정렬     |
| DFS | Post-order  | LRV | 자식 노드를 먼저 방문한 후 부모 노드 처리 | 트리 삭제, 메모리 해제       |
| BFS | Level-order |  -  | 너비 우선 탐색, 큐를 사용하여 구현     | 최단 경로 탐색, 넓이 기반 탐색  |

#### (1) 전위 순회 (Preorder; VLR)
- 루트 노드 $\rightarrow$ 왼쪽 서브트리 $\rightarrow$ 오른쪽 서브트리
![|400](https://imgur.com/tqoB8Ce.png)
```
preorder(n)
    if n ≠ NULL :
        VisitNode(n)         // 루트 노드 처리
        preorder(n.left)     // 왼쪽 서브 트리 처리
        preorder(n.right)    // 오른쪽 서브 트리 처리
```

```c
void preorder(TNode* n) {
	if (n == NULL) return; // Early Return
	VisitNode(n);
	preorder(n->left);
	preorder(n->right);
}
```

#### (2) 중위 순회 (Inorder; LVR)
- 왼쪽 서브트리 $\rightarrow$ 루트 노드 $\rightarrow$ 오른쪽 서브트리
- ==(결과가 오름차순으로 정렬됨)==
- $T(n) = T(k) + T(n-k-1) + d;\quad T(0)=c$
![|400](https://imgur.com/2s0Wowm.png)
```
inorder(n)
    if n ≠ NULL :
        inorder(n.left)      // 왼쪽 서브 트리 처리
        VisitNode(n)         // 루트 노드 처리
        inorder(n.right)     // 오른쪽 서브 트리 처리
```

```c
void inorder(TNode* n) {
	if (n == NULL) return; // Early Return
	inorder(n->left);
	VisitNode(n);
	inorder(n->right);
}
```

#### (3) 후위 순회 (Postorder; LRV)
- 왼쪽 서브트리 $\rightarrow$ 오른쪽 서브트리 $\rightarrow$ 루트 노드
![|400](https://imgur.com/YqCYNMn.png)
```
postorder(n)
    if n ≠ NULL :
        postorder(n.left)    // 왼쪽 서브 트리 처리
        postorder(n.right)   // 오른쪽 서브 트리 처리
        VisitNode(n)         // 루트 노드 처리
```

```c
void postorder(TNode* n) {
	if (n == NULL) return; // Early Return
	postorder(n->left);
	postorder(n->right);
	VisitNode(n);
}
```

#### (4) 레벨 순회 (Level Order)
- 큐(Queue)를 사용하여 구현
```
levelorder(root)
    if root ≠ NULL :
        init_queue()
        enqueue(root)
        while not is_empty(queue) :
            n ← dequeue()
            if n ≠ NULL :
                VisitNode(n)
                enqueue(n.left)
                enqueue(n.right)
```

```c
void levelorder(TNode* root) {
	if (root == NULL) return; // Early Return
	init_queue();
	enqueue(root);
	while (!is_empty()) {
		TNode* n = dequeue();
		if (n != NULL) {
			VisitNode(n);
			enqueue(n->left);
			enqueue(n->right);
		}
	}
}
```

### 2.3. 이진 트리 관련 문제
#### (1) 노드 개수 구하기
- `1 + 왼쪽 서브트리 노드 수 + 오른쪽 서브트리 노드 수` (후위 순회 응용)
```
count_node(n)
    if n = NULL : return 0
    n_left ← count_node(n.left)     // 왼쪽 서브 트리 처리
    n_right ← count_node(n.right)   // 오른쪽 서브 트리 처리
    return 1 + n_left + n_right      // 루트 노드 처리
```

```c
int count_node(TNode* n) {
    if (n == NULL) return 0;
    else return 1 + count_node(n->left) + count_node(n->right);
}
```

#### (2) 트리의 높이 구하기
- `1 + max(왼쪽 서브트리 높이, 오른쪽 서브트리 높이)` (후위 순회 응용)
```
calc_height(n)
    if n = NULL : return 0
    else : return 1 + max(calc_height(n.left), calc_height(n.right))
```

```c
int calc_height(TNode* n) {
    if (n == NULL) return 0;
    else return 1 + MAX(calc_height(n->left), calc_height(n->right));
}
```

#### (3) 트리를 좌우로 대칭시키기
- 좌우 자식 노드의 포인터를 교환한 후, 서브 트리에 대해 재귀 호출 (전위 순회 응용)
```
reverse(n)
    if n ≠ NULL :
        n.left ↔ n.right         // 루트 처리: 좌우 서브 트리 교환
        reverse(n.left)           // 왼쪽 서브 트리 처리
        reverse(n.right)          // 오른쪽 서브 트리 처리
```

```c
void reverse(TNode* p) {
    if (p != NULL) {
        TNode* tmp = p->left;
        p->left = p->right;
        p->right = tmp;
        reverse(p->left);
        reverse(p->right);
    }
}
```

#### (4) 노드의 레벨 구하기 (전위 순회)
- 타겟(key)을 찾을 때까지 내려가며 level 값을 1씩 증가시킴
```
calc_level(n, key, level)
    if n = NULL : return 0
    if n = key : return level       // 레벨이 결정됨 (> 0)
    lev ← calc_level(n.left, key, level+1)   // 왼쪽 서브 트리 처리
    if lev > 0 : return lev
    else: return calc_level(n.right, key, level+1)  // 오른쪽 서브 트리 처리
```

```c
int calc_level(TNode* n, TNode* key, int level) {
    if (n == NULL) return 0;           // 찾는 노드 key가 없음
    if (n == key) return level;        // 노드 key 찾음. level 반환
    int l = calc_level(n->left, key, level + 1);
    if (l > 0) return l;
    return calc_level(n->right, key, level + 1);
}
```

## 3. 이진 힙 트리의 구현
### 3.1. 힙의 표현
- **이진 힙**(Binary Heap)
	- 배열 객체이지만, 거의 완전 이진 트리(Nearly complete binary tree)로 간주할 수 있다.

- **속성**
	- `length[A]`: 배열에 있는 요소의 총 개수
	- `heap-size[A]`: 배열 내에 저장된 '힙' 요소의 개수 (`heap-size[A] <= length[A]`)

- **인덱스 계산 (1-based index 기준)**
	- `PARENT(i)` = $\lfloor i / 2 \rfloor$
	- `LEFT(i)` = $2i$
	- `RIGHT(i)` = $2i + 1$

- **최대 힙 (Max-heap)**
	- 루트를 제외한 모든 노드 $i$에 대해 **$A[\text{PARENT}(i)] \ge A[i]$** 를 만족

### 3.2. 힙의 연산
- 완전 이진 트리의 구조를 지키기 위한 연산
    - **삽입**(up-heap): 마지막 노드에 추가 후 위로 조정
    - **삭제**(down-heap): 루트 제거 후 마지막 노드를 루트로 이동, 아래로 조정

#### (1) 힙의 삽입 연산 (Up-heap)
- 마지막 노드 위치에 삽입 → 부모와 비교하며 위로 이동
```
heap_push(n)
    heap_size ← heap_size + 1
    i ← heap_size
    A[i] ← node
    while i ≠ 1 :
        if KEY(i) > KEY(PARENT(i)) :
            A[i] ↔ A[PARENT(i)]
            i ← PARENT(i)
        else : break
```
- 시간 복잡도: $O(\lg{n})$

#### (2) 힙의 삭제 연산 (Down-heap)
- 루트 노드를 삭제 → 마지막 노드를 루트로 이동 → 아래로 내려가며 정렬
```
heap_pop()
    root ← A[1]              // 삭제할 루트 노드 저장
    A[1] ← A[heap_size]     // 말단 노드를 루트에 복사
    heap_size ← heap_size - 1
    i ← 1                   // 루트의 위치
    while LEFT(i) ≤ heap_size :  // 자식 노드가 남아 있을 때까지
        if LEFT(i) < heap_size and KEY(LEFT(i)) > KEY(RIGHT(i)) :
            child ← LEFT(i)     // 왼쪽이 크면 왼쪽 자식 위치
        else : child ← RIGHT(i) // 오른쪽이 크면 오른쪽 자식 위치

        if KEY(i) > KEY(child) : break  // 자식보다 크면 제자리 찾았음
        else :
            A[i] ↔ A[child]    // 자식과 교환
            i ← child          // 자식 위치로 내려옴
    return root                 // 저장해 둔 루트를 반환
```
- 시간 복잡도: $O(\lg{n})$

## 4. BST의 구현
### 4.1. BST의 연산
#### Search
- 탐색값이 키값보다 작으면 왼쪽, 크면 오른쪽으로 이동
```
search(root, key)
    if root = NULL : return NULL
    if KEY(root) = key : return root        // 루트의 키가 key와 같으면 탐색 성공
    else if KEY(root) < key :
        return search(root.left, key)       // 왼쪽 서브 트리 탐색
    else :
        return search(root.right, key)      // 오른쪽 서브 트리 탐색
```

#### Insert
- 탐색 연산 후 적절한 위치에 노드 추가
```
insert(root, n)
    if KEY(n) < KEY(root) :       // root보다 키가 작으면 → 왼쪽
        if root.left = NULL :
            root.left ← n        // n이 왼쪽 자식
        else insert(root.left, n) // 있으면 왼쪽 서브 트리에 삽입
    else if KEY(n) > KEY(root) :  // root보다 키가 크면 → 오른쪽
        if root.right = NULL :
            root.right ← n       // n이 오른쪽 자식
        else : insert(root.right, n)  // 있으면 오른쪽 서브 트리에 삽입
    else :                        // 중복된 키가 있음.
        delete_node(n)            // 노드 n 삭제
```

#### Delete
> 재귀적 방식으로 구현하면
> 트리의 구조와 재귀의 특성(하위 문제의 해결)이 딱 맞아떨어지기 때문에
> 코드가 매우 직관적이고 깔끔해진다.

- Case 1: **자식 0개** 단말 노드의 삭제
	- 해당 노드를 삭제하고 부모 노드의 링크를 NULL로 변경
- Case 2: **자식 1개** 노드의 삭제
	- 해당 노드를 삭제하고 부모 노드의 링크를 자식 노드로 변경
- Case 3: **자식 2개** 노드의 삭제
	- **후계자 노드**를 이용
	    - 왼쪽 서브 트리에서 가장 큰 노드, 또는
	    - 오른쪽 서브 트리에서 가장 작은 노드
	- 후계자의 데이터를 삭제할 노드에 복사하고, 실제로는 후계자 노드를 삭제

```
delete(root, key)
    n ← 삭제할 노드
    parent ← n의 부모 노드
    if n = NULL : return root

    // case1: n이 단말 노드인 경우
    if n.left=NULL and n.right=NULL :
        if parent=NULL : root ← NULL
        else if parent.left = n : parent.left ← NULL
        else : parent.right ← NULL

    // case2: n이 하나의 자식만 갖는 경우
    else if n.left=NULL or n.right = NULL :
        child ← n의 유일한 자식
        if parent = NULL : root ← child
        else if parent.left = n : parent.left ← child
        else : parent.right ← child

    // case3: n이 양쪽 자식을 모두 갖는 경우
    else :
        succ ← n.right          // 오른쪽
        while succ.left ≠ NULL :
            succ ← succ.left
        n.data ← succ.data      // 노드의 데이터 복사
        n.right ← delete(n.right, KEY(succ))

    return root
```
