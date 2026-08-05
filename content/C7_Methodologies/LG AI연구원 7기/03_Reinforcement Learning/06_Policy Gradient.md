---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-24
---
> **Policy Gradient (정책 경사)**
> 행동(정책) 자체를 직접 조정
> 가치 함수를 거치지 않고, 정책(Policy) 자체를 파라미터화하여 직접 최적화하는 방법을 다룬다.

- **기본 개념**
	- 정책 $\pi_\theta(a|s)$를 신경망 등으로 만들고, 보상을 최대화하는 방향으로 파라미터 $\theta$를 경사 상승법으로 업데이트

- **장점**
	- 연속적인 행동 공간(Continuous Action Space) 처리에 유리, 확률적 정책 학습 가능

- **REINFORCE 알고리즘**
	- 몬테카를로 기반의 가장 기본적인 정책 경사 알고리즘

- **베이스라인 (Baseline)**
	- 리턴에서 특정 값(베이스라인, 주로 $V(s)$)을 빼주어 학습의 분산(Variance)을 줄이는 기법

- **Actor-Critic (액터-크리틱)**
	- **Actor**: 정책을 업데이트 (행동 결정)
	- **Critic**: 가치 함수를 학습하여 Actor에게 피드백 제공 (평가)

- **발전된 알고리즘**
	- **TRPO (Trust Region Policy Optimization)**: 정책이 너무 급격하게 변하지 않도록 KL Divergence 제약 조건을 둠 (단조로운 성능 향상 보장)
	- **PPO (Proximal Policy Optimization)**: TRPO의 복잡한 계산을 단순화(Clipping)하여 성능과 구현 용이성을 모두 잡은 ==현재 가장 대중적인 알고리즘==
