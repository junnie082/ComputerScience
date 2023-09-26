# Database03

## 데이터 조회, SELECT 문

#### 2023.09.26.(화)

```sql
SELECT column1, column2, ...
FROM 테이블 명
WHERE 조건
ORDER BY 정렬 순서;
```

- 조건 연산자:

```sql
SELECT *
FROM subway_statistics
WHERE station_name = '잠실(216)'
AND ( boarding_time = 7
      OR boarding_time = 9 );
```

- LIKE 연산자:

```sql
SELECT *
FROM subway_statistics
WHERE station_name LIKE '잠실%';
```

- IN 연산자:

```sql
SELECT *
FROM subway_statistics
WHERE station_name LIKE '선릉%'
AND boarding_time IN (7, 9);
```

`boarding_time IN (7, 9)`는 `boarding_time = 7 OR boarding_time = 9`와 같다.

- BETWEEN 연산자:

```sql
SELECT *
FROM subway_statistics
WHERE station_name LIKE '선릉%'
AND passenger_number BETWEEN 500 AND 1000;
```

`BETWEEN a AND b`에서 a는 b보다 작다.

- ORDER BY

```sql
SELECT *
FROM subway_statistics
ORDER BY station_name;
```

```sql
SELECT *
FROM subway_statistics
WHERE station_name LIKE '선릉%'
ORDER BY 1,2,3,4,5,6
```
