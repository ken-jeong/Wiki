---
aliases: []
type: Lecture
tags:
  - 2-2/오픈소스SW기초
draft: false
date: 2025-11-10
---
## 1. 새로운 작업
- **브랜치**(branch)
	- 기존의 코드 흐름에서 벗어나 독립적인 작업 공간을 만드는 기능
	- 나무줄기에서 가지가 뻗어 나가는 것과 유사
	- 원래의 코드(master 브랜치)에 영향을 주지 않고 새로운 기능을 추가하거나 버그를 수정

- **깃 브랜치 특징**
	- 가상 폴더: 깃의 브랜치는 작업 폴더를 실제로 복사하지 않고, 가상 폴더로 생성한다.
	- 독립적인 동작: 브랜치를 이용하면 원본 폴더와 분리하여 독립적으로 개발 작업을 수행할 수 있다.
	- 빠른 동작: 포인터와 유사한 Blob(Binary large object) 개념을 도입하여 내부를 구조화

## 2. 실습 준비
- **기본 브랜치**(master 브랜치)
	- Git은 저장소를 처음 만들 때 master라는 이름의 기본 브랜치를 자동으로 생성
	- *최초 커밋 필요?*

```sh
git branch -M master main # master 브랜치 이름 변경
```

- **저장소 생성 및 초기화**
	- 브랜치 실습을 위해 새로운 폴더를 만들고 `git init` 명령어로 Git 저장소를 초기화하여 실습 환경을 구축한다.

## 3. 브랜치 생성 (branch)
- **생성 방법**
	- `git branch [브랜치 이름]` 명령어를 사용하여 새로운 브랜치를 만든다.
```sh
git branch 브랜치이름 시작위치
# 시작위치: 생략(=HEAD) / Commit ID / Tag Name
```

- **브랜치의 역할**
	- 브랜치는 **특정 커밋을 가리키는 포인터 역할**을 하며, 이를 통해 여러 개발 흐름을 동시에 관리할 수 있다.

- **이름 지정 규칙**
	- 브랜치 이름은 작업 내용과 연관 지어 정하는 것이 좋으며, 일반적으로 'git flow'와 같은 방법론에서 제시하는 이름 규칙을 따르기도 한다.
	- (예: feature, develop, release, hotfix)

### cf. Git Flow 워크플로우 전략
- 네이밍 규칙
	- **소문자** 사용
	- 단어 사이는 **하이픈**(-)으로 연결 (Snake_case보다는 Kebab-case 권장)
	- **슬래시**(/)로 계층 구조 표현

- [Git Flow](https://git.jiny.dev)
	- 메인 브랜치 master → 보조 브랜치 hotfix
	- 메인 브랜치 develop → 보조 브랜치 feature, release

## 4. 브랜치 확인
```sh
# 기본 목록 확인
❯ git branch # 로컬 브랜치 목록 (-l, --list)
  feature
* master

❯ git branch -r # 리모트 브랜치 목록 (--remotes)
  remotes/origin/function
  remotes/origin/master

❯ git branch -a # 모든 브랜치 목록 (--all)
  feature
* master
  remotes/origin/function
  remotes/origin/master
```

```sh
# 상세 정보 확인 (--verbose)
❯ git branch -v # 체크아웃(*) 브랜치명 커밋ID 커밋명
  feature fc152c5 function working
* master  a70d8c5 master working...

❯ git branch -vv # -v에 원격 업스트림 브랜치 정보 포함
  feature fc152c5 [origin/function] function working
* master  a70d8c5 [origin/master] master working...
```

## 5. 브랜치 이동 (checkout)
- **작업 공간의 변화**
	- 브랜치를 이동하면 워킹 디렉터리의 파일 내용이 해당 브랜치의 최신 커밋 상태로 변경된다.

```sh
# 작업 브랜치 변경 전 현재 브랜치를 정리(커밋 완료)한다.

git checkout [브랜치 이름] # 작업 브랜치 이동

git checkout - # 이전 브랜치로 이동
git checkout HEAD~1 # 한 단계 전

git checkout -b 브랜치이름 # 생성+이동 옵션
```

### cf. stash
```sh
git stash # 스태시
git stash save # 여러 스태시
git stash save "m" # 메시지 추가

git stash list # 스택 목록
git stash show # 차이점
git stash show -p # 상세 차이점

git stash pop # 스태시 불러오기
git stash apply # 스태시 복사
git stash drop # 스태시 삭제

git clean # 워킹 디렉터리 청소
```

- 작업 브랜치를 변경하려면 워킹 디렉터리는 깨끗한(clean) 상태로 정리되어 있어야 한다.
- 워킹 디렉터리에 작업 중인 내용이나 커밋되지 않은 변경 사항들이 남아 있으면 브랜치를 변경할 수 없다.
- 현재 수정 작업을 멈추고, 다른 브랜치에 있는 코드를 수정하려면 스태시(stash) 기능을 사용할 수 있다.

## 6. 브랜치 공간
- **독립된 작업 환경**
	- 각 브랜치는 독립적인 작업 공간을 가지므로, 한 브랜치에서의 수정 사항이 다른 브랜치에 즉시 영향을 주지 않는다.

- **로그 확인**
	- `git log` 명령어를 `--graph` 옵션과 함께 사용하면 브랜치의 분기 및 병합 과정을 시각적으로 확인할 수 있다.
```sh
git log --graph --all
```

## 7. HEAD 포인터
|	 용어	 |	상태 비유	|		 의미		  |   필요한 행동   |
| :--------: | :---------: | :-----------------: | :--------: |
|  **HEAD**  |  **현 위치**   |   지금 작업 중인 커밋/브랜치   |	 -	  |
| **AHEAD**  | **업로드 필요**  |  내가 짠 코드가 서버보다 최신   | `git push` |
| **BEHIND** | **다운로드 필요** | 서버에 새 코드가 있고 나는 구버전 | `git pull` |

### 7.1. HEAD (현재 위치)
- **정의**: **지금 내가 보고 있는 커밋**(버전)을 가리키는 포인터
	- 쉽게 말해 지도 위의 현 위치 핀과 같다.

- **특징**
	- 현재 작업 중인 `브랜치`의 가장 최신 커밋을 가리킨다.
	- `checkout` 명령어로 다른 브랜치나 커밋으로 이동하면, HEAD도 그곳으로 이동힌다.
	- HEAD가 가리키는 곳이 곧 내 컴퓨터 파일 탐색기에 보이는 파일들의 상태이다.

### 7.2. AHEAD (앞서 있음)
- **상황**: 내 컴퓨터(Local)에는 새로운 커밋이 있는데, 원격 저장소(Remote)에는 아직 그 커밋이 없을 때

- **해결**: `git push`를 해서 내 작업을 서버에 올려야 한다.
	- *예: 내 로컬은 10페이지까지 썼는데, 서버에는 5페이지까지만 저장된 상태*

### 7.3. BEHIND (뒤쳐져 있음)
- **상황**: 원격 저장소(Remote)에는 새로운 커밋이 있는데, 내 컴퓨터(Local)에는 아직 없을 때

- **해결**: `git pull`을 해서 서버의 최신 변경 사항을 내 컴퓨터로 가져와야 한다.
	- *예: 팀원이 15페이지까지 써서 서버에 올렸는데, 내 컴퓨터는 아직 10페이지인 상태*

## 8. 생성과 이동
- **동시 실행**: 새로운 브랜치를 생성함과 동시에 해당 브랜치로 바로 이동한다.
```sh
git checkout -b [새 브랜치 이름]
```

## 9. 원격 브랜치
- **로컬 브랜치**(Local Branch): 로컬 저장소에 있는 브랜치
	- **트래킹 브랜치**(Tracking Branch): 원격 브랜치와 직접적인 연결 정보가 있는 로컬 브랜치

- **원격 브랜치**(Remote Branch): 원격 저장소에 있는 브랜치
	- **업스트림 브랜치**(Upstream Branch): 트래킹 브랜치가 추적하는 대상인 원격 브랜치

```sh
git remote add origin 주소 # 원격 저장소 등록
git remote show origin # 등록된 원격 저장소 확인
ls .git/refs/ # 원격 브랜치 정보

git push -u origin master # 브랜치 업로드
git push -u origin hotfix # 브랜치 업로드
git push -u origin feature:function # 다른 이름으로 브랜치 업로드

git checkout --track origin/브랜치이름 # 트래킹 브랜치 생성
git branch -u origin/브랜치이름 # 업스트림 브랜치 연결
```

## 10. 브랜치 전송
- `git push`: 로컬 저장소에서 만든 브랜치와 커밋 내역을 원격 저장소로 전송(업로드)
- `git fetch`: 원격 저장소의 최신 브랜치 정보를 로컬로 가져오지만, 병합은 하지 않는다.

## 11. 브랜치 삭제
```sh
git branch -d [브랜치 이름] # 로컬 저장소의 브랜치를 삭제한다.
git branch -D [브랜치 이름] # 병합되지 않은 변경 사항이 있는 경우 강제로 삭제한다.

git push origin --delete [브랜치 이름] # 원격 저장소의 브랜치를 삭제한다.
```

## 12. 정리
- 브랜치는 기존 코드를 안전하게 유지하면서 새로운 기능을 개발하거나 수정 작업을 할 수 있는 독립된 공간을 제공
- 이를 통해 여러 작업을 동시에 진행할 수 있으며, 다른 개발자와의 효율적인 협업 가능

## Dev.
- [6장 실습 예제](https://github.com/jinygit/gitstudy06)
- [git-branch](https://jyheo.github.io/github-lecture/git-branch.html)

### cf. 데이터 저장 단위
- **Blob**(Binary Large Object)

1. 무엇을 저장하나?
	- **파일의 내용**(Content)만 저장힌다.
	- 파일명, 디렉토리 위치, 생성 날짜, 파일 권한 같은 **메타데이터는 저장하지 않는다.** (이런 정보는 Tree라는 다른 객체가 관리한다.)

2. 어떻게 식별하나?
	- 파일의 내용을 바탕으로 생성된 **SHA-1 해시값**을 이름(ID)으로 사용한다.
	- 즉, **내용이 같으면 무조건 같은 Blob**으로 취급한다.

3. 주요 특징 (효율성)
	- **중복 저장 방지**: 만약 A.txt와 B.txt의 파일 이름은 다르지만 **내용이 완전히 똑같다면**, Git은 이 내용을 담은 **Blob을 딱 하나만 만들어서 공유**한다.
	- **불변성**: 한 번 만들어진 Blob은 수정되지 않는다. 파일 내용이 바뀌면 새로운 Blob이 생성된다.

### cf. Git 명령어 구분
- **Plumbing** (저수준 명령어)
	- Git의 **내부 동작을 처리하는 핵심 명령어**
	- 주로 Git 자체의 코어나 스크립트가 사용하며, 일반 사용자가 직접 쓸 일은 거의 없다.

- **Porcelain** (고수준 명령어)
	- 일반적인 **사용자를 위한 명령어**
	- Git을 사용할 때 입력하는 대부분의 명령어가 여기에 속한다.
