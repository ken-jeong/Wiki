---
aliases: []
type: Lecture
tags:
  - 2-2/오픈소스SW기초
draft: false
date: 2025-10-20
---
## 1. 코드의 변화
- **코드 이력 관리**: 깃(Git)은 개발 중인 코드의 변경 이력을 관리하는 버전 관리 시스템

- **커밋**(Commit)
	-  ‘~를 적어 두다’라는 의미와 유사하다.
	- 깃에서 코드의 변경 사항을 기록하는 작업을 의미한다.
	- 의미 있는 변경 작업들을 저장소에 기록하는 동작이다.

- **버전 관리의 필요성**
	- 개발 과정에서 코드는 수없이 수정된다. 변경 시점을 저장해두면 문제가 발생했을 때 특정 시점으로 되돌아갈 수 있다.
	- 깃은 변경 내용과 시점 등의 이력을 커밋으로 기록하여 사용자가 쉽게 이전 시점으로 되돌아갈 수 있도록 돕는다.

- **파일 관리 방식의 차이**
	- **파일 복사**: 파일을 직접 복사하여 버전을 관리하면 중복되는 내용으로 인해 파일 용량이 커지고 관리할 파일이 많아지는 단점이 있다.
	- **깃의 커밋**: 깃은 변경된 부분만 추출하여 저장(스냅샷)하므로 동일한 파일 이름으로 효율적인 관리가 가능하다.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIrN0JYKWKFj-HBhWgZTbZOqFlL4Fca8Qyre0-NuaFjG0pkA-UomEj5-l8SDz1DNpeowNQaUxAWfmdIhywxeC8PriHzvw6TZpsgJ0A1htO0JVUIO2MBV2MhPJy-J1OKMFq8Fr-/s1600/git-transport.png)

## 2. 새 파일 생성 및 감지
- **새 파일 생성**: 작업 디렉터리(Working Directory)에 새로운 파일을 생성하면, 깃은 이를 감지한다.
- **상태 확인**(`git status`): `git status` 명령어를 사용하면 깃의 현재 상태를 확인할 수 있다. 새로 생성된 파일은 추적되지 않은 파일(Untracked files)로 표시된다.
```sh
git status
```

## 3. 깃에 새 파일 등록
- **스테이지(Stage) 영역**: 깃은 커밋할 파일들을 등록하는 스테이지 영역을 가지고 있다. 워킹 디렉터리의 파일을 바로 커밋하는 것이 아니라, 스테이지에 등록된 파일만 커밋 대상이 된다.

- **파일 등록**(`git add`): `git add` 명령어를 사용하여 워킹 디렉터리의 파일을 스테이지 영역에 등록(추가)한다. 이 과정을 통해 파일은 추적 가능한(Tracked) 상태로 변경된다.
```sh
git add 파일이름
git add . # 현재 디렉터리의 모든 변경 사항
```

- **등록 취소**(`git rm --cached`): 스테이지에 등록된 파일을 다시 내리려면(Unstage) `git rm --cached 파일이름` 명령어를 사용한다. (커밋하지 않은 경우)
```sh
git rm --cached 파일이름
```

- **파일 이름 변경**(`git mv`): `git mv 기존이름 새이름` 명령어를 사용하여 깃에서 추적 중인 파일의 이름을 변경할 수 있다.
```sh
git mv index.html home.html

# 내부적으로는 다음 명령이 실행되어 이름 변경을 추적한다.
mv index.html home.html
git rm index.html
git add home.html
```

## 4. 첫 번째 커밋
- [Git의 동작원리](https://wonderwalls.tistory.com/entry/Git의-동작원리)

### 4.1. HEAD
- **HEAD**
	- 현재 브랜치의 **가장 마지막 커밋**을 가리키는 포인터
	- 새로운 커밋이 생성될 때마다 HEAD는 해당 커밋으로 이동한다.
	- *최소한 한 번 이상 커밋을 해야만 HEAD가 생성 후 존재한다.*

### 4.2. Snapshot
- **스냅샷**(Snapshot): 깃은 파일을 복사하는 대신, 변경된 부분을 스냅샷 형태로 저장하여 용량을 효율적으로 사용하고 버전 간의 차이를 빠르게 처리한다.

- delta-based version control
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fc3jvxy%2FbtsIzopkm9p%2FAAAAAAAAAAAAAAAAAAAAAD8W13Y6zP7ioB26k-DefBsv9aHt9_n2R85Lm9XfhMmZ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DqzgHfCy0yUmrrgqruPKwllYT93k%253D)

- stream of snapshots
![|500](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbq8vMn%2FbtsIyCohWTl%2FAAAAAAAAAAAAAAAAAAAAAAlxJR6GrQMhGI4wVHHa36xoHvK9zbxIu3ueqxp30P23%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3D1bTk5%252Frll2YrKvynoXi9GOujVxE%253D)

### 4.3. Commit
- 깃은 스테이지 영역의 변경된 내용을 기준으로 스냅샷을 만들어 커밋
- 워킹 디렉터리가 깨끗하게 정리되어 있지 않으면 커밋 명령어를 수행할 수 없음
	- 수정된 내용이 스테이지 영역으로 등록되지 않으면 커밋을 할 수 없음
	- 커밋을 하려면 스테이지 영역에 새로운 변경 내용이 있어야 함
	- 커밋은 수정된 내용을 한 번만 등록함

- **커밋 명령어**(`git commit`)
	- 스테이지 영역에 등록된 파일들을 `git commit` 명령어를 통해 저장소에 영구적으로 기록한다.

- **커밋 메시지**
	- 커밋 시에는 변경 사항을 설명하는 메시지를 반드시 작성해야 한다.
	- `git commit`만 입력하면 텍스트 편집기가 열린다.
	- `-m "메시지"` 옵션을 사용하여 한 줄 메시지를 바로 작성할 수 있다.

- **파일 등록과 커밋 동시 실행**(`git commit -a`)
	- `git commit -a -m "메시지"` 와 같이 `-a` 옵션을 사용하면, 추적 중인 파일의 수정사항을 스테이징과 커밋을 동시에 처리할 수 있다. (단, 새로 생성된 파일은 `git add`를 먼저 해야 한다.)

## 5. 커밋 확인
- **상태 확인**(`git status`): 커밋이 완료되면 워킹 디렉터리는 깨끗한 상태(working tree clean)가 되며, 스테이지 영역은 초기화된다.

- **로그 확인**(`git log`): `git log` 명령어를 사용하면 커밋 이력을 시간 순서대로 확인할 수 있다. 각 커밋은 고유한 커밋 아이디, 작성자, 날짜, 커밋 메시지를 포함한다.

## 6. 두 번째 커밋
- **파일 수정**: 기존 파일을 수정한 후 `git status`로 확인하면 'modified' 상태로 표시된다.
- **수정된 파일 되돌리기**(`git checkout`): `git checkout -- 파일이름` 명령어를 사용하면 수정된 파일을 이전 커밋 상태로 되돌릴 수 있다.
- **커밋 과정**: 수정된 파일은 다시 `git add` 명령어로 스테이지에 등록한 후, `git commit`으로 커밋해야 한다.

## 7. 메시지가 없는 빈 커밋
- **빈 커밋**(Empty Commit): 특별한 경우, `--allow-empty-message` 옵션을 사용하여 메시지 없이 커밋을 생성할 수 있다.
```sh
git commit --allow-empty-message -m "" # 메시지가 없는 빈 커밋
```

## 8. 커밋 아이디
- **커밋 아이디**(Commit ID)
	- 각 커밋을 식별하는 고유한 값으로, SHA-1 해시 알고리즘을 통해 생성된 40글자의 16진수 값이다.
	- cf. [SHA-1 해시 함수 테스트](https://ko.rakko.tools/tools/10)
	- 콘텐츠를 추적하고 분산형 저장 관리 환경에서 충돌을 방지한다.

- **단축키**
	- 전체 40자리 아이디 대신, 앞의 7자리 정도의 짧은 아이디만으로도 커밋을 구분할 수 있다.

```sh
# 커밋ID 확인
❯ git rev-parse master
a70d8c5f4d9ab7d2bbbf844c7c0547779baf79b0
```

## 9. 커밋 로그
- **로그 확인**(`git log`): 커밋 메시지, 아이디, 브랜치 경로 등 다양한 정보를 확인할 수 있다.
```sh
git log 파일이름 # 특정 파일의 커밋 이력만 확인
git show 커밋ID # 특정 커밋의 상세 정보

git log -p # diff를 포함하여 출력
git log --stat # 히스토리 출력
git log --pretty=short # 시간 정보 생략
git log --pretty=oneline # 각 커밋을 한 줄로 표시
```

## 10. diff 명령어
- **변경 내용 확인**(`git diff`): 파일의 변경된 내용을 비교하여 보여주는 명령어
```sh
git diff # 워킹 디렉터리와 스테이지 영역 간의 차이를 비교
git diff HEAD # 워킹 디렉터리와 마지막 커밋(HEAD) 간의 차이를 비교
git commit -v # diff 내용을 추가하여 커밋
```

- *cf. 유닉스 계열의 운영 체제의 diff 명령어*
```sh
diff -u # unified context
```

## 11. 정리
- **커밋의 중요성**: 커밋은 깃에서 소스 코드를 관리하는 첫 단계이자 핵심 기능이다.
- **좋은 커밋 습관**: 너무 많은 코드를 한 번에 수정하고 커밋하기보다는, 작은 기능 단위로 나누어 자주 커밋하는 것이 좋다. 이렇게 하면 변경 사항을 검토하기 쉽고 오류를 빠르게 찾을 수 있다.

## Dev.
### cf. 운영체제별 줄바꿈 문자
|    운영체제     |                  줄바꿈 문자 조합                  | ASCII 코드 |
| :---------: | :-----------------------------------------: | :------: |
| **Windows** | **CR**(Carriage Return) + **LF**(Line Feed) |  13 10   |
|  **Unix**   |           **CR**(Carriage Return)           |    13    |
|  **macOS**  |              **LF**(Line Feed)              |    10    |

### cf. Commit Options
- `git commit` 명령어의 주요 옵션
	- `-m`(`--message`): 커밋 메시지를 별도의 편집기 없이 인라인으로 바로 작성한다.
	- `-a`(`--all`): 수정되거나 삭제된 파일들을 자동으로 스테이징 영역에 추가(add)하고 커밋한다.
	- `-am`: `-a`와 `-m` 옵션을 함께 사용한다.
	- `--amend`: 가장 최근의 커밋을 수정한다. (메시지를 수정하거나, 빠진 파일을 추가하여 커밋 내용을 덮어쓴다.)
	- `-v`(`--verbose`): 커밋 메시지 편집기를 열 때 변경 내용을 diff 형식으로 함께 보여준다.

- 다른 예
```sh
GIT_AUTHOR_DATE="2025-05-28 12:00:00" GIT_COMMITTER_DATE="2025-05-28 12:00:00" git commit --allow-empty -m "Initial commit"

GIT_AUTHOR_DATE="2025-05-30 12:00:00" GIT_COMMITTER_DATE="2025-05-30 12:00:00" git commit -m "docs: add README.md"
```

### cf. Commit Convention Types
- *Write a Good Commit Message*
	- 협업을 위해 중요함
	- 정보를 전달할 수 있도록, 간결하게
	- 써야할 내용이 정리가 잘 안된다면, 여러 건의 변경이나 버그 픽스가 섞여 있는 것임

- **Commit Convention Types**
	- 제목의 가장 앞에 오는 키워드로, 어떤 작업을 했는지 명시한다.

|      태그      |                   설명                   |
| :----------: | :------------------------------------: |
|   **feat**   |          새로운 기능 추가 (Feature)           |
|   **fix**    |              버그 수정 (Fix)               |
|   **docs**   |        문서 수정 (README.md, 주석 등)         |
|  **style**   |      코드 포맷팅, 세미콜론 누락 등 (코드 변경 없음)      |
| **refactor** |        기능 변경 없는 코드 구조 개선 (리팩토링)        |
|   **test**   |            테스트 코드 추가 또는 수정             |
|  **chore**   | 빌드 설정 수정, 패키지 매니저 설정 등 (프로덕션 코드 변경 없음) |
|   **perf**   |                 성능 개선                  |
|    **ci**    |              CI 설정 파일 수정               |
