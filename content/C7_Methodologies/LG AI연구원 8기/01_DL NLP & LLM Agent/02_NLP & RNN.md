---
aliases: []
type: Lecture
tags:
  - 2-2/LG-AI연구원-8기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-07-14
---
> [!note] Supplementary Materials
> - 인공지능 [[09_텍스트 전처리 & 워드 임베딩]]
> - LG AI연구원 8기 [[02_NLP & RNN]]

## 요약
- 토큰화로 텍스트를 단위로 쪼개고, Word2Vec 같은 임베딩으로 의미를 벡터화한 뒤, RNN으로 시퀀스를 처리한다.
- RNN은 가변 길이 입력을 처리할 수 있지만 vanishing gradient 문제로 장기 의존성 학습이 어렵고, 이를 해결하기 위해 게이트 구조를 가진 LSTM이 등장했다.

## 1. 토큰화
- **토큰화**(Tokenization)는
	- 텍스트를 의미 있는 단위(단어, 서브워드 등)로 분할하는 과정이다.
	- Prefix, Suffix, Exception 등의 규칙을 통해 분할된다.
	
	- 각 토큰은 고유 인덱스로 매핑된다.
	- 사전에 없는 토큰은 `<UNK>`로 처리된다.

### 1.1. 주요 이슈
1. **배칭과 패딩**
	- 길이가 서로 다른 문장을 한 배치로 묶기 위해 짧은 시퀀스에 패딩을 추가한다.

2. **Vocab Size 문제**
	- 너무 크면 → 계산량 증가, 희소성(sparsity) 문제
	- 너무 작으면 → OOV(Out-of-Vocabulary) 문제

- 해결책
	- **Character-level 토큰화**
	- **Byte Pair Encoding (BPE)**

## 2. 워드 임베딩
- **Word2Vec (Mikolov, 2013)**
	- 비슷한 문맥에서 등장하는 단어들이 벡터 공간에서 가까이 위치하도록 학습한다.
	- 두 가지 모델이 있다.
	1. **CBOW**: 주변 문맥 단어들로 중심 단어를 예측
		- $E = -\log p(w_t | w_{t-c}, \ldots, w_{t+c})$
	2. **Skip-gram**: 중심 단어로 주변 문맥 단어들을 예측
		- $E = -\log p(w_{t-c}, \ldots, w_{t+c} | w_t)$
		- 은닉층 가중치 행렬의 각 행이 단어 벡터(look-up table)가 된다.

## 3. 언어 모델
- **언어 모델** (Language Model)
	- 언어 모델은 단어 시퀀스의 확률 분포를 학습한 모델로,
	- 다음 단어를 예측하거나 새로운 문장을 생성할 수 있다.

$$P(w_1, \ldots, w_T) = P(w_1) \cdot P(w_2|w_1) \cdot P(w_3|w_1,w_2) \cdots P(w_n|w_1,\ldots,w_{n-1})$$

- **Fixed-window 신경 언어 모델**은
	- 고정된 개수의 이전 단어만 사용해 다음 단어를 예측한다.
	- 하지만 임의 길이 입력을 처리할 수 없다는 한계가 있어, 가변 길이 입력을 다룰 수 있는 구조가 필요했다.

## 4. RNN
- **RNN** (Recurrent Neural Network)
	- RNN은 순차 데이터(sequential data) 모델링에 자연스러운 구조로, 다음 특징을 가진다.
	1. 매 시점(time step)마다 **같은 가중치**를 반복 적용
	2. 은닉 상태(hidden state)에 정보를 오랫동안 기억(memory) 가능
	3. 매 시점에서 입력을 받음

- **핵심 수식**
	- $s_t = f(Ux_t + Ws_{t-1}), \quad o_t = Vh_t$

- **RNN 언어 모델의 장점**
	- 임의 길이 입력 처리 가능
	- 과거 정보를 활용 가능
	- 입력 길이가 늘어도 모델 크기 불변
	- 가중치 공유로 처리 방식의 대칭성 확보

- **단점**
	- 순환 계산으로 인해 속도가 느림
	- 실제로는 먼 과거 정보 접근이 어려움

### 4.1. RNN 학습
- **손실 함수**
	- 매 시점 예측 분포와 실제 다음 단어(원-핫) 간의 cross-entropy
$$J(\theta) = \frac{1}{T}\sum_{t=1}^{T} -\log \hat{y}^{(t)}_{x_{t+1}}$$

- **Teacher Forcing**
	- 학습 시 모델의 예측이 아닌 실제 정답 단어를 다음 입력으로 사용한다.

- **SGD 사용**
	- 전체 코퍼스가 아닌 문장(또는 배치) 단위로 손실과 그래디언트를 계산해 가중치를 업데이트한다.

- **Backpropagation Through Time (BPTT)**
	- 반복되는 가중치에 대한 그래디언트는 각 시점에서의 그래디언트의 합으로 계산된다.
$$\frac{\partial J^{(t)}}{\partial W_h} = \sum_{i=1}^{t} \left.\frac{\partial J^{(t)}}{\partial W_h}\right|_{(i)}$$

- 실용적으로는 약 20 timestep 정도로 truncated하여 학습합니다.

- **텍스트 생성(Rollout)**
	- 샘플링된 출력을 다음 시점의 입력으로 사용하여 반복적으로 텍스트를 생성한다.

### 4.2. RNN의 문제점과 LSTM
- 문제: **기울기 소실 (Vanishing Gradient)**
	- 시간이 흐를수록 초기 시점 입력의 영향력이 감소하며 사라져,
	- 장기 의존성을 학습하기 어렵다.

- 해결: **LSTM (Long Short-Term Memory)**
	- 3개의 게이트로 정보 흐름을 제어해 장기 정보를 보존한다.
	1. **Input gate**: 입력이 셀에 미치는 영향 조절
	2. **Forget gate**: 이전 셀 상태가 현재 셀에 미치는 영향 조절
	3. **Output gate**: 셀이 출력에 미치는 영향 조절

### 4.3. 문장 인코딩으로서의 RNN
- 감정 분류(sentiment classification) 등의 태스크에서 RNN으로 문장을 인코딩할 때,
	- **기본 방식**: 마지막 hidden state를 문장 표현으로 사용
	- **더 좋은 방식**: 모든 hidden state에 대해 element-wise max나 mean을 취해 사용
