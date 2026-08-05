---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-09-11
---
## 1. HTML5 개요
### 1.1. 웹 기술의 3요소
- **HTML5**: 웹 페이지의 **내용**(Content)과 구조 작성
- **CSS3**: 웹 페이지의 **스타일**(Style) 지정
- **JavaScript**: 웹 페이지의 **상호작용**(Interaction) 담당

### 1.2. HTML5의 특징
- *HTML5*
	- 웹 페이지를 기술하기 위한 마크업(markup) 언어
	- 파일 확장자는 `*.htm` 혹은 `*.html`
	- 태그를 이용해서 문서의 구조를 표현한다. (예: `<body>`, `<head>`, `<title>`, `<table>`, `<img>`)
	- HTML은 태그로 이루어진 요소(element)로 구성

- **플랫폼 의존성 제거**: ActiveX나 Flash 같은 별도 플러그인 없이 작동
- **표준화된 JavaScript API 제공**
	- 비디오/오디오 재생 태그
	- 2D/3D 그래픽(Canvas API), 2D 그래픽(SVG API)
	- 웹 스토리지, 웹 SQL 로컬 데이터베이스
	- 파일 입출력, 위치 정보, 웹 워커, 웹 소켓
	- 오프라인 웹 애플리케이션, 드래그 앤 드롭

### 1.3. 문서 기본 구조
- `<!DOCTYPE html>` 선언으로 브라우저가 HTML5 문서임을 인식
	- `<!DOCTYPE>`: 웹 페이지에 사용된 HTML의 종류와 버전을 지정
	- HTML 4.01, XHTML 1.0은 DTD 경로까지 길게 명시해야 했음
- `<html>` 태그로 시작하고 끝나며, `<head>`(설정, 제목)와 `<body>`(실제 내용)로 구성
```html
<!DOCTYPE html>
<html>
	<head>
		<title>나의 홈페이지</title>
	</head>
	<body>
		<p>Hi, My Homepage!</p>
	</body>
</html>
```

### 1.4. 요소·태그·속성
- **요소(Element)**
	- 시작 태그 + 콘텐츠 + 종료 태그
		- (예: `<title>제목</title>`)
	- 단독 태그: `<태그이름 />`

- **태그 이름**
	- 공백 없는 문자열, 대소문자 구분 없음

- **속성(Attribute)**
	- 요소에 대한 추가적인 정보 제공
	- 항상 **시작 태그** 내에 `이름="값"` 형태로 기술
		- (예: `<h1 title="툴팁">`)

- *태그 종류 한눈에 보기*
	- **글자 태그**: h, p, br, pre, 글자 표현 태그 → [[#2. 글자 태그]]
	- **리스트·테이블 태그**: ol, ul, li / table, tr, td, th → [[#3. 리스트와 테이블]]
	- **이미지·링크 태그**: img, a → [[#4. 이미지와 링크]]
	- **멀티미디어 태그**: audio, video, iframe, embed, object → [[#5. 멀티미디어와 외부 콘텐츠]]
		- **iframe(인라인 프레임) 태그**: ==현재 html 페이지 내 영역에 다른 html 페이지 프레임을 삽입하여 출력한다.==
		- **object 태그, embed 태그**: iframe 태그를 대체하여 HTML 파일이 아닌 비디오, 오디오 등 외부의 애플리케이션 파일을 포함하는 데 주로 사용한다.
	- **시맨틱(semantic) 태그**: 브라우저에게 요소의 의미나 목적을 명확하게 알려주는 요소 → [[#7. 시맨틱 태그]]
	- **폼 태그**: form, input, select 등 → [[#8. 입력 양식 (Form)]]

## 2. 글자 태그
### 2.1. 구조 태그
- `<h1~6>`: 제목(Heading), 숫자가 작을수록 크고 굵음
- `<p>`: 문단(Paragraph), 앞뒤로 줄바꿈 자동 발생
- `<br>`: 물리적인 강제 줄바꿈 (종료 태그 없음)
- `<pre>`: (previously formatted text) 입력한 공백, 탭, 줄바꿈을 그대로 표시
- `<!-- -->`: 주석

### 2.2. 글자 표현 태그
- `<i>`: 기울임꼴 (다른 글자와 구별되는 italic, 단순 시각적)
- `<b>`: 굵게 (중요도와 관련 없이 bold)
- `<em>`: 강조 (기울임꼴, 의미적 강조)
- `<strong>`: 강한 강조 (굵게, `<em>`보다 더 강조)
- `<sub>`, `<sup>`: 아래 첨자, 위 첨자
- `<ins>`, `<del>`: 밑줄(추가됨), 취소선(삭제됨)
- `<hr>`: 수평 가로줄 (주제 변경처럼 앞·뒤 내용을 의미적으로 분리)

### 2.3. 특수 문자
- 특수 기호(Entity)로 입력
	- `&nbsp;`: 공백 문자 한 개 (non-breaking space)
	- `&lt;`: `<`
	- `&gt;`: `>`
	- `&quot;`: `"`
	- `&amp;`: `&`

## 3. 리스트와 테이블
### 3.1. 리스트
- `<ol>`: 순서가 있는 목록 (1, 2, 3...)
- `<ul>`: 순서가 없는 목록 (글머리 기호)
- `<li>`: 목록의 각 항목
- `<dl>`, `<dt>`, `<dd>`: 정의 리스트 (항목과 그 설명)
```html
<dl>
	<dt>에스프레소</dt>
	<dd>- 커피의 기본, 커피의 원액이다.</dd>
	<dt>아메리카노</dt>
	<dd>- 에스프레소에 물을 넣은 것</dd>
</dl>
```

### 3.2. 테이블
- `<table>`: 표 전체 컨테이너
	- `width`: 표의 전체 너비 (기본값은 셀 내용에 따라 달라짐)
	- `border`: 표의 테두리 (0이면 테두리 없음)
- `<tr>`: 행(Row)
- `<th>`: 제목 셀(가운데 정렬, 굵게)
- `<td>`: 데이터 셀(Cell)
- 속성: `rowspan`(==행 병합==), `colspan`(==열 병합==)
```html
<table border="1">
	<tr> <th>1열</th> <th>2열</th> <th>3열</th> </tr>
	<tr> <td rowspan="2">1행 1열</td> <td>1행 2열</td> <td>1행 3열</td> </tr>
	<tr> <td>2행 2열</td> <td>2행 3열</td> </tr>
	<tr> <td colspan="3">3행 1열</td> </tr>
</table>
```

## 4. 이미지와 링크
### 4.1. 이미지
- **`<img>`**
	- 속성: `src`(파일 경로), `width`/`height`(크기), `alt`(이미지를 표시하지 못할 때의 대체 텍스트)
	- 경로 규칙 (웹 문서가 있는 폴더 기준)
		- 같은 폴더: `파일명`
		- 상위 폴더: `../파일명`
		- 하위 폴더: `폴더명/파일명`
		- 이미지는 별도 디렉터리에 저장하면 통합 관리가 용이

- **`<figure>`, `<figcaption>`**: 이미지와 캡션을 그룹화하는 시맨틱 태그
	- `<figure>`: *독립적인 콘텐츠를 표현*
	- `<figcaption>`: *콘텐츠 추가 설명*

### 4.2. 하이퍼링크
- **`<a>`** ("anchor"의 약자)
	- 링크: 모든 형식의 자료를 연결하여 내비게이션이 가능하도록 하는 구성요소
	- 앵커: HTML 문서 내에서 링크의 출발점이나 도착점
	- 속성: `href`(이동할 주소), `target`(새 페이지가 열릴 위치)

| target    | 설명                    |
| --------- | --------------------- |
| `_blank`  | 새로운 윈도우(탭)에서 연다       |
| `_self`   | 현재 윈도우(탭)에서 연다 (기본값)  |
| `_parent` | 부모 프레임에 연다            |
| `_top`    | 모든 프레임을 취소하고 전체 창에 연다 |

### 4.3. 책갈피 링크
- 문서 내 특정 지점으로 이동
	- 시작점 앵커의 설정
		- `<a href="#고유아이디">링크 설정된 '고유아이디' 위치로 이동</a>`
	- 목적지 앵커의 설정
		- `<a id="고유아이디">문서 내 이동할 목적지</a>`

## 5. 멀티미디어와 외부 콘텐츠
### 5.1. 오디오와 비디오
- **`<audio>` & `<video>`**
	- HTML5에서 플러그인 없이 재생 가능
	- 주요 속성
		- `src`(경로), `controls`(제어바 표시), `autoplay`(자동 재생), `loop`(반복)
		- `width`/`height`(비디오 표시 영역), `poster`(비디오 로딩 중 보여줄 이미지)
		- `preload`: 미리 로딩할지 여부
			- `auto`(기본): 페이지 로드 후 바로 다운로드
			- `metadata`: 재생 전까지 메타데이터만 다운로드
			- `none`: 재생 시작 전까지 다운로드 안 함
	- `<source>` 태그로 여러 포맷을 나열해 호환성 확보, 미지원 시 대체 메시지 출력
```html
<video width="640" height="480" controls autoplay>
	<source src="media/trailer.mp4" type="video/mp4">
	<source src="media/trailer.ogv" type="video/ogg">
	<p>Your user agent does not support HTML5.</p>
</video>
```

### 5.2. 인라인 프레임 (iframe)
- 현재 페이지 영역 안에 다른 웹 페이지(또는 유튜브 영상)를 삽입
- 속성: `src`(출력할 파일 URL), `width`/`height`(프레임 크기), `name`(프레임 이름)
```html
<iframe src="image_table.html" width="500" height="250"></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/9bZkp7q19f0" allowfullscreen></iframe>
```

### 5.3. embed와 object
- *iframe의 단점*
	- 외부 디자인을 포함하기 때문에 내부 디자인에 영향
	- 웹 보안 정책으로 일부 사이트는 iframe 콘텐츠를 차단
- iframe을 대체하여 비디오, 오디오 등 외부 애플리케이션 파일을 포함할 때 주로 사용
```html
<embed src="media/trailer.mp4" width="450" height="300"></embed>
<object data="image_table.html" width="200" height="150">
	<!-- 브라우저가 지원하지 않을 경우 대체 콘텐츠 -->
</object>
```

## 6. 블록 요소 vs 인라인 요소
- **블록 요소 (Block)**: ==한 줄을 모두 차지함 - 줄바꿈 일어남==
	- 예: `<h1>`, `<p>`, `<ul>`, `<ol>`, `<li>`, `<table>`, `<div>` 등
	- `<div>`: 구역을 나누거나 태그들을 묶어 그룹화하는 블록 컨테이너

- **인라인 요소 (Inline)**: ==콘텐츠 크기만큼만 차지함 - 줄바꿈 없음==
	- 예: `<a>`, `<img>`, `<strong>`, `<em>`, `<input>`, `<i>`, `<b>`, `<sub>`, `<sup>`, `<ins>`, `<del>`, `<span>` 등
	- `<span>`: 문장 내 특정 부분을 묶어 스타일을 줄 때 사용하는 인라인 컨테이너

## 7. 시맨틱 태그
### 7.1. 시맨틱 웹
- **시맨틱 웹(Semantic Web)**
	- W3C가 설정한 표준을 통해 웹을 확장한 것 (Web 3.0)
	- 목표: 인터넷 상의 데이터를 ==기계가 읽고 이해==할 수 있도록 만드는 것
	- *기존 웹은 문서 단위로 정보를 주고받지만, 시맨틱 웹은 문서 구조를 알고 있다.*

- **시맨틱 태그**
	- 브라우저에게 요소의 의미나 목적을 명확하게 알려주는 요소
	- W3C가 많은 웹 페이지를 분석(id, class)하여 자주 쓰이는 구조를 표준 태그로 정의

### 7.2. 구조 태그
- `<header>`: 페이지 제목, 페이지를 소개하는 간단한 설명
- `<nav>`: 페이지 내 목차(메뉴)
- `<section>`: 문서 본문의 절(구역)
- `<article>`: 독립적인 콘텐츠 영역
- `<aside>`: 본문과 관련된 기사(사이드바)
- `<footer>`: 주로 저자 또는 저작권 정보

```html
<header><h1>HTML, CSS, JAVASCRIPT 소개</h1></header>
<nav>
	<ul>
		<li><a href="#html">HTML5</a></li>
		<li><a href="#css">CSS3</a></li>
	</ul>
</nav>
<section>
	<article id="html"><h2>HTML5</h2><p>문서의 구조와 내용을 기술합니다.</p></article>
	<article id="css"><h2>CSS3</h2><p>문서의 스타일을 기술합니다.</p></article>
</section>
<footer><p>저자: 홍길동</p></footer>
```

### 7.3. 내용 태그
- **블록**
	- `<figure>` : 본문에 삽입하는 사진, 차트, 삽화, 코드 등을 그림으로 표현
	- `<details>`: 상세 정보를 담는 시맨틱 블록 태그
	- `<summary>` : `<details>`로 구성되는 블록의 제목 표현

- **인라인**
	- `<mark>`: 중요한 텍스트임을 표시
	- `<time>` : 텍스트의 내용이 시간임을 표시
	- `<meter>`: 주어진 범위나 %의 데이터 량 표시
	- `<progress>`: 작업의 진행 정도 표시

### 7.4. HTML5에서 제거된 태그
- `<big>`, `<center>`, `<dir>`, `<font>`, `<tt>`, `<u>`, `<xmp>`, `<acronym>`, `<applet>`, `<basefont>`, `<frame>`, `<frameset>`, `<noframes>`, `<strike>`

## 8. 입력 양식 (Form)
### 8.1. form 태그와 전송 방식
- **`<form>` 태그**: ==사용자가 입력한 데이터를 서버로 전송==
	- *주요 속성*
		- `name`: 폼의 이름 (스크립트 식별용)
		- `action`: 폼 데이터를 처리할 서버 스크립트 URL
		- `method`: 데이터 전송 방식(get/post)

- **전송 방식**
	- **GET**: URL 뒤에 `?` 파라미터를 붙여 전송 (최대 2048자 제한, 보안 취약)
		- 예: `https://search.naver.com/search.naver?query=설악산`
	- **POST**: HTTP Request 헤더에 담아 전송 (길이 제한 없음, 보안 유지)
```html
<form name="input" action="http://www.tukorea.ac.kr/getid.jsp" method="get">
	사용자 아이디: <input type="text" name="userid"><br>
	<input type="submit" value="제출">  <!-- 폼 데이터를 서버로 전송 -->
	<input type="reset" value="초기화"> <!-- 입력 값 모두 초기화 -->
</form>
```

### 8.2. input 태그
- **`<input>` 태그**: 다양한 입력 필드 생성
	- 기본 타입: `text`, `password`, `submit`, `reset`, `button`, `radio`, `checkbox`
		- `radio`: 같은 `name` 그룹 내에서 **하나만** 선택
		- `checkbox`: 여러 항목 **동시 선택** 가능, `checked`로 초기 선택
	- **HTML5 신규 타입**: `email`, `url`, `tel`, `color`, `month`, `date`, `week`, `time`, `datetime-local`, `number`, `range` 등 (모바일 키패드 자동 변경 및 유효성 검사 지원)
		- `number`/`range`는 `min`, `max`, `step` 속성 사용
	- 주요 속성: `name`(서버 식별자), `value`(값)
	- **HTML5 신규 속성**
		- `autocomplete`(자동 완성), `autofocus`(로드 시 자동 포커스)
		- `placeholder`(흐린 입력 힌트), `readonly`(읽기 전용)
		- `required`(제출 전 필수 입력), `pattern`(허용 입력 형태를 정규식으로 지정)
```html
<input type="radio" name="gender" value="male">남성
<input type="radio" name="gender" value="female">여성

<input type="checkbox" name="fruits" value="apple" checked>Apple
<input type="checkbox" name="fruits" value="grape">Grape
```

### 8.3. 기타 폼 태그
- `<textarea>`: 여러 줄 텍스트 입력
- `<select>`, `<option>`: 드롭다운 메뉴 (`selected`로 초기 선택), `<optgroup>`으로 목록 그룹화
- `<label>`: 폼 요소의 텍스트 라벨 (클릭 시 연결된 input 활성화)
- `<fieldset>`, `<legend>`: 폼 요소 그룹화 및 그룹 제목
- `<button>`: 버튼 생성
```html
<fieldset>
	<legend>인적사항입력</legend>
	<select name="cars">
		<option value="bmw">BMW</option>
		<option value="hyundai" selected>현대자동차</option>
	</select>
</fieldset>
```
