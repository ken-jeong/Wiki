---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-10-01
---
## 1. 자바스크립트 객체 (Objects)
> 현실 세계의 사물처럼 **데이터**(속성, Property)와 **동작**(메서드, Method)을 하나로 묶은 단위이다.

- *객체 지향 프로그래밍*(OOP: Object-Oriented Programming)
	- 실제 세계가 객체들로 구성된 것처럼, 소프트웨어도 객체로 구성하는 방법

### 1.1. 객체의 종류
- **내장 객체 (Built-in)**
	- 기본 내장(Standard Built-in): JS 엔진에 포함된 객체
		- Date, String, Array, Object, Number, Boolean, JSON, Math, Reflect
	- 브라우저 객체
		- **DOM**(Document Object Model): 브라우저가 HTML 문서를 파싱하여 요소들을 객체 트리로 정의
		- **BOM**(Browser Object Model): 브라우저 관련 정보 제공

- **사용자 정의 객체 (Custom)**
	- 개발자가 직접 정의한 객체

- *사용자 정의 객체 생성 방법 (4가지)*

| 방법                | 생성 개수       | 비고                    |
| ----------------- | ----------- | --------------------- |
| 객체 리터럴 `{}`       | 하나 (싱글톤)    | 가장 많이 사용, 권장          |
| `new Object()`    | 하나 (싱글톤)    | 빈 객체 생성 후 속성 추가       |
| 생성자 함수 + `new`    | 여러 개        | `this`로 속성 할당         |
| `class` (ES6)     | 여러 개        | Java/Python과 유사한 문법   |

### 1.2. 싱글톤 객체 생성 (리터럴, Object)
- **객체 리터럴 `{}`**
	- 하나의 명령문으로 객체를 정의하고 생성, 가장 많이 사용
	- 객체를 하나만 생성(싱글톤, Singleton)할 때 유용
	- 빈 객체: `let car = {};`

- **Object 생성자 `new Object()`**
	- 빈 객체 생성 후 속성 추가
	- (싱글톤이라면 리터럴 방식을 더 권장)
	- *JS 엔진은 객체 리터럴을 만나면 내부적으로 Object 생성자 함수를 사용해 객체를 생성*

```js
// 객체 리터럴 (권장)
const person1 = {
  name: "Alice",
  sayHello: function() {
    console.log(`Hello, I'm ${this.name}!`);
  }
};

// new Object() (JS 엔진)
const person2 = new Object();
person2.name = "Bob";
person2.sayHello = function() {
  console.log(`Hello, I'm ${this.name}!`);
};
```

### 1.3. 여러 객체 생성 (생성자 함수, class)
- **생성자 함수**
	- `function` 키워드로 정의, `new` 연산자로 여러 객체(인스턴스) 생성 가능
	- `this`에 연결된 속성·메서드는 **public** (외부에서 참조 가능)
	- 생성자 함수 내에서 선언된 일반 변수는 **private** (내부에서만 참조 가능)
	- `this` 바인딩
		- 생성자 함수 호출: `this`는 생성되는 **인스턴스**
		- 일반 함수 호출: `this`는 브라우저 환경에서 **window** 객체

- **Class (ES6)**
	- ECMAScript 2015부터 `class` 키워드 도입
	- Java나 Python의 클래스와 동일한 개념

```js
// 생성자 함수로 객체 정의
function Person(name) {
  this.name = name;
  this.sayHello = function() {
    console.log(`Hello, I'm ${this.name}!`);
  };
}
const person3 = new Person("Charlie");

// class 키워드로 클래스 정의 (ES6~)
class PersonClass {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log(`Hello, I'm ${this.name}!`);
  }
}
const person4 = new PersonClass("Diana");
```

### 1.4. 속성 접근 및 관리
- **접근**
	- 점 표기법(Dot Notation, `obj.key`) 또는 괄호 표기법(Bracket Notation, `obj['key']`)
	- (키에 공백이 있거나 변수로 접근 시 괄호 표기법 사용, 여러 단어 키는 따옴표로 묶음)

- **추가/삭제**
	- 값을 할당하여 추가
	- `delete` 키워드로 삭제

- **탐색**
	- `in` 연산자로 키 존재 여부 확인: `"key" in obj`
	- `for...in` 루프로 모든 속성 순회: `for (let key in obj)`

```js
let user = {
	name: "John",
	age: 30,
	"like birds": true   // 여러 단어 키는 따옴표 필요
};

user.hobby = "coding";      // 추가 - Dot
user["sex"] = "male";       // 추가 - Bracket
delete user.age;            // 삭제 - Dot
delete user["like birds"];  // 삭제 - Bracket

console.log("hobby" in user); // true
for (let prop in user) {
	console.log(prop);          // name, hobby, sex
}
```

## 2. 객체 상속 (Inheritance)
### 2.1. 프로토타입(Prototype) 기반 상속
- ==프로토타입 상속==
	- JS는 다른 객체를 프로토타입으로 삼아 그 객체의 속성/메서드에 접근할 수 있는 식으로 상속한다.
	- 모든 객체는 숨겨진 **프로토타입** 속성을 가지며, 이를 통해 다른 객체(프로토타입)를 참조한다.
	- 생성자 함수의 `prototype` 객체를 통해 상속을 구현한다.

- **`prototype` vs `__proto__`**
	- `prototype`: 함수 객체가 가진, 자신의 프로토타입 객체를 가리키는 링크
	- `__proto__`: 객체 간의 상속 관계를 표현하는 링크
	- `new Point()`로 만든 모든 객체(p1, p2)는 같은 `Point.prototype`을 참조한다.

- ==프로토타입 체인==
	- JS는 객체의 속성/메서드를 찾을 때 먼저 객체 자신에게서 찾고,
	- 없으면 그 객체의 프로토타입에서 찾는다. (`__proto__` 링크를 따라감)
	- 그래도 없으면 그 프로토타입의 프로토타입에서 찾는 식이다.
	- 예: `p1` → `Point.prototype` → `Object.prototype`

- *cf.*
	- 상속이 실제로는 프로토타입을 공유하며, 따라서 공간이 절약된다.

```js
function Point(xpos, ypos) {
	this.x = xpos;
	this.y = ypos;
}
Point.prototype.getDistance = function () {
	return `${this.x}, ${this.y} + getDistance`;
};
const p1 = new Point(10, 20);
const p2 = new Point(50, 80);
p2.getDistance(); // p2에 없으므로 Point.prototype에서 찾아 실행
```

- *프로토타입으로 상속 구현*
```js
function Vehicle(make, model) {
	this.make = make;
	this.model = model;
}
Vehicle.prototype.getDetails = function() {
	return `${this.make} ${this.model}`;
};
Vehicle.prototype.drive = function() {
	return `${this.make} ${this.model}, drive()`;
}
function Car(make, model, doors) {
	Vehicle.call(this, make, model);
	this.doors = doors;
}
Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;
Car.prototype.getDetails = function() {
	return `${this.make} ${this.model}, ${this.doors} doors`;
};
const car = new Car('Benz', 'E300', 4); console.log(car.getDetails());
console.log(car.drive());
```

### 2.2. 클래스(Class) 상속 (ES6)
- `extends` 키워드를 사용하여 상속 구현
- `super()`로 부모 생성자 호출
- **오버라이딩(Overriding)**: 부모의 메서드를 자식 클래스에서 재정의하여 사용 가능

```js
class Vehicle {
	constructor(make, model) {
		this.make = make;
		this.model = model;
	} // 메서드는 Vehicle.prototype에 생성
	getDetails() {
		return `${this.make} ${this.model}`;
	}
	drive() {
		return `${this.make} ${this.model}, drive()`;
	}
} 

class Car extends Vehicle {
	constructor(make, model, doors) {
		super(make, model); // 상위 클래스 생성자
		this.doors = doors;
	}
	getDetails() { // 메서드 오버라이딩
		return `${this.make} ${this.model}, ${this.doors} doors`;
	}
}
const car = new Car('Benz', 'E300', 4); console.log(car.getDetails());
console.log(car.drive()); 
```

## 3. 자바스크립트 배열 (Arrays)
### 3.1. 배열의 특징
- **해시 테이블**(Hash Table)로 구현된 객체이다.
	- 인덱스로 접근하는 경우 일반 배열보다 느림
	- 특정 요소 탐색, 삽입/삭제는 일반 배열보다 빠름

- **희소 배열**(sparse array)
	- 요소를 위한 메모리 공간이 동일한 크기가 아니어도 되며, 연속적이지 않을 수 있다.
	- cf. **밀집 배열**(dense array): 동일한 크기의 메모리 공간이 연속적으로 나열된 일반적인 배열

- 하나의 배열에 **여러 가지 자료형(숫자, 문자, 객체 등)을 혼합**하여 저장할 수 있다.

- 크기가 동적으로 조절된다.
	- *현재 배열의 크기보다 큰 인덱스를 사용할 수 있다. 이 때 배열의 크기가 늘어나며, 인덱스에 대응하는 요소가 없는 부분인 홀(hole)이 생기면 undefined처럼 보인다.*
```js
let myArray = new Array();
myArray[0] = "apple";
myArray[99] = "banana"; // length는 100, 1~98은 홀(undefined)

let mixed = ["apple", new Date(), 3.14]; // 자료형 혼합
```

### 3.2. 배열 생성
- ==생성==: 배열 리터럴 `[]` 또는 `new Array()`
- **속성**: `length` (배열의 크기)
```js
// 배열 리터럴
let fruits = ["apple", "banana", "cherry"];

// Array 객체
let myArray = new Array();
myArray[0] = "apple";
myArray[1] = "banana";
myArray[2] = "cherry";

let fruitsArray = new Array("apple", "banana", "cherry");

for (let i = 0; i < fruits.length; i++) {
	console.log(fruits[i]);
}
```

### 3.3. 배열 메서드
- **추가/제거**
	- `push()`, `pop()`: 배열 끝에 추가 / 끝 요소 제거 후 반환
	- `unshift()`, `shift()`: 배열 앞에 추가 / 첫 요소 제거 후 반환
	- `splice()`: 배열 요소를 제거/삽입 (원본 변경)

- **변환/검색**
	- `slice()`: 배열의 일부를 복사하여 새로운 배열로 반환
	- `concat()`: 배열 합치기
	- `join()`: 배열 요소를 하나의 문자열로 결합 (기본 구분자 `,`)
	- `indexOf()`: 값으로 인덱스 찾기 (없으면 `-1`)
	- `sort()`: 정렬 (기본은 문자열(알파벳) 순서, 비교 함수로 숫자 정렬)

```js
let joined = [5, 15, 25].concat([3, 13, 23]); // [5, 15, 25, 3, 13, 23]
joined.push(100);                // [5, 15, 25, 3, 13, 23, 100]
joined.pop();                    // 100 반환
joined.shift();                  // 5 반환
joined.unshift(50);              // [50, 15, 25, 3, 13, 23]
joined.slice(3);                 // [3, 13, 23]
joined.sort((a, b) => b - a);    // b-a 내림차순, a-b 오름차순 → [50, 25, 23, 15, 13, 3]

let fruits = ["Apple", "Banana", "Orange"];
fruits.indexOf("banana");        // -1 (대소문자 구분)
fruits.indexOf("Banana");        // 1
fruits.join();                   // "Apple,Banana,Orange"
fruits.join("");                 // "AppleBananaOrange"
fruits.join("+");                // "Apple+Banana+Orange"
delete fruits[1];                // [ 'Apple', <1 empty item>, 'Orange' ] (홀 생성)
```

### 3.4. 고차 함수 (filter, map)
- 콜백 함수를 받아 내부적으로 순회하며 처리하므로 `for`문 없이 데이터 처리가 가능하다.
	- **`filter()`**: 콜백의 반환값이 true인 요소만 모아 새로운 배열 반환
	- **`map()`**: ==모든 요소를 함수로 변환하여 새로운 배열 반환==
- 완전한 구문: `arr.filter(function(element, index, array) { }, thisArg)` (`map`도 동일)
	- `element`: 현재 요소, `index`: 위치, `array`: 전체 배열, `thisArg`: 콜백 내부의 `this`
```js
let num = [1, 2, 3, 4, 5];
let isEven = num.filter(function(value) {
	return value % 2 === 0;  // 짝수이면 true
});                          // [2, 4]

let fruits = ["Apple", "Banana", "Orange"];
let indexed = fruits.map((item, index) => `${index}: ${item}`);
// ["0: Apple", "1: Banana", "2: Orange"]

// 객체 배열에서 map()
const students = [{ id: '20230001', name: "한공대" }, { id: '20230002', name: "인공지능" }];
students.map((item, index) => `${index}: 학번(${item.id}), 이름(${item.name})`);
```

- *map, filter를 콜백 함수로 직접 구현*
```js
let arr = [1, 2, 3, 4, 5];

function map(func) {
	const result = [];
	for (let i = 0; i < arr.length; i++) {
		result.push(func(arr[i], i, arr));
	}
	return result;
}
map((item) => item * 2);  // [2, 4, 6, 8, 10]

function filter(func) {
	const result = [];
	for (let i = 0; i < arr.length; i++) {
		if (func(arr[i], i, arr)) result.push(arr[i]);
	}
	return result;
}
filter((item) => item % 2 === 0);  // [2, 4]
```

### 3.5. 다차원 배열
- 배열 안에 배열을 포함하여 2차원, 3차원 배열 생성 가능 (중첩 배열)
```js
let twoDimension = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];      // 2차원 배열
let threeDimension = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]; // 3차원 배열

twoDimension[0][2];        // 3
threeDimension[0][1][1];   // 4
```

```js
let multiArray = [];
for(let i = 0; i < 3; i++) {
	multiArray[i] = [];
	for(let j = 0; j < 3; j++) {
		multiArray[i][j] = i + j;
	}
}
console.log(multiArray) // [ [ 0, 1, 2 ], [ 1, 2, 3 ], [ 2, 3, 4 ] ]

multiArray[1].push(7);
console.log(multiArray); // [ [ 0, 1, 2 ], [ 1, 2, 3, 7 ], [ 2, 3, 4 ] ]

multiArray.pop();
console.log(multiArray); // [ [ 0, 1, 2 ], [ 1, 2, 3, 7 ] ]

multiArray[1].shift();
console.log(multiArray); // [ [ 0, 1, 2 ], [ 2, 3, 7 ] ]
```

## 4. 분할 대입과 인자 전달
### 4.1. 분할 대입 (Destructuring Assignment)
- ==객체나 배열에서 값을 간편하게 추출하여 변수에 할당하는 문법==
	- **객체**: `let { id, name } = student;` (변수 선언부에 `{}`, 키 이름과 일치하는 속성 추출)
	- **배열**: `let [ id, name ] = student;` (변수 선언부에 `[]`, 저장된 순서대로 할당)

```js
// 객체 분할 대입
const user = {name: "Alice", age: 25};
const {name, age} = user;
console.log(`name: ${name}, age: ${age}`);

// 배열 분할 대입
const arr = [1, 2, 3];
const [a, b, c] = arr;
console.log(`a: ${a}, b: ${b}, c: ${c}`);
```

### 4.2. 인자 전달 방식
- **Call by Value (기본형)**
	- 숫자, 문자열, 불리언, null, undefined, symbol은 값이 복사되어 전달
	- (매개변수를 변경해도 원본 영향 없음)

- **Call by Reference (참조형)**
	- 객체, 배열, 함수는 **주소 값**이 매개변수에 복사되어 전달
	- 매개변수와 실인자가 같은 객체를 공유 → 함수 내에서 속성 변경 시 **원본 객체도 변경**됨

```js
const swap = (a, b) => { let tmp = a; a = b; b = tmp; };
let x = 5;
let y = 100;
swap(x, y);
console.log(x, y); // 5, 100

let func = function(b) { b = b + 10; }
x = 20;
func(x);
console.log(x); // 20

let funcA = function(b) { b.a = 5; };
x = {a: 1};
funcA(x);
console.log(x); // { a: 5 }

let funcB = (b) => { b = b.a = 5; b.b = 7; };
y = {c: 9};
funcB(y);
console.log(y); // { c: 9, a: 5 }
```
- *funcB 해석* (대입은 오른쪽부터)
	1. `b.a = 5`: 원본 객체에 `a: 5` 추가
	2. `b = 5`: 매개변수 `b`가 기본형 5를 가리키도록 변경 (원본과 연결 끊김)
	3. `b.b = 7`: 기본형에 속성을 할당했으므로 무시됨

## 5. 주요 내장 객체
### 5.1. Date 객체
- 날짜와 시간을 저장 및 관리
- 생성
```js
new Date()                 // 현재 날짜와 시간
new Date(milliseconds)     // 1970/01/01 이후의 밀리초
new Date(dateString)       // 다양한 문자열
new Date(year, month, date[, hours[, minutes[, seconds[, ms]]]]) // 상세 날짜
```
- 메서드
	- `getFullYear()`, `getMonth()`, `getDate()`, `getDay()`, `getHours()`, `getMinutes()`, `getSeconds()`, `getMilliseconds()`: 연도, 월, 일, 요일, 시, 분, 초, 밀리초 반환
		- 주의: `getMonth()`는 0~11, `getDay()`는 0(일)~6(토)
	- `getTime()`: 1970년 1월 1일 이후 현재까지의 밀리초
	- `getTimezoneOffset()`: UTC와 현지 시간의 차이(분)
	- `setFullYear()`, `setMonth()`, `setDate()`, `setHours()` 등: 특정 시점의 값 설정
```js
// 1초마다 갱신되는 시계
function setClock() {
	let now = new Date();
	let s = now.getHours() + ':' + now.getMinutes() + ':' + now.getSeconds();
	document.getElementById('clock').innerHTML = s;
	setTimeout(setClock, 1000);
}
setClock();
```

### 5.2. String 객체
- 문자열 처리를 위한 객체
- 문자열 리터럴(`"ABCD"`)과 문자열 객체(`new String("ABCD")`) 2종류가 있음
- 속성: `length`

| 메서드                        | 설명                                        |
| -------------------------- | ----------------------------------------- |
| `charAt(pos)`              | 해당 인덱스의 문자 반환                             |
| `charCodeAt(pos)`          | 해당 인덱스 문자의 유니코드(UTF-16) 번호 반환             |
| `concat(args)`             | 2개 이상의 문자열을 하나로 합침                        |
| `indexOf(search, pos)`     | 찾는 문자열이 시작되는 인덱스 반환                       |
| `lastIndexOf(search, pos)` | 특정 문자열이 마지막에 등장하는 인덱스 반환                  |
| `match(regExp)`            | 정규 표현식에 맞는 문자열을 찾아 배열로 반환                 |
| `replace(regExp, replace)` | 특정 문자열을 지정한 문자열로 바꾼 새 문자열 반환              |
| `search(regExp)`           | 정규 표현식에 맞는 문자열이 처음 등장하는 인덱스 반환            |
| `slice(start, end)`        | 시작~종료 위치를 잘라 반환 (음수면 끝에서부터)               |
| `split(separator, limit)`  | 구분자를 기준으로 분리해 배열로 반환                      |
| `substr(start, length)`    | 시작 인덱스부터 길이만큼 추출 (비권장)                    |
| `substring(start, end)`    | `slice()`와 같으나 음수를 쓸 수 없음                 |
| `toLowerCase()`            | 모두 소문자로                                   |
| `toUpperCase()`            | 모두 대문자로                                   |
| `trim()`                   | 양 끝의 공백과 줄 바꿈 문자(LF, CR 등)를 제거한 새 문자열 반환 |

```js
let str = new String("JavaScript And React");
str.charAt(0);                 // J
str.concat(" Programming");    // JavaScript And React Programming
str.indexOf("A");              // 11
str.indexOf("React");          // 15
str.slice(4, 10);              // Script
str.substr(4, 6);              // Script
str.toLowerCase();             // javascript and react
str.replace("And", "&");       // JavaScript & React
" T U K O R E A ".trim();      // "T U K O R E A"
str.split(" ");                // ["JavaScript", "And", "React"]
```

### 5.3. Math 객체
- 수학 연산을 위한 속성과 메서드 제공
- `new`로 생성하지 않고 **정적(Static)으로 바로 사용** (예: `Math.PI`, `Math.random()`, `Math.round()`)
- 속성: `E`(자연로그 밑, 2.718), `LN2`(0.693), `LN10`(2.303), `PI`(3.14159), `SQRT2`(1.414), `SQRT1_2`(1/√2, 0.707)
- 메서드
	- `abs(x)`: 절댓값
	- `ceil(x)`: x 이상의 가장 작은 정수 (올림), `floor(x)`: x 이하의 가장 큰 정수 (내림)
	- `round(x)`: 소수점 첫째 자리에서 반올림
	- `max(...)`: 가장 **큰** 수, `min(...)`: 가장 **작은** 수
	- `pow(x, y)`: x의 y승, `sqrt(x)`: 제곱근
	- `exp(x)`: eˣ, `log(x)`: 자연로그(ln x)
	- `sin(x)`, `cos(x)`, `tan(x)`, `asin(x)`, `acos(x)`, `atan(x)`: 삼각함수 (라디안 단위)
	- `random()`: 0 이상 1 미만의 난수
```js
Math.sin((x * Math.PI) / 180.0); // 도(degree) → 라디안 변환 후 sin
```

## 6. 예외 처리 (Exception Handling)
### 6.1. try-catch-finally
- **예외**(exception): 프로그램 실행 중에 발생하는 런타임 오류

- **목적**
	- 런타임 오류(예외)를 처리하여 프로그램이 비정상 종료되는 것을 방지

- **구조**
	- `try { ... }`: 예외가 발생할 가능성이 있는 코드
	- `catch (e) { ... }`: 예외 발생 시 실행될 코드 (에러 처리)
	- `finally { ... }`: 예외 발생 여부와 상관없이 무조건 실행되는 코드
```js
try {
	// 예외를 처리하길 원하는 실행 코드;
	if (/* 조건 */) throw "예외 내용";
} catch (ex) {
	// 예외가 발생할 경우에 실행될 코드;
} finally {
	// try 블록이 종료되면 무조건 실행될 코드;
} 
```

### 6.2. throw
- 개발자가 정한 기준에 맞지 않으면 의도적으로 예외를 발생시킬 때 사용 (`throw "에러 메시지"`)
```js
const sum = (x, y) => {
	if (typeof x !== 'number' || typeof y !== 'number') {
		throw "숫자를 입력하세요"
	}
	return x + y;
};

try {
	sum("50", 10)
} catch (e) {
	console.log(e); // 숫자를 입력하세요
}
```

- *예제: 숫자 맞히기*
```js
let solution = 53;
function test() {
	try {
		let x = document.getElementById("number").value;
		if (x == "") throw "입력없음";
		if (isNaN(x)) throw "숫자가 아님";
		if (x > solution) throw "너무 큼";
		if (x < solution) throw "너무 작음";
		if (x == solution) throw "성공";
	} catch (error) {
		document.getElementById("message").innerHTML = "힌트: " + error;
	}
}
```
