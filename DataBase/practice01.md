# 데이터베이스 실습

### 1.1 SQL 정의 및 분류

- SQL (Structured Query Language)

  - 관계형 데이터베이스 표준 언어로서 가장 많이 사용되는 데이터 언어
    - SYSTEM R'이라는 실험용 DBMS를 위한 데이터 언어로 IBM 연구소에서 처음 개발
    - 현재는 미국 표준(ANSI)과 국제 표준(ISO) 관계형 데이터베이스 표준 언어로 승인
  - ORACLE, MS SQL-Server, MySQL 등 거의 모든 관계형 DBMS가 지원

- SQL을 구성하는 3가지 부속 언어의 분류와 관련 주요 기능

![database77](/assets/images/2023-12-07/database77.jpeg)

### 2.1 SQL 데이터 정의문

- SQL 정의문 (DDL)

  - Data Definition Language
  - 데이터베이스 구조 정의
  - 데이터베이스 객체 생성/수정/삭제

  2.1.1 데이터베이스 생성

- 1. 데이터베이스 생성 & 삭제

```SQL
DROP DATABASE IF EXISTS univDB;
CREATE DATABASE IF NOT EXISTS univDB;
```

- 2. 기본 데이터베이스 지정 (sql 명령어 실행 대상 지정)

```SQL
-- SQL 명령어를 실행할 대상인 기본 데이터베이스를 univDB로 지정
USE univDB;

SELECT database(); -- 현재 사용 데이터베이스 확인
```

![database78](/assets/images/2023-12-07/database78.jpeg)

2.1.2 테이블 생성 CREATE 문

- CREATE TABLE 문의 형식

```SQL
CREATE TABLE 테이블_이름
({열_이름 데이터_유형 [NULL|NOT NULL] [DEFAULT 기본_값],
    [PRIMARY KEY (열_이름_리스트),]
 {[UNIQUE (열_이름_리스트),]} *
 {[FOREIGN KEY (열_이름_리스트) REFERENCES 테이블_이름 (열_이름_리스트),]}*
});
```

- 데이터 유형

![database79](/assets/images/2023-12-07/database79.jpeg)

- SQL 제어문 (DCL)
  - Data Control Language
  - 데이터베이스 관리 및 통제
  - 데이터베이스 백업 및 복원
  - 데이터베이스의 사용자 등록 및 권한 관리

![database80](/assets/images/2023-12-07/database80.jpeg)

2.1.1 사용자 및 권한 관리

- 계정 생성: CREATE USER
  - CREATE USER 명령문의 형식

```SQL
CREATE USER 사용자_계정 IDENTIFIED BY '비밀번호';
```

- CREATE USER 명령문의 예제

```SQL
CREATE USER 'user1'@'127.1.1.1' IDENTIFIED BY '1111';
CREATE USER 'user2'@'localhost' IDENTIFIED BY '2222';
CREATE USER 'user3'@'198.182.10.2' IDENTIFIED BY '3333';
CREATE USER 'user4'@'%' IDENTIFIED BY '4444';
```

- 생성된 사용자 계정 정보 확인

```SQL
SELECT host, user FROM mysql.user;
```

- 권한부여: GRANT

- 권한 철회: REVOKE

- 계정 삭졔: DROP USER

### 2.3 SQL 데이터 조작문

- SQL 조작문 (DML)

  - Data Manipulation Language
  - 데이터베이스 데이터 관리
  - 데이터 입력/수정/삭제/검색

  2.3.1 데이터 검색 SELECT 문

* 행 검색

  - 테이블로부터 데이터를 검색하기 위해서는 SELECT문 사용

* SELECT문의 형식: SELECT절, FROM절

```SQL
SELECT 이름, 주소
FROM 학생;
```

```SQL
SELECT 학번, 이름, 주소, 학년, 나이, 성별, 휴대폰번호, 소속학과
FROM 학생;
```

```SQL
SELECT *
FROM 학생;
```

```SQL
SELECT DISTINCT 소속학과
FROM 학생;
```

- 조건 검색: WHERE절

```SQL
SELECT 이름, 학년, 소속학과, 휴대폰번호
FROM 학생
WHERE 학년 >= 2 AND 소속학과 = '컴퓨터';
```

- 순서화 검색: ORDER BY 절

```SQL
SELECT 이름, 학년, 소속학과
FROM 학생
WHERE 소속학과 = '컴퓨터' OR 소속학과 = '정보통신'
ORDER BY 학년 ASC;
```

- 집계 함수 검색

* 실제 테이블 저장 값이 아닌 행의 개수(count) 또는 특정 열의 값 평균(average)을 구하는 질의가 필요할 경우를 위해 SQL은 집계 함수를 제공
* 집계 함수
  - 각 열에 대한 기본 통계 결과를 반환하는 함수
  - COUNT(), MAX(), MIN(), SUM(), AVG()

![database81](/assets/images/2023-12-07/database81.jpeg)

```SQL
SELECT COUNT(*)
FROM 학생;
```

```SQL
SELECT FROM(학번)
FROM 학생;
```

```SQL
SELECT COUNT(*) AS 학생수1, COUNT(주소) AS 학생수2, COUNT(DISTINCT 주소) AS 학생수3
FROM 학생;
```

```SQL
SELECT AVG(나이) '여학생 평균나이'
FROM 학생
WHERE 성별 = '여';
```

- 조인 검색: JOIN

  - 조인 검색은 둘 이상의 테이블로부터 연관된 행들의 결합을 통해서 검색 결과를 생성

  - 크로스 조인
    - '조인\_조건식' 없이 이루어진 조인
    - 관계 대수의 카티션 프로덕트 연산을 적용한 결과(두 테이블을 곱한 결과)를 반환
    - 대부분의 행이 의미 없는 기계적인 결합임

```SQL
SELECT *
FROM 학생, 수강;
```

```SQL
SELECT *
FROM 학생 CROSS JOIN 수강;
```

- 동등 조인
  - '조인\_조건식'에 '=' 연산자를 사용하는 동등 조건에 의한 조인
  - 두 테이블 행들 사이의 의미 있는 조합만을 검색
  - 크로스 조인의 결과 테이블 중에서 의미 없는 조합을 제외함으로써 관계 대수의 동등 조인 연산을 적용한 결과를 반환

```SQL
SELECT *
FROM 학생, 수강
WHERE 학생.학번 = 수강.학번;
```

```SQL
SELECT *
FROM 학생 JOIN 수강 ON 학생.학번 = 수강.학번;
```

2.3.2 행 삽입 INSERT 문

- INSERT 문의 형식:

```SQL
INSERT INTO 테이블_이름 [(열_리스트)]
VALUES (열_값_리스트);
```

```SQL
INSERT INTO 학생1
VALUES ('g001', '김연아2', '서울 서초', 4, 23, '여', '010-1111-2222', '컴퓨터');
```

2.3.3 행 수정 UPDATE ans

- UPDATE문의 형식:

```SQL
UPDATE 테이블_이름
SET 열_이름 = 산술식 [{, 열_이름 = 산술식}]
[WHERE 수정_조건식];
```

```SQL
UPDATE 학생1
SET 학년 = 3
WHERE 이름 = '이은진';

SELECT * FROM 학생1 WHERE 이름 = '이은진';
```

```SQL
UPDATE 학생1
SET 학년 = 학년 + 1, 소속학과 = '자유전공학부'
WHERE 학년 = 4;

SELECT * FROM 학생1;
```

2.3.4 행 삭제 DELETE 문

- DELETE문의 형식:

```SQL
DELETE FROM 테이블_이름
[WHERE 삭제_조건식];
```

- 단일 행 삭제

```SQL
DELETE FROM 학생1
WHERE 이름='송윤아';

SELECT * FROM 학생1;
```

- 복수 행 삭제

```SQL
DELETE FROM 학생1
WHERE 학년 = 3;

SELECT * FROM 학생1;
```

### 3.1 뷰

- 뷰(View)
  - 데이터베이스를 바라보는 창문(window)
  - 실제 데이터를 저장하지 않는 가상 테이블(virtual table)
  - 뷰에 대해 사용자가 질의를 요청할 때 비로소 DBMS는 뷰 정의를 참조하여 질의를 수행하고 그 결과를 사용자에게 반환
  - 주로 기반 테이블로부터 정의되지만 또 다른 뷰를 기반으로도 정의될 수 있음

![database82](/assets/images/2023-12-07/database82.jpeg)

- 뷰의 장점

1. 편의성
   : 복잡한 질의문 작성이 쉽고 간단해진다.

2. 보안성
   : 데이터 보안 유지가 쉽다.

3. 재사용성
   : 반복되는 질의문 작성에 효율적이다.

4. 독립성
   : 스키마 변경에도 뷰 질의문은 변경할 필요가 없다.

### 3.2 인덱스

4.1 인덱스 개념

- 인덱스(index)

  - 테이블 안의 데이터를 쉽고 빠르게 찾을 수 있도록 만든 데이터베이스 객체
  - 책 페이지를 목차나 색인을 통해 쉽게 찾는 것과 같은 원리
  - 대부분 DBMS는 B-트리(Balanced tree) 구조의 인덱스를 지원

- 장점

  - 루트 노드로부터 리프 노드까지의 탐색 길이가 같아서 모든 데이터에 대한 일정 수준의 검색 시간을 보장

- 단, 불필요한 인덱스는 오히려 성능을 떨어뜨리고 저장 공간만 낭비할 수 있음

![database83](/assets/images/2023-12-07/database83.jpeg)
