---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-07-03
---
## 1. 전통적 머신러닝 알고리즘
- 딥러닝이 필요 없을 수도 있다
	- 문제가 단순하면 모델도 단순해야 한다.
	- 단순한 문제에 딥러닝을 쓰는 건 과한(overkill) 선택일 수 있다.

### 1.1. Naive Bayes (스팸 필터 예시)
> 해석 가능한(interpretable) 명시적 모델의 대표 사례이다.

- **데이터 표현**
	- 이메일을 단어 사전 크기(예: 50000차원)의 **indicator vector**로 표현한다.
	- 특정 단어가 등장하면 1, 아니면 0

- **Naive Bayes 가정**
	- 클래스 $y$가 주어졌을 때 각 단어($x_j$)들이 ==서로 조건부 독립이라고 가정한다.==
	- 덕분에 결합확률이 곱으로 단순화된다.
$$p(x_1, \dots, x_d \mid y) = \prod_{j=1}^{d} p(x_j \mid y)$$

- **파라미터 추정 (MLE)**
	- 각 단어가 스팸/비스팸에서 등장할 확률 $\phi_{j|y=1}, \phi_{j|y=0}$과
	- 사전확률 $\phi_y$를 단순 빈도 계산으로 추정한다.
	
	- $\phi_{j|y=1} = p(x_j=1|y=1)$: 스팸일 때 단어 j가 등장할 확률
	- $\phi_{j|y=0} = p(x_j=1|y=0)$: 정상일 때 단어 j가 등장할 확률
	- $\phi_y = p(y=1)$: 스팸일 사전확률
	- 각 파라미터는 학습 데이터에서 단순히 빈도수를 세어 계산한다.

- **분류**
	- 베이즈 정리로 $p(y=1 \mid x)$를 계산한다.
$$p(y=1|x) = \dfrac
{\left(\prod_{j=1}^{d} p(x_j|y=1)\right) p(y=1)}
{\left(\prod_{j=1}^{d} p(x_j|y=1)\right) p(y=1)
+ \left(\prod_{j=1}^{d} p(x_j|y=0)\right) p(y=0)}$$

#### (1) Laplace Smoothing
- 학습 데이터에 한 번도 등장하지 않은 단어가 있으면 확률이 0이 되어 전체 곱이 0이 되는 문제가 생긴다.
- 이를 해결하기 위해 분자에 1, 분모에 2(클래스 수)를 더해 **count를 1부터 시작**한다.
$$\phi_{j|y=1} = \frac{1 + \sum_{i=1}^{n} 1{x_j^{(i)}=1 \wedge y^{(i)}=1}}{2 + \sum_{i=1}^{n} 1{y^{(i)}=1}}$$

### 1.2. 결정 트리 (Decision Tree)
- 순서도(flowchart) 형태의 구조이다.

- **구성**
	- 내부 노드: **결정**(decision)
	- 가지(branch): **결정의 결과**
	- 잎 노드(leaf): **최종 출력**(분류면 클래스 라벨, 회귀면 값)

- **예시**
	- 분류: "털이 있는가? → 새끼를 낳는가?"로 포유류 판별
	- 회귀: "면적 > 2000? → 방 개수 > 3?"으로 집값 예측

- **분할 기준 (Splitting Criteria)**
	- 분류는 **엔트로피**(Entropy; 분산도) 측정,
	- 회귀는 부분집합 내 **분산(Variance)** 측정으로 노드를 나눈다.

## 2. 앙상블(Ensemble) 기법
| 비교    | Bagging         | Boosting       |
| ----- | --------------- | -------------- |
| 학습 방식 | 병렬(Parallel)    | 순차(Sequential) |
| 목적    | 분산(Variance) 감소 | 편향(Bias) 감소    |
| 결합    | 투표 / 평균         | 가중 투표 / 가중 평균  |

### 2.1. Bagging
- **배깅 (Bagging; Bootstrap Aggregating)**
	1. 원본 데이터셋에서 중복을 허용하여 무작위로 샘플(subset)을 추출한다.
		- (이를 **복원 추출** 또는 **부트스트랩**이라고 한다.)
	2. 추출된 여러 개의 샘플 데이터셋으로 각각 독립적인 모델을 학습시킨다.
		- (**병렬 학습**)
	3. 각 모델의 예측 결과를 하나로 합친다.
		- 분류 문제는 다수결 투표(majority vote)
		- 회귀 문제는 평균값 계산(average)
	
	- 모델의 분산을 줄여 과적합을 방지하고 모델 강건성(robustness)을 높인다.

- 대표 알고리즘: **랜덤 포레스트 (Random Forest)**
	- 여러 결정 트리를 결합하는 앙상블 기법

### 2.2. Boosting
- **부스팅 (Boosting)**
	1. 먼저 첫 번째 모델을 학습시킨다.
	2. 첫 번째 모델이 틀린 데이터(예측 오류가 큰 데이터)에 가중치를 부여하여 두 번째 모델을 학습시킨다.
	3. 이 과정을 순차적으로 반복하면서 이전 모델의 약점을 보완해 나간다. (**직렬/순차 학습**)
	4. 최종적으로 각 모델의 성능에 따른 가중치를 반영하여 결합한다.

- 대표 알고리즘: **AdaBoost (Adaptive Boosting)**
	- 순차적 학습 방식
	- 먼저 모델을 학습한 뒤 **틀린 샘플에 더 큰 가중치**를 부여하고, 다음 모델이 이 오류를 보정하도록 학습한다.
	- 최종적으로 각 모델의 예측을 오류율 기반 가중합으로 합친다.

## 3. 그 외 지도학습 응용 사례
> 각 사례를 (데이터 / 모델 / 손실)로 정리하면:

### 3.1. CNN
- **초해상도 (Super Resolution)**
	- Data: (저해상도, 고해상도) 쌍
	- Model: CNN
	- Loss: 픽셀 단위 MSE(Pixel-wise MSE) 또는 PSNR

- **객체 탐지 (Object Detection)**
	- Data: (이미지, 박스 + 라벨)
	- Model: CNN (예: Faster R-CNN)
	- Loss: 박스 좌표 차이 + 분류 손실

### 3.2. BERT
- **BERT — Masked Language Modeling**
	- Data: (마스킹된 문장, 원본 토큰화 문장)
	- Model: Transformer
	- Loss: Classification Loss
	- 예: `"The quick [MASK] fox jumps over [MASK] lazy dog"`의 마스크를 맞춤

- **BERT — Next Sentence Prediction**
	- Data: (문장 쌍, 연속 여부 binary 라벨)
	- Model: Transformer
	- Loss: Classification Loss

### 3.3. 이상 탐지
- **이상 탐지 (Anomaly Detection)**
	- Data: (센서 입력, 이상 여부)
	- Model: Deep Learning
	- Loss: (가중) Classification Loss

## 4. 지도학습을 넘어서
### 4.1. 준지도학습
- **준지도학습 (Semi-Supervised Learning)**
	- 라벨링 비용이 비싸므로 라벨 없는 데이터로부터 학습한다.

- **진행 순서**
	- 대량의 unlabeled 데이터로 비지도 사전학습
	- → 소량의 labeled 데이터로 지도 미세조정
	- → 자기학습/증류

- **핵심**
	- 대조 학습(contrastive learning)
		- (SimCLR 등)
		- 같은 이미지의 다양한 변형(crop, color jitter, rotate, cutout 등) 간의 일치도를 최대화한다.
	- 데이터 증강(augmentation)

### 4.2. 생성 모델
- **생성 모델 (Generative Models)**
	- 라벨 없이 $x$만 가지고 **데이터 분포 $p(x)$ 자체를 학습**하는 것이 목표이다.

1. **GAN (Generative Adversarial Networks)**
	- 생성자(Generator)가 가짜 이미지를 만들고,
	- 판별자(Discriminator)가 진짜(1)/가짜(0)를 구별하며 서로 적대적으로 경쟁한다.

2. **Diffusion Models**
	- **forward SDE**(data → noise)
		- 데이터에 점진적으로 노이즈를 더한다.
		- $dx = f(x,t)dt + g(t)dw$
	- **reverse SDE**(noise → data)
		- score function을 이용해 노이즈에서 데이터를 복원한다.
		- $dx = [f(x,t) - g^2(t)\nabla_x \log p_t(x)]dt + g(t)d\bar{w}$
	- 핵심은 **score function** $\nabla_x \log p_t(x)$ 학습이다.
