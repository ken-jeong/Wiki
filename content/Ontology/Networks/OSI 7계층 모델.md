---
aliases:
  - OSI 7-Layer Model
type: Ontology
tags:
  - Networks
draft: false
up:
prev:
same:
next:
  - "[[TCP-IP|TCP/IP 4계층 모델]]"
down:
---
> Open Systems Interconnection
> 7-Layer Model

## OSI 7계층 모델
- 1984년, 국제표준화기구(ISO)에서 제정한 이론적·개념적 표준 참조 모델이다.
- 네트워크 통신 흐름을 단계별로 가장 세분화하여 설명하기 때문에, 개념 이해와 트러블슈팅의 기준점으로 사용된다.
- 실제 구현된 시스템이 아닌 일종의 개념적 모델이다.

### 상위 계층 (Upper Layers) - Software
#### L7. 응용 계층 (Application Layer)
- 사용자와 직접 맞닿아 네트워크 **서비스**를 제공하는 인터페이스
- (웹 브라우징, 이메일 등)

- *실제로 받은 데이터를 처리*

#### L6. 표현 계층 (Presentation Layer)
- 데이터의 형식 변환(**포맷팅**), **암호화**/복호화, 압축 등을 수행
- JPEG, ASCII, UTF-8, SSL/TLS 등

- *받은 데이터를 해석하는 방법*

#### L5. 세션 계층 (Session Layer)
- 통신 장치 간의 **연결**(세션) 생성, 유지, 종료 및 동기화 담당
- RPC, NetBIOS, 소켓 통신 등

- *통신 장치 간의 연결을 유지할 수 있는 방법*

### 하위 계층 (Lower Layers) - Hardware
#### L4. 전송 계층 (Transport Layer)
- 종단 간(End-to-End) 신뢰성 있는 통신을 보장한다.
	- 데이터를 **세그먼트**(Segment) 단위로 나눈다.
	- TCP의 경우 오류 복구와 흐름 제어를 수행한다.
- 예시
	- **TCP**: 연결 지향, Handshake
	- **UDP**: 비연결 지향
- **포트 번호**(Port Number)로 프로세스를 식별한다.
- *Process to Process*

#### L3. 네트워크 계층 (Network Layer)
- 논리적 주소(**IP 주소**)를 기반으로, 데이터가 목적지까지 찾아가는 최적의 경로(라우터-**라우팅**) 결정
	- **CIDR**
	- **Subnet Mask**
	- **ARP** (Address Resolution Protocol)
- *Host to Host*

네트워크 계층만 사용할 경우 여러 응용이 동시에 통신 불가능

#### L2. 데이터 링크 계층 (Data Link Layer)
- 물리적으로 직접 연결된 인접 장비 간의 신뢰성 있는 전송
- **MAC 주소 (Media Access Control Address)** 사용: 네트워크 인터페이스에 부여된 고유의 주소
	- 같은 LAN의 유니캐스트 동작
	- -> 로컬 네트워크 외부로 통신 불가능
- 에러 탐지 및 흐름 제어 기능을 수행한다.
- **스위치**
- *Node to Node (Hop to Hop)*

#### L1. 물리 계층 (Physical Layer)
- 주요 단위: 0과 1의 **비트**(Bits) 데이터를
- 전기적/광학적 신호로 변환하여,
- **물리 매체**(케이블 등)를 통해 전송한다.
	- + 허브(**브로드캐스트 동작** -> 충돌 발생 및 유니캐스트 불가능), 리피터 등
