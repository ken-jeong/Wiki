---
aliases:
  - HTTP Response
type: Ontology
tags:
  - Networks
draft: false
up:
  - "[[HTTP]]"
prev:
  - "[[HTTP 요청]]"
same:
next:
down:
  - "[[HTTP 버전]]"
  - "[[HTTP 상태 코드]]"
---
## HTTP 응답
서버가 클라이언트의 요청을 받아 처리한 결과를 돌려주는 메시지이다.

### 응답 메시지의 구조
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=UTF-8
Content-Length: 42
Date: Tue, 30 Mar 2026 03:00:00 GMT
(빈 줄)
{
  "id": 1,
  "name": "홍길동"
}
```

1. **상태 라인 (Status Line)**
	- **HTTP 버전**: 응답에 사용된 프로토콜 버전 (예: HTTP/1.1)
	- **HTTP 상태 코드**: 처리 결과를 나타내는 3자리 숫자 (예: 200, 404)
	- **이유 문구** (Reason Phrase): 상태 코드를 사람이 읽기 쉽게 설명한 단어 (예: OK, Not Found)

2. **헤더 (Headers)**
	- 응답에 대한 부가 정보를 전달한다.
	- 예:
		- Content-Type(본문 데이터 형식)
		- Content-Length(본문 크기)
		- Set-Cookie(쿠키 저장 요청)
		- Server(서버 소프트웨어 정보)

3. **공백 라인 (Empty Line)**
	- 헤더와 본문을 구분하는 필수 빈 줄이다.

4. **본문 (Body)**
	- 클라이언트가 요청한 실제 데이터(HTML 파일, JSON 데이터, 이미지 파일 등)가 포함된다.
	- (데이터가 필요 없는 응답에는 비어 있을 수 있음)
