# SQL Queries

## Select
두 열 이상의 값들이 궁금하다면 아래와 같이 작성하여 결과물을 볼 수 있습니다.  
```sql
SELECT name, genre
FROM movies;
```

<br>

## As
`AS`는 컬럼이나 테이블 별칭을 사용해서 부를 수 있는 키워드입니다.  
어떤 것이든 마음대로 사용할 수 있고 다만 작은 따옴표로 묶어주기만 하면 됩니다.  
- 항상 그래야만 하는 건 아니지만, 별칭은 작은따옴표로 묶는 것이 좋습니다.
- `AS`를 사용할 때 테이블의 컬럼 이름이 재설정되는 것은 아닙니다. 별칭은 오직 결과에서만 나타납니다.
```sql
SELECT name AS 'titles'
FROM movies;
```

<br>

## Distinct
`DISTINCT`는 유니크한 값을 결과로 반환하는데 사용됩니다.  
특정한 열에서 모든 중복된 값을 필터링해 내보냅니다.  
```sql
SELECT DISTINCT genre
FROM movies;
```

<br>

## Where
원하는 정보만을 얻기 위해 `WHERE`문을 사용해서 쿼리 결과를 제한할 수 있습니다.  
```sql
SELECT * 
FROM movies 
WHERE imdb_rating < 5;  -- 조건을 설정할 수 있습니다. 해당 열에서 값이 5보다 작은 행만 반환합니다.
```
`WHERE`문에서 비교 연산자는 아래의 것들을 사용할 수 있습니다.  
- `=`
- `!=`
- `>`
- `<`
- `>=`
- `<=`

<br>

## Like
`LIKE`는 비슷한 값들을 비교할때 유용한 연산자입니다.  
열에서 값이 특정한 패턴에 맞는지 평가한 뒤 맞는 행들만 보여줍니다. 예를 들어, 'Se7en' 와 'Seven'을 둘 다 찾고 싶다면, 'Se_en' 패턴을 사용할 수 있습니다.  
```sql
SELECT *
FROM movies
WHERE name LIKE 'Se_en';
```

`LIKE`와 함께 사용할 수 있는 다른 문자는 퍼센트 기호(`%`)입니다.  
`%`는 패턴에서 0 또는 더 많은 글자가 일치하는지 확인할 수 있습니다. 예들 들어, `A%`는 글자 'A'와 일치하기도 하며, 'A'로 시작하는 모든 글자와 일치합니다. `%a`는 'a'와 'a'로 끝나는 모든 글자와 일치합니다.  
```sql
SELECT * 
FROM movies
WHERE name LIKE 'The %';  -- 'The '로 시작하는 이름을 모두 가져옵니다.
```

<br>

## Is Null
알 수 없는 값은 `NULL`로 표시됩니다.  
`NULL`을 테스트할 때는 `=` 와 `!=` 같은 연산자를 사용할 수 없습니다.  
사용할 수 있는 연산자는 아래 두가지만 있습니다.  
- `IS NULL`
- `IS NOT NULL`
```sql
SELECT name
FROM movies
WHERE imdb_rating IS NULL;  -- 평점이 없는 것들만 찾습니다.
```

<br>

## Between
`BETWEEN`은 `WHERE`문에서 특정 범위 내에 있는 결과 집합을 필터링하는데 사용합니다.  
2개의 값을 허용하는데, 숫자, 텍스트, 날짜만 가능합니다.  
텍스트는 알파벳 순서를 기준으로 하며 AND 뒤에 오는 글자는 포함시키지 않습니다.  
```sql
SELECT *
FROM movies
WHERE year BETWEEN 'D' AND 'G'  -- 'D', 'E', 'F'로 시작하는 것들만 출력합니다.
```

숫자는 AND 뒤에 오는 숫자 또한 포함시킵니다.  
```sql
SELECT *
FROM movies
WHERE year BETWEEN 1970 AND 1979  -- 1970~1979 값들만 출력합니다.
```

<br>

## And
`WHERE`문에서 여러 조건을 결합해 사용할 때 씁니다.  
`AND`문을 하나 사용해 두개의 조건이 있다면, 그 두 조건을 모두 만족하는 결과만 출력됩니다.  
```sql
SELECT *
FROM movies
WHERE year < 1985
   AND genre = 'horror';
```

<br>

## Or
`AND`는 모든 조건이 참일 때만 행을 표시하지만, `OR`는 어떤 조건이든 하나만이라도 참이면 행을 표시합니다.  
```sql
SELECT *
FROM movies
WHERE genre = 'comedy'
   OR genre = 'romance';
```

<br>

## Order By
종종 특정한 순서로 결과 집합 데이터를 나열하는 것은 유용할 수 있습니다.  
`ORDER BY`를 사용하면 알파벳 순이든 숫자 순이든 결과를 정렬할 수 있습니다.  
```sql
SELECT *
FROM movies
ORDER BY name;
```

정렬하는 순서를 내림차순으로 할 수도 있습니다. 아래의 키워드를 사용하면 됩니다.  
- `DESC`: 내림차순(높음->낮음)  
- `ASC`: 오름차순(낮음->높음)  

```sql
SELECT name, year, imdb_rating
FROM movies
ORDER BY imdb_rating DESC;
```

<br>

## Limit
사실 대부분의 SQL 테이블에는 수십만개의 행이 포함되어 있기 때문에, 행 수를 제한하는 것이 중요합니다.  
`LIMIT`를 사용하면 결과 집합이 가질 최대 행 수를 지정할 수 있습니다. 이렇게 하면 화면 공간 절약은 물론 퀴리 실행속도도 빨라집니다.  
```sql
SELECT *
FROM movies
ORDER BY imdb_rating DESC
LIMIT 3;
```

<br>

## Case
SQL에서 if-then 로직을 다루는 방식입니다. 일반적으로 `SELECT`문에서 사용합니다.  
- 각 `WHEN`은 조건을 테스트하고 그 뒤의 `THEN`은 참일때의 값을 문자열로 반환합니다.  
- `ELSE` 뒤에는 모든 조건이 거짓일 때 반환할 값을 입력할 수 있습니다.  
- `CASE`문은 `END`로 끝나야 합니다.  
```sql
SELECT name,
 CASE
  WHEN genre = 'romance' THEN 'Chill'
  WHEN genre = 'comedy' THEN 'Chill'
  ELSE 'Intense'
 END
FROM movies;
```

<br>
<br>

## Reference
[Codecademy Learn SQL](https://www.codecademy.com/learn/learn-sql)

<br>
