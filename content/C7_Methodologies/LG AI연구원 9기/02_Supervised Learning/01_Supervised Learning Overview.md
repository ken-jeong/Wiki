---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-06-28
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

- 지도학습은
	1. **레이블이 있는 데이터셋**에서,
	2. **함수 클래스** $\mathcal{G}$와 손실 함수 $\ell$을 설정하고,
	3. **전체 손실 함수** $\mathcal{L}(\theta)$를 최소화하는 함수를 찾는 과정이다.

- 선형 회귀는 가장 단순하면서 대표적인 예시로,
	- 함수 클래스를 직선으로, 손실을 MSE로 사용하며,
	- 그래디언트를 0으로 만들어 해를 구한다.

## 1. 지도학습의 응용 예시
> **지도학습 (Supervised Learning)**
> 데이터($x$)와 정답 라벨($y$)이 주어졌을 때,
> 이를 가장 잘 설명하는 함수(모델)를 찾아내는 과정이다.

### 1.1. 응용 사례
#### (1) 회귀
- **가격 예측** (Price Prediction)
	- 과거 시세 데이터로 미래 가격을 예측

#### (2) 분류
- **이미지 분류** (Image Classification)
	- CIFAR10 데이터셋처럼 이미지를 보고 airplane, automobile, bird, cat 등의 10개 범주로 분류

- **텍스트 분류** (Text Classification)
	- 영화 리뷰같은 텍스트를 보고 긍정/부정 등으로 분류

- **다음 단어 예측** (Next Word Prediction)
	- 다음에 올 단어를 확률적으로 예측
	- "The cat sat on the ---" → mat

- **번역** (Translation)
	- 한 언어를 다른 언어로 변환
	- 영어→한국어 ("My dog is cute" → "우리 강아지는 귀여워요")

### 1.2. 형식화
- **데이터 (Data; $x$)**
	- 보통 벡터 형태의 입력 $x \in \mathcal{X}$

- **정답 (Label; $y$)**
	- 레이블 $y \in \mathcal{Y}$

- **데이터셋 (Dataset)**
	- 입력-레이블 쌍의 모음 $(x^{(1)}, y^{(1)}), \dots, (x^{(n)}, y^{(n)})$

- 위 예시들은 입력 $x$와 레이블 $y$가 무엇인지에 따라 구분된다.
	- 이미지 분류에서는 이미지가 $x$이고 클래스명이 $y$이다.
	- 텍스트 분류에서는 문장이 $x$이고 Positive/Negative가 $y$이다.

## 2. 머신러닝의 등장 배경
- 머신러닝 이전의 **규칙 기반(Rule-based) 알고리즘**
	- 전문가 지식에 의존
	- "숫자 0을 어떻게 정의할 것인가", "주가는 언제 오르는가" 같은 복잡한 문제는 규칙으로 풀기 어려웠다.

- **머신러닝**
	- 데이터를 제공하고 알고리즘이 스스로 판단하게 하는 방식
	- Arthur Lee Samuel의 표현을 빌리면, 명시적으로 프로그래밍하지 않고도 컴퓨터가 학습하는 능력을 갖게 하는 분야이다.

## 3. 지도학습
### 3.1. 두 가지 유형
> 지도학습은 레이블의 종류에 따라 두 가지로 나뉜다.

1. **회귀 (Regression)**
	- 레이블 $y$가 연속적·실수(continuous·real)인 경우
	- 가격 예측 등

2. **분류 (Classification)**
	- 레이블 $y$가 이산적·유한한(discrete·finite) 경우
	- 이미지 범주 분류, 텍스트 긍정/부정 분류, 번역 등

> 레이블이 없는 데이터를 다루는 **비지도학습**(Unsupervised Learning)과 대비된다.

### 3.2. 수학적 설정
- **핵심 목표**
	- 입력을 정답으로 매핑하는 **참 함수(True Function)** $f^\star$를 근사하는 것이다.
	- 즉 $f^\star(x^{(i)}) = y^{(i)}$를 만족하는 함수를 근사한다.
	- 이를 위해 함수 후보들의 집합인 **함수 클래스** $\mathcal{G}$를 정하고, 그 안에서 $f^\star$를 잘 근사하는 $g_\theta \in \mathcal{G}$를 찾는다.

- "잘 근사한다(Approximate Well)"는 것을 정의하기 위해,
	- 모든 입력에 대해 $g_\theta(x) \approx f^\star(x)$가 되는 것이 이상적이지만,
	- 이는 실현 불가능(infeasible)하다.
	- 대신 주어진 데이터셋에 대해 비슷한 함숫값을 가지도록,
	- $g_\theta(x^{(i)}) \approx f^\star(x^{(i)}) = y^{(i)}$가 되는 것을 목표로 한다.

- 이 '근사 정도'는 **손실 함수**로 측정한다.
	- 개별 데이터의 오차를 재는 **점별 손실(Pointwise Loss)**
		- $\ell(g_\theta(x^{(i)}), y^{(i)})$
		- MSE, 교차 엔트로피 등
	- 이를 합한 **전체 손실**
		- $\mathcal{L}(\theta) = \sum_{i=1}^{n} \ell(g_\theta(x^{(i)}), y^{(i)})$


### 3.3. 정리
- 정리하면 지도학습의 절차는
	1. 레이블이 있는 데이터셋이 주어지고,
	2. 함수 클래스 $\mathcal{G}$와 손실 함수 $\ell$을 정하고,
	3. 전체 손실 $\mathcal{L}(\theta)$를 최소화하는 $g_\theta$를 찾는 것이다.

- **구성 요소**
	- **데이터(Data, $x$)**
		- 입력 벡터
		- (예: 이미지, 텍스트, 숫자 등)
	- **라벨(Label, $y$)**
		- 예측해야 할 정답
		- (예: 고양이/강아지, 주식 가격)
	- **함수 클래스(Model Class, $\mathcal{G}$)**
		- 데이터와 라벨의 관계를 나타내는 가설들의 집합
		- (예: 선형 모델 $y=ax+b$)
	- **손실 함수(Loss Function, $\ell$)**
		- 예측값($g(x)$)과 실제값($y$)의 차이를 측정하는 함수
		- (예: MSE, Cross-Entropy)

- **목표**
	- 손실 함수의 총합을 최소화하는 파라미터 $\theta$를 찾는 것

## 4. 선형 회귀(Linear Regression) 예시
- 키-몸무게 데이터처럼 양의 상관관계가 있는 경우를 예로 든다.
	- $x \in \mathcal{X} = \mathbb{R}$, $y \in \mathcal{Y} = \mathbb{R}$ (둘 다 실수)

- **함수 클래스 (Model Class, $\mathcal{G}$)**
	- 직선들의 집합 $\mathcal{G} = {g_{a,b}(x) = ax + b}$로 잡는다.
	- 여기서 **파라미터**는 $\theta = (a, b)$이다.

- **손실 함수 (Loss Function, $\ell$)**
	- **평균 제곱 오차(MSE)** 를 사용한다.
	- $\ell(g_\theta(x^{(i)}), y^{(i)}) = (g_\theta(x^{(i)}) - y^{(i)})^2 = ((ax^{(i)} + b) - y^{(i)})^2$

### 4.1. 왜 MSE인가?
- 절댓값 오차나 수직 거리를 써도 되지만, MSE를 쓰는 이유는 **미분 가능하고 해석적인 해(analytic solution)를 가지기** 때문이다.

- 실제로 손실 함수를 전개·정리하면 $S_x, S_y, S_{xx}, S_{xy}$ 같은 합 항들로 단순화된다.
	- $L(a,b) = \dfrac{1}{3}\left[C - 2aS_{xy} - 2bS_y + a^2 S_{xx} + 2abS_x + 3b^2\right]$

- 이를 $a, b$에 대해 편미분하여 그래디언트를 구한다:
	- $\nabla L(a,b) = \left(\dfrac{2}{3}(aS_{xx} + bS_x - S_{xy}),\ \dfrac{2}{3}(aS_x + 3b - S_y)\right)$

- 이 그래디언트를 0으로 놓고 풀면 최적의 $a, b$를 해석적으로 구할 수 있다.
