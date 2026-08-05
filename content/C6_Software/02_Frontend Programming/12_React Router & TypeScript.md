---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-11-26
---
## 1. React Router
### 1.1. 개념 및 특징
- **라우팅**(Routing)
	- URL 주소 경로에 따라 다른 컴포넌트를 화면에 보여주는 기능이다.

- **CSR**(Client Side Rendering)
	- 리액트는 CSR 방식을 사용한다.
	- 초기 로딩 시 HTML과 JS를 다운로드하여 SPA(Single Page Application)를 실행한다.
	- 페이지 이동 시 서버로부터 새로운 페이지를 받는 것이 아니라, 필요한 데이터만 요청하여 클라이언트(브라우저)에서 페이지를 교체한다.

- **SSR**(Server Side Rendering)
	- 클라이언트 요청 시마다 서버가 새로운 페이지(HTML, JS)를 생성하여 제공하는 방식이다.

### 1.2. 설치 및 설정
- **설치**: `npm install react-router` (SPA 내부에서 페이지 이동을 가능하게 함)

- **3가지 모드**
	- **선언적 모드**: 기본적인 라우팅 기능 (단순 SPA)
	- **데이터 모드**: 데이터 로딩 및 처리(Loader, Action) 포함
	- **프레임워크 모드**: 풀스택 수준 기능 제공

- **설정 컴포넌트**
	- `<BrowserRouter>`: 라우터를 적용할 최상위 컴포넌트
	- `<Routes>`: `<Route>`들의 상위 엘리먼트
	- `<Route path="경로" element={<컴포넌트 />} />`: URL 경로와 보여줄 컴포넌트 매핑
	- `<Link to="경로">`: 페이지를 새로고침 하지 않고 이동하는 링크 컴포넌트

#### (1) 실습
```jsx
// Home.jsx
import React from 'react';
import { Link } from "react-router";

const Home = () => {
	return (
		<div className="app">
			<h2 style={{color: "blue"}}>Home Page</h2>
			<p><Link to={`/post`}>게시물 목록 보기</Link></p>
		</div>
	);
}

export default Home;
```

```css
/* App.css */
.app {
	margin: 5 auto;
	text-align: center;
}

.app > table {
	width:70%;
	margin-left: auto;
	margin-right: auto;
	border-collapse: collapse;
	background-color: #e0f5cc;
}

.app th, td {
	padding: 3px;
	border: 1px solid black;
	text-align: center;
}
```

```jsx
// App.jsx
import ReactDOM from "react-dom";
import { BrowserRouter, Routes, Route } from "react-router";
import {Fragment} from "react";
import Count from "./components/Count.jsx";
import Input from "./components/Input.jsx";
import CountEffect from "./components/CountEffect.jsx";
import InputRef from "./components/InputRef.jsx";
import Signup from "./components/Signup.jsx";
import Post from "./components/Post.jsx";
import Home from "./components/Home.jsx";
import "./styles/App.css";

const App = () => {
	return (
		<BrowserRouter>
		<Routes>
			<Route exact path="/" element={<Home/>}/>
			<Route path="/post" element={<Post/>}/>
		</Routes>
		</BrowserRouter>
	);
};

export default App;
```

### 1.3. 동적 경로 라우팅
- **URL 파라미터**
	- `path="/post/:id"`와 같이 설정
	- `useParams()` 훅을 사용하여 파라미터 값(`{id: 값}`)을 읽어옴

- **쿼리 스트링**
	- `?id=7`과 같은 형태
	- `useSearchParams()` 훅을 사용하여 값을 조회 (`params.get('key')`)

## 2. AXIOS
- **AXIOS**: Promise 기반의 HTTP 비동기 통신 라이브러리
    - 응답 데이터를 자동으로 JSON 객체로 파싱한다.
    - `npm install axios`로 설치한다.

### 2.1. 사용 문법
- **GET 요청 (데이터 조회)**
```javascript
axios.get("URL")
  .then((res) => console.log(res.data))
  .catch((err) => console.error(err));
```

- **POST 요청 (데이터 전송)**
```javascript
axios.post("URL", {데이터객체})
  .then(response => { /* ... */ })
  .catch(error => { /* ... */ });
```

#### (1) 예제
```jsx
axios.get("http://210.93.60.16:8080/frontend/task/score")
```

```jsx
// StudentList.jsx
import React, { useState, useEffect } from "react";
import axios from "axios";
import { Link } from "react-router";

const StudentList = () => {
	const [studentList, setStudentList] = useState([]);
	
	useEffect(() => {
		fetchStudents ()
	}, []);
	
	const fetchStudents = () => {
		axios.get('http://210.93.60.16:8080/frontend/RestController?cmd=list')
		.then((response) => {
			setStudentList(response.data);
		})
		.catch((error) => {
			console.log("Error fetching students:", error);
		});
	}
	
	return (
		<div className="app">
			<table>
				<thead>
					<tr>
						<th>계정</th>
						<th>비밀번호</th>
						<th>이름</th>
						<th>학번</th>
						<th>학과</th>
						<th>전화번호</th>
						<th>이메일</th>
					</tr>
				</thead>
				<tbody>
				{
					studentList.map((item)=>{
						return (
							<tr>
								<td>{item.id}</td>
								<td>{item.passwd}</td>
								<td>{item.username}</td>
								<td>{item.snum}</td>
								<td>{item.depart}</td>
								<td>{item.mobile}</td>
								<td>{item.email}</td>
							</tr>
						);
					})
				}
				</tbody>
			</table>
			<p><Link to={`/`}>홈 페이지</Link></p>
		</div>
	);
}

export default StudentList
```

```jsx
// index.js
import ReactDOM from "react-dom";
import {Fragment} from "react";
import Count from "./components/Count";
import Input from "./components/Input";
import CountEffect from "./components/CountEffect";
import InputRef from "./components/InputRef";
import Signup from "./components/Signup";
import Post from "./components/Post";
import Home from "./components/Home";
import StudentList from "./components/StudentList";
import "./styles/App.css";

const App = () => {
	return (
		<BrowserRouter>
		<Routes>
			<Route exact path="/" element={<Home/>} />
			<Route path="/post" element={<Post/>} />
			<Route path="/student" element={<StudentList/>} />
		</Routes>
		</BrowserRouter>
	);
};

export default App;
```

```jsx
// Home.jsx
import React from 'react';
import { Link } from "react-router";

const Home = () => {
	return (
		<div className="app">
			<h2 style={{ color: "blue"}}>Home Page</h2>
			<p><Link to={`/post`}>게시물 목록 보기</Link></p>
			<p><Link to={`/student`}>학생 목록 보기</Link></p>
		</div>
	);
}

export default Home;
```

### 2.2. 리액트 활용 예제
- `useState`로 데이터를 저장할 상태를 관리한다.
- `useEffect`를 사용하여 컴포넌트 마운트 시 `axios.get`을 호출해 서버 데이터를 받아오고 상태를 업데이트한다.

## 3. TypeScript
### 3.1. 개념 및 특징
- **정의**: 마이크로소프트가 개발한 자바스크립트 기반의 **정적 타입 문법**을 추가한 언어이다.

- **특징**
	- 자바스크립트의 슈퍼셋(Superset)이다.
	- 컴파일러(tsc)나 바벨을 통해 자바스크립트 코드로 변환된다.
	- 코드 작성 단계에서 **타입 체크**가 가능하여 실행 시간 오류를 줄이고 디버깅이 용이하다.

- **설치**: `npm install -g typescript` (실행: `tsc 파일명.ts`)

### 3.2. 자료형(Data Types)
- **기본 타입**
	- `boolean`: true, faulse
	- `string`
	- `number`: 부동 소수점 값 사용
	- `null`: 값이 없음
	- `undefined`: 대입한 적 없음
	- `symbol`
	- `bigint`

```ts
// type_number.ts
function addNumber(x: number, y: number): number {
	return x + y;
}

let num1: number = 5;
let num2: number = 2.5;

let result: number = addNumber(num1, num2);
console.log(result);
```

- **객체 및 배열**
	- `Array`: `type[]` 또는 `Array<type>` (제네릭) 사용
	- `Tuple`: 고정된 수의 요소를 가지며, 각 요소의 타입이 정해져 있는 배열
	- `Enum`: 숫자 혹은 문자열 값 집합에 이름을 부여

- **특수 타입**
	- `any`: 모든 타입 허용 (사용 지양)
	- `unknown`: 모든 값 할당 가능하나, 다른 타입 변수에 할당 불가 (안전한 any)
	- `void`: 주로 함수의 반환 값이 없을 때 사용
	- `never`: 절대 발생할 수 없는 타입 (무한 루프 등)

- **고급 타입**
	- `Union (|)`: 여러 타입 중 하나 선택 (OR)
	- `Intersection (&)`: 여러 타입을 결합하여 새로운 타입 생성 (AND)

```ts
type ID = number | string; // Type Alias & Union
let user_id: ID = 123;
user_id = "abc-456";

interface Person {
  name: string;
}

interface Employee {
  salary: number;
}

type FullTimeEmployee = Person & Employee; // Intersection

const jun: FullTimeEmployee = {
  name: "Jun",
  salary: 5000,
};
```

### 3.3. 타입 정의 방식
- **Type Alias**(타입 별칭)
	- `type Name = ...` 형태로 기존 타입에 이름을 붙인다.
	- 확장이 불가능하다.

- **Interface**(인터페이스)
	- `interface Name { ... }` 형태로 객체의 구조(규칙)를 정의한다.
	- `extends`를 통해 확장이 가능하다.

- **클래스 구현**
	- `implements` 키워드를 사용하여 Interface나 Type Alias에서 정의한 규칙을 따르는 클래스를 구현할 수 있다.

```ts
type Pet = {
	name: string;
	cry(): void;
};

class Cat implements Pet {
	name: string;
	cry() { console.log("야옹야옹"); }
}

let myCat = new Cat();
myCat.name = "페르시안";
myCat.cry();
```

### 3.4. tsconfig.json
- TypeScript 프로젝트의 설정 파일
	- 컴파일러 설정
	- 프로젝트 루트 지정
	- 파일 및 폴더 관리
	- 개발 도구 지원
