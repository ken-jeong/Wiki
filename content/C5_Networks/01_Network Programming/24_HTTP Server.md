---
aliases: []
type: Lecture
tags:
  - 3-1/네트워크프로그래밍
draft: false
date: 2026-06-09
---
## 1. HTTP 개요
**웹 서버의 기능**
- [[HTTP]]을 기반으로 웹 페이지에 해당하는 파일을 클라이언트에게 전송하는 역할을 하는 서버이다.

## 2. 간단한 웹 서버 구현
- 간단한 웹 서버는 GET 요청을 파싱해 해당 파일을 적절한 헤더와 함께 전송한 뒤 연결을 끊는 구조로 만들 수 있다.

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
