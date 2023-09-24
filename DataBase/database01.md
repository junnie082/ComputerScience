# Database01

## 데이터베이스 세팅 및 기본 문법

#### 2023.09.23.(토)

## [mySql 설치 및 시작, 종료, 재시작](https://2day-is-seonday.tistory.com/entry/Mac맥-Homebrew-MySql-설치-및-서버-시작-종료-재시작하기)

- `brew install mysql`: mysql 서버 설치
- `mysql.server start`: mysql 서버 시작
- `mysql.server restart`: mysql 서버 재시작
- `mysql.server stop`: mysql 서버 종료


## [mySql 설치](https://velog.io/@ryong9rrr/mySQL-기본-사용법-정리예제)

- `mysql -uroot -p`: 나의 mysql 계정으로 접속한다.
- `SHOW DATABASES;`: 생성된 데이터베이스를 확인할 수 있다.
- `use 데이터베이스이름;`: 해당 데이터베이스에 접속한다.
- `SHOW TABLES;`: 테이블의 목록을 확인할 수 있다.

## [SQL 기본 문법](http://www.tcpschool.com/mysql/mysql_basic_alter)

## [테이블 만들기, 수정하기](https://blog.naver.com/pjok1122/221539169731)

### 테이블 만들기

```bash
mysql> SHOW DATABASES;
```

데이터베이스 확인

```bash
mysql> CREATE DATABASE testDB;
```

데이터베이스 생성

```bash
mysql> DROP DATABASE testDB;
```

데이터베이스 삭제

```bash
mysql> CREATE DATABASE testDB;
```

```bash
mysql> use testDB;
```

데이터베이스 선택

```bash
CREATE TABLE emp03 (
    emp_id INT NOT NULL,
    emp_name VARCHAR(100) NOT NULL,
    gender VARCHAR(10) NULL,
    age INT NULL,
    hire_date DATE NULL,
    etc VARCHAR(300) NULL,
    PRIMARY KEY (emp_id)
);
```

테이블 생성

```bash
mysql> DESC testTable;
```

앞에서 만든 테이블의 컬럼들과 옵션들을 확인하고 싶다면:

### 테이블 수정하기

```bash
ALTER TABLE testTable
ADD column_name data_type;
```

추가된 컬럼은 반드시 마지막에 위치하게 됨

```bash
ALTER TABLE testTable
DROP COLUMN column_name;
```

```bash
ALTER TABLE table_name CHANGE column_name column_name data_type
```

컬럼의 데이터 유형, 디폴트 값, NOT NULL 제약 조건에 대한 변경을 포함할 수 있음. 타입을 변경할 경우에는 NULL 값이 있는지, 데이터 사이즈가 맞는지 등등을 고려하여 반영해야 함.

```bash
ALTER TABLE table_name
ADD CONSTRAINT constraint_name constraint_condition (column_name);
```

컬럼에 제약조건을 추가할 수 있음.
