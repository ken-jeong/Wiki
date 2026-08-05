---
aliases: []
type: Lecture
tags:
  - 3-1/모바일프로그래밍
draft: false
date: 2026-03-13
---
## 요약
|      키워드      |        설명        |
| :-----------: | :--------------: |
|  `lateinit`   |      늦은 초기화      |
|     `var`     |      가변 변수       |
|     `val`     |      불변 변수       |
| `by lazy { }` |   최초 접근 시 초기화    |
|     `fun`     |      함수 선언       |
|     `is`      |    타입 확인 연산자     |
|     `in`      |    범위 지정 연산자     |
|     `..`      | 범위 연산자 (끝 값 포함)  |
|    `until`    | 범위 연산자 (끝 값 미포함) |
|    `step`     |     반복 증가 단위     |
|   `downTo`    |      역순 반복       |

## 1. 코틀린 언어 소개
### 1.1. 등장 배경 및 특징
- **개발사**: JetBrains의 오픈소스 그룹
- **기반**: JVM (안드로이드에서는 DVM을 통해 실행)
- **공식 언어**: 2017년 구글이 안드로이드 공식 언어로 지정
- **컴파일**:코틀린 컴파일러(`kotlinc`)가 `.kt` 파일 → 자바 바이트코드(`.class`) 생성

|  장점   |                설명                 |
| :---: | :-------------------------------: |
| 자바 호환 |    자바와 100% 호환, 자바 라이브러리 사용 가능    |
|  간결성  |     최신 언어 기법으로 훨씬 짧은 코드 작성 가능     |
| 널 안전성 | `nullable` / `not null`로 변수 구분 선언 |
|  비동기  |  코루틴(coroutines)으로 비동기 프로그래밍 간편화  |

### 1.2. 실습 환경 및 파일 구성
- **스크래치(Scratch)**
	- 안드로이드 스튜디오에서 제공하며,
	- 복잡한 코드 없이 간단한 코틀린 예제를 빠르게 테스트할 수 있는 기능

- **시작점**
	- 일반적인 코틀린 소스 파일은 `main()` 함수에서 시작하지만,
	- 안드로이드는 이벤트 기반이므로 앱 구동 시 `main()`이 없음

- **파일 구성**
	- `package`, `import`, 변수 선언, 함수, 클래스로 구성됨
	- 패키지 경로는 실제 폴더 위치를 의미하며 `import`로 다른 클래스를 불러옴

#### cf. 프로젝트 구조
|             구분              |                       내용                       |
| :-------------------------: | :--------------------------------------------: |
|       **manifests/**        | AndroidManifest.xml (액티비티, 서비스 등을 시스템에 알리는 역할) |
|          **java/**          |               코틀린 소스 코드 (액티비티 등)               |
|          **res/**           |  drawable, layout(xml), mipmap(icon), values   |
| *build.gradle.kts (Module)* |        앱의 패키지명, SDK 버전, 외부 라이브러리 의존성 등         |

### 1.3. 패키지 경로
```kotlin
package com.example.test3					// 패키지

import java.text.SimpleDateFormat			// 임포트
import java.util.*

var data = 10								// 전역 변수

fun formatDate(date: Date): String { ... }	// 함수

class User { ... }							// 클래스
```

> 패키지 경로가 다를 경우 `import` 구문으로 명시적으로 불러와야 함

### 1.4. Scratch 파일
- Android Studio: `[File] → [New] → [Scratch File] → [Kotlin]`
- 코틀린 소스 파일은 `main()`에서 시작 (안드로이드 앱은 이벤트 기반이므로 `main()` 없음)

## 2. 변수
### 2.1. 변수 선언
|  키워드  |       의미        | 재할당  |
| :---: | :-------------: | :--: |
| `var` | variable; 가변 변수 | ✅ 가능 |
| `val` |  value; 불변 변수   | ❌ 불가 |

```kotlin
var data1 = 10	// 재할당 가능
val data2 = 10	// 재할당 불가
```

#### (1) 타입 지정
```kotlin
val data1 = 10		// 타입 추론; 대입하는 값을 통해 유추
val data2: Int = 10	// 타입 명시
```

#### (2) 초기화
- 최상위 변수 / 클래스 멤버 변수 → **선언과 동시에 초기화 필수**
- 함수 내부 변수 → 나중에 초기화해도 무방 (단, 사용 전에는 반드시 초기화)

#### (3) 늦은 초기화
- ==lateinit==
	- 나중에 초기화할 것을 명시적으로 선언
	- `var` 변수에만 사용 가능
	- `Int`, `Long`, `Short`, `Double`, `Float`, `Boolean`, `Byte`에는 사용 불가
```kotlin
lateinit var data1: String	// OK
lateinit val data2: String	// 오류; val 불가
lateinit var data3: Int		// 오류; 기본 타입 불가
```

- `by lazy { }`; 델리게이트(delegate) 방식
	- 초기화는 최초 접근 시 1회만 실행됨
	- `val` 변수에만 사용 가능
	- 마지막 줄의 값이 초기값
```kotlin
val data4: Int by lazy {
	println("in lazy......")
	10	// 마지막 줄의 값이 초기값으로 할당
}
```

#### (4) 널 안전성
- ==널 안전성 (Null Safety)==
	- 기본적으로 코틀린의 객체는 `null`을 가질 수 없음
	- 타입 뒤에 **물음표**(`?`)를 붙여야만 `null` 대입이 가능

```kotlin
var data1: Int = 10
data1 = null	// 오류; not null

var data2: Int? = 10
data2 = null	// OK; nullable
```

### 2.2. 데이터 타입
- 코틀린의 모든 변수는 **객체**이므로 기본 타입도 클래스로 취급됨

| 분류  |               타입               |            비고            |
| :-: | :----------------------------: | :----------------------: |
| 정수  | `Byte`, `Short`, `Int`, `Long` |            -             |
| 실수  |   `Float`(`f` 필수), `Double`    |            -             |
| 문자  |             `Char`             |     문자만 저장, 숫자 대입 불가     |
| 문자열 |            `String`            | 큰따옴표 `"` 또는 삼중 따옴표 `"""` |
| 논리  |           `Boolean`            |            -             |

#### (1) 특수 타입
- `Any`
	- 코틀린의 최상위 클래스 (자바의 Object와 유사)
	- 모든 타입 할당 가능
```kotlin
val data1: Any = 10
```

- ==Unit==
	- 반환값이 없는 함수에 사용
	- 자바의 void와 유사하지만 실제 존재하는 객체
```kotlin
fun some(): Unit {
	println(10 + 20)
}
```

- `Nothing`
	- 항상 `null`만 반환하거나 예외(Exception)를 던지는 함수의 반환 타입으로 사용
```kotlin
val data1: Nothing? = null

fun some1(): Nothing? {
	return null
}

fun some2(): Nothing {
	throw Exception()
}
```

### 2.3. 문자열 템플릿
- ==문자열 템플릿==
	- 문자열 안에서 변수나 표현식을 `$` 또는 `${}`로 삽입
```kotlin
println("name : $name, sum : ${sum(10)}, plus : ${10 + 20}")
```

### 2.4. 컬렉션 타입
#### (1) Array
```kotlin
// Array<타입>
val data1: Array<Int> = Array(3, { 0 })	// 크기 3, 초기값 0
val data1 = arrayOf<Int>(10, 20, 30)	// 값과 함께 선언; arrayOf<타입>()

val data2: IntArray = IntArray(3, {0})
val data2: IntArray = intArrayOf(0, 1, 2)
```

- 인덱스 접근: `data1[0]` 또는 `data1.get(0)`
- 데이터 변경: `data1[0] = 10` 또는 `data1.set(0, 10)`
- 기초 타입 배열: `IntArray`, `BooleanArray` 등 별도 클래스 제공

#### (2) List, Set, Map
|      타입       |        함수         |          특징          |
| :-----------: | :---------------: | :------------------: |
|    `List`     |    `listOf()`     | 순서 있음, 중복 허용, **불변** |
|     `Set`     |     `setOf()`     | 순서 없음, 중복 불허, **불변** |
|     `Map`     |     `mapOf()`     | 키-값 쌍, 순서 없음, **불변** |
| `MutableList` | `mutableListOf()` | 순서 있음, 중복 허용, **가변** |
| `MutableSet`  | `mutableSetOf()`  | 순서 없음, 중복 불허, **가변** |
| `MutableMap`  | `mutableMapOf()`  | 키-값 쌍, 순서 없음, **가변** |

- **컬렉션 구분**
	- **불변(Immutable)**
		- `List`, `Set`, `Map`
		- (읽기 전용, `listOf()`, `setOf()` 등)
	- **가변(Mutable)**
		- `MutableList`, `MutableSet`, `MutableMap`
		- (추가/변경 가능, `mutableListOf()` 등, `add()`, `set()` 지원)

- `Map`은 키와 값의 쌍으로 이루어지며 `mapOf("키" to "값")` 형태로 생성
```kotlin
// Map 생성 (Pair 또는 to 사용)
var map = mapOf<String, String>(Pair("one", "hello"), "two" to "world")
```

## 3. 함수
### 3.1. 함수 선언
```kotlin
// fun 함수명(매개변수명: 타입): 반환타입 { ... }
fun some(data1: Int, data2: Int = 10): Int {
	return data1 * data2
}
some(10)		// 100 (data2 기본값 10 적용)
some(10, 20)	// 200
some(data2 = 20, data1 = 10)
```

1. `fun` 키워드를 사용
2. 매개변수에는 `var` / `val` **사용 불가** (`val`이 **자동 적용**)
	- 단, 주생성자의 매개변수(클래스의 멤버 변수)에는 `var`/`val` 선언이 가능
3. 매개변수에 **기본값** 선언 가능
	- **명명된 매개변수**(named parameter)로 순서 무관하게 전달 가능
4. 반환 타입 생략 시 `Unit` 자동 적용

## 4. 조건문
### 4.1. if~else
- **`if~else` 문**
	- 타 언어와 구조가 같음
	- 마지막 줄의 값이 반환됨

#### (1) 표현식
```kotlin
// if문 표현식 (Expression)
val result = if (data > 0) {
	println("data > 0")
	true	// 참일 때 반환값
} else {	// 표현식 else 필수
	println("data <= 0")
	false	// 거짓일 때 반환값
}
```

### 4.2. when
- 타 언어의 `switch`문과 유사하나 훨씬 강력함
	- 정수뿐만 아니라 다양한 타입의 데이터를 조건으로 지정 가능

```kotlin
when (data) {
    10 -> println("data is 10")
    20 -> println("data is 20")
    else -> println("data is not valid")
}
```

- 문자열, 범위, 타입 등 다양한 조건 사용 가능
	- `in`: 범위 지정 연산자 (`..`은 끝 값 포함)
	- `is`: 타입 확인 연산자
```kotlin
when (data) {
    20, 30       -> println("data is 20 or 30")
    in 1..10     -> println("data is 1..10")
    is String    -> println("data is String")
    else         -> println("data is not valid")
}
```

#### (1) 표현식
```kotlin
val result = when {
    data <= 0   -> "data is <= 0"
    data > 100  -> "data is > 100"
    else        -> "data is valid" // 표현식으로 사용 시 else 생략 불가
}
```

## 5. 반복문
### 5.1. for
```kotlin
for (i in 1..10) { ... }		// 1 ~ 10 (1씩 증가)
for (i in 1 until 10) { ... }	// 1 ~ 9  (10 미포함)
for (i in 2..10 step 2) { ... }	// 2, 4, 6, 8, 10
for (i in 10 downTo 1) { ... }	// 10 ~ 1 (1씩 감소)
```

#### (1) 컬렉션과 반복문
```kotlin
// indices 사용
for (i in data.indices) {
	// 배열/컬렉션의 인덱스 값을 가져옴
	print(data[i])
}

// withIndex() 사용 (인덱스 + 값)
for ((index, value) in data.withIndex()) {
	// 인덱스와 실제 데이터(값)를 함께 가져옴
	print(value)
}
```

|       연산자       |  비교 내용  |             특징             |
| :-------------: | :-----: | :------------------------: |
|    **`==`**     | 값 (내용)  | 코틀린의 표준 비교 방식, `null-safe` |
|    **`===`**    | 주소 (참조) |      메모리상 같은 객체인지 확인       |
| **`.equals()`** | 값 (내용)  |   객체 비교 메서드 (직접 호출은 지양)    |

```kotlin
fun main() {
	val a = String("hello")
	val b = String("hello")
	val c = a
	
	// 1. == (값 비교)
	println(a == b)  // true (내용이 같음)
	
	// 2. === (참조 비교)
	println(a === b) // false (서로 다른 객체 인스턴스임)
	println(a === c) // true (같은 객체를 참조함)
}
```

### 5.2. while
```kotlin
while (x < 10) {
	// 조건이 참일 때 블록을 반복 실행
	// (기존 언어와 동일)
    sum1 += ++x
}
```
