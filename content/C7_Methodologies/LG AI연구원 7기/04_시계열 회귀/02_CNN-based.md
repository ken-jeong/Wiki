---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
draft: false
date: 2025-07-26
---
> 합성곱 신경망(CNN) 기반의 시계열 회귀

## 1. CNN의 기본 개념
- 이미지 처리에 특화된 신경망으로 **Convolution(합성곱)**, **Pooling(풀링)** 연산으로 구성

- **특징**
	- **Sparse Connection**: 전체가 아닌 일부 픽셀만 연결
	- **Shared Weight**: 동일한 필터를 전체 영역에 반복 적용 (위치 불변성 특징 추출)
	- **Hyperparameters**: Filter 크기 및 개수, Stride(이동 간격), Padding(테두리 채움)

## 2. 시계열 데이터에의 적용 (1-D Convolution)
- **2-D Conv vs 1-D Conv**
	- 이미지는 가로/세로 픽셀 간의 공간적 연관성(Spatial Correlation)이 중요하여 2-D Conv를 사용
	- ==시계열 데이터는 변수(Feature) 간의 순서가 공간적 의미를 갖지 않으므로, **시간 축으로만 움직이는 1-D Convolution**이 적합함==
	- 필터의 세로 크기는 변수의 개수($D$)와 동일하게 설정하여 한 번에 모든 변수를 고려함

- **Task 유형**
	- 분류(Classification), 회귀(Regression), 예측(Forecasting), 이상 탐지(Anomaly Detection) 등 다양한 태스크에 적용 가능

## 3. Dilated Convolution
- **목적**: 긴 시계열 데이터를 효율적으로 처리하기 위함.
- **방법**: 필터가 데이터를 촘촘히 보지 않고 일정 간격을 띄워서(Dilated) 봄으로써, 적은 레이어로도 넓은 수용 영역(Receptive Field)을 확보하여 장기 패턴을 학습.
