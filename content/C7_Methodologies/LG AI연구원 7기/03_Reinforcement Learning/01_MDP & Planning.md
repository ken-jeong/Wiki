---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-19
---
> **MDP and Planning (MDP와 계획)**
> 규칙을 완벽히 알 때의 최적 전략 찾기
> 강화학습의 기초가 되는 마르코프 결정 과정(MDP)과 이를 해결하는 고전적인 방법론을 다룬다.

- **순차적 의사결정 (Sequential Decision Making)**
	- ==에이전트(Agent)가 환경(World)과 상호작용하며 보상(Reward)의 총합을 최대화하는 행동(Action)을 선택하는 과정==이다.

- ==마르코프 결정 과정 (MDP)==
	- **구성요소**: 상태($S$), 행동($A$), 전이 확률($P$), 보상($R$), 감가율($\gamma$)
	- **마르코프 성질**: 미래는 오직 현재 상태에만 의존하며 과거와는 무관하다는 가정

- **가치 함수 (Value Function)**
	- **상태 가치 함수 $V(s)$**: 특정 상태에서 시작했을 때 기대되는 미래 보상의 총합
	- **벨만 방정식 (Bellman Equation)**: 현재 가치와 미래 가치 사이의 관계식

- **계획 (Planning) 알고리즘**
	- ==환경의 모델($P, R$)을 완벽히 알 때 최적 정책을 찾는 방법== (Dynamic Programming)
	- **정책 반복 (Policy Iteration)**: 정책 평가(Evaluation)와 정책 발전(Improvement)을 반복하여 수렴
	- **가치 반복 (Value Iteration)**: 벨만 최적 방정식을 이용해 가치 함수를 반복적으로 업데이트하여 최적 가치와 정책을 도출
