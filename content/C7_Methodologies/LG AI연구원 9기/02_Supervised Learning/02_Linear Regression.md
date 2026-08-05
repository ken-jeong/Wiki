---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-06-29
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

## 요약
- 선형 회귀는 **정규방정식** $a = (X^TX)^{-1}X^TY$으로 깔끔히 풀리지만,
- 특징을 추가해 비선형 패턴까지 다룰 수 있지만 overfitting 위험이 커지므로
- Validation 분리, 정규화, 증강으로 일반화 성능을 관리해야 한다.

## 1. 선형 회귀
> **선형 회귀 (Linear Regression)**
> 입력 $x$와 출력 $y$ 사이에 선형 관계($y = ax + b$)가 있다고 가정하고 모델링하는 기법이다.

### 1.1. 1차원 선형 회귀
- 입력 변수 하나로 출력을 예측하는 가장 기본적인 문제이다.
	- 예: 키-몸무게 예측

- 세 가지 요소로 구성된다.
	1. **함수 클래스(모델)**
		- $\mathcal{G} = g_{a,b}(x) = ax + b$
	2. **파라미터**
		- $\theta = (a, b)$
	3. **손실 함수**
		- **평균 제곱 오차**(MSE, Mean Squared Error)
		- 예측값과 실제값 차이의 제곱
		- $\ell(g_\theta(x^{(i)}), y^{(i)}) = (g_\theta(x^{(i)}) - y^{(i)})^2 = ((ax^{(i)}+b) - y^{(i)})^2$
		- 오차의 제곱을 최소화하는 방식이며, 미분 가능하여 분석적 해를 구하기 용이하다.

### 1.2. 다차원 선형 회귀
- 입력이 여러 개인 경우이다.
	- **데이터**: $x = (x_1, x_2, \dots, x_d) \in \mathbb{R}^d$
	- 예: 키와 손 크기로 몸무게 예측

- **모델**
	- $g_a(x) = a^\top x + b$
	- 또는 $x_0 = 1$ 항을 추가해 $g_a(x) = a^\top x$ 로 깔끔하게 표현 (편향 $b$를 가중치에 흡수)

- **손실**
	- 여전히 2차 함수(quadratic) 형태라서, **gradient = 0** 으로 두면 최적해를 구할 수 있다.
	- 각 파라미터 $a_1, a_2, b$에 대한 편미분을 0으로 놓으면 $(n+1)$개 변수의 연립 일차 방정식이 된다.

## 2. 정규방정식
> **정규 방정식 (Normal Equation)**
> 행렬 연산을 통해 한 번에 최적의 해($a = (X^T X)^{-1} X^T Y$)를 구할 수 있다.

- 행렬로 표현하면
	- 데이터 행렬 $X \in \mathbb{R}^{n\times d}$, 타깃 $Y \in \mathbb{R}^n$ 에 대해
	- $\mathcal{L}(a) = |Xa - Y|^2 = Y^TY - 2Y^TXa + a^TX^TXa$

- 미분해서 0으로 놓으면:
	- $\dfrac{\partial \mathcal{L}}{\partial a} = -2X^TY + 2X^TXa = 0$
	- $\Rightarrow \boxed{a = (X^TX)^{-1}X^TY}$

- 이것이 선형 회귀의 닫힌 형식 해(Closed-form Solution)이다.

## 3. 비선형 데이터와 특징(Feature) 추가
- 데이터가 곡선 형태여도, **입력을 변형해 특징을 추가**하면 선형 회귀 틀을 그대로 쓸 수 있다.

- 예를 들어 2차식을 맞추려면
	- 데이터를 $\tilde{x} = (1, x, x^2)$ 로 재정의하고
	- $g_a(x) = a_0 + a_1 x + a_2 x^2$ 로 둔다.
	- 이는 진짜 "이차 회귀"가 아니라 **여전히 파라미터 $a$에 대해 선형**인 회귀이다.
	- (손실은 동일한 MSE)

- 일반화하면
	- $\tilde{x} = (x_1, \dots, x_d, k_1(x), k_2(x), \dots)$ 처럼 임의의 특징을 붙일 수 있다.
	- 특징이 많을수록 표현력은 커지지만 **과적합(overfitting)** 위험이 커지므로 주의해야 하며, 특징은 데이터 관찰이나 도메인 전문성으로 선택한다.

### 3.1. 과적합 vs. 과소적합 (Bias-Variance Tradeoff)
- **과소적합(Underfit)** - **높은 편향(High Bias)**
	- 모델이 너무 단순하여, 데이터를 충분히 설명하지 못 함

- **적절(Just right)**
	- 적절한 복잡도
	- 패턴을 잘 잡음

- **과적합(Overfit)** - **높은 분산(High Variance)**
	- 모델이 너무 복잡하고, 데이터의 변화에 민감해짐
	
	- 학습 데이터는 거의 완벽하지만, 데이터가 조금만 바뀌어도 곡선의 계수가 크게 출렁여서 불안정하다.
	- **데이터가 많아지면** 안정적으로 비슷한 곡선을 그려, 과적합이 완화된다. ("More data is better")

#### (1) 과적합을 어떻게 알아챌까: Train / Validation / Test
- 고차원 데이터는 시각화가 어려우므로 **검증 세트(Validation Set)** 를 사용한다.
	- **Train ≈ Validation 손실**: 일반화(generalization) 잘 됨 → 좋음
	- **Train 손실 ≪ Validation 손실**: 과적합

- 데이터 분할은 보통
	- **Train set** (~80%): 모델 학습
	- **Validation set** (~20%): 모델 검증·결정에 영향
	- **Test set**: 별도의 데이터로 최종 평가 (학습 단계에선 보통 알 수 없음 )

#### (2) 과적합 대응 방법
1. **더 많은 데이터 확보**
	- 효과적이지만 비쌈
	- (생성 모델로 샘플 생성은 주의 필요)

2. **데이터 증강(Augmentation)**
	- 이동/뒤집기/확대/회전/노이즈/색 변형 등으로 데이터 늘리기
	- 단, 데이터 특성에 맞아야 함 (예: 숫자 '2'를 좌우 반전하면 '2'가 아님)

3. **교차 검증(Cross Validation)**
	- 데이터가 부족할 때 K-fold로 나눠 돌아가며 검증

4. **정규화(Regularization)**
	- 손실 함수에 가중치 $a$의 크기에 대한 페널티를 주는 항(L2 Term 등) 추가
	- $\lambda$가 크면 강하게, 작으면 약하게 규제
$$\mathcal{L}(a) + \lambda \mathcal{L}_{reg}(a) = \sum_{i=1}^n \ell(g_\theta(x^{(i)}), y^{(i)}) + \lambda|a|^2$$

#### (3) 모델 선택 원칙
- **오컴의 면도날 (Occam's Razor)**
	- 가정이 가장 적은 설명이 대체로 옳다. (→ 가능한 단순한 모델 선호)

- 사전 지식/이론과 데이터 관찰을 통한 feature 선택

- 낮은 training error + train-validation error 일치 추구
