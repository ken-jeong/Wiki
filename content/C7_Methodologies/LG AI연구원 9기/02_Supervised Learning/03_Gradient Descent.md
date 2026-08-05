---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-06-30
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
- Gradient Descent는 기울기(경사)를 따라 내려간다.
- 안정적 학습을 위해 랜덤 초기화, 적절한 학습률 조절, 관성(momentum) 등을 사용한다.

## 1. 출발점: 지도학습의 최적화 문제
- 지도학습의 목표는
	- 데이터셋 $(x^{(i)}, y^{(i)})$에 대해
	- 손실함수 $\mathcal{L}(\theta) = \sum_{i=1}^{n} \ell(g_\theta(x^{(i)}), y^{(i)})$의
	- ==손실(loss)을 최소화하는 파라미터 $\theta$를 찾는 문제==이다.

- 고등학교 수학에서는 $f'(x)=0$을 풀어 최소값을 찾지만, 여기서는 $\nabla\mathcal{L}(\theta)=0$을 직접 푸는 게 불가능하고, 설령 찾더라도 **지역 최소값(local minimum)**일 수 있다.

- 문제는 함수 클래스(예: 심층신경망)가 복잡하고 파라미터가 수백만~수십억 개에 달하며, 손실 함수도 복잡해서 $\nabla \mathcal{L}(\theta) = 0$을 만족하는 **해석적 해(analytic solution)를 구할 수 없다**.

## 2. Gradient Descent (경사 하강법)
- **핵심 가정**
	- 함수 전체의 모양(global view)은 모르지만, 현재 위치의 기울기(gradient)는 안다.
	- 따라서 할 수 있는 최선은 현재 위치의 경사를 따라 내려가는 것이다.

- **알고리즘**
	1. 임의의 점 $\theta_0$에서 시작
	2. $i$번째 단계에서 gradient $\nabla \mathcal{L}(\theta_i)$ 계산
	3. 파라미터 업데이트: $\theta_{i+1} = \theta_i - \alpha \nabla\mathcal{L}(\theta_i)$
	
	- $\nabla\mathcal{L}(\theta_i)$: 가장 가파르게 **증가**하는 방향(steepest increasing direction)이므로, 음수를 곱해(−) 감소 방향으로 이동한다.
	- $\alpha$: 학습률(learning rate)

### 2.1. 학습률(Learning Rate)의 영향
- $f(x)=(x-3)^2$ 예시($x_0 = 0$에서 시작)로 본 학습률별 영향
	- 작은 학습률 $\alpha=0.01$: 수렴이 너무 느려서 최솟값에 도달하지 못함
	- 적절한 학습률 $\alpha=0.1$: 적절하게 최소점 $x=3$으로 수렴
	- 큰 학습률 $\alpha=0.7$: 최소값 주변에서 진동(oscillation)하며 천천히 수렴
	- 너무 큰 학습률 $\alpha=1.05$: 발산(divergence), 오히려 최솟값에서 멀어짐

- → **실무 원칙**
	- 손실이 너무 천천히 감소하면 학습률을 키우고, 발산하면 학습률을 줄인다.
	- 또한 초기값에 따라 다른 지역 최소값에 빠질 수 있으므로, **여러 초기값에서 시작**(random initialization)해 본다.
		- 예: Mishra's Bird function 같은 복잡한 함수에서 초기값에 따라 결과가 달라짐

### 2.2. 실전에서의 변형: SGD와 Mini-batch
- 기울기는 자동미분 도구가 계산해주므로 직접 유도할 필요는 없다.
- 다만 데이터가 매우 많을 때, 전체 데이터에 대한 Gradient를 매번 계산하는 것은 비효율적일 수 있다.
- Gradient는 선형이라 평균이 의미를 가지기 때문애, **일부만 샘플링**해서 추정할 수 있다.

- 이로부터 세 가지 변형이 나온디.
	1. **GD (Batch):** 전체 데이터로 기울기 계산 후 1회 업데이트
	2. **SGD (Stochastic):** 데이터 1개마다 기울기 계산·업데이트
	3. **Mini-batch:** 크기 $b$의 미니배치마다 계산·업데이트 (실무 표준)

|        방법        | 한 업데이트당 사용 데이터 |      특징       |
| :--------------: | :------------: | :-----------: |
| Gradient Descent |    전체 $n$개     |   정확하지만 느림    |
|  Mini-batch GD   |  $b$개 샘플 (배치)  | 절충안, 가장 널리 쓰임 |
| SGD (Stochastic) |     1개 샘플      |  빠르지만 노이즈 많음  |

- 선형회귀의 경우 기울기를 $\nabla\ell = (\theta^\top x^{(i)} - y^{(i)})x^{(i)}$로 명시적으로 쓸 수 있지만, 일반적으로는 자동미분(automatic differentiation) 도구가 gradient를 계산해준다.

### 2.3. GD의 단점과 개선(Optimizers)
- **단점**: 평탄한 구간(plateau)이나 지역 최소값에 갇힐 수 있음

이를 보완하는 옵티마이저들:

| 옵티마이저        | 특징                      |
| ------------ | ----------------------- |
| Momentum SGD | 고정 학습률 + 관성             |
| RMSProp      | 기울기 크기에 따라 학습률 적응       |
| ADAM         | 관성 + 적응성 모두 사용 (가장 보편적) |

1. **Momentum SGD**
	- 이전 gradient들의 가중평균(velocity)을 누적해 관성을 부여한다.
	- $v_t = \beta v_{t-1} + (1-\beta)\nabla\mathcal{L}(\theta_{t-1}), \quad \theta_t = \theta_{t-1} - \alpha v_t$
	- $\beta=0$이면 일반 GD, $\beta$가 클수록 관성이 커진다.
	- 진동을 줄이고 평탄한 영역도 통과할 수 있다.

2. **RMSProp**
	- 최근 gradient 크기의 제곱평균으로 학습률을 좌표별로 자동 조정한다.
	- $E[g^2]_t = \beta E[g^2]_{t-1} + (1-\beta)g_t^2, \quad \theta_t = \theta_{t-1} - \dfrac{\alpha}{\sqrt{E[g^2]_t + \epsilon}}, g_t$
	- gradient가 큰 차원은 업데이트를 작게, 작은 차원은 크게 가져간다.

3. **ADAM**
	- Momentum(1차 모멘트)과 RMSProp(2차 모멘트)을 결합하고,
	- 편향 보정(bias correction)을 추가한 것이다.
	- 가장 널리 쓰는 옵티마이저다.
	- $m_t = \beta_1 m_{t-1} + (1-\beta_1)g_t, \quad v_t = \beta_2 v_{t-1} + (1-\beta_2)g_t^2$
	- $\hat{m}_t = \frac{m_t}{1-\beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1-\beta_2^t}, \quad \theta_t = \theta_{t-1} - \frac{\alpha \hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$

### 2.4. 학습률 스케줄링
- **학습률 스케줄링** (Learning Rate Scheduling)
	- 학습 진행에 따라 학습률을 동적으로 조절하는 기법
	- *예: 학습 초기에는 큰 학습률을, 후반에는 작은 학습률을 사용하여 안정적으로 수렴하게 함*

1. **Step Scheduling**
	- $s$ 스텝(epoch)마다 $\alpha := \alpha \times d$로 감소시킨다.

2. **Exponential Scheduling**
	- $\alpha = \alpha_0 \times e^{-\gamma t}$로 지수적으로 감소시킨다.

3. **Adaptive Scheduling (Annealing)**
	- 검증 손실(validation loss)이 개선되지 않고 일정 횟수($p$, patience) 동안 개선되지 않으면 학습률을 감소시킨다.
