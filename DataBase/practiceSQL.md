# Practice SQL

## SQL 실습

#### 2023.12.06.(수)

### 1.1 내장함수

- SQL 내장 함수
  - 내장 함수(built-in function)와 사용자 정의 함수(user-defined function) 구분
  - 열 이름이나 상수 값을 입력받아 값 하나를 결과로 반환
  - 기본 연산 함수와 시스템 정보 제공 함수, 데이터 가공 함수 등이 지원
  - SELECT절이나 WHERE 절 뿐만 아니라 UPDATE SET절 등에서도 사용 가능

![database47](/assets/images/2023-12-06/database47.jpeg)

1.1.1 MySQL 주요 내장함수

- 숫자 함수

```SQL
SELECT ABS(+17), CEIL(3.28), FLOOR(4.259);
```

```SQL
SELECT 학번, SUM(기말성적)/COUNT(*), ROUND(SUM(기말성적)/COUNT(*),2)
FROM 수강
GROUP BY 학번
```

- 문자 함수

```SQL
SELECT LENGTH(소속학과), RIGHT(학번, 2), REPEAT('*', 나이), CONCAT(소속학과, '학과')
FROM 학생;
```

```SQL
SELECT SUBSTRING(주소, 1, 2), REPLACE(SUBSTRING(휴대폰번호, 5, 9), '-',',')
FROM 학생;
```

- 날짜, 시간 함수

```SQL
SELECT 신청날짜, LAST_DAY(신청날짜)
FROM 수강
WHERE YEAR(신청날짜) = '2019';
```

```SQL
SELECT SYSDATE(), DATEDIFF(신청날짜, '2019-01-01')
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
  - 저장: 최적화된 SQL 문을 미리 데이터베이스에 작성해둘 수 있고 복잡한 SQL 문을 전달할 필요가 없어 네트워크 부하가 줄어들며 여러 응용 프로그램 간의 공유가 가능함

![database48](/assets/images/2023-12-06/database48.jpeg)

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
CREATE PROCEDURE InsertOrUpdateCourse (
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
    END IF;
END
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
    SELECT 중간성적, 기말성적 FROM tnrkd;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET NoMoreData = TRUE;
    SET Count = 0;
  OPEN ScoreListCursor;
  REPEAT
    FETCH ScoreListCursor INTO Midterm, Final;
    IF Midterm > Final THEN
      SET Best = Midterm;
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

* DROP PROCEDURE 명령문의 형식 및 예제

```SQL
DROP PROCEDURE 프로시저_이름;
```

```SQL
DROP CREATE PROCEDURE InsertOrUpdateCourse;
```

```SQL
DROP PROCEDURE InsertOrUpdateCourse;
```

### 1.3 트리거(trigger)

- 트리거
  - 데이터 변경 등 명세된 이벤트 발생시 감지하여 자동 실행되는 사용자 정의 프로시저
  - INSERT, UPDATE, DELETE 명령문의 실행 직전\*후 자동으로 호출되어 실행
  - 보통 무결성 제약 조건을 유지하거나 업무 규칙 등을 적용하기 위해 사용

![database49](/assets/images/2023-12-06/database49.jpeg)

1.3.1 트리거 생성 및 실행

- CREATE TRIGGER 명령문의 형식

```SQL
CREATE TRIGGER 트리거_이름
[BEFORE | AFTER][INSERT|UPDATE|DELETE] ON 테이블이름 FOR EACH ROW
BEGIN
'''
  SQL 명령문;
'''
END
```

> TIP) 트리거 참조 테이블 OLD & NEW
> 테이블에 어떠한 처리가 이루어지기 직전의 값과 직후의 값들을 저장하는 특별한 테이블이 OLD 테이블과 NEW 테이블이다.

- OLD,열이름: 처리 직전의 특정 열 값
- NEW,열이름: 처리 직후의 특정 열 값

- 트리거의 생성

```SQL
DELIMITER //
CREATE TRIGGER AfterInsertStu
AFTER INSERT ON 학생 FOR EACH ROW
  BEGIN
    IF (NEW.성별 = '남') THEN
      UPDATE 남녀학생총수 SET 인원수 = 인원수 + 1 WHERE 성별 = '남';
    ELSEIF (NEW.성별 = '여') THEN
      UPDATE 남녀학생총수 SET 인원수 = 인원수 + 1 WHERE 성벽 = '여';
    END IF;
  END;
//
DELIMTER
```

- 트리거의 실행

```SQL
INSERT INTO 학생
VALUES('s008', '최동석', '경기 수원', 2, 26, '남', '010-8888-6666', '컴퓨터');

SELECT * FROM 학생;
SELECT * FROM 남녀학생총수;
```

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
    - 테이블 함수는 각 행이 하나 이상의 열로 구성된 테이블을 반환

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
  - 전송되는 SQL 명령문의 양을 줄이고 미리 컴파일된 코드를 호출함으로써 반복적인 처리시 성능 향상

- 트리거

  - 복잡한 데이터 무결성을 강화, 다양한 데이터 처리 업무 규칙을 구현
  - 성능이 다소 저하될 수 있음

- 사용자 정의 함수
  - 특정 값뿐만 아니라 테이블도 반환할 수 있어 제한된 SQL 명령문의 기능을 확장시키고 명령문 작성의 편의성을 향상시킴

|  구분  |                                  저장 프로시저                                   |                                         트리거                                         |                                 사용자 정의 함수                                  |
| :----: | :------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
| 공통점 |              데이터베이스 객체, DBMS 안에 저장, 절차적 언어로 작성               |
| 차이점 | CREATE PROCEDURE CALL 문으로 직접 호출 실행, SQL 문을 포함 반환값 제공(선택사항) | CREATE TRIGGER INSERT,UPDATE,DELETE문 실행시 자동 호출 실행, SQL문을 포함, 반환값 없음 | CREATE FUNCTION SELECT문 실행시 간접 호출 실행 SQL문에 포함 반환값 제공(필수사항) |

### 1.6 트랜잭션

- 트랜잭션(transaction)
  - 한 묶음으로 처리되어야 하는 SQL 명령문들의 집합
    - 트랜잭션에 속한 SQL 명령문들은 모두 정상 처리되어야 함께 완료되며 하나의 명령문이라도 오류가 발생하면 전체를 취소함

![database50](/assets/images/2023-12-06/database50.jpeg)

- 커밋(commit): 정상 종료

  - 트랜잭션의 실행 결과를 데이터베이스에 최종적으로 반영하는 것
  - 임시로 실행 처리한 트랜잭션의 실행 결과를 실제 데이터베이스에 반영하는 명령

- 롤백(rollback)

  - 실행 결과를 반영하지 않고 취소하여 원래 상태로 되돌리는 것

- 롤백이 가능하므로 커밋되기 전까지는 데이터베이스에 완전하게 반영되었다고 확신할 수 없는 임시 실행 결과일 뿐임

* 2개의 UPDATE문으로 구성된 '계좌이체' 트랜잭션의 예

![database51](/assets/images/2023-12-06/database51.jpeg)

- 2개의 UPDATE문은 모두 실행되어야 하고, 둘 중 하나의 UPDATE 문만 실행되어서는 곤란하므로 트랜잭션으로 묶어 처리해야 함
- START TRANSACTION 명령문과 COMMIT 명령문으로 2개의 UPDATE문을 둘러쌈으로써 트랜잭션을 구성

  1.6.1 트랜잭션의 ACID 특성

* 원자성(Atomicity)

  - 트랜잭션 안의 SQL 명령문을 모두 성공적으로 실행하여 오나료하거나 아니면 모두 철회하여 무효화해야함을 의미
    - '전부 혹은 전무(all or nothing)' 실행 규칙을 적용
    - 만약 장애가 발생한다면 장애 발생 시점에 이미 정상 완료되어 커밋된 트랜잭션은 제외하고 완료되지 않은 트랜잭션은 모두 취소시킴

* 일관성(Consistency)

  - 데이터베이스가 트랜잭션 실행 전의 일관된 상태에서 트랜잭션 실행 후에도 또 다른 일관된 상태로 전환되어야함을 의미

* 고립성(Isolation)

  - 커밋될 때까지 트랜잭션이 수행한 임시 실행 결과가 다른 트랜잭션에게 공개되지 않아야함을 의미

* 지속성(Durability)
  - 일단 트랜잭션이 커밋되면 그 트랜잭션의 실행 결과는 장애가 발생하더라도 보존되어야함을 의미

### 트랜잭션의 ACID 특성

1.6.1 트랜잭션의 ACID 특성

- 트랜잭션의 특성과 기법

![database52](/assets/images/2023-12-06/database52.jpeg)

- 트랜잭션 지원 DBMS 모듈

  - 동시성 제어(concurrency control) 모듈

    - 동시에 실행되는 트랜잭션 간의 간섭을 제어
    - 최종적으로 각 트랜잭션이 순차적으로 실행한 결과와 동일한 고립성 결과를 보장하고 트랜잭션 실행 이전과 이후의 데이터베이스 일관성이 항상 유지되도록 함

  - 회복(recovery) 모듈

    - 완전한 트랜잭션 결과의 복구를 보장
    - 장애 발생 시 트랜잭션 실행의 원자성을 보장하고 커밋된 트랜잭션의 결과는 반드시 데이터베이스에 반영되도록 지속성을 지원

  - 대표적 동시성 제어 기법은 락킹(locking)
  - 대표적인 회복 기법은 로깅(logging)

    1.6.2 트랜잭션의 종류

- 트랜잭션의 종류

  - 트랜잭션은 설정 모드에 따라 3가지 트랜잭션으로 구분

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

* 명시적 트랜잭션 적용 예

```SQL
START TRANSACTION;
  DELETE FROM 학생 WHERE 성별 = '남';
  SELECT * FROM 학생;
ROLLBACK;
  SELECT * FROM 학생;
```

1.6.4 자동완료 트랜잭션

- SQL 문 하나를 독립된 하나의 트랜잭션으로 자동 정의하고 SQL 문의 실행 결과에 따라 자동으로 커밋 또는 롤백하는 트랜잭션
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
SELECT * FROM 과목;
```

1.6.5 수동완료 트랜잭션

- 트랜잭션의 끝만 사용자가 직접 명시적으로 지정하는 트랜잭션
- '암시적(implicit) 트랜잭션'
- 다음 명령문 실행하면 START TRANSACTION 명령문 없이도 트랜잭션이 자동으로 시작

```SQL
CREATE, ALTER, INSERT, UPDATE, DELETE, SELECT, DROP, FETCH, GRANT, OPEN, REVOKE, TRUNCATE TABLE
```

- 수동완료 트랜잭션의 설정
  - 다음과 같이 수동완료 모드를 설정하면 START TRANSACTION 명령문은 필요 없으나 COMMIT 명령문이나 ROLLBACK 명령문은 반드시 명세해야 함

```SQL
SELECT @@AUTOCOMMIT; -- 자동커밋 현재 설정 상태 확인
SET AUTOCOMMIT = 0; -- 수동완료 모드 설정(자동완료 모드 해제)
```

1.6.5 수동완료 트랜잭션

- 수동완료 트랜잭션의 적용 예

```SQL
DELETE FROM 과목 WHERE 이름 = '연극학개론';
SELECT * FROM 과목;
INSERT INTO 과목 VALUES('c008', '독서와토론', '111', '교양학부', 2);
SELECT * FROM 과목;
ROLLBACK;
SELECT * FROM 과목;
SET AUTOCOMMIT = 1;  -- 자동완료 모드 설정
```

1.6.6 트랜잭션과 로그

- DBMS의 트랜잭션 처리 과정

![database54](/assets/images/2023-12-06/database54.jpeg)

- 로그 데이터베이스에 기록되는 트랜잭션 로그의 기본 구조
  - 로그 일련번호를 포함하여 트랜잭션\_식별자와 START, COMMIT, ROLLBACK, 명령문 유형과 변경 내용 등을 기록

```SQL
[트랜잭션_식별자, 로그_유형, 데이터_항목, 변경전_값, 변경후_값]
```

- 트랜잭션 로그의 예

```SQL
[T1, START]
[T1, UPDATE, 학생(김연아), 중간성적, 88, 99]
[T2, START]
[T2, UPDATE, 학생(이승엽), 기말성적, 89, 93]
[CHECKPOINT]
[T2, UPDATE, 학생(홍길동), 기말성적, 77, 66]
[T1, UPDATE, 학생(이영애), 중간성적, 91, 97]
[T1, COMMIT]
[T2, COMMIT]
```

1.6.7 체크 포인트

- 체크 포인트(check point)

  - '검사점'
  - 특정 시점까지의 데이터 버퍼 안의 모든 변경 내용을 데이터베이스 파일에 물리적으로 저장하는 시점
  - 장애 발생 시 복구 작업량을 줄이기 위해서 주기적으로 동기화를 수행
  - 체크 포인트 역시 로그에 기록

- 체크 포인트 시점에는 버퍼 캐시 안의 데이터와 디스크 안의 저장 데이터베이스가 100% 일치하는 동기화가 수행

  - 체크 포인트가 트랜잭션 로그에 기록되는 순간 모든 데이터 버퍼 안의 데이터들이 데이터베이스 파일에 쓰여짐
  - 회복 대상을 마지막 체크 포인트 시점 이후의 로그 내용으로만 제한시킴

    1.6.8 로그를 이용한 회복 기법

- 회복의 기본 개념

  - 장애 발생 시점에 이미 커밋된 트랜잭션의 변경 내용은 로그를 이용하여 반드시 데이터베이스에 반영함. 반면 커밋되지 못하고 중단된 트랜잭션의 실행 결과는 철회되어야 함

- 트랜잭션 로그를 이용한 회복 기법의 적용 과정

  - 회복은 장애 발생 시점에 중단된 트랜잭션의 작업 내용을 롤백(rollback) 즉 철회시키기 위해서 로그 기록의 역순으로 이전 상태로 되돌리는 취소(UNDO) 작업을 진행한다.
  - 마지막 체크 포인트 이후의 커밋되지 않은 트랜잭션의 로그 내용들은 모두 롤백(roll-back) 즉, 순서의 역순으로 취소된다.
  - 반대로, 장애 발생 시점까지 정상적으로 종료된 트랜잭션의 실행 결과를 보장하기 위해서는 로그 기록의 순서대로 다시 실행하는 재실행(REDO) 작업을 진행한다.
  - 마지막 체크 포인트 이후의 커밋된 트랜잭션의 로그 내용들은 모두 롤포워드(rollfor-ward) 즉, 로그 순서대로 재실행한다.

- 지연 갱신(deferred update) 모드 가정

```SQL
[T1, START]
[T1, UPDATE, 학생(김연아), 중간성적, 88, 99]
[T1, START]
[T2, UPDATE, 학생(이승엽), 기말성적, 89, 93]
[CHECKPOINT]
[T2, UPDATE, 학생(홍길동), 기말성적, 77, 66]
[T1, UPDATE, 학생(이영애), 중간성적, 91, 97]
[T1, COMMIT]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~장애발생
```

![database55](/assets/images/2023-12-06/database55.jpeg)

1.6.9 트랜잭션과 로크

- 데이터베이스는 적절한 절차 없이 동시 접근을 허용할 경우, 심각한 문제 발생 가능
- 로크 잠금 - 다중 사용자 환경에서 동시 접근을 올바르게 제어하기 위한 대표적 기법

![database56](/assets/images/2023-12-06/database56.jpeg)

1.6.10 로크

- 로크의 개념

  - 바람직한 동시성(concurrency)이란 2개 이상의 트랜잭션을 동시에 실행하는 비직렬 스케쥴(non-serial schedule)의 결과가 트랜잭션을 뒤섞지 않고 순차적으로 실행하는 직렬 스케쥴(serial schedule)의 결과와 같도록 보장하는 것임
  - 이를 보장하는 트랜잭션 스케쥴을 '직렬 가능(serializable)'이라고 함
  - 직렬 가능성(serializability)을 보장하기 위한 방법 중 하나가 로크를 이용하는 것

- 로크(lock) 또는 잠금

  - 데이터베이스의 데이터를 다른 사용자가 접근하지 못하도록 잠그는 것
  - 다수 트랜잭션의 동시 처리를 보장하는 동시성 제어 기법 중 대표적인 기법

    - 트랜잭션 내에서 데이터에 SQL 명령문을 실행하기 전에 반드시 해당 데이터에 로크를 설정(lock)하여 잠가야 함
    - 설정된 로크는 SQL 명령문 실행 직후가 아닌 트랜잭션 커밋 혹은 롤백이 실행됨과 동시에 일제히 해제(unlock)해야 함

      1.6.11 로크의 종류

- 공유 로크(shared lock)

  - 데이터를 검색하는 SELECT문을 실행하기 위한 읽기 전용 로크
  - 읽기만 가능하고 수정은 불가능
  - 공유 로크가 설정된 데이터에 대해서는 추가적으로 공유 로크 설정 가능

- 독점 로크(exclusive lock)
  - INSERT, UPDATE, DELETE문과 같은 데이터 변경을 위한 배타적 로크
  - 특정 데이터에 대해서 오직 하나의 트랜잭션만 독점 로크를 설정
  - 이미 독점 로크가 설정되어 있을 경우, 공유 로크도 독점 로크도 모두 추가 설정 불가능
  - 데이터에 대한 모든 공유 로크와 독점 로크가 해제된 이후에만 독점 로크 설정 가능

|   종류    |                          공유 락                          |              독점 락               |
| :-------: | :-------------------------------------------------------: | :--------------------------------: |
| 잠금 목적 |                         읽기 목적                         |             쓰기 목적              |
| 잠금 시작 |                     SELECT문 실행 시                      | INSERT, UPDATE, DELETE문 실행 직전 |
| 잠금 해제 |         SELECT문 실행 직후 또는 트랜잭션 종료 후          |          트랜잭션 종료 후          |
|   특성    | 다른 공유 로크와는 양립하지만 독점 로크와는 양립하지 않음 |  어떤 다른 로크와도 양립하지 않음  |

1.6.12 로크 설정과 해제

- 로크 단위와 로크 설정

  - 데이터베이스 안의 모든 데이터를 접근할 때는 먼저 로크 테이블(lock table) 안에 해당 데이터에 대한 로크 정보를 기록
  - 로크 단위(lock granularity): 로크 잠금 대상의 크기
    - 테이블 행 단위나 테이블, 데이터베이스 단위로 로크 설정 가능
    - 로크 단위가 커지면 관리는 쉬워지지만 로크의 충돌이 자주 발생함
    - 로크를 어느 단위로 설정하고 해제할지를 결정하는 것도 결정해야 하는 중요한 문제

- 로그 양립성(lock compatability)
  - 독점 로크와 공유 로크 2가지 유형의 로크 사이에 서로 추가 잠금 허용 여부를 결정하는 규칙

![database57](/assets/images/2023-12-06/database57.jpeg)

1.6.13 2-단계 로킹 규약(2-phased locking protocol)

- 각 트랜잭션별로 로크를 설정하는 과정과 로크를 해제하는 과정 2단계로 진행함으로써 필요한 순간까지 획득한 로크를 유지하도록 하는 규칙

  - 1단계: 로크 확장 단계(growing phase)
    - 접근하고자 하는 데이터에 대한 모든 로크를 획득할 때까지 새로운 로크를 지속적으로 요청하여 잠금을 설정
    - 보유하고 있는 로크를 해제할 수는 없고 로크를 추가로 획득할 수만 있음
  - 2단계: 로크 축소 단계(shrinking phase)
    - 필요로 하는 모든 로크를 획득하는 시점인 로크 포인트(lock point)가 되면 보유하고 있던 로크를 점차적으로 해제
    - 일단 하나라도 로크를 해제하기 시작한다면 다시 새로운 로크를 요청할 수는 없음

![database58](/assets/images/2023-12-06/database58.jpeg)

1.6.14 트랜잭션의 고립 수준

- 로크 설정목적은 적절한 수준에서 트랜잭션 동시 실행이 이루어지도록 하기 위함

- 고립 수준(isolation level)

  - 트랜잭션이 다른 트랜잭션과 고립되는 정도를 의미
  - 필요로 하는 트랜잭션의 실행 수준에 따른 다양한 고립 수준을 정의하고 상황에 맞는 적절한 수준의 잠금 전략을 수행하도록 함

- SQL 명령문에서 설정 가능한 4가지 트랜잭션 고립 수준

![database59](/assets/images/2023-12-06/database59.jpeg)

1.6.15 고립 수준에 따라 발생할 수 있는 문제 유형

- 오손 데이터 읽기(dirty data read) 문제

  - 커밋되지 않은 트랜잭션 T1의 수정된 중간 결과를 읽어오는 것이 허용된다면 발생할 수 있는 문제
  - 트랜잭션 T2가 T1의 중간 결과 값을 읽어 온 뒤에 T1이 최종적으로 커밋되지 않고 롤백되는 경우, T2가 읽은 결과 값은 존재하지 않는 데이터 즉, 오손 데이터(dirty data)를 읽어 온 결과가 됨

- 반복 불가능 읽기(non-repeatable read) 문제
  - 트랜잭션 T1이 읽어서 처리 중인 데이터를 트랜잭션 T2가 자유롭게 변경하는 것이 허용된다면 발생할 수 있는 문제
  - T1이 같은 값을 반복해서 읽을 때 그 사이 T2가 변경한다면 값이 다를 수 있어 반복 읽기가 불가능한 상황이 됨

* 유령 데이터 읽기(phantom data read) 문제

  - 트랜잭션 T1이 읽어서 처리 중인 데이터를 트랜잭션 T2가 변경하는 것을 방지하더라도 트랜잭션 T2의 데이터 추가가 허용된다면 발생할 수 있는 문제
  - 트랜잭션 T1이 같은 값을 반복해서 읽을 때 이전에 없던 트랜잭션 T2가 중간에 추가한 데이터 즉, 유령 데이터(phantom data)가 나타날 수 있음

  1.6.16 고립 수준 설정

* 사용자가 SET 명령문을 통해 허용 가능한 고립 수준을 설정하면 고립 수준에 따라서 트랜잭션 간의 로크 설정 및 유지 방식이 결정

* 고립 수준을 정의하는 명령문의 형식

```SQL
SET TRANSACTION ISOLATION LEVEL 고립_수준_키워드;
```

- 4가지 고립 수준을 설정하는 예
  - 한 번 설정된 고립 수준은 재설정 전까지는 계속해서 수행되는 트랜잭션들에게 적용됨

```SQL
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;  -- 고립수준 0
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  -- 고립수준 1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  -- 고립수준 2
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  -- 고립수준 3
```

1.6.17. 트랜잭션 고립 수준 적용 예

```SQL
-- <사용자 1>
SET SQL_SAFE_UPDATES = 0;
USE univDB;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  -- 고립수준 1

START TRANSACTION;
SELECT * FROM 고객;
SELECT * FROM 고객;  -- 대기상태(이전 버전을 읽음)
UPDATE 고객 SET 나이 = 나이 + 100;  -- 대기상태
SELECT * FROM 고객;
ROLLBACK;

SELECT * FROM 고객;
```

```SQL
-- <사용자 2>
SET SQL_SAFE_UPDATES = 0;
USE univDB;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  -- 고립수준 1

START TRANSACTION;
SELECT * FROM 고객;
UPDATE 고객 SET 나이 = 10 WHERE 성별 = '여';
SELECT * FROM 고객;
COMMIT;
```
