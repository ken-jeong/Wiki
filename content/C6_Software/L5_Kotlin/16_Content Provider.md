---
aliases: []
type: Lecture
tags:
  - 3-1/모바일프로그래밍
draft: false
date: 2026-05-22
---
## 1. 콘텐트 프로바이더 개요
- **콘텐트 프로바이더** (Content Provider)
	- **앱의 데이터를 다른 앱과 공유**할 때 사용하는 컴포넌트이다.
	- 안드로이드는 보안상 외부 앱이 다른 앱의 데이터에 직접 접근하는 것을 막기 때문에, 데이터를 공유하려면 이 컴포넌트가 필요하다.
	- 데이터를 공개하는 앱이 콘텐트 프로바이더를 구현해 두면, 외부 앱이 그것을 통해 DB·파일·메모리 데이터에 접근하고 수정할 수 있다.
	- 주소록, 갤러리, 카메라, 지도, 전화 등 구글 기본 앱과 연동할 때 자주 쓰인다.

- 공유 측은 `ContentProvider`를 상속해 구현한다.
- 사용 측은 `ContentResolver` + URI로 접근한다.

### 1.1. 작성 방법 (데이터 제공 측)
- `ContentProvider` 클래스를 상속하고 다음 함수를 재정의한다.
	1. `onCreate()`: 생명주기 함수, 시스템이 객체 생성 시 자동 호출
	2. `query()`, `insert()`, `update()`, `delete()`: 외부 앱이 데이터를 조회·삽입·수정·삭제할 때 호출
	3. `getType()`: MIME 타입 반환

- 또한 안드로이드 컴포넌트이므로
	- 매니페스트에 `<provider>`로 등록해야 하며,
	- `name`(클래스명)뿐 아니라
	- ==authorities 속성(외부 식별용 고유 문자열)도 반드시 선언==해야 한다.

```xml
<provider
	android:name=".MyContentProvider"
	android:authorities="com.example.test_provider"
	android:enabled="true"
	android:exported="true" />
```

### 1.2. 이용 방법 (데이터 사용 측)
- 콘텐트 프로바이더는 인텐트와 무관하며, 시스템이 자동 생성해 주므로 미리 작성된 함수만 호출하면 된다.

- **외부 앱**을 쓰려면
	- 매니페스트의 `<queries>`에 대상 앱의 패키지명(`<package>`)
	- 또는 authorities(`<provider>`)를 명시해 **패키지 공개 설정**을 해야 한다.

```xml
<queries>
	<package android:name="com.example.test_outter" />
</queries>
```

- 실제 **데이터 조작**은
	- **ContentResolver 객체**(`contentResolver` 속성)로 한다.
	- `query()`, `insert()`, `update()`, `delete()` 함수를 제공한다.

```
query(uri, projection, selection, selectionArgs, sortOrder)
insert(uri, values)
update(uri, values, where, selectionArgs)
delete(uri, where, selectionArgs)
```

### 1.3. URI 구조
```
content://com.example.test_provider/user/1
└─프로토콜─┘└───호스트(authorities)───┘└─경로─┘
```

URI 구조
1. `content://`: 프로토콜(scheme)
2. `com.example.test_provider`: 호스트(host) = authorities 값
3. `/user/1`: 경로(path, 선택 사항)

경로로 조건을 지정할 수 있다.
- **단어로 끝나면**(`/user`) 해당 데이터 전체를 의미한다.
- **숫자로 끝나면**(`/user/1`) 그 번호로 식별되는 데이터를 의미한다.
- 단, 실제 프로바이더가 경로를 활용하지 않으면 의미가 없다.

`ContentValues`
- `insert()`/`update()`에 넘기는 Map 형태의 키-값 집합 객체이다.

## 2. 안드로이드 기본 앱과 연동
| 연동 대상 |      액션       |                       데이터(URI)                       |
| :---: | :-----------: | :--------------------------------------------------: |
|  주소록  | `ACTION_PICK` | `ContactsContract.CommonDataKinds.Phone.CONTENT_URI` |
|  갤러리  | `ACTION_PICK` |    `MediaStore.Images.Media.EXTERNAL_CONTENT_URI`    |
|  지도   | `ACTION_VIEW` |                     `geo:위도,경도`                      |
|  전화   | `ACTION_CALL` |                      `tel:전화번호`                      |

### 2.1. 주소록 앱
- **퍼미션 설정**
```xml
<uses-permission
	android:name="android.permission.READ_CONTACTS" />
```

- **연동 절차**
	1. `Intent.ACTION_PICK` + `ContactsContract.CommonDataKinds.Phone.CONTENT_URI`로 목록 화면을 띄움
	2. 사용자가 한 명을 선택하면 결과가 URI 형태로 돌아오며, URI 끝의 숫자가 식별값
	3. 이 식별값(선택된 사람의 URI)을 `contentResolver.query()`에 넣고, `DISPLAY_NAME`·`NUMBER`를 지정해 이름·전화번호를 가져옴

- **주요 상수**
	- `ContactsContract.Contacts.CONTENT_URI`: 모든 사람
	- `ContactsContract.CommonDataKinds.Phone.CONTENT_URI`: 전화번호 보유자
	- `ContactsContract.CommonDataKinds.Email.CONTENT_URI`: 이메일 보유자

### 2.2. 갤러리 앱
- **안드로이드의 이미지표현**
	1. `Drawable`: 주로 리소스용
	2. `Bitmap`: 파일/네트워크 이미지용
		- `BitmapFactory`의 `decode~` 함수(`decodeFile`, `decodeStream`, `decodeResource`, `decodeByteArray`)로 생성
	- 상호 변환 가능

- **OOM(Out Of Memory) 주의**
	- 큰 이미지를 원본 그대로 불러오면 메모리 부족 오류 발생
	- `inJustDecodeBounds = true`로 실제 디코딩 없이 이미지 크기 정보만 얻은 뒤, 화면 출력 크기와 비교해 `inSampleSize` 값을 계산
	- `BitmapFactory.Options`의 `inSampleSize`로 크기를 줄여 해결

```kotlin
val option = BitmapFactory.Options()
option.inSampleSize = 4 // 가로·세로 1/4 → 전체 1/16 크기로 디코딩
val bitmap = BitmapFactory.decodeStream(inputStream, null, option)
```

- **연동 절차**
	1. `Intent.ACTION_PICK`
	2. `MediaStore.Images.Media.EXTERNAL_CONTENT_URI`
	3. `type = "image/*"`
	4. 갤러리 실행 후 선택한 이미지를 축소 비율 적용해 화면에 출력

### 2.3. 지도 앱
- 위도·경도가 있으면 지도 앱으로 위치 표시 가능

- 반드시 `geo:`로 시작, 위도·경도는 쉼표 구분)
```kotlin
val intent = Intent(
	Intent.ACTION_VIEW,
	Uri.parse("geo:37.5662952,126.9779451")
)
startActivity(intent)
```

### 2.4. 전화 앱
- **매니페스트 설정**
```xml
<uses-feature
    android:name="android.hardware.telephony"
    android:required="false" />
<uses-permission
	android:name="android.permission.CALL_PHONE" />
```

- **전화 걸기 기능 실행**
```kotlin
val intent = Intent(
	Intent.ACTION_CALL, Uri.parse("tel:02-120")
)
startActivity(intent)
```
