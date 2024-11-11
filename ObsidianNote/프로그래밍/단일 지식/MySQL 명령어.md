#mysql

mysql 명령어는 대소문자 구분이 없다.
### ubuntu에서 mysql 실행 
```ubuntu
/usr/local/mysql> bin/mysqld_safe --user=mysql &
```

### ubuntu에서 mysql 로그인 
```ubuntu
/usr/local/mysql> bin/mysql -u root -p
```
### ubuntu에서 mysql 서버 종료
```
/usr/local/mysql> bin/mysql -u root -p shutdown
```

### 비밀번호 변경
```mysql
ALTER USER 'root'@'localhost'IDENTIFIED BY 'root-password';

//이러 방식도 있다고 하는데 잘 작동하는지는 모르겠음
SET PASSWORD = PASSWORD("변경 비밀번호");
```

### 데이터 베이스 보기
```MySQL
SHOW DATABASES;
SHOW DATABASE [name];
```

### 데이터 베이스 생성
```MySQL
CREATE DATABASE [name];
```

### 데이터 베이스 사용할거라고 알리기
```MySQL
USE [name];
```

### 테이블 생성하기
```MySQL

#example )
CREATE TABLE cats 
( 
id INT unsigned NOT NULL AUTO_INCREMENT, # Unique ID for the record 
name VARCHAR(150) NOT NULL, # Name of the cat 
owner VARCHAR(150) NOT NULL, # Owner of the cat
birth DATE NOT NULL, # Birthday of the cat 
PRIMARY KEY (id) # Make the id the primary key 
);

CREATE TABLE [테이블 이름] (
[열] [데이터유형] [열 의 제약조건]
)

```

[데이터 타입](https://www.w3schools.com/mysql/mysql_datatypes.asp)
NOT NULL : 값이 무조건 존재 해야함
 AUTO_INCREMENT : 중복되지않음
 값을 지정하지 않을경우 자동으로 생성해서 조건을 맞추기 때문에 보통은 값을 입력할떄 따로 설정하지 않음

PRIMARY KEY(id)  : 기본 키를 정의 하는 부분
테이블의 각 줄을 식별하기위한 기준이 되는 변수 를 선택하는 것
특징
- 고유성: 중복되면안됨 (AURO_INCREMENT)
- NULL이 아님
### 테이블 보기
```MySQL
SHOW TABLES;
```

### 테이블 구조 보기
```MySQL
DESC [테이블 이름];
```

### 테이블 타입 변경
데이터 테이블의 타입을 변경하기위한 기능들 이다.
```MySQL
ALTER TABLE [테이블 이름] [키워드] [어떤 열인지] [타입];
```
#### 키워드 MODIFY : 데이터만 변경
```MySQL
ALTER TABLE [테이블 이름] MODIFY [어떤 열인지] [타입];

이런 방식으로 추가 설정할수있다.
ALTER TABLE [테이블 이름] MODIFY [어떤 열인지] [타입] NOT NULL;
```
#### 키워드 CHANGE : 데이터만 변경 + 열의 이름 변경
```MySQL
ALTER TABLE [테이블 이름] CHANGE [어떤 기존열] [세로운 열이름] [타입];
```

### 열 추가
```MySQL
ALTER TABLE [테이블 이름] ADD COLUMN [열이름] [데이터 타입] [제약 조건];
```
### 열 제거
```MySQL
ALTER TABLE [테이블이름] DROP COLUMN [열이름];
```
### 열 교체
```MySQL
ALTER TABLE [테이블 이름] CHANGE COLUMN [기존이름] [세이름] [데이터 타입];
```

### 값추가 하기
```MySQL
INSERT INTO [테이블 이름] (열1, 열2 ...) VALUES (값1, 값2 ...);
```

### 값 읽기
기본적인 형식
```MySQL
SELECT 열1, 열2, ... FROM [테이블 이름] WHERE 조건;
```

모든 열 조회
```MySQL
SELECT * FROM 테이블 이름;
```

특정 열 만 조회
```MySQL
ex)
SELECT id, title FROM 테이블 이름;
```

조건을 사용한 조회
```MySQL
SELECT [열] FROM [테이블 이름] WHERE 조건;
ex)
SELECT [열] FROM [테이블 이름] WHERE title = "tit";
SELECT [열] FROM [테이블 이름] WHERE id = "1";
```

정련된 결과로 조회
```MySQL
SELECT * FROM [테이블 이름] ORDER BY [선택할 행] [오름차순: ASC, 내림차순: DESC];
```

다양한 조건 (chat GPT)
```MySQL
-- 특정 값과 일치하는 행 조회
SELECT * FROM users WHERE username = 'john_doe';

-- 비교 연산자를 사용한 조건 조회
SELECT * FROM users WHERE age > 30;

-- 논리 연산자를 사용한 조건 조회
SELECT * FROM users WHERE age > 30 AND email LIKE '%example.com';

-- 범위 조건을 사용한 조회
SELECT * FROM users WHERE age BETWEEN 25 AND 35;

-- 집합 조건을 사용한 조회
SELECT * FROM users WHERE username IN ('alice', 'bob');

-- NULL 조건을 사용한 조회
SELECT * FROM users WHERE age IS NOT NULL;

-- 패턴 매칭을 사용한 조회
SELECT * FROM users WHERE email LIKE '%example.com';

-- 날짜 및 시간 조건을 사용한 조회
SELECT * FROM users WHERE created_at > '2024-01-01 00:00:00';

-- 조합 조건을 사용한 조회
SELECT * FROM users WHERE age > 25 AND (username = 'alice' OR username = 'bob');

SELECT * FROM users LIMIT 10; // 10개만 출력하기
```

LIKE 는 같은지 벼교하는 연산자이다.
LIKE는 2개의 와일드 카드가 있는데
% : 0개 이상의 문자와 일치
_ : 정확히 1개의 임이의 문자와 일치
'%abc' => 'dddabc' 가능
'a_cd' => 'abcd' 가능 'abbcd' 불가능
ㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓㅓ
IN 은 OR와 비슷하지만 더 쉽게 사용할수있게 되어있다.
```MySQL
SELECT * FROM users WHERE username = 'alice' OR username = 'bob' OR username = 'carol';
SELECT * FROM users WHERE username IN ('alice', 'bob', 'carol');
```
이 2개는 서로 같은 기능을한다.


### UPDATE
```MySQL
UPDATE [테이블 이름] SET 열1 = 값1, 열2 = 값2, ... WHERE [조건];

ex)
UPDATE test_table SET title="atitle" WHERE id=1;
```

### DELETE
```MySQL
DELETE FROM [테이블 이름] WHERE [조건문];
```

### UNIQUE
```MySQL
ALTER TABLE [테이블 이름] 
MODIFY [열 이름] [타입] NOT NULL UNIQUE;
```
값을 UNIQUE로 바꾸기 그러면 동일한 값은 존제하지 않게 만들수있다.

### JOIN
```MYSQL
SELECT * FROM [테이블 1 이름] INNER JOIN [테이블 2] ON [테이블 1 공유값]=[테이블 2 공유값]
```
INNER JOIN - 일치하는 행만 반환
```MySQL
SELECT 열1, 열2, ...
FROM 테이블1
INNER JOIN 테이블2 ON 테이블1.공통열 = 테이블2.공통열;
```
LEFT JOIN - 왼쪽 테이블의 모든 행과 일치하는 오른쪽 행 테이블 반환
```MySQL
SELECT 열1, 열2, ...
FROM 테이블1
LEFT JOIN 테이블2 ON 테이블1.공통열 = 테이블2.공통열;
```
RIGHT JOIN - 오른쪽 테이블의 모든 행과 일치하는 외쪽 테이블 행 반환
```MySQL
SELECT 열1, 열2, ...
FROM 테이블1
RIGHT JOIN 테이블2 ON 테이블1.공통열 = 테이블2.공통열;
```
FULL OUTER JOIN - 두 테이블의 모든 행 반환
```MySQL
SELECT users.username, orders.product, orders.amount
FROM users
LEFT JOIN orders ON users.user_id = orders.user_id
UNION
SELECT users.username, orders.product, orders.amount
FROM users
RIGHT JOIN orders ON users.user_id = orders.user_id;
```
CROSS JOIN - 가능한 모든 테이블 반환
```MySQL
SELECT 열1, 열2, ...
FROM 테이블1
CROSS JOIN 테이블2;
```
SELF JOIN - 동일한 테이블 내에서 스스로 조인

여러게 가있지만 우선은 간단하게만 해놓고 
만약에 필요할떄 사용하는게 좋을거같다 일반적이게는 INNER JOIN만 쓸듯

join을 한번에 3개에 테이블 에서 할수도 있다.

### GROUP BY
특정 기준으로 그룹을 잡아서  특정값을 계산 하는 기능이다
SELECT 출력에 사용된다.

| id  | product    | category        | amount | sale_date  |
| --- | ---------- | --------------- | ------ | ---------- |
| 1   | Laptop     | Electronics     | 1000   | 2023-01-01 |
| 2   | Smartphone | Electronics     | 800    | 2023-01-02 |
| 3   | TV         | Electronics     | 1200   | 2023-01-03 |
| 4   | Blender    | Home Appliances | 200    | 2023-01-04 |
| 5   | Mixer      | Home Appliances | 150    | 2023-01-05 |
```MySQL
SELECT category, SUM(amount) AS total_sales
FROM sales
GROUP BY category;
```

| category        | total_sales |     |
| --------------- | ----------- | --- |
| Electronics     | 3000        |     |
| Home Appliances | 350         |     |
출력을 하면 이렇게 나온다

category를 이용해서 그룹화 하여 같은 카테고리 끼리 sales 테이블을 amount를 합해서 보여주는 쿼리문 이다.
SUM, AVG, COUNT, MIN, MAX 등과 같이 사용한다고 한다.

이 코드를 실행시킬떄 문제 가 발생했다 
```MySQL
SELECT writing.id, COUNT(writing_like.writing_id) from writing
LEFT JOIN writing_like ON writing.id=writing_like.writing_id
GROUP BY writing_like.writing_id;
```
그 이유는 writing_liek.writing_id값이 left join 에서 NULL을 가질수있어서다
left join은 왼쪽 테이블에 존제하는것들이 기준이 되는데 왼쪽에 없을수 있기 때문이다.

따라서 group by에 사용하는 값을  현제 비교하는 중인 값인 writing.id 로 하면된다.
```MySQL
SELECT writing.id, COUNT(writing_like.writing_id) from writing
LEFT JOIN writing_like ON writing.id=writing_like.writing_id
GROUP BY writing.id;
```
또한 JOIN 과 GROUP BY를 사용할때 WHERE는  JOIN 다음 GROUP BY 전에 사용한다.

### 트렌젝션
트렌젝션은 무조건 동시에 벌어져야 하는 일을 해결하기위해 생겼다 
특징은
- Atomicity( 원자성 ): 트렉젝션의 모든 작업은 완전히 수행되거나, 전혀 수행되지 않아야 한다. 일부만 적용되는것은 허용되지 않는다.
- Consistency( 일관성 ): 트렉젝션이 완료되면 데이터 베이스를 일괏어 있는 상태로 유지되어야한다.
- Isolation( 고립성 ): 트렌젝션이 실행되는 동안 다른 트랜젝션이 간섭할수 없습니다. 동시에 트렌젝션이 실행될때, 각 트렌젝션은 독립적이고 영향을 받지 않아야 한다.
- Durability( 지속성 ): 트렌젝션이 완료되면 영구적으로 반영되어야한다. 시스탬에 장애가 발생하더라도 결과가 소실되면 안된다.

트렌젝션이 발생되는 예는
어떤 상품 페이지에서 주문을 할떄 주문 즉시 돈 관련 데이터가 사용되고 상품관련 데이터 가 사용되는 이과정 사이에서 시스탬에 문제가 발생하면
돈만 나가고 상품이 주문이 안되는 경우가 발생할수 있다.

따라서 동시에 돈과 상품이 오가야 하는데 컴퓨터에 동시란 없으니까.
트렌젝션을 사용해서 첫번째 작업을하고 두번째 작업까지 완료되면 그제서야 양쪽에 값은 적용하는 트렌젝션을 사용하게 된다.

**문법**
- START TRANSACTION: 트랜잭션의 시작을 표시한다.
- COMMIT: 트랜잭션 내의 모든 작업을 영구적으로 데이터베이스에 반영한다.
- ROLLBACK: 트랜잭션 내의 모든 작업을 취소하고 시작 전 상태로 되돌린다.

```MySQL
-- 트랜잭션 시작
START TRANSACTION;

-- 계좌 A에서 100을 차감
UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A';

-- 계좌 B에 100을 추가
UPDATE accounts SET balance = balance + 100 WHERE account_id = 'B';

-- 모든 작업이 성공하면 커밋
COMMIT;
```


### 백업
백업 도구의 종류
1. mysqldump
2. mysqlhotcopy
3. Binary Log
4. Percona XtarBackup
5. MySQL Enterprise Backup

- 논리적 백업: 'mysqldump' 도구를 사용하여 SQL덤프 파일 생성
- 물리적 백업: 'mysqlhotcopy', Percona XtraBackup, MySQL Enterprise Backup
- Binary Log:  트랜잭션 로그를 이용해 시점 복구 가능

논리적 백업
- 데이터 베이스의 구조와 데이터를 SQL 스크립트로 저장하는 방식
- 장점
	- 휴대 가능하고 다른 시스탬으로 쉽게 이동할수 있다.
	- 특정 테이블 이나 데이터 베이스만 선택적으로 백업할 수 있다.
	- 데이터 베이스 엔진에 종속되지 않음
- 단점
	- 대규모 데이터 베이스에서는 백업과 복구 속도가 느리다.
	- 복구시 데이터베이스 서버가 가동 중지 될수 있다.

물리적 백업
- 데이터 베이스 파일을 직접 복사하여 백업 하는 방식
- 장점
	- 대규모 데이터 베이스 서버에 적합, 백업과 복구 속도가 빠르다.
	- 또한 서버가 가동중인 상태에서 백업 할수있다.
- 단점
	- 백업 파일이 데이터 베이스 엔진에 종속적이다.
	- 휴대성이 떨어지며 시스탬 이동시 주의가 필요하다.

mysqldump를 이용한 방법 
백업하기
```MySQL
#기본 사용법
mysqldump -u [username] -p [database_name] > [backup_file].sql

#특정 테이블 사용법
mysqldump -u [username] -p [database_name] [table_name] > [backup_file].sql

#모든 데이터베이스 백업
mysqldump -u [username] -p --all-databases > all_databases_backup.sql
```
다시 적용하기 (복원)
```MySQL
#데이터 베이스 복원
mysql -u [username] -p [database_name] < [backup_file].sql
```
데이터 베이스 다시 적용할떄는 먼저 데이터베이스를 생성한 상태에서 적용하는거다.

mysqldump 사용할떄 sudo를 사용하면 해당 명령어를 찾지 못한다. 컴파일 해서 설치해서 그럴수도있다.
#### '<' '>' 이 꺽쇠 들을 조심 해야한다 파일을 어디다가 쓸지 정하는 문구 인데 잘못하면 파일을 복원하려다가 복원하는 파일 내용을 없에서 복원하지 못하게 될수도 있다.


### TIMESTAMP 와  DATATIME 에 비교
```MySQL
[이름] TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
```
이걸로 자동으로 현제 시간이 저장되게 할수있고
```MySQL
[이름] TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
```
이걸로 레코드가 업데이트 될떄 마다 자동으로 갱신되게 할수도 있다.
- timestamp는 서버에 시간대에 따라서 변화되고, 클라이언트에 시간대에 따라 출력 된다.
- datatime은 저장된 데로 출력된다.
- timestamp은 20238 년까지 유효하다.

### 이름이 abc인 값의 개수 구하기
```MySQL
SELECT COUNT(*) AS count FROM your_table WHERE name = 'abc';
```
as 는 별칭을 붙치는 역할이다. 해당 결과 이름을 count라는 컬럼으로 만든거다.

### IN과 서브쿼리
서브 쿼리는 쿼리문 안에있는 쿼리이다
```MySQl
SELECT first_name, last_name,   
       (SELECT COUNT(*) FROM orders 
       WHERE orders.customer_id = customers.customer_id)
       AS order_count   
FROM customers;
```


이런식으로 서브쿼리를 사용할수있다.

### WITH
with 는 재사용 가능한 공통 테이블 표현시을 정의하는 방법이다.(CTE)
```mysql
WITH [cte_name] AS (
	SELECT [column_name(s)] 
	FROM [table_name] 
	WHERE [condition] 
) 
SELECT [column_name(s)]
FROM [table_name]
JOIN [cte_name] ON [join_condition]
WHERE [condition];
```
이렇게 with를 통해서 임시 테이블을 만들고
해당 테이블을 사용할수있다.


### UNION
union은 이러한 2 개의 select문의 결과를 합치는데 사용한다.
UNION은 똑같은 row를 제거하고
UNIONALL은 유지시킨다.
```MYSQL
SELECT id, name FROM table1
UNION ALL
SELECT id, name FROM table2;
```

### WITH RECURSIVE
재귀 쿼리
```MySQL

```
재귀 쿼리는 
- (CTE) with 문을 실행시킬 부분과
- 첫 실행 문장
- UNION ALL
- 재귀 시킬 문장
- 재귀 종료 조건
으로 이루어져 있다.


예제
```mysql
WITH RECURSIVE cte_count 
AS ( 
    -- Non-Recursive 문장( 첫번째 루프에서만 실행됨 )
    SELECT 1 AS n
    UNION ALL
    -- Recursive 문장(읽어 올 때마다 행의 위치가 기억되어 다음번 읽어 올 때 다음 행으로 이동함)
    SELECT n + 1 AS num 
    FROM cte_count
    WHERE n < 3 
)
SELECT * FROM cte_count;
```



```mysql
WITH RECURSIVE CommentTree AS (
    -- 초기 선택 집합: 루트 노드를 선택
    SELECT id, parent_id, content
    FROM comments
    WHERE id = 1  -- 최상위 부모 댓글 ID
    
    UNION ALL
    
    -- 재귀 부분: 자식 노드를 선택하여 트리를 확장
    SELECT c.id, c.parent_id, c.content
    FROM comments AS c
    INNER JOIN CommentTree AS ct ON c.parent_id = ct.id
)
SELECT COUNT(*) AS total_replies
FROM CommentTree
WHERE id != 1;  -- 최상위 부모 댓글을 제외한 나머지 자식 댓글 수를 계산
```

commentTree에 id  가 1인 값을 부모로 하여 
트리 형태로 comment가 이루어져있다고 가정했을때
부모로 계속 올라가면 id가 1인 값이 있는지 확인하고 그 개수를 가지고 오는 쿼리 이다.


이 쿼리 문은 댓글을 깊이 우선 탐색 처럼 보이게 해주는 너부우선 탐색 쿼리 이다.
{중괄호} 는 변경 가능한 값
```MYSQL
WITH RECURSIVE CommentTree AS (
	SELECT id, content, 1 as level, CAST(id AS NCHAR) AS path
	FROM comments
	WHERE writing_id = {63} AND parent_comment_id = 0

	UNION ALL

	SELECT c.id, c.content, level + 1, CONCAT(ct.path, c.id)
	FROM comments AS c
	INNER JOIN CommentTree AS ct ON c.parent_comment_id = ct.id
)
SELECT *
FROM CommentTree
ORDER BY path ASC
LIMIT {20} OFFSET {0}
```