---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-11-12
---
## 1. React 소개
- **React**
	- 2013년 페이스북(현 Meta)이 개발한 자바스크립트 라이브러리로, 사용자 인터페이스(UI)를 구축하는 데 사용된다.

### 1.1. 주요 특징
- **JSX**(Javascript Syntax eXtension)
	- 자바스크립트의 확장 문법
	- HTML 코드를 자바스크립트 내부에 작성할 수 있다.
	- 이 코드는 브라우저가 이해할 수 있는 자바스크립트로 변환된다.

- **컴포넌트 기반 아키텍처**
	- UI를 재사용 가능한 독립적인 단위인 컴포넌트로 분리하여 개발한다.
	- 이를 통해 코드의 재사용성과 유지보수성이 향상된다.

- **가상 DOM(Virtual DOM)**
	- 실제 DOM을 추상화한 개념으로, 메모리에 가상으로 존재하는 DOM을 의미한다.
	- React는 렌더링 이전/이후의 화면 구조를 가진 2개의 가상 DOM 객체를 유지한다.
	- 웹 페이지 상태 변화가 일어나면 가상 DOM 객체를 비교하여 변경된 부분만 실제 DOM에 반영한다.
	- 이 과정은 불필요한 화면의 갱신을 최소화하여 성능을 향상시킨다.

- **React Native**
	- 모바일 UI 프레임워크인 React Native를 사용하면 모바일 애플리케이션도 개발할 수 있다.

## 2. React 프로젝트 생성
### 2.1. 프로젝트 시작
> React 프로젝트를 시작하는 방법에는 크게 두 가지가 있다.

- **Create React App(CRA)**
	- 과거에 가장 일반적으로 사용되던 방식
	- Meta에서 제공하는 명령어 `npx create-react-app [프로젝트명]`을 통해 프로젝트를 설정
	- 2025년 2월 14일부터 공식 지원이 종료

- **Vite**
	- CRA의 대안으로 떠오르는 최신 빌드 도구
	- Babel 대신 SWC(Speedy Web Compiler)를 사용하여 개발 속도를 크게 향상
	- `npm create vite@latest` 명령어로 프로젝트를 생성
	- React뿐만 아니라 다양한 프레임워크를 지원

### 2.2. 프로젝트 폴더 구조
- 폴더 구조
	- `node_modules`(라이브러리)
	- `public`(정적 파일)
	- `src`(소스 코드)
	- `package.json`(프로젝트 정보 및 의존성) 등

## 3. JSX 문법
> JSX는 자바스크립트 코드 내에서 UI를 직관적으로 표현할 수 있게 해주는 문법

### 3.1. JSX 문법 특징
- **JSX 요소(element)**
	- HTML 태그를 JavaScript 코드의 변수에 저장하거나 인자로 전달할 수 있다.

- **JSX 속성(attribute)**
	- 태그의 속성명은 camelCase로 작성한다.
	- `class`가 아닌 `className`으로 작성한다.
	- 속성에 따옴표를 이용하여 문자열 리터럴을 정의한다.
```jsx
// HTML
<h1 class="greetings">Hello</h1>;
// JSX
<h1 className="greetings">Hello</h1>; 
```

- **태그 정의**
	- 콘텐츠가 없는 모든 태그는 닫혀야 한다. (Self-closing Tag; HTML 보다 엄격)
	- <div />, <img />

- **JavaScript 표현식**
	- JSX 요소 내에서 중괄호(`{}`)를 사용하여 JavaScript 변수값을 사용할 수 있다.

- **return 문장**
	- return값은 항상 하나의 요소를 리턴해야 한다.
	- 2개 이상의 요소를 리턴할 경우,
		- ① 최상위 요소로 해당 요소를 감싸 리턴한다.
		- ② React의 Fragment를 사용한다.
		- ③ 빈 태그를 사용한다.
```jsx
// app.jsx (Fragment를 사용하여 여러 요소 반환)
import { Fragment } from "react";

const App = () => {
  return (
    <Fragment>
      <h1>hello</h1>
      <p>안녕하세요!</p>
    </Fragment>
  );
};

export default App;
```

- **컴파일**
	- Babel과 같은 트랜스파일러는 JSX 코드를 `React.createElement()` 함수 호출로 변환하여 최종적으로 JavaScript 객체를 생성한다.

## 4. 요소(Element) 렌더링
- **React 요소**
	- React 앱을 구성하는 가장 작은 단위
	- 화면에 표시될 내용을 기술한 자바스크립트 객체
	- 이 요소들은 가상 DOM에 존재한다.

- **렌더링 과정**
	- 모든 React 앱은 하나의 '루트 DOM 노드'(일반적으로 `index.html`의 `<div id="root"></div>`)를 가진다.
	- `ReactDOM.render()`(또는 최신 버전의 `createRoot().render()`) 함수를 통해 React 요소를 이 루트 노드에 렌더링하며, 이 과정에서 가상 DOM의 내용이 실제 브라우저 DOM으로 이동하여 화면에 표시된다.

## 5. 컴포넌트(Component)
- **컴포넌트**
	- React 애플리케이션의 모듈화/캡슐화된 구성 요소 단위 (코드 재사용성과 확장성)
	- UI를 표현하기 위해 `props`를 입력받아 React 요소를 출력하는 함수

- **컴포넌트의 종류**
	- **클래스형 컴포넌트**
		- JavaScript ES6의 `class` 문법으로 정의
	- **함수형 컴포넌트**
		- JavaScript 함수 문법으로 정의 (`props`를 인수로 받고 컴포넌트의 마크업을 반환하는 함수)
		- 클래스형 컴포넌트보다 코드가 간결하고 성능이 좋아 선호된다.

```jsx
// 클래스형 컴포넌트
import React from 'react';

class Welcome extends React.Component {
	render() {
		return <h1>Hello, {this.props.name}</h1>;
	}
}

export default Welcome;

// 함수형 컴포넌트
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}
export default Welcome; 
```

- **컴포넌트 이름 규칙**
	- 컴포넌트 이름은 반드시 대문자로 시작
	- 소문자로 시작하는 컴포넌트를 DOM 태그로 인식

### 5.1. 실습 1
```jsx
// src/components/Header.jsx
function Header() {
  return (
    <header className="header">
      <h3>헤더(header) 컴포넌트입니다.</h3>
    </header>
  );
}
export default Header;
```

```jsx
// src/components/Home.jsx
import Post from "./Post";

function Home() {
  return (
    <div className="home">
      <h3>홈(home) 컴포넌트 - 본문 내용을 표현합니다.</h3>
      <Post />
    </div>
  );
}
export default Home;
```

```jsx
// src/components/Footer.jsx
function Footer() {
  return (
    <footer className="footer">
      <h3>풋터(footer) 컴포넌트입니다.</h3>
    </footer>
  );
}
export default Footer;
```

```jsx
// src/App.jsx (컴포넌트 조합)
import { Fragment } from "react";
import Header from './components/Header.jsx';
import Home from './components/Home.jsx';
import Footer from './components/Footer.jsx';

const App = () => {
  return (
    <Fragment>
      <Header />
      <Home />
      <Footer />
    </Fragment>
  );
};
export default App;
```

### 5.2. 실습 2
```jsx
// src/components/Post.jsx
import React from "react";

const Post = () => {
  const headerName = ["번호", "제목", "작성일", "작성자"];
  const postList = [
    { id: '1', title: "HTML", createdDate: "2024-07-01", name: "버너스 리" },
    { id: '2', title: "JavaScript", createdDate: "2024-09-03", name: "브랜든 아이크" },
    { id: '3', title: "React", createdDate: "2024-10-04", name: "조던 워커" },
    { id: '4', title: "Spring", createdDate: "2024-08-02", name: "로드 존슨" },
    { id: '5', title: "Solidity", createdDate: "2024-08-02", name: "가빈 우드" },
    { id: '6', title: "Bitcoin", createdDate: "2024-11-05", name: "사토시" }
  ];

  return (
    <>
      <table>
        <thead>
          <tr>
            {headerName.map((item, index) => <th key={index}>{item}</th>)}
          </tr>
        </thead>
        <tbody>
          {postList.map((item, index) => (
            <tr key={item.id}>
              <td>{index + 1}</td>
              <td>{item.title}</td>
              <td>{item.createdDate}</td>
              <td>{item.name}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </>
  );
};
export default Post;
```

## 6. 컴포넌트 스타일링
> 컴포넌트에 스타일을 적용하는 방법은 다음과 같다.

- **기본 CSS**
	- 외부 CSS 파일을 작성하고,
	- 컴포넌트의 `className` 속성을 통해 스타일을 적용

- **Styled-components**
	- 스타일을 컴포넌트의 일부로 캡슐화하는 라이브러리

- **CSS Module**
	- CSS 파일명을 `[이름].module.css` 형태로 만들어
	- 스타일이 해당 컴포넌트에만 적용되도록 범위를 제한하는 방식

### 6.1. 실습
- src/styles/App.css
```css
.header {
	background-color: #e2c100;
	margin: 0 auto;
	border: 0px solid #000000;
	padding: 20px;
	width: 700px;
	height: 50px;
	text-align: center;
}

.footer {
	background-color: #87c7e4;
	margin: 0 auto;
	border: 0px solid #000000;
	padding: 20px;
	width: 700px;
	height: 50px;
	text-align: center;
}

.home {
	background-color: #dbe25a;
	margin: 0 auto;
	border: 0px solid #000000;
	padding: 20px;
	width: 700px;
	height: 400px;
	overflow: scroll;
	text-align: center;
}

.home > table {
	background-color: #e0f5cc;
	width:70%;
	margin-left:auto;
	margin-right:auto;
	border-collapse : collapse;
	border : 0px solid black;
}

.home th {
	background-color: #bfdfec;
}
```

## 7. props
> `props`(properties)는 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하기 위한 매개변수

- **데이터 전달**: JSX에서 `key-value` 형태로 자식 컴포넌트에 값을 전달한다. (예: `<Header title="제목" />`)
- **읽기 전용**: 자식 컴포넌트는 전달받은 `props`를 직접 수정할 수 없다. (단방향 데이터 흐름)
- **다양한 값 전달**: 문자열 외의 값(숫자, 객체 등)은 중괄호 `{}`를 사용해 전달한다.
- **구조 분해 할당**: `props` 객체에서 필요한 값을 추출하여 사용할 수 있다. (예: `function Header({ title })`)
- **`props.children`**: 컴포넌트 태그 사이에 다른 컴포넌트나 요소를 넣으면, `children`이라는 `props` 속성으로 전달된다.

## 8. 빌드와 실행
- **빌드(Build)**: `npm run build` 명령어를 실행하면, 개발된 JSX 및 자바스크립트 코드가 브라우저에서 직접 실행될 수 있는 정적인 HTML, CSS, JS 파일로 변환된다. 이 결과물은 보통 `dist` 폴더에 생성된다.

- **실행**: 빌드된 정적 파일들은 웹 서버(예: Node.js의 Express)를 통해 사용자에게 제공될 수 있다. 서버는 요청이 들어올 때 `dist` 폴더 안의 `index.html` 파일을 전송하여 React 애플리케이션을 실행시킨다.
