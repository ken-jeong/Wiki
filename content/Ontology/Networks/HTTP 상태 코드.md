---
aliases:
  - HTTP Status Codes
type: Ontology
tags:
  - Networks
draft: false
up:
  - "[[HTTP 응답]]"
prev:
  - "[[HTTP 버전]]"
same:
next:
down:
---
- 클라이언트(웹 브라우저 등)가 보낸 요청에 대해 **서버가 어떻게 처리했는지**를 알려주는 3자리 숫자 코드
- 요청의 성공/실패 여부를 판단한다.
- 서버에서 클라이언트로 전송된 메시지 **첫 라인**에 기술된다.

## Standard Codes
### 1xx 정보 (Informational)
- `100 Continue`: 요청의 시작 부분이 받아들여졌으니 작업을 계속 진행하라.

### 2xx 성공 (Successful)
- `200 OK`: 요청 성공, 요청 객체는 이 메시지에 포함되어 전송됨
- **201 Created**: 요청이 성공하여 새로운 리소스가 생성되었다.
- **204 No Content**: 요청은 성공했으나, 응답으로 보낼 데이터(본문)가 없다.

### 3xx 리다이렉션 (Redirection)
- `301 Moved Permanently`: 요청 객체가 이동됨, 새 위치는 메시지의 `Location:` 참조

- **301 Moved Permanently**: 요청한 리소스의 URL이 영구적으로 변경되었다.
- **302 Found**: 요청한 리소스의 URL이 임시로 변경되었다.
- **304 Not Modified**: 클라이언트가 가진 캐시가 유효하므로, 서버에서 데이터를 새로 다운로드하지 않고 캐시된 자원을 재사용하면 된다.

### 4xx 클라이언트 에러 (Client Error)
- `400 Bad Request`: 요청 메시지를 서버가 이해할 수 없음
- `404 Not Found`: 요청 문서를 서버에서 찾을 수 없음

- **400 Bad Request**: 요청 자체가 잘못되어 서버가 이해할 수 없다.
- **401 Unauthorized**: 해당 리소스에 접근하기 위한 인증(로그인 등)이 필요하거나 실패했다.
- **403 Forbidden**: 서버가 클라이언트의 신원을 알고 있지만, 해당 리소스에 대한 접근 권한이 없다.
- **404 Not Found**: 요청한 리소스를 서버에서 찾을 수 없다.

### 5xx 서버 에러 (Server Error)
- `505 HTTP Version Not Supported`: 요청된 HTTP 버전을 서버가 지원하지 않음

- **500 Internal Server Error**: 서버 내부의 알 수 없는 오류가 발생했다.
- **502 Bad Gateway**: 게이트웨이나 프록시 서버가 상위 서버로부터 잘못된 응답을 받았다.
- **503 Service Unavailable**: 서버가 일시적인 과부하 또는 점검으로 인해 현재 요청을 처리할 수 없다.
