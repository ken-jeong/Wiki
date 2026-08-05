---
aliases: []
type: Lecture
tags:
  - 2-2/프론트엔드프로그래밍
draft: false
date: 2025-11-06
---
## 1. Node.js 소개
- **Node.js란?**
	- 2009년 5월 27일에 출시된 JavaScript 런타임 환경
	- Chrome의 V8 엔진을 사용하여 브라우저가 아닌 서버 환경에서 JavaScript를 실행할 수 있게 해준다.
	- 내장된 http 라이브러리를 통해 별도의 웹 서버 프로그램 없이 서버를 구축할 수 있다.
	- NPM(Node Package Manager)을 통해 방대한 JavaScript 라이브러리를 사용할 수 있다.

- **Express란?**
	- Node.js를 위한 웹 애플리케이션 프레임워크이다.
	- 클래스와 라이브러리 집합을 제공하여 서버 개발을 더 쉽고 간편하게 만들어 준다.

- **Node.js 아키텍처**
	- **단일 스레드 이벤트 루프** 아키텍처를 기반으로 동작한다.
	- V8 엔진이 JavaScript 코드를 기계어 코드로 변환한다.
	- C++ 기반의 `libuv` 라이브러리를 통해 비동기 I/O 작업을 처리하여 효율적인 성능을 낸다.

### 1.1. 전역 객체
- **전역 변수와 전역 객체**
	- Node.js 환경 어디서든 접근할 수 있는 최상위 객체(`global`)의 멤버들
	- **전역 변수**: `__filename`(현재 파일 경로), `__dirname`(현재 디렉토리 경로)
	- **전역 객체**: `console`(콘솔 출력), `process`(실행 중인 프로세스 정보), `require`(모듈 불러오기) 등

```js
// global.js
console.log('filename:', __filename);
console.log('dirname:', __dirname);

process.argv.forEach(function (item, index) {
    console.log(index + ' : ' + typeof(item) + ' : ', item);
});
```

## 2. Node.js 프로젝트
- **NPM**(Node Package Manager)
	- Node.js 패키지 관리자
	- [중앙 저장소](https://npmjs.com)

- `npm init` 명령어 (또는 `npm create`)
	- Node.js 프로젝트를 초기화하는 명령어
	- 프로젝트의 메타데이터를 입력받아 `package.json` 파일을 생성한다.

- `package.json` 파일
	- 프로젝트의 메타데이터 (이름, 버전 등)
	- 패키지 의존성 (`npm install` 명령어로 설치)
	- 스크립트 실행 방법 (`npm run start` 등)

- **프로젝트 스크립트 사용**
	1. `package.json`의 `scripts` 항목에 `"start": "node index.js"`와 같이 스크립트를 추가한다.
	2. `index.js` 파일을 생성하고 서버 코드를 작성한다.
	3. 터미널에서 `npm run start` 명령어를 실행하면 `node index.js`가 실행되어 서버가 구동된다.

### 2.1. 실습 (httpserver 프로젝트)
- **간단한 HTTP 서버 예제**
	- Node.js의 내장 `http` 모듈을 사용하여 간단한 웹 서버를 만들 수 있다.
	- `http.createServer()`를 통해 서버 인스턴스를 생성하고, `listen()` 함수로 특정 포트에서 요청을 대기한다.
	- 리스너 함수는 `request` 이벤트에 대한 콜백(callback) 역할을 수행하여 요청을 처리한다.

```js
// index.js (HTTP 서버 코드)
let http = require("http");
let server = http.createServer(function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
    res.write("<html>");
    res.write("<body><h3>Hello World - 안녕</h3></body>");
    res.write("</html>");
    res.end();
});

server.listen(8080, "localhost");
console.log("Server is running");
```

## 3. Node.js 모듈
- **모듈(Module)이란?**
	- 코드 재사용을 위한 단위로, 독립적인 기능을 갖는 파일 또는 함수들의 집합이다.
	- 기능별로 코드를 분리하여 개발 효율성과 유지보수성을 높인다.
	- *객체 지향 언어의 class 또는 package 단위*

- JavaScript script 태그는 파일마다 선언된 전역 변수가 중복되는 문제

### 3.1. 모듈 시스템
#### (1) CJS
- **CJS**(CommonJS)
	- Node.js의 모듈 시스템
	- `require`와 `module.exports` 문장을 사용한다.
	- `package.json` 파일에서 `"type": "commonjs"`으로 설정한다.

#### (2) ESM
- **ESM**(ECMAScript Modules)
	- 최신 프레임워크(React, Vue, Angular 등)의 모듈 시스템
	- `import`와 `export` 문장을 사용한다.
	- `package.json` 파일에서 `"type": "module"`으로 설정한다.

- **모듈 개별 내보내기**
	- `export` 키워드를 변수나 함수 앞에 붙여 다른 파일에서 사용할 수 있도록 공개한다.
	- `export default`로 내보내면 불러올 때 자유롭게 이름을 지정할 수 있다.

- **모듈 개별 불러오기**
	- `import { 변수/함수명, ... } from "파일경로"`

- **모듈 전부 불러오기**
	- `import * as "모듈이름" from "파일경로"`

### 3.2. 실습 (usemodule 프로젝트)
- **package.json 설정**
	- npm init으로 파일 생성 후, ESM을 사용하기 위해 "type": "module" 필드를 추가
	- scripts에 "start": "node index.js"를 추가

```js
// circle.js (모듈 파일)
export const PI = 3.141592; // 원주율

export function getArea(radius) { // 원의 면적
    return PI * radius * radius;
}

export function getCircumference(radius) { // 원의 둘레
    return PI * radius * 2;
}
```

```js
// index.js (모듈 사용 파일)
import { PI, getArea, getCircumference } from "./circle.js";

console.log(PI, getArea(10), getCircumference(10));
```

## 4. 라이브러리 사용
- **외부 라이브러리 설치**
	- `npm install [패키지명]` 명령어를 통해 `npmjs.com`에 등록된 외부 모듈을 설치할 수 있다.
	- 이는 JAVA의 MAVEN이나 Gradle과 유사한 개념이다.

- **Express 라이브러리**
	- Node.js를 위한 빠르고 간결한 웹 서버 프레임워크이다.
	- `npm install --save express` 명령어로 설치할 수 있다.
		- (npm 5+ 버전부터는 `--save` 옵션이 기본값이라 생략 가능)
	- 설치 시 `package.json`의 `dependencies` 항목에 자동으로 추가된다.

- **Express를 이용한 서버 작성 및 라우팅**
	- `require('express')`로 모듈을 불러와 간단하게 서버를 생성할 수 있다.
	- **페이지 라우팅**: `get()`, `post()` 등의 메서드를 사용하여 특정 URL 경로에 대한 요청을 처리하는 이벤트 리스너를 설정한다.
	- **REST API 제공**: Express를 사용하여 HTML 파일뿐만 아니라 JSON 형식의 데이터를 제공하는 REST API 서버를 쉽게 구축할 수 있다.

### 4.1. 실습 (expressserver 프로젝트)
```js
// index.js
let express = require('express');
let server = express();
let path = require('path');

server.set('port', 8080);

server.get('/', function(req, res) {
    res.type('text/html');
    res.sendFile(path.join(__dirname, 'index.html'));
});

server.get('/json', function(req, res) {
    res.type('application/json');
    res.header("Access-Control-Allow-Origin", "*");
    res.sendFile(path.join(__dirname, 'student.json'));
});

server.listen(server.get('port'), function() {
    console.log('Server Started');
});
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
</head>
<body>
    <h3>Hello, world!</h3>
</body>
</html>
```

```json
"scripts": {
	"start": "node index.js",
	"test": "echo \"Error: no test specified\" && exit 1"
},
```

```json
// student.json
[
{"id":"hansol","passwd":"hansol","username":"정한솔","snum":"2012152040","depart":"컴퓨터(주)","mobile":"010-1111-9999","email":"hansol@tukorea.ac.kr"},
{"id":"hodong","passwd":"password","username":"강호동","snum":"20189999","depart":"소프트웨어공학","mobile":"010-5555-0000","email":"hodong@tukorea.ac.kr"},
{"id":"kildong","passwd":"password","username":"홍길동","snum":"20219999","depart":"소프트공학","mobile":"010-3333-4444","email":"kildong@tukorea.ac.kr"},
{"id":"minho","passwd":"password","username":"최민호","snum":"20221234","depart":"컴퓨터공학","mobile":"010-2222-3334","email":"minho@tukorea.ac.kr"},
{"id":"tuk","passwd":"1234","username":"한공대","snum":"20231234","depart":"컴퓨터","mobile":"010-4545-7878","email":"tuk@tukorea.ac.kr"
]
```
