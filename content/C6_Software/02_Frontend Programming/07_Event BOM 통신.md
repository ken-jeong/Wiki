---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-10-29
---
## 1. 이벤트
- **이벤트**(Event)
	- 웹 페이지에서 사용자의 행동(마우스 클릭, 키보드 입력 등)이나 브라우저의 상태 변화에 따라 발생하는 사건

- **이벤트 처리 절차**
	1.  **이벤트 선정**: 반응할 이벤트를 결정한다. (예: `click`, `mouseover`)
	2.  **이벤트 핸들러 작성**: 이벤트 발생 시 실행될 코드를 함수(이벤트 리스너)로 작성한다.
	3.  **이벤트 핸들러 등록**: 특정 HTML 요소에 작성된 이벤트 핸들러를 연결한다.

### 1.1. 이벤트 리스너 작성 방법
1. **인라인 방식**: HTML 태그 내에 `onmouseover`와 같은 속성으로 직접 코드를 작성한다.
2. **속성 대입**: 자바스크립트에서 DOM 객체의 이벤트 속성에 함수를 직접 할당한다.
3. `addEventListener("event", listener)`
	- 표준 방식으로, 하나의 요소에 여러 이벤트 리스너를 등록할 수 있다.
	- `removeEventListener()`로 제거도 가능하다.

```html
<!-- HTML 요소 인라인 -->
<div onmouseover="this.style.backgroundColor='blue'">Hello World!</div>

<!-- 객체의 이벤트 리스너 속성에 함수를 대입 -->
<div id="target">Hello World!</div>
<script>
	function changeColor() {
		document.getElementById("target").style.backgroundColor="blue";
	}
	document.getElementById("special").onmouseover = changeColor;
</script>

<!-- 객체의 addEventListener() 함수를 사용 -->
<script>
	let obj = document.getElementById("target");
	obj.addEventListener("mouseover", changeColor);
	
	// 또는 익명 함수로 이벤트 리스너 작성
	obj.addEventListener("mouseover", function() {
		document.getElementById("target").style.backgroundColor="blue";
	});
</script>
```

### 1.2. 이벤트 전파 방식
- **이벤트 전파 방식**
	- **버블링**(bubbling): 안쪽 요소의 이벤트 처리 → 바깥쪽 요소의 이벤트 처리 (기본값)
	- **캡처링**(capturing): 바깥쪽 요소의 이벤트 처리 → 안쪽 요소의 이벤트 처리

## 2. 마우스 관련 이벤트
- 사용자의 마우스 조작과 관련된 다양한 이벤트
	- `click`: 마우스 버튼을 클릭했을 때 발생
	- `mouseover`: 마우스 커서가 요소 위로 올라왔을 때 발생
	- `mouseout`: 마우스 커서가 요소를 벗어났을 때 발생
	- `mousedown`: 마우스 버튼을 누르는 순간 발생
	- `mouseup`: 눌렀던 마우스 버튼을 떼는 순간 발생

```html
<h1 onclick=“change()”> 이것은 클릭 가능한 헤딩입니다. </h1>
```

## 3. 문서 로딩 관련 이벤트
- **웹 페이지의 로딩 상태와 관련된 이벤트**
	- `"load"` 이벤트: HTML 문서가 완전히 로드되었을 때 발생하는 이벤트
	- `"unload"` 이벤트: 사용자가 웹 페이지를 떠날 때 발생하는 이벤트

## 4. 입력 값의 유효성 검증
- 사용자가 입력한 데이터가 유효한지 서버로 전송하기 전에 클라이언트 측에서 미리 검사하는 과정
- 불필요한 서버 요청을 줄이고 사용자 경험을 향상
- 공백, 데이터 길이, 형식 등을 검증

## 5. 정규식
- **정규식**(Regular Expression)
	- 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용되는 형식 언어
	- 입력값의 유효성 검증, 특히 복잡한 패턴(이메일, 전화번호 등)을 검사할 때 유용

- **주요 메타문자**
	- `^`: 문자열 시작
	- `$`: 문자열 끝
	- `[0-9]`: 0부터 9까지의 숫자 중 하나
	- `+`: 1회 이상 반복

## 6. 브라우저 객체 모델
- **브라우저 객체 모델**(BOM; Browser Object Model)
	- 웹 브라우저의 기능을 객체처럼 다루는 모델

### 6.1. 주요 객체
- `window` 객체
	- BOM의 최상위 객체로, 브라우저 창 자체를 나타낸다.
	- `open()`, `setTimeout()`, `setInterval()` 등의 메서드를 제공한다.

- `navigator` 객체: 브라우저의 정보를 제공한다. (종류, 버전 등)
- `screen` 객체: 사용자 화면의 정보를 제공한다. (해상도 등)

- `location` 객체: 브라우저의 URL과 관련된 객체이다.
- `history` 객체: 브라우저의 방문 기록을 제어한다. (`back()`, `forward()`, `go()`)

## 7. 비동기 처리
- **비동기 처리**
	- 서버와 데이터를 교환할 때 페이지 전체를 새로고침하지 않고 일부 데이터만 업데이트하는 방식
	- 더 빠르고 부드러운 사용자 경험을 제공

- **비동기 통신 방식**
	- [[07_Event BOM 통신#7.1. XMLHttpRequest 객체|XMLHttpRequest 객체]]: XML 데이터를 서버로부터 전송받아 처리하는 객체
	- [[07_Event BOM 통신#7.2. Promise 객체|Promise 객체]]: 비동기 처리 상태(대기/이행/실패)를 관리하는 객체
	- [[07_Event BOM 통신#7.3. fetch() 메서드|fetch() 메서드]]: Promise 객체를 반환하는 메서드
	- [[07_Event BOM 통신#7.4. async await 문법|async await 문법]]: Promise를 동기 코드처럼 작성하는 문법

### 7.1. XMLHttpRequest 객체
- **XMLHttpRequest 객체**
	- XML 데이터를 서버로부터 전송받아 처리하는 객체
	- *XML 데이터를 주로 사용했지만 현재는 JSON도 널리 사용*

- **JSON**(JavaScript Object Notation)
	- 서버와 클라이언트 간 데이터 교환에 널리 사용되는 경량의 텍스트 기반 데이터 형식
	- 키-값 쌍으로 구성되어 가독성이 높고 자바스크립트에서 다루기 쉽다.

### 7.2. Promise 객체
- **Promise 객체**
	- 비동기 처리 상태(대기/이행/실패)를 관리하는 객체
	- 콜백 함수의 문제점인 콜백 헬(가독성, 제어 처리 시점, 에러 처리 한계)을 해결
	- new Promise(), resolve(), then() 구조

- **Promise의 상태**(states)
	- **대기**(pending): 비동기 처리 로직이 아직 완료되지 않은 상태
	- **이행**(fulfilled): 비동기 처리가 완료되어 Promise가 결과 값을 반환해준 상태
	- **실패**(rejected): 비동기 처리가 실패하거나 오류가 발생한 상태

### 7.3. fetch() 메서드
- **fetch() 메서드**
	- Promise 객체를 반환하는 메서드
	- `fetch(url).then(response => {}).then(data => {});`
	- 브라우저 전용 API로 개발된 웹 표준
	- XMLHttpRequest 객체의 이벤트 기반 콜백 함수보다 적용하기 쉽다.

```js
fetch(url)
  .then(response => {
    return response.json();
  })
  .then(data => {
    console.log(data);
  });
```

### 7.4. async await 문법
- **async await 문법**
	- Promise를 동기 코드처럼 작성하는 문법
	- function 키워드 앞에 async 키워드를, 비동기로 처리되는 부분 앞에 await 키워드를 선언한다.
	- async 함수 내에서 await 키워드를 사용하여 비동기 작업이 완료될 때까지 기다린다.

```js
async function fetchData(url) {
  try {
    const response = await fetch(url);
    const data = await response.json(); 
    return data;
  } catch (error) {
    console.error(error);
  }
}
```
