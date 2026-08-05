---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-09-17
---
## 1. CSS 개요
### 1.1. 정의와 목적
- **정의**
	- CSS(Cascading Style Sheets)는 HTML 문서의 스타일(디자인, 레이아웃 등)을 지정하는 표준 언어이다.
	- *W3C 웹 컨소시엄 개발, XML 문서에도 적용 가능*

- **목적**
	- 문서의 **구조**(HTML)와 **표현**(CSS)을 분리하여 유지보수성과 효율성을 높인다.

- **CSS의 기능**
	- 선택자, 박스 모델, 배경 및 경계선, 텍스트 효과
	- 2D/3D 변환, 애니메이션, 다중 컬럼 레이아웃, 사용자 인터페이스

### 1.2. 문법
- `선택자 { 속성: 값; }` 형태 (예: `p { background-color: yellow; }`)
- 선택자(selector): 스타일을 변경하고 싶은 HTML 요소를 선택
- 반드시 끝에 세미콜론(`;`)을 사용한다.
- 주석: `/* ... */`

## 2. CSS 적용 방법
### 2.1. 선언 방식
1. **외부 스타일 시트 (External)**: `.css` 파일로 분리하여 `<link>` 태그로 연결 (권장, 여러 페이지에 동일 스타일 적용)
2. **내부 스타일 시트 (Internal)**: HTML `<head>` 내 `<style>` 태그에 작성
3. **인라인 스타일 (Inline)**: 태그 내 `style` 속성에 직접 작성 (선언이 2개 이상이면 `;`로 구분)
```html
<!-- 외부 -->
<link type="text/css" rel="stylesheet" href="mystyle.css">
<style> @import url(mystyle.css); </style>

<!-- 내부 -->
<style> h1 { color: red; } </style>

<!-- 인라인 -->
<h1 style="color: red">This is a headline.</h1>
```

### 2.2. 우선순위와 상속
- **우선순위**: ==인라인 > 내부 > 외부== 순서로 적용된다

- **상속**: 부모 요소의 스타일(예: 색상, 폰트)은 자식 요소에게 상속된다.
	- 예: `body { color: blue; }` → `<body>` 안의 `<p>`도 파란색

## 3. 선택자 (Selector)
> 스타일을 적용할 HTML 요소를 선택하는 방법이다.

### 3.1. 기본 선택자
- **타입 선택자**: 태그 이름 사용, 해당 요소 전체 선택 (예: `p`)
- **전체 선택자**: 페이지 안의 모든 요소 선택 (`*`)
- **아이디(ID) 선택자**: 특정 고유 요소 선택 (`#id`), 문서 내 유일해야 함
- **클래스(Class) 선택자**: 여러 요소를 하나의 클래스로 묶어 스타일 지정 (`.class`)
- **속성 선택자**(attribute selector), **의사 클래스**(pseudo-class)
```css
*		/* 전체 선택자 */
태그		/* 태그 선택자 (타입 선택자) */
.class	/* 클래스 선택자 */
#id		/* 아이디 선택자 */
```

```html
<style>
	#target { background-color: yellow; color: blue; }
	.root { background-color: yellow; color: blue; }
</style>
<p id="target">id가 target인 단락입니다.</p>
<p class="root">클래스가 root인 단락입니다.</p>
```

### 3.2. 선택자 그룹과 결합자
- **선택자 그룹**
	- 콤마(`,`)로 구분하여 여러 선택자에 동일 스타일 적용
```css
h1, h3, p { font-family: sans-serif; }	/* 그룹 선택자 */
```

- **결합자 (Combinators)**
	- **하위(자손) 선택자** `s1 s2`: 공백 사용, s1 내부의 모든 s2 (후손 관계)
	- **자식(직계) 선택자** `s1 > s2`: s1의 바로 아래 자식 s2만 선택 (자식 관계)
```css
body em { color: red; }		/* body 안의 em 후손 요소 */
body > h1 { color: blue; }	/* body의 직계 자식 h1 요소 */
```

### 3.3. 의사 클래스
- 클래스가 정의된 것처럼 간주, 콜론(`:`)으로 표기
- 요소의 특정 상태를 선택

| 의사 클래스     | 설명                |
| ---------- | ----------------- |
| `:link`    | 아직 방문하지 않은 링크     |
| `:visited` | 방문한 링크            |
| `:hover`   | 마우스가 링크 위에 있을 때   |
| `:active`  | 마우스로 링크를 클릭하고 있는 상태 |
| `:before`  | 요소의 시작 부분에 콘텐츠 추가 |

## 4. 주요 속성
- **자주 쓰는 속성**
	- `color`(텍스트 색상), `background-color`(배경색), `background-image`(배경 이미지)
	- `font-size`, `font-weight`(볼드), `font-style`(이탤릭)
	- `padding`, `border`, `text-align`, `list-style`

### 4.1. 색상
- 이름: `red`
- 16진수: `#FF0000`
- 10진수 RGB: `rgb(255, 0, 0)`
- 퍼센트 RGB: `rgb(100%, 0%, 0%)`

### 4.2. 폰트
- 속성: `font`(한 줄 설정), `font-family`(글꼴), `font-size`(크기), `font-style`(normal/italic/oblique), `font-weight`(normal/bold)
- 단위
	- `pt`: 포인트 (1/72 inch)
	- `px`: 픽셀
	- `%`: 부모 요소의 폰트 크기 기준
	- `em`: 부모 요소의 폰트 크기 기준 배수 (W3C 권장)
	- 키워드: `xx-small` ~ `medium` ~ `xx-large`

### 4.3. 텍스트
- `text-align`(수평 정렬), `text-decoration`(장식), `text-indent`(들여쓰기), `text-shadow`(그림자), `text-transform`(대소문자 변환)
- `line-height`(줄 높이), `letter-spacing`(글자 간격), `direction`(작성 방향)

### 4.4. 리스트와 테이블
- **리스트**
	- `list-style`(한 줄 설정), `list-style-type`(마커 타입), `list-style-image`(마커 이미지), `list-style-position`(마커 위치 안/밖)

- **테이블**
	- `border`, `border-collapse`(이웃 셀 테두리 합치기), `border-spacing`(셀 간 거리)
	- `width`, `height`, `empty-cells`(빈 셀 표시 여부)

## 5. 박스 모델
### 5.1. 구성 요소
- **박스 모델** (Box Model)
	- 웹 브라우저는 ==HTML 요소를 하나의 사각형 박스로 간주==하고 그린다.

- **구성 요소** (안쪽 → 바깥쪽)
	- **Content (내용물)**: 텍스트나 이미지가 들어가는 실질적 영역
	- **Padding (패딩)**: 내용물과 테두리 사이의 안쪽 여백 (배경색/배경 이미지가 보임)
	- **Border (테두리)**: 패딩과 내용물을 감싸는 경계선
	- **Margin (마진)**: 테두리 바깥의 외부 여백 (투명함)

### 5.2. 크기와 여백
- **크기 설정**
	- `width`, `height`로 설정

- **여백 설정**
	- 각 변: `margin-top`, `margin-right`, `margin-bottom`, `margin-left` (padding도 동일)
	- 축약형: 상 → 우 → 하 → 좌 (시계 방향)
		- `margin: 10px 20px 30px 40px;` (상 10, 우 20, 하 30, 좌 40)
		- `margin: 10px 20px;` (상하 10, 좌우 20)
		- `padding: 10px;` (네 방향 모두 10)

- **박스 크기 계산**
	- 전체 너비 = margin + border + padding + **width** + padding + border + margin
```css
#target {
	width: 200px;
	padding: 10px;
	border: 5px solid red;
	margin: 20px;
}
/* 전체 너비 = 20 + 5 + 10 + 200 + 10 + 5 + 20 = 270px */
```

### 5.3. 수평 정렬
> HTML5는 `align` 속성을 삭제했으므로 CSS로 정렬한다.

- 인라인 요소 중앙 정렬: 블록 컨테이너에 `text-align: center;`
- 블록 요소 중앙 정렬: `margin-left: auto; margin-right: auto;` (==width 지정 필요==)
- Flexbox 이용 중앙 정렬: 부모에 `display: flex; justify-content: center;`

## 6. 레이아웃 (Layout)
> 요소의 위치와 크기를 결정하여 배치하는 방식이다.

- **요소 배치 방식**
	- `display`: 블록/인라인 변경(block, inline, inline-block), 전체 레이아웃 변경(Flexbox, Grid → 반응형 디자인에 적합)
	- `float`: Flexbox와 Grid로 대체됨
	- `position`: 요소의 위치를 결정

### 6.1. display 속성
- `block`: 블록 요소처럼 배치 (줄 바꿈 일어남, 크기 조절 가능)
- `inline`: 인라인 요소처럼 배치 (줄 바꿈 없음, `width`, `height`, `margin-top/bottom` 변경 불가)
- `inline-block`: 줄 바꿈 없으나 `width`, `height`, `padding`, `margin` 변경 가능
- `none`: 없는 것으로 간주 (요소 제거, 공간도 차지하지 않음)
- cf. `visibility: hidden`: 화면에서 감춰지지만 요소(공간)는 존재
```css
/* 세로 목록(li)을 가로 메뉴바로 만들기 */
.menubar li {
	display: inline;
	background-color: yellow;
	border: 1px solid red;
	padding: .5em;
}
```

### 6.2. Flexbox (1차원 레이아웃)
- ==행(Row) 또는 열(Column) 한 방향으로 배치==
- 요소 간 공간 배분과 정렬로 효율적 배치, 소규모의 단순한 레이아웃에 적합
- `flex-direction`: 배치 방향 설정
	- `row`(기본값): 왼쪽 → 오른쪽
	- `column`: 위 → 아래
- `justify-content`: 주축(Main axis) 정렬
	- `flex-start`, `flex-end`, `center`, `space-between`, `space-evenly`, `space-around`
- `align-items`: 교차축(Cross axis) 정렬
	- `flex-start`, `flex-end`, `center`, `baseline`
```css
.flex-container {
	display: flex;
	flex-direction: row;
	justify-content: flex-start;
}
```

### 6.3. Grid (2차원 레이아웃)
- ==행과 열의 격자(Grid) 형태로 배치==
- 대규모의 복잡한 웹 페이지 레이아웃에 적합
- `display: grid`
- `grid-template-columns`, `grid-template-rows`: 열과 행의 수와 너비 결정
	- 단위 `fr`(fraction), `repeat(반복 횟수, 반복 값)`
- `gap`(`grid-gap`), `grid-column-gap`, `grid-row-gap`: 간격
- `grid-column-start/end`(`grid-column`), `grid-row-start/end`(`grid-row`): 영역 지정
- 정렬: `justify-items`(수평), `align-items`(수직) - `start`, `end`, `center`, `stretch`(기본값)
```css
.grid-container {
	display: grid;
	grid-template-columns: auto auto auto;	/* 3열 */
	gap: 10px;
}
.item1 {
	grid-column-start: 1;
	grid-column-end: 3;	/* 1~2열 차지 */
}
```

### 6.4. float 속성
- 요소를 브라우저의 가장 왼쪽(`left`)이나 오른쪽(`right`)에 배치
	- `float: right`면 가장 오른쪽에 배치되고, 뒤따르는 요소는 그 왼쪽에 배치
- `clear`: float 흐름을 제거할 때 사용
- *간단한 좌우 정렬용으로 사용하며, flex나 grid로 대체된다.*

### 6.5. position 속성
- *요소의 위치를 직접 제어할 때 사용한다.*
- `top`, `bottom`, `left`, `right`로 위치를 정하며, position 값이 **기준 위치**를 결정

| 속성값        | 지정 방식 | 의미                                                              |
| ---------- | ----- | --------------------------------------------------------------- |
| `static`   | 정적 위치 | 기본값, 문서 순서대로 배치. `top/left` 등의 영향을 받지 않음                         |
| `relative` | 상대 위치 | 자신의 원래(정적) 위치를 기준으로 오프셋만큼 이동                                    |
| `absolute` | 절대 위치 | 가장 가까운 포지셔닝된 상위(부모) 요소 기준. 다른 박스와 독립적이며 중첩 가능                  |
| `fixed`    | 고정 위치 | 브라우저 창(뷰포트) 기준. 스크롤해도 위치 고정, 다른 박스와 중첩 가능                       |

### 6.6. div와 시맨틱 요소를 이용한 레이아웃
- **`<div>` 요소를 이용한 레이아웃**
	- `<div>`는 논리적 섹션일 뿐 자체적인 의미는 없음

- **시멘틱 요소를 이용한 레이아웃**
	- `<div>` 대신 의미가 있는 태그(`header`, `nav`, `section`, `article`, `aside`, `footer` 등)를 사용하는 것이 권장됨 → [[02_HTML5#7. 시맨틱 태그]]
	- 시멘틱 요소는 문서의 구조와 의미만 표현하고, 위치와 모양은 CSS로 별도 정의
```css
/* header / (nav + content) / footer 구조 */
#header { width: 100%; height: 50px; }
#wrapper { display: flex; flex-direction: row; justify-content: space-between; }
#nav { width: 30%; height: 300px; }
#content { width: 70%; height: 300px; }
#footer { width: 100%; height: 50px; }
```
