---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-10-08
---
## 1. DOM (Document Object Model) 개요
- **정의**
	- 브라우저가 ==HTML 문서를 해석하여 요소(Element)들을 트리(Tree) 형태로 구조화==한 객체 모델이다.

- **역할**
	- 자바스크립트는 DOM을 통해 HTML의 각 요소에 접근하고, 변경하여 동적으로 화면을 갱신(렌더링)한다.

- **구조**
	- DOM 트리의 최상위 노드는 `document` 객체이다.
	- **BOM (Browser Object Model)**: 웹 브라우저 전체를 객체로 표현한 것으로, 최상위 객체는 `window`이다. (`document`도 `window`의 하위 객체)

- **렌더링 과정**
	- DOM 트리 틀 생성(document) → HTML 요소 읽기 및 객체 생성 → 화면 출력 → 변경 시 갱신

## 2. DOM 객체의 5가지 구성 요소
- *HTML 요소의 구성 요소 5가지*
	- **요소**: 기본 단위
	- **속성**: 추가 정보
	- **CSS style**: 스타일
	- **이벤트 리스너**: 동작
	- **innerHTML**: 요소 내부 콘텐츠 (`getElementById("id").innerHTML = "";`)

> DOM 객체(노드)는 HTML 요소의 특성을 반영하며 다음 5가지를 포함한다.

1. **프로퍼티 (Property)**: HTML 태그의 속성(attribute)을 반영하는 멤버 변수 (예: `id`, `tagName`)
2. **메서드 (Method)**: 태그를 제어하는 멤버 함수
3. **컬렉션 (Collection)**: 자식 객체들의 주소를 가지는 배열과 유사한 집합적 정보
4. **이벤트 리스너 (Event Listener)**: `onclick` 등 약 70여 개의 이벤트 처리기
5. **CSS3 스타일 (Style)**: CSS 스타일 시트 정보를 반영하여 모양을 제어 (`style` 프로퍼티)

## 3. HTML 요소 찾기
> 동적인 페이지 작성을 위해 요소를 찾는 다양한 방법을 제공한다.

- ==직접 찾기==
	- `document.getElementById("id")`: ID로 찾기 (단일 요소)
	- `document.getElementsByTagName("tag")`: 태그 이름으로 찾기 (배열 반환)
	- `document.getElementsByClassName("menu")`: 클래스 이름으로 찾기
	- `document.querySelectorAll("selector")`: CSS 선택자로 찾기 (배열 반환)

- **DOM 트리 순회 (관계 이용)**
	- `childNodes`: 모든 자식 요소 배열
	- `firstChild` / `lastChild`: 첫 번째/마지막 자식
	- `parentNode`: 부모 노드
	- `nextSibling` / `previousSibling`: 다음/이전 형제 노드

## 4. HTML 요소 값 읽기
- **일반 태그**
	- `innerHTML` 속성을 통해 시작 태그와 종료 태그 사이의 내용을 읽는다.

- **입력 양식 (Form)**
	- `value` 속성을 사용한다.
	- **Forms 배열 객체**: `document.forms` 또는 `document.폼이름`으로 접근 가능

- **컨트롤별 읽기 방법**
	- **Text/Textarea**: `.value`
	- **Checkbox**: `.checked` (true/false 확인)
	- **Radio**: 같은 `name`을 가진 배열을 순회하며 `.checked`가 true인 항목의 `.value` 확인
	- **Select (Dropdown)**: `.selectedIndex`로 선택된 인덱스를 찾고 `.options[index].value`로 값 확인

|                적용 대상                |         속성명          |
| :---------------------------------: | :------------------: |
| `<input>`, `<textarea>`, `<select>` |     **`.value`**     |
|   `<input type="checkbox/radio">`   |    **`.checked`**    |
|             `<select>`              | **`.selectedIndex`** |

```js
// 라디오 버튼에서 읽기
function getValueR() {
	let list = document.getElementsByName('color');
	for (let i = 0; i < list.length; i++) {
		if (list[i].checked) { //
			document.getElementById("result").value = list[i].value;
			break;
		}
	}
}

// SelectList에서 값 읽기
function getValueL() {
	let selected, s;
	selected = document.getElementById("myList").selectedIndex; //
	s = document.getElementById("myList").options[selected].value;
	document.getElementById("result4").value = s;
}
```

## 5. HTML 요소 변경하기
> DOM을 통해 요소의 내용, 속성, 스타일을 변경할 수 있다.

1. **내용 변경**
	- `innerHTML` 속성에 HTML 코드나 텍스트를 대입
	- *주의:* `<input>` 등 닫는 태그가 없는 폼 요소는 `innerHTML` 대신 `value`를 사용해야 함

2. **속성 변경**
	- 객체의 프로퍼티에 직접 접근하여 변경
	- (예: `img.src = "..."`)

3. **스타일 변경**
	-  `style` 객체의 프로퍼티 변경
	- (예: `.style.color = "red"`, `.style.visibility = "hidden"`)
	- 활용 예: 다크 모드/라이트 모드 토글

## 6. DOM 노드 삭제와 추가
> `document` 객체의 메서드를 사용하여 DOM 트리를 조작한다.

- **주요 메서드**
	- `createElement("tag")`: 새로운 요소 생성
	- `appendChild(node)`: 부모 노드에 자식 노드 추가
	- `replaceChild(new, old)`: 요소 교체
	- `removeChild(node)`: 자식 노드 삭제

- **생성 과정**
	- 요소 생성(`createElement`) → 부모 찾기(`getElementById`) → 연결(`appendChild`)

- **테이블 조작**
	- `insertRow()`, `insertCell()`을 사용하면 더 간편하게 행/열 추가 가능

- **document.write()**
	- ==DOM 트리 생성 후 호출하면 기존 DOM 트리를 모두 지우고 쓰기 때문에 주의== 필요
