# 데이터베이스 실습 Week 2

#### 2023.12.07.(목)

### 1.1 내장함수

- SQL 내장 함수
  - 내장 함수(built-in function)와 사용자 정의 함수(user-defined function) 구분
  - 열 이름이나 상수 값을 입력받아 값 하나를 결과로 반환
  - 기본 연산 함수와 시스템 정보 제공 함수, 데이터 가공 함수 등이 지원
  - SELECT 절이나 WHERE 절 뿐만 아니라 UPDATE SET 절 등에서도 사용 가능

![database84](/assets/images/2023-12-07/database84.jpeg)

1.1.1 MySQL 주요 내장함수

- 숫자 함수

```SQL
SELECT ABS(+17), ABS(-17), CEIL(3.28), FLOOR(4.59);
```

```SQL
SELECT 학번, SUM(기말성적)/COUNT(*), ROUND(SUM(기말성적)/COUNT(*), 2)
FROM 수강
GROUP BY 학번
```

- 문자 함수

```SQL
SELECT LENGTH(소속학과), RIGHT(학번, 2), REPEAT('*', 나이), CONCAT(소속학과, '학과')
FROM 학생;
```

```SQL
SELECT SUBSTRING(주소, 1, 2), REPLACE(SUBSTRING(휴대폰번호, 5, 9), '-', ',')
FROM 학생;
```

1.1.1 MySQL 주요 내장함수

- 날짜, 시간 함수

```SQL
SELECT 신청날짜, LAST_DAY(신청날짜)
FROM 수강
WHERE YEAR(신청날짜) = '2019';
```

```SQL
SELECT SYSDATE(),  DATEDIFF(신청날짜, '2019-01-01')
FROM 수강;
```

```SQL
SELECT 신청날짜, DATE_FORMAT(신청날짜, '%b/%d/%y'), DATE_FORMAT(신청날짜, '%Y년%c월%e일')
FROM 수강;
```

### 1.2 저장 프로시저

- 저장 프로시저(stored procedure)
  - 미리 작성하여 데이터베이스 안에 저장한 SQL 문장들의 묶음
  - 독립된 프로그램으로 데이터베이스 안에 하나의 객체로 저장
  - 장점: 최적화된 SQL문을 미리 데이터베이스에 작성해둘 수 있고 복잡한 SQL문을 전달할 필요가 없어 네트워크 부하가 줄어들며 여러 응용 프로그램 간의 공유가 가능함

![database85](/assets/images/2023-12-07/database85.jpeg)

1.2.1 삽입 \* 수정 저장 프로시저 생성

- CREATE PROCEDURE 문의 형식

```SQL
CREATE PROCEDURE 프로시저_이름
BEGIN
  '''
  SQL 명령문;
  '''
END
```

- '과목' 테이블의 데이터 삽입 수정 \* 저장 프로시저

```SQL
DELIMITER //
CREATE PROCEDURE InsertOrUpdateCourse(
    IN CourseNo VARCHAR(4),
    IN CourseName VARCHAR(20),
    IN CourseRoom CHAR(3),
    IN CourseDept VARCHAR(20),
    IN CourseCredit INT
)
BEGIN
  DECLARE Count INT;
  SELECT COUNT(*) INTO Count FROM 과목 WHERE 과목번호 = CourseNo;
  IF (Count = 0) THEN
    INSERT INTO 과목(과목번호, 이름, 강의실, 개설학과, 시수)
    VALUES(CourseNo, CourseName, CourseRoom, CourseDept, CourseCredit);
 ELSE
   UPDATE 과목
   SET 이름 = CourseName, 강의실 = CourseRoom, 개설학과 = CourseDept, 시수 = CourseCredit
   WHERE 과목번호 = CourseNo;
 ENDIT;
END //
DELIMITER;
```

- 삽입 저장 프로시저의 호출

```SQL
-- 행 삽입 예
CALL InsertOrUpdateCourse('c006', '연극학개론', '310', '교양학부', 2);
SELECT * FROM 과목;
```

- 수정 저장 프로시저의 호출

```SQL
-- 행 수정 예
CALL InsertOrUpdateCourse('c006', '연극학개론', '410', '교양학부', 2);
SELECT * FROM 과목;
```

1.2.2 검색 저장 프로시저의 생성 및 활용

- 'SelectAverageOfBestScore' 프로시저 작성

```SQL
DELIMITER //
CREATE PROCEDURE SelectAverageOfBestScore (
    IN Score INT,
    OUT Count INT
)
BEGIN
  DECLARE NoMoreData INT DEFAULT FALSE;
  DECLARE Midterm INT;
  DECLARE Final INT;
  DECLARE Best INT;
  DECLARE ScoreListCursor CURSOR FOR
    SELECT 중간성적, 기말성적 FROM 수강;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET NoMoreData = TRUE;
    SET Count = 0;
  OPEN ScoreListCursor;
  REPEAT
    FETCH ScoreListCursor INTO Midterm, Final;
    IF Midterm > Final THEN
      SET Best = Final;
    ELSE
      SET BEST = Final;
    END IF;
    IF (Best >= Score) THEN
      SET Count = Count + 1;
    END IF;
  UNTIL NoMoreData END REPEAT;
  CLOSE ScoreListCursor;
END;
//
DELIMITER;
```

1.2.3 프로시저 호출 및 삭제

- 검색 저장 프로시저의 호출

```SQL
-- 행 검색 예
CALL SelectAverageOfBestScore(95, @Count);
SELECT @Count;
```

- 저장 프로시저의 삭제
  - DROP PROCEDURE 명령문의 형식 및 예제

```SQL
DROP PROCEDURE 프로시저_이름;
```

```SQL
DROP CREATE PROCEDURE InsertOrUpdateCourse;
```

```SQL
DROP PROCEDURE InsertOrUpdateCourse;
```

### 1.3 트리거 (trigger)

- 트리거

  - 데이터 변경 등 명세된 이벤트 발생시 감지하여 자동 실행되는 사용자 정의 프로시저
  - INSERT, UPDATE, DELETE 명령문의 실행 직전 \* 후 자동으로 호출되어 실행
  - 보통 무결성 제약 조건을 유지하거나 업무 규칙 등을 적용하기 위해 사용

  ![database86](/assets/images/2023-12-07/database86.jpeg)

  1.3.1 트리거 생성 및 실행

  - CREATE TRIGGER 명령문의 형식

```SQL
CREATE TRIGGER 트리거_이름
[BEFORE|AFTER][INSERT|UPDATE|DELETE] ON 테이블이름 FOR EACH ROW
BEGIN
  '''
  SQL 명령문;
  '''
END
```

```
TIP) 트리거 참조 테이블 OLD & NEW
테이블에 어떠한 처리가 이루어지기 직전의 값과 직후의 값들을 저장하는 특별한 테이블이 OLD 테이블과 NEW 테이블이다.
* OLD, 열이름: 처리 직전의 특정 열 값
* NEW, 열이름: 처리 직후의 특정 열 값
```

1.3.1 트리거 생성 및 실행

- 트리거의 생성

```SQL
DELIMITER //
CREATE TRIGGER AfterInsertStu
AFTER INSERT ON 학생 FOR EACH ROW
BEGIN
  IF (NEW.성별 = '남') THEN
    UPDATE 남녀학생총수 SET 인원수 = 인원수 + 1 WHERE 성별 = '남';
  ELSEIF (NEW.성별 = '여') THEN
    UPDATE 남녀학생총수 SET 인원수 = 인원수 + 1 WHERE 성별 = '여';
  END IF;
END;
//
DELIMITER;
```

- 트리거의 실행

```SQL
INSERT INTO 학생
VALUES('S008', '최동석', '경기 수원', 2, 26, '남', '010-8888-6666', '컴퓨터');

SELECT * FROM 학생;
SELECT * FROM 남녀학생총수;
```

### 1.3 트리거 (trigger)

1.3.2 트리거의 삭제

- DROP TRIGGER 명령문의 형식

```SQL
DROP TRIGGER 트리거_이름;
```

- DROP TRIGGER 적용 예

```SQL
SHOW TRIGGERS;
```

```SQL
DROP TRIGGER AfterInsertStu;
```

### 1.4 사용자 정의 함수

- 사용자 정의 함수(user defined function)

  - 사용자가 직접 정의한 함수로 DBMS 안에 독립된 데이터베이스 객체로 저장
  - SELECT 문이나 프로시저 안에서 호출되어 특정 기능을 수행하고 결과 값을 반환하는 용도로 사용
    - 스칼라 함수는 하나의 값 또는 NULL을 반환
    - 테이블 함수는 각 행이 하나 이상의 열로 구성된 테이블로 반환

  1.4.1 사용자 정의 함수 생성

- CREATE FUNCTION 명령문의 형식

```SQL
CREATE FUNCTION 함수명(매개변수 매개변수_자료형)
RETURNS 반환값_자료형
BEGIN
  '''
  SQL 명령문;
  '''
  RETURN 반환값;
END
```

- 사용자 정의 함수 작성

```SQL
DELIMITER //
CREATE FUNCTION Fn_Grade(grade CHAR(1))
RETURNS VARCHAR(10)
BEGIN
  DECLARE ret_grade VARCHAR(10);
  IF (grade = 'A') THEN
    SET ret_grade = '최우수';
  ELSEIF (grade = 'B') THEN
    SET ret_grade = '우수';
  ELSEIF (grade = 'C') THEN
    SET ret_grade = '보통';
  ELSEIF (grade = 'D' OR grade = 'F') THEN
    SET ret_grade = '미흡';
  END IF;
  RETURN ret_grade;
END
//
DELIMITER;
```

1.4.2 사용자 정의 함수 적용

```SQL
SELECT 학번, 과목번호, 평가학점, Fn_Grade(평가학점) AS '평가 등급' FROM 수강;
```

1.4.3 사용자 정의 함수 삭제

- DROP FUNCTION 명령문의 형식

```SQL
DROP FUNCTION 사용자정의함수_이름;
```

- DROP FUNCTION 적용 예

```SQL
SHOW CREATE FUNCTION Fn_Grade;
```

```SQL
DROP FUNCTION Fn_Grade;
```

### 1.5 저장 프로시저, 트리거, 사용자 정의 함수의 비교

- 저장 프로시저

  - 여러 응용 프로그램 사이에 공유함으로써 처리의 일관성 향상과 보안 강화
  - 전송되는 SQL 명령문의 양을 줄이고 미리 컴파일된 코드를 호출함으로써 반복적인 처리 시 성능 향상

- 트리거

  - 복잡한 데이터 무결성을 강화, 다양한 데이터 처리 업무 규칙을 구현
  - 성능이 다소 저하될 수 있음

- 사용자 정의 함수
  - 특정 값뿐만 아니라 테이블도 반환할 수 있어 제한된 SQL 명령문의 기능을 확장시키고 명령문 작성의 편의성을 향상시킴

|구분|저장 프로시저|트리거|사용자 정의 함수|
|공통점| 데이터베이스 객체, DBMS 안에 저장, 절차적 언어로 작성|
|차이점| CREATE PROCEDURE, CALL 문으로 직접 호출 실행, SQL 문을 포함, 반환값 제공(선택사항)|CREATE TRIGGER, INSERT, UPDATE, DELETE문 실행시 자동 호출 실행, SQL 문을 포함, 반환값 없음|CREATE FUNCTION, SELECT 문 실행시 간접 호출 실행, SQL 문에 포함, 반환값 제공(필수사항)|

### 1.6 트랜잭션

- 트랜잭션(transaction)

* 한 묶음으로 처리되어야 하는 SQL 명령문들의 집합
  - 트랜잭션에 속한 SQL 명령문들은 모두 정상 처리되어야 함께 완료되며 하나의 명령문이라도 오류가 발생하면 전체를 취소함

![database87](/assets/images/2023-12-07/database87.jpeg)

- 커밋(commit): 정상 종료

  - 트랜잭션의 실행 결과를 데이터베이스에 최종적으로 반영하는 것
  - 임시로 실행 처리한 트랜잭션의 실행 결과를 실제 데이터베이스에 반영하는 명령

- 롤백(rollback)

  - 실행 결과를 반영하지 않고 취소하여 원래 상태로 되돌리는 것

- 롤백이 가능하므로 커밋되기 전까지는 데이터베이스에 완전하게 반영되었다고 확신할 수 없는 임시 실행 결과일 뿐임

* 2개의 UPDATE 문으로 구성된 '계좌이체' 트랜잭션의 예

![database88](/assets/images/2023-12-07/database88.jpeg)

- 2개의 UPDATE문은 모두 실행되어야 하고, 둘 중 하나의 UPDATE 문만 실행되어서는 곤란하므로 트랜잭션으로 묶어 처리해야 함
- START TRANSACTION 명령문과 COMMIT 명령문으로 2개의 UPDATE문을 둘러쌈으로써 트랜잭션을 구성

  1.6.1 트랜잭션의 ACID 특성

* 원자성 (Atomicity)

- 트랜잭션 안의 SQL 명령문을 모두 성공적으로 실행하여 완료하거나 아니면 모두 철회하여 무효화해야함을 의미
  - '전부 혹은 전무(all or nothing)' 실행 규칙을 적용
  - 만약 장애가 발생한다면 장애 발생 시점에 이미 정상 완료되어 커밋된 트랜잭션은 제외하고 완료되지 않은 트랜잭션은 모두 취소시킴

* 일관성 (Consistency)

  - 데이터베이스가 트랜잭션 실행 전의 일관된 상태에서 트랜잭션 실행 후에도 또 다른 일관된 상태로 전환되어야함을 의미

* 고립성 (Isolation)

  - 커밋될 때까지 트랜잭션이 수행한 임시 실행 결과가 다른 트랜잭션에게 공개되지 않아야함을 의미

* 지속성(Durability)

- 일단 트랜잭션이 커밋되면 그 트랜잭션의 실행 결과는 장애가 발생하더라도 보존되어야 함을 의미

* 트랜잭션의 특성과 기법

![database89](/assets/images/2023-12-07/database89.jpeg)

- 트랜잭션 지원 DBMS 모듈

  - 동시성 제어(concurrency control) 모듈
    - 동시에 실행되는 트랜잭션 간의 간섭을 제어
    - 최종적으로 각 트랜잭션이 순차적으로 실행한 결과와 동일한 고립성 결과를 보장하고 트랜잭션 실행 이전과 이후의 데이터베이스 일관성이 항상 유지되도록 함
  - 회복(recovery) 모듈
    - 완전한 트랜잭션 결과의 복구를 보장
    - 장애 발생 시 트랜잭션 실행의 원자성을 보장하고 커밋된 트랜잭션의 결과는 반드시 데이터베이스에 반영되도록 지속성을 지원
  - 대표적 동시성 제어 기법은 락킹(locking)
  - 대표적인 회복 기법은 로깅(logging)

  1.6.2 트랜잭션의 종류 - 트랜잭션은 설정 모드에 따라 3가지 트랜잭션으로 구분

![database90](/assets/images/2023-12-07/database90.jpeg)

1.6.3 명시적 트랜잭션

- 트랜잭션의 시작과 끝을 사용자가 직접 명시적으로 지정하는 트랜잭션
- '사용자 트랜잭션' 또는 '수동 트랜잭션'
- 명시적 트랜잭션을 정의하는 명령문의 형식

```SQL
START TRANSACTION;
   '''
   SQL 명령문;
   '''
{COMMIT|ROLLBACK};
```

- START TRANSACTION 명령문은 직접 트랜잭션의 시작을 지시
- COMMIT 명령문은 트랜잭션의 모든 처리 결과의 정상적 처리를 확정하는 명령어로 변경 내용을 모두 실제 데이터베이스에 영구적으로 반영함
- ROLLBACK 명령문은 작업 중 장애나 문제가 발생하여 트랜잭션의 처리 과정에서 발생한 변경 내용을 모두 취소하는 명령어로 트랜잭션 시작 이전의 원래 상태로 되돌림

1.6.3 명시적 트랜잭션

- 명시적 트랜잭션 적용 예

```SQL
START TRANSACTION;
  DELETE FROM 학생 WHERE 성별 = '남';
  SELECT * FROM 학생;
ROLLBACK;
SELECT * FROM 학생;
```

1.6.4 자동완료 트랜잭션

- SQL문 하나를 독립된 하나의 트랜잭션으로 자동 정의하고 SQL문의 실행 결과에 따라 자동으로 커밋 또는 롤백하는 트랜잭션

- 기본 모드의 트랜잭션, '시스템 트랜잭션'

- DBMS가 각 SQL 문장 앞뒤에 START TRANSACTION 문과 COMMIT 문 또는 ROLLBACK 문을 자동으로 붙여 실행

- 자동완료 트랜잭션의 설정
  - '@@AUTOCOMMIT'은 트랜잭션 모드 설정 값을 저장하고 있는 시스템 변수
  - 값이 '1'이면 자동완료 모드가 설정된 것

```SQL
SELECT @@AUTOCOMMIT;
SET AUTOCOMMIT = 1;
```

- 자동완료 트랜잭션의 적용 예

```SQL
INSERT INTO 과목 VALUES('c007', '영어회화', '333', '교양학부', 3);
SELECT * FROM 과목;
ROLLBACK;
SELECT * FROM 과목; 1
```
