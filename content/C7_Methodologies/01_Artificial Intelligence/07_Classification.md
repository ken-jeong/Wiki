---
aliases: []
type: Lecture
tags:
  - 3-1/인공지능
draft: false
date: 2026-04-27
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

## 핵심 요약
|   개념   |                핵심 내용                |
| :----: | :---------------------------------: |
| 활성화 함수 |          Sigmoid → 비선형성 부여          |
| 손실 함수  |          이진: BCE / 다중: CCE          |
| 다층 구조  |           비선형 결정경계 학습 가능            |
| 학습 방법  | Gradient Descent + Backpropagation  |
| 다중 분류  | Softmax + Categorical Cross Entropy |
> 선형 모델의 한계를 **비선형 활성화 함수**와 **다층 구조**로 극복하고,
> **역전파 알고리즘**으로 효율적으로 학습한다.

## 1. 분류 문제
### 1.1. Regression vs. Classification
> 회귀와 분류의 차이는 **출력값의 형태**에 있다.

- **회귀** (Regression)
	- 출력 $\hat{y}$가 연속적(continuous, 실수)인 값

- **분류** (Classification)
	- 출력 $\hat{y}$가 이산적(discrete, 유한)인 값
	- 정해진 카테고리(클래스) 중 하나를 선택한다.
	- 분류 문제의 핵심 변수는 **클래스의 수**

### 1.2. 이진 분류
- **이진 분류** (Binary Classification)
	- 두 가지 중 하나를 고르는 가장 기본적인 분류
	- 모델 출력이 하나의 확률값 $\hat{y}$일 때 사용한다.
	- 예: 스팸/정상, 합격/불합격, 양성/음성 분류

> 가장 기본적인 분류 문제 설정

- 데이터셋
	- 주어진 데이터: $(x^{(1)}, y^{(1)}), \dots, (x^{(n)}, y^{(n)})$
	- 여기서 $x^{(i)} = (x_1^{(i)}, x_2^{(i)})$, $y^{(i)} \in {-1, 1}$

- 함수 클래스 $\mathcal{G}$와 손실 함수 $\ell$을 설정
	- 선형 분류기 (Linear Classifier)
		- $\mathcal{G} = {g_{a,b}(x) = \text{sign}(a^\top x + b)}$
	- 0-1 손실 (0-1 Loss)
		- $\ell(g_{a,b}(x^{(i)}), y^{(i)}) = \mathbf{1}(g_{a,b}(x^{(i)}) \neq y^{(i)})$
		- 분류가 틀리면 1, 맞으면 0

- **가정**
	- 데이터가 **선형 분리 가능**(linearly separable)
	- 선형 분리 가능하면 손실 0인 완벽한 분류기가 존재

### 1.3. 다중 클래스 분류
- **다중 클래스 분류** (Multi-class Classification)
	- 여러 클래스를 분류
	- 즉 모델 출력이 클래스별 확률분포일 때 사용한다.
	- 예: 이미지 클래스 분류(MNIST 숫자, CIFAR-10 등), 품종 분류

- 다중 클래스 분류의 접근법은 두 가지이다.
	1. 하나는 **1 vs. others** 이진 분류기를 여러 개 사용하는 것이고,
	2. 다른 하나는 **Softmax**를 직접 사용하는 것이다.

## 2. 분류 모델의 출력
|  구분   |         Logit         |       Sigmoid        |        Softmax         |
| :---: | :-------------------: | :------------------: | :--------------------: |
|  방향   |        확률 → 실수        |     실수 → 확률 (이진)     |     실수 벡터 → 확률 분포      |
| 입력 범위 |       $(0, 1)$        | $(-\infty, +\infty)$ | $(-\infty, +\infty)^K$ |
| 출력 범위 | $(-\infty, +\infty)$  |       $(0, 1)$       |   $(0, 1)^K$, 합 = 1    |
| 주요 용도 | 로지스틱 회귀,<br>raw score |      이진 분류 출력층       |       다중 클래스 출력층       |
| 손실 함수 |           —           |         BCE          |          CCE           |

### 2.1. Guess
> 분류 모델에서 예측 결과 $\hat{y}$는 두 가지 방식으로 표현할 수 있다.

#### (1) Hard Guess
- **Hard Guess**
	- "이것은 A다" (단정)
	- $g_\theta(x^{(i)}) = 1$ 또는 $-1$로 단정적으로 분류

#### (2) Soft Guess
- **Soft Guess**
	- "이것이 A일 **확률**은 70%다" (확신의 정도를 함께 표현)
	- $g_\theta(x^{(i)}) = \begin{bmatrix} Pr(y^{(i)}=-1) \ Pr(y^{(i)}=1) \end{bmatrix}$ (각 클래스에 속할 확률)

- **Soft Guess를 위한 함수**
	- **Sigmoid**: 결과값을 0~1 사이 확률로 변환 (보통 이진 분류용)
	- **Softmax**: 여러 클래스에 대해 **확률의 합이 1이 되도록** 변환 (보통 다중 분류용)

- cf. 결정 경계(decision boundary)에
	- 가까운 점은 "아마도(probably)" 어느 쪽,
	- 멀리 있는 점은 "확실히(certainly)" 어느 쪽이라고 표현할 수 있다.

### 2.2. Logit
$$\text{확률} \xrightarrow{\text{logit}} \text{실수 (log-odds)}$$

- **Logit**(log-odds)은
	- 확률 $p$를 → 실수 전체 범위 $(-\infty, +\infty)$로 변환하는 함수다.
	- 확률을 선형 모델에서 다루기 쉬운 형태로 바꿔준다.

- **넓은 의미로의 Logit**은
	- **신경망에서 활성화 함수를 적용하기 전의 출력값 (raw score)** 을 가리키는 용어로도 쓰인다.

- **수식**은: $\text{logit}(p) = \log \dfrac{p}{1-p}$

- **여기서**,
	- $\dfrac{p}{1-p}$: 오즈(odds) — "성공 확률 대 실패 확률"의 비율
	- 이를 로그로 변환한 것이 **log-odds (로그 오즈)**

- **특성**은:
	- $p < 0.5$ → logit < 0
	- $p = 0.5$ → logit = 0
	- $p > 0.5$ → logit > 0

### 2.3. Logistic Function (Sigmoid)
- **Logistic Function (Sigmoid)**
	- 임의의 실수 $z$를 → **(0, 1) 범위의 확률**로 변환한다.
	- Logit의 **역함수**이다.
	- 이진 분류에서 출력층 활성화 함수로 주로 사용된다.

- **수식**은: $\sigma(z) = \dfrac{1}{1 + e^{-z}}$

- **특성**은:
	- 출력 범위: $(0, 1)$ — 출력값이 0~1 사이라 확률을 모델링/해석하기 적합하다.
	- 입력 $z = 0$일 때 출력 0.5
	- S자 형태의 부드러운(smooth) 곡선 — 미분 가능

#### (1) 복습. 미분
- Sigmoid의 미분은 자기 자신으로 표현되어 역전파 계산이 간편하다:
	- $\sigma'(z) = \sigma(z),(1 - \sigma(z))$

- **특성**은:
	- 최댓값은 $z=0$일 때 $0.25$
	- $|z|$가 커질수록 0에 수렴 → **Vanishing Gradient** 문제 발생 가능

#### (2) 참고. Logit과의 관계
> Logit과 Sigmoid는 서로 **역함수** 관계다.
$$\text{logit}\bigl(\sigma(z)\bigr) = z$$
$$\sigma\bigl(\text{logit}(p)\bigr) = p$$

### 2.4. Softmax
- **Softmax**는
	- $K$개의 임의 실수 벡터 $\mathbf{z} = [z_1, z_2, \ldots, z_K]$를 → **합이 1인 확률 분포**로 변환한다.
	- 다중 클래스 분류(multi-class classification)에서 출력층 활성화 함수로 사용된다.

- **수식**은:
$$p(y=i \mid x)
= \sigma(z)_i
= \mathrm{softmax}(z)_i
= \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}
, \quad i=1,\dots,K$$

- **특성**은
	- 각 출력값은 모두 $(0, 1)$ 사이의 양수이다.
	- 모든 출력값의 합은 정확히 1이 되어 확률 분포처럼 해석 가능하게 한다.
	- 지수 함수를 사용하므로 큰 값이 더욱 강조된다.

#### (1) 예시
- 로짓 벡터 $\mathbf{z} = [2.0,\ 1.0,\ 0.1]$:
- $e^{2.0} = 7.389,\quad e^{1.0} = 2.718,\quad e^{0.1} = 1.105$
- $\text{합} = 11.212$
- $\text{softmax}(\mathbf{z}) = \left[\dfrac{7.389}{11.212},\ \dfrac{2.718}{11.212},\ \dfrac{1.105}{11.212}\right] \approx [0.659,\ 0.242,\ 0.099]$

#### (2) Logistic ⊆ Softmax
> Logistic Function의 다중 클래스 일반화 -> Softmax

- Logistic Function은 이진 분류($K=2$)인 Softmax과 동치이다:
$$\text{softmax}(z_1)
= \frac{e^{z_1}}{e^{z_1}+e^{z_2}}
= \frac{1}{1+e^{-(z_1-z_2)}}
= \sigma(z_1-z_2)$$

#### (3) 참고. 수치 안정성 (Numerical Stability)
- $e^{z_i}$는 $z_i$가 클 때 오버플로우가 발생할 수 있다. 실제 구현에서는 최댓값을 빼는 트릭을 사용한다:
$$\text{softmax}(z_i) = \frac{e^{z_i - \max(\mathbf{z})}}{\sum_j e^{z_j - \max(\mathbf{z})}}$$

- 수학적으로 동일한 결과이지만 수치 안정성이 크게 향상된다.

#### (4) 왜 Softmax + CEE가 한 쌍인가
|       이유        |                            설명                             |
| :-------------: | :-------------------------------------------------------: |
|    **확률 해석**    |    Softmax 출력이 확률 분포이므로,<br>확률 기반 loss인 CEE가 자연스럽게 매칭     |
|   **MLE와 동치**   |            CEE 최소화<br>= 정답 클래스의 likelihood 최대화            |
| **Gradient 깔끔** | Softmax + CEE 조합은<br>gradient가 $\hat{y} - y$로 매우 단순하게 떨어짐 |

## 3. 결정 경계
### 3.1. 결정 경계의 정의
- **결정 경계** (Decision Boundary)
	- 데이터를 서로 다른 그룹으로 나누기 위해 긋는 **경계선**이다.
	- 데이터의 차원에 따라 결정 경계의 형태가 달라진다.

- 차원에 따른 결정 경계의 형태
	- 1D: 점 (임계값)
	- 2D: 직선
	- 3D: 평면
	- 3D 초과: 초평면 (Hyperplane)

### 3.2. 결정 경계의 수학적 의미
- 분류기는 세 가지 정보를 제공한다.
	1. **분류 확률**
	2. **기준값** (cut-off value, 이진 분류에서 기본값 0.5)
	3. **분류 결과**

- 2D 로지스틱 회귀의 경우:
	- $\hat{p} = \dfrac{1}{1+e^{-(w_1x_1+w_2x_2+w_0)}}$

- 결정 경계는 **시그모이드 출력이 정확히 0.5가 되는 지점**이다.
	- 시그모이드는 입력이 0일 때 0.5를 출력하므로,
	- $\boxed{w_1x_1+w_2x_2+w_0 = 0}$이 곧 결정 경계가 된다.
	- 즉, **결정 경계 = 선형 결합 = 0인 집합**

### 3.3. 임계값 / 절단점
- **임계값**(Threshold) 또는 **절단점**(Cut-off)
	- 예측 결과로 특정 클래스에 속할 확률값을 출력할 때, 이 확률을 기반으로 최종 결정을 내리려면 **임계값**(Threshold) 또는 **절단점**(Cut-off)이 필요하다.
	- 0.5가 기본값이지만, 질병 진단처럼 위음성(FN)을 줄이는 게 중요한 상황에서는 위양성(FP)을 감수하더라도 값을 낮추는 게 유리하다.
	- 이 값을 바꾸면 결정 경계선의 위치도 달라진다.

### 3.4. 다중 클래스로의 확장
- 다중 클래스에서는 각 클래스에 대한 조건부 확률 함수가 따로 존재한다.
	- $f_0(x) = P(y=0 \mid X=x)$
	- $f_1(x) = P(y=1 \mid X=x)$
	- $f_2(x) = P(y=2 \mid X=x)$

- $f_k(x)$가 다른 클래스보다 큰 영역이 곧 클래스 $k$로 분류되는 영역이 되며,
	- 이 경계가 다중 클래스의 결정 경계를 형성한다.

## 4. 분류 모델의 Loss Function
- 명확한 이해를 위해,
	- **Loss Function**을 단일 데이터 포인트에 대한 오차로 정의하고,
	- **Cost Function**을 전체 배치에 대한 Loss Function들의 평균값으로 정의한다.

- 이는 회귀 모델에서 다음과 같이 정의된다.
	- Loss Function: **Squared Error** $(y-\hat{y})^2$
	- Cost Function: **Mean Squared Error** $L=\dfrac{1}{N}\sum_{n=1}^{N}(y^{(n)}-\hat{y}^{(n)})^2$

### 4.1. Squared Error의 문제점
> Loss Function으로 Squared Error를 사용하는 문제 (개별 샘플 관점)

- 분류 모델은 보통 출력층에 **Sigmoid**나 **Softmax** 같은 비선형 활성화 함수를 사용한다.
	- 이때 Squared Error를 손실 함수로 쓰면 다음 문제가 발생한다.

#### (1) Gradient Vanishing (기울기 소실)
- Sigmoid + Squared Error 조합의 미분을 보면:
	- $\dfrac{\partial L}{\partial w} = (\hat{y} - y) \cdot \sigma'(z) \cdot x$

- 여기서 $\sigma'(z) = \sigma(z)(1-\sigma(z))$ 인데,
	- 예측이 크게 틀린 경우 (예: 정답은 1인데 $\hat{y} \approx 0$)
	- $\sigma'(z)$ 값이 0에 가까워져 **기울기가 거의 0이 된다**.
	- 즉, 많이 틀릴수록 오히려 학습이 느려지는 모순이 생긴다.

- 반면 Cross Entropy를 쓰면
	- $\sigma'(z)$ 항이 약분되어 사라져서 $(\hat{y} - y) \cdot x$ 형태가 되므로,
	- 많이 틀릴수록 빠르게 학습된다.

#### (2) 확률 해석에 부적합
- 분류 문제의 출력은 "확률"이며,
- MLE(최대우도추정) 관점에서 Bernoulli/Categorical 분포의 음의 로그 우도는 자연스럽게 Cross Entropy가 된다.
- Squared Error는 가우시안 분포를 가정하는 회귀용 손실이라 분류의 확률적 가정과 맞지 않는다.

#### (3) 오분류 페널티가 약함
- MSE는 오차를 제곱하는데, 확률값은 $[0, 1]$ 범위라 제곱하면 오차가 더 작아진다.
- 예를 들어 정답이 1인데 0.1로 예측한 경우:
	- MSE: $(1-0.1)^2 = 0.81$
	- Cross Entropy: $-\log(0.1) \approx 2.30$

- 즉 MSE는 **확신을 가지고 틀린 예측에 대한 페널티가 약해서**,
	- 모델이 잘못된 확신을 가져도 충분히 교정되지 않는다.

#### (4) 다중 클래스 확장 불가
- "강아지=0, 고양이=1, 호랑이=2"처럼 정수를 부여하면 순서 없는 카테고리에 인위적 순서가 생긴다.
- 고양이가 강아지와 호랑이의 '중간'이 되어 무의미해진다.
- 반면 CEE는 one-hot 벡터 기반으로 클래스 간 독립성을 자연스럽게 보존한다.

### 4.2. Entropy
#### cf. 정보 이론 기초
- **코딩 길이** (Coding Length, 또는 부호 길이)
	- 특정 메시지나 기호(Source Symbol; $x$)를
	- 부호화(Encoding; $c(x)$)했을 때,
	- 그 결과물인 **부호어(Codeword)를 구성하는 기호의 개수**($l(x)$)
	- 일반적으로 디지털 통신이나 컴퓨터 분야에서는 이진 부호(Binary Code)를 주로 사용하므로, 코딩 길이는 **비트(bit) 수**로 표현되는 경우가 많다.

- **최적의 코딩 길이** (Optimal Code Length)
	- 평균 코딩 길이를 최소화하기 위해서는 자주 나타나는 기호(확률 $P(x)$가 높은 기호)에는 짧은 코드를 할당하고, 드물게 나타나는 기호(확률 $P(x)$가 낮은 기호)에는 긴 코드를 할당해야 한다.
	- $l^*(x) = -\log_2 p(x)$

#### (1) Entropy
- **Entropy** (정보이론)
	- 어떤 정보 소스가 발생시키는 **불확실성**(Uncertainty) 또는 **평균 정보량**(Average Information Content)을 측정하는 수학적 척도
	- → 확률 분포 $p$의 **불확실성**을 수치화한 값

- **수식**
$$H(p) = -\sum_{x} p(x) \log_2 p(x)$$

- **해석**
	- 특정 결과가 확실할수록 → 엔트로피 **0**
	- 모든 결과가 균등할수록 → 엔트로피 **최대**
	- 엔트로피보다 더 짧게 코딩하는 것은 불가능

### 4.3. Cross Entropy
- **Cross Entropy** (교차 엔트로피; 정보이론)
	- 실제 분포 $p$를 따르는 사건을,
	- 예측 분포 $q$로 표현할 때 필요한 **평균 정보량**

- **수식**
$$H(p, q) = -\sum_{x} p(x) \log q(x)$$

- **해석**
	- $p = q$ 이면 $H(p, q) = H(p)$ → **이 값이 이론적 최솟값**
	- $H(p, q) = H(p) + D_{KL}(p | q)$ 관계가 성립
	- $p \ne q$ 이면 항상 $H(p, q) \ge H(p)$ (**KL 발산**)

- cf.
	- Cross Entropy = Entropy + KL Divergence
	- 예측이 실제보다 비효율적이므로 Cross Entropy ≥ Entropy
	- 두 분포가 같아질수록 Cross Entropy는 Entropy에 가까워짐

### 4.4. 머신러닝 관점의 Cross Entropy
> 확률 예측을 어떻게 페널티(penalize)할 것인가?
> → 실제 일어난 사건에 더 큰 확률을 부여했을수록 손실이 작아야 한다.

- **수식**
$$H(y, \hat{y}) = -\sum_{c=1}^{C} y_c \log \hat{y}_c$$

- $y$가 one-hot인 정답 레이블 벡터일 때,
	- 정답 클래스 $\text{target}$ 하나만 $y_{\text{target}} = 1$이고
	- 나머지는 0이므로 합산이 한 항으로 줄어든다.
	- $\hat{y}_c$는 Softmax를 통과한 모델의 예측 확률 출력이다.
$$H(y, \hat{y}) = -\log \hat{y}_{\text{target}}$$
> cf. 이 때, 크로스 엔트로피를 최소화하는 것 = KL 발산을 최소화하는 것

- **예시**
	- 정답 레이블 $y=[1,0,0]$
	- 예측 확률 $\hat{y} = [0.7, 0.2, 0.1]$일 때,
		- $L = -(1 \cdot \log 0.7 + 0 \cdot \log 0.2 + 0 \cdot \log 0.1) \approx 0.357$
	- 예측 확률 $\hat{y} = [0.3, 0.6, 0.1]$일 때,
		- $L = -(1 \cdot \log 0.3 + 0 \cdot \log 0.6 + 0 \cdot \log 0.1) \approx 1.204$

- 이진 분류($C=2$)일 때, 다음과 같이 전개할 수 있다.
$$\begin{align*}
H(y, \hat{y}) &= -y_1 \log \hat{y}_1 - y_2 \log \hat{y}_2 \\
              &= -y \log \hat{y} - (1-y) \log (1-\hat{y})
\end{align*}$$

## 5. 분류 모델의 Cost Function
### 5.1. Mean Squared Error (MSE)
> 라벨이 0/1이니까 그냥 **MSE**로 학습해서 분류하면 안 되나요?
> → 수학적으로는 가능, **통계학적으로는 부적절**, 실용적으로는 종종 작동

> Cost Function으로 Mean Squared Error를 사용하는 문제 (전체 데이터셋 관점)

- **왜 되는 것처럼 보이는가?**
	- 이진 라벨 $y \in {0,1}$에 대해 MSE를 최소화하면, 이론적 최적해는 다음과 같다.
	- $\hat{y}^* = \mathbb{E}[y|x] = P(y=1|x)$
	- 즉, MSE 최적해가 사후확률과 일치하므로 0.5 임계값으로 자르면 베이즈 분류기처럼 동작한다. 그래서 되는 것처럼 보인다.

#### (1) Non-convex (비볼록) 손실 표면
- Sigmoid/Softmax + MSE 조합은 파라미터에 대해 **비볼록 함수**가 된다.
- 따라서 손실 표면에 local minima가 많아져서 최적화가 어렵고,
	- 좋은 해에 수렴하지 않을 가능성이 커진다.

- 반대로 Sigmoid + Cross Entropy 조합은 **볼록 함수(convex)** 이므로 전역 최솟값에 안정적으로 수렴한다.

#### (2) 실습: iris로 "MSE vs BCE" 직접 비교
> 같은 모델(ShallowMLP, §6.4)이라도 **loss가 다르면 푸는 방식이 다르다**.

```python
from sklearn.datasets import load_iris
SEED = 42  # 공통 import(torch, pandas 등)와 train_model 정의는 §7.4 참고

# 1. 데이터 로드 → 이진 분류 문제로 변환 ("setosa인가 아닌가")
iris = load_iris()
X = pd.DataFrame(iris.data, columns=iris.feature_names)   # (150, 4)
y = (iris.target != 0).astype(int)                        # setosa=0, 나머지=1

# 2. 분할 — stratify=y로 클래스 비율 유지 (§8.3)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=SEED, stratify=y
)

# 3. 텐서 변환 — y는 unsqueeze(1)로 (N,) → (N,1): 모델 출력 shape와 맞추기
X_train_t = torch.tensor(X_train.values, dtype=torch.float32)
y_train_t = torch.tensor(y_train, dtype=torch.float32).unsqueeze(1)
X_test_t  = torch.tensor(X_test.values, dtype=torch.float32)
y_test_t  = torch.tensor(y_test, dtype=torch.float32).unsqueeze(1)

train_loader = DataLoader(TensorDataset(X_train_t, y_train_t),
                          batch_size=16, shuffle=True)
```

```python
# ==== 방식 A: Regression처럼 풀기 (라벨 0/1을 실수로 보고 MSE) ====
reg_model     = ShallowMLP(input_size=4, hidden_size=8, output_size=1)
criterion_reg = nn.MSELoss()
optimizer_reg = optim.Adam(reg_model.parameters(), lr=0.01)
train_model(reg_model, train_loader, criterion_reg, optimizer_reg, num_epochs=50)

# 평가: 연속값 예측을 반올림해서 0/1로
reg_model.eval()
with torch.no_grad():
    preds      = reg_model(X_test_t.to(DEVICE)).cpu()
    y_pred_reg = preds.round().clamp(0, 1) # 반올림 → 분류

# ==== 방식 B: Classification으로 풀기 (BCEWithLogitsLoss) ====
cls_model     = ShallowMLP(input_size=4, hidden_size=8, output_size=1)
criterion_cls = nn.BCEWithLogitsLoss() # Sigmoid + BCE 내장 → logits 그대로 입력
optimizer_cls = optim.Adam(cls_model.parameters(), lr=0.01)
train_model(cls_model, train_loader, criterion_cls, optimizer_cls, num_epochs=50)

# 평가: sigmoid 통과 → 0.5 기준 (logit > 0 과 동일!)
cls_model.eval()
with torch.no_grad():
    logits     = cls_model(X_test_t.to(DEVICE)).cpu()
    probs      = torch.sigmoid(logits)  # logit → 확률
    y_pred_cls = (probs >= 0.5).float() # == (logits > 0).float()
```

- 결과 해석
	- setosa는 **선형 분리 가능**해서 두 방식 모두 정확도가 높게 나온다. → "실용적으로 종종 작동"
	- 하지만 일반적인 문제에서는 위의 이유들(비볼록, 약한 페널티 등) 때문에 **분류 전용 loss가 우월**하다.

### 5.2. Cross Entropy Error (CEE)
- **Cross Entropy Error** (CEE)
    - 분류 모델의 Loss Function으로 사용되는 Cross Entropy의 구체적인 계산 형태
    - 모델의 예측 확률 분포와 실제 정답 분포 사이의 비유사성을 수치화하여, 이를 **최소화하는 방향으로 학습**이 진행된다.

|    관점     |                                이유                                |
| :-------: | :--------------------------------------------------------------: |
|  **적합도**  | 분류 출력은 확률 분포로 해석되어야 하는데,<br>MSE는 연속 실수값의 차이에 집중하여 확률 분포 모델링에 부적합 |
| **학습 효율** |      **실제 분포와 예측 분포의 비유사성을 측정**하는 CEE가<br>분류에서 학습 성능이 더 좋음       |
| **핵심 직관** |               정답 클래스에 부여한 확률이 높을수록 손실 ↓, 낮을수록 손실 ↑               |

- Cross Entropy를 손실 함수로 사용하는 핵심 이유
    1. **미분 가능**
	    - 역전파(backpropagation)를 통한 경사 하강법 적용 가능
    2. **Softmax/Sigmoid와 궁합이 좋음**
	    - 결합 시 gradient가 $\hat{y} - y$로 매우 단순
    3. **확률 분포 간 차이를 자연스럽게 표현**
	    - 분류 문제의 본질과 부합
    4. **오답에 강한 패널티**
	    - 정답 클래스에 낮은 확률(오답 클래스에 높은 확률)을 주었을 때 손실이 급격히 증가

### 5.3. Categorical Cross Entropy (CCE)
- 실제 학습 시에는
	- 단일 샘플의 Cross Entropy에서
	- $N$개 배치 전체에 대해 평균 크로스 엔트로피 손실을 취한다.
$$L = -\frac{1}{N} \sum_{n=1}^{N} \sum_{c=1}^{C} y_c^{(n)} \log \hat{y}_c^{(n)}$$

- $y$가 one-hot인 정답 레이블 벡터, $\hat{y}$가 예측값일 때,
	- $H(y, \hat{y}) = -\log \hat{y}_{\text{target}}$이므로 다음과 같다.
$$L = -\frac{1}{N} \sum_{n=1}^{N} \log \hat{y}_{\text{target}}^{(n)}$$

#### (1) nn.CrossEntropyLoss
> **Categorical Cross Entropy**를 PyTorch에서 구현한 것이다.

$$\text{logits} \xrightarrow{\text{softmax}} \hat{y} \xrightarrow{\text{CCE}} L$$
- `nn.CrossEntropyLoss`는 내부적으로 **`LogSoftmax` + `NLLLoss`** 를 결합한 형태다.
- 즉, 모델의 마지막 출력에 별도로 softmax를 적용하지 않고 **raw logits를 그대로 입력**해야 한다.
- 이 두 단계를 한 번에 처리하기 때문에 수치적으로 더 안정적(log-sum-exp trick 활용)이다.

### 5.4. Binary Cross Entropy (BCE)
- 이진 분류($C=2$)일 때,
	- $H(y, \hat{y}) = -y \log \hat{y} - (1-y) \log (1-\hat{y})$이므로 다음과 같다.
$$L = -\frac{1}{N} \sum_{n=1}^{N} \left[ y^{(n)} \log \hat{y}^{(n)} + (1 - y^{(n)}) \log (1 - \hat{y}^{(n)}) \right]$$

#### (1) nn.BCEWithLogitsLoss (권장)
> **Binary Cross Entropy**를 PyTorch에서 구현한 것이다.

$$\text{logits} \xrightarrow{\text{sigmoid}} \hat{y} \xrightarrow{\text{BCE}} L$$
- `nn.BCEWithLogitsLoss`는 내부적으로 **`Sigmoid` + `BCELoss`** 를 결합한 형태다.
- 따라서 모델 출력에 sigmoid를 적용하지 않고 **raw logits를 그대로 입력**해야 한다.

#### (2) nn.BCELoss (비권장)
> sigmoid를 따로 적용한 뒤 사용해야 하며 수치적으로 덜 안정적이라
> 일반적으로 `BCEWithLogitsLoss` 사용이 권장된다.

### 5.5. PyTorch 구현
#### (1) 선택 기준
- **이진 분류**의 경우
	- 출력 노드가 1개면 `BCEWithLogitsLoss` (권장)
	- 출력 노드가 2개면 `CrossEntropyLoss`

- **다중 클래스 분류**의 경우
	- `CrossEntropyLoss` (권장)

#### (2) 흔한 실수
- **CrossEntropyLoss인데 target이 float 또는 [N,1]**
	- → `RuntimeError: expected scalar type Long but found Float`
	- `y = y.long()`로 변경, unsqueeze 제거

## 6. 다층 신경망(Multi-Layer NN)
### 6.1. 단층 신경망(Perceptron)의 한계
- 단층 퍼셉트론은 **선형 결정 경계만** 만들 수 있다.
- 따라서 비선형적으로 분포된 데이터(예: 동심원 형태, XOR 문제)는 제대로 분류하지 못한다.
- → 이 한계를 극복하려면 **층을 쌓아야** 한다.

### 6.2. 분류 모델의 커널 트릭
> cf.
> Linear Regression의 다항 특성처럼 추가 feature를 허용 → 비선형 경계 가능

- ==선형 분류기로 해결 불가능한 데이터(예: 원형 경계)에는 추가 특성을 만들어 적용한다.==
- 커널(Kernel)을 사용하여 결정 경계(Decision Boundary)의 모양을 복잡한 곡선이나 유연한 형태로 바꾸는 것 (특히 서포트 벡터 머신, SVM)

- **커널 트릭** (Kernel Trick)
	- 데이터를 실제로 고차원으로 하나하나 변환하려면 계산량이 너무 많아져 컴퓨터가 감당하기 어렵다. 이때 사용하는 것이 **커널 트릭**이다.
	- 커널 트릭은 데이터를 실제로 고차원으로 변환하지 않고도, **원래 차원의 데이터 간의 유사도(내적)를 계산하는 특수한 함수(커널 함수)를 사용하여 마치 고차원에 다녀온 것과 같은 효과**를 내는 기술이다.
	- 이를 통해 계산 효율성을 극대화하면서 복잡한 경계를 만들 수 있다.

- 어떤 커널 함수를 선택하느냐에 따라 결정 경계의 형태가 달라진다.
	- **선형 커널 (Linear Kernel)**
		- 경계를 변형하지 않고 직선(혹은 평면) 그대로 유지한다.
	- **다항식 커널 (Polynomial Kernel)**
		- 경계를 완만한 곡선 형태로 구부린다.
		- 이차 커널 (Quadratic Kernel) $\tilde{x} = (x_1, x_2, x_1^2, x_2^2, x_1 x_2)$ → 비선형 타원 경계 표현
	- **RBF/가우시안 커널 (Radial Basis Function)**
		- 데이터 주위에 동그라미를 그리듯 경계를 만든다.
		- 매우 유연하여 복잡한 패턴이나 섬(island) 모양의 독립된 경계도 만들어낼 수 있다.

### 6.3. 기본 구조
$$\text{입력층(Input)} \rightarrow \text{은닉층(Hidden)} \rightarrow \text{출력층(Output)}$$

- 각 노드(뉴런)에서는 두 가지 연산이 순차적으로 수행된다.
	1. **선형 결합**: $z = \sum_{j=1}^{p} x_j w_j$
	2. **비선형 활성화**: $y = \sigma(z)$

> 핵심: 비선형 활성화 함수가 없으면 층을 아무리 쌓아도 결국 하나의 선형 변환과 같다. **비선형성이 다층 구조의 의미를 만든다.**

### 6.4. 은닉층 1개를 가진 MLP 수식
- 은닉층 노드의 출력
$$h_1^{(1)} = \sigma^{(1)}(z_1^{(1)}) = \frac{1}{1+e^{-(w_{11}^{(1)}x_1 + \cdots + w_{p1}^{(1)}x_p)}}$$

- 전체 모델 출력
$$\hat{y}_i = f(x_i) = \sigma^{(2)}\left(net^{(2)}\left(\sigma^{(1)}\left(net^{(1)}(x_i)\right)\right)\right)$$

#### PyTorch 구현
> 위 수식을 그대로 코드로 옮긴 은닉층 1개짜리 MLP

```python
class ShallowMLP(nn.Module):
    def __init__(self, input_size, hidden_size, output_size):
        super().__init__()
        # input
        self.fc1  = nn.Linear(input_size, hidden_size) # 선형 결합
        self.relu = nn.ReLU() # 비선형 활성화
        self.fc2  = nn.Linear(hidden_size, output_size) # 선형 결합
    
	def forward(self, x):
		# output: sigmoid 없는 raw logit
        return self.fc2(self.relu(self.fc1(x))) 
```

```python
class MLP(nn.Module):
	def __init__(self, input_dim, hidden1, hidden2, num_classes):
		super().__init__()
		self.net = nn.Sequential(
			nn.Linear(input_dim, hidden1),
			nn.ReLU(),
			nn.Linear(hidden1, hidden2),
			nn.ReLU(),
			nn.Linear(hidden2, num_classes), # raw logits
		)
	
	def forward(self, x):
		return self.net(x)
```

### 6.5. 층 수에 따른 표현력
|       구조        |          표현 가능한 영역           |
| :-------------: | :--------------------------: |
| **단층(1-layer)** |   초평면으로 나뉜 반평면(Half-plane)   |
| **2층(2-layer)** |          Convex 영역           |
| **3층(3-layer)** | 임의의 복잡한 영역 (뉴런 수에 따라 표현력 결정) |

## 7. 학습: 역전파(Backpropagation)
### 7.1. 경사하강법(Gradient Descent)
- 각 가중치는 손실에 대한 기울기 방향의 반대로 업데이트된다.
$$w_{11}^{(1)}(t+1) = w_{11}^{(1)}(t) - \alpha \cdot \frac{dl_i}{dw_{11}^{(1)}}$$

### 7.2. 연쇄법칙(Chain Rule) 적용
- **출력층에 가까운 가중치**의 미분 (체인이 짧음)
$$\frac{dl_i}{dw_{1y}^{(2)}} = \frac{dl_i}{d\hat{y}_i} \cdot \frac{d\hat{y}}{dw_{1y}^{(2)}}$$

- **입력층에 가까운 가중치**의 미분 (체인이 김)
$$\frac{dl_i}{dw_{11}^{(1)}} = \frac{dl_i}{d\hat{y}_i} \cdot \frac{d\hat{y}}{dh_{i1}^{(1)}} \cdot \frac{dh_{i1}^{(1)}}{dz_{i1}^{(1)}} \cdot \frac{dz_{i1}^{(1)}}{dw_{11}^{(1)}}$$

### 7.3. Backpropagation의 핵심 아이디어
> Chain rule 기반 gradient 계산 과정에서,
> **출력층 근방 파라미터의 미분 정보를 재사용**하여
> 이전 레이어 파라미터 미분 시 **중복 계산을 제거**함으로써 계산 효율을 높이는 것

- Gradient의 규칙성을 활용
- 반복적으로 나타나는 부분을 효율적으로 처리(메모이제이션)
- TensorFlow, PyTorch, Theano 등 라이브러리는 **자동 미분(autograd)** 기능을 제공하여 이 과정을 자동화

### 7.4. PyTorch 학습 루프 (공통 골격)
> §7.1~7.3의 "경사하강법 + 역전파"가 코드에서는 아래 **5단계**로 나타난다.
> ③→④→⑤ 순서가 핵심 (순서 바꾼 코드에서 틀린 곳 찾기 단골).

```python
for epoch in range(num_epochs): # 공통 학습 루프
	for X_b, y_b in loader:
		outputs = model(X_b)              # ① 순전파
		loss    = criterion(outputs, y_b) # ② 손실 계산
		optimizer.zero_grad()             # ③ 이전 기울기 초기화
		loss.backward()  # ④ 역전파 — autograd가 chain rule 수행
		optimizer.step() # ⑤ 가중치 업데이트 — 경사하강법
```

- 평가 시에는 항상 `model.eval()` + `with torch.no_grad()` 한 쌍을 사용한다.

## 8. 불균형 데이터(Imbalanced Data)에서의 Loss
- 클래스 간 샘플 수 차이가 클 때,
	- 단순 Cross-Entropy는 **다수 클래스(majority class)에 편향**되어 학습된다.
	- 소수 클래스(minority class)의 신호가 손실 함수에서 묻혀버린다.

### 8.1. 이진 분류에서의 손실 기여 복습
| 실제 Y  |      손실에 기여하는 항      |
| :---: | :------------------: |
| Y = 0 | $-\log \hat{P}(Y=0)$ |
| Y = 1 | $-\log \hat{P}(Y=1)$ |

- 정답 클래스의 예측 확률에 $\log$를 씌운 값만 손실로 반영된다.
- → 다수 클래스 샘플이 많으면 그쪽 손실이 압도적으로 커서 모델이 그쪽으로 쏠린다.

### 8.2. 보정 기법
- **Weighted Cross-Entropy**
	- 클래스별 가중치 $w_c$ 부여
	- $L = -\sum_{c} w_c \cdot P(Y=c) \log \hat{P}(Y=c)$

- **Focal Loss**
	- 쉬운 샘플의 기여도를 줄이고, 어려운 샘플에 집중

- **Re-sampling**
    - **Over-sampling**: 소수 클래스 증강
    - **Under-sampling**: 다수 클래스 축소

### 8.3. 데이터 분할 시: `stratify` 파라미터
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y)
```

- `stratify=y`: **클래스 비율을 유지하면서** train/test 분할
- 불균형 상황에서 일반 랜덤 분할은 train/test의 클래스 비율이 제각각이 될 수 있음
- → `stratify`로 원본 비율을 보존해야 평가가 신뢰성을 가짐
