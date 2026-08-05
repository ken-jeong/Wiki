---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-21
---
> **Model-Free Control (모델 프리 제어)**
> 평가를 바탕으로 더 나은 행동 선택하기
> 모델 없이 최적의 정책(Policy)을 찾아내는 알고리즘을 다룬다.

- **탐험(Exploration) vs 활용(Exploitation)**
	- 이미 알고 있는 좋은 행동만 할 것인가(활용), 새로운 가능성을 시도할 것인가(탐험)의 딜레마
	- ($\epsilon$-greedy 정책 사용)

- **SARSA (On-Policy)**
	- 현재 정책을 따르면서 얻은 경험 ($s, a, r, s', a'$)을 이용해 Q함수를 업데이트
	- 자신이 수행하는 정책을 평가하고 개선함

- **Q-Learning (Off-Policy)**
	- 행동은 탐험적 정책을 따르지만, 학습은 최적 가치(max Q)를 목표로 함 ($s, a, r, s'$)
	- **Maximization Bias**: 최대값을 추정치로 사용하기 때문에 가치를 과대평가하는 경향이 있음
	- -> **Double Q-Learning**으로 해결 (평가와 선택을 분리)
