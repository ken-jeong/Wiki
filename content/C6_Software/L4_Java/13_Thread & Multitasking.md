---
aliases: []
type: Lecture
tags:
  - 2-2/자바
draft: false
date: 2025-11-26
---
## 1. 멀티태스킹
- **멀티태스킹**(Multi-tasking)
	- 하나의 응용프로그램이 여러 개의 작업(태스크)을 동시에 처리하는 것

- **사례**
	- 미디어 플레이어 (오디오 재생 + 비디오 재생 + UI 처리)
	- 테트리스 게임 (배경음악 + 블록 이동 + 키 입력 처리)

- **자바의 특징**: 자바는 멀티스레딩(Multi-threading)을 통해 멀티태스킹을 구현한다.

## 2. 자바의 멀티스레딩
- **스레드**(Thread)
	- 사용자가 작성한 코드로서, JVM에 의해 스케줄링되어 실행되는 단위이다.
	- 하나의 응용프로그램은 하나 이상의 스레드로 구성된다.
	- 마치 바늘 하나에 실(thread)이 꿰어 작업하는 것과 유사하다.

- **장점**: 한 스레드가 대기하는 동안 다른 스레드를 실행하여 프로그램 전체의 시간 지연을 줄이고 효율을 높인다.

- **구조**
	- 웹 서버 등에서 각 클라이언트 요청당 하나의 스레드를 생성하여 처리하는 방식이 대표적이다.
	- JVM은 하나의 자바 응용프로그램만 실행하며, 그 안에서 여러 스레드를 관리한다.

## 3. 스레드 만들기
스레드를 생성하는 방법은 크게 두 가지가 있다.
두 방법 모두 스레드 코드는 `run()` 메서드에 작성하며, 실행 시에는 `start()` 메서드를 호출해야 한다.

### 3.1. Thread 클래스 상속
- `java.lang.Thread`를 상속받는 새 클래스 작성
- `run()` 메서드를 오버라이딩(Overriding)하여 스레드 코드 작성
- 객체 생성 후 `.start()` 호출

```java
class TimerThread extends Thread { // 스레드 클래스 작성
	// ...
	@Override public void run() {
		// run() 오버라이딩
	}
}
```

```java
TimerThread th = new TimerThread(); // 스레드 객체 생성
th.start(); // 스레드 시작
```

### 3.2. Runnable 인터페이스 구현
- `java.lang.Runnable` 인터페이스를 구현하는 새 클래스 작성
- `run()` 추상 메서드를 구현
- `Thread` 객체 생성 시 생성자에 Runnable 구현 객체를 전달
- Thread 객체의 `.start()` 호출

```java
class TimerRunnable implements Runnable { // 스레드 클래스 작성
	// ...
	@Override public void run() {
		// run() 메서드 구현
	}
}
```

```java
Thread th = new Thread(new TimerRunnable()); // 스레드 객체 생성
th.start(); // 스레드 시작
```

### 3.3. 주의사항
- `run()` 메서드가 종료되면 스레드도 종료된다.
- 한 번 종료된 스레드는 다시 `start()` 할 수 없으며, 새 객체를 생성해야 한다.
- 한 스레드에서 다른 스레드를 강제로 종료시킬 수 있다.

## 4. 스레드 생명 주기와 스케줄링
### 4.1. 스레드 상태 (6가지)
- `NEW`: 생성되었으나 아직 실행 준비가 안 됨
- `RUNNABLE`: 실행 중이거나 실행 대기 상태
- `WAITING`: `wait()` 호출로 인해 대기 중인 상태
- `TIMED_WAITING`: `sleep(n)` 등으로 일정 시간 대기 중인 상태
- `BLOCK`: I/O 작업이나 동기화 락(lock) 대기 등으로 차단된 상태
- `TERMINATED`: 실행 종료

### 4.2. 우선순위(Priority)
- 1(최소) ~ 10(최대) 사이의 값을 가진다. (기본값: 5)
- `setPriority()`로 변경이 가능하다.
- JVM은 철저한 우선순위 기반으로 스케줄링하며, 동일 우선순위는 라운드 로빈(Round Robin) 방식을 사용한다.

### 4.3. Main 스레드
- JVM이 응용프로그램을 시작할 때 처음 생성되는 스레드로, `main()` 메서드를 실행한다.

## 5. 스레드 종료
스레드를 종료시키는 방법에는 두 가지가 있다.

### 5.1. 스스로 종료
- `run()` 메서드 내의 코드가 모두 실행되어 리턴되면 자연스럽게 종료된다.

### 5.2. 강제 종료
- **interrupt() 사용**
	- `th.interrupt()`를 호출하면 스레드 내에서 `InterruptedException`이 발생하며,
	- 이를 catch하여 `return` 하는 방식으로 종료한다.

- **flag 사용 (권장)**
	- 불린(boolean) 변수(예: `flag`)를 두고,
	- 외부에서 이 값을 변경하면 `run()` 메서드 내의 반복문이 조건을 확인하여 루프를 탈출(`return`)하게 만든다.

## 6. 스레드 동기화
- **문제점**
	- 다수의 스레드가 공유 데이터에 동시에 접근하면 데이터 무결성이 깨질 수 있다.
	- (예: 프린터 동시 출력, 은행 잔고 동시 수정)

- **해결책  - 스레드 동기화**(Thread Synchronization)
	- 공유 데이터에 접근하는 스레드들을 한 줄로 세워,
	- 한 번에 하나의 스레드만 배타적으로 접근하게 한다.

### 6.1. synchronized 키워드
- **동기화 메서드**: 메서드 선언부에 사용 (`synchronized void method() {}`)
- **동기화 블록**: 특정 코드 블록만 지정 (`synchronized(this) { ... }`)
- 먼저 실행한 스레드가 객체의 모니터(락)를 소유하며, 작업이 끝날 때까지 다른 스레드는 대기한다.

```java
class SharedBoard {
	private int sum = 0; // 집계판의 합
	
	synchronized public void add() {
		int n = sum;
		Thread.yield(); // 현재 실행 중인 스레드 양보
		n += 10; // 10 증가
		sum = n; // 증가한 값을 집계합에 기록
		System.out.println(Thread.currentThread().getName() + " : " + sum);
	}
	
	public int getSum() {
		return sum;
	}
}
```

## 7. wait(), notify(), notifyAll()을 이용한 스레드 동기화
- **목적**: 두 개 이상의 스레드가 협력해야 할 때 (예: 생산자-소비자 문제) 실행 순서를 제어하기 위해 사용한다.

- **메서드 (Object 클래스 소속)**
	- `wait()`: 다른 스레드가 깨워줄 때까지 락을 풀고 대기 상태로 들어간다.
	- `notify()`: 대기 중인 스레드 중 하나를 깨워 RUNNABLE 상태로 만든다.
	- `notifyAll()`: 대기 중인 모든 스레드를 깨운다.

- **주의**: 이 메서드들은 반드시 `synchronized` 블록 내에서만 사용해야 한다.
