---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-22
---
> **Function Approximation (함수 근사)**
> 상태 공간이 너무 커서 테이블(표) 형태로 가치를 저장할 수 없을 때, 함수(주로 신경망)를 이용해 근사하는 방법을 다룬다.

- **필요성**
	- 바둑이나 자율주행처럼 상태가 무수히 많은 경우 테이블로 $Q(s,a)$를 모두 저장할 수 없음

- **선형 함수 근사 (Linear VFA)**
	- 상태를 특징 벡터(Feature Vector)로 변환하고 가중치와의 선형 결합으로 가치를 근사

- **최적화**
	- 실제 가치(또는 타겟)와 근사한 가치의 차이(MSE)를 줄이는 방향으로 경사 하강법(SGD) 사용

- **Deadly Triad (죽음의 3요소)**
	- 다음 3가지가 결합되면 학습이 발산(불안정)할 위험이 있음
	1. 함수 근사 (Function Approximation)
	2. 부트스트래핑 (Bootstrapping, 예: TD)
	3. 오프 폴리시 (Off-Policy) 학습
