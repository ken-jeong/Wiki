---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-07-02
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

## 1. Soft Guess (확률적 예측)
> 분류 문제에서 출력 방식을 두 가지로 구분한다.

- **Hard guess**
	- 결과를 +1 또는 -1로 단정
	- $g_\theta(x) = 1$ 또는 $-1$

- **Soft guess**
	- 각 클래스에 대한 확률을 출력
	- 예: $[Pr(y=-1), Pr(y=1)]$

- 일기예보에서 "비 올 확률 70%"처럼 확신의 정도를 표현하는 것이 soft guess이다.
- 결정 경계에 가까운 점은 "아마도 빨강", 멀리 떨어진 점은 "확실히 파랑"처럼 거리에 따라 확신도가 달라진다.

## 2. 모델 정의: Logistic Regression
- **로지스틱 함수(시그모이드)** 를 사용해== 확률을 모델링한다.==
	- $\sigma(x) = \dfrac{1}{1+e^{-x}}$
	- 이 함수는 부드러운(smooth) 곡선이고,
	- 출력이 0~1 사이라 확률 해석이 가능하다.

### 2.1. 입력
> 로지스틱 회귀에서 선형 예측값 $z = w^T x + b$가 바로 logit이며,
> 이를 다시 확률로 변환할 때 **Sigmoid**를 사용한다.

- 입력으로는 선형 결합 $a^\mathsf{T}x + b$를 넣는다.
- 이때 $|a^\mathsf{T}x+b|$ 값이
	- **작으면** 결정 경계 근처라 두 클래스의 확률이 비슷해지고(불확실),
	- **크면** 경계에서 멀리 떨어져 어느 한 쪽 확률이 1에 가까운 상태(확신)에 대응한다.

- 이진 분류 모델의 최종 출력
	- $\hat{y} = \sigma(w^T x + b) = \dfrac{1}{1 + e^{-(w^T x + b)}}$

### 2.2. Model Class
- **Model Class**
	- $\mathcal{G} = \left\{ g_{a,b}(x) = \begin{bmatrix} \dfrac{e^{-(a^Tx+b)}}{1+e^{-(a^Tx+b)}} \\ \dfrac{1}{1+e^{-(a^Tx+b)}} \end{bmatrix} \right\}$

### 2.3. 손실 함수: Cross Entropy Loss
- 예측 확률을 평가하는 손실로, 정답에 높은 확률을 줄수록 손실이 낮아져야 한다.
	- $\ell(g_{a,b}(x^{(i)}), y^{(i)}) = \log \dfrac{1}{\hat{y}(y^{(i)})}$
	- 여기서 $\hat{y}(y^{(i)})$는 실제 라벨에 모델이 부여한 확률이다.

- 이는 분포 간 차이를 재는 **KL 발산(relative entropy)** 에서 유도된다.
- 정답이 one-hot 분포일 때, KL 발산이 정답 클래스에 할당된 확률의 로그 형태로 정리된다.
$$D(p|q) = \sum_x p(x)\log\frac{p(x)}{q(x)}$$

## 3. Logistic Regression의 학습
> 세 가지 구성 요소로 정리된다.
1. **데이터셋 (Dataset)**
	- 이진 라벨이 붙은 $(x^{(1)}, y^{(1)}), \dots, (x^{(n)}, y^{(n)})$

2. **함수 클래스 (Function Class)**
	- 위의 시그모이드 기반 모델

3. **손실 함수 (Loss function)**
	- cross entropy

- 전체 손실은 다음과 같이 깔끔하게 정리되고, 이는 미분 가능하므로 **경사하강법(Gradient Descent)** 을 적용할 수 있다.
$$\mathcal{L}(\theta) = \frac{1}{n}\sum_{i=1}^{n}\log\left(1 + e^{-y^{(i)}\theta^\mathsf{T}x^{(i)}}\right)$$

### 3.1. Logistic Loss vs. Hinge Loss
- 이를 $h(z) = \log(1+e^{-z})$ 형태로 보면 **로지스틱 손실**이며, 비슷한 역할을 하는 **힌지 손실** $\max(0, 1-x)$ 과 대비된다.
	- 로지스틱 손실은 매끄럽게 0으로 수렴한다.
	- 힌지 손실은 특정 지점 이후 정확히 0이 되는 꺾인 형태이다. (SVM에서 사용)

## 4. Multiclass Classification (다중 분류)
- 클래스가 여러 개일 때(예: CIFAR-10의 비행기/자동차/새 등)는 두 가지 접근이 있다.
	1. **1 vs. others**: 이진 분류기를 여러 개 적용
	2. **Softmax**: 라벨 전체에 대한 확률 분포를 한 번에 출력

### 4.1. Softmax
- **Softmax 함수**는 로지스틱 함수의 일반화이다.
$$\sigma(\mathbf{z})_i = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$
- 지수를 취하므로 모든 출력이 양수이고, 합이 1이 되어 확률로 해석된다.

### 4.2. Softmax Regression
- 이진 분류는 softmax에서 특징이 하나만 필요한 특수한 경우이므로, **로지스틱 회귀 ⊆ softmax 회귀** 관계가 성립한다.

- Function Class: $h = Ax + B$, $\hat{y} = \sigma(h)$, $A \in \mathbb{R}^{d\times k}$, $B \in \mathbb{R}^k$ 형태이다.
- Loss Function: 동일하게 cross entropy $\ell = \log \dfrac{1}{\hat{y}(y^{(i)})}$를 쓴다.

## 5. 평가 지표
> [[10_성능 평가 측도]]

- 확률 출력(예: 0.7)을 최종 결정으로 바꿀 때 **임계값(threshold)** 선택이 중요하다.
- 0.5가 무난하지만, 코로나 검사처럼 놓치면 안 되는 상황에서는 false positive를 감수하더라도 임계값을 조정한다.

- 주요 지표
	- **Precision** = $\dfrac{tp}{tp+fp}$ (검출된 것 중 진짜 비율)
	- **Recall** = $\dfrac{tp}{tp+fn}$ (실제 정답 중 검출한 비율)
	- **F1-score**: Precision과 Recall의 조화평균 (둘 사이 트레이드오프를 한 수치로)

- **ROC 곡선**
	- TPR($\dfrac{TP}{TP+FN}$)과 FPR($\dfrac{FP}{FP+TN}$)의 관계를 그린 것이다.
	- 무조건 양성/음성으로 답하면 양 끝점에, 무작위 추측은 대각선에 놓이며, 곡선이 좌상단(완벽한 분류기)에 가까울수록 좋은 모델이다.
