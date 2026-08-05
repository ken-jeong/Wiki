---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-09-24
---
## 1. 자바스크립트 개요
### 1.1. 역사와 표준
- 넷스케이프의 브랜든 아이크가 개발(초기명 LiveScript), 1995년 JavaScript로 변경
- 1997년 ECMA에서 ECMA-262로 표준화(ECMAScript)
	- JavaScript는 ECMAScript 사양을 준수하는 범용 스크립트 언어
- 최신 표준의 분기점은 **ES6 (ES2015)**
- 구형 브라우저 호환을 위해 **Babel** 같은 트랜스파일러를 사용한다.
	- ES6 코드 → ES5 코드로 변환, React JSX → JS로 변환

### 1.2. 특징
- **인터프리터 언어**
	- 브라우저가 소스 코드를 즉시 해석 및 실행

- **동적 타이핑**
	- 변수 선언 시가 아닌 할당 시 타입 결정
	- (TypeScript는 ES6의 확장으로 정적 타이핑 지원)

- **객체 기반**
	- 객체는 선언과 생성이 동시에 처리
	- 프로토타입 기반으로 객체가 공유하는 메서드 정의

- **함수형 프로그래밍**
	- 함수를 객체로 취급
	- 화살표 함수(arrow function): 람다식(lambda expression) 지원
	- 콜백 함수(callback function): 함수를 인수로 전달
	- 고계 함수(higher-order function): 함수를 인수로 받거나 함수를 반환하는 함수

### 1.3. 활용
- HTML/CSS 동적 제어, 이벤트 처리, 서버 통신(AJAX)

- *cf.*
	- 사용자의 요청인 이벤트에 반응하는 동작을 구현할 수 있다.
	- HTML 컨텐츠를 동적으로 생성, 삭제, 변경할 수 있다.
	- HTML 요소(element)의 속성(CSS)들을 동적으로 변경할 수 있다.
	- 사용자 입력 값들을 계산 또는 검증할 수 있다.
	- 게임, 애니메이션과 같은 대화형 컨텐츠를 구현할 수 있다.
	- 웹 서버와의 통신을 구현할 수 있다.

### 1.4. 확장 기술
- *jQuery*
	- 자바스크립트 라이브러리이며 클라이언트 스크립트를 단순화 할 수 있도록 설계
	- 점차적으로 프론트엔드 프레임워크(가상 돔 활용으로 성능 향상)의 등장으로 필요성 저하

- *JSON*(JavaScript Object Notation)
	- 속성-값 쌍으로 이루어진 데이터 오브젝트를 전달하기 위한 개방형 표준 포맷
	- 브라우저/서버 통신(AJAX)을 위해, 넓게는 XML을 대체하는 주요 데이터 포맷

- *React.js, Vue.js, Angular.js*
	- 프론트엔드 프레임워크
	- 현재 가장 인기 있는 반응형 프레임워크

- *Node.js 기반*: Express.js, Next.js, NestJS(TypeScript)
	- 백엔드 프레임워크
	- 서버 환경에서 자바스크립트로 애플리케이션을 작성할 수 있게 한다.

## 2. 자바스크립트 시작하기
### 2.1. 코드 위치
- **내부**(`<script>` 태그): `<head>` 또는 `<body>` 안에 위치
	- `<body>` 콘텐츠보다 **먼저** 실행 보장 → `<head>`에 위치
	- `<body>` 콘텐츠보다 **나중에** 실행 보장 → `<body>` 맨 끝에 위치
- **외부**(`<script>`의 `src` 속성)
- **인라인**(HTML 태그 내 이벤트 속성)

```html
<head>
	<!-- 내부 자바스크립트 -->
	<script>
		// 주석
		/* 주석 */
		document.write("Hello World!");
	</script>
	
	<!-- 외부 자바스크립트 -->
	<script src="myscript.js"></script>
</head>

<body>
	<!-- 인라인 자바스크립트 -->
	<button type="button" onclick="alert('반갑습니다.')">
		버튼을 누르세요!
	</button>
</body>
```

### 2.2. 문장 규칙
- 웹 브라우저에게 내리는 명령으로, 순차적으로 실행
- 블록(`{}`) 단위로 묶을 수 있음
- **대소문자를 구별**
- 주석: 단일 `// ...`, 다중 `/* ... */`

### 2.3. 실행과 디버깅
- 터미널에서 JS 파일 실행
```sh
node var.js # 터미널 실행
```

- *VS Code 디버깅*
	- `[실행] → [디버깅 시작]`(F5) → 디버거 선택
		- HTML 내 JS 디버깅: **웹 앱(Chrome)** 선택
		- JS 파일 디버깅: **Node.js** 선택
	- 중단점 설정/해제: `F9`

## 3. 변수
### 3.1. 선언 키워드 (var, let, const)
- **선언 방식**
	- 값을 저장하는 저장소, 자료형을 지정하지 않는다.
	- (`let`, `const`, `var`)

- **키워드 비교**
	- **var**: 재선언/재할당 가능. 구형 방식
		- *문제점: 같은 변수명으로 재선언할 수 있고, 무조건 재할당 가능*
		- → 2015년 ES6에서 `let`, `const` 추가
	- **let (ES6)**: 재할당 가능, **재선언 불가능**
	- **const (ES6)**: **재할당/재선언 불가능** (상수)
	- 스코프·호이스팅까지 포함한 비교는 → [[#10.3. var·let·const 요약]]

```js
var value = "홍길동";
value = "강호동"; // 재할당 가능
var value = 2024; // 재선언 가능

let v1 = "홍길동";
v1 = "강호동";    // 재할당 가능
let v1 = 2024;    // SyntaxError: Identifier 'v1' has already been declared

const v2 = "홍길동";
v2 = "강호동";    // TypeError: Assignment to constant variable.
```

### 3.2. 명명 규칙
- 문자, `$`, `_`로 시작한다.
- 숫자로 시작할 수 없다.
- 대소문자를 구분한다.
- 스크립트 안에서 유일해야 한다.

### 3.3. 변수 값을 HTML에 출력
```html
<h1 id="test"></h1>
<script>
	let x = 50;
	let y = 100;
	let z = x + y;
	document.getElementById("test").innerHTML = z; // id가 test인 요소의 내용을 z로 변경
</script>
```

## 4. 자료형
### 4.1. 기본형
- **기본형** (Primitive Type)
	- 변경 불가능한(Immutable) 값
	- *재할당은 값을 수정하지 않고 변수가 가리키는 메모리 주소를 바꾼다.*

- *종류*
	- **숫자**(Number): 정수나 실수
		- Infinity: 무한값 (1/0)
		- NaN(Not a Number): 표현할 수 없는 숫자 (예: `1 * "Hello"`)
	- **문자열**(String): `""`(큰따옴표; double quote) 또는 `''`(작은따옴표; single quote)로 표현
		- ES6 **템플릿 리터럴**(Template Literal): 백틱 `` ` `` 사용
	- **불리언**(Boolean): true 또는 false
	- **Null**: 변수를 선언하고 빈 값을 할당한 상태
	- **Undefined**: 변수를 선언하고 값을 할당하지 않은 상태
	- **Symbol**: 변경 불가능한 원시 타입의 값이며 고유한 값, 객체의 프로퍼티 키로 사용

- ==템플릿 문자열의 작성==
```js
const a = 5, b = 3;
console.log(`${a} + ${b} = ${a + b}`);

const name = "홍길동";
const message = `이름은 ${name}입니다.`;
```

- *`typeof`로 자료형 확인*
```js
let x;               // undefined
x = 100;             // number
x = "자바스크립트";   // string
console.log(typeof x);
```

### 4.2. 참조형
- **참조형** (Reference Type)
	- 기본형 이외의 모든 값은 참조형이자 객체(object)형이다.
	- 변경 가능한(Mutable) 값, 크기가 동적으로 변한다.
	- 실제 데이터는 별도의 메모리 공간(Heap)에 저장하고, 변수는 그 주소를 참조한다.
		- 참조를 통해 메모리 공간의 실제 객체 속성/요소를 직접 변경
	- 종류: ==객체(Object), 배열, 함수==

- *객체*: 사물의 속성과 동작을 묶어서 표현 → [[05_JavaScript 객체와 배열#1. 자바스크립트 객체 (Objects)]]
```js
let myCar = { model: "bmw", color: "red", hp: 100 }; // 객체 리터럴
console.log(myCar.model);
```

### 4.3. 형변환
- **형변환** (Type Casting)
	- **암시적**: JS 엔진이 자동으로 변환 (문자열↔숫자, 불리언↔숫자)
	- **명시적**: `String()`, `Number()` 등을 사용해 개발자가 직접 변환
```js
// 암시적 형변환
console.log("5" * 2);          // 10
console.log(1 + "2");          // "12"
console.log(20 + "24");        // "2024" (문자열)
console.log(20 + true);        // 21 (true → 1)

// 명시적 형변환
console.log(Number("123"));    // 123
console.log(String(123));      // "123"
console.log(String(true));     // "true"
```

### 4.4. 인자 전달
- **기본형**: 불리언, 숫자, 문자열 등
	- **call by value**: 함수 내 변경 시 원본 영향 ❌

- **참조형**: 객체, 배열, 함수 등
	- **call by reference**: 함수 내 변경 시 원본 영향 ⭕

- 예제는 → [[05_JavaScript 객체와 배열#4.2. 인자 전달 방식]]

## 5. 연산자
### 5.1. 산술 연산자
| 연산자  | 설명  |  예   |     결과     |
| :--: | :-: | :--: | :--------: |
| `+`  | 덧셈  | 3+2  |     5      |
| `-`  | 뺄셈  | 3-2  |     1      |
| `*`  | 곱셈  | 3*2  |     6      |
| `/`  | 나눗셈 | 3/2  |    1.5     |
| `%`  | 나머지 | 3%2  |     1      |
| `++` | 증가  | ++x  | x의 값 3 → 4 |
| `--` | 감소  | --x  | x의 값 3 → 2 |

### 5.2. 비교 연산자
- `==`, `!=`: 값만 비교
- `>`, `<`, `>=`, `<=`: 대소 비교
- **`===`, `!==`**: **값과 데이터 타입**을 모두 엄격하게 비교
	- `===`: *타입과 값 일치할 때 참*
	- `!==`: *타입이 다르거나 값이 다르면 참*

### 5.3. 논리 연산자와 3항 연산자
- **논리 연산자**
	- `&&` (AND), `||` (OR), `!` (NOT)

- **3항 연산자**
	- `조건 ? 참일때값 : 거짓일때값`
	- 예: `max_value = (x > y) ? x : y;`

## 6. 입출력
### 6.1. 입력
- `prompt()`: *사용자에게 어떤 사항을 알려주고, 문자열을 입력할 수 있는 창을 표시한다.*
- `confirm()`: *사용자에게 어떤 사항을 알려주고, 확인/취소를 요구한다. (확인 true / 취소 false 반환)*
```js
let age = parseInt(prompt("나이를 입력하세요", "만나이로 입력합니다."));
let user = confirm("confirm()은 사용자의 답변을 전달합니다.");
```

### 6.2. 출력
- `document.write()`: *페이지 로딩 후 호출하면 전체 페이지를 다시 쓴다. (기존 요소 전부 삭제)*
- `console.log()`: *디버깅용으로 많이 사용하는 함수이다.* (크롬: 우클릭 → 검사 또는 `Ctrl+Shift+I`)
- `innerHTML`: *HTML 요소의 내용을 변경한다.*
- `document.getElementById(id)`: `id` 속성으로 HTML 요소에 접근
```js
function func() {
	let e = document.getElementById("test");
	e.style.color = "blue"; // 요소의 CSS 변경
}
```

## 7. 조건문
### 7.1. if-else
- 조건의 참/거짓에 따라 코드 분기
```js
let time = new Date().getHours();
let msg;
document.getElementById("test1").innerHTML = "time=" + time;
if (time < 12) {
	msg = "Good Morning";
} else {
	msg = "Good Afternoon";
}
document.getElementById("test2").innerHTML = msg;
```

### 7.2. switch
- 많은 코드 중 특정 값에 해당하는 케이스(case)를 선택하여 실행
- `break`가 없으면 다음 case로 이어짐, 해당 없으면 `default` 실행
```js
let day = new Date().getDay();
switch (day) {
	case 0:
	case 6:
		day = "주말";
		break;
	case 1:
	case 2:
	case 3:
	case 4:
	case 5:
		day = "주중";
		break;
}
document.getElementById("test").innerHTML = "오늘은 " + day + "입니다";
```

## 8. 반복문
### 8.1. while
- 조건이 참인 동안 반복
```js
let i = 0;
while (i < 10) {
	document.write("카운터 : " + i + "<br>");
	i++;
}
```

### 8.2. for
- 초기식, 조건식, 증감식을 이용해 정해진 횟수만큼 반복
```js
// 구구단표
document.write("<table border=2 width=50%>");
for (let i = 1; i <= 9; i++) {
	document.write("<tr>");
	document.write("<td>" + i + "</td>");
	for (let j = 2; j <= 9; j++) {
		document.write("<td>" + i * j + "</td>");
	}
	document.write("</tr>");
}
document.write("</table>");
```

### 8.3. break와 continue
- `break`: 루프 탈출
- `continue`: 현재 반복의 나머지를 생략하고 다음 반복으로

## 9. 함수
### 9.1. 함수의 정의
- 입력을 받아 특정 작업을 수행하고 결과를 반환하는 명령어들의 집합
- 매개변수 타입을 검사하지 않음

- *cf.*
	- JavaScript 내장 함수와 사용자 정의 함수로 구분된다.
	- 사용자 정의 함수는 매개변수와 인수의 개수/타입을 확인하지 않는다.
		- 인수가 매개변수보다 적다면 나머지 매개변수는 undefined로 설정된다.

### 9.2. 함수 선언식·표현식·화살표 함수
- **함수 선언식**: `function 이름() {}` - **호이스팅(Hoisting) 됨** (선언 전 호출 가능)
```js
// 함수 선언식: function을 선언하고 함수명을 기재한다.
function name(a) {
	let b = a + 10;
	return b;
}
```

- **함수 표현식**: `var 변수 = function() {}` - 호이스팅 안 됨
```js
// 함수 표현식: 변수를 선언하고 함수를 대입한다.
var func = function[name](a) { // [name] 생략 시 익명 함수
	let b = a + 10;
	return b;
};
```

- **화살표 함수 (ES6)**: `const 이름 = (매개변수) => 반환값` - 람다식 표현, 간결함
	- 매개변수가 **한 개**면 소괄호 생략 가능 (두 개 이상이면 생략 불가)
	- 처리를 **한 행으로 반환**하면 중괄호와 `return` 생략 가능
		- 단, 중괄호 없이 `return`만 쓰는 `(a, b) => return a + b;`는 오류
```js
// 화살표 함수: Java의 람다식(lambda expression)과 유사한 함수 (ES6 추가)
const funcAdd = (a, b) => {
	return a + b;
};
const funcAdd2 = (a, b) => a + b; // return 생략
const double = x => x * 2;        // 소괄호 생략
```

### 9.3. 익명 함수와 콜백 함수
- **익명 함수**(anonymous function): 함수 이름 없이 만들어 한 번만 사용하는 함수
```js
let greeting = function(name) {
	console.log("안녕하세요 " + name);
};
greeting("홍길동");
```

- **콜백 함수**: 다른 함수의 인자로 전달되어 나중에 호출되는 함수
```js
function mainFunc(callback) {
	console.log("main Function");
	callback();
}
function subFunc() {
	console.log("sub Function");
}
mainFunc(subFunc);

// 익명 함수를 콜백으로 전달
function loop(callback) {
	for (let i = 0; i < 3; i++) callback(i);
}
loop(function(i) { console.log(`${i}번째 함수 호출`); });
```

### 9.4. 내장 함수
- `eval(string)`: 문자열을 계산/실행하여 결과 반환 → `eval("1+2+3")` = 6
- `parseInt()`, `parseFloat()`: 문자열을 정수/실수로 변환 → `parseInt("123.45")` = 123, `parseFloat("123.45")` = 123.45
- `setTimeout(함수, 지연시간ms)`: 일정 시간 후 함수 호출 → `setTimeout(myfunc, 1000)` (1초 후)

### 9.5. 함수 요약
|     구분     | 세미콜론 | 익명  | 호이스팅 |                 예시                  |
| :--------: | :--: | :-: | :--: | :---------------------------------: |
| **함수 선언식** |  ❌   |  ❌  |  ⭕   |       `function func(a) { }`        |
| **함수 표현식** |  ⭕   |  ⭕  |  ❌   | `var func = function[func](a) { };` |
| **화살표 함수** |  ⭕   |  ⭕  |  ❌   |   `const func = (a, b) => a + b;`   |

## 10. 스코프와 호이스팅
### 10.1. 스코프(scope)
> 변수와 함수가 접근 가능한 범위이다. `var`는 함수 스코프, `let/const`는 블록 스코프

- **전역 스코프**(global scope)
	- 함수 외부에 선언되는 전역 변수는 어디서든 접근할 수 있다.
	- JS는 시작점(Entry Point)이 없다.
	- 함수를 제외한 영역(if/for/while 등)에서 ==var로 선언한 변수==는 전역 변수로 취급된다.

- **지역/함수 스코프**(local or function scope)
	- 해당 함수 내에서만 접근할 수 있다.

- **블록 레벨 스코프**(block level scope)
	- 해당 변수가 선언된 중괄호 블록에서만 접근할 수 있다.
	- ==let과 const로 선언된 변수==가 해당한다. (단, var로 선언된 변수는 전역 스코프이다.)

- **스코프 체인**(scope chain)
	- 중첩 함수의 경우 내부 함수는 외부 함수의 변수에 접근할 수 있다.

### 10.2. 호이스팅(Hoisting)
- 변수 및 함수 선언이 해당 스코프의 최상단으로 끌어올려진 것처럼 동작하는 현상
	- 호출 코드가 선언보다 위에 있어도 선언이 위에 있는 것처럼 동작한다.
	- JavaScript는 코드 실행 전 준비 단계에서 함수를 미리 찾아 생성해 둔다.

- **함수 호이스팅**: 함수 선언식은 선언 전에 호출해도 정상 실행
```js
greeting("홍길동"); // 정상 실행
function greeting(name) {
	console.log("안녕하세요 " + name);
}
```

- **변수 호이스팅**: 변수는 **선언 → 초기화 → 할당**의 과정을 거쳐서 생성된다.
	- `var`: 호이스팅되지만 할당은 안 됨 - 선언과 초기화만 진행 → `undefined`
	- `let`, `const`: 호이스팅되지만 접근 불가 - 선언만 진행 → `ReferenceError` (TDZ)
```js
console.log(myVar); // undefined
console.log(myLet); // ReferenceError: Cannot access 'myLet' before initialization

var myVar = "var 할당";
let myLet = "let 할당";
```

### 10.3. var·let·const 요약
|    선언     | 재선언 | 재할당 | 선언 호이스팅 | 자동 초기화 | Scope |
| :-------: | :-: | :-: | :-----: | :----: | :---: |
|  **var**  |  ⭕  |  ⭕  |    ⭕    |   ⭕    |  함수   |
|  **let**  |  ❌  |  ⭕  |    ⭕    |   ❌    |  블록   |
| **const** |  ❌  |  ❌  |    ⭕    |   ❌    |  블록   |
