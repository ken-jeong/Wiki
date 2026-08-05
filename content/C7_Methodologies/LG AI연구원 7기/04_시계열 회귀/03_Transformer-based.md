---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-27
---
> 트랜스포머(Transformer) 기반의 시계열 회귀

## 1. Transformer 개요
- 기존 RNN의 ==순차적 처리 방식에서 벗어나 **Attention(주의 집중)** 메커니즘만을 사용==하여 병렬 처리가 가능하고 장기 의존성을 잘 학습하는 모델 (원래 NLP 분야에서 등장).

- **핵심 요소**
	- **Self-Attention**: Query(Q), Key(K), Value(V) 벡터를 이용해 문장(시퀀스) 내 단어들 간의 관계를 파악
	- **Multi-Head Attention**: 여러 개의 Attention을 병렬로 수행하여 다양한 관점의 정보를 포착
	- **Positional Encoding**: 순서 정보가 없는 구조적 한계를 극복하기 위해 위치 정보를 입력값에 더해줌
	- **Encoder-Decoder**: 입력 정보를 처리하는 인코더와 출력을 생성하는 디코더 구조 (시계열 모델에서는 주로 인코더만 사용)

## 2. Time-Series Transformer (TST)
- **기본 논문**
	- Zerveas et al. (KDD 2021)의 연구를 기반으로 함

- **구조적 특징**
	- Transformer의 **Encoder 구조만 사용**
	- 시계열 데이터의 특성(Outlier 존재 등)을 고려하여 NLP에서 쓰던 Layer Normalization 대신 **Batch Normalization**을 사용했을 때 성능이 더 우수함

## 3. 학습 방법 (2 Phases)
1. **Pre-training (비지도 학습)**
	- 입력 시계열 데이터의 일부를 **Masking**하고, 그 가려진 부분을 예측하는 방식으로 학습
	- 데이터의 내재된 표현(Representation)을 학습하는 과정
	- 마스킹은 랜덤이 아닌 기하 분포를 따르는 세그먼트 단위 마스킹(Markov Chain 기반)이 효과적

2. **Fine-tuning (지도 학습)**
	- Pre-training된 모델에 Output Layer(Linear Layer)를 추가하여 실제 풀고자 하는 문제(회귀, 분류 등)에 맞게 미세 조정

## 4. 성능 및 결론
- TST(Pre-trained) 모델은 레이블이 적은 상황(Semi-supervised)에서도 지도 학습(Supervised)만 수행한 모델보다 훨씬 우수한 성능을 보임
- 시계열 데이터의 결측치 보간(Imputation), 분류, 회귀 등 다양한 태스크에 범용적으로 적용 가능함
