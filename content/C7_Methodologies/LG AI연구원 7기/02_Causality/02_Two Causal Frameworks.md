---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-11
---
- **요약** (Summary)
	1. **조정(Adjustment)**: PO의 비교란 가정과 SCM의 조정 기준(백도어 등)은 모두 공변량 조정을 통해 인과 효과를 식별하기 위한 논리적 근거를 제공한다.
	2. **ATE와 CATE**: CATE는 특정 공변량 조건 하의 효과이며, 이를 전체 분포에 대해 평균 내면 ATE가 된다.
	3. **추정 기법**: 성향 점수를 활용한 **IPW**, 그리고 회귀 모델과 IPW를 결합하여 안정성을 높인 **이중 강건(Doubly Robust)** 추정량이 널리 사용된다.

## 1. 잠재적 결과 프레임워크(PO)
처치(Treatment) 전후의 잠재적 결과를 비교하는 데 초점을 둔다.

- **SUTVA (Stable Unit Treatment Value Assumption)**
	- 인과 추론을 위해 필요한 기본적인 가정이다.
	1. **상호 간섭 없음 (No Interference)**
		- 나의 결과가 타인의 처치 여부에 영향을 받지 않아야 한다.
		- (위반 사례: 백신 접종의 집단 면역 효과, SNS 상의 전파 등)
	2. **처치의 일관성 (Consistency)**
		- 처치의 '버전'이 하나여야 한다.
		- 관측된 처치($X$)가 동일하다면, 관측된 결과($Y$)는 해당 잠재적 결과($Y(x)$)와 같아야 한다.

- **인과적 추정량 (Causal Estimands)**
	- **ATE (Average Treatment Effect)**
		- 전체 집단에 대한 평균 처치 효과 ($\tau = E[Y(1) - Y(0)]$)
	- **CATE (Conditional ATE)**
		- 특정 공변량($Z$)을 가진 집단의 평균 처치 효과
	- **ATT**
		- 처치를 받은 집단에 대한 평균 처치 효과

- **무작위 배정 (Randomized Experiments)**
	- 무작위 배정은 관측된 공변량($Z$)뿐만 아니라 관측되지 않은 공변량($U$)까지 균형을 맞춘다.
	- **Unconfoundedness (교란 없음)**
		- 처치가 잠재적 결과와 독립임을 보장한다. ($X \perp (Y(1), Y(0))$)
		- 따라서 연관성이 곧 인과성을 의미하게 된다.

- **관찰 연구(Observational Studies)에서의 가정**
	1.  **No Unmeasured Confounding (비교란 가정)**
		- 공변량 $Z$를 통제했을 때, 처치는 마치 무작위로 배정된 것과 같아야 한다. ($\{Y(1), Y(0)\} \perp X | Z$)
	2.  **Positivity (Overlap, 긍정성/중첩)**
		- 모든 공변량 $Z$ 값에 대해 처치군과 대조군이 존재할 확률이 0이나 1이 아니어야 한다. ($0 < P(X=1|Z) < 1$)

- **식별 (Identification)**
	- 위 두 가정을 만족하면, 인과 효과(ATE)를 관측된 데이터의 조건부 기댓값 차이로 계산할 수 있다.

## 2. 구조적 인과 모델(SCM)
그래프(DAG)를 통해 변수 간의 인과 관계와 가정을 명확히 시각화한다.

- **인과 그래프와 $do(\cdot)$ 연산**
	- 모델이 미지수일 때, 그래프($G$)를 통해 $P(V)$(관측 분포)에서 $P(y|do(x))$(개입 분포)를 도출하는 것이 목표이다.
	- **Truncated Factorization**: 마르코프 가정 하에서, 변수 $X$에 개입($do(x)$)하면 $X$로 들어오는 화살표가 제거되고, 나머지 조건부 확률들의 곱으로 분포가 표현된다.

- **d-separation**
	- 그래프 상에서의 분리(Separation)는 확률 분포 상의 조건부 독립(Conditional Independence)을 의미한다.

- **백도어 기준 (Back-door Criterion)**
	- 관측된 연관성(Association)은 '인과(Causal)'와 '교란(Confounding)'이 섞여 있다.
	- 변수 집합 $Z$가 다음 두 조건을 만족하면 백도어 기준을 충족하며, 이를 통해 교란을 제거하고 인과 효과를 식별할 수 있다.
		1. $Z$의 어떤 노드도 $X$의 후손(descendant)이 아님 (인과 경로를 방해하지 않음)
		2. $Z$가 $X$로 들어가는 화살표를 포함한 모든 경로(교란 경로)를 차단함

- **조정 기준 (Adjustment Criterion)**
	- 백도어 기준보다 더 일반적이고 완전한 기준이다.
	- SCM의 그래프적 기준은 PO의 'Unconfoundedness' 가정과 수학적으로 동치이다.

## 3. 효율적인 추정을 위한 방법론 (Estimation Methods)
인과 효과를 식별한 후, 실제 데이터로 값을 추정하는 방법들이다.

- **회귀 분석 기반 추정 (Regression Estimator)**
	- 처치군($X=1$)과 대조군($X=0$) 각각에 대해 결과($Y$)를 예측하는 회귀 모델($\mu_1(z), \mu_0(z)$)을 학습하여 차이를 계산한다.

- **역확률 가중치 (Inverse Probability Weighting, IPW)**
	- **성향 점수** (Propensity Score, $e(Z) = P(X=1|Z)$)를 사용한다.
	- ==각 샘플에 처치를 받을 확률의 역수를 가중치로 부여하여, 마치 무작위 실험을 한 것과 같은 가상의 모집단을 만들어 ATE를 추정한다.==

- **이중 강건 추정량 (Doubly Robust Estimator)**
	- 회귀 분석 모델과 IPW 방식을 결합한 방법이다.
	- **장점**: ==결과 예측 모델($\mu$)이나 성향 점수 모델($e$) 중 **하나만이라도 정확하면** 일관된(consistent) 추정치를 얻을 수 있어, 모델 설정 오류(misspecification)에 강건한다.==
