# Database08

## 서브쿼리

#### 2023.11.03.(금)

## [MYSQL 서브쿼리 개념 & 문법 100 정리](https://inpa.tistory.com/entry/MYSQL-📚-서브쿼리-정리)

### 서브쿼리(Subquery)

서브쿼리(subquery)란 다른 쿼리 내부에 포함되어 있는 SELECT 문을 의미한다.
서브쿼리를 포함하고 있는 쿼리를 외부쿼리(outer query)라고 부르며, 서브쿼리는 내부쿼리(inner query)라고도 부른다.
서브쿼리는 다음과 같이 괄호()로 감싸져서 표현된다.

[서브 쿼리 실행 순서]

- 서브쿼리 실행 -> 메인(부모) 쿼리 실행

```sql
SELECT *
FROM main_table 55
WHERE target_id IN (
    SELECT 9d
    FROM sub_table_noindex_55
    WHERE id < 500
);
```

1. 서브쿼리는 하나의 SQL 문 안에 포함되어 있는 또다른 SQL 문을 말한다.

2. (SELECT \* FROM table) 같이 괄호() 안에 있는 쿼리를 서브 쿼리라 말한다.

- 서브쿼리(=자식쿼리, 내부쿼리) - 메인쿼리 컬럼 사용 가능
- 메인쿼리(=부모쿼리, 외부쿼리) - 서브쿼리 컬럼 사용 불가

> Java 객체지향의 상속과 똑같은 개념이다.
> 상속당한 자식 객체는 부모 객체의 인스턴스를 사용할 수 있고, 부모는 자식객체의 인스턴스를 사용할 수 있다.

[서브 쿼리 장점]

1. 서브쿼리는 쿼리를 구조화시키므로, 쿼리의 각 부분을 명확히 구분할 수 있게 해준다.
2. 서브쿼리는 복잡한 JOIN이나 UNION과 같은 동작을 수행할 수 있는 또다른 방법을 제공
3. 서브쿼리는 복잡한 JOIN이나 UNION보다 좀 더 읽기 편함 (가독성이 좋음)

#### 서브쿼리의 위치에 따른 명칭

```sql
SELECT col1, (SELECT ...) -- 스칼라 서브쿼리(Scalar Sub Query): 하나의 칼럼처럼 사용 (표현 용도)
FROM (SELECT ...) -- 인라인 뷰(Inline View): 하나의 테이블처럼 사용 (테이블 대체 용도)
WHERE col = (SELECT ...) -- 일반 서브쿼리: 하나의 변수(상수)처럼 사용 (서브쿼리의 결과에 따라 달라지는 조건절)
```

#### 중첩 서브쿼리(Nested Subquery)

- WHERE 문에 나타나는 서브쿼리

```sql
SELECT name, height
FROM userTbl
WHERE height > 177;
```

---> 조건값을 상수로 할 때

```sql
SELECT name, height
FROM userTbl
WHERE height > (SELECT height FROM userTbl WHERE name in ('김경호'));
```

---> 조건값을 SELECT로 특정할 때 (단 결과가 값이 하나여야 함)

```sql
SELECT name, height
FROM userTbl
WHERE height = any(SELECT height FROM userTbl WHERE addr in ('경남'));
```

---> 조건에 값이 여러 개 들어올 땐 any.
---> any는 in과 동일한 의미.
---> or을 의미한다.

```sql
SELECT *
FROM city
WHERE population > all(SELECT population FROM city WHERE district = 'New York');
```

---> all은 도출된 모든 조건값에 대해 만족할 때.
---> and를 의미한다.

#### 인라인 뷰(Inline View)

- FROM 문에 나타나는 서브쿼리
- 참고로 서브 쿼리가 FROM 절에 사용되는 경우 무조건 AS 별칭을 지정해 주어야 한다.

```sql
SELECT EX1.name, EX1.salary
FROM (
    SELECT *
    FROM employee AS Ii
    WHERE Ii.office_worker='사원'
) EX1; -- 서브쿼리 별칭
```

### 스칼라 서브쿼리(Scalar Subquery)

- SELECT 문에 나타나는 서브쿼리
- 딴 테이블에서 어떠한 값을 가져올 때 쓰임
- 하나의 레코드만 리턴이 가능하며, 두 개 이상의 레코드는 리턴할 수 없다.
- 일치하는 데이터가 없더라도 NULL 값을 리턴할 수 있다. 이는 원래 그룹함수의 특징 중에 하나인데 스칼라 서브쿼리 또한 이 특징을 가지고 있다.

```sql
SELECT D.DEPTNO, (SELECT MIN(EMPNO) FROM EMP WHERE DEPTNO = D.DEPTNO) as EMPNO
FROM DEPT D
ORDER BY D.DEPTNO
```

### 서브 쿼리 실행 조건

1. 서브쿼리는 SELECT 문으로만 작성할 수 있다. (정확히 SELECT 문 쿼리밖에 사용할 수 없는 것이다.)
2. 반드시 괄호() 안에 존재하여야 한다.
3. 괄호가 끝나고 끝에 ;(세미콜론)을 쓰지 않는다.
4. ORDER BY를 사용할 수 없다.

### 서브쿼리 사용 가능한 곳

MySQL에서 서브쿼리를 포함할 수 있는 외부쿼리는 SELECT, INSERT, UPDATE, DELETE, SET, DO 문이 있다.

이러한 서브쿼리는 또다시 다른 서브쿼리 안에 포함될 수 있다.

1. SELECT
2. FROM
3. WHERE
4. HAVING
5. ORDER BY
6. INSERT 문의 VALUES 부분 대체제
7. UPDATE 문의 SET 부분 대체제

> 서브쿼리도 별칭(alias) 사용이 가능하다.

### 서브쿼리 실전 예제

```sql
CREATE TABLE employee (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(64),
    salary INT,
    office_worker VARCHAR(64)
)

INSERT INTO employee VALUES(1, '허사장', 200000000, '사장');

INSERT INTO employee (name,salary,office_worker) VALUES('유부장',10000000,'부장');
INSERT INTO employee (name,salary,office_worker) VALUES('박차장',5000000,'차장');
INSERT INTO employee (name,salary,office_worker) VALUES('정과장',4000000,'과장');
INSERT INTO employee (name,salary,office_worker) VALUES('정대리',3895000,'대리');
INSERT INTO employee (name,salary,office_worker) VALUES('노사원',2500000,'사원');
INSERT INTO employee (name,salary,office_worker) VALUES('하사원',2000000,'사원');
INSERT INTO employee (name,salary,office_worker) VALUES('길인턴',1000000,'인턴');
```

#### 중첩 서브쿼리 - 단일 행

```sql
-- Nested Subquery - 단일 행
-- 정대리라는 사람의 직급을 구하시오.
SELECT office_worker
FROM employee
WHERE office_worker = (SELECT office_worker FROM employee WHERE name = '정대리')
```

#### 중첩 서브쿼리 - 복수(다중) 행

-> IN, ANY, ALL, EXISTS 등의 연산자로 얻은 서브쿼리 결과 여러개의 행을 반환

```sql
-- Nested Subquery - 복수(다중) 행
-- 정대리보다 급여가 높은 사람들을 구하시오.
SELECT *
FROM employee
WHERE salary > (
    SELECT salary
    FROM employee
    WHERE Name = '정대리'
)
```

```sql
-- 직급이 사원인 사람들을 구하시오.
SELECT *
FROM employee
WHERE office_worker IN (
    SELECT office_worker
    FROM employee
    WHERE office_worker = '사원')
```

#### 인라인 뷰(Inline View)

```sql
-- 인라인 뷰(Inline View)
-- 잘못된 구문 - 꼭 파생 테이블엔 별칭을 정해줘야 합니다.
SELECT * FROM (SELECT * FROM employee WHERE office_worker = '사원') (X)
/* SQL 오류 (1248): Every derived table must have its own alias */

-- 직급이 사원인 사람들의 이름과 급여를 구하시오.
SELECT EX1.name, EX1.salary
FROM (
    SELECT *
    FROM employee AS Ii
    WHERE Ii.office_worker='사원') EX1;
```

#### 스칼라 서브쿼리 (Scalar Subquery)

```sql
-- 스칼라 서브쿼리(Scalar Subquery)
-- 정대리 급여와 테이블 전체 평균 급여를 구하시오.
SELECT name, salary, (
    SELECT ROUND(AVG(salary), -1)
    FROM employee) AS '평균급여'
FROM employee
WHERE name = '정대리';
```

#### INSERT 문 서브쿼리 (INSERT Subquery)

```sql
-- 테이블2의 정보를 뽑아서 그 데이터를 테이블1에 넣어준다.
-- value() 들어갈 자리를 서브쿼리로 대체했다.
INSERT INTO table1 (SELECT * FROM table2);
```

#### DELETE 문 서브쿼리 (DELETE Subquery)

```sql
-- 인턴의 정보를 구해와서 삭제한다.
DELETE FROM employee
WHERE id = (SELECT id FROM employee WHERE office_worker = '인턴');
```

### UPDATE 문 서브쿼리 (UPDATE Subquery)

```sql
-- 인턴에 정보를 구해와서 급여를 10만원 인상한다.
UPDATE employee SET salary=(salary+100000)
WHERE id = (SELECT id FROM employee WHERE office_worker = '인턴');
```

## [SQL/MySQL 서브쿼리 (SubQuery)](https://snowple.tistory.com/360)

![database1](/assets/images/2023-11-03/database1.png)

![database2](/assets/images/2023-11-03/database2.png)

![database3](/assets/images/2023-11-03/database3.png)

![database4](/assets/images/2023-11-03/database4.png)

### 서브쿼리(Subquery)

서브쿼리란 하나의 SQL 문 안에 포함되어 있는 또 다른 SQL 문을 말한다. 서브쿼리는 메인쿼리가 서브쿼리를 포함하는 종속적인 관계이다.

```sql
#메인쿼리
SELECT *
FROM db_table
WHERE table_fk IN(
    #서브쿼리
    SELECT table_fk FROM db_table_other WHERE ...
)
```

조인(Join)은 조인에 참여하는 모든 테이블이 대등한 관계에 있기 때문에 조인에 참여하는 모든 테이블의 칼럼을 어느 위치에서라도 자유롭게 사용할 수 있다. 그러나 서브쿼리는 메인쿼리의 컬럼을 모두 사용할 수 있지만, 메인쿼리는 서브쿼리의 칼럼을 사용할 수 없다.

조인은 집합 간의 곱의 관계이다. 따라서 1:1 관계 테이블이 조인하면 1(=1 _ 1) 레벨의 집합이 생성되고, 1:M 관계가 조인하면 M(=1 _ M) 레벨의 집합이 된다. 그리고 M:N 관계의 테이블이 조인하면 MN 레벨의 집합이 된다.

그러나, 서브쿼리는 서브쿼리 레벨과는 상관없이 항상 메인쿼리 레벨로 결과 집합이 생성된다.

조인쿼리와 서브쿼리 방식을 상황에 맞게 사용하는 것이 중요하다.

### 서브쿼리를 사용할 때 주의할 점

1. 서브쿼리를 괄호로 감싸서 사용한다.
2. 서브쿼리는 단일 행 또는 복수 행 비교 연산자와 함께 사용 가능
3. 서브쿼리에서는 ORDER BY를 사용하지 못한다.

### 서브쿼리가 사용이 가능한 곳

- SELECT
- FROM
- WHERE
- HAVING
- ORDER BY
- INSERT문의 VALUES
- UPDATE문의 SET

### 서브쿼리의 종류

1. 단일행 서브쿼리(Single Row Subquery)

```sql
SELECT *
FROM Player
WHERE Team_ID = (
    SELECT Team_ID
    FROM Player
    WHERE Player_name = "yonglae"
)
ORDER BY Team_name;
```

2. 다중행 서브쿼리(Multiple Row Subquery)

```sql
SELECT *
FROM Team
WHERE Team_ID IN (
    SELECT Team_ID
    FROM Player
    WHERE Player_name = "yonglae"
)
ORDER BY Team_name;
```

3. 다중컬럼 서브쿼리(Multi Column Subquery)

```sql
SELECT *
FROM Player
WHERE (Team_ID, Height) IN (
    SELECT Team_ID, MIN(Height)
    FROM Player
    GROUP By Team_ID
)
ORDER BY Team_ID, Player_name;
```

### 위치에 따라 사용되는 서브쿼리

1. SELECT 절에서 사용되는 서브쿼리

```sql
SELECT Player_name, Height, (
    SELECT AVG(Height)
    From Player p
    WHERE p.Team_ID = x.Team_ID) as AVG_Height
FROM Player x
```

2. FROM 절에서 사용되는 서브쿼리

FROM 절에서 사용되는 서브쿼리를 인라인 뷰(Inline View)라고 한다. FROM 절에는 테이블 명이 오도록 되어 있다. 그런데 서브쿼리가 FROM 절에 사용되면 뷰(View)처럼 결과가 동적으로 생성된 테이블로 사용할 수 있다. 임시적인 뷰이기 때문에 데이터베이스에 저장되지는 않는다.
또한, 인라인 뷰로 동적으로 생성된 테이블이여서 인라인 뷰의 칼럼은 자유롭게 참조가 가능하다.

```sql
SELECT t.Team_name, p.Player_name, p.Height
FROM Player t, (
    SELECT Team_ID, Player_name, Back_no
    FROM Player
    WHERE Position = 'MF') p
WHERE p.Team_ID = t.Team_ID;
```
