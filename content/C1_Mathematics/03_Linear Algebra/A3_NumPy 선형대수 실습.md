---
aliases: []
type: Lecture
tags:
  - 2-1/선형대수학
draft: false
date: 2025-03-28
---
## 요약
- NumPy/SciPy로 행렬식·역행렬·대각합·고유값·SVD·연립방정식·최소자승·노름을 계산하는 실습 정리
- 참고: <https://docs.scipy.org/doc/scipy/reference/linalg.html#module-scipy.linalg>

**[ Python NumPy ] 선형대수 (Linear Algebra)** — 이 단원에서 다루는 함수 목록

| 개념 | 영문 | 함수 |
|---|---|---|
| 대각행렬 | Diagonal matrix | `np.diag(x)` |
| 내적 | Dot product, Inner product | `np.dot(a, b)` |
| 대각합 | Trace | `np.trace(x)` |
| 행렬식 | Matrix Determinant | `np.linalg.det(x)` |
| 역행렬 | Inverse of a matrix | `np.linalg.inv(x)` |
| 고유값·고유벡터 | Eigenvalue, Eigenvector | `w, v = np.linalg.eig(x)` |
| 특이값 분해 | Singular Value Decomposition | `u, s, vh = np.linalg.svd(A)` |
| 연립방정식 해 풀기 | Solve a linear matrix equation | `np.linalg.solve(a, b)` |
| 최소자승 해 풀기 | Least-squares solution | `m, c = np.linalg.lstsq(A, y, rcond=None)[0]` |

## 1. 행렬의 기본 계산
- 관련: [[03_Determinant]], [[02_Matrix]]

### 1.1. 행렬식 (det)
```python
import numpy as np

a = [2, 1, 1, 4]
A = np.array(a)
A                     # array([2, 1, 1, 4])

A = A.reshape(2, 2)
A                     # array([[2, 1],
                      #        [1, 4]])

np.linalg.det(A)      # 7.000000000000001
```
> 1차원 배열을 `reshape`로 2차원 행렬로 바꾼 뒤에야 행렬식 계산이 가능하다.
> 부동소수점 오차 때문에 정확히 `7`이 아니라 `7.000000000000001`이 나오는 점에 주의.

```python
d = np.array([[1, 2], [3, 4]])
np.linalg.det(d)          # -2.0
```

### 1.2. 역행렬 (inv)
```python
b = [3, 1, 0, 2, -1, 1, 5, 5, -7]
B = np.array(b)
B = B.reshape(3, 3)   # array([[ 3,  1,  0],
                      #        [ 2, -1,  1],
                      #        [ 5,  5, -7]])

np.linalg.inv(B)
# array([[ 0.08,  0.28,  0.04],
#        [ 0.76, -0.84, -0.12],
#        [ 0.6 , -0.4 , -0.2 ]])
```

```python
e = np.array(range(4)).reshape(2, 2)   # [[0, 1],
                                       #  [2, 3]]
e_inv = np.linalg.inv(e)
e_inv
# array([[-1.5,  0.5],
#        [ 1. ,  0. ]])
```

### 1.3. 대각행렬 (diag)
```python
x = np.arange(9).reshape(3, 3)
x
# array([[0, 1, 2],
#        [3, 4, 5],
#        [6, 7, 8]])

np.diag(x)              # array([0, 4, 8])       ← 행렬 → 대각 원소 추출
np.diag(np.diag(x))     # array([[0, 0, 0],
                        #        [0, 4, 0],
                        #        [0, 0, 8]])     ← 1차원 → 대각행렬 생성
```
> **핵심**: `np.diag`는 인자가 2차원이면 *대각 원소를 뽑고*, 1차원이면 *대각행렬을 만든다*.
> 두 번 적용하면 원 행렬의 대각 성분만 남는 행렬이 된다.

### 1.4. 내적 (Dot product)
```python
a = np.arange(4).reshape(2, 2)   # [[0, 1],
                                 #  [2, 3]]

a * a            # array([[0, 1],      ← 원소별(element-wise) 곱
                 #        [4, 9]])

np.dot(a, a)     # array([[ 2,  3],    ← 행렬 곱
                 #        [ 6, 11]])

a.dot(a)         # array([[ 2,  3],    ← 메서드 형태, 결과 동일
                 #        [ 6, 11]])
```
> **주의**: `*`는 행렬 곱이 아니라 원소별 곱이다.
> 행렬 곱은 `np.dot`, `a.dot(b)`, 또는 `np.matmul` / `@`.

### 1.5. 대각합 (Trace)
```python
b = np.arange(16).reshape(4, 4)
b
# array([[ 0,  1,  2,  3],
#        [ 4,  5,  6,  7],
#        [ 8,  9, 10, 11],
#        [12, 13, 14, 15]])

np.trace(b)      # 30   (= 0 + 5 + 10 + 15)

c = np.arange(27).reshape(3, 3, 3)
np.trace(c)      # array([36, 39, 42])
```
> 2차원에서는 스칼라(대각 원소의 합), 3차원 이상에서는 앞 두 축을 따라 합해 **배열**이 반환된다.

## 2. 고유값·고유벡터와 SVD
- 관련: [[07-1_고유벡터와 고유값]], [[07-3_SVD & PCA]]

### 2.1. 고유값과 고유벡터 (eig)
```python
f = np.array([[4, 2], [3, 5]])
f
# array([[4, 2],
#        [3, 5]])

w, v = np.linalg.eig(f)

w   # array([2., 7.])            ← 고유값
v   # array([[-0.70710678, -0.5547002 ],
    #        [ 0.70710678, -0.83205029]])   ← 고유벡터(열 단위)

v[:, 0]   # array([-0.70710678,  0.70710678])   ← w[0]=2 에 대응하는 고유벡터
v[:, 1]   # array([-0.5547002 , -0.83205029])   ← w[1]=7 에 대응하는 고유벡터
```
> **핵심**: 고유벡터는 `v`의 **열(column)** 이다.
> `v[:, i]`가 `w[i]`에 대응하며, 각 벡터는 길이 1로 정규화되어 있다.

### 2.2. 특이값 분해 (SVD)
```python
A = np.array([[3, 6], [2, 3], [0, 0], [0, 0]])
A
# array([[3, 6],
#        [2, 3],
#        [0, 0],
#        [0, 0]])

u, s, vh = np.linalg.svd(A)

u
# array([[-0.8816746 , -0.47185793,  0.,  0.],
#        [-0.47185793,  0.8816746 ,  0.,  0.],
#        [ 0.        ,  0.        ,  1.,  0.],
#        [ 0.        ,  0.        ,  0.,  1.]])

s     # array([7.60555128, 0.39444872])   ← 특이값(1차원 배열로 반환)

vh
# array([[-0.47185793, -0.8816746 ],
#        [ 0.8816746 , -0.47185793]])
```
> $A_{4×2} = U_{4×4} · Σ_{4×2} · V^T_{2×2}$
> `s`는 대각행렬이 아니라 **특이값 1차원 배열**로 반환되므로, 복원하려면 별도로 $Σ$를 구성해야 한다.

## 3. 선형방정식의 해 (p.174)
- 관련: [[04_선형방정식의 해법과 응용]]

### 3.1. 연립방정식 해 풀기 (solve)
```python
a = np.array([[4, 3], [3, 2]])
a
# array([[4, 3],
#        [3, 2]])

b = np.array([23, 16])
b                        # array([23, 16])

x = np.linalg.solve(a, b)
x                        # array([2., 5.])

np.allclose(np.dot(a, x), b)   # True   ← 검산
```
> 즉 $4x + 3y = 23,\ 3x + 2y = 16$ → $x = 2,\ y = 5$.
> `np.allclose`로 $A·x ≈ b$인지 확인하는 것이 검산 관례.

### 3.2. 최소자승 해 (Least-squares solution)
```python
x = np.array([0, 1, 2, 3])
x                        # array([0, 1, 2, 3])

y = np.array([-1, 0.2, 0.9, 2.1])
y                        # array([-1. ,  0.2,  0.9,  2.1])

A = np.vstack([x, np.ones(len(x))]).T
A
# array([[0., 1.],
#        [1., 1.],
#        [2., 1.],
#        [3., 1.]])

m, c = np.linalg.lstsq(A, y, rcond=None)[0]
# m = 1.0 (기울기), c = -0.95 (절편)
```
> 직선 $y = mx + c$에 대한 **회귀(직선 적합)** 문제 → [[04_선형방정식의 해법과 응용#3.3. 최소제곱 직선|최소제곱 직선]]
> `np.vstack([x, np.ones(len(x))]).T`로 설계행렬(design matrix)을 만드는 패턴이 핵심.
> `rcond=None`은 경고 방지를 위한 최신 권장 설정. `[0]`은 반환 튜플 중 해(solution) 부분.

## 4. SciPy `scipy.linalg` 활용
### 4.1. 행렬의 곱셈
```python
import numpy as np
import scipy.linalg as linalg

A = np.array([[1, 2, -1],
              [2, 7,  4],
              [0, 4, -1]])
b = np.array([1, 0, 1.2])

# matrix-vector multiplication (세 가지 방법, 결과 동일)
y1 = np.matmul(A, b)
y2 = np.dot(A, b)
y3 = A.dot(b)

y1    # array([-0.2,  6.8, -1.2])
```

### 4.2. determinant, solve, inverse
```python
# determinant
det = linalg.det(A)
det                       # -27.0

# solve
x = linalg.solve(A, b)
r = A.dot(x) - b          # check 0 vector

x     # array([ 0.18518519,  0.19259259, -0.42962963])
r     # array([0., 0., 0.])   ← 잔차가 0 벡터이면 해가 맞음

# inverse
Ainv = linalg.inv(A)
Ainv
# array([[ 0.85185185,  0.07407407, -0.55555556],
#        [-0.07407407,  0.03703704,  0.22222222],
#        [-0.2962963 ,  0.14814815, -0.11111111]])
```
> **포인트**: `numpy.linalg`와 `scipy.linalg`는 이름이 거의 같지만, SciPy 쪽이 기능이 더 많고 항상 LAPACK을 사용한다.
> 해를 구한 뒤 **잔차 $A·x − b$가 영벡터인지 확인**하는 습관을 들일 것.

## 5. 노름 (norm)
- 관련: [[05_Vector#1.3. 벡터의 크기(노름; norm)|벡터의 크기(노름)]]

### 5.1. 벡터 노름의 종류
```python
# vector norm
norm1   = linalg.norm(x, 1)          # L1 norm  == sum(np.abs(x))
norm2   = linalg.norm(x)             # L2 norm  == np.sqrt(sum(x*x))
# normp = linalg.norm(x, p)          # p norm
normMax = linalg.norm(x, np.inf)     # max norm == np.max(abs(x))
```

| 노름 | 호출 | 동치 계산 |
|---|---|---|
| L1 | `linalg.norm(x, 1)` | `sum(np.abs(x))` |
| L2 (기본값) | `linalg.norm(x)` | `np.sqrt(sum(x*x))` |
| p-노름 | `linalg.norm(x, p)` | — |
| max(무한대) 노름 | `linalg.norm(x, np.inf)` | `np.max(abs(x))` |

### 5.2. 행렬에 축(axis) 지정하여 행별 노름 구하기
```python
import numpy as np
x = np.random.randint(low=1, high=10, size=(5, 3))
print(x)
# [[5 1 7]
#  [9 8 5]
#  [9 8 5]
#  [5 2 1]
#  [7 1 6]]

L1_norm = np.linalg.norm(x, axis=1, ord=1)
print(L1_norm)
print('shape:', L1_norm.shape)
# [13. 22. 22.  8. 14.]
# shape: (5,)

L2_norm = np.linalg.norm(x, axis=1, ord=2)
print(L2_norm)
print('shape:', L2_norm.shape)
# [8.66025404 13.03840481 13.03840481 5.47722558 9.2736185]
# shape: (5,)
```
> `axis=1` → 각 **행**마다 노름 계산 → 결과 shape `(5,)`.
> `ord=1`은 L1, `ord=2`는 L2. (난수를 쓰므로 실행할 때마다 값은 달라짐)

## 6. 요약 치트시트
```python
np.diag(x)                          # 대각 추출 / 대각행렬 생성
np.dot(a, b) / a.dot(b) / a @ b     # 행렬 곱  (a*b 는 원소별 곱!)
np.trace(x)                         # 대각합
np.linalg.det(x)                    # 행렬식
np.linalg.inv(x)                    # 역행렬
w, v = np.linalg.eig(x)             # 고유값 w, 고유벡터 v (열 기준)
u, s, vh = np.linalg.svd(A)         # 특이값 분해
x = np.linalg.solve(a, b)           # 연립방정식
np.allclose(a.dot(x), b)            # 해 검산
m, c = np.linalg.lstsq(A, y, rcond=None)[0]   # 최소자승(직선 적합)
np.linalg.norm(x, axis=1, ord=1)    # 노름 (ord=1,2,np.inf)
import scipy.linalg as linalg       # SciPy 버전: det, solve, inv, norm ...
```
