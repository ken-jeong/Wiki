---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-23
---
> **Deep Q Learning (DQN)**
> 딥러닝(Neural Networks)을 강화학습에 성공적으로 적용한 DQN과 그 발전 모델들을 다룬다.

- **DQN (Deep Q-Network)**
	- 딥러닝을 이용해 Q함수를 근사. 아타리(Atari) 게임에서 인간 수준 성능 달성

- **DQN의 핵심 기술 (불안정성 해결)**
	1. **Experience Replay (경험 재사용)**
		- 데이터를 버퍼에 저장하고 무작위로 샘플링하여 학습
		- 데이터 간의 상관관계(Correlation)를 끊음
	2. **Fixed Q-Targets (타겟 고정)**
		- 타겟 값을 계산하는 네트워크를 일정 기간 고정시켜 학습 목표가 흔들리는 것을 방지

- **발전된 알고리즘**
	- **Double DQN**: Q-Learning의 과대평가 문제 해결
	- **Prioritized Experience Replay**: 중요한(오차가 큰) 데이터를 더 자주 학습
	- **Dueling DQN**: 가치 함수를 상태 가치($V$)와 어드밴티지($A$)로 나누어 학습
