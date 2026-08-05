---
aliases:
  - HTTP Request
type: Ontology
tags:
  - Networks
draft: false
up:
  - "[[HTTP]]"
prev:
same:
next:
  - "[[HTTP 응답]]"
down:
  - "[[HTTP 메서드]]"
  - "[[URI (URL, URN)]]"
  - "[[HTTP 버전]]"
---
## HTTP 요청
클라이언트가 특정 동작(데이터 조회, 등록, 수정, 삭제 등)을 서버에 요구하기 위해 보내는 메시지이다.

### 요청 메시지의 구조
HTTP 메시지는 **요청**과 **응답** 두 종류이며, **ASCII 형식(텍스트 수준)** 이다.

```http
GET /users/1 HTTP/1.1
Host: api.example.com
User-Agent: Mozilla/5.0
Accept: application/json
(빈 줄)
{"message": "GET 방식은 보통 본문(Body)이 비어 있습니다."}
```

1. **시작 라인/요청 라인 (Start Line/Request Line)**
	- **HTTP 메서드**
		- 수행할 동작의 종류
		- (GET, POST, PUT, DELETE 등)
	- **요청 대상(URI / URL Path)**
		- 리소스의 경로
		- (예: /users/1, /search?q=http)
	- **HTTP 버전**
		- 사용하는 프로토콜 버전
		- (예: HTTP/1.1, HTTP/2)

2. **헤더 (Headers)**
	- 요청에 대한 부가 정보(메타데이터)를 Key: Value 형태로 전달한다.
	- 예:
		- Host(요청 대상 도메인)
		- User-Agent(클라이언트 환경 정보)
		- Content-Type(본문 데이터의 타입)
		- Authorization(인증 토큰)

3. **공백 라인 (Empty Line)**
	- 헤더의 끝과 바디(Body)의 시작을 구분하기 위한 필수 빈 줄이다.

4. **본문 (Body)**
	- 서버로 전송할 실제 데이터이다.
	- ==주로 POST, PUT, PATCH 요청 시== JSON, Form 데이터 등을 담아 전송하며, GET, DELETE 요청 시에는 대개 비워둔다.
