---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-11-19
---
## 1. 컴포넌트 생명주기
- **컴포넌트 생명주기**(component lifecycle)
	1. **생성**(mount): 컴포넌트가 생성되어 DOM에 삽입되는 단계
	2. **갱신**(update): 컴포넌트의 `props`나 `state`가 변경되어 리렌더링이 발생하는 단계
	3. **제거**(unmount): 컴포넌트가 DOM에서 제거되는 단계

## 2. Hook
- **정의**
	- 함수형 컴포넌트에서 상태(state) 관리와 생명주기 기능을 연동할 수 있게 해주는 함수

- **주요 내장 훅**
	- `useState`: 상태 관리
	- `useEffect`: 부수 효과(Side effect) 처리
	- `useRef`: DOM 접근 또는 변하지 않는 값 유지
	- `useMemo`, `useCallback`: 성능 최적화

```jsx
// useState();
const [state, setState] = useState(초기값);

// useEffect();
useEffect(() => {}); // 렌더링마다 실행
useEffect(() => {}, []); // 최초 마운트 시 실행
useEffect(() => {}, [dependency]); // 최초 마운트 및 의존성 변수 값이 변경될 때 실행
useEffect(() => {return () => {};}, []); // 언마운트 시 실행
```

- **사용 규칙**
	- 반드시 컴포넌트의 **최상위 레벨**에서만 호출 (조건문, 반복문 내 호출 금지)
	- **리액트 함수 컴포넌트** 또는 **커스텀 훅** 내부에서만 호출 가능

### 2.1. useState()
- `useState();`: 컴포넌트 내에서 변경 가능한 동적인 값(State)을 관리하는 대표적인 훅

- **특징**
	- 일반 변수와 달리, **값이 변경되면 컴포넌트가 리렌더링**(Re-rendering) 되어 화면이 갱신
	- `setState()` 함수를 통해서만 값을 변경해야 한다.

### 2.2. useEffect()
- `useEffect();`: 컴포넌트가 렌더링된 이후에 비동기 작업이나 특정 작업(Side effect)을 수행하기 위한 훅

### 2.3. useRef()
- `useRef();`: DOM 요소에 직접 접근하기 위해 사용하는 훅

- **특징**
	- `const ref = useRef(초기값);` 형태로 사용하며, 반환된 객체의 `.current` 속성을 통해 값에 접근
	- **값이 변경되어도 컴포넌트가 리렌더링 되지 않는다.** (렌더링과 무관한 값 저장에 유용)
	- 주로 `<input>` 태그의 포커스(focus)를 주거나 값을 초기화하는 등 DOM을 직접 조작할 때 사용

#### (1) 예제
```jsx
// InputRef.jsx
import {useState, useRef} from "react";

const InputRef = () => {
	const [text, setText] = useState("");
	const textRef = useRef(); // useRef로 생성한 객체 textRef에 저장
	const onChangeText = (event) => {
		setText(event.target.value);
	};
	const onClickButton = () => {
		alert(text);
		textRef.current.value = ""; // textRef 로 입력 폼을 초기화
	}
	return (
		<div className="app">
			<h2 style={{ color: "blue"}}>useRef 함수를 이용한 입력 값 예제!</h2>
			<input ref={textRef} value={text} onChange={onChangeText}/>
			<button onClick={onClickButton}>작성 완료</button>
			<div>{text}</div><br></br>
		</div>
	);
}

export default InputRef; 
```

## 3. 이벤트 핸들링
- **표기법**: HTML과 달리 리액트에서는 카멜 표기법(camelCase)을 사용
	- (예: `onclick` → `onClick`)
- **전달 방식**: 함수 호출 구문이 아닌 **함수 그 자체**를 전달
	- (예: `onClick={handleClick}`)

- **주요 메서드**
	- `event.preventDefault()`: 폼 제출 시 페이지 새로고침 같은 브라우저 고유 동작을 막는다.
	- `event.stopPropagation()`: 이벤트가 상위 엘리먼트로 전파되는 것을 막는다.

- **종류**
	- `onClick`, `onChange`, `onSubmit` 등을 사용하여 사용자 입력을 처리한다.

### 3.1. 예제
```jsx
// Signup.jsx
import {useState} from "react";

const Signup = () => {
	const [name, setName] = useState("");
	const [age, setAge] = useState(0);
	const [gender, setGender] = useState("남성");
	
	const onChangeName = (event) => { setName(event.target.value); };
	const onChangeAge = (event) => { setAge(event.target.value); };
	const onChangeGender = (event) => { setGender(event.target.value); };
	
	const onClickSubmit = (event) => {
		document.getElementById("view").innerText = `onClickSubmit: 이름: ${name}, 나이: ${age}, 성별: ${gender}`;
		event.preventDefault(); // 해당 이벤트의 기본 동작을 막아준다
	}
	
	return (
		<div className="app">
			<h2 style={{ color: "blue"}}>폼 입력시 발생하는 이벤트 처리 예제!</h2>
			<form onSubmit={onClickSubmit}>
				이름: <input value={name} onChange={onChangeName}/><br/>
				나이: <input value={age} onChange={onChangeAge}/><br/>
				성별:
				<select value={gender} onChange={onChangeGender}>
					<option value="여성">여성</option>
					<option value="남성">남성</option>
				</select>
				<button type="submit">제출</button>
			</form>
			<p id="view"></p>
		</div>
	);
}

export default Signup;
```

```jsx
// App.jsx
import {Fragment} from "react";
import Count from "./components/Count";
import Input from "./components/Input";
import Calculator from "./components/Calculator.jsx";
import CountEffect from "./components/CountEffect";
import InputRef from "./components/InputRef";
import Signup from "./components/Signup";
import "./styles/App.css";

const App = () => {
	return (
		<Fragment>
			<Signup/>
		</Fragment>
	);
};

export default App;
```
