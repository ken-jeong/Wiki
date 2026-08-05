---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-07-01
---
> [!note]- Supplementary Materials
> **인공지능**
> - [[07_Classification]] (메인 자료)
> - [TensorFlow Neural Network Playground](https://playground.tensorflow.org)
> 
> **PyTorch**
> - [[04_CEE (MNIST)]]
> 
> **LG AI연구원 8기**
> - [[01_머신러닝과 딥러닝의 기초]]
> 
> **LG AI연구원 9기** - Supervised Learning
> - [[04_Classification]]
> - [[05_Logistic Regression]]

## Summary
- 발전 흐름
	- 단순한 분류
	- 최선의 분류(margin)
	- →현실적인 분류(slack)

- 선형 분리 가정(Perceptron)
	- → 여러 해 중 최선 선택(Margin/SVM)
	- → 분리 불가능한 경우 처리(Soft-margin/Hinge Loss)
	- → 비선형 확장(Kernel)

| 방법                    | 특징                                          |
| --------------------- | ------------------------------------------- |
| **Perceptron**        | 0-1 loss, 미분 불가능, 단순하지만 infeasible 시 멈추지 않음 |
| **SVM** (Hard-margin) | 0-1 loss, 미분 불가능, 경계 근처 점만 고려, 선형 분리 가능해야 함 |
| **Soft-margin SVM**   | 경계 근처 점 + 위반 점 고려, convex optimization으로 해결 |

## 1. Perceptron Algorithm
- **Perceptron Algorithm** (Rosenblatt, 1957)
	- 가장 고전적인 학습 알고리즘
	
	1. 가중치 $a$, bias $b$ 초기화
	2. 각 샘플마다 예측
		- $\hat{y}_i =  \text{sign}(z_i) = \text{sign}(\mathbf{a} \cdot \mathbf{x}_i + b)$
	3. 예측 후 잘못 분류된 경우에만 가중치를 업데이트
	    - $\mathbf{a} := \mathbf{a} + \eta(y_i - \hat{y}_i)\mathbf{x}_i$
	    - $b := b + \eta(y_i - \hat{y}_i)$
	
	- **장점**: 해가 있으면 반드시 수렴하며, 매우 단순하다.
	- **단점**: 해가 없으면 영원히 멈추지 않고, 해가 없는지 알 수도 없다.

## 2. Linear Programming 접근
- Perceptron의 단점을 보완하기 위해 LP로 정식화한다.
	- $\text{minimize } 0 \quad \text{subject to } y_i(\mathbf{a} \cdot \mathbf{x}_i + b) > 0 \quad \forall i$

- 목적함수 없이(null objective) 단순히 제약 조건 만족 여부만 확인한다.
	- $y_i(a \cdot x_i + b) > 0, ; \forall i$

- Perceptron과 달리 **문제가 infeasible(해 없음)한 경우를 알려준다**는 장점이 있다.

## 3. Margin과 SVM
### 3.1. Margin 개념
- 선형 분리 가능한 데이터에는 분류 경계가 무수히 많이 존재한다.
- 해가 여러 개일 때, 이 중 어떤 경계가 가장 좋은가?

- → **Margin** 개념 도입
	- Margin: 가장 가까운 점까지의 거리
	- 이를 최대화(Maximum)하면 **robust**한 분류기
	- *마진을 최대화하는 결정 경계를 찾음으로써 모델의 일반화 성능을 높인다.*

- 점 $\mathbf{x}_0$에서 초평면 경계 $\mathbf{a}^T\mathbf{x} + b = 0$까지의 거리
	- $d = \dfrac{|\mathbf{a}^T\mathbf{x}_0 + b|}{|\mathbf{a}|}$

### 3.2. SVM
- **SVM (Support Vector Machine)**
	- 제약을 고정하고 분모(가중치 norm)를 최소화하는 형태로 변환한다.
	- $\text{minimize } \dfrac{1}{2}|\mathbf{a}|^2 \quad \text{subject to } y^{(i)}(\mathbf{a} \cdot \mathbf{x}^{(i)} + b) \geq 1 \quad \forall i$

- 이는 Convex 문제(QCQP; Quadratically Constrained Quadratic Program)로,
- 표준 solver로 풀 수 있고 infeasible 여부도 판별 가능하다.

### 3.3. Soft-margin SVM
- 실제 데이터는 선형 분리 불가능한 경우가 많다.
- 이때 기존의 SVM은 오류를 허용하지 않아 → 선형 분리 불가능하면 infeasible로 끝나버린다.

- **slack variable(penalty) $\xi_i$** 도입으로 오차를 허용해 해결한다.
	- $\text{minimize } \dfrac{1}{2}|\mathbf{a}|^2 + C\sum_{i=1}^{n}\xi_i$
	- $\text{subject to } y_i(\mathbf{a}^T\mathbf{x}_i + b) \geq 1 - \xi_i, \quad \xi_i \geq 0$

- 하이퍼파라미터 C의 역할
	- **큰 C**: 위반(outlier)에 강한 페널티 → 모든 점을 정확히 분류하려 함
	- **작은 C**: 일반적인 샘플의 패턴에 집중 → 일부 outlier는 무시

### 3.4. Hinge Loss
> 0-1 Loss(미분 불가능) 대신 사용하는 SVM의 손실 함수

- Soft-margin SVM은 **Hinge Loss**로 다시 표현할 수 있다.
	- $\mathcal{L}(y_i, f(x_i)) = \max(0, 1 - y_i(a^\top x_i + b))$

- 마진 안쪽이거나 잘못 분류된 경우에만 손실이 생기며, 목적함수는 다음과 같이 정의된다.
	- $\dfrac{1}{2}|\mathbf{a}|^2 + C\sum_{i=1}^{n}\mathcal{L}(y_i, f(\mathbf{x}_i))$

- **C의 역할**
	- C가 크면 outlier에 민감(penalize)
	- C가 작으면 일반 샘플에 집중

## cf. Kernel
- [[07_Classification#cf. 분류 모델의 커널 트릭]]
