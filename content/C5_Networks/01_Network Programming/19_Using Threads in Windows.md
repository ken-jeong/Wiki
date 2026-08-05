---
aliases: []
type: Lecture
tags:
  - 3-1/네트워크프로그래밍
draft: false
date: 2026-05-05
---
```c
HANDLE h_thread = (HANDLE)_beginthreadex(
	NULL,         0,                 // winsock: NULL, 0
	count_thread, (void*)&parameter, // 스레드 함수, 파라미터
	0,            &thread_id         // initflag, 스레드 ID
);

WaitForSingleObject(h_thread, INFINITE);

WaitForMultipleObjects(
	NUM_THREAD, hThreads, // 오브젝트 수, 오브젝트 배열
	TRUE,       INFINITE  // 전부,      timeout
);
```

## 1. 커널 오브젝트
- **커널 오브젝트** (Kernel Objects)
	- 시스템 리소스를 관리하기 위해 운영체제가 만드는 데이터 블록

- 운영체제가 만들고 관리하는 리소스
	1. 프로그램 실행과 관련된 프로세스·스레드
	2. 입출력 도구인 소켓·파일
	3. 스레드 간 동기화에 쓰이는 세마포어·뮤텍스

- 핵심
	- 커널 오브젝트의 **소유자가 운영체제**
	- 생성·관리·소멸이 모두 운영체제의 몫
	- 리소스 종류에 따라 커널 오브젝트의 형태도 달라진다.

### 1.1. 커널 오브젝트의 상태
> 커널 오브젝트는 두 가지 상태를 가진다.

1. **non-signaled 상태**
	- 이벤트가 아직 발생하지 않은(특정 상황에 이르지 않은) 상태

2. **signaled 상태**
	- 이벤트가 발생한(특정 상황에 도달한) 상태

- 예를 들어
	- **프로세스나 스레드의 커널 오브젝트는 생성 시 on-signaled 상태였다가 종료 시 signaled 상태로 바뀐다.**
	- 이 상태 정보를 통해 우리는 리소스의 상황을 인식할 수 있다.

### 1.2. 커널 오브젝트의 모드
1. **auto-reset 모드**
	- Wait 계열 함수가 반환되면서 자동으로 다시 non-signaled 상태로 돌아오는 커널 오브젝트

2. **manual-reset 모드**
	- 그렇지 않은 커널 오브젝트

### 1.3. 커널 오브젝트의 상태 확인
#### (1) `WaitForSingleObject()`
> 전달된 핸들의 커널 오브젝트가 signaled 상태가 되어야 함수가 반환된다.

```c
DWORD WaitForSingleObject(
	HANDLE hHandle,       // h_thread
	DWORD  dwMilliseconds // INFINITE -> signaled가 될 때까지 무한 대기
);
```

- 반환
	- `WAIT_OBJECT_0`: signaled
	- `WAIT_TIMEOUT`: 타임아웃

#### (2) `WaitForMultipleObjects()`
> 여러 커널 오브젝트를 한 번에 관찰

```c
DWORD WaitForMultipleObjects(
    DWORD nCount,
    const HANDLE* lpHandles,
    
    BOOL  bWaitAll,
	    // TRUE면 모든 오브젝트가 signaled가 되어야 반환
	    // FALSE면 하나라도 signaled가 되면 반환
    DWORD dwMilliseconds
);
```

## 2. 프로세스와 스레드
- **프로세스와 스레드의 관계**
	- 현대 운영체제는 스레드를 OS 레벨에서 지원하므로 `main` 함수 호출조차도 하나의 스레드가 수행한다.
	- 프로세스는 스레드를 담는 상자이다.
	- 추가 스레드를 만들지 않으면 단일 스레드 모델, 추가로 만들면 멀티 스레드 모델 프로그램이 된다.

## 3. 윈도우 기반의 스레드 생성
### 방법 1. `CreateThread()`
- **스레드 생성 함수**
```c
#include <windows.h>
HANDLE CreateThread( // 성공 시 스레드 핸들, 실패 시 NULL이 반환
	LPSECURITY_ATTRIBUTES lpThreadAttributes,
	SIZE_T dwStackSize,
	
	LPTHREAD_START_ROUTINE lpStartAddress,	// ** 스레드가 실행할 main 함수 **
	LPVOID lpParameter,						// ** main 함수에 전달할 인자 **
	
	DWORD dwCreationFlags,
	LPDWORD lpThreadId
);
```

- **환경설정 주의사항**
	- 멀티 스레드 프로그램을 작성할 때는
	- 프로젝트 속성에서 런타임 라이브러리를 "다중 스레드(/MT)" 또는 "다중 스레드 디버그(/MTd)" 등
	- 멀티 스레드용으로 지정해야 C/C++ 표준 함수가 안정적으로 호출된다.

### 방법 2. `_beginthreadex()` (권장)
- `_beginthreadex()`
	- **C 표준 함수의 안전한 호출**
	- 매개변수의 의미와 순서는 `CreateThread`와 동일하다.
	- 이 함수로 생성된 스레드는 표준 C/C++ 함수에 대해 안정적으로 동작한다.
	- 따라서 ==표준 C 함수를 호출하는 경우에는 반드시== `_beginthreadex`를 써야한다.

```c
#include <process.h>

uintptr_t _beginthreadex(
   void     *security,  // 보통 기본값인 NULL을 주로 전달
   unsigned stack_size, // 0을 전달하면 기본 스택 크기를 사용
   
   unsigned (*start_address)(void *), // ** 새 스레드가 실행할 함수의 포인터 **
   void     *arglist, // ** 스레드 함수가 실행될 때 인자로 넘겨줄 데이터의 포인터 **
   
   unsigned initflag, // 스레드의 초기 생성 상태를 제어하는 플래그
   unsigned *thrdaddr // 생성된 Thread ID 값을 저장할 변수의 주소
);
```

- *cf.*
	- `CreateThread()`는 단순히 OS 레벨 스레드만 생성한다.
	- CRT(C 런타임 라이브러리)는 새 스레드가 생긴 걸 모른다.
	- 그래서 CRT가 필요한 내부 구조를 **초기화**하지 않는다.

- ==리눅스와 달리 윈도우의 스레드는 스레드 함수가 반환되면 자동으로 소멸==된다.
