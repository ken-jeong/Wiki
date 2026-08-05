---
aliases: []
type: Lecture
tags:
  - 3-1/인공지능
draft: false
date: 2026-03-05
---
> [!note]- Supplementary Materials
> **인공지능**
> - [[01_AI Overview]]
> - [[02_ML Overview]]
> - [[03_ML Optimization]]
> 
> **LG AI연구원 9기** - Supervised Learning
> - [[01_Supervised Learning Overview]]
> - [[02_Linear Regression]]
> - [[03_Gradient Descent]]
> 
> **LG AI연구원 9기** - Tabular ML
> - [[01_Intro to Tabular ML]]

## 1. 정의
### 1.1. 지능
지능을 정의하는 것은 까다롭다.

- **지능**(Intelligence)
	- 단일한 능력이 아니라, 정보를 받아들이고 이를 처리하며 문제를 해결하는 다양한 인지적 기능들의 복합체이다.
	- 심리학과 인지과학에서는 지능을 구성하는 요소를 규명하기 위해 다양한 이론을 제시해 왔다.

#### 지능의 요소
현대 인지과학 및 인공지능(AI) 분야에서 공통적으로 다루는 지능의 핵심 인지적 요소는 다음과 같다.

- **지각 능력 (Perception)**
	- 감각 기관을 통해 외부 정보를 받아들이고 그 의미를 해석하는 능력
	- (시각, 청각 정보 처리 등)

- **주의 집중력 (Attention)**
	- 수많은 자극 중에서 필요한 정보에 선택적으로 초점을 맞추고 이를 유지하는 능력

- **기억 (Memory)**
	- 정보를 뇌에 등록하고(부호화), 저장하며, 필요한 때에 다시 꺼내는(인출) 능력
	- (특히 정보를 일시적으로 보유하며 처리하는 '작업 기억'이 지능과 밀접하다.)

- **추론 및 논리적 사고 (Inference, Logic, Reasoning)**
	- 기존에 알고 있는 사실을 바탕으로 새로운 결론을 이끌어내거나 논리적 오류를 잡아내는 능력

- **문제 해결 및 의사결정 (Problem Solving & Decision Making)**
	- 목표를 달성하기 위해 장애물을 분석하고, 대안을 마련하여 최선의 선택을 내리는 과정

- **학습 능력 (Learning/Adaptability)**
	- 과거의 경험과 피드백을 통해 행동이나 지식을 수정하고 새로운 환경에 적응하는 능력

기타 지능의 요소로 볼 수 있는 것들은 다음과 같다.

- 이해 (Understanding)
- 일반화 (Generalizing)
- 상식 (Common Sense)
- 직관 (Intuition)
- 감정 (Emotion)
- 자기 인식 (Self-awareness)

### 1.2. 인공지능
따라서 인공지능을 정의하는 것 역시 까다롭다.

- **인공지능**(AI; Artificial Intelligence)
	- 인간 지능의 단면(학습, 추론, 지각, 이해 등)을 기계(컴퓨터 소프트웨어나 시스템)로 모방하거나 구현하는 포괄적 개념

```markdown
인공지능 ⊃ 머신러닝 ⊃ 딥러닝
```

#### 다양한 정의
다음과 같이 다양하게 정의되기도 한다.

- **Haugeland** (1985)
	- 컴퓨터가 진정한 의미에서 마음을 가지고 생각하게 만드는 노력

- **Charniak & McDermott** (1985)
	- 계산 모델을 이용한 정신 능력 연구

- **Rich & Knight** (1991)
	- 현재 사람이 더 잘하는 일을 컴퓨터가 하도록 만드는 연구

- **Kurzweil** (1992)
	- 사람이 수행할 때 지능이 요구되는 기능을 기계가 수행하게 만드는 기술

- **Luger & Stublefield** (1993)
	- 지능적 행동의 자동화와 관련된 컴퓨터 과학의 한 분야

### 1.3. 머신러닝
- **전통적 프로그래밍**(Traditional Programming); 명시적
	- 데이터(Data) + **규칙**(Rules; Algorithm) = 결과(Answers)
	- 사람이 직접 규칙을 작성하여, 데이터를 입력하면 결과가 출력된다.

- **머신러닝**(ML; Machine Learning); 암시적
	- 인공지능의 한 분야
	
	- 데이터(Data) + **결과**(Answers) = 규칙(Rules; Model)
	- 알고리즘이 스스로 데이터로부터 규칙을 학습한다/찾아낸다.
	
	- 사람이 직접 어떤 특징(Feature)을 추출해야 하는지 정의한다.

---

- **Why Machine Learning?**
	- 개/고양이 분류같이 사람이 직접 설계하기 어려운 규칙도 있다.

- **Misconceptions about ML**
	- 데이터가 있다고 해서 모델이 알아서 생성되지는 않는다.
	- 모델의 형태는 사람이 결정해야 하며, 파라미터를 데이터로부터 학습한다.

#### 주목받는 이유
1. **빅데이터 시대**
	- 인터넷, [[IoT]], 모바일 등의 발전으로 학습시킬 데이터가 증가함
	- 항공, 의료, 금융, 제조 등 다양한 비즈니스 분야에서 데이터 축적

2. **알고리즘 발달**
	- 비정형 데이터 처리
	- *cf. 딥러닝 알고리즘: NN(CNN, RNN), Transformer*
	- 자율주행, 의료, 품질관리, 챗봇(GPT) 등에서 실용적 성능 입증

3. **Computing Power 향상**
	- 클라우드 컴퓨팅 및 대규모 데이터 센터의 보급
	- GPU 컴퓨팅의 적극적 활용 (병렬 연산의 DL 학습 가속)

### 1.4. 딥러닝
- **딥러닝**(DL; Deep Learning)
	- 머신러닝의 한 종류
	
	- 알고리즘이 스스로 중요한 특징을 찾아서 학습한다.

> Large NN에서 데이터가 많아질수록 꺾이지 않고 계속해서 성능이 상승한다. (Andrew Ng)

### 1.5. 기타 구분
#### Weak/Strong
- **Weak AI (Narrow AI)**
	- 특정 태스크에 특화된 AI (오늘날 실제로 활용되는 AI)
	- 행동, 결정, 아이디어가 프로그래밍되어 있음
	- 기계가 지능을 보여줄 수 있지만, 마음·정신 상태·의식은 없다는 철학적 입장

- **Strong AI (General AI)**
	- 사람의 포괄적 지능을 모사
	- 다양한 태스크를 범용적으로 처리
	- (아직 존재하지 않음; 영화 'I, Robot' 수준)
	
	- 문맥에 따라 AGI와 비슷하게 쓰이지만, 철학에서는 마음·의식의 실제 보유 여부까지 포함하기도 한다.

#### AGI/ASI
- 범용 인공지능(AGI; Artificial General Intelligence)
	- 서로 다른 다양한 과업을 사람처럼 범용적으로 수행하는 지능
	- 아직 합의된 기준을 충족한 시스템은 없다.

- 초지능(ASI; Artificial Super-intelligence)
	- 대부분의 지적 과업에서 인간을 능가한다고 가정하는 초지능 개념

## 2. 연구
### 2.1. 기반
> AI는 다양한 학문이 융합된 분야이다.

- **인공지능**
	1. 탐색 분야
		- (알고리즘)
	2. **기계학습**
		- 통계학(확률), 최적화, 선형대수, 정보이론, 컴퓨터공학

### 2.2. 관점
|           구분           |   Humanly<br>(인간적으로)   |   Rationally<br>(합리적으로)    |
| :--------------------: | :--------------------: | :------------------------: |
| **Thinking**<br>(생각하는) | 사람처럼 사고<br>(뇌과학, 인지과학) |     논리적 사고<br>불확실성 처리      |
|  **Acting**<br>(행동하는)  |  사람처럼 행동<br>(튜링 테스트)   | 목표 최대화 행동<br>(현재 주류 AI 연구) |

- **Humanly** (인간답게)
	- 인간과 같은 방식으로 문제 해결

- **Rationally** (합리적으로)
	- 어떤 목적을 정의하고 목적을 최대화 (논리 기반)

- *cf. 인간적 vs. 합리적을 나누는 것은 감정일까?*
	- 합리적은 목표를 향하지만,
	- 인간적은 목표가 명확하지 않거나, 목표가 명확해도 합리적이지 않다.

- 현재 많이 알려진 AI 연구는 주로 **Acting humanly**와 **Acting rationally**에 집중한다.
	- *(Acting은 눈에 바로 보이는 연구라 지원이 많은 경향이 있음)*

- **Thinking Humanly**
	- 인지과학(Cognitive Science) 분야가 담당
	- 인간의 사고 방식을 모델링
	- 관찰 방법은 사고 과정 관찰, 행동 관찰, 뇌 활동 관찰 세 가지
	- *기본적인 LLM*

- **Acting Rationally**
	- 주어진 환경에서 불확실성을 극복하고 최선의 결과를 얻기 위해 행동하는 합리적 에이전트(Rational Agent) (현재 인공지능 연구의 주류 방식)
	- 예: NASA 행성 탐사 로봇 (환경을 분석, 충전 계획, 자율적으로 행동)

## 3. 역사
| 시기 | 특징 |
| :---: | :---: |
| 1943~1956 | 인공 뉴런, 튜링의 모방 게임, AI 분야의 성립 |
| 1956~1974 | 초기 기호주의 AI와 퍼셉트론 연구 |
| 약 1974~1980 | 1차 AI 겨울 |
| 1980~1987 | 전문가 시스템의 확산과 AI 붐 |
| 약 1987~1993 | 2차 AI 겨울 |
| 1990s~2000s | 통계적 머신러닝과 신경망 연구의 발전 |
| 2006~2012 | 심층 신경망 학습법의 개선 |
| 2012~2017 | 딥러닝의 도약과 확산 |
| 2017~2022 | Transformer와 대규모 사전학습 모델 |
| 2022~현재 | ChatGPT와 생성형 AI의 대중화 |

### 3.1. 초기 기반과 AI의 탄생
> 1940s~1960s

- 1943
	- 워런 매컬럭(Warren McCulloch)과 월터 피츠(Walter Pitts)가 신경 활동을 논리적으로 표현한 초기 인공 뉴런 모형 제안

- 1950
	- 앨런 튜링(Alan Turing)이 논문에서 모방 게임(Imitation Game)을 제안
	- 이후 **튜링 테스트(Turing Test)**로 불림

- 1956
	- 다트머스 하계 연구 프로젝트(Dartmouth Summer Research Project) 개최
	- **인공지능(Artificial Intelligence)**이라는 명칭이 학문 분야의 이름으로 자리 잡음
	- 존 매카시(John McCarthy), 마빈 민스키(Marvin Minsky), 클로드 섀넌(Claude Shannon) 등 참여

- 1957
	- 프랭크 로젠블랫(Frank Rosenblatt)이 **퍼셉트론(Perceptron)** 발표
	- 생물학적 뉴런을 단순화하여 모사한 초기 학습 모델

- 1960
	- 버나드 위드로(Bernard Widrow)와 마시안 호프(Marcian Hoff)가 **ADALINE** 발표

- 1969
	- 마빈 민스키(Marvin Minsky)와 시모어 페퍼트(Seymour Papert)가 단층 퍼셉트론의 표현 한계를 분석
	- 단층 퍼셉트론은 선형 분리가 불가능한 **XOR 문제**를 해결할 수 없음
	- 이 결과는 당시 신경망 연구의 위축에 영향을 줌

### 3.2. 규칙 기반 AI와 AI 겨울
> 1960s~1990s

- 특정 분야의 전문 지식을 `IF 조건 THEN 결과` 형태의 규칙으로 표현하는 **전문가 시스템(Expert System)** 발달
	- 예: DENDRAL, MYCIN

- 약 1974~1980: **1차 AI 겨울**
	- 과도한 기대에 비해 성과가 제한적이었고 연구 자금과 관심이 감소

- 1980s: 전문가 시스템의 상업적 확산으로 AI 연구가 다시 활성화

- 약 1987~1993: **2차 AI 겨울**
	- 전문가 시스템의 높은 구축·유지 비용과 확장성 문제가 드러나며 투자와 관심이 감소

- 규칙 기반 시스템의 한계
	- 방대한 지식을 사람이 직접 규칙으로 작성하고 지속적으로 관리하기 어려움
	- 새로운 상황에 자동으로 적응하기 어려움

### 3.3. 신경망의 부활과 통계적 머신러닝
> 1980s~2000s

- 1986
	- 데이비드 루멜하트(David Rumelhart), 제프리 힌턴(Geoffrey Hinton), 로널드 윌리엄스(Ronald Williams)가 **역전파(Backpropagation)**를 이용한 다층 신경망 학습을 대중화
	- 다층 퍼셉트론(MLP; Multi-layer Perceptron)은 XOR과 같은 비선형 문제를 학습할 수 있음

- 1989
	- 얀 르쿤(Yann LeCun) 등이 역전파를 이용한 초기 CNN으로 손글씨 우편번호 인식 연구 발표

- 1995
	- 코리나 코르테스(Corinna Cortes)와 블라디미르 바프니크(Vladimir Vapnik)가 **SVM(Support Vector Machine)** 발표

- 1997
	- IBM의 **Deep Blue**가 체스 세계 챔피언 가리 카스파로프(Garry Kasparov)와의 공식 매치에서 승리

- 1998
	- 얀 르쿤 등이 논문 *Gradient-Based Learning Applied to Document Recognition*에서 **LeNet-5** 소개

- 1990s~2000s
	- 신경망 외에도 Decision Tree, SVM, AdaBoost, Random Forest, Gradient Boosting 등 통계적 머신러닝 방법이 널리 발전하고 활용됨

### 3.4. 딥러닝의 도약
> 2006~2017

- 2006
	- 제프리 힌턴, 사이먼 오신데로(Simon Osindero), 이휘 테(Yee-Whye Teh)가 심층 신뢰망(DBN)의 층별 사전학습 방법 발표
	- 깊은 신경망을 효과적으로 학습할 수 있는 가능성을 보여줌

- 2012
	- **AlexNet**이 ImageNet 대규모 이미지 인식 대회(ILSVRC)에서 기존 방식보다 큰 폭으로 향상된 성능을 달성
	- GPU, 대규모 데이터, 심층 CNN을 결합한 딥러닝의 전환점

- 2016
	- DeepMind의 **AlphaGo**가 이세돌 9단과의 대국에서 승리

- 2017
	- 논문 *Attention Is All You Need*에서 **Transformer** 구조 제안

### 3.5. 기반 모델과 생성형 AI
> 2018~현재

- 2018
	- **BERT** 등 Transformer 기반 대규모 사전학습 언어 모델 확산

- 2020
	- **GPT-3** 발표로 대규모 언어 모델의 범용적 생성 능력이 주목받음

- 2022
	- **ChatGPT** 공개
	- 대화형 생성 AI가 대중과 산업 전반으로 빠르게 확산

- 2023~현재
	- 텍스트뿐 아니라 이미지·음성·영상 등을 함께 처리하는 멀티모달 기반 모델과 생성형 AI 서비스가 발전

## 4. AI 시대의 비즈니스 마켓 변화
### 4.1. AI의 기술적 위치
- **AI = 응용기술**
	- 소프트웨어의 토대는 빅테크 기업들이 형성
	- 하드웨어의 토대는 반도체 기업들이 형성

> AI는 독립적인 산업이 아니라 **소프트웨어 + 데이터 + 하드웨어 기반의 응용기술**이다.

### 4.2. AI 시대의 기업 구조 변화
> Not IT but some Bigtech

- **세계 시가총액 상위 기업**
	- 대부분 AI/반도체 관련 기업으로 재편
	- NVIDIA, Microsoft, TSMC, Apple, Amazon 등
	- Alphabet (수직 통합과 비용 절감)

- **AI 발전 궤적**
	- AlexNet(2012) → AlphaGo(2016) → Transformer(2017)
	- → AlphaFold(2021) → ChatGPT(2023)
	
	- → AGI(Artificial General Intelligence, 인공일반지능): 5년 이내?
	- → ASI(Artificial Super Intelligence, 초인공지능): 10년 이상?

### 4.3. 직업 시장 변화
> World Economic Forum 2023 & 2025 자료

- **증가하는 직업**
	- AI & Machine Learning Specialists
	- Data Analysts / Data Scientists
	- Robotics Engineers
	- Sustainability Specialists
	- Cybersecurity Specialists

- **감소하는 직업**
	- 은행 창구 직원
	- 계산원
	- 데이터 입력 직원
	- 회계 사무직

> → AI가 **반복적 업무를 자동화**하기 때문

## 5. 생각해볼 지점
- **Jensen Huang (NVIDIA CEO)**
	- "AI가 코딩을 대신할 것, 아이들은 더 이상 코딩을 배울 필요 없다"
	- 하지만 현재 AI는 도구(tool) 수준

- **이어령 (문학평론가)**
	- "인공지능이 인간을 지배할까?"
	- 인간과 AI의 공존에 대한 근본적 질문

## cf. 참고문헌
```markdown
1. Ian Goodfellow, Yoshua Bengio, Aaron Courville, Deep Learning, 2016, MIT Press
2. 김건희 교수, 인공지능 기초, 2020, K-MOOC, 서울대학교
3. 김성범 교수, Machine Learning, 2020, 고려대학교
4. 이상민 교수, Machine Learning, 2020, 광운대학교
5. 김대수, 처음 만나는 인공지능, 2020, 생능출판
```
