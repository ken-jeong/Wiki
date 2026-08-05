---
aliases: []
type: Lecture
tags:
  - 3-1/모바일프로그래밍
draft: false
date: 2026-05-01
---
## 1. 서비스 이해하기
- **서비스 (Service)**
	- 화면 없이 ==백그라운드 작업==을 처리하는 안드로이드 컴포넌트이다.
	- 오래 걸리는 작업을 담당한다.
	- 컴포넌트이므로 생명주기를 시스템이 관리한다.

### 1.1. 생성과 등록
- `Service` 클래스를 상속받아 작성한다.
- 매니페스트에 등록하며 `android:name`은 필수이다.
- 명시적 인텐트면 클래스명만, 암시적 인텐트면 `<intent-filter>`를 추가한다.

### 1.2. 실행 방법 2가지
> 어떤 함수로 실행하느냐에 따라 생명주기가 달라진다.

|  구분   | startService() |    bindService()     |
| :---: | :------------: | :------------------: |
|  용도   |  컴포넌트와 상호작용 X  |     컴포넌트와 상호작용 O     |
|  종료   | stopService()  |   unbindService()    |
| 추가 객체 |      불필요       | ServiceConnection 필요 |

- **외부 앱 서비스**는
	- 암시적 인텐트로 실행하므로 `setPackage()`로 패키지명을 명시해야 한다.
	- 안드로이드 11(API 30)부터는 매니페스트에 `<queries>`로 패키지 공개 상태를 등록해야 한다.

#### (1) startService()
- ==생명주기==
	1. `onCreate()`: 최초 1회 실행
	2. → `onStartCommand()`: 호출될 때마다 반복 실행
	3. → `onDestroy()`

#### (2) bindService()
- `bindService()`의 세 번째 인자 `Context.BIND_AUTO_CREATE`는 서비스가 실행 중이 아니어도 생성해 실행하라는 의미이다.

- 다른 컴포넌트와 데이터를 주고받거나 상호작용할 때 사용한다.
- `ServiceConnection` 객체가 필요하다.

- ==생명주기==
	1. `onCreate()`
	2. → `onBind()`: ==필수로 구현==, `IBinder` 구현 객체를 반환
	3. → `onUnbind()`
	4. → `onDestroy()`

## 2. 바인딩 서비스
### 2.1. IBinder
- **바인딩 서비스**
	- ==다른 컴포넌트와 데이터를 주고받아야 할 때== 사용한다.

- 구현
	- 액티비티는 `ServiceConnection`의 `onServiceConnected()` 두 번째 매개변수로 그 객체를 받아, 캐스팅 후 함수를 직접 호출(`serviceBinder.funA(10)`)해 매개변수·반환값으로 데이터를 교환한다.

### 2.2. AIDL
- **AIDL**(Android Interface Definition Language)은
	- 서로 다른 프로세스 간 통신(IPC)을 위한 방식이다.
	- (기본적으로 프로세스 간 메모리 접근은 불가능)

- 구현
	- 제공 앱·이용 앱 모두 `build.gradle.kts`에 `buildFeatures { aidl = true }`를 설정해야 한다.
	- `.aidl` 파일에는 **통신용 함수만 선언**하고, 실제 로직은 서비스의 `onBind()`에서 `Stub`을 반환하며 구현한다.
	- 서비스 제공 앱은 매니페스트에 `<intent-filter>`로 암시적 실행을 등록하고, 이용 앱은 **동일한 AIDL 파일을 가지고** 있어야 하며 `Stub.asInterface(service)`로 객체를 받아 함수를 호출한다.

## 3. 백그라운드 제약
- **백그라운드 제약** (Android 8.0+)
	- 앱이 백그라운드 상태일 때 일반적인 방법으로 서비스를 시작하면 오류가 발생한다.

### 3.1. 브로드캐스트 리시버
> 커스텀 액션으로 선언한 리시버는 암시적 인텐트로 실행되지 않을 수 있다.

1. **명시적 인텐트**
	- `setPackage()`로 패키지를 명시하고 `<queries>`에 등록해야 한다.

2. **registerReceiver()**
	- `registerReceiver()`로 동적 등록하면 패키지명 없이도 실행된다.
	- (단, `RECEIVER_EXPORTED` 설정 필요)

### 3.2. 서비스
- 앱이 백그라운드 상태에서 인텐트를 전달하면 오류가 발생한다.
	- (보이는 액티비티, 포그라운드 서비스, 바인딩된 경우 등만 포그라운드로 간주)

- **예외**적으로
	1. FCM 고우선순위 처리,
	2. SMS/MMS 수신,
	3. 알림의 PendingIntent 실행 등은 백그라운드에서도 정상 실행된다.

## 4. 포그라운드 서비스로 해결
- `startForegroundService()`로 실행 후
	- 반드시 `startForeground(알림)`을 호출해 사용자에게 **알림**(Notification)을 띄워야 한다.
	- (안 하면 강제 종료)

- 버전 호환 코드:
	- API 26(`O`) 미만이면 `startService()`
	- API 26(`O`) 이상이면 `startForegroundService()`

- 매니페스트에
	- `foregroundServiceType`(camera, dataSync, specialUse 등)을 지정하고,
	- 그에 맞는 퍼미션과 함께 `FOREGROUND_SERVICE`, `POST_NOTIFICATIONS` 퍼미션을 선언해야 한다.

## 5. 잡 스케줄러로 해결
- **잡 스케줄러** (Job Scheduler)
	- 백그라운드 제약을 보완하는 방법이다.
	- API 21부터 제공되었으나 백그라운드 제약(API 26) 이후 중요성이 커졌다.
	- **조건을 명시할 수 있는 상황에서만 백그라운드 처리**가 가능하다.

- **구성 요소 3가지**
	1. **잡 서비스 (JobService)**: 백그라운드 작업을 구현한 서비스
	2. **잡 인포 (JobInfo)**: 서비스 정보와 실행 조건 지정
	3. **잡 스케줄러 (JobScheduler)**: JobInfo를 시스템에 등록

### 5.1. 잡 서비스 구현
- `JobService`를 상속, `BIND_JOB_SERVICE` 퍼미션으로 등록한다.

```kotlin
class MyJobService : JobService() {
	override fun onStartJob(params: JobParameters?): Boolean {
	}
	
	override fun onStopJob(params: JobParameters?): Boolean {
	}
}
```

- `onStartJob()`
	- 작업 구현
	- 반드시 재정의

- `onStartJob()` 반환값:
	- `false`면 작업 완료(바로 종료),
	- `true`면 작업이 아직 안 끝났다는 의미로 스레드 등에서 처리 후 `jobFinished()`를 호출해 마무리한다.

- `onStopJob()` 반환값:
	- `false`면 등록 취소,
	- `true`면 재등록

### 5.2. JobInfo 설정
- `JobInfo.Builder(식별값, ComponentName)`으로 생성하고 세터로 조건 지정 후 `schedule()`로 등록한다.

- 주요 조건
	- `setRequiredNetworkType()`(네트워크),
	- `setRequiresCharging()`/`setRequiresBatteryNotLow()`(배터리),
	- `setPeriodic()`(실행 주기, **최소 15분**·정확한 시간 보장 안 됨),
	- `setMinimumLatency()`(지연 시간),
	- `setOverrideDeadline()`(마감 시간),
	- `setPersisted(true)`(재부팅 후 유지, `RECEIVE_BOOT_COMPLETED` 퍼미션 필요)

- 데이터 전달
	- `setExtras(PersistableBundle)`로 키-값 저장
	- → 서비스에서 `onStartJob()`의 `JobParameters.extras`로 꺼낸다.
