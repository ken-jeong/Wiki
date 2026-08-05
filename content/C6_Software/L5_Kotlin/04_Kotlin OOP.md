---
aliases: []
type: Lecture
tags:
  - 3-1/모바일프로그래밍
draft: false
date: 2026-03-13
---
## 요약
|      항목       |          키워드           |      객체 생성       |         비고          |
| :-----------: | :--------------------: | :--------------: | :-----------------: |
|  **일반 클래스**   |    `class Name { }`    |        필요        |  `equals()`: 주소 비교  |
|  **데이터 클래스**  |  `data class Name()`   |        필요        | `equals()`: 멤버 값 비교 |
| **오브젝트 클래스**  |      `object { }`      |    선언과 동시에 생성    |       익명 클래스        |
| **컴패니언 오브젝트** | `companion object { }` | 불필요 (클래스 명으로 접근) |     `static` 대체     |

| 주 생성자 | 보조 생성자 |       비고        |
| :---: | :----: | :-------------: |
|   X   |   O    | `this()` 연결 불필요 |
|   O   |   O    | `this()` 호출 필수  |
 
## 1. 클래스와 생성자
### 1.1. 클래스 선언
- `class` 키워드로 선언
- 본문이 없으면 블록 `{ }` 생략 가능 (단, 함수는 본문이 없어도 생략 불가)
- 클래스 멤버 구성: **생성자, 변수, 함수, 클래스(내부 클래스)**
	- 클래스 내부에 다른 클래스 선언 가능 (내부 클래스)
	- 함수도 내부에 다른 함수 정의 가능하지만 외부에서 접근 불가

```kotlin
class User {
    var name = "kkang"
    constructor(name: String) {
        this.name = name
    }
    fun someFun() {
        println("name : $name")
    }
    class SomeClass { }
}
```

### 1.2. 객체 생성
- `new` 키워드 **사용하지 않음**
- 클래스명과 동일한 함수 형태로 객체 생성

```kotlin
val user = User("kim")
user.someFun()
```

### 1.3. 생성자(Constructor)
#### (1) 주 생성자 (Primary Constructor; 출제)
|  항목   |           내용           |
| :---: | :--------------------: |
| 선언 위치 |        클래스 선언부         |
|  키워드  | `constructor` (생략 가능)  |
|  개수   |       클래스당 최대 1개       |
| 필수 여부 | 필수 아님; 없으면 컴파일러가 자동 생성 |

```kotlin
class User constructor() { }	// 명시적 선언
class User() { }				// constructor 생략
class User { }					// 매개변수 없는 주 생성자 자동 생성
```

1. **기본 매개변수 (지역 변수)**
	- 생성자 매개변수는 기본적으로 **생성자 내부(init)에서만 접근 가능**
	- 일반 멤버 함수에서 접근 불가 → 오류 발생

2. **var / val 키워드로 멤버 변수 선언**
	- 주 생성자 매개변수에 `var`/`val` 키워드를 붙이면 **클래스 멤버 변수**로 승격
	- 일반 메소드 매개변수에는 불가, **주 생성자만 가능**

```kotlin
class User(val name: String, val count: Int) {	// 클래스의 멤버 변수
	fun someFun() {
		println("name : $name, count : $count")	// 성공
	}
}
```

3. **초기화 블록** (init)
	- 주 생성자는 본문이 없으므로, `init { }` 형태로 주 생성자의 본문 구현
	- 객체 생성 시 **자동 실행**
	- 파라미터와 반환값 없음 (함수가 아님)
	- 주 생성자 본문이 불필요하면 선언 안 해도 됨

```kotlin
class User(name: String, count: Int) {
    init {
        println("i am init....")
    }
}
// User("kkang", 10) 호출 시 → "i am init..." 출력
```

#### (2) 보조 생성자
- **보조 생성자** (Secondary Constructor)
	- 클래스 **본문** 안에 `constructor` 키워드로 선언
	- **여러 개** 선언 가능

```kotlin
class User {
    constructor(name: String) {
        println("constructor(name: String) call...")
    }
    constructor(name: String, count: Int) {
        println("constructor(name: String, count: Int) call...")
    }
}
```

- 보조 생성자와 주 생성자 연결
	- 주 생성자가 있는 상태에서 보조 생성자 사용 시, `this()`로 주 생성자 반드시 호출
	- 생성자 호출 순서: **주 생성자 → init 블록 → 보조 생성자**
	- 보조 생성자가 여러 개인 경우, `this()`로 다른 보조 생성자를 통해 연결 가능

```kotlin
class User(name: String) {
    constructor(name: String, count: Int): this(name) {
        // 주 생성자 먼저 실행됨
    }
    constructor(name: String, count: Int, email: String): this(name, count) {
        // 앞의 보조 생성자를 거쳐 주 생성자 호출
    }
}
```

> **핵심**: 보조 생성자로 객체를 생성하면, 어떤 방식으로든 **주 생성자가 반드시 호출**되어야 함

## 2. 상속 (클래스 재사용)
- **상속의 장점**
	- 상위 클래스의 **변수와 함수를 하위 클래스에서 자신의 멤버처럼** 사용 가능

### 2.1. 상속과 오버라이딩
1. 코틀린 클래스는 기본적으로 **상속 불가 (final)**
	- ==상속 허용하려면 상위 클래스 및 멤버에 `open` 키워드 필요==

2. 선언부에 `: 상위클래스명` 형태로 상속
	- 하위 클래스 생성자에서 **상위 클래스 생성자 반드시 호출**
```kotlin
open class Super(name: String) { }

// 방법 1: 주 생성자에서 상위 클래스 호출
class Sub(name: String): Super(name) { }

// 방법 2: 보조 생성자에서 super()로 호출
class Sub: Super {
    constructor(name: String): super(name) { }
}
```

3. 오버라이딩 (Overriding / 재정의)
	- 상위 클래스 멤버(변수 또는 함수)를 같은 이름으로 하위 클래스에서 재선언
	- 코틀린은 **변수도 오버라이딩 가능**
	- ==하위 클래스에서 재정의할 때는 `override` 키워드를 사용==
```kotlin
open class Super {
    open var someData = 10
    open fun someFun() { println("i am super class function : $someData") }
}
class Sub: Super() {
    override var someData = 20
    override fun someFun() { println("i am sub class function : $someData") }
}
// Sub().someFun() → "i am sub class function : 20"
```

- *연습 문제*
```kotlin
class Test1() {
// open class Test1() {
	var test = 1
    // open var test = 1
	init {
		println("init")
	}
	constructor(name: String): this() {
		println("secondary: $name")
	}
}
class Test2: Test1() {
	val test = 1
	// override var test = 1
	init {
		println("Test2 init")
	}
}
```

### 2.2. 접근 제한자 (Access Modifiers)
|     접근 제한자     | 최상위(클래스 외부)에서 이용 |     클래스 멤버에서 이용     |
| :------------: | :--------------: | :-----------------: |
| `public` (기본값) |    모든 파일에서 가능    |     모든 클래스에서 가능     |
|   `internal`   |   같은 모듈 내에서 가능   |    같은 모듈 내에서 가능     |
|  `protected`   |    **사용 불가**     | 상속 관계의 하위 클래스에서만 가능 |
|   `private`    |   파일 내부에서만 이용    |    클래스 내부에서만 이용     |
> **주의**: `protected`는 최상위(top-level)에서 사용 불가

```kotlin
open class Super {
    var publicData = 10         // public (기본)
    protected var protectedData = 20
    private var privateData = 30
}
class Sub: Super() {
    fun subFun() {
        publicData++     // 성공
        protectedData++  // 성공
        privateData++    // 오류!
    }
}
fun main() {
    val obj = Super()
    obj.publicData++     // 성공
    obj.protectedData++  // 오류!
    obj.privateData++    // 오류!
}
```

## 3. 코틀린의 클래스 종류
### 3.1. 데이터 클래스 (Data Class)
- `data` 키워드로 선언
- VO(Value-Object) 클래스 용도로 편리하게 사용
- **주 생성자에 선언한 멤버 변수**를 대상으로 아래 함수 자동 생성

|   자동 제공 함수   |                    동작                    |
| :----------: | :--------------------------------------: |
|  `equals()`  | 주 생성자 멤버 변수의 **값**을 비교 *(일반 클래스는 주소 비교)* |
| `toString()` |        주 생성자 멤버 변수의 데이터를 문자열로 반환         |

```kotlin
class NonDataClass(val name: String, val email: String, val age: Int)
data class DataClass(val name: String, val email: String, val age: Int)

val non1 = NonDataClass("kkang", "a@a.com", 10)
val non2 = NonDataClass("kkang", "a@a.com", 10)
val data1 = DataClass("kkang", "a@a.com", 10)
val data2 = DataClass("kkang", "a@a.com", 10)

println(non1.equals(non2))		// false	(주소 비교)
println(data1.equals(data2))	// true		(값 비교)
```

#### (1) toString() 비교
```kotlin
data class DataClass(val test1: String, var test2: String) {
    val test3 = 10   // 본문 멤버 → toString() 대상 아님
}
println(DataClass("1","2").toString())
// 출력: DataClass(test1=1, test2=2)
// (test3는 주 생성자 멤버가 아니므로 제외)
```

#### (2) equals()에서 주 생성자 멤버만 비교되는 예
```kotlin
data class DataClass(val name: String, val email: String, val age: Int) {
    lateinit var address: String
    constructor(name: String, email: String, age: Int, address: String): this(name, email, age) {
        this.address = address
    }
}
val obj1 = DataClass("kkang", "a@a.com", 10, "seoul")
val obj2 = DataClass("kkang", "a@a.com", 10, "busan")
println(obj1.equals(obj2))  // true (address는 보조 생성자 멤버 → 비교 제외)
```

### 3.2. 오브젝트 클래스 (Object Class; 익명 클래스)
- **익명 클래스** (클래스 이름 없음)를 만들 목적으로 사용
- `object` 키워드로 **선언과 동시에 객체 생성**
- 클래스명이 없으므로 반복 객체 생성 불가

```kotlin
val obj = object {
    var data = 10
    fun some() { println("data : $data") }
}
// 타입을 지정하지 않으면 멤버 접근 시 오류!
```

#### (1) 타입 지정 방법 (상위 클래스 또는 인터페이스 명시)
```kotlin
open class Super {
    open var data = 10
    open fun some() { println("i am super some() : $data") }
}
val obj: Super = object: Super() {
    override var data = 20
    override fun some() { println("i am object some() : $data") }
}
obj.data = 30    // 성공
obj.some()       // 성공 → "i am object some() : 30"
```

### 3.3. 컴패니언 클래스 (Companion Object)
- **객체 생성 없이** 클래스 이름으로 멤버에 접근할 때 사용
- Java의 `static` 키워드 역할 (코틀린은 `static` 미지원)
- `companion object { }` 형태로 클래스 내부에 선언

```kotlin
class MyClass {
	companion object {
		var data = 10
		fun some() { println(data) }
	}
}
fun main() {
	MyClass.data = 20	// 클래스명으로 직접 접근 (성공)
	MyClass.some()		// 클래스명으로 직접 호출 (성공)
}
```
