---
aliases: []
type: Lecture
tags:
  - 2-2/NAVER-Cloud
draft: false
date: 2025-11-29
---
## 1. API 및 REST API
### 1.1. API(Application Programming Interface)
- **정의**
	- 소프트웨어 간의 상호작용을 가능하게 하는 인터페이스로,
	- 시스템 간 통신을 간편하고 효율적으로 만든다.

#### (1) 주요 종류
- **REST API**
	- HTTP 기반, 리소스 지향적 설계
	- 현대 웹에서 가장 많이 사용됨
- **SOAP API**
	- XML 기반 프로토콜
	- 주로 복잡한 트랜잭션 처리에 사용됨
- **GraphQL**
	- 클라이언트가 필요한 데이터만 명확히 요청할 수 있는 쿼리 언어
	- (데이터 서브셋 선택 가능)

### 1.2. REST API의 기본 구조
- **기반 프로토콜**: HTTP 프로토콜을 기반으로 동작한다.
- **주요 메서드**: GET, POST, PUT, DELETE
- **데이터 포맷**: 주로 **JSON**(JavaScript Object Notation) 포맷을 사용하여 데이터를 주고받는다.

- **구성 요소**
	- **엔드포인트**(Endpoint): 특정 리소스에 접근하기 위한 경로(URL)
	- **리소스**(Resource): 데이터나 서비스를 의미

### 1.3. REST의 6가지 원칙
1. **무상태성**(Statelessness): 서버는 요청 간 상태를 기억하지 않으며, 각 요청은 독립적임
2. **클라이언트-서버 구조**: 두 영역이 명확히 분리되어 독립적으로 동작
3. **캐시 가능성**: 리소스를 캐시 하여 성능 향상 가능
4. **일관된 인터페이스**: URL을 통해 리소스에 접근하는 등 일관된 방식 제공
5. **계층적 시스템**: 클라이언트가 여러 서버를 중계하여 데이터를 받을 수 있음
6. **코드 온 디맨드**(선택적): 서버가 필요에 따라 클라이언트로 코드를 전송 가능

### 1.4. 요청과 응답(Request & Response)
- **헤더**(Header): 메타데이터 포함
		- 요청 형식 (Content-Type)
		- 인증 정보 (Authorization)
		- 사용자 에이전트 등

- **바디**(Body)
	- 실제 전송 데이터 포함
	- 주로 POST, PUT 요청 시 사용되며 JSON/XML 포맷을 따름

- [[HTTP 상태 코드]]

### 1.6. API 인증 방식
- **기본 인증** (API Key)
	- 고유한 문자열(Key)을 사용하여 신원을 증명하는 단순하고 직관적인 방식

- **OAuth2**
	- 현대 웹/모바일에서 널리 쓰이는 진보된 토큰 기반 인증 방식

- **JWT**(JSON Web Token)
	- 정보를 인코딩하여 안전하게 주고받는 토큰 기반 인증
	- 사용자나 시스템 식별 및 보안성 강화에 사용됨

## 2. 실습 환경 구축 (Windows Server)
실습을 진행하기 위한 클라우드 서버 환경을 구성하는 단계이다.

- **서버 생성**
	- NCloud Console에서 Windows Server (High-CPU, 2vCPU, 4GB RAM, 50GB HDD) 생성

- **설정**
	- ACG(방화벽) 설정, 관리자 비밀번호 확인, 공인 IP 접속

- **개발 환경 설치**
	- Windows 방화벽 해제
	- Python 3.11 설치 (PATH 추가 필수)
	- Visual Studio Code (VS Code) 설치 및 Python/Jupyter Extension 설정

## 3. CLOVA AI Services
네이버의 음성 및 시각 지능 서비스를 활용하는 실습이다.

### 3.1. CLOVA Dubbing (동영상 더빙)
- **기능**: 입력한 텍스트를 AI 보이스로 변환하여 동영상에 입히는 서비스
- **실습**: 무료/유료 버전 차이 이해, 프로젝트 그룹 생성, 텍스트 입력 및 보이스 선택 후 영상 다운로드

### 3.2. CLOVA Voice (TTS - Text to Speech)
- **기능**: 텍스트를 자연스러운 음성으로 변환(음성 합성)
- **준비**: Application 등록 (Client ID/Secret 발급)
- **API 실습**
	- 화자(Speaker), 속도, 볼륨, 피치, 감정 등을 코드로 조절
	- `requests` 라이브러리를 통해 mp3/wav 파일 생성
	- **Gradio 연동**: 웹 UI를 통해 텍스트를 입력받고 즉시 음성으로 변환하여 재생하는 앱 구현

### 3.3. CLOVA Speech (STT - Speech to Text)
- **기능**: 긴 음성 인식, 화자 분리, 키워드 부스팅 등
- **준비**: 도메인 생성 및 빌더 실행
- **실습**
	- **콘솔**: 음성 파일 업로드 후 텍스트 변환 결과 확인
	- **API**: Secret Key와 Invoke URL을 사용하여 Python 코드로 음성 파일 전송 및 JSON 결과 파싱
	- **Gradio 연동**: 음성 파일을 업로드하거나 마이크로 녹음하면 텍스트로 변환해주는 웹 앱 구현

### 3.4. CLOVA OCR (광학 문자 인식)
- **기능**: 이미지 속의 텍스트를 감지하고 인식 (General / Template 모드)
- **준비**: API Gateway 연동, Secret Key 생성
- **실습**
	- 이미지(URL 또는 로컬 파일)를 Base64로 인코딩하여 전송
	- **General 모드**: 이미지 전체 텍스트 추출
	- **Template 모드**: 지정된 양식의 특정 필드 값 추출
	- **Gradio 연동**: 이미지를 업로드하면 추출된 텍스트를 보여주는 웹 앱 구현

## 4. Papago Translation Services
네이버의 인공신경망 기반 번역 서비스를 활용하는 실습이다.

### 4.1. Text Translation (텍스트 번역)
- **기능**: 문장 번역, 언어 감지, 높임말 번역, 용어집(Glossary) 활용
- **실습**
	- Application 등록 및 인증키 확보
	- HTML 파일 내 텍스트 추출(`BeautifulSoup`) 후 번역 및 재구성
	- **Gradio 연동**: 원문 텍스트와 언어를 선택하여 번역 결과를 출력하는 웹 앱

### 4.2. Website Translation (웹사이트 번역)
- **기능**: 웹페이지의 구조를 유지하며 내용을 번역
- **실습**: URL을 입력받아 해당 페이지의 HTML 텍스트를 추출하고 번역하여 보여주는 기능 구현

### 4.3. Image Translation (이미지 번역)
- **기능**
	1. **Image to Text**: 이미지 내 텍스트만 추출하여 번역
	2. **Image to Image**: 이미지 내 텍스트를 번역된 텍스트로 시각적으로 대체(AR 번역)
- **실습**
	- `multipart/form-data`로 이미지 전송
	- 결과값으로 받은 Base64 이미지를 디코딩하여 파일로 저장
	- **심화 (Web Capture)**: `Playwright` 라이브러리를 사용하여 특정 URL의 스크린샷을 찍고, 이를 즉시 Papago API로 보내 번역된 이미지를 반환받는 자동화 파이프라인 구축
	- **Gradio 연동**: 이미지 업로드 또는 웹사이트 URL 입력을 통해 번역된 이미지를 확인하는 통합 앱 구현

## 5. 핵심 도구 및 라이브러리
강의 전반에 걸쳐 사용된 주요 도구이다.

- **Gradio**: Python 코드를 손쉽게 웹 인터페이스(UI)로 만들어주는 라이브러리 (데모 앱 제작용)
- **Requests**: HTTP API 호출을 위한 라이브러리
- **BeautifulSoup**: 웹 페이지 HTML 파싱
- **Playwright**: 웹 브라우저 자동화 및 스크린샷 캡처
- **Base64/JSON**: 데이터 인코딩 및 통신 포맷 처리
