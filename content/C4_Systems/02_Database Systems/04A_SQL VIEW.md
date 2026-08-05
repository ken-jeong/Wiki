---
aliases: []
type: Lecture
tags:
  - 2-2/데이터베이스
draft: false
date: 2025-11-17
---
## 1. VIEW
- **뷰(VIEW)**
	- 하나 이상의 테이블에서 유도된 가상의 테이블
	- 물리적으로 데이터를 저장하지 않고, SELECT 쿼리문 자체를 데이터베이스에 저장해 두었다가 호출 시점에 결과를 생성하여 보여주는 논리적인 테이블

### 1.1. 뷰 정의
#### (1) 단일 테이블 뷰 생성
- `EMPLOYEE` 테이블에서 3번 부서에 근무하는 사원들의 사원번호, 이름, 직책을 보여주는 `EMP_DNO3` 뷰를 생성
```sql
-- 뷰 생성
CREATE OR REPLACE VIEW EMP_DNO3 (ENO, ENAME, TITLE)
AS SELECT EMPNO, EMPNAME, TITLE
   FROM EMPLOYEE
   WHERE DNO = 3;

-- 뷰 조회
SELECT ENO, ENAME, TITLE FROM EMP_DNO3;
```

#### (2) 두 테이블 조인 뷰 생성
- `EMPLOYEE` 테이블과 `DEPARTMENT` 테이블을 조인하여 '기획' 부서에 근무하는 사원들의 이름, 직책, 급여를 보여주는 `EMP_PLANNING` 뷰를 생성
```sql
-- 뷰 생성
CREATE OR REPLACE VIEW EMP_PLANNING
AS SELECT E.EMPNAME, E.TITLE, E.SALARY
   FROM EMPLOYEE E, DEPARTMENT D
   WHERE E.DNO = D.DEPTNO AND D.DEPTNAME = '기획';

-- 뷰 조회
SELECT * FROM EMP_PLANNING;
```

### 1.2. 뷰의 장점
1. **쿼리 단순화**: 복잡한 쿼리를 미리 정의해 두어, 간단한 호출만으로 쉽게 조회할 수 있다.
2. **데이터 무결성**: WITH CHECK OPTION 등을 통해 조건에 위배되는 데이터 변경을 막아 무결성을 유지한다.
3. **데이터 독립성**: 원본 테이블 구조가 바뀌어도 뷰를 사용하는 응용 프로그램은 수정할 필요가 없다.
4. **보안 강화**: 사용자에게 필요한 데이터만 선별적으로 보여주어, 중요한 정보에 대한 직접 접근을 차단한다.

#### (1) `WITH CHECK OPTION`을 이용한 뷰 생성
- 3번 부서 사원만 보여주며, 뷰를 통한 데이터 수정 시 부서 번호(DNO)가 3번이 아닌 값으로 변경되지 않도록 하는 `EMP_DNO3` 뷰를 생성
```sql
CREATE OR REPLACE VIEW EMP_DNO3 (ENO, DNO, ENAME, TITLE)
AS SELECT EMPNO, DNO, EMPNAME, TITLE
   FROM EMPLOYEE
   WHERE DNO=3
   WITH CHECK OPTION;
```

#### (2) 뷰를 통한 데이터 수정 시도 (실패 예제)
- `WITH CHECK OPTION`으로 생성된 `EMP_DNO3` 뷰의 데이터를 조건에 맞지 않는 값(DNO = 2)으로 변경을 시도하면 오류가 발생
```sql
UPDATE EMP_DNO3
SET DNO = 2
WHERE ENO = 3427;
```

### 1.3. 뷰의 생성, 수정, 삭제
- **생성**
	- `CREATE VIEW 뷰이름 AS SELECT 문;` 형식으로 생성한다.
	- 기존에 동일한 이름의 뷰가 있을 경우를 대비해 `CREATE OR REPLACE VIEW`를 사용하면 편리하게 수정할 수 있다.

- **수정**
	- 대부분의 DBMS는 `ALTER VIEW`를 지원하지 않으므로, `CREATE OR REPLACE VIEW`를 사용하거나 기존 뷰를 `DROP` 한 후 다시 `CREATE` 하는 방식으로 수정한다.

- **삭제**
	- `DROP VIEW 뷰이름;` 명령어로 뷰를 삭제할 수 있다.

### 1.4. 뷰의 갱신(INSERT, UPDATE, DELETE)
> 뷰에 대한 데이터 갱신은 기본 테이블의 데이터를 변경하는 것을 의미하지만, 모든 뷰가 갱신 가능한 것은 아니다. 다음과 같은 경우 갱신에 제약이 따른다.

- **갱신 불가능한 뷰**
	1. 기본 테이블의 **기본 키**(PK)**가 뷰에 포함되지 않은 경우
	2. 기본 테이블의 **`NOT NULL` 속성**이 뷰에서 빠져 있는 경우
	3. 합계, 평균 등 **집단 함수**가 사용된 경우
	4. **조인**(JOIN)을 통해 여러 테이블이 연결된 경우

#### (1) 조인 뷰에 대한 INSERT 시도 (실패 예제)
- `EMP_PLANNING` 뷰에 데이터를 삽입하려고 하면, 기본 키(`EMPNO`) 값이 없어 오류가 발생
```sql
INSERT INTO EMP_PLANNING
VALUES ('박지선', '대리', 2500000);
```

#### (2) 집계 함수를 포함한 뷰 생성
- 부서별 평균 급여를 보여주는 `EMP_AVGSAL` 뷰를 생성 (이러한 뷰는 갱신이 불가능)
```sql
CREATE OR REPLACE VIEW EMP_AVGSAL(DNO, AVGSAL)
AS SELECT DNO, AVG(SALARY)
   FROM EMPLOYEE
   GROUP BY DNO;
```

#### (3) 사원 이름과 부서 이름 뷰 생성
- 사원 이름과 소속된 부서의 이름을 함께 출력하는 `EMP_DEPT` 뷰를 생성
```sql
-- 뷰 생성
CREATE OR REPLACE VIEW EMP_DEPT (NAME, DEPTNM)
AS SELECT EMPNAME, DEPTNAME
   FROM EMPLOYEE, DEPARTMENT
   WHERE DNO=DEPTNO;

-- 뷰 조회
SELECT * FROM EMP_DEPT;
```

## 2. RDBMS의 시스템 카탈로그
- **시스템 카탈로그**(system catalog)
	- 데이터베이스의 모든 객체(테이블, 뷰, 인덱스, 사용자, 권한 등)와 구조에 대한 정보를 담고 있는 시스템 테이블의 집합
	- 메타데이터(데이터 사전)를 저장한다.
	- 쿼리를 처리하거나 최적화할 때 참고한다.

### 2.1. 시스템 카탈로그의 역할
- **DBMS에 의한 사용**
	- DBMS가 스스로 생성하고 유지한다.
	- 사용자가 DDL(데이터 정의어)을 통해 스키마를 변경하면 자동으로 갱신된다.

- **쿼리 처리 및 최적화**
	- 사용자가 쿼리를 실행하면, DBMS는 시스템 카탈로그를 참조하여 테이블 존재 여부, 컬럼 정보, 데이터 타입, 사용자 권한, 인덱스 정보 등을 확인하고 가장 효율적인 실행 계획을 수립한다.

## 3. 오라클 시스템 카탈로그
- 오라클 시스템 카탈로그 - **데이터 사전(Data Dictionary)**
	- `SYSTEM` 테이블스페이스에 저장된다.
	- 사용자는 보통 직접 기본 테이블에 접근하는 대신, 이해하기 쉬운 형태로 정보를 제공하는 데이터 사전 뷰(View)를 통해 정보를 조회한다.

### 3.1. 주요 데이터 사전 뷰
> 오라클의 데이터 사전 뷰는 접근 범위에 따라 다음과 같은 접두사를 가진다.

- `USER_*`
	- 현재 사용자가 소유한 객체에 대한 정보를 보여준다.
	- (예: `USER_TABLES`, `USER_VIEWS`)

- `ALL_*`
	- 현재 사용자가 접근할 수 있는 모든 객체에 대한 정보를 보여준다.
	- (예: `ALL_TABLES`, `ALL_CATALOG`)

- `DBA_*`
	- 데이터베이스의 모든 객체에 대한 정보를 포함하며,
	- 주로 DBA 권한을 가진 사용자가 접근한다.

### 3.2. 실습
#### (1) 사용자가 접근 가능한 객체 정보 검색
- 'KIM' 사용자가 접근할 수 있는 모든 테이블, 뷰 등의 객체 정보를 `ALL_CATALOG` 뷰에서 조회
```sql
SELECT *
FROM ALL_CATALOG
WHERE OWNER='KIM';
```

#### (2) 테이블의 애트리뷰트(컬럼) 정보 검색
- 현재 사용자가 소유한 `EMPLOYEE` 테이블의 모든 애트리뷰트 이름과 데이터 타입을 `USER_TAB_COLUMNS` 뷰에서 조회
```sql
SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE
FROM USER_TAB_COLUMNS
WHERE TABLE_NAME = 'EMPLOYEE';
```

#### (3) 뷰의 정의(SELECT문) 확인
- 현재 사용자가 소유한 뷰들의 이름과 해당 뷰를 정의한 SELECT문을 `USER_VIEWS` 뷰에서 조회
```sql
SELECT VIEW_NAME, TEXT
FROM USER_VIEWS;
```

#### (4) INDEX 생성 후 통계 정보 확인
- `EMPLOYEE` 테이블의 부서번호(`DNO`) 애트리뷰트에 `EMPDNO_IDX`라는 이름의 인덱스를 생성
```sql
CREATE INDEX EMPDNO_IDX ON EMPLOYEE(DNO);
```

- `EMPDNO_IDX` 인덱스에 대한 통계 정보를 `USER_INDEXES` 뷰에서 조회
```sql
SELECT INDEX_NAME, INITIAL_EXTENT, DISTINCT_KEYS, NUM_ROWS, SAMPLE_SIZE, LAST_ANALYZED
FROM USER_INDEXES
WHERE INDEX_NAME = 'EMPDNO_IDX';
```
