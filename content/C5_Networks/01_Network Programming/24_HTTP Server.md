---
aliases: []
type: Lecture
tags:
  - 3-1/네트워크프로그래밍
draft: false
date: 2026-06-09
---
- HTTP는 "요청-응답-연결종료"로 동작하는 stateless 프로토콜이며,
- 간단한 웹 서버는 GET 요청을 파싱해 해당 파일을 적절한 헤더와 함께 전송한 뒤 연결을 끊는 구조로 만들 수 있다.

## 1. HTTP 개요
**웹 서버의 기능**
- HTTP 프로토콜을 기반으로 웹 페이지에 해당하는 파일을 클라이언트에게 전송하는 역할을 하는 서버이다.
- HTTP는 Hypertext의 전송을 목적으로 설계된 **애플리케이션 레벨**의 프로토콜이다.
- Hypertext란 마우스 클릭을 통해 이동이 가능한, 일반적으로 HTML로 이뤄진 텍스트를 뜻한다.

**HTTP의 핵심 특성 — Stateless**
- HTTP의 기본 통신 방식은 "데이터 요청 → 데이터 응답 → 연결 종료"의 흐름으로 이뤄진다.
- 한 번의 요청·응답이 끝나면 연결을 끊어버리기 때문에 클라이언트의 **상태 정보를 유지하지 않는(stateless)** 프로토콜이다.

### 1.1. HTTP 요청과 응답 메시지
**요청(Request) 메시지 구조**
- 요청 라인: 요청 방식·파일·프로토콜 정보 (예: `GET /index.html HTTP/1.1`)
- 메시지 헤더: 부가 정보 (예: `User-Agent`, `Accept`)
- 공백 라인
- 메시지 몸체: **POST 방식 요청 시에만** 삽입된다.

**응답(Response) 메시지 구조**
- 상태 라인: 프로토콜과 상태코드 (예: `HTTP/1.1 200 OK`)
- 메시지 헤더: `Server`, `Content-type`, `Content-length` 등
- 공백 라인
- 메시지 몸체: 실제 전송할 HTML 등의 데이터

[[HTTP 상태 코드]]

## 2. 간단한 웹 서버 구현
- **전체 동작 흐름 (멀티스레드 기반)** 서버는 `accept`로 연결을 받을 때마다 `_beginthreadex`로 스레드(`RequestHandler`)를 생성해 클라이언트 요청을 병렬 처리한다.

- 핵심 함수 4가지로 구성된다.

### 2.1. RequestHandler (요청 분석)`
1. `recv`로 요청 메시지 수신
2. 문자열에 `"HTTP/"`가 없으면 잘못된 요청 → 오류 처리
3. `strtok`으로 요청 라인을 파싱해 method가 `"GET"`인지 확인 (이 예제는 GET만 지원)
4. 요청 파일 이름을 추출하고 Content-type을 확인한 뒤 `SendData` 호출

### 2.2. ContentType (콘텐츠 타입 판별)
- 파일 이름에서 확장자를 분리해, `html`/`htm`이면 `text/html`, 그 외에는 `text/plain`을 반환한다.

### 2.3. SendData (정상 응답)
- 응답 헤더(`HTTP/1.0 200 OK`, `Server`, `Content-length`, `Content-type`)를 차례로 전송하고, 요청한 파일을 열어 `fgets`로 한 줄씩 읽어 `send`로 전송한다.
- 파일 열기에 실패하면 오류 메시지를 보낸다.
- 응답 후 `closesocket`으로 연결을 종료한다. (HTTP 특성)

### 2.4. SendErrorMSG (오류 응답)
- `HTTP/1.0 400 Bad Request` 헤더와 함께 오류 안내 HTML을 전송한 뒤 연결을 종료한다.

## 3. 리눅스 기반 구현
- 윈도우 기반 웹 서버를 단순히 리눅스 환경으로 옮긴 것으로, 입출력 과정에서 **표준 입출력 함수**를 사용하도록 바꾼 것이 차이점이다.
