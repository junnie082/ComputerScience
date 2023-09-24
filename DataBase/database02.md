# Database02

## 데이터 입력과 삭제

#### 2023.09.24.(일)

## Insert

```bash
mysql> INSERT INTO emp03 (emp_id, emp_name, gender, age, hire_date) VALUES (2, '김유신', '남성', 44, '2018-01-01');
```

테이블에 값 넣기

```bash
mysql> select * from emp03;
```

emp03 테이블 확인하기

### rollback 과 commit

내 mysql 은 매 명령어 전마다 `start transaction;` 을 해줘야 `rollback`이 적용 되었음.
무언가를 잘못해서 적용을 되돌리고 싶을 때 `rollback`, 문제가 없다면 `commit`을 해주어 데이터베이스에 적용을 한다.

## Delete

```bash
mysql> delete from emp03;
```

emp03 테이블이 모두 삭제됨.

```bash
mysql> delete from emp03
    -> where emp_id = 2;
```

emp03 테이블이 emp_id가 2인 로우가 삭제됨.

## subway_statistics 테이블 만들기

![sql1](/assets/images/2023-09-24/sql1.png)
![sql2](/assets/images/2023-09-24/sql2.png)

varchar2(100)로 데이터 타입을 설정하면 오류가 나고, varchar(100)으로 설정했더니 잘 작동하였다.
