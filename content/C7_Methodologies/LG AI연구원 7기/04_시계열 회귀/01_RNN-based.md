---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-25
---
> 순환신경망(RNN) 기반의 시계열 회귀

## 1. 데이터의 종류
- **Non-Sequential Data**
	- 시간 정보가 없는 정적인 데이터
	- (예: 고객 정보 기반 대출 여부 예측)

- **Sequential (Time-Series) Data**
	- 시간 순서가 중요한 데이터
	- $(N, T, D)$ 형태의 3차원 텐서로 표현됨 ($N$: 관측치, $T$: 시점, $D$: 변수)
	- (예: 반도체 센서 값을 통한 불량 예측)

## 2. RNN (Recurrent Neural Network)
- **기본 구조**
	- 이전 시점의 Hidden State($h_{t-1}$)와 현 시점의 입력($x_t$)을 받아 현 시점의 정보를 갱신하는 순환 구조

- **문제점**
	- ==시퀀스가 길어질수록 그래디언트 소실(Vanishing) 또는 폭주(Exploding) 문제가 발생하여 장기 의존성(Long-term dependency) 학습이 어려움==

## 3. RNN의 발전 모델 (LSTM & GRU)
- **LSTM (Long Short-Term Memory)**
	- Cell State를 도입하고 3개의 Gate(Forget, Input, Output)를 통해 정보의 저장, 삭제, 출력을 조절하여 ==장기 의존성 문제를 해결==

- **GRU (Gated Recurrent Unit)**
	- LSTM을 단순화한 구조
	- Reset Gate와 Update Gate만 사용하며 Cell State 없이 Hidden State만 존재
	- LSTM과 성능은 비슷하면서 연산 효율이 높음

## 4. RNN Variations & Attention
- **Bidirectional RNN**
	- 과거뿐만 아니라 미래 시점의 정보도 함께 고려(순방향+역방향)

- **Attention Mechanism**
	- 시퀀스의 모든 시점 정보를 고정된 크기의 벡터로 압축할 때 발생하는 정보 손실을 막기 위해 제안됨
	- **핵심**: ==출력 시점에 입력 시퀀스의 어느 부분(Time step)이 중요한지 가중치(Score)를 계산하여 반영==
	- **종류**: Bahdanau Attention (Concat 방식), Luong Attention (Dot Product 등 다양한 방식)
