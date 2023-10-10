# SQL 1

## Database

> 체계적인 데이터 모음

- 데이터를 저장하고 조작 (CRUD)
  - 구조적 저장

- **데이터**: 저장이나 처리에 효율정인 형태로 변환된 정보



## 관계형 데이터베이스(Relational Database)

> 데이터 간에 관계가 있는 데이터 항목들의 모음

- **관계**: 여러 테이블 간의 (논리적) 연결
- 테이블, 행, 열의 정보를 구조화하는 방식
- 서로 관련된 데이터 포인터를 저장하고 이에 대한 액세스를 제공
- 이 관계로 인해 두 테이블을 사용하여 데이터를 다양한 형식으로 조회할 수 있음
- 데이터는 기본 키 또는 외래 키를 통해 결합(join)될 수 있는 여러 테이블에 걸쳐 구조화됨



### 관계형 데이터베이스 관련 키워드

- Table (Relation)

- Field (Column, Attribute)
  - 각 필드에는 고유한 데이터 형식(타입)이 지정됨

- Record (Row, Tuple)
  - 각 레코드에는 구체적인 데이터 값이 저장됨
- Database (Schema)
  - 테이블의 집합

- Primary Key (기본 키)
  - 각 레코드의 고유한 값
  - 관계형 데이터베이스에서 레코드의 식별자로 활용
-  Foreign Key (외래 키)
  - 테이블의 필드 중 다른 테이블의 레코드를 식별할 수 있는 키
  - 다른 테이블의 기본 키를 참조
  - 각 레코드에서 서로 다른 테이블 간의 관계를 만드는 데 사용



### DBMS(Database Management System)

> 데이터베이스를 관리하는 소프트웨어 프로그램

- 데이터 저장 및 관리를 용이하게 하는 시스템
- 데이터베이스와 사용자 간의 인터페이스 역할
- 사용자가 데이터 구성, 업데이트, 모니터링, 백업, 복구 등을 할 수 있도록 도움



### RDBMS(Relational Database Management System)

> 관계형 데이터베이스를 관리하는 소프트웨어 프로그램



### SQLite

> 경량의 오픈 소스 데이터베이스 관리 시스템

- 컴퓨터나 모바일 기기에 내장되어 간단하고 효율적인 데이터 저장 및 관리를 제공



## SQL(Structure Query Language)

> 데이터베이스에 정보를 저장하고 처리하기 위한 프로그래밍 언어

- 테이블의 형태로 구조화된 관계형 데이터베이스에게 요청을 질의(요청)
- 관계형 데이터베이스와의 대화를 하기 위해 사용되는 프로그래밍 언어



### SQL Syntax

```sql
SELECT column_name FROM table_name;
```

- SQL 키워드는 대소문자를 구분하지 않음
  - 하지만 대문자로 작성하는 것을 권장 (명시적 구분)
- 각 SQL Statements의 끝에는 세미콜론(;)이 필요
  - 세미콜론은 각 SQL Statements을 구분하는 방법 (명령어의 마침표)



### SQL Statements

```sql
SELECT column_name FROM table_name;
```

- 해당 예시 코드는 SELECT Statement라 부름
- 이 Statement는 SELECT, FROM 2개의 keyword로 구분됨



### 수행 목적에 따른 SQL Statements 4가지 유형

|               유형               |                  역할                  |                 SQL 키워드                  |
| :------------------------------: | :------------------------------------: | :-----------------------------------------: |
|  DDL (Data Definition Language)  |    데이터의 기본 구조 및 형식 변경     |         CREATE<br />DROP<br />ALTER         |
|    DQL (Data Query Language)     |              데이터 검색               |                   SELECT                    |
| DML (Data Manipulation Language) |     데이터 조작 (추가, 수정, 삭제)     |       INSERT<br />UPDATE<br />DELETE        |
|   DCL (Data Control Language)    | 데이터 및 작업에 대한 사용자 권한 제어 | COMMIT<br />ROLLBACK<br />GRANT<br />REVOKE |



### Query

- 데이터베이스로부터 정보를 요청하는 것
- 일반적으로 SQL로 작성하는 코드를 쿼리문(SQL문)이라 함



## Querying data

### SELECT statement

> 테이블에서 데이터를 조회

- '*' (asterisk)를 사용하여 모든 필드 선택



#### SELECT syntax

```sql
SELECT
	select_list
FROM
	table_name;
```

- SELECT 키워드 이후 데이터를 선택하려는 필드를 하나 이상 지정
- FROM 키워드 이후 데이터를 선택하려는 테이블의 이름을 지정



##### SELECT syntax 활용

- 테이블 employees에서 LastName 필드의 모든 데이터를 조회

```sql
SELECT
  LastName
FROM
  employees;
```



- 테이블 employees에서 LastName, FirstName 필드의 모든 데이터를 조회

```sql
SELECT
  LastName, FirstName
FROM
  employees;
```



- 테이블 employees에서 모든 필드 데이터를 조회

```sql
SELECT
  *
FROM
  employees;
```



- 테이블 employees에서 FirstName 필드의 모든 데이터를 조회
  - 단, 조회 시 FirstName이 아닌 '이름'으로 출력될 수 있도록 변경

```sql
SELECT
  FirstName AS '이름'
FROM
  employees;
```



- 테이블 track에서 Name, Milliseconds 필드의 모든 데이터 조회
  - 단, Milliseconds 필드는 60000으로 나눠 분 단위 값으로 출력

```sql
SELECT
  Name, Milliseconds / 60000 AS '재생 시간(분)'
FROM
  tracks;
```



## Sorting data

### ORDER BY statement

> 조회 결과의 레코드를 정렬



#### ORDER BY syntax

```sql
SELECT
  select_list
FROM
  table_name
ORDER BY
  clumn1 [ASC|DESC],
  clumn2 [ASC|DESC],
  ...;
```

- FROM clause 뒤에 위치
- 하나 이상의 컬럼을 기준으로 결과를 오름차순(ASC, 기본 값), 내림차순(DESC)으로 정렬



##### ORDER BY 활용

- 테이블 employees에서 FirstName 필드의 모든 데이터를 오름차순/내림차순으로 조회

```sql
SELECT 
  FirstName
FROM
  employees
ORDER BY
  FirstName;

SELECT 
  FirstName
FROM
  employees
ORDER BY
  FirstName DESC;
```



- 테이블 customers에서 Country 필드를 기준으로 내림차순으로 정렬한 다음 City 필드 기준으로 오름차순으로 정렬하여 조회

```sql
SELECT
  Country, City
FROM
  customers
ORDER BY
  Country DESC,
  City ASC;
```



- 테이블 tracks에서 Milliseconds 필드를 기준으로 내림차순으로 정렬한 다음 Name, Milliseconds 필드의 모든 데이터를 조회
  - 단, Milliseconds 필드는 60000으로 나눠 분 단위 값으로 출력

```sql
SELECT
  Name, Milliseconds / 60000 AS '재생 시간(분)'
FROM
  tracks
ORDER BY
  Milliseconds DESC;
```



#### 정렬에서의 NULL

- NULL 값이 존재할 경우 오름차순 정렬 시 결과에 NULL이 먼저 출력



## Filtering data

### DISTINCT statement

> 조회 결과에서 중복된 레코드를 제거



#### DISTINCT syntax

```sql
SELECT DISTINCT
  select_list
FROM
  table_name;
```

- SELECT 키워드 바로 뒤에 작성
- SELECT DISTINCT 키워드 다음에 고유한 값을 선택하려난 하나 이상의 필드를 지정



##### DISTINCT 활용

- 테이블 customers에서 Country 필드의 모든 데이터를 중복 없이 오름차순 조회

```sql
SELECT DISTINCT
  Country
FROM
  customers
ORDER BY
  Country;
```



### WHERE statement

> 조회 시 특정 검색 조건을 지정



#### WHERE syntax

```sql
SELECT
  select_list
FROM
  table_name
WHERE
  search_condition;
```

- FROM clause 뒤에 위치
- search_condition은 비교연산자 및 논리연산자(AND, OR, NOT 등)를 사용하는 구문이 사용됨



##### WHERE 활용

- 테이블 customers에서 City 필드 값이 'Prague'인 데이터의 LastName, FirstName, City 조회
  - 부정은 '!=' 사용

```sql
SELECT
  LastName, FirstName, City
FROM
  customers
WHERE
  City = 'Prague';
```



- 테이블 customers에서 Company 필드 값이 NULL이고 Country  필드 값이 'USA'인 데이터의 LastName, FirstName, Company, Country 조회

```sql
SELECT
  LastName, FirstName, Company, Country
FROM
  customers
WHERE
  Company IS NULL 
  AND Country = 'USA';
```



- 테이블 tracks에서 Bytes 필드 값이 100,000 이상 500,000 이하인 데이터의 Name, Bytes 조회

```sql
SELECT
  Name, Bytes
FROM
  tracks
WHERE
  -- 100000 <= Bytes <= 500000;
  Bytes BETWEEN 100000 AND 500000;
```



- 테이블 tracks에서 Bytes 필드 값이 100,000 이상 500,000 이하인 데이터의 Name, Bytes을 Bytes를 기준으로 오름차순 조회

```sql
SELECT
  Name, Bytes
FROM
  tracks
WHERE
  Bytes BETWEEN 100000 AND 500000
ORDER BY
  Bytes;
```



- 테이블 customers에서 Country 필드 값이 'Canada' 또는 'Germany' 또는 'France'인 데이터의 LastName, FirstName, Country 조회
  - 부정은 'NOT IN' 사용

```sql
SELECT
  LastName, FirstName, Country
FROM 
  customers
WHERE
  Country IN ('Canada', 'Germany', 'France');
  -- Country = 'Canada'
  -- OR Country = 'Germany'
  -- OR Country = 'France';
```



- 테이블 customers에서 LastName 필드 값이 son으로 끝나는 데이터의 LastName, FirstName 조회

```sql
SELECT
  LastName, FirstName
FROM
  customers
WHERE
  LastName LIKE '%son';
```



- 테이블 customers에서 FirstName 필드 값이 4자리면서 'a'로 끝나는 데이터의 LastName, FirstName 조회

```sql
SELECT
  LastName, FirstName
FROM
  customers
WHERE
  FirstName LIKE '___a';
```



#### Operators

**IN Operator**

- 값이 특정 목록 안에 있는지 확인



**LIKE Operator**

- 값이 특정 패턴에 일치하는지 확인 (Widcards와 함께 사용)



**Widcard Characters**

- '%' : 0개 이상의 문자열과 일치하는지 확인
- '_' : 단일 문자와 일치하는지 확인



### LIMIT clause

> 조회하는 레코드 수를 제한



#### LIMIT syntax

```sql
SELECT
  select_list
FROM
  table_name
LIMIT [offset,] row_count;
```

- 하나 또는 두 개의 인자를 사용 (0 또는 양의 정수)
- row_count는 조회하는 최대 레코드 수를 지정



##### LIMIT 활용

- 테이블 tracks에서 TrackId, Name, Bytes 필드 데이터를 Bytes 기준 내림차순으로 7개만 조회

```sql
SELECT
  TrackId, Name, Bytes
FROM
  tracks
ORDER BY
  Bytes DESC
LIMIT 7;
```



- 테이블 tracks에서 TrackId, Name, Bytes 필드 데이터를 Bytes 기준 내림차순으로 4번째부터 7번째 데이터만 조회

```sql
SELECT
  TrackId, Name, Bytes
FROM
  tracks
ORDER BY
  Bytes DESC
LIMIT 3, 4;
-- LIMIT 4 OFFSET 3;
```



## Grouping data

### GROUP BY clause

> 레코드를 그룹화하여 요약본 생성 ('집계 함수'와 함께 사용)



#### 집계 함수(Aggregation Functions)

> 값에 대한 계산을 수행하고 단일한 값을 반환하는 함수
>
> SUM, AVG, MAX, MIN, COUNT



#### GROUP BY syntax

```sql
SELECT
  c1, c2,..., cn, aggregate_function(ci)
FROM
  table_name
GROUP BY
  c1, c2, ..., cn;
```

- FROM 및 WHERE 절 뒤에 배치
- GROUP BY 절 뒤에 그룹화할 필드 목록을 작성



##### GROUP BY 활용

- COUNT 함수가 각 그룹에 대한 집계된 값을 계산

```sql
SELECT
  Country, COUNT(*)
FROM
  customers
GROUP BY
  Country;
```



- 테이블 track에서 Composer 필드를 그룹화하여 각 그룹에 대한 Bytes의 평균 값을 내림차순 조회

```sql
SELECT
  Composer, 
  AVG(Bytes) AS avgOfBytes
FROM
  tracks
GROUP BY
  Composer
ORDER BY
  avgOfBytes DESC;
```



- 테이블 tracks에서 Composer 필드를 그룹화하여 각 그룹에 대한 Milliseconds의 평균 값이 10 미만인 데이터 조회
  - 단, Milliseconds 필드는 60000으로 나눠 분 단위 값의 평균으로 계산

```sql
-- 에러 발생
SELECT
  Composer,
  AVG(Milliseconds / 60000) AS avgOfMinute
FROM
  tracks
WHERE
  avgOfMinute < 10
GROUP BY
  Composer;
  
-- 에러 해결
SELECT
  Composer,
  AVG(Milliseconds / 60000) AS avgOfMinute
FROM
  tracks
GROUP BY
  Composer
HAVING
  avgOfMinute < 10;
```



#### HAVING clause

> 집계 항목에 대한 세부 조건을 지정

- 주로 GROUP BY와 함께 사용되며 GROUP BY가 없다면 WHERE처럼 동작



# SQL 2

## Create a table

### CREATE TABLE statement

> 테이블 생성



#### CREATE TABLE syntax

```sql
CREATE TABLE table_name (
  column_1 date_type constraints,
  column_1 date_type constraints,
  ...,
)
```

- 각 필드에 적용할 데이터 타입 작성
- 테이블 및 필드에 대한 제약조건(constraints) 작성



##### CREATE TABLE 활용

- example 테이블 생성 및 확인

```sql
CREATE TABLE examples (
  ExamId INTEGER PRIMARY KEY AUTOINCREMENT,
  LastName VARCHAR(50) NOT NULL,
  FirstName VARCHAR(50) NOT NULL
);
```



- 테이블 스키마(구조) 확인

```sql
PRAGMA table_info('examples');

-- cid
-- "Column Id"를 의미하는 컬럼
-- 각 컬럼의 고유한 식별자를 나타내는 정수 값
-- 일반적으로는 사용자가 직접 사용하지 않으며,
-- PRAGMA와 같은 메타데이터 조회 작업에서 컬럼의 정보를
-- 식별하는 용도로 활용됨
```



#### SQLite 데이터 타입

- NULL: 아무런 값도 포함하지 않음을 나타냄
- INTEGER: 정수
- REAL: 부동 소수점
- TEXT: 문자열
- BLOB: 이미지, 동영상, 문서 등의 바이너리 데이터



#### 제약 조건(Constraints)

> 테이블의 필드에 적용되는 규칙 또는 제한 사항
>
> (데이터의 무결성을 유지하고 데이터베이스의 일관성을 보장)

- **PRIMARY KEY**
  - 해당 필드를 기본 키로 지정
  - INTEGET 타입에만 적용되며, INT, BIGINT 등과 같은 정수 유형은 적용되지 않음
- **NOT NULL**
  - 해당 필드에 NULL 값을 허용하지 않도록 지정
- **FOREIGN KEY**
  - 다른 테이블과의 외래 키 관계를 정의



#### AUTOINCREMENT keyword

> 자동으로 고유한 정수 값을 생성하고 할당하는 필드 속성

- 필드의 자동 증가를 나타내는 특수한 키워드
- 주로 primary key 필드에 적용
- INTEGER PRIMARY KEY AUTOINCREMENT가 작성된 필드는 항상 새로운 레코드에 대해 이전 최댓값보다 큰 값을 할당
- 삭제된 값은 무시되며 재사용할 수 없게 됨



## Modifying table fields

### ALTER TABLE statement

> 테이블 및 필드 조작



#### ALTER TABLE 역할

|          명령어           |       역할       |
| :-----------------------: | :--------------: |
|  ALTER TABLE ADD COLUMN   |    필드 추가     |
| ALTER TABLE RENAME COLUMN |  필드 이름 변경  |
|  ALTER TABLE DROP COLUMN  |    필드 삭제     |
|   ALTER TABLE RENAME TO   | 테이블 이름 변경 |



#### ALTER TABLE ADD COLUMN syntax

```sql
ALTER TABLE
  table_name
ADD COLUMN
  column_definition;
```

- ADD COLUMN 키워드 이후 추가하고자 하는 새 필드 이름과 데이터 타입 및 제약 조건 작성



##### ALTER TABLE ADD COLUMN 활용

- example  테이블에 조건에 맞는 Country 필드 추가

```sql
ALTER TABLE
  examples
Add COLUMN
  Country VARCHAR(100) NOT NULL;
```



- example 테이블에 조건에 맞는 Age, Address 필드 추가

```sql
-- ALTER TABLE
--   examples
-- Add COLUMN
--   Age INTEGER NOT NULL,
--   Address VARCHAR(100) NOT NULL;

-- sqlite는 단일 문을 사용하여 한번에 여러 열을
-- 추가하는 것을 지원하지 않음

ALTER TABLE
  examples
Add COLUMN
  Age INTEGER NOT NULL;

ALTER TABLE
  examples
Add COLUMN
  Address VARCHAR(100) NOT NULL;
```



#### ALTER TABLE RENAME COLUMN syntax

```sql
ALTER TABLE
  table_name
RENAME COLUMN
  current_name TO new_name;
```

- RENAME COLUMN 키워드 뒤에 이름을 바꾸려는 필드의 이름을 지정하고 TO 키워드 뒤에 새 이름을 지정



#### ALTER TABLE DROP COLUMN syntax

```sql
ALTER TABLE
  table_name
DROP COLUMN
  current_name;
```

- DROP COLUMN 키워드 뒤에 삭제하려는 필드의 이름을 지정
- 삭제하는 필드가 다른 부분에서 참조되지 않고 PRIMARY KEY가 아니며 UNIQUE 제약 조건이 없는 경우에만 작동



#### ALTER TABLE RENAME TO syntax

```sql
ALTER TABLE
  table_name
RENAME TO
  new_table_name;
```

- RENAME TO 키워드 뒤에 새로운 테이블 이름 지정



## DELETE a table

### DROP TABLE statement

> 테이블 삭제



#### DROP TABLE syntax

```sql
DROP TABLE table_name;
```

- DROP TABLE statement 이후 삭제할 테이블 이름 작성



## Insert data

### INSERT statement

> 테이블 레코드 삽입



#### INSERT syntax

```sql
INSERT INTO table_name (c1, c2, ...)
VALUES (v1, v2, ...);
```

- INSERT INTO 절 다음에 테이블 이름과 괄호 안에 필드 목록 작성
- VALUES 키워드 다음 괄호 안에 해당 필드에 삽입할 값 목록 작성



##### INSERT 활용

- articles 테이블에 데이터 입력

```sql
INSERT INTO
  articles (title, content, createdAt)
VALUES
  ('hello', 'world', '2000-01-01');

INSERT INTO
  articles (title, content, createdAt)
VALUES
  ('title1', 'content1', '1900-01-01'),
  ('title2', 'content2', '1800-01-01'),
  ('title3', 'content3', '1700-01-01');

INSERT INTO
  articles (title, content, createdAt)
VALUES
  ('mytitle', 'mycontent', DATE());
```



## Update data

### UPDATE statement

> 테이블 레코드 수정



#### UPDATE syntax

```sql
UPDATE table_name
SET column_name = expression,
[WHERE
  condition];
```

- SET 절 다음에 수정할 필드와 새 값을 지정
- WHERE 절에서 수정할 레코드를 지정하는 조건 작성
- WHERE 절을 작성하지 않으면 모든 레코드를 수정



##### UPDATE 활용

- articles 테이블 1번 레코드의 title 필드 값을 'update Title'로 변경

```sql
UPDATE
  articles
SET
  title = 'update Title'
WHERE
  id = 1;
```



- articles 테이블 2번 레코드의 title, content 필드 값을 각각 'update Title', 'update Content'로 변경

```sql
UPDATE
  articles
SET
  title = 'update Title',
  content = 'update Content'
WHERE
  id = 2;
```



## Delete data

### DELETE statement

> 테이블 레코드 삭제



#### DELETE stntax

```sql
DELETE FROM table_name
[WHERE
  condition];
```

- DELETE FROM 절 다음에 테이블 이름 작성
- WHERE 절에서 삭제할 레코드를 지정하는 조건 작성
- WHERE 절을 작성하지 않으면 모든 레코드를 삭제



##### DELETE 활용

- articles 테이블의 1번 레코드 삭제

```sql
DELETE FROM
  articles
WHERE
  id = 1;
```



- articles 테이블에서 작성일이 오래된 순으로 레코드 2개 삭제

```sql
DELETE FROM
  articles
WHERE id IN (
  SELECT id FROM articles
  ORDER BY createdAt
  LIMIT 2
  );
```



## Join

- 데이터가 하나의 테이블로 작성되어 있다면, 동명이인이 있거나 특정 데이터가 수정되었을 때 문제가 발생할 수 있음
- 테이블에 각각 외래 키 필드를 작성하여 나누어서 분류해야 함

![Inner Join](https://www.tutorialspoint.com/sql/images/innerjoin_2.jpg)

- Join의 필요성

  - 테이블을 분리하면 데이터 관리는 용이해질 수 있으나 출력 시에는 문제가 있음

  - 테이블 한 개만을 출력할 수밖에 없어 다른 테이블과 결합하여 출력하는 것이 필요해짐
  - 그러므로, join을 사용해서 둘 이상의 테이블에서 데이터를 검색



### INNER JOIN clause

> 두 테이블에서 값이 일치하는 레코드에 대해서만 결과를 반환

<img src="https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png" alt="img" style="zoom:50%;" />



#### INNER JOIN syntax

```sql
SELECT
  select_list
FROM
  table_a
INNER JOIN table_b
  ON table_b.fk = table_a.pk;
```

- FROM 절 이후 메인 테이블 지정 (table_a)
- INNER JOIN 절 이후 메인 테이블과 조인할 테이블을 지정 (table_b)
- ON 키워드 이후 조인 조건을 작성
- 조인 조건은 table_a과 table_b 간의 레코드를 일치시키는 규칙을 지정



##### INNER JOIN 활용

- 모든 게시글(제목, 내용)을 작성자 이름과 함께 출력

```sql
SELECT articles.title, articles.content, users.name   -- articles와 user에 같은 항목이 있을 경우 충돌하므로 테이블 이름도 함께 작성
FROM articles   -- 메인 테이블이 먼저 출력됨
INNER JOIN users
  ON users.id = articles.userId;
```



- 1번 회원이 작성한 모든 게시글의 제목과 작성자명을 조회

```sql
SELECT articles.title, users.name
FROM articles
INNER JOIN users
  ON users.id = articles.userId
  WHERE users.id = 1;
```



### LEFT JOIN clause

> 오른쪽 테이블의 일치하는 레코드와 함께 왼쪽 테이블의 모든 레코드 반환

<img src="https://i.stack.imgur.com/VkAT5.png" alt="Lightbox" style="zoom:50%;" />



#### LEFT JOIN syntax

```sql
SELECT
  select_list
FROM
  table_a
LEFT JOIN table_b
  ON table_b.fk = table_a.pk;
```

- FROM 절 이후 왼쪽 테이블 지정(table_a)
- LEFT JOIN 절 이후 오른쪽 테이블 지정 (table_b)
- ON 키워드 이후 조인 조건을 작성
  - 왼쪽 테이블의 각 레코드를 오른쪽 테이블의 모든 레코드와 일치시킴



##### LEFT JOIN 활용

- 오른쪽 테이블과 매칭되는 레코드가 없으면 NULL을 표시
  - 게시글을 작성한 이력이 없는 회원 정보 조회

```sql
SELECT * FROM users
LEFT JOIN articles
  ON articles.userId = users.id
WHERE articles.userId IS NULL;
```
