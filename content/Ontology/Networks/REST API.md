---
aliases:
type: Ontology
tags:
  - Networks
draft: false
up:
  - "[[API]]"
prev:
same:
next:
  - "[[GraphQL]]"
down:
---
> [REST API 제대로 알고 사용하기](https://meetup.nhncloud.com/posts/92)

## 1. 개념 및 정의
> REST와 API를 따로 보면 의미가 잘 드러난다.

**REST** (Representational State Transfer)
- ==자원의 이름을 이용해 해당 자원의 상태(State)를 주고받는 것==
- 비유: 특정 방식으로 견적서(자원)를 요청하면, 특정 방식으로 주는 것

**REST API**
- 특정 방식으로 무언가를 주면, 요청한 자원을 특정 방식으로 찾아주는 데이터 송수신 약속

**핵심**
- HTTP 요청(GET, POST 등)을 통해 자원(Resource)을 명시하고 처리 결과를 받음

## 2. Request(요청) 구성 요소
> 클라이언트가 서버로 요청을 보낼 때 고려해야 할 3가지

1. **HTTP Method (행위)**
	- 서버가 수행해야 할 동작 (동사)
	- **GET**: 조회 (Read)
	- **POST**: 생성 (Create)
	- **PUT**: 전체 수정 (Update/Replace)
	- **PATCH**: 일부 수정 (Update/Modify)
	- **DELETE**: 삭제 (Destroy)

2. **URI (Endpoint, 자원)**
	- 자원의 위치 (명사)
	- 메뉴판의 메뉴 역할

3. **Request 객체 (표현)**
	- 자원을 식별하거나 데이터를 보낼 때 사용하는 옵션
	- **Path Variable**
		- 주소에 포함된 변수
		- 특정 자원(ID 등)을 식별할 때 사용
		- (예: `/boards/4`)
	- **Query String**
		- `?` 이후의 변수
		- 정렬, 필터링에 적합
		- (예: `?day=Mon`)
	- **Request Body**
		- 주소에 보이지 않음(JSON 등)
		- 데이터 양이 많거나 생성/수정 시 사용

## 3. RESTful API의 URI 설계 원칙
URI 설계는 REST API의 가독성과 일관성을 좌우한다.

> 소문자 명사(복수형)로 자원의 계층을 슬래시와 하이픈을 써서 표현하고,
> 동작은 HTTP 메서드,
> 검색·정렬은 쿼리 스트링으로 처리한다.

### 3.1. 자원은 '명사'로, 행위는 'HTTP 메서드'로 표현
- URI에는 행위(동사)를 포함하지 않고, 식별하고자 하는 **자원(명사)**만 둔다.
- 행위(조회, 생성, 수정, 삭제)는 `GET`, `POST`, `PUT`, `DELETE` 등의 HTTP 메서드로 나타낸다.
	- ❌ `GET /users/create` / `POST /users/delete`
	- ⭕ `POST /users` / `DELETE /users/1`

### 3.2. 복수형(Plural) 명사 사용 권장
- 자원의 컬렉션을 나타낼 때는 일관성을 위해 단수형보다 **복수형 명사**를 쓰는 것이 표준적이다.
	- ❌ `/user/1`, `/post`
	- ⭕ `/users/1`, `/posts`

### 3.3. 슬래시(`/`)로 계층 관계 표현
- 슬래시는 자원 간의 포함 관계나 계층 구조를 나타낼 때 사용한다.
	- ⭕ `/users/123/orders` (123번 사용자의 주문 목록)
	- ⭕ `/users/123/orders/45` (123번 사용자의 45번 주문)

### 3.4. 마지막 슬래시(Trailing Slash) 사용 금지
- URI 끝에는 슬래시를 붙이지 않는다. 혼란을 주거나 캐싱 시 다른 자원으로 인식될 수 있다.
	- ❌ `/users/`
	- ⭕ `/users`

### 3.5. 소문자 사용
- URI는 대소문자를 구분(Case-sensitive)하는 경우가 많아 혼선을 방지하기 위해 **모두 소문자**로 작성한다.
	- ❌ `/Users/NewOrders`
	- ⭕ `/users/new-orders`

### 3.6. 하이픈(`-`) 사용, 언더스코어(`_`) 지양 (Kebab-case)
- 단어와 단어를 조합할 때는 가독성을 위해 **하이픈(`-`)**을 사용한다. 밑줄(`_`)은 글꼴에 따라 밑줄 친 링크와 겹쳐 보이지 않을 수 있다.
	- ❌ `/user_profiles`
	- ⭕ `/user-profiles`

### 3.7. 파일 확장자 미포함
- URI에 `.json`, `.html` 등의 확장자를 넣지 않는다. 데이터 형식은 HTTP 헤더(`Accept`, `Content-Type`)를 통해 협상한다.
	- ❌ `/users/123.json`
	- ⭕ `/users/123`

### 3.8. 필터링·정렬·페이징은 쿼리 파라미터(`?`) 활용
- 새로운 자원을 가리키는 것이 아니라 자원의 정렬, 검색, 페이징 처리를 할 때는 경로(Path) 대신 쿼리 스트링을 사용한다.
	- ❌ `/users/page/2/sort/name`
	- ⭕ `/users?page=2&sort=name`
	- ⭕ `/products?category=electronics&order=desc`

## 4. Response (응답) 구성 요소
> 서버가 클라이언트에게 답할 때 고려해야 할 것

클라이언트가 요청한 내용에 대해 **어떤 상태로, 어떤 데이터를 돌려줄지**를 일관된 방식으로 설계해야 한다.
적절한 HTTP 상태 코드와 응답 본문 구조를 통해 결과를 명확히 전달하는 것이 핵심이다.

- [[HTTP 상태 코드]]

- **Response Body**
	- 요청한 데이터(JSON 형태 등)

- **Error Message & Code**
	- 클라이언트가 상세한 예외 처리를 할 수 있도록 에러 원인과 메시지를 규격화하여 전달
