# SQL

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

| 유형                             | 역할                                   | SQL 키워드                     |
| -------------------------------- | -------------------------------------- | ------------------------------ |
| DDL (Data Definition Language)   | 데이터의 기본 구조 및 형식 변경        | CREATE<br />DROP<br />ALTER    |
| DQL (Data Query Language)        | 데이터 검색                            | SELECT                         |
| DML (Data Manipulation Language) | 데이터 조작 (추가, 수정, 삭제)         | INSERT<br />UPDATE<br />DELETE |
| DCL (Data Control Language)      | 데이터 및 작업에 대한 사용자 권한 제어 | COMMIT<br />R                  |



### Query

- 데이터베이스로부터 정보를 요청하는 것
- 일반적으로 SQL로 작성하는 코드를 쿼리문(SQL문)이라 함



## Querying data

### SELECT

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



#### SELECT 활용

1. 테이블 employees에서 LastName 필드의 모든 데이터를 조회

2. 테이블 employees에서 LastName, FirstName 필드의 모든 데이터를 조회

3. 테이블 employees에서 모든 필드 데이터를 조회

4. 테이블 employees에서 FirstName 필드의 모든 데이터를 조회

   (단, 조회 시 FirstName이 아닌 '이름'으로 출력될 수 있도록 변경)

5. 테이블 track에서 Name, Milliseconds 필드의 모든 데이터 조회

   (단, Milliseconds 필드는 60000으로 나눠 분 단위 값으로 출력)

```sql
SELECT
  LastName
FROM
  employees;

SELECT
  LastName, FirstName
FROM
  employees;

SELECT
  *
FROM
  employees;

SELECT
  FirstName AS '이름'
FROM
  employees;

SELECT
  Name, Milliseconds / 60000 AS '재생 시간(분)'
FROM
  tracks;
```



## Sorting data

### ORDER BY

> 조회 결과의 레코드를 정렬



#### ORDER BY syntax

- FROM clause 뒤에 위치
- 하나 이상의 컬럼을 기준으로 결과를 오름차순(ASC, 기본 값), 내림차순(DESC)으로 정렬



#### ORDER BY 활용



#### 정렬에서의 NULL

- NULL 값이 존재할 경우 오름차순 정렬 시 결과에 NULL이 먼저 출력



## Filtering data

### DISTINCT

> 조회 결과에서 중복된 레코드를 제거



#### DISTINCT syntax

- SELECT 키워드 바로 뒤에 작성







