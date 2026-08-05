---
aliases: []
type: Lecture
tags:
  - 2-1/자료구조
draft: false
date: 2025-04-01
---
1. [[02_Array & Struct]]
	- C 배열: [[08_Arrays & Strings]]
	- C 구조체: [[17_struct, union, enum]]
	- 자료구조 배열
	- 응용: 희소 행렬, 다항식

2. [[06_List]]
	- C 포인터/동적 할당
	- 연결 구조: [[05_Pointer & Linked Structure]]
	- 단순 연결 리스트
	- 이중 연결 리스트

## 1. 포인터
- **포인터**(Pointer)
	- 메모리의 주소를 저장하는 변수
	- **반드시 NULL로 초기화**![[03_변수와 데이터 입력#^garbage]]

- **연산자**
	- `*` (역참조 연산자): 포인터가 가리키는 주소의 실제 값을 가져온다.
	- `&` (주소 연산자): 변수의 주소값을 가져온다.
![[09_포인터#^operator]]

- **이중 포인터**: 포인터 변수의 주소를 저장하는 포인터 (`int **pp`)
```c
int i = 10;
int* p = &i;
int** pp = &p;
```

### 1.1. 배열과 포인터
- **배열과 포인터**: 배열의 이름은 배열의 시작 주소를 가리키는 상수 포인터 역할
![[10_배열과 포인터#^arrayname]]
```c
int A[5], *p=NULL; // 배열 A와 포인터 p 선언 (반드시 NULL로 초기화)
p = A;
p[3] = 20; // p[3]은 A[3]과 동일
```

### 1.2. 구조체와 포인터
- **구조체 포인터**: `->` 연산자를 통해 구조체 멤버에 접근 (예: `p->data`)
![[17_struct, union, enum#^operator]]
```c
typedef struct {
	int degree;
	float coef[MAX_DEGREE];
} Polynomial;

Polynomial a;
Polynomial* p = NULL;
p = &a; // 이제 p가 구조체 a를 가리킴

a.degree = 5; // 구조체 a를 통한 멤버 접근: . 연산자 사용
p->coef[0] = 1; // 포인터 p를 통한 멤버 접근: -> 연산자 사용
```

- **자체 참조 구조체**(self-referential structure)
	- 자기 자신과 같은 형태의 구조체를 가리키는 포인터를 멤버로 갖는 구조체
	- (연결 리스트의 노드 구현 시 필수)
	- [[17_struct, union, enum#2.3. 자기 참조 구조체 (17-9.c)]]
```c
typedef struct ListNode {
	char data[10];
	struct ListNode* link; // 자신과 같은 구조체를 가리킬 포인터
} Node;
```

## 2. 동적 할당
### 2.1. C 동적 할당 복습
- **동적 메모리 할당**(Dynamic Memory Allocation)
	- 프로그램 실행 중 필요한 만큼 메모리를 운영체제로부터 할당받고, 사용 후 반납하는 기능

- **주요 함수 (`stdlib.h`)**
	- `malloc()`: 바이트 단위로 메모리 할당
	- `free()`: 할당된 메모리 반납 (해제)
	- `realloc()`: 이미 할당된 메모리의 크기를 조절 (기존 데이터 유지)

```c
void* malloc(size_t size); // 메모리 요청
// 성공 시: 할당된 메모리 블록의 시작 주소 (`void*`)
// 실패 시: `NULL` (null check 필요)

void* calloc(size_t num, size_t size);
// 성공 시: 할당된 메모리 블록의 시작 주소 (`void*`), 모든 바이트는 0으로 초기화
// 실패 시: `NULL`

void* realloc(void* ptr, size_t new_size); // 메모리 크기 변경
// 성공 시: 재할당된 메모리 블록의 시작 주소 (`void*`) – 위치가 바뀔 수 있음
// 실패 시: `NULL` (원래의 메모리는 그대로 유지됨)

void free(void* ptr); // 할당한 메모리 반납
// 반환값 없음 (`void`)
```

- *cf. 메모리 누수*
	- 동적으로 할당한 메모리를 필요 없어진 시점에 적절히 해제하지 않으면 그 메모리는 계속 차지됨
	- 계속해서 메모리를 할당하면서 해제는 하지 않으면 전체 사용 가능 메모리가 줄어듦

![[22_명령어#cf. RAM 메모리 구조]]
![[04_객체 포인터와 객체 배열, 객체의 동적 생성#cf. 세그폴트]]

### 2.2. 동적 할당을 이용한 배열 구조의 스택
- 기존의 고정 크기 배열 스택의 단점을 보완

- **초기화**: `malloc`을 사용해 초기 용량만큼 배열을 생성

- **삽입(`push`) 연산 개선**
	- 스택이 꽉 찼을 경우(`is_full`), `realloc`을 사용하여 배열의 크기(`MAX_SIZE`)를 2배로 늘린 뒤 데이터를 저장
	- 이를 통해 오버플로우 문제를 해결

## 3. 연결된 구조
![[01_Data Structures & Algorithms#2.2. 저장 방식에 따른 분류]]

- **개념**
	- 자료들을 메모리의 연속된 공간에 두지 않고,
	- 여기저기 흩어진 자료들을 **포인터**(링크)로 연결하여 관리하는 방식

- **배열 구조 vs 연결된 구조 비교**
	- **접근 속도**: 배열은 $O(1)$ (인덱스로 즉시 접근), 연결 구조는 $O(n)$ (순차 접근)
	- **용량**: 배열은 고정(낭비되거나 부족), 연결 구조는 동적(필요한 만큼만 사용)
	- **중간 삽입/삭제**: 배열은 데이터 이동 필요($O(n)$), 연결 구조는 링크만 변경하면 됨($O(1)$, 위치를 아는 경우)

- **용어**
	- **노드**(Node): 데이터 필드(값) + 링크 필드(다음 노드의 주소)
	- **헤드 포인터**(Head Pointer): 첫 번째 노드의 주소를 저장하는 포인터
	
	- head pointer는 head node를 가리킴
		- 스택의 `top`, 큐의 `front`, 원형 단순 연결된 큐의 `rear의 link field`
	- tail pointer는 tail node를 가리킴(rear)
		- 큐의 `rear`

### 3.1. 단순 연결된 구조
- **단순 연결된 구조**(Singly Linked)
	- **node**: ==data field== + ==link field== → (후속 node)
	- `tail node의 link field`는 `NULL`을 가리킴

- **원형 단순 연결된 구조**(Circular Linked)
	- `tail node의 link field`는 NULL대신 `head node`를 가리킴

### 3.2. 이중 연결된 구조
- **이중 연결된 구조**(DoublyLinked)
	- **node**: (선행 node) ← ==link field== + ==data field== + ==link field== → (후속 node)
	- `tail node의 link field`는 `NULL`을 가리킴

- **원형 이중 연결된 구조**(Circular Linked)
	- `tail node의 link field`는 NULL대신 `head node`를 가리킴

## 4. 단순 연결 구조 응용: 연결된 스택
- **구조**
	- 헤드 포인터(`top`)가 스택의 최상단 노드를 가리킨다.

- **주요 연산 구현**
	- `init`: `top = NULL`
	- `is_empty`: `top == NULL`
	- `push(e)`: 새 노드 생성 → 새 노드의 링크가 현재 `top`을 가리킴 → `top`을 새 노드로 변경
	- `pop()`: `top` 노드를 임시 포인터에 저장 → `top`을 다음 노드로 변경 → 임시 포인터(`free`) 삭제
	- `peek()`: `top->data` 반환
	- **메모리 해제**: 스택 사용이 끝나면 모든 노드를 순회하며 `free` 해야 메모리 누수가 없다.

### 4.1. 배열 구조의 스택 (동적 할당)
#### (1) 데이터
```c
typedef int Element;
Element* data = NULL;
int top;
int MAX_SIZE = 10;
```

#### (2) 기초 연산
```c
void error(char str[]) {
	printf("%s\n", str);
	exit(1);
}

void init_stack() {
	data = (Element*)malloc(sizeof(Element) * MAX_SIZE);
	if (data == NULL) error("Memory allocation failed!");
	top = -1;
}

void overflow() {
	MAX_SIZE *= 2; // 용량을 두 배 늘림
	data = (Element*)realloc(data, sizeof(Element) * MAX_SIZE);
	if (data == NULL) error("Memory allocation failed!");
	printf("realloc(%d)\n", MAX_SIZE);
	// 또는 error("Overflow Error!");
}

int is_empty() { return top == -1; }
int is_full() { return (top == (MAX_SIZE - 1)); }
```

#### (3) 스택 연산
```c
void push(Element e) {
	if (is_full()) overflow();
	data[++top] = e;
}

Element pop() {
	if (is_empty()) error("Underflow Error!");
	return data[top--];
}

Element peek() {
	if (is_empty()) error("Underflow Error!");
	return data[top];
}
```

### 4.2. 단순 연결된 스택
#### (1) 데이터
```c
typedef int Element;
typedef struct Node {	// 자기참조 구조체
	Element data;		// 데이터 필드(스택 요소)
	struct Node* link;	// 링크 필드
} Node;
Node* top = NULL;
```

#### (2) 기초 연산
```c
Node* alloc_node(Element e) {
	Node* p = (Node*)malloc(sizeof(Node));
	if (p == NULL) error("Memory allocation failed!");
	p->data = e;	// 데이터 초기화
	p->link = NULL;	// 링크 초기화
	return p;
}

Element free_node(Node* p) {
	Element e = p->data;	// 데이터 복사
	free(p);				// 동적 해제
	return e;				// 데이터 반환
}

void error(char* str) {
	printf("%s\n", str);
	exit(1);
}

void init_stack() { top = NULL; }
int is_empty() { return top == NULL; }
int is_full() { return 0; }
```

#### (3) 스택 연산
```c
void push(Element e) {			// 삽입 연산
    Node* p = alloc_node(e);	// p = 삽입할 새로운 노드
    p->link = top;				// p의 link는 원래의 top 노드
    top = p;					// p가 이제 새로운 top
}

Element pop() {					// 삭제 연산
    if (is_empty()) error("Underflow Error!");
    Node* p = top;				// p = 삭제할 원래의 top 노드
    top = p->link;				// 새로운 top은 top의 link(top 다음)
    return free_node(p);		// 원래 있던 top은 동적 해제 후 리턴
}

int size() { // 연결된 스택의 요소 수, 모든 노드 방문/순회(troversal)
    int count = 0;
    for (Node* p = top; p != NULL; p = p->link) // 시간 복잡도 O(n)
        count++;
    return count;
}
```

## 5. 원형 연결 구조 응용: 연결된 큐
- **구조**
	- 마지막 노드의 링크가 첫 번째 노드를 가리키는 구조

- **변형된 원형 연결 큐**
	- `rear` 포인터 하나만 있어도 큐의 앞(`front`)과 뒤(`rear`)를 모두 접근할 수 있다.
	- `rear`: 마지막 노드
	- `rear->link`: 첫 번째 노드 (삭제 위치)

- **주요 연산**
	- `enqueue(e)`: 새 노드를 `rear` 다음에 삽입하고, 원형 연결 유지
	- `dequeue()`: `rear->link`가 가리키는 노드(맨 앞) 삭제 및 반환

### 5.1. 단순 연결된 큐
- **Lab 5.5. 단순 연결 구조 큐 (비원형)**
	- `front`와 `rear` 두 개의 포인터를 사용
	- `rear`의 링크는 `NULL`
	- 단순 연결 리스트의 맨 뒤에 넣고(`enqueue`), 맨 앞에서 뺀다(`dequeue`).

#### (1) 데이터
```c
typedef struct Node {			// 데이터는 노드에 저장되어 관리됨
    Element data;				// 데이터 
    struct Node* link;			// 링크: 다음 노드를 가리킴 
} Node;
Node* front = NULL;				// 연결된 큐의 전단
Node* rear = NULL;				// 연결된 큐의 후단
```
#### (2) 연산
```c
void enqueue(Element e) {
    Node* p = alloc_node(e);	// 새 노드 p
    if (is_empty())				// 공백 상태의 삽입이면
        front = rear = p;		// front와 rear가 p를 가리킴
    else {						// 큐가 공백 상태가 아니면
        rear->link = p;			// p를 rear의 후속노드로 연결하고
        rear = p;				// rear를 이동(p를 가리키도록)
    }
}

Element dequeue() {
    if (is_empty()) error("Underflow Error!");
    Node* p = front;			// 삭제할 노드를 가리키는 포인터
    front = front->link;		// front는 한 칸 전진
    if (front == NULL)			// 삭제 후 공백 상태가 되면, (node 1개)
        rear = NULL;			// rear도 NULL로 초기화
    return free_node(p);		// p를 동적 해제하고 데이터 반환
}
```

### 5.2. 원형 단순 연결된 큐
#### (1) 데이터
```c
typedef int Element;
typedef struct Node {	// 자기참조 구조체
    Element data;		// 데이터 필드(스택 요소)
    struct Node* link;	// 링크 필드
} Node;
Node* rear = NULL;
```

#### (2) 기초 연산
```c
Node* alloc_node(Element e) {
	Node* p = (Node*)malloc(sizeof(Node));
	if (p == NULL) error("Memory allocation failed!");
	p->data = e;	// 데이터 초기화
	p->link = NULL;	// 링크 초기화
	return p;
}

Element free_node(Node* p) {
	Element e = p->data;	// 데이터 복사
	free(p);				// 동적 해제
	return e;				// 데이터 반환
}

void error(char* str) {
	printf("%s\n", str);
	exit(1);
}

void init_queue() { rear = NULL; }
int is_empty() { return rear == NULL; }
int is_full() { return 0; }
```

#### (3) 큐 연산
```c
void enqueue(Element e) {
    Node* p = alloc_node(e);	// 새로운 노드를 가리키는 p
    if (is_empty()) {			// < 공백 상태이면 >
        rear = p;				// 1개뿐이니까 p가 rear
        p->link = p;			// 1개뿐이니까 rear.link는 rear를 가리킴
    } else {					// < 공백 상태가 아니면 >
        p->link = rear->link;	// tail node의 link(top)를 따라함
        rear->link = p;			// 원래 tail node는 새 노드 p를 가리킴
        rear = p;				// 이제 새 노드 p가 tail node
    }
}

Element dequeue() {
    if (is_empty()) error("Underflow Error!");
    Node* p = rear->link; 		// (tail이 가리키는) head를 p로 가리킴
    if (rear == p) rear = NULL;	// if tail = head이면 rear NULL
    else rear->link = p->link;	// 이제 tail은 head의 다음 노드를 가리킴
    return free_node(p);		// 원래 head는 동적 해제
}

int size() {
    if (is_empty()) return 0;	// 공백인 경우는 0 반환
    int count = 1;
    for (Node* p = rear->link; p != rear; p = p->link) count++;
    return count;
}
```

## 6. 연결된 덱과 단순 연결 구조의 한계
### 6.1. 단순 연결된 덱
- **단순 연결된 덱**(Linked Deque)
	- 전단 삽입(`add_front`), 전단 삭제(`delete_front`), 후단 삽입(`add_rear`)은 $O(1)$로 쉽게 구현 가능

- **단순 연결 리스트로 덱(Deque) 구현 시 문제점**
	- **후단 삭제(`delete_rear`)의 한계**: 마지막 노드를 삭제하려면 그 **이전 노드**의 링크를 `NULL`로 바꿔야 하는데, 단순 연결 리스트는 이전 노드로 갈 수 있는 방법이 없다.
	- 따라서 처음부터 다시 찾아가야 하므로 $O(n)$의 시간이 걸린다.
		- (front부터 모든 노드 방문, $O(1)$로 구현 불가능)

### 6.2. 이중 연결된 덱
- **해결책**
	- 이 문제를 해결하기 위해 양쪽 방향으로 이동 가능한 **이중 연결 구조**(Doubly Linked Structure)를 사용
