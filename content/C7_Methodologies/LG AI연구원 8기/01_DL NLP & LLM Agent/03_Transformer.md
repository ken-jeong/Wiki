---
aliases: []
type: Lecture
tags:
  - 2-2/LG-AI연구원-8기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-07-15
---
- **트랜스포머**
	- RNN의 순차적 처리 한계를 극복하고,
	- Self-Attention을 통해 병렬 처리와 문맥 파악 능력을 비약적으로 향상시킨 현대 LLM(거대언어모델)의 핵심 아키텍처

## 1. 언어 모델
- 언어 모델은 다음과 같은 흐름으로 발전해 왔다.
	1. **Seq2Seq** (Encoder-Decoder): RNN 기반의 기본적인 번역 모델
	2. **Seq2Seq with Attention**: 고정된 문맥 벡터의 한계를 극복하기 위해 어텐션 메커니즘 도입
	3. **Transformer**: RNN을 완전히 제거하고, **Self-Attention**만으로 구성된 모델 (병렬 처리 가능, 장거리 의존성 해결)

## 2. Seq2Seq 모델
- **Seq2Seq 모델**
	- 기계 번역이나 대화 생성처럼 입력 시퀀스를 출력 시퀀스로 변환하는 **인코더-디코더 구조**의 모델
	- **인코더**(Encoder) RNN이 입력 문장 전체를 하나의 실수 벡터(context vector)로 압축하면,
	- **디코더**(Decoder) RNN이 그 벡터를 초기 hidden state로 받아 출력 문장을 생성한다.
	- 즉 디코더는 인코딩 결과에 조건부로 동작하는 언어 모델 역할을 한다.

### 2.1. 기존 RNN 기반 모델의 문제
- 기존 Seq2Seq는 디코더가 오직 마지막 hidden state 하나에만 의존해 번역을 생성한다.

1. **병목 현상**(Bottleneck)
	- 긴 문장의 모든 정보를 고정된 크기의 벡터 하나(마지막 Hidden State)에 압축해서 담아야 하므로 정보가 유실된다.

2. **장기 의존성(Long-term Dependency) 포착이 어려운 문제**
	- 문장 앞부분의 정보가 뒷부분까지 전달되기 어렵다.
	- 거리가 먼 단어 간의 관계를 학습하기 어렵다. (기울기 소실; Vanishing Gradient)

3. **병렬화 불가능**
	- 입력 데이터를 순차적으로 처리해야 하므로 병렬화가 불가능하여 학습 비용이 크다.

## 3. Attention Mechanism
- **해결 아이디어 - 어텐션 메커니즘 (Attention Mechanism)**
	- 디코더가 출력 단어를 예측할 때마다,
	- 입력 문장의 모든 부분을 동일하게 보지 않고,
	- 현재 예측해야 할 단어와 관련이 깊은 부분에 얼마나 집중(Attention)할지를 계산하자.

### 3.1. 작동 방식
> 문장 내의 각 단어가 다른 모든 단어와 어떤 연관이 있는지를 계산한다.

- **작동 방식 (Query, Key, Value)**
	- **Query (질문):** 현재 예측하려는 단어 또는 찾고자 하는 기준 벡터
	- **Key (키):** 입력 문장에 있는 단어들의 주소 또는 인덱스 역할을 하는 벡터
	- **Value (값):** 입력 단어들이 가지고 있는 실제 의미 정보 벡터
	- $\text{Attention}(Q,K,V) = \text{softmax}\left(\dfrac{QK^\top}{\sqrt{d_k}}\right)V$로 계산한다.

- **과정**
	1. Query(디코더의 현재 상태)와 모든 Key(상태) 사이의 유사도(연관성; Score)를 계산한다.
	2. 이를 정규화(Softmax)하여 확률값(가중치; Attention Weights)을 얻는다.
	3. 이 가중치를 각 Value에 곱하고 모두 더해(인코더 상태들의 가중합; Context Vector) 현재 가장 연관성이 높은 정보를 추출(디코더에 전달)해 낸다.

---

1. **정렬 점수 (Alignment score)**
	- 디코더의 직전 hidden state $s_{i-1}$와 각 입력 hidden state $h_j$가 얼마나 잘 맞는지 점수를 계산한다. ($e_{ij} = \text{score}(s_{i-1}, h_j)$)
	- 점수 함수로는 내적, $s_{i-1}^\top W_a h_j$, 또는 단일 은닉층 신경망 방식 등이 있다.

2. **정규화 (Softmax)**
	- 점수를 softmax로 정규화해 어텐션 가중치 $\alpha_{ij}$를 얻는다.

3. **Context vector**
	- 가중치를 사용해 입력 hidden state들의 가중합 $c_i = \sum_j \alpha_{ij} h_j$를 구하고, 이를 디코더에 활용한다.

## 4. Transformer
- **핵심 개념**
	- 2017년 구글 연구팀이 발표한 논문 Attention Is All You Need에서 제안된 딥러닝 모델 아키텍처
	- 기존의 RNN이나 CNN을 완전히 배제하고, **오직 어텐션(Self-attention) 메커니즘만을 사용하여 병렬 처리가 가능한 모델을 구성**
	
	- 처음에 **영어-독일어, 영어-프랑스어 번역**에서 RNN/LSTM 대비 높은 정확도/병렬화를 위한 단순 번역 모델로 설계되어 발표
	- 그러나 이 모델이 가진 뛰어난 병렬 연산 능력과 대규모 데이터 학습 효율성이 증명

- **RNN vs 트랜스포머**
	- **모든 단어를 한 번에(병렬로) 입력받아 처리**하기 때문에 연산 속도가 빠르다.
	- 단어 간의 거리에 상관없이 정보 접근이 가능(Path length: $O(1)$)하여 장거리 의존성 문제를 해결했다.

### 4.1. 핵심 구성 요소
1. **Positional Embedding**
	- RNN과 달리, 트랜스포머는 순서 정보를 자체적으로 알지 못하므로,
	- sin/cos 함수 기반의 위치 임베딩을 토큰 임베딩에 더해 단어 순서 정보를 주입해준다.

2. **Self-attention**
	- 한 문장 내에서 각 단어가 다른 모든 단어에 미치는 영향을 계산하고 가중치를 부여해, 단어마다 문맥을 반영한 새로운 표현을 만든다.
	- 예를 들어 "The animal didn't cross the street because it was too tired"에서 "it"의 표현이 "animal"을 강하게 참조하도록 학습된다.

3. **Multi-head Attention**
	- 어텐션을 서로 다른 선형 투영으로 나누어 이 과정을 여러 번 병렬 수행한 뒤 결과를 결합한다.
	- 문장의 다양한 문맥적 특징을 동시에 포착한다.

### 4.2. 트랜스포머 구조
- 전체적으로 **인코더 6층 + 디코더 6층** 스택으로 구성된다.

- **인코더**
	- 각 층은 (1) Multi-Head Self-Attention과 (2) Position-wise Feed Forward Network 두 개의 서브층으로 구성된다.
	- 각 서브층 출력은 `LayerNorm(x + Sublayer(x))` 형태로, 잔차 연결(residual connection)과 층 정규화를 적용한다.

- **디코더**
	- 인코더와 거의 같지만 서브층이 세 개이다.
	1. Masked Multi-Head Self-Attention
	2. **Encoder-Decoder Attention**(Query는 디코더에서, Key·Value는 인코더 출력에서 가져옴),
	3. Feed Forward Network
	- 마지막에 Linear + Softmax로 출력 확률을 만든다.

- **구조적 특징**
	- **Residual Connection**(잔차 연결): 정보 손실 방지 및 학습 안정화
	- **Layer Normalization**: 각 층의 출력을 정규화
	- **Feed Forward Network**: 각 위치별로 독립적으로 적용되는 신경망

### 4.3. 트랜스포머를 이용한 언어 생성
- **학습 (Training)**
	- 다음 단어 예측을 분류 문제로 보고 Cross Entropy Loss로 학습한다.

- **추론 (Inference)**
	- 단어를 하나씩 예측하고, 예측된 단어를 다시 입력에 포함시켜 다음 단어를 예측하는 **자기회귀**(Autoregressive) 방식이다.
	- 예측한 단어를 문장 끝에 붙여 다시 입력으로 사용하면서 한 단어씩 생성한다.

- **탐색 알고리즘 (Search Algorithm)**
	- **Greedy Search**: 매 단계 확률이 가장 높은 단어만 선택
	- **Beam Search**: 각 단계에서 가장 확률이 높은 상위 `num_beams`개의 후보 경로를 유지하다가, 탐색하여 최종적으로 전체 확률이 가장 높은 경로를 선택(더 자연스러운 문장을 생성)
