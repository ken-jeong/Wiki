---
aliases: []
type: Lecture
tags:
  - 3-1/모바일프로그래밍
draft: false
date: 2026-03-13
---
## 1. 람다 함수와 고차함수
### 1.1. 람다 함수
- **람다 함수** (Lambda Function)
	- 람다식이라고도 함; **익명 함수 정의 기법**
	- 코틀린은 고차함수를 지원하기 때문에 람다식을 자주 사용
	
	- `fun` 키워드와 함수 이름이 없음
	- 중괄호 `{ }`로 표현
		- `{ }` 안에 화살표(`->`)가 있으며, **왼쪽 = 매개변수**, **오른쪽 = 함수 본문**
		- 반환값은 함수 본문의 **마지막 표현식** (==return문 사용 불가==)

```kotlin
// 일반 함수
fun sum(no1: Int, no2: Int): Int {
    return no1 + no2
}

// 람다 함수 { 매개변수 -> 함수 본문 }
val sum = {no1: Int, no2: Int -> no1 + no2}
```

### 1.2. 람다 함수 호출 방법
|     방법      |                     예시                      |
| :---------: | :-----------------------------------------: |
| 변수에 대입 후 호출 |      `val sum = {...}` → `sum(10, 20)`      |
| 선언과 동시에 호출  | `{no1: Int, no2: Int -> no1 + no2}(10, 20)` |

### 1.3. 매개변수 처리에 따른 람다식 작성법
- **매개변수 없는 람다 함수**
	- 화살표까지 생략 가능
```kotlin
// 매개변수 없는 람다 함수
{-> println("function call")}	// 화살표 포함
{println("function call")}		// 화살표 생략
```

- **매개변수가 1개인 람다 함수**
	- 매개변수 선언 없이 **`it` 키워드**로 대체 가능
	- 단, 타입을 식별할 수 있는 경우에만 사용 가능
```kotlin
// 매개변수가 1개인 람다 함수
val some = {no: Int -> println(no)}		// 일반 선언
val some: (Int) -> Unit = {println(it)}	// it 키워드 사용
val some = {println(it)}				// 오류 — 타입 식별 불가
```

### 1.4. 함수 타입(Function Type)
- **함수 타입**
	- 코틀린은 함수를 변수에 대입할 수 있다.
	- 변수에 함수를 넣으려면 해당 변수를 함수 타입으로 선언해야 한다.
	- -> 함수를 선언할 때 나타내는 매개변수와 반환 타입
```kotlin
// 함수 타입을 이용해 변수에 대입
val some: (Int, Int) -> Int = {no1: Int, no2: Int -> no1 + no2}
//         ↑ 함수 타입                  ↑ 함수 내용
```

- **타입 별칭** (`typealias`)
	- 타입의 별칭을 선언하는 키워드
	- 함수 타입을 선언할 때 주로 사용
```kotlin
typealias MyInt = Int						// 데이터 타입 별칭
typealias MyFunType = (Int, Int) -> Boolean	// 함수 타입 별칭

val someFun: MyFunType = {no1: Int, no2: Int -> no1 > no2}
```

- **매개변수 타입 생략**
	- 타입을 유추할 수 있으면 생략 가능
```kotlin
// 세 가지 방식 모두 동일
val someFun = {no1: Int, no2: Int -> no1 > no2}
val someFun: (Int, Int) -> Boolean = {no1, no2 -> no1 > no2}
val someFun: MyFunType = {no1, no2 -> no1 > no2}
```

### 1.5. 고차 함수
- **고차 함수** (Higher-Order Function)
	- 함수를 **매개변수로 전달받거나 반환하는** 함수

- ==출제==
```kotlin
fun hofFun(arg: (Int) -> Boolean): () -> String () {
	val result = if(arg(10)) {
		"valid"
	} else {
		"invalid"
	}
	return {"hofFun Result: $result"}
}
fun main() {
	val result = hofFun({no -> no > 0})
	println(result())
}
```

## 2. 널 안전성 (Null Safety)
### 2.1. 널(null)과 널 포인트 예외(NPE)
- 객체가 선언되었지만 **초기화되지 않은 상태** (주소를 가지지 못한 상태)
- 널인 객체를 사용하면 **NullPointerException** 발생
- 코틀린은 이러한 예외가 발생하지 않도록 하는 널 안전성을 언어 차원에서 지원

### 2.2. 널 안전성 연산자
| 연산자  |   이름    |            설명             |
| :--: | :-----: | :-----------------------: |
| `?`  |  널 허용   |  변수 타입 뒤에 붙여 null 대입 허용   |
| `?.` |  안전 호출  | null이면 null 반환, 아니면 멤버 접근 |
| `?:` | 엘비스 연산자 |   null일 때 대신할 값이나 구문 지정   |
| `!!` |  예외 발생  |    null이면 의도적으로 NPE 발생    |

#### (1) `?`: 널 허용
- 코틀린의 변수는 기본적으로 Null을 허용하지 않는다. (널 불허)
- 변수에 Null을 대입할 수 있게 하려면 타입 뒤에 `?`를 붙여야 한다.

```kotlin
var data1: String = "kkang"
data1 = null    // 오류 — 널 불허 타입

var data2: String? = "kkang"
data2 = null    // 성공 — 널 허용 타입
```

#### (2) `?.`: 안전 호출 연산자
- 널 허용(`?`)으로 선언된 객체의 멤버에 접근할 때 반드시 사용해야 한다.
- 객체가 Null이 아니면 정상적으로 멤버에 접근하고,
- **Null이면 NPE를 발생시키지 않고 그냥 `null`을 반환**한다.

```kotlin
var data: String? = "kkang"
var length = data.length    // 오류
var length = data?.length   // 성공 (null이면 null 반환)
```

#### (3) `?:`: 엘비스 연산자
- 객체가 Null일 때 `null` 대신 **특정 기본값이나 대체 구문을 실행**하도록 처리하는 연산자이다.

```kotlin
var data: String? = "kkang"
println("data length : ${data?.length ?: -1}")
// data = "kkang" → 출력: data length : 5
// data = null    → 출력: data length : -1
```

#### (4) `!!`: 예외 발생 연산자
- 의도적으로 객체가 널일 때 NullPointerException 예외를 발생시키는 연산자이다.
- 안전성 처리보다, 해당 상황에서 무조건 예외를 던져야 하는 특수한 로직이 필요할 때 사용한다.

```kotlin
fun some(data: String?): Int {
    return data!!.length  // null이면 NPE 강제 발생
}
println(some("kkang"))  // → 5
println(some(null))     // → NullPointerException 발생
```
