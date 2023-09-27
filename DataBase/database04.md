# Database04

## SQL 연산자와 함수

#### 2023.09.27.(수)

![function1](/assets/images/2023-09-27/database1.png)

![function2](/assets/images/2023-09-27/database2.png)

![function3](/assets/images/2023-09-27/database3.png)

![function4](/assets/images/2023-09-27/database4.png)
![function5](/assets/images/2023-09-27/database5.png)
![function6](/assets/images/2023-09-27/database6.png)
![function7](/assets/images/2023-09-27/database7.png)
![function8](/assets/images/2023-09-27/database8.png)
![function9](/assets/images/2023-09-27/database9.png)
![function10](/assets/images/2023-09-27/database10.png)
![function1](/assets/images/2023-09-27/database1.png)

```sql
SELECT CONCAT('MY', 'SQL') FROM DUAL;
SELECT CONCAT('M', 'Y', 'S', 'Q', 'L') FROM DUAL;
```

```sql
mysql> SELECT INSTR('foobar', 'bar');
-> 4

mysql> SELECT INSTR('mysql', 'sql');
-> 3

mysql> SELECT INSTR('habony', 'test');
-> 0
```

```sql
SELECT SYSDATE();
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 SECOND);
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 MINUTE);
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 HOUR);
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 DAY);
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 MONTH);
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR);
```

```sql
SELECT DATE_ADD(NOW(), INTERVAL -1 YEAR);
```

```sql
SELECT CAST(150 AS CHAR);
```

```sql
SELECT CAST("14:06:10" AS TIME);
```

```sql
SELECT STR_TO_DATE('20201103', '%Y%m%d%H%i%s');
```

```sql
SELECT IFNULL(name, '값이 없습니다')
FROM TEST;
```
