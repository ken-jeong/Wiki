---
aliases: []
type: Lecture
tags:
  - 2-1/선형대수학
draft: false
date: 2025-03-10
---
## 1. 행렬
### 1.1. 행렬의 정의
$$A = [a_{mn}] =
\begin{pmatrix}
a_{11}& a_{12}& \cdots& a_{1n}\\
a_{21}& a_{22}& \cdots& a_{2n}\\
\vdots& \vdots& \ddots& \vdots\\
a_{m1}& a_{m2}& \cdots& a_{mn}
\end{pmatrix}$$

- **행렬 (Matrix)**
	- 수나 문자를 직사각형 모양으로 배열하여 괄호로 묶어 나타낸 것이다.
	- 어원은 라틴어 Mater(어머니)에서 왔다.
	
	1. **행**(Row)
		- 성분을 가로로 배열한 줄
		- 가로의 순서쌍이 **행벡터**(Row Vector)
	
	2. **열**(Column)
		- 성분을 세로로 배열한 줄 
		- 세로의 순서쌍이 **열벡터**(Column Vector)

- **행렬의 크기**
	- m개의 행과 n개의 열을 가지면 $m×n$ 행렬 또는 $(m, n)$ 행렬이라 부른다.

- **행렬의 성분**(Entry)
	- 행렬을 구성하는 각각의 수(또는 문자)

- **행렬 $A$의 $(i, j)$ 성분 $a_{ij}$** (ij-항)
	- 행렬 $A$의 $i$번째 행과 $j$번째 열이 만나는 위치에 놓인 성분

### 1.2. 행렬의 연산
- 행렬의 **상등**(Equal)
	- 두 행렬의 크기가 같고, 같은 위치로 대응하는 성분(항)이 모두 같으면 상등이다.

- 행렬의 **합과 차**
	- 두 행렬의 크기가 같을 때, 같은 위치로 대응하는 성분(항)끼리 더하거나 뺀다.
	
	- A−B는 A+(−1)B로 정의된다.
	
	- 교환·결합·항등(O 영행렬)·역원 법칙이 성립한다.

- 행렬의 **스칼라 곱**(실수배)
	- 행렬 $A$의 모든 성분에 실수 $k$를 곱한 것을 성분으로 갖는 행렬은 $kA$이다.

#### (1) 곱
- 행렬의 **곱**(Multiplication)
	- 행렬 $A$가 $m×n$ 행렬이고, 행렬 $B$가 $n×r$ 행렬일 때만 정의된다.
	- 이때 곱셈의 결과인 행렬 $AB$의 크기는 $m×r$이다.
	- 즉 **A의 열 수 = B의 행 수**라는 조건이 필요하다.
	
	- 행렬 $A$의 $i$번째 행과 행렬 $B$의 $j$번째 열을 곱한 합은 $AB$의 $(i, j)$ 성분($AB_{ij})$이다.
	
	- 부분행렬(submatrix)로 분할하여 블록 단위로 곱셈을 수행할 수 있다.

- 행렬의 **곱셈법칙**
	- **결합법칙**은 성립한다.
	- **왼쪽 분배법칙, 오른쪽 분배법칙**이 성립한다. *(교환법칙이 되지 않도록 주의한다.)*
	- **교환법칙은 일반적으로는 성립하지 않는다.** ($AB ≠ BA$)
		1. 단위행렬과의 연산에서는 곱셈의 교환법칙이 성립한다. ($AE=EA=A$)
		2. $A$가 $B$, $E$의 결합으로 나타나면 $AB=BA$가 성립한다.

- **영인자**
	- $A=O$ 또는 $B=O$이면 $AB=O$를 만족한다. (단, $O$는 영행렬)
	- 그러나 $AB=O$이라고 하여 반드시 $A=O$ 또는 $B=O$인 것은 아니다.
	- 이처럼, $A≠O$, $B≠O$이면서 $AB=O$를 만족시키는 행렬 $A$, $B$를 영인자라고 부른다.

- 행렬의 결합
	- $A\begin{pmatrix}a\\ b\end{pmatrix} = \begin{pmatrix}p\\ q\end{pmatrix}$, $A\begin{pmatrix}c\\ d\end{pmatrix} = \begin{pmatrix}r\\ s\end{pmatrix}$이면, $A\begin{pmatrix}a&c\\ b&d\end{pmatrix} = \begin{pmatrix}p&r\\ q&s\end{pmatrix}$이다.

#### (2) 거듭제곱
- 행렬의 **거듭제곱**
	- $A^{O} = I$
	1. $A^m=±E$가 되는 $m$값 찾기 (단, $E$는 단위행렬)
	2. 케일리-헤밀턴 정리를 이용한 행렬의 차수 줄이기
		- $A^2-tr(A)A+|A|E=O$ (단, $E$는 단위행렬, $O$는 영행렬)
		- *케일리-헤밀턴 정리의 역은 성립하지 않는다.*
	3. 독특한 성분배열을 가진 행렬의 거듭제곱
		- $A = \begin{pmatrix}a&0\\0&b\end{pmatrix}$일 때, $A^n = \begin{pmatrix}a^n&0\\0&b^n\end{pmatrix}$이다.
		- $A = \begin{pmatrix}1&a\\0&1\end{pmatrix}$일 때, $A^n = \begin{pmatrix}1&na\\0&1\end{pmatrix}$이다.
		- $A = \begin{pmatrix}b&1\\0&b\end{pmatrix}$일 때, $A^n = \begin{pmatrix}b^n&nb^{-1}\\0&b^n\end{pmatrix}$이다.

### cf. 행렬의 곱셈의 4가지 관점
- $A=\begin{bmatrix}1&2\\3&4\end{bmatrix}$
- $B=\begin{bmatrix}5&6\\7&8\end{bmatrix}$

#### (1) 내적의 관점
- 행렬 곱 AB의 각 원소는 **A의 행과 B의 열의 내적**(닮음의 정도)이다.
	- $(AB)_{11}=\begin{bmatrix}1&2\end{bmatrix}·\begin{bmatrix}5\\7\end{bmatrix}=19$
	- $(AB)_{12}=\begin{bmatrix}1&2\end{bmatrix}·\begin{bmatrix}6\\8\end{bmatrix}=22$
	- $(AB)_{21}=\begin{bmatrix}3&4\end{bmatrix}·\begin{bmatrix}5\\7\end{bmatrix}=43$
	- $(AB)_{22}=\begin{bmatrix}3&4\end{bmatrix}·\begin{bmatrix}6\\8\end{bmatrix}=50$
	- $AB=\begin{bmatrix}19&22\\43&50\end{bmatrix}$

#### (2) 외적의 관점 (rank-1 matrix의 합)
- 행렬 곱 AB는 A의 열 벡터와 B의 행 벡터의 외적(outer product) 합으로 표현된다.
	- A의 열: $a_1=\begin{bmatrix}1\\3\end{bmatrix}$, $a_2=\begin{bmatrix}2\\4\end{bmatrix}$
	- B의 행: $b_1^T=\begin{bmatrix}5&6\end{bmatrix}$, $b_2^T=\begin{bmatrix}7&8\end{bmatrix}$
	- $AB=a_1b_1^T+a_2b_2^T=\begin{bmatrix}5&6\\15&18\end{bmatrix}+\begin{bmatrix}14&16\\28&32\end{bmatrix}=\begin{bmatrix}19&22\\43&50\end{bmatrix}$

#### (3) row space
- 행렬 곱 AB의 각 행은 B의 행 벡터의 선형결합이다.
	- A의 첫 번째 행: $a_1^T=\begin{bmatrix}1&2\end{bmatrix}:$
		- $AB_{1:}=1×\begin{bmatrix}5&6\end{bmatrix}+2×\begin{bmatrix}7&8\end{bmatrix}=\begin{bmatrix}19&22\end{bmatrix}$
	- A의 두 번째 행: $a_2^T=\begin{bmatrix}3&4\end{bmatrix}:$
		- $AB_{2:}=3×\begin{bmatrix}5&6\end{bmatrix}+4×\begin{bmatrix}7&8\end{bmatrix}=\begin{bmatrix}43&50\end{bmatrix}$

![[06_Vector Space#(1) row space]]

#### (4) column space
- 행렬 곱 AB의 각 열은 A의 열 벡터의 선형결합이다.
	- B의 첫 번째 열: $b_{:1}=\begin{bmatrix}5\\7\end{bmatrix}:$
		- $AB_{:1}=5×\begin{bmatrix}1\\3\end{bmatrix}+7×\begin{bmatrix}2\\4\end{bmatrix}=\begin{bmatrix}19\\43\end{bmatrix}$
	- B의 두 번째 열: $b_{:2}=\begin{bmatrix}6\\8\end{bmatrix}:$
		- $AB_{:2}=6×\begin{bmatrix}1\\3\end{bmatrix}+8×\begin{bmatrix}2\\4\end{bmatrix}=\begin{bmatrix}22\\50\end{bmatrix}$

![[06_Vector Space#(2) column space]]

## 2. 특수한 행렬
| 종류                | 특징                           |
| ----------------- | ---------------------------- |
| **대각행렬**          | 주대각선 외 모든 항이 0               |
| **항등행렬(단위행렬) Iₙ** | 대각선이 모두 1, 나머지 0 ($AIₙ = A$) |
| **영행렬 O**         | 모든 성분이 0 ($A+O = A$)         |
| **전치행렬 Aᵀ**       | 행과 열을 서로 바꾼 행렬               |
| **대칭행렬**          | $A = Aᵀ$ (aᵢⱼ = aⱼᵢ)         |
| **교대행렬**          | $A = −Aᵀ$ (aᵢⱼ = −aⱼᵢ)       |
| **삼각행렬**          | 상부/하부삼각행렬, 한쪽 항이 모두 0        |

### 2.1. 정방행렬
- **장방행렬** (rectangular matrix)
	- $m×n$ 행렬 중에서 $m≠n$을 만족하는 행렬

- **$n$차 정방행렬** (Square Matrix of Order $n$)
	- $m×n$ 행렬 중에서 $m=n$을 만족하는 행렬
	- 즉, 행과 열의 개수가 같은 $n×n$ 행렬

#### (1) 주대각선 & 대각합
- **주대각선 성분/원소 (Diagonal Entry)**
	- 정방행렬에 대하여 행과 열의 위치번호가 같은 성분들 ($a_{11}, a_{22}, …$)

- **대각항**
	- 주대각선 위의 모든 성분들

- **주대각선 (Main Diagonal)**
	- 주대각선 성분들이 놓인 선

- **대각합 (Trace)**
	- 행렬 $A$의 주대각선 성분들의 합 ($\sum_{i=1}^na_{ii}$)
	- $Tr(A)$, $tr(A)$, $trace(A)$

- **Properties of Trace**
	- **선형성**을 만족한다.
		1. $tr(A±B)=tr(A)±tr(B)$ (덧셈에 대한 분해)
		2. $tr(cA)=c·tr(A)$ (스칼라 $c$)
	- $tr(AB)=tr(BA)$, $tr(ABC)=tr(CAB)=tr(BCA)$ (곱에 대한 성질 + cyclic property)
	- $tr(A)=tr(A^T)$ (전치와의 관계)
	- $tr(I_n)=n$ (항등행렬과의 관계)

#### (2) 삼각행렬
- **삼각행렬 (Triangular Matrix)**
	- 정방행렬의 ==주대각선 성분들을 연결한 선을 기준==으로,
	1. **상 삼각행렬**(Upper Triangular Matrix; $U$)
		- 위가 삼각형인 행렬 (아래는 전부 0)
	2. **하 삼각행렬**(Lower Triangular Matrix; $L$)
		- 아래가 삼각형인 행렬 (위는 전부 0)

- **삼각행렬의 곱**
	- $L_1×L_2=L_3$, $U_1×U_2=U_3$
	- 상/하 삼각행렬꼴로 만든 후에 계산을 하면 빠르다.

#### (3) 대각행렬
- **대각행렬 (Diagonal Matrix; $D$)**
	- n차 정방행렬 $A$의 주대각선 성분 이외의 모든 성분이 0인 행렬
	- (Diagonal Entry를 제외한 나머지 Entry가 0인 행렬)
	
	- 대각행렬은 대칭행렬이다. ($A=A^T$)

- **대각행렬의 곱**
	- $D_1×D_2=D_3$
	- $A$가 대각행렬이면 $A^n$도 대각행렬이다.

- 대각행렬과 임의의 행렬을 곱할 때
	- 곱 위치에 따라 행 배수(또는 열 배수)가 결정된다.
	- 대각행렬 $A$를 $B$ 앞에 곱할 경우 → 각 행의 배수
	- 대각행렬 $A$를 $B$ 뒤에 곱할 경우 → 각 열의 배수

-  *cf. 길이만 변하는 선형변환*
	- **선형변환**: 덧셈과 스칼라곱에 대해 보존되는 **함수**
	- 대각행렬에서의 **고유값**($λ$): 대각행렬의 주대각 원소
	- 대각행렬에서의 **고유벡터**: 단위벡터 $e_n$

#### (4) 항등/단위행렬
- **항등/단위행렬 (Identity/Unit Matrix; $I_n$)**
	- 주대각선 성분이 모두 1이고 이외의 성분이 모두 0인 행렬
	- 단위행렬 $I$는 단위실수 1과 성질이 유사하다.

### 2.2. 전치행렬
- **전치행렬 (Transpose Matrix; $A^T$)**
	- 행과 열을 서로 바꾸어 만든 행렬

- **전치행렬의 성질 1**
	- $(A^T)^T=A$
	- $(kA)^T=kA^T$ (단, $k$는 실수)
	- $(A±B)^T=A^T±B^T$ (일반화 가능)
	- $(AB)^T=B^TA^T$ (일반화 가능)

- **전치행렬의 성질 2**
	- $det(A^T)=det(A)$
	- $(A^T)^{-1}=(A^{-1})^T=A^{-T}$

- **자신과 전치의 곱** ($AA^T$ 또는 $A^TA$)
	- 벡터의 내적으로 빠르게 구할 수 있다.
	- $AA^T$와 $A^TA$는 대칭 행렬이다.
	- *cf. 장방행렬 → 셀프 전치 곱 → 정방행렬 → 역행렬*

#### (1) 대칭행렬
- **대칭행렬 (Symmetric Matrix)**
	- $A = A^T$를 만족하는 정방행렬 $A$를 대칭행렬이라 한다.

- **대칭행렬의 성질**
	- $PP^T$는 대칭행렬이다.
	- $P^TP$는 대칭행렬이다.
	- $P+P^T$는 대칭행렬이다.
	
	- $A$가 대칭행렬이면, $A^{-1}$도 대칭행렬이다.
	- $A$가 대칭행렬이면, $A^n$도 대칭행렬이다.
	- $A$가 대칭행렬이면, $kA$도 대칭행렬이다
	
	- 두 대칭행렬 $A$와 $B$에 대하여 $A+B$, $A-B$도 대칭행렬이다.
	- 두 대칭행렬 $A$와 $B$에 대하여 $AB$가 항상 대칭행렬인 것은 아니다.
	- 두 대칭행렬 $A$와 $B$에 대하여 $AB=BA$이면 $AB$는 대칭행렬이다.
	
	- 성분이 모두 실수인 대칭행렬 $A$의 고유치는 실수이다.
	- 대칭행렬의 서로 다른 고유치에 대응하는 고유벡터는 서로 수직이다.

#### (2) 교대행렬
- **교대행렬 (반대칭행렬; Skew-symmetric Matrix)**
	- $A = -A^T$를 만족하는 정방행렬 $A$를 교대행렬이라 한다.
	- 교대행렬의 주대각성분은 모두 0이다.

- **교대행렬의 성질**
	- $P-P^T$는 교대행렬이다.
	- $A$가 교대행렬이고 역행렬이 존재하면 $A^{-1}$도 교대행렬이다.
	- $A$가 교대행렬이라고 하여, $A^n$이 항상 교대행렬인 것은 아니다.
	- 교대행렬 $A$가 홀수 차 정방행렬이면 행렬식은 항상 0이다.

- **대칭행렬과 교대행렬의 합**
	- 모든 정방행렬은 대칭행렬과 교대행렬의 합으로 나타낼 수 있다.
	- $P = \dfrac{P+P^T}{2} + \dfrac{P-P^T}{2}$가 항상 성립한다.

#### (3) 직교행렬
- **직교행렬 (Orthogonal Matrix; $Q$)**
	- $QQ^T=Q^TQ=I$를 만족하는 정방행렬 $Q$
	- 단위행렬 또는 단위행렬의 행/열을 뒤섞은 **순열행렬**(permutation matrix)은 항상 직교행렬이다.
	- **유니터리행렬**(unitary matrix): 복소수(complex numbers)를 다루는 직교행렬

### 2.3. 영행렬
- **영행렬 (Zero Matrix)**
	- 모든 성분이 0인 행렬

## 3. 행렬의 기본 연산과 사다리꼴
### 3.1. 행렬의 기본 행 연산
- **기본 행 연산 3가지**
	- ① 두 행 교환,
	- ② 한 행에 0이 아닌 상수 곱하기,
	- ③ 한 행에 상수를 곱해 다른 행에 더하기

- 이로 만들어진 행렬을 **행 동치**(row equivalent)라 한다.

### 3.2. 행 사다리꼴 (REF)
- **행 사다리꼴(REF)**과 **기약 행 사다리꼴(RREF)**
	- 피벗(각 행에서 처음 나오는 0이 아닌 수)을 기준으로 정리한 형태이다.
	- 이를 구하는 방법이 **가우스 소거법**(전향단계만)과 **가우스-조단 소거법**(후향단계까지)이다.

- **계수(rank)**
	- 행 사다리꼴로 만들었을 때 0이 아닌 행의 개수 = 피벗의 개수이다.

- **응용**
	- 그래프의 인접행렬, 최단 경로 문제, 패스트푸드 세트메뉴 재료 계산, 선형 변환(x축/y축 반사, 원점 대칭) 등에 활용된다.

## 4. 인공지능과 행렬
- 신경망에서 뉴런의 입력값 Xᵢ와 연결강도(weight) Wᵢ를 곱하고 합하는 연산이 핵심인데, 이것이 곧 행렬의 곱(또는 벡터의 내적)이다.
- 따라서 행렬 연산은 인공지능에서 필수적이며, 그 외에도 선형방정식 표현, 재무 분석, GPS, 네트워크 관리 등 생활 전반에 응용된다.
