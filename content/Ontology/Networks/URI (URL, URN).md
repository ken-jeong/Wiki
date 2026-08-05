---
aliases:
type: Ontology
tags:
  - Networks
draft: false
up:
  - "[[HTTP 요청]]"
prev:
  - "[[HTTP 메서드]]"
same:
next:
  - "[[HTTP 버전]]"
down:
---
## 1. URI
> (Uniform Resource Identifier, 통합 자원 식별자)

인터넷에 있는 자원을 식별하기 위한 고유한 문자열 규약이다.
- 자원의 위치(Locator)뿐만 아니라 이름(Name)이나 다른 방식으로 자원을 특정하는 모든 것을 통틀어 URI라고 부른다.

### 1.1. URL
> (Uniform Resource Locator, 통합 자원 위치)

자원이 "어디에 위치해 있는가"와 "어떤 프로토콜로 접근해야 하는가"를 나타내는 식별자이다.

| 구성 요소                  | 예                     |
| ---------------------- | --------------------- |
| **Scheme/Protocol**        | `https://`            |
| **Host/Domain Name**       | `www.example.com`     |
| **Port**                   | `:443`                |
| **Path**                   | `/products/books`     |
| **Query String/Parameter** | `?category=it&page=1` |
| **Fragment/Anchor/Hash**   | `#reviews`            |

### 1.2. URN
> (Uniform Resource Name, 통합 자원 이름)

자원의 위치(URL)가 바뀌어도 영구적으로 자원을 식별할 수 있도록 부여한 고유한 이름이다.

- 예: 책의 국제 표준 도서 번호(ISBN)
	- urn:isbn:9788956746425 (어느 서버에 파일이 있는지와 상관없이 자원 자체를 식별)

## 예시로 보는 차이
| 예시                                 | URL 여부      | URI 여부 | 설명                                                                         |
| ---------------------------------- | ----------- | ------ | -------------------------------------------------------------------------- |
| https://example.com/index.html     | **O**       | **O**  | 프로토콜과 위치가 명시되어 있으므로 URL이자 URI이다.                                           |
| https://example.com/search?q=apple | **△ / O**   | **O**  | 쿼리 스트링(?q=apple)을 포함해 특정 결과 자원을 식별하므로 URI이다. (보통 웹 주소 전체도 넓은 의미의 URL로 통용됨) |
| https://example.com/doc#section2   | **X** (끝부분) | **O**  | #section2(프래그먼트)는 페이지 내 특정 위치를 가리키는 식별자이므로 전체 문자열은 URI에 해당한다.              |
| urn:isbn:0451450523                | **X**       | **O**  | 어디서 다운로드받는지(위치/프로토콜)는 없지만 특정 책을 식별하므로 URN이자 URI이다.                         |
| mailto:user@example.com            | **X**       | **O**  | 이메일 주소 자원을 식별하지만 웹상 파일 위치를 나타내지 않으므로 URI이다.                                |
