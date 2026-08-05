---
aliases: []
type: Lecture
tags:
  - 3-1/모바일프로그래밍
draft: false
date: 2026-05-01
---
## 1. 브로드캐스트 리시버 개요
- **브로드캐스트 리시버** (Broadcast Receiver)
	- 안드로이드 4대 컴포넌트 중 하나
	- **시스템에서 발생하는 이벤트로 실행**되는 컴포넌트이다.
	- 사용자 이벤트가 아니라 부팅 완료, 배터리 잔량 부족 같은 시스템 상황을 알리는 정보를 처리한다.
	- 액티비티와 유사하게 인텐트를 시스템에 전달하는 방식으로 실행된다.

### 1.1. 구현 방법
- `BroadcastReceiver`를 상속받는 클래스를 선언

- **생명주기 함수**는
	- ==onReceive() 함수 단일로 구성==된다.
	- 리시버가 실행되면 `onReceive()`가 자동 호출되고, 호출한 인텐트 객체를 매개변수로 받는다.
	- ==10초 이내에 처리==를 완료해야 한다.

### 1.2. 등록 방식
|   등록 방식   |           등록 위치            |                     비고                     |    주요 활용 사례    |
| :-------: | :------------------------: | :----------------------------------------: | :------------: |
| **정적 등록** |    AndroidManifest.xml     |            exported,<br>enabled            | BOOT_COMPLETED |
| **동적 등록** | 코드 내<br>registerReceiver() | RECEIVER_EXPORTED,<br>unregisterReceiver() | SCREEN_ON/OFF  |

#### (1) 정적 등록
- **정적 등록** (매니페스트 등록)
	- 컴포넌트이므로 `AndroidManifest.xml`에 `<receiver>`로 등록한다.
	- `enabled` 속성: 리시버의 활성화 여부 (`true`/`false`)
	- ==exported 속성==: 외부 앱의 이벤트 처리 여부 (`true`/`false`)

- 주의
	- 매니페스트에 `<intent-filter>`를 선언했더라도,
	- 최신 안드로이드 버전에서는 암시적 인텐트로는 실행이 제한될 수 있다.

#### (2) 동적 등록
- **동적 등록** (코드 내 등록)
	- 매니페스트에 등록하지 않고,
	- 특정 액티비티/서비스 실행 시에만 동작하도록 코드로 등록할 수 있다.

- ==코드==
```kotlin
// 1. 등록
registerReceiver(리시버_객체, 인텐트_필터, 공개_상태_정보)
	// 암시적 인텐트 실행 시 RECEIVER_EXPORTED
	// 명시적 인텐트 실행 시 RECEIVER_NOT_EXPORTED

// 2. 해제
unregisterReceiver(리시버_객체)
```

- 예: `SCREEN_ON`/`SCREEN_OFF`

### 1.3. 실행 방식
- **실행 방식** (명시적 vs 암시적)
	- 매니페스트에 **클래스명** 등록 → 명시적 실행
	- 매니페스트에 **인텐트 필터** 등록 → 암시적 인텐트로는 **실행 불가**
	- 코드에서 `registerReceiver()`로 등록 → 암시적 인텐트로도 실행 가능

- **실행 흐름** (인텐트 전달 예제)
	- 액티비티에서 버튼 클릭
	- → `Intent(this, MyReceiver::class.java)` 생성
	- → `sendBroadcast(intent)`로 시스템에 전달
	- → 리시버의 `onReceive()` 실행

|      구분      | 실행 대상 없음  |  1개   |         여러 개         |
| :----------: | :-------: | :---: | :------------------: |
| **액티비티 인텐트** |   오류 발생   | 정상 실행 |      사용자가 1개 선택      |
| **리시버 인텐트**  | 오류 발생 안 함 | 정상 실행 | **조건에 맞는 리시버 모두 실행** |

## 2. 시스템 상태 분석
### 2.1. 부팅 완료 (`BOOT_COMPLETED`)
- 기기 부팅 완료 시 `android.intent.action.BOOT_COMPLETED` 액션의 인텐트 발생
- **매니페스트에 인텐트 필터 구성** + `RECEIVE_BOOT_COMPLETED` 권한 추가 필요

### 2.2. 화면 켬/끔 (`SCREEN_ON` / `SCREEN_OFF`)
- 매니페스트 등록으로는 동작하지 않음
	- → **반드시 `registerReceiver()`로 동적 등록**

- 하나의 필터로 묶어 둘 다 감지
	- `android.intent.action.SCREEN_ON`
	- `android.intent.action.SCREEN_OFF`

- 수시로 실행되므로 불필요할 때 등록 해제

### 2.3. 배터리 상태 정보
#### (1) 배터리 상태 변경 이벤트 수신 (리시버 사용)
- 배터리 상태가 변할 때마다 이벤트를 받는다.

| 주요 액션 문자열                   | 의미            |
| --------------------------- | ------------- |
| `BATTERY_LOW`               | 배터리 낮은 상태로 변경 |
| `BATTERY_OKAY`              | 정상 상태로 변경     |
| `BATTERY_CHANGED`           | 충전 상태 변경      |
| `ACTION_POWER_CONNECTED`    | 전원 공급 시작      |
| `ACTION_POWER_DISCONNECTED` | 전원 공급 중단      |

#### (2) 현재 배터리 정보 즉시 파악 (리시버 이벤트 대기 없이 파악)
- `registerReceiver()`의 첫 번째 인자(리시버 객체)에 `null`을 전달하면, 현재 배터리 상태를 담은 인텐트를 즉시 반환받을 수 있다.
```kotlin
val batteryStatus = registerReceiver(
	null, IntentFilter(ACTION_BATTERY_CHANGED)
)
```

- 반환된 인텐트에서 엑스트라 값 추출:
    - `EXTRA_STATUS` → 충전 상태 파악 (예: `BATTERY_STATUS_CHARGING`이면 충전 중)
    - `EXTRA_PLUGGED` → **USB(저속 충전)**, **AC(고속 충전)** 구분

- 충전량 계산:
    - `EXTRA_LEVEL`(현재 충전량) ÷ `EXTRA_SCALE`(최대 충전량) × 100 = 배터리 %
