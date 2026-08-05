---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-11-05
---
## 1. jQuery 소개
- **jQuery**
	- 자바스크립트 코드를 더 적은 양으로 작성할 수 있게 해주는 라이브러리
	- 2006년 존 레식(John Resig)이 발표
	- MIT 라이선스 하에 배포되는 오픈 소스 소프트웨어

- **사용법**
	- `jquery.com`에서 직접 파일을 다운로드하여 사용
```html
<head>
	<script src="http://code.jquery.com/jquery-3.7.1.min.js"></script>
</head>
```

- **파일 버전**
	- **Uncompressed**(`.js`): 압축되지 않은 원본 파일로, 학습 및 개발용으로 적합
	- **Minified**(`.min.js`): 파일 용량을 최소화하기 위해 공백, 주석 등을 제거한 압축 파일로, 실제 서비스 환경에서 사용

## 2. jQuery 문장 구조
```js
$(selector).action();
```
- **기본 구조**: `$(선택자).동작함수();` 형태로 구성
	- `$()`: 원하는 HTML 요소를 선택
	- `.동작함수()`: 선택된 요소에 대해 수행할 작업

- **주요 선택자**
	- **타입 선택자**: `$("p")` (모든 `<p>` 요소 선택)
	- **.class 선택자**: `$(".menu")` (클래스가 "menu"인 모든 요소 선택)
	- **`#id` 선택자**: `$("#check")` (id가 "check"인 요소 선택)
	- `$("*")`(모든 요소), `$(this)`(현재 요소), `$("p:first")`(첫 번째 p 요소) 등 다양한 선택자

- **DOM 로딩 완료 후 실행**: `$(document).ready(function() {});`

## 3. jQuery 이벤트 처리
- **jQuery 이벤트 처리**
	- 사용자의 행동(마우스 클릭, 키보드 입력 등)에 반응하여 특정 코드를 실행하는 기능

- **주요 이벤트**
	- **마우스 이벤트**: `click`, `mouseover`, `mouseenter`, `mouseleave` 등
	- **키보드 이벤트**: `keypress`
	- **입력 필드 이벤트**: `change`, `focus`(포커스를 얻을 때), `blur`(포커스를 잃을 때)

- **이벤트 연결**
	- **단축 메서드**: `$("p").click(function() {});` 처럼 이벤트 이름으로 된 메서드를 직접 사용
	- **bind 메서드**: `$("p").bind("click", function() {});` 와 같이 사용하여 이벤트를 연결

### 3.1. 실습
```html
<!DOCTYPE html>
<html>
<head>
	<script src="http://code.jquery.com/jquery-3.7.1.js"></script>
</head>
<body>
	<p id=name>안녕하세요</p>
	<script>
		// JavaScript -> JQuery 변환
		/*
		function update() {
			let v = document.getElementById("name");
			v.innerHTML = "<em>변경합니다.</em>";
			alert( v.innerHTML);
		}
		*/
		$("#name").click(function() {
			this.innerHTML = "<em>변경합니다. </em>";
			alert(this.innerHTML);
		});
	</script>
</body>
</html>
```

## 4. jQuery를 이용한 애니메이션
- **jQuery 애니메이션**
	- 웹 페이지의 요소를 동적으로 움직이게 하거나 시각적 효과를 주는 기능

- **주요 애니메이션 메서드**
	- `show()`/`hide()`: 요소를 나타나게 하거나 사라지게 한다.
	- `toggle()`: `show()`와 `hide()`를 번갈아 실행한다.
	- `fadeIn()`/`fadeOut()`: 요소를 서서히 나타나게 하거나 사라지게 한다.
	- `slideDown()`/`slideUp()`: 요소를 슬라이드 방식으로 나타나게 하거나 사라지게 한다.
	- `animate()`: CSS 속성을 변경하여 사용자 정의 애니메이션 효과를 만든다.

- **메서드 체이닝**(Method Chaining)
	- 하나의 jQuery 객체에 여러 메서드를 연속적으로 연결하여 실행
	- 예: `$("#cat").show().fadeOut("slow").slideDown("slow");`

### 4.1. 실습
```html
<!DOCTYPE html>
<html>
<head>
	<script src="http://code.jquery.com/jquery-3.7.1.js"></script>
	<script>
		$(document).ready(function () {
			$("button").click(function () {
				$("#cat").show().fadeOut("slow").slideDown("slow");
			});
		});
	</script>
</head>
<body>
	<button>메서드 체이닝</button>
	<img id="cat" src="https://i.pinimg.com/736x/6f/7f/0b/6f7f0bbccc81c01b7f242b877c6fda6a.jpg" alt="" width="120" height="100" style="display: none"/>
</body>
</html>
```

## 5. jQuery를 이용한 DOM 변경
- **jQuery DOM 변경**
	- DOM(Document Object Model) 트리의 요소에 접근하여 내용을 변경하거나, 요소를 추가 및 삭제하는 기능

- **DOM 관련 메서드**
	- `text()`: 요소의 텍스트 콘텐츠를 가져오거나 설정
	- `html()`: 요소의 HTML 콘텐츠를 가져오거나 설정
	- `val()`: 입력 필드의 값을 가져오거나 설정
	- `append()`/`prepend()`: 선택된 요소의 내부에 자식 요소를 추가
	- `remove()`: 선택된 요소와 그 자식 요소를 모두 삭제
	- `empty()`: 선택된 요소의 자식 요소만 삭제

### 5.1. 실습
```html
<!DOCTYPE html>
<html>
<head>
	<script src="http://code.jquery.com/jquery-3.7.1.js"></script>
	<script>
		$(document).ready(function () {
			$("#button1").click(function () {
				$("p").append("<b style='color:red'>Hello! </b>.");
			});
			$("#button2").click(function () {
				$("p").prepend("<b style='color:red'>Hello! </b>.");
			});
		});
	</script>
</head>
<body>
	<p>I would like to say: </p>
	<button id="button1">append()</button>
	<button id="button2">prepend()</button>
</body>
</html>
```

## 6. jQuery를 이용한 CSS 조작
- **jQuery CSS 조작**
	- 선택된 요소의 스타일 속성을 변경하거나 클래스를 추가/삭제하는 기능

- **CSS 조작 메서드**
	- `css()`: 특정 스타일 속성 값을 가져오거나 설정
	- `addClass()`: 요소에 하나 이상의 클래스를 추가
	- `removeClass()`: 요소에서 하나 이상의 클래스를 삭제

### 6.1. 실습
```html
<!DOCTYPE html>
<html>
<head>
	<script src="http://code.jquery.com/jquery-3.7.1.min.js"></script>
	<script>
		$(document).ready(function () {
			$("button").click(function () {
				$("#div1").addClass("warning");
			});
		});
	</script>
	<style>
		.warning {
			border: 1px solid black;
			background-color: yellow;
		}
	</style>
</head>
<body>
	<div id="div1"><p>예제 단락입니다.</p></div><br>
	<button>addClass()</button>
</body>
```

## 7. AJAX 개요
- **AJAX(Asynchronous JavaScript and XML)**
	- JavaScript와 XML, JSON을 이용한 비동기적 정보 교환 기법
	- 브라우저의 XMLHttpRequest 객체 또는 Fetch 를 이용하여 비동기적으로 업데이트
	- 웹페이지를 리로드(reload) 하지 않고 일부 또는 전체 데이터를 갱신하는 방식
	- 빠르고 동적인 대화형 웹 페이지를 만드는 데 유용

- **동작 원리**
	- XMLHttpRequest 객체 생성
	- XML 메시지 구성, 웹 서버 요청
	- XML 메시지 수신
	- 메시지 파싱 후 DOM 객체 업데이트

- **jQuery AJAX 문법**: `$.ajax()` 메서드를 사용하여 서버와 통신
	- **jQuery 3.0 이전**: `success`, `error`, `complete` 콜백 함수를 사용
	- **jQuery 3.0 이상**: `$.ajax()`가 반환하는 Promise 객체의 `.done()`, `.fail()`, `.always()` 메서드 사용을 권장

- **주요 설정**: `url`(요청 주소), `type`(전송 방식: GET, POST 등), `dataType`(응답 데이터 타입), `data`(전송할 데이터) 등

### 7.1. 실습
```html
<!DOCTYPE html>
<html>
<head>
	<script src="http://code.jquery.com/jquery-3.7.1.js"></script>
</head>
<body>
	<button id="request">request-Ajax-jQuery</button>
	<div id="target"></div>
	<script>
		$("#request").click(function(){
			$.ajax({
				url: "http://0.0.0.0:8080/frontend/RestController?cmd=list",
				type: "GET",
				dataType: "json",
				success: function(data) {
					alert("success-성공");
					console.log(data);
				},
				complete: function() {
					alert("complete");
					console.log("complete");
				},
				error: function(request, status, error) {
					alert("error-실패");
					console.log(error);
				}
			});
		});
	</script>
</body>
</html>
```

## 8. AJAX 구현
### 8.1. CORS
- **CORS**(Cross-Origin Resource Sharing)
	- 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제

- **SOP**(Same-Origin Policy)
	- 보안을 위해 브라우저는 기본적으로 동일한 출처의 리소스만 접근을 허용

- **해결 방법**
	- 서버 측에서 응답 헤더에 `Access-Control-Allow-Origin`을 설정하여 특정 출처의 접근을 허용하거나, 프록시 서버를 사용하여 해결

## 9. JavaScript to jQuery
- JavaScript로 작성된 코드에 jQuery를 사용하면 훨씬 간결하게 변환할 수 있다.

### 9.1. 선택자
| 구분               | Vanilla JavaScript                           | jQuery                   |
| :--------------- | :------------------------------------------- | :----------------------- |
| **ID로 선택**       | `document.getElementById("target")`          | `$("#target")`           |
| **태그 이름으로 선택**   | `document.getElementsByTagName("p")`         | `$("p")`                 |
| **특정 태그 선택**     | `document.getElementsByTagName("p")[0]`      | `$("p").eq(0)`           |
| **CSS 선택자로 선택**  | `document.querySelector("p")`                | `$("p")`                 |
| **클래스 이름으로 선택**  | `document.getElementsByClassName("myClass")` | `$(".myClass")`          |
| **Name 속성으로 선택** | `document.getElementsByName("firstForm")`    | `($("[name=firstForm]")` |

### 9.2. DOM 조작
| 구분                   | Vanilla JavaScript                                               | jQuery                                  |
| :------------------- | :--------------------------------------------------------------- | :-------------------------------------- |
| **HTML 콘텐츠 가져오기/설정** | `document.getElementById("target").innerHTML`                    | `$("#target").html()`                   |
| **텍스트 콘텐츠 가져오기/설정**  | `document.getElementsByClassName("myClass")[0].innerText`        | `$(".myClass").text()`                  |
| **CSS 스타일 변경**       | `document.getElementById("target").style.color = "red";`         | `$("#target").css("color", "red");`     |
| **속성 변경**            | `document.getElementsByClassName("myClass")[0].width = "300px";` | `$(".myClass").attr("width", "300px");` |

### 9.3. 이벤트
- **문서 로드 시 실행**
	- Vanilla JavaScript: `window.onload = function(){ }`
	- jQuery: `$(document).ready(function(){ });` 또는 `$(function() { });`

- **이벤트 리스너 추가 (클릭 이벤트)**
	- Vanilla JavaScript: `target.addEventListner(“click”, function(){ });`
	- jQuery: `$(“#target”).click(function(){ });`
