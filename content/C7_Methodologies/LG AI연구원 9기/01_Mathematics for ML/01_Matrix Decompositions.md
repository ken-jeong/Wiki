---
aliases: []
type: Lecture
tags:
  - 2-1/LG-AI연구원-7기
  - 3-1/LG-AI연구원-9기
draft: false
date: 2026-07-04
---
> 점점 더 일반적인 행렬에 적용 가능한 분해로 확장되는 구조

## 1. 행렬을 어떻게 요약하는가?
1. [[02_Matrix#(1) 주대각선 & 대각합|대각합 (Trace)]]
2. [[03_Determinant|행렬식 (Determinant)]]
3. [[07-1_고유벡터와 고유값|고유벡터와 고유값 (Eigenvectors & Eigenvalues)]]

## 2. 행렬을 어떻게 분해하는가?
### 2.1. Cholesky Decomposition
> 주어진 행렬을 어떤 작은 행렬의 두 개의 곱셈으로 나타낼 수 없을까?

- **Cholesky Decomposition (슐레스키 분해)**
	- 실수 $9 = 3 \times 3$처럼 행렬을 "자기 자신의 형태"로 분해하는 아이디어이다.

- **정리**
	- 대칭이고 양의 정부호(symmetric positive definite)인 $A$는
	- $A = LL^T$로 분해되며,
	- $L$은 양의 대각원소를 갖는 하삼각행렬이고, 유일하며, A의 **Cholesky factor**이다.

- **활용**
	- 빠른 determinant 계산
		- $\det(A) = \det(L)^2 = \prod_i l_{ii}^2$ (대각곱의 제곱으로 계산된다.)
	- 확률변수의 선형변환
	- 다변량 가우시안의 공분산 분해

### 2.2. Diagonalization
> [[07-2_행렬의 대각화|행렬의 대각화 (Diagonalization)]]

- **고유값 분해** (Eigendecomposition)
	- diagonalizable한 matrix A
		- $D=P^{-1}AP$ → $A^k=PD^kP^{-1}$
	- 특별히 orthogonally($P^{-1}=P^T$) diagonalizable한 matrix A
		- $D=P^TAP$ → $A=PDP^T$
	- 정리하자면 $A=PDP^{-1}$로 표현하는 방식

- Matrix A가 symmetric한 경우에는 항상 orthogonally diagonalizable하다.
	- Matrix A가 symmetric한 경우에는 Spectral Theorem에 의해서 모든 Eigenvalue들이 Real Number가 되고, Eigenvector들이 서로 수직하게 된다.

### 2.3. 특이값 분해
> [[07-3_SVD & PCA#1. 특이값 분해 (SVD)|특이값 분해 (SVD)]]

- **스토리라인**
	- 고유값 분해(Eigendecomposition)은 대칭행렬에만 적용
	- Matrix A가 Symmetric하지 않고, 심지어 Square Matrix도 아닌 경우?
	- → ① 비대칭 정방행렬, ② 비정방행렬($m \times n$)까지 확장한 것이 **SVD**

- 배경
	- 임의의 $A \in \mathbb{R}^{m\times n}$에 대해 $S = A^TA$는 항상 대칭이고 양의 준정부호
	- ($S=A^TA$ is always symmetric, positive semidefinite.)

- **특이값 분해 (SVD; Singular Value Decomposition)**
	- **정리**: 모든 $A$는 $A = U\Sigma V^T$로 분해된다.
		- Singular Vector $U \in \mathbb{R}^{m\times m}$
		- Singular Value $\Sigma$: $m \times n$ 행렬, 대각에 특이값 $\sigma_i \geq 0$
		- Right Singular Vector $V \in \mathbb{R}^{n\times n}$: 직교행렬 (좌/우 특이벡터)
	- U와 V는 항상 orthogonal matrix가 된다. ($U$, $U^T$, $V$, $V^T$는 identity가 된다.)

- **작동 원리** (정방행렬 기준)
	- $A^TA = VDV^T$로 직교 대각화
	- → 고유값 $\lambda_1 \geq \cdots \geq \lambda_r > 0$
	- → $\sigma_i = \sqrt{\lambda_i}$, $u_i = Av_i/\sqrt{\lambda_i}$로 좌특이벡터 구성
	- → $U\Sigma = AV$ 성립

|-|EVD ($A = PDP^{-1}$)|SVD ($A = U\Sigma V^T$)|
|---|---|---|
|존재성|정방 + 고유기저 존재 시|**항상 존재**|
|직교성|$P$는 일반적으로 직교 아님|$U, V$ 모두 직교 (회전)|
|정의역/공역|동일|다른 벡터공간 가능|

- 연결
	- $A$의 좌특이벡터는 $AA^T$의,
	- 우특이벡터는 $A^TA$의 고유벡터이며,
	- 특이값은 이들 고유값의 제곱근
	- **$A$가 대칭이면 EVD = SVD**
