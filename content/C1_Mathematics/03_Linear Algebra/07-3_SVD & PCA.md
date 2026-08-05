---
aliases: []
type: Lecture
tags:
  - 2-1/선형대수학
draft: false
date: 2025-04-28
---
- **참고 자료**
	- 선형대수학(2-1)
		- [[07-3_SVD & PCA]]
	- LG AI연구원
		- [[03_PCA]]

> 선형대수학과 데이터 과학에서 데이터 차원 축소, 노이즈 제거, 특징 추출 등을 위해 가장 널리 사용되는 두 가지 핵심 기법

## 1. SVD
- **SVD** (Singular Value Decomposition, 특이값 분해)
	- 행렬 분해(Matrix Factorization) 기법 중 하나
	- **임의의 $m \times n$ 차원 행렬 $A$를 세 개의 행렬의 곱으로 분해**하는 방법
	- 정방행렬(Square Matrix)이 아닌 직사각형 행렬에도 적용할 수 있다는 점이 특징

### 1.1. 구성
$$A = U \Sigma V^T$$

1. **$U$ (Left Singular Vectors, 좌특이벡터 행렬)**
	- $m \times m$ 크기의 직교행렬(Orthogonal Matrix, $U^T U = I$)
	- $A A^T$의 ==고유벡터(Eigenvectors)들로 구성==되며, 열 벡터들은 서로 직교한다.

2. **$\Sigma$ (Singular Values, 특이값 행렬)**
	- $m \times n$ 크기의 대각행렬(Diagonal Matrix)
	- 대각 성분에는 $A$의 ==특이값(Singular Values)이 내림차순으로 정렬==되어 들어간다.
	- 이 특이값들은 해당 방향으로 데이터가 얼마나 퍼져 있는지(스케일링 정도)를 나타낸다.

3. **$V^T$ (Right Singular Vectors, 우특이벡터 행렬의 전치)**
	- $V$는 $n \times n$ 크기의 직교행렬($V^T V = I$)
	- $A^T A$의 고유벡터들로 구성된다.

### 1.2. 기하학적 의미
- SVD는 어떤 선형 변환(행렬 $A$)을 행하더라도,
- 이를 **회전(Rotation, $V^T$) $\rightarrow$ 스케일링(Scaling, $\Sigma$) $\rightarrow$ 회전(Rotation, $U$)**의 세 단계로 쪼개어 설명할 수 있음을 의미한다.

### cf. Truncated SVD (절단된 SVD)
- SVD를 실무에 적용할 때는 전체 특이값 중 가장 크기가 큰 $k$개만 남기고 나머지를 버리는 **Truncated SVD**를 주로 사용한다.
- 이를 통해 원본 행렬을 완벽하게 복원할 수는 없지만, 가장 중요한 정보(큰 분산)만 보존하면서 데이터의 크기를 대폭 줄일 수 있다.
- (추천 시스템, 이미지 압축 등에 활용)

### 예제 1
$$A=\begin{pmatrix}
1& 2& 0& 0\\
2& 1& 0& 0\end{pmatrix}$$

#### (1) 고유값(특이값) 구하기
- **$AA^T$ 구하기**
$$AA^T=\begin{pmatrix}
1& 2& 0& 0\\
2& 1& 0& 0\end{pmatrix}
\begin{pmatrix}
1& 2\\
2& 1\\
0& 0\\
0& 0\end{pmatrix}
=\begin{pmatrix}
5& 4\\
4& 5\end{pmatrix}$$
- **고유값 및 특이값 구하기**
$λ^2-10λ+9=0$ → **고유값**은 9, 1 → **특이값**은 3, 1

#### (2) 고유벡터 행렬 구하기
- **정규화된 고유벡터 구하기 (λ=9)**
$$\begin{pmatrix}
-4& 4\\
4& -4\end{pmatrix}
⇒\frac{1}{\sqrt2}\begin{pmatrix}
1\\
1\end{pmatrix}$$
- **정규화된 고유벡터 구하기 (λ=1)**
$$\begin{pmatrix}
4& 4\\
4& 4\end{pmatrix}
⇒\frac{1}{\sqrt2}\begin{pmatrix}
-1\\
1\end{pmatrix}$$
- **정규화된 최종 고유벡터 행렬 구하기**
$$\frac{1}{\sqrt2}\begin{pmatrix}
1& -1\\
1& 1\end{pmatrix}$$

#### (3) 최종 분해 결과
$$USV^T=\begin{pmatrix}
1/\sqrt2& -1/\sqrt2\\
1/\sqrt2& 1/\sqrt2\end{pmatrix}
\begin{pmatrix}
3& 0& 0& 0\\
0& 1& 0& 0\end{pmatrix}
\begin{pmatrix}
1/\sqrt2& 1/\sqrt2& 0& 0\\
-1/\sqrt2& 1/\sqrt2& 0& 0\\
0& 0& 1& 0\\
0& 0& 0& 1\end{pmatrix}$$

### 예제 2
$$A=\begin{pmatrix}
4& 11& 14\\
8& 7& -2\end{pmatrix}$$

- **AAT**
$$AA^T=\begin{pmatrix}
4& 11& 14\\
8& 7& -2\end{pmatrix}
\begin{pmatrix}
4& 8\\
11& 7\\
14& -2\end{pmatrix}
=\begin{pmatrix}
333& 81\\
81& 117\end{pmatrix}$$
$λ^2-450λ+32400=0$
$λ^2-450λ+2^4×3^4×5^2=0$
180×180 아니니까 패스 90×360 통과 → **고유값**은 90, 360 → **특이값**은 3, 1
$$A-90I=\begin{pmatrix}
243& 81\\
81& 27\end{pmatrix}
⇒\frac{1}{\sqrt{10}}\begin{pmatrix}
1\\
-3\end{pmatrix}$$
$$A-360I=\begin{pmatrix}
-27& 81\\
81& -243\end{pmatrix}
⇒\frac{1}{\sqrt{10}}\begin{pmatrix}
3\\
1\end{pmatrix}$$

- **ATA**
$$A^TA=\begin{pmatrix}
4& 8\\
11& 7\\
14& -2\end{pmatrix}
\begin{pmatrix}
4& 11& 14\\
8& 7& -2\end{pmatrix}
=\begin{pmatrix}
80& 100& 40\\
100& 170& 140\\
40& 140& 200\end{pmatrix}$$
$$A-90I=\begin{pmatrix}
-10& 100& 40\\
100& 80& 140\\
40& 140& 110\end{pmatrix}
⇒\frac{1}{\sqrt{9}}\begin{pmatrix}
2\\
1\\
-2\end{pmatrix}$$
$$A-360I=\begin{pmatrix}
-280& 100& 40\\
100& -190& 140\\
40& 140& -160\end{pmatrix}
⇒\frac{1}{\sqrt{9}}\begin{pmatrix}
1\\
2\\
2\end{pmatrix}$$
$$A-0I=\begin{pmatrix}
80& 100& 40\\
100& 170& 140\\
40& 140& 200\end{pmatrix}
⇒\frac{1}{\sqrt{9}}\begin{pmatrix}
-2\\
2\\
-1\end{pmatrix}$$

- **결론**
$$USV^T=\frac{1}{\sqrt{10}}\begin{pmatrix}
3& 1\\
1& -3\end{pmatrix}
\begin{pmatrix}
\sqrt{360}& 0& 0\\
0& \sqrt{90}& 0\end{pmatrix}
\frac{1}{\sqrt{9}}\begin{pmatrix}
1& 2& -2\\
2& 1& 2\\
2& -2& -1\end{pmatrix}$$

## 2. PCA
- **PCA** (Principal Component Analysis, 주성분 분석)
	- 고차원 데이터를 정보 손실을 최소화하면서 저차원으로 축소한다.
	- 대표적인 **비지도 학습(Unsupervised Learning) 차원 축소 기법**

### 2.1. 목적 및 개념
- 데이터의 **분산(Variance)이 가장 큰 방향**을 찾아 그 방향으로 데이터를 사영(Projection)시킨다.
- 가장 분산이 큰 축을 **제1 주성분(PC1)**, 그 주성분과 서로 직교하면서 그다음으로 분산이 큰 축을 **제2 주성분(PC2)**으로 설정하여 데이터를 변환한다.
- 이 과정에서 상관관계가 높은 다차원 변수들이 서로 무상관(Uncorrelated)인 몇 개의 주성분 변수로 요약된다.

### 2.2. 일반적인 계산 단계
1. **데이터 정규화**
	- 변수들의 스케일을 맞추기 위해 평균을 0, 분산을 1로 맞춘다.
	- (특히 평균을 0으로 맞추는 Centering 과정이 필수적이다.)

2. **공분산 행렬(Covariance Matrix) 계산**
	- 데이터의 공분산 행렬 $C$를 구한다.

3. **고유값 분해(Eigenvalue Decomposition)**
	- 공분산 행렬의 고유벡터와 고유값을 구한다.
	- **고유벡터(Eigenvector):** 주성분의 방향(축)을 의미한다.
	- **고유값(Eigenvalue):** 해당 주성분 축으로 사영했을 때의 데이터 분산의 크기를 의미한다.

4. **차원 축소**
	- 고유값이 큰 순서대로 상위 $k$개의 고유벡터를 선택하여 데이터를 해당 축으로 사영시킨다.

## 3. SVD와 PCA의 관계
| 구분         | SVD (특이값 분해)                            | PCA (주성분 분석)                         |
| :--------- | :-------------------------------------- | :----------------------------------- |
| **정의**     | 행렬을 $U, \Sigma, V^T$ 세 개로 나누는 대수적 분해 기법 | 데이터의 분산을 극대화하는 축을 찾는 통계적/머신러닝 기법     |
| **대상 행렬**  | 임의의 모든 $m \times n$ 행렬                  | 정규화된 데이터의 공분산 행렬 ($n \times n$ 정방행렬) |
| **중요 전처리** | 필수적이지 않음                                | 평균을 0으로 만드는 Centering이 필수적임          |
| **주요 목적**  | 행렬 근사, 노이즈 제거, 추천 시스템(CF)               | 데이터 시각화, 고차원 데이터의 차원 축소, 특징 추출       |

- SVD와 PCA는 개별적인 개념처럼 보이지만, 수학적으로 매우 깊이 연결되어 있다.
- 실제로 **PCA는 데이터 행렬에 SVD를 적용하여 아주 효율적으로 계산할 수 있다.**

### 3.1. 수학적 연결 고리
평균이 0으로 맞춰진(Centering이 완료된) 데이터 행렬을 $X$ (크기: $n \times d$, $n$은 데이터 개수, $d$는 변수 개수)라고 하겠다.

1. 데이터 행렬 $X$의 공분산 행렬 $C$는 다음과 같이 정의된다.
$$C = \frac{1}{n-1} X^T X$$

2. 이때 $X$에 SVD를 적용하면 다음과 같이 표현된다.
$$X = U \Sigma V^T$$

3. 이를 공분산 행렬 식에 대입해 본다.
$$C = \frac{1}{n-1} (U \Sigma V^T)^T (U \Sigma V^T)$$
$$C = \frac{1}{n-1} (V \Sigma^T U^T) (U \Sigma V^T)$$
$U$는 직교행렬이므로 $U^T U = I$가 되어 사라진다.
$$C = V \left( \frac{\Sigma^2}{n-1} \right) V^T$$

이 식의 형태는 공분산 행렬 $C$의 **고유값 분해(Eigenvalue Decomposition)** 형태와 정확히 일치한다.

### 3.2. 결론적인 매칭 관계
- **주성분(Principal Components, 사영할 축)**
	- SVD의 우특이벡터 행렬 $V$의 열 벡터들이 곧 PCA의 주성분 벡터(공분산 행렬의 고유벡터)이다.

- **고유값(Eigenvalues, 분산의 크기)**
	- PCA의 고유값 $\lambda_i$는 SVD의 특이값 $\sigma_i$와 다음 관계를 가진다.
	- $\lambda_i = \dfrac{\sigma_i^2}{n-1}$

### 3.3. 실제 구현에서의 차이점
- 이론상으로는 공분산 행렬을 만들어 고유값 분해를 하든, 원본 데이터에 SVD를 수행하든 결과는 같다.
- 하지만 수치 해석적인 안정성과 연산 효율성 때문에 scikit-learn 등 많은 머신러닝 라이브러리의 PCA는 **공분산 행렬을 직접 구하지 않고 데이터 행렬에 SVD(특히 Randomized SVD 등)를 직접 적용하여 주성분을 추출**하도록 구현되어 있다.

## 4. 큰 그림: 하나의 아이디어로 묶기
- SVD와 PCA는 모두 **대칭행렬($A^TA$ 또는 공분산 행렬)의 고유값·고유벡터**를 다루는 문제이다.
	- SVD는 이것으로 행렬을 분해해 데이터를 압축하고,
	- PCA는 같은 원리로 분산이 큰 방향을 찾아 차원을 줄인다.
	- 두 방법 모두 **"큰 고유값에 해당하는 방향이 가장 중요하다"** 는 동일한 아이디어 위에 서 있다.

### 4.1. SVD를 처음부터 유도하기
- **출발점: 선형변환을 가장 크게 만드는 방향 찾기**
	- 행렬 $A$를 선형변환으로 볼 때, 단위구면 위의 벡터 중 $\|Ax\|$를 최대로 만드는 방향을 찾는 것이 목적이다.
	- 크기는 항상 양수이므로 $\|Ax\|^2$을 최대화하면 되고,
	- $\|Ax\|^2 = (Ax)^T(Ax) = x^T(A^TA)x$로 쓸 수 있다.
	- 여기서 핵심은 **$A^TA$가 대칭행렬**이라는 점이다. (PCA에서 공분산 $S$가 대칭인 것과 같은 구조)

- **특이값의 정의**
	- 대칭행렬 $A^TA$의 고유값 $\lambda_i$와 정규직교 고유벡터 $v_i$에 대해 $\|Av_i\|^2 = \lambda_i \ge 0$ 이 성립한다.
	- 이때 $\sigma_i = \sqrt{\lambda_i}$ 를 행렬 $A$의 **특이값(singular value)** 이라 하고, 보통 큰 순서대로 정렬한다. ($\sigma_1 \ge \sigma_2 \ge \dots \ge 0$)

- **두 가지 핵심 정리**
	1. **열공간의 직교기저**
		- $A^TA$의 양의 고유값에 대응하는 고유벡터로부터 $\{Av_1, \dots, Av_r\}$을 만들면 이것이 $A$의 열공간의 직교기저가 되고, $\text{rank}(A) = r$ 이다.
	2. **SVD**
		- $\text{rank}(A)=r$ 인 $m\times n$ 행렬 $A$는 직교행렬 $U, V$를 이용해 $A = U\Sigma V^T$ 로 분해된다.
	- 여기서 분해를 만드는 핵심 관계식은 $u_i = \dfrac{1}{\sigma_i}Av_i$, 즉 **$Av_i = \sigma_i u_i$** 이며, 부족한 차원은 정규직교기저로 확장한다.

- **활용 — 이미지 압축**
	- 이미지를 행렬로 바꾼 뒤 SVD를 적용하고, 큰 특이값 몇 개만 남겨 행렬을 재결합하면 원본에 가까운 이미지를 적은 데이터로 압축·전송할 수 있다. (위 `cf. Truncated SVD`와 동일한 원리)

### 4.2. PCA의 기하학적 직관
- 데이터에 내재된 분산을 가장 잘 설명하는 **주성분(PC)** 을 찾는 것이 PCA이고, 각 PC는 공분산 행렬의 고유벡터, 대응하는 고유값이 클수록 더 많은 분산을 설명한다.
- 주성분의 개수는 변수의 개수와 같으며, 분산을 많이 설명하는 상위 몇 개만 골라 차원을 줄이거나 시각화에 쓴다.

- **타원형으로 퍼진 데이터에서**
	- ① 투영했을 때 가장 넓게 퍼지는(분산이 큰) 방향을 PC1로,
	- ② 그에 직교(orthogonal)하는 다음 방향을 PC2로 잡는다.
	- 각 PC는 원래 변수들의 선형결합($\text{PC1} = aX_1 + bX_2$)이며, 전체 분산을 나눠 설명한다.
	- (예: 전체 100 중 PC1=90, PC2=10)

- **SVD로 본 차원 축소**
	- 실수 행렬 $X = U\Sigma V^T$ 에서 **Truncated SVD**는 상위 $k$개의 특이값·특이벡터만 사용해 $X$를 근사한다.
	- $X' = U'\Sigma'$ 를 축소된 데이터로 사용해 $n$차원을 $k$(예: 2)차원으로 줄인다.
	- 직관적으로 $U, V$는 데이터 영역을 **회전**시키고 $\Sigma$는 **스케일**을 바꾼다. (§1.2의 기하학적 의미와 동일)

### 4.3. 몇 개의 주성분을 남길까 - 스크리 플롯
- 고유값을 큰 순서로 정렬한 뒤, 상위 $m$개가 설명하는 분산의 비율
$$\frac{\sum_{j=1}^{m} \lambda_j}{\sum_{i=1}^{d} \lambda_i}$$
을 기준으로 남길 주성분 수를 정한다.
- 이를 시각화한 것이 **스크리 플롯(scree plot)** 으로, 고유값이 급격히 꺾이는 지점(**elbow**)을 보고 차원 수를 결정한다.

## Python 실습
- [[A3_NumPy 선형대수 실습#2.2. 특이값 분해 (SVD)|np.linalg.svd로 특이값 분해하기]]
