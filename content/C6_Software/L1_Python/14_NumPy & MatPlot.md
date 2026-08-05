---
aliases: []
type: Lecture
tags:
  - 1-1/프로그래밍기초
draft: false
date: 2022-06-09
---
## 1. MatPlot(데이터 시각화)
> 파이썬에서 그래프를 그리기 위한 표준 라이브러리로,
> MATLAB과 유사한 기능을 제공하며 무료 오픈소스이다.

- **기본 사용법**
	- `import matplotlib.pyplot as plt`

- **그래프 종류**
	- **선 그래프**(Line Plot): `plt.plot(x, y)`
		- 스타일 지정: 색상, 마커(점), 선 스타일 등을 지정 가능 (예: `"sm"`은 사각형 마커, 자홍색)
		- 옵션: 축 레이블(`xlabel`, `ylabel`), 제목(`title`), 범례(`legend`) 설정 가능
	- **막대 그래프**(Bar Chart): `plt.bar(x, y)`
	- **3차원 그래프**: `projection='3d'`를 사용하여 3차원 데이터를 시각화

- **다중 그래프**: 하나의 축에 여러 개의 선을 동시에 그리거나, 2차 함수($y=x^2$) 등을 시각화할 수 있다.

## 2. NumPy 기초(수치 계산)
> 행렬(Matrix) 계산을 위한 파이썬 핵심 라이브러리이다.

- **특징**
	- 대량의 데이터를 처리할 때 파이썬 리스트보다 **속도가 훨씬 빠르고 메모리 효율적**이다.
	- scikit-learn, tensorflow 등 인공지능 라이브러리의 기반이 된다.
	- 데이터가 연속된 메모리 공간에 저장된다.

- **배열 생성**
	- `np.array([리스트])`: 리스트를 넘파이 배열로 변환
	- `np.arange(start, stop, step)`: 일정 간격의 배열 생성
	- `np.linspace(start, stop, count)`: 시작과 끝 사이를 등간격으로 나눈 배열 생성

## 3. NumPy 연산 및 기능
- **브로드캐스팅**(Broadcasting)
	- 배열의 모든 요소에 동일한 연산을 한 번에 적용하는 기능이다.
	- (예: `F - 32`, 배열 간 덧셈 등)

- **인덱싱과 슬라이싱**
	- 기본 슬라이싱: `arr[start:end]`
	- **논리적 인덱싱**(Boolean Indexing)
		- 조건에 맞는 데이터만 추출하는 강력한 기능 (예: `ages[ages > 20]`)

- **난수 생성**(Random)
	- `np.random.rand()`: 0~1 사이 균일 분포 난수
	- `np.random.randn()`: 표준 정규 분포 난수 (평균 0, 표준편차 1)
	- `np.random.normal(mu, sigma, n)`: 평균($\mu$)과 표준편차($\sigma$)를 지정한 정규 분포 생성

- **통계 함수**
	- `sum()`(합계), `mean()`(평균), `std()`(표준편차), `var()`(분산), `min()`, `max()`
	- (축 `axis`를 지정하여 행/열 단위 계산 가능)

## 4. 선형대수 (Linear Algebra)
> NumPy는 선형대수 계산을 위한 서브 모듈(`linalg`)을 제공한다.

- **전치 행렬**: `.transpose()` 또는 `.T` 속성 사용
- **대각행렬** (Diagonal matrix): `np.diag(x)`
- **내적** (Dot product, Inner product): `np.dot(a, b)`
- **대각합** (Trace): `np.trace(x)`

- **행렬식** (Matrix Determinant): `np.linalg.det(x)`
- **역행렬** (Inverse of a matrix): `np.linalg.inv(x)`
- **고유값** (Eigenvalue), **고유벡터** (Eigenvector): `w, v = np.linalg.eig(x)`
- **특이값 분해** (Singular Value Decomposition): `u, s, vh = np.linalg.svd(A)`
- **연립방정식 해 풀기** (Solve a linear matrix equation): `np.linalg.solve(a, b)`를 사용하여 미지수 계산 ($Ax = B$ 꼴 선형 방정식)
- **최소자승 해 풀기** (Compute the Least-squares solution): `m, c = np.linalg.lstsq(A, y, rcond=None)[0]`
