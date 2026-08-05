---
aliases: []
type: Lecture
tags:
  - 1-2/프로그래밍
draft: false
date: 2022-11-22
---
## 1. 구조체
- **배열**(array)
	- 서로 같은 자료형의 데이터를 하나로 묶은 모임

- **구조체**(Structure)
	- 서로 다른 자료형의 데이터를 하나로 묶은 모임

### 1.1. 구조체의 정의
- `struct` 키워드를 사용하여 정의한다.
```c
struct 구조체_이름 { //struct 새로운 자료형의 이름
	자료형1 변수1; // 구조체의 필드(멤버 변수)
	자료형2 변수2;
	자료형3 변수3;
};
```

```c
struct profile {	// 신상 명세 구조체 선언
	char name[20];	// -> 이름을 저장할 배열 멤버
	int age;		// -> 나이 (int형 멤버)
	double height;	// -> 키 (double형 멤버)
	char* intro;	// -> 자기소개를 위한 포인터
}; // 세미콜론 사용
```

- `typedef`를 사용하면 매번 `struct`를 쓰지 않고 새로운 자료형처럼 사용할 수 있다.
```c
typedef struct Student_t {
	int id;
	char name[20];
	double score;
} Student; // 구조체의 별칭
```

#### 중첩 구조체
- 구조체의 멤버로 다른 구조체를 사용할 수 있다.
```c
struct profile {	// 신상명세 구조체 선언
	double height;	// -> 키
};

struct student {
	struct profile pf;	// profile 구조체를 멤버로 사용
	double grade;		// 학점을 저장할 멤버
};

int main(void) {
	struct student yuni; // student 구조체 변수 선언
	
	yuni.pf.height = 164.5; // pf 멤버의 height 멤버에 저장
	yuni.grade = 4.3;
	return 0;
}
```

### 1.2. 구조체의 선언
```c
// 선언
Student a;
```

#### 구조체 배열의 선언/초기화
- **구조체 배열**
	- 구조체를 요소로 가지는 배열을 만들 수 있다.
	- 예: `Friend list[45];`

- 구조체 배열과 구조체 배열을 처리하는 함수
```c
struct address {	// 주소록을 만들 구조체 선언
	char name[20];	// -> 이름을 저장할 멤버
	int age;		// -> 나이를 저장할 멤버
	char tel[20];	// -> 전화번호를 저장할 멤버
	char addr[80];	// -> 주소를 저장할 멤버
};

void print_list(struct address* lp) {	// 매개변수는 구조체 포인터
	for (int i = 0; i < 2; i++) {		// 배열 요소의 개수 만큼 반복
		printf("%10s%5d%15s%20s\n",		// -> 각 배열 요소의 멤버 출력
			(lp + i)->name, (lp + i)->age, (lp + i)->tel, (lp + i)->addr);
	}
}

int main(void) {
	struct address list[2] = { // 요소가 2개인 구조체 배열 선언
		{"홍길동", 23, "111 - 1111", "울릉도 독도"},
		{"이순신", 35, "222 - 2222", "서울 건천동"}
	};
	
	print_list(list);

	for (int i = 0; i < 2; i++) {	// 배열 요소 수만큼 반복
		printf("%10s%5d%15s%20s\n",	// -> 각 배열 요소의 멤버 출력
			list[i].name, list[i].age, list[i].tel, list[i].addr);
	}
	return 0;
}
```

### 1.3. 구조체의 초기화
```c
// 선언과 동시에 초기화
Student a = { 202403156, "홍길동", 96.3 };
// 멤버 접근 - 항목 연산자(membership operator) ‘.’
a.id = 30830;
a.score = 92.3;
strcpy_s(a.name, 20, "홍수아");
```

- **구조체의 연산**
	- 대입 연산자 `=`를 제외하고 일반적인 연산자들은 지원하지 않는다.
	- 기본적으로 deep copy
	- 포인터에 대해서는 shallow copy (직접 deep copy 필요)

### 1.4. 구조체 멤버에 접근하기
- 멤버 접근 연산자 `.` (점)을 사용 (예: `student.id`)

```c
int main(void) {
	struct profile yuni; // profile 구조체 변수 선언
	
	strcpy(yuni.name, "서하윤");	// name 배열 멤버에 이름 복사
	yuni.age = 17;					// age 멤버에 나이 저장
	yuni.height = 164.5;			// height 멤버에 키 저장
	
	yuni.intro = (char*)malloc(80);	// 동적할당
	printf("자기소개: ");
	gets(yuni.intro);				// 할당한 공간에 자기소개 입력
	
	printf("이름 : %s\n", yuni.name); // 각 멤버의 데이터 출력
	printf("나이 : %d\n", yuni.age);
	printf("키 : %.1lf\n", yuni.height);
	printf("자기소개 : %s\n", yuni.intro);
	
	free(yuni.intro); // 동적 할당 영역 반환
	return 0;
}
```

- **패딩 바이트**
	- 구조체 멤버의 크기가 들쑥날쑥한 경우,
	- 멤버 사이에 패딩 바이트를 넣어 멤버들을 가지런하게 정렬한다.

### 1.6. 구조체의 연산
- **대입 연산**
	- 대입 연산자(`=`)를 통해 구조체 변수 간 복사가 가능하다.

```c
struct student {   // 학생 구조체 선언
	int id;        // 학번
	char name[20]; // 이름
	double grade;  // 학점
};

int main(void) {
	struct student s1 = { 315, "홍길동", 2.4 }, // 구조체 변수 선언과 초기화
		s2 = { 316, "이순신", 3.7 },
		s3 = { 317, "세종대왕", 4.4 },
		max = s1; // 최고 학점을 저장할 구조체 변수
	
	if (s2.grade > max.grade) max = s2; // s2가 더 높으면 max에 대입
	if (s3.grade > max.grade) max = s3; // s3가 더 높으면 max에 대입
	return 0;
}
```

- **비교 연산**
	- 비교 연산자(`==`, `!=`)는 **사용 불가**
	- 비교하려면 내부 멤버들을 일일이 비교하는 함수를 직접 만들어야 한다.

## 2. 구조체 활용
### 예제
- **구조체 변형**
```c
int main(void) {
	struct student s[4] = { // 구조체 변수 선언과 초기화
		{315, "홍길동", 2.4}, // s[0]에 s[1]을 최고 학점으로 가정
		{315, "홍길동", 2.4},
		{316, "이순신", 3.7},
		{317, "세종대왕", 4.4} };
	
	int size = sizeof(s) / sizeof(struct student);
	for (int i = 1; i < size; i++)
		if (s[i].grade > s[0].grade) s[0] = s[i];
	
	printf("학번: %d\n", s[0].id);
	return 0;
}
```

### 2.1. 구조체와 함수
- **전달 방식**: 기본적으로 **Call-by-Value (값에 의한 호출)**
	- 함수에 구조체를 넘겨서 내용을 변경하더라도, 원본에는 영향을 주지 않는다. (복사본이 전달됨)
	- 원본을 수정하려면 포인터를 사용해야 한다.

- 구조체 변수를 함수 매개변수에 사용하기
```c
struct vision {		// 로봇의 시력을 저장할 구조체
	double left;	// -> 왼쪽 눈
	double right;	// -> 오른쪽 눈
};

struct vision exchange(struct vision robot) { // 구조체를 반환하는 함수
	double temp;		// 교환을 위한 임시 변수
	temp = robot.left;	// 좌우 시력 교환
	robot.left = robot.right;
	robot.right = temp;
	return robot;		// 구조체 변수 반환
}

int main(void) {
	struct vision robot; // 구조체 변수 선언
	
	printf("시력 입력 : ");
	scanf("%lf%lf", &(robot.left), &(robot.right)); // 시력 입력
	
	robot = exchange(robot); // 교환 함수 호출
	printf("바뀐 시력 : %.1lf %.1lf\n", robot.left, robot.right);
	return 0;
}
```

### 2.2. 구조체 포인터와 -> 연산자
- **도트 연산자**(`.`), **화살표 연산자**(`->`)
	- (구조체/포인터를 통한 맴버) 접근이라는 의미로 사용된다.
```c
struct score { // 구조체 선언
	int kor;   // -> 국어 점수를 저장할 멤버
	int eng;   // -> 영어 점수
};

int main(void) {
	struct score yuni = { 90, 80, 70 };	// 구조체 변수 선언과 초기화
	struct score* ps = &yuni;			// 구조체 포인터에 주소 저장
	
	printf("국어 : %d\n", (*ps).kor);	// 구조체 포인터로 멤버 접근
	printf("영어 : %d\n", ps->eng);		// -> 연산자 사용
	return 0;
}
```

### 2.3. 자기 참조 구조체
- **자기 참조 구조체**
	- 개별적으로 할당된 구조체 변수들을 포인터로 연결하면 관련된 데이터를 하나로 묶어 관리할 수 있다. 이때 자기 참조 구조체를 사용한다.

- **연결 리스트**
	- 구조체 변수를 포인터로 연결한 것

```c
// 17-9.c
struct list {			// 자기 참조 구조체
	int num;			// -> 데이터를 저장하는 멤버
	struct list* next;	// -> 구조체 자신을 가리키는 포인터 멤버
};

int main(void) {
	// 구조체 변수 초기화
	struct list a = { 10, 0 }, b = { 20, 0 }, c = { 30, 0 };
	// 헤드 포인터 초기화
	struct list* head = &a, * current;
	
	a.next = &b; // a의 포인터 멤버가 b를 가리킴
	b.next = &c; // b의 포인터 멤버가 c를 가리킴
	
	// head가 가리키는 a의 num 멤버 사용
	printf("head->num : %d\n", head->num);
	// head로 b의 num 멤버 사용
	printf("head->next->num : %d\n", head->next->num);
	
	printf("list all : ");
	current = head;				// 최초 current 포인터가 a를 가리킴
	while (current != NULL) {	// 마지막 구조체 변수까지 출력하면 반복 종료
		// current가 가리키는 구조체 변수의 num 출력
		printf("%d ", current->num);
		// current가 다음 구조체 변수를 가리키도록 함
		current = current->next;
	}
	return 0;
}
```

## 3. 공용체
```c
union student {		// 공용체 선언
	int num;		// -> 학번을 저장할 멤버
	double grade;	// -> 학점을 저장할 멤버
};

int main(void) {
	union student s1 = { 315 };			// 공용체 변수의 선언과 초기화
	
	printf("학번 : %d\n", s1.num);		// 학번 멤버 출력 (315)
	
	s1.grade = 4.4;						// 학점 멤버에 값 대입
	printf("학점 : %.1lf\n", s1.grade);	// -> (4.4)
	
	printf("학번 : %d\n", s1.num);		// 학번 다시 출력 (잘못된 값)
	return 0;
}
```

## 4. 열거형
```c
enum season { SPRING, SUMMER, FALL, WINTER }; // 열거형 선언

int main(void) {
	enum season ss = SPRING;	// 열거형 변수 선언
	char* pc = NULL;			// 문자열을 저장할 포인터
	
	switch (ss) // 열거 멤버 판단
	{
	case SPRING:				// 봄이면
		pc = "inline"; break;	// -> 인라인 문자열 선택
	case SUMMER:				// 여름이면
		pc = "swimming"; break;	// -> 수영 문자열 선택
	case FALL:					// 가을이면
		pc = "trip"; break;		// -> 여행 문자열 선택
	case WINTER:				// 겨울이면
		pc = "skiing"; break;	// -> 스키 문자열 선택
	}
	
	printf("나의 레저 활동: %s\n", pc); // 선택된 문자열 출력
	return 0;
}
```

### 2.6. typedef를 사용한 형 재정의 (17-12.c)
```c
struct student {
	int num;
	double grade;
};
typedef struct student Student; // Student형으로 재정의
```

```c
typedef struct { // 재정의될 것이므로 구조체 이름 생략
	int num;
	double grade;
} Student; // 재정의된 자료형 이름

void print_data(Student* ps) { // Student 포인터로 멤버 접근
	printf("학번 : %d\n", ps->num); 
	printf("학점 : %.1lf\n", ps->grade);
}

int main(void) {
	Student s1 = { 315, 4.2 }; // Student형의 변수 선언과 초기화
	
	print_data(&s1); // Student형 변수의 주소 전달
	return 0;
}
```
