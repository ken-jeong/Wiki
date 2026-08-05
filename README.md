# Archive

개인 학습 기록과 기술 노트를 연결해 가꾸는 디지털 가든입니다. [Quartz 4](https://quartz.jzhao.xyz/)로 Markdown 문서를 정적 사이트로 빌드하며, 공개된 노트는 [albert-jeong.pages.dev](https://albert-jeong.pages.dev)에서 볼 수 있습니다.

![Archive의 노트 그래프 뷰](assets/graph_view.png)

## 노트 구조

노트는 지식을 주제별로 나누는 **Computing** 분류와 자료의 활용 상태를 나타내는 **PARA** 분류를 함께 사용합니다. 전체 탐색의 시작점은 [`content/index.md`](content/index.md)입니다.

### Computing

| 경로 | 주요 내용 |
| --- | --- |
| [`content/C0_Liberal Arts`](content/C0_Liberal%20Arts) | 기초 교양, 컴퓨터과학 개론, 심리학, 철학 |
| [`content/C1_Mathematics`](content/C1_Mathematics) | 미적분학, 선형대수학, 확률과 통계 |
| [`content/C2_Computation`](content/C2_Computation) | 이산수학, 자료구조, 알고리즘, 최적화 |
| [`content/C3_Hardware`](content/C3_Hardware) | 하드웨어 기초, 논리회로, 컴퓨터구조, 임베디드 시스템 |
| [`content/C4_Systems`](content/C4_Systems) | 유닉스, 데이터베이스, 운영체제, 시스템 프로그래밍과 서버 운영 |
| [`content/C5_Networks`](content/C5_Networks) | 인프라, 네트워크 프로그래밍, 컴퓨터 네트워크, 클라우드 |
| [`content/C6_Software`](content/C6_Software) | 프로그래밍 언어, 프론트엔드, 소프트웨어 공학, 웹 서비스 |
| [`content/C7_Methodologies`](content/C7_Methodologies) | 인공지능, 언어 모델, PyTorch, MLOps |
| [`content/C8_Applied`](content/C8_Applied) | 부트캠프, 커뮤니티, 현장 프로젝트, Applied AI, 보안 |

### PARA

| 경로 | 역할 |
| --- | --- |
| [`content/P1_Project`](content/P1_Project) | 목표와 완료 시점이 있는 현재 프로젝트와 실행 자료 |
| [`content/P2_Area`](content/P2_Area) | 대학, 포트폴리오, 진로처럼 지속적으로 관리할 영역 |
| [`content/P3_Resource`](content/P3_Resource) | 여러 프로젝트에서 참고하거나 재사용할 자료 |
| [`content/P4_Archive`](content/P4_Archive) | 완료되었거나 현재 사용하지 않는 프로젝트와 자료 |

## 사이트 구성

현재 저장소는 Quartz 4.5.1을 기반으로 하며 다음 기능을 사용합니다.

- 한국어 인터페이스와 수정일 기준 날짜 표시
- SPA 탐색, 링크 미리보기, 검색, 탐색기
- 그래프 뷰, 백링크, 목차, 다크 모드, 리더 모드
- Obsidian·GitHub Flavored Markdown, 구문 강조, KaTeX 수식
- 폴더·태그 페이지, 별칭 리디렉션, RSS, 사이트맵, 소셜 미리보기 이미지
- Google Analytics

사이트 주소, 테마, 변환기와 출력 기능은 [`quartz.config.ts`](quartz.config.ts), 화면 구성은 [`quartz.layout.ts`](quartz.layout.ts)에서 관리합니다.

## 로컬 실행

Node.js 22 이상과 npm 10.9.2 이상이 필요합니다. [`.node-version`](.node-version)은 Node.js 22.16.0을 지정합니다.

```bash
npm ci
npx quartz build --serve
```

터미널에 표시되는 로컬 주소로 접속하면 파일 변경 사항이 자동으로 반영됩니다. 배포용 정적 파일은 다음 명령으로 `public/`에 생성합니다.

```bash
npx quartz build
```

타입, 코드 스타일, 테스트는 다음 명령으로 확인합니다.

```bash
npm run check
npm test
```

## 공개 노트 동기화

사이트 콘텐츠는 `content/` 아래의 Markdown 파일입니다. [`copy.sh`](copy.sh)는 기본적으로 iCloud의 Obsidian `Vault`에서 frontmatter에 `draft: false`가 명시된 문서만 골라 원본 폴더 구조대로 가져옵니다.

1. `copy.sh`의 `SRC`가 실제 Obsidian vault 경로를 가리키는지 확인합니다.
2. 현재 `content/`의 필요한 변경 사항을 커밋하거나 백업합니다.
3. 스크립트를 실행한 뒤 로컬 빌드로 링크와 화면을 확인합니다.

```bash
./copy.sh
npx quartz build --serve
```

> [!CAUTION]
> 스크립트는 복사를 시작하기 전에 `content/`의 기존 내용을 `.gitkeep`만 남기고 삭제합니다. 또한 Markdown 파일만 복사하므로 이미지 등 첨부 파일이 필요하면 별도로 옮겨야 합니다.

Quartz의 `RemoveDrafts` 필터도 빌드 단계에서 초안 문서를 제외합니다. 공개 여부를 바꿀 때는 각 노트의 frontmatter를 함께 확인하세요.

## 주요 파일

| 경로 | 역할 |
| --- | --- |
| [`content/`](content) | 공개할 Markdown 노트 |
| [`content/index.md`](content/index.md) | 사이트 홈과 전체 분류 진입점 |
| [`copy.sh`](copy.sh) | Obsidian vault의 공개 노트 동기화 |
| [`quartz.config.ts`](quartz.config.ts) | 사이트 메타데이터, 테마, 플러그인 설정 |
| [`quartz.layout.ts`](quartz.layout.ts) | 콘텐츠·목록 페이지 레이아웃 |
| [`quartz/styles/custom.scss`](quartz/styles/custom.scss) | 사용자 정의 스타일 |
| [`docs/`](docs) | 저장소에 포함된 Quartz 사용 문서 |

## 배포

기본 주소는 `albert-jeong.pages.dev`로 설정되어 있습니다. 배포 환경에서는 빌드 명령으로 `npx quartz build`, 출력 디렉터리로 `public`을 사용합니다. 도메인을 바꾸면 [`quartz.config.ts`](quartz.config.ts)의 `baseUrl`도 함께 수정해야 RSS, 사이트맵과 내부 링크가 올바르게 생성됩니다.

## 라이선스

Quartz 소스 코드는 [`LICENSE.txt`](LICENSE.txt)의 MIT License를 따릅니다. `content/`의 문서는 별도 허가 없이 재배포할 수 없습니다.
