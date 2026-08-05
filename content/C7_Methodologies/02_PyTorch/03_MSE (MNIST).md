---
aliases: []
type: Lecture
tags:
  - 2-1/PyTorch
draft: false
date: 2025-06-25
---
## 흐름
- 손글씨 숫자(0~9)를 인식하는 신경망을 학습시키고,
- 학습된 모델을 저장·불러와 활용하는 과정을 다룬다.

- 학습 데이터 6만 개로 모델을 훈련하고,
- MSE를 손실함수로 사용하는 단순한 1-layer 신경망을 구현한다.

## 1. MNIST 데이터셋
- **MNIST**
	- Modified National Institute of Standards and Technology의 약자로,
	- 기계학습 분야에서 가장 유명한 손글씨 숫자 이미지 데이터셋

- **이미지 특성**
	- 28×28 크기의 grayscale 이미지
	- 픽셀값 0~255
	- 각 이미지는 0~9 레이블을 가짐

- **구성**
	- 학습 이미지/레이블 각 60,000개
	- 테스트 이미지/레이블 각 10,000개

## 2. 핵심 개념
- **One-hot vector**
	- 정답 레이블을 10차원 벡터로 표현
	- 예를 들어 숫자 1은 `[0,1,0,0,0,0,0,0,0,0]`로 나타낸다.

- **Softmax 함수**
	- 출력층에서 사용하며, 모든 출력의 합이 1이 되도록 만들어 확률처럼 해석할 수 있게 한다.
	- 여러 클래스 중 하나를 분류할 때 적합하다. (이진 분류는 sigmoid도 가능)
	- $y_k = \dfrac{\exp(a_k)}{\sum_{i=1}^{n} \exp(a_i)}$

- **Linear Transformation**
	- `Y = softmax(X·W + b)` 형태로
	- 784개의 픽셀 정보를 10개의 클래스 정보로 압축 변환한다.
	
	- X: [100, 784] (이미지 100개, 평탄화)
	- W: [784, 10] (가중치)
	- b: [10] (편향, broadcast)
	- L: [100, 10] (출력)

### 2.1. MSE
> 회귀 문제의 **평균 제곱 오차**(MSE; Mean Squared Error)
> 예측값과 실제값의 차이를 제곱해서 평균, 큰 오차에 더 민감

## 3. Gradient Descent
### 3.1. Gradient Descent
> **경사하강법**(Gradient Descent)
> 어떤 손실함수가 정의되었을 때, 손실함수의 값이 최소가 되는 지점을 찾아 가는 방법

### 3.2. 미니배치 학습
> **미니배치**(Mini-batch)
> 전체 데이터를 한 번에 학습하지 않고 효율성을 위해 일부만 무작위로 추출하여 학습하는 방식이다.

- 전체 6만 개 데이터로 매번 MSE를 계산하면 비효율적이므로,
	- 100개 정도를 랜덤 추출한 **미니배치**(Mini-batch)로 학습한다.
	- 매 단계마다 새로운 미니배치를 생성하여 이전보다 MSE가 작아지는 방향으로 가중치를 업데이트하고,
	- 더 이상 작아지지 않으면 학습을 종료한다.

#### (1) Epoch vs. Iteration
> **에포크**(Epoch): 전체 데이터를 한 번 다 본 횟수이다.
> **이터레이션**(Iteration): 한 번의 가중치 업데이트가 일어나는 단위(미니배치 학습 1회)이다.

- **1 Iteration**: 미니배치 1개에 대한 1회 학습
- **1 Epoch**: 전체 데이터셋을 한 번 학습 완료

- 예: 데이터 60,000개, batch_size 100
	- **1 Epoch당 iteration 수**: 60,000 ÷ 100 = **600 iterations**
	- **10,000 iteration → Epoch**: 10,000 ÷ 600 ≈ **16.67 Epoch**
	- **20 Epoch → iteration**: 20 × 600 = **12,000 iterations**

### 3.3. Stochastic Gradient Descent
> **확률적 경사하강법**(SGD: Stochastic Gradient Descent)
> 매개변수 W를 갱신($W←W−η⋅\frac{∂L}{∂W}​​$)하는 알고리즘
> early stopping, epoch 제한 등으로 오차가 더 이상 변하지 않을 때까지 반복한다.

## 4. PyTorch 구현
### 4.1. 구현 핵심
- **데이터 로드**
	- `transforms.ToTensor()`는 세 가지 기능을 수행한다.
		- shape 변환 (H,W)
		- → (1,H,W), 픽셀값 정규화 0~255
		- → 0~1, 타입 변환 uint8
		- → float32

- **모델 구성** (`nn.Sequential` 사용)
```python
def build_model(input_size, num_classes):
    return nn.Sequential(
        nn.Flatten(),                      # 28x28 → 784
        nn.Linear(input_size, num_classes),
        nn.Softmax()
    )
model = build_model(784, 10).to(device)
```

- **손실 함수와 옵티마이저**
```python
loss_criterion = nn.MSELoss()
optimizer = optim.SGD(model.parameters(), lr=0.2)
```

### 4.2. 학습 결과
- 1-layer 신경망으로 테스트 정확도 **약 91.2%** 달성
- 단순한 구조 치고는 양호한 결과
- 손실함수로 **MSE**를 사용했다는 점이 핵심

### 4.3. 모델 저장 (Save Model)
|      방법       |                   코드                   |     특징     |
| :-----------: | :------------------------------------: | :--------: |
| 파라미터만 저장 (권장) | `torch.save(model.state_dict(), PATH)` | 간단, 이식성 좋음 |
|   전체 모델 저장    |       `torch.save(model, PATH)`        | 폴더 구조에 종속됨 |

### 4.4. 저장된 모델의 활용
- 저장된 `.pth` 파일은 세 가지 용도로 load할 수 있다.
	- **이어서 학습 (Resume Training)**: 학습을 중단했다가 나중에 이어서 진행
	- **배포 (Deploy)**: 학습된 모델을 서비스에 활용
	- **전이학습 (Transfer Learning)**: 사전 학습된 모델의 일부 계층 가중치를 그대로 사용해 적은 데이터로도 높은 인식률을 얻는 기법

### 4.5. 추론 (Inference)
- 학습이 끝난 모델로 새로운 입력 이미지에 대해 예측을 수행하는 단계
	- Training Phase에서는 예측값과 정답을 비교해 파라미터를 업데이트하지만,
	- Testing Phase(inference)에서는 고정된 파라미터로 출력 레이블만 산출한다.
