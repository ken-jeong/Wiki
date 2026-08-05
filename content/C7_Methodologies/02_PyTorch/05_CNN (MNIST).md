---
aliases: []
type: Lecture
tags:
  - 2-1/PyTorch
draft: false
date: 2025-06-27
---
> [!note] Supplementary Materials
> - 인공지능 [[08_Image Data & CNN]]
> - PyTorch [[05_CNN (MNIST)]]

## 1. MNIST 실습
- **기본 구현**:
	- `nn.Conv2d`, `nn.ReLU`, `nn.Flatten`, `nn.Linear` 등을 `nn.Sequential`로 쌓아 구현한다.

- **구조 설계 팁**:
	- 층이 깊어질수록(Layer가 넘어갈수록) 이미지의 크기는 줄이고, 채널(필터)의 수는 늘리는 방식이 일반적이다.
	- 각 layer의 뉴런 수가 이전 layer의 대략 절반이 되도록 필터 크기와 채널 수를 조정

- **성능 향상 기법**:
	- 채널 수를 늘려 더 큰 CNN을 만들 수 있으나(예: 6→12→24 채널), 학습이 진행되며 test loss가 다시 증가하는 **오버피팅**이 나타날 수 있다.
    1. **Bigger CNN**: 레이어를 더 깊게 쌓고 필터 수를 늘린다.
    2. **Dropout**: 과적합을 막기 위해 학습 시 일부 뉴런을 랜덤하게 비활성화한다.

- **텐서보드** (Tensorboard)
	- 학습 중인 모델의 train 로그(loss, accuracy) 변화를 실시간 그래프로 시각화하여 모니터링하는 도구

## 2. 주요 CNN 모델
> ImageNet Challenge
> 이미지 인식 대회(ILSVRC)를 통해 발전해 온 주요 모델들
> GPU 활용과 함께 층이 깊어지면서 오류율이 급격히 낮아졌다.

|  연도  |         모델          | Top-5 오류율 |
| :--: | :-----------------: | :-------: |
| 1998 |       LeNet-5       |     —     |
| 2012 |       AlexNet       |   15.3%   |
| 2013 |        ZFNet        |   14.8%   |
| 2014 | GoogLeNet/Inception |   6.67%   |
| 2014 |       VGGNet        |   7.3%    |
| 2015 |       ResNet        |   3.6%    |

- **LeNet-5** (1998)
	- 얀 르쿤이 개발한 최초의 CNN 모델 중 하나로 손글씨 숫자를 인식한다.
	- 우편번호 인식 등에 사용

- **AlexNet** (2012)
	- 이미지넷 대회(ILSVRC) 우승 모델로, Deep CNN, ReLU, Dropout, max pooling, GPU 병렬 처리 도입 (딥러닝 붐)

- **VGGNet** (2014)
	- 3x3의 작은 필터를 깊게 쌓아 성능을 높임
	- 구조가 단순하여 널리 쓰임

- **GoogLeNet/Inception** (2014)
	- 다양한 크기의 필터를 병렬로 적용하는 Inception 모듈 구조 도입

- **ResNet** (2015)
	- **잔차 연결 (Residual Connection)** (Skip Connection)을 도입하여 152층까지 깊게 쌓아도 학습이 잘 되도록 함 (사람의 인식률을 뛰어넘은 시점)
