# SQL Basics
SQL에서 공식 예약어는 all caps로 작성합니다.  
소문자로 써도 작동하지만, 대문자로 쓰는 것이 컨벤션입니다.  
변수명은 소문자로 작성합니다.  

<br>

## 관계형 데이터베이스
관계형 데이터베이스는 정보를 하나 이상의 테이블로 구성하는 데이터베이스입니다.  
여기서 테이블은 행과 열로 구성된 데이터 모음입니다.  
테이블의 열은 특정 유형의 데이터 값 집합이고, 테이블의 행은 테이블의 단일 레코드입니다.  

### Data Type
관계형 데이터베이스에 저장된 모든 데이터는 특정한 데이터 유형입니다.  
가장 일반적인 데이터 유형은 다음과 같습니다.  
- `INTEGER`, a positive or negative whole number
- `TEXT`, a text string
- `DATE`, the date formatted as YYYY-MM-DD
- `REAL`, a decimal value

### SQL문?
아래 코드는 SQL문입니다. 명령문은 데이터베이스가 유효한 명령으로 인식하는 텍스트입니다.  
명령문은 항상 세미콜론`;`으로 끝납니다.  
```sql
SELECT * FROM users;
```

<br>

## Create
`CREATE`문을 사용하면 데이터베이스에 새 테이블을 만들 수 있습니다.
```sql
CREATE TABLE table_name (
    column_1 data_type, 
    column_2 data_type, 
    column_3 data_type
);

CREATE TABLE users (
    id INTEGER,
    name TEXT,
    age INTEGER
);
```
파라미터는 인자로 전달되는 열, 데이터 유형 또는 값입니다.

<br>

## Insert
`INSERT`문은 테이블에 새 행을 삽입합니다.  
새 레코드를 추가하려는 경우에 `INSERT`문을 사용할 수 있습니다.
```sql
INSERT INTO celebs (id, name, age)  -- 지정된 행을 추가하는 구문입니다. ()는 삽입될 col을 식별하는 매개변수입니다.
VALUES (1, 'Justin Bieber', 22);  -- 삽입될 데이터를 나타내는 구문입니다.
```

<br>

## Select
```sql
SELECT name FROM users;
```
위와 같이 작성하게 되면 user 테이블에서 name을 가지는 모든 컬럼의 데이터를 가져옵니다.
```sql
SELECT * FROM users;
```
이렇게 작성하면 user 테이블에서 모든 컬럼의 데이터를 가져옵니다.

<br>

## Alter
`ALTER TABLE`문은 테이블에 새 열을 추가합니다.
```sql
ALTER TABLE users  -- 변경할 테이블 이름을 나타내는 구문입니다.
ADD COLUMN twitter_handle TEXT;  -- 새로 추가할 행/열과 열이름, 타입을 작성합니다.
```
`NULL`은 SQL에서 알수 없는 값에 대한 특별한 값입니다.  
열이 추가되기 전에 있었던 행들은 새로운 twitter_handle값에 대해 `NULL`(∅) 값을 가집니다.

<br>

## Update
`UPDATE`문은 테이블의 행을 편집합니다.
`UPDATE`로 이미 존재하던 데이터를 수정할 수 있습니다.
```sql
UPDATE users  -- 업데이트 할 테이블 이름을 나타내는 구문입니다.
SET twitter_handle = '@taylorswift13'  -- 변경할 열 값을 설정합니다.
WHERE id = 4;  -- 새 값으로 업데이트 할 행을 나타냅니다.
```

<br>

## Delete
`DELETE FROM`문은 테이블에서 하나 이상의 행을 삭제합니다.
`DELETE FROM`로 이미 존재하던 데이터를 삭제할 수 있습니다.
```sql
DELETE FROM users  -- 행을 삭제하기 원하는 테이블 이름을 나타내는 구문입니다.
WHERE twitter_handle IS NULL;  -- 삭제할 행을 선택합니다. twitter_handle 값이 NULL인 것을 모두 찾습니다.
```

<br>

## Constraints
Constraints는 열을 사용할 수 있는 방법에 대한 정보를 추가합니다.
규칙에 맞지 않는 데이터를 제한할 수 있습니다.
```sql
CREATE TABLE users (
   id INTEGER PRIMARY KEY, 
   name TEXT UNIQUE,
   date_of_birth TEXT NOT NULL,
   date_of_death TEXT DEFAULT 'Not Applicable'
);
```

1. PRIMARY KEY: 행을 식별하는데 사용합니다. 동일한 값을 가지고 있는 행이 있으면 새로운 행을 추가하지 못하게 합니다.
2. UNIQUE: 모든 행에 다른 값을 가져야 합니다. 여러 UNIQUE열을 가질 수 있다는 것이 PRIMARY KEY와 다릅니다.
3. NOT NULL: 해당하는 열은 값을 항상 가져야 합니다.
4. DEFAULT: 값을 열에 지정하지 않을 때, 추가될 default 인수를 설정할 수 있습니다.

<br>
<br>

## Reference
[Codecademy Learn SQL](https://www.codecademy.com/learn/learn-sql)

<br>
