# Pandas - loc, iloc

## loc, iloc 속성을 사용하는 인덱싱
loc : 라벨값 기반의 2차원 인덱싱  
iloc : 순서를 나타내는 정수 기반의 2차원 인덱싱

<br>

### 기본 자료구조 인덱스와 차이
행과 열을 동시에 인덱싱 하는 구조는 기본 자료구조 인덱스와 차이가 있음
```python
df['열']
df[:'행'] # 슬라이싱이 반드시 필요
df['열'][:'행'] # 슬라이싱이 반드시 필요
```

<br>

### loc, iloc 속성을 사용하는 인덱싱
pandas 패키지는 `[행번호, 열번호]` 인덱싱 불가하지만, iloc 또는 loc 속성 사용하면 가능
```python
iloc[행번호, 열번호]
loc[행제목, 열제목]
```

<br>
<br>

## loc 인덱서 사용
```python
df.loc[행제목] # 행우선 인덱서
df.loc[행제목, 열제목]
```

df.loc['a'] 이렇게 쓰면 행을 기준으로 찾는다.  
값이 하나면 무조건 행을 찾는다. 그리고 시리즈로 반환한다.  
인덱서의 키값을 하나만 받는 경우 행기준 인덱싱이므로 행 인덱스에서 a를 찾는다.  
loc 인덱서에서는 열 단독 인덱싱은 불가능 함  
```python
# 예제 DF  생성
# 10-21 범위의 숫자를  value로 갖는 3행 4열의 df 
df = pd.DataFrame(np.arange(10,22).reshape(3,4),
                  index=['a','b','c'],
                  columns = ["A","B","C","D"])
df
	A	B	C	D
a	10	11	12	13
b	14	15	16	17
c	18	19	20	21
```

<br>

### 한 행 추출
```python
df.loc['a']

A    10
B    11
C    12
D    13
Name: a, dtype: int64
```

<br>

### 여러행 추출
```python
df['b':'c']
	A	B	C	D
b	14	15	16	17
c	18	19	20	21

df.loc['b':'c'] # loc를 사용하지 않은 것과 같은 결과값을 같는다.
	A	B	C	D
b	14	15	16	17
c	18	19	20	21

df.loc['b'] # 시리즈로 추출
df.loc[['b']] # df로 추출
```

<br>

### 비연속적인 행 추출
```python
df.loc[['a', 'c']]
    A	B	C	D
a	10	11	12	13
c	18	19	20	21

df.loc['a', 'c'] # 이렇게 쓰면 a행의 c열을 가져오라는 뜻이기에 에러 발생
```

<br>

### 열 추출
```python
df.B
a    11
b    15
c    19
Name: B, dtype: int64

df[['B']]
	B
a	11
b	15
c	19

df[['B', 'C']]
	B	C
a	11	12
b	15	16
c	19	20
```

<br>

### boolean selection으로 row 선택하기
```python
df.A # 시리즈 반환
a    10
b    14
c    18
Name: A, dtype: int64

df.A > 15 # 불리언 시리즈 반환
a    False
b    False
c     True
Name: A, dtype: bool

df[df.A > 15]
	A	B	C	D
c	18	19	20	21

df.loc[df.A > 15]
	A	B	C	D
c	18	19	20	21
```

<br>

### boolean series를 반환하는 함수를 인덱스값으로 사용
위와 똑같은 과정을 함수를 사용해서 해보겠다.
```python
# 함수 생성: df를 인수로 받아서 해당 df로 연산 후 결과값으로 불리언시리즈를 반환
def sel_row(df):
    return df.A > 15

sel_row(df)
a    False
b    False
c     True
Name: A, dtype: bool

df.loc[sel_row(df)]
	A	B	C	D
c	18	19	20	21

df[sel_row(df)] # loc를 사용하지 않아도 가능
```
이런 식으로 함수를 사용할 수도 있다.

<br>

### loc 인덱서 슬라이싱
loc를 이용하면 행 인덱스 값이 수치 인덱스여도 위치값으로 접근하지 않는다.
```python
# 예제 df
df2 = pd.DataFrame(np.arange(10,26).reshape(4,4),
                  columns=['a','b','c','d'])
df2 # 행인덱스는 지정하지 않아서 0부터 1씩 증가되는 정수 인덱스 자동 생성

    a	b	c	d
0	10	11	12	13
1	14	15	16	17
2	18	19	20	21
3	22	23	24	25

df2.loc[1:3]
	a	b	c	d
1	14	15	16	17
2	18	19	20	21
3	22	23	24	25

df2[1:3]
	a	b	c	d
1	14	15	16	17
2	18	19	20	21
```

<br>

### loc 인덱서 사용 요소 값 접근
인덱싱으로 행과 열을 모두 받아 접근한다. `df.loc[행제목, 열제목]`
loc니까 라벨(문자열)인덱스 사용
```python
df2.loc[0, 'a']
# 10

df2.loc[0, 'a'] = 100
df2
	a	b	c	d
0	100	11	12	13
1	14	15	16	17
2	18	19	20	21
3	22	23	24	25
```

```python
df.loc[['a', 'b']]['A'] # loc 연산 한번, df연산 한번 진행하고 시리즈 반환
a    10
b    14
Name: A, dtype: int64

df.loc[['a', 'b'],'A'] # loc 연산 한번 진행하고 시리즈 반환
a    10
b    14
Name: A, dtype: int64

df.loc[['a', 'b'],['A']] # df 반환
	A
a	10
b	14
```

<br>

### loc를 이용한 indexing 정리
df의 a행의 모든 열 추출
```python
df.loc['a']  # a행의 모든 열 추출, 시리즈
A    10
B    11
C    12
D    13
Name: a, dtype: int64

df.loc[['a']] # a행의 모든 열 추출, df
	A	B	C	D
a	10	11	12	13

df.loc['a',:] # a행의 모든 열 추출, 시리즈
A    10
B    11
C    12
D    13
Name: a, dtype: int64

df.loc[['a'],:] # a행의 모든 열 추출, df
	A	B	C	D
a	10	11	12	13
```

a행의 B, C열을 추출하기
```python
df.loc['a', 'B':'C'] # 시리즈 반환
B    11
C    12
Name: a, dtype: int64

df.loc[['a'], 'B':'C'] # df 반환
    B	C
a	11	12

df.loc[['a'], ['B','C']] # df 반환
    B	C
a	11	12

df.loc['a', ['B','C']] # 시리즈 반환
B    11
C    12
Name: a, dtype: int64
```

b행부터 모든 행의 A열을 추출
```python
df.loc['b':, 'A'] # 시리즈 반환
b    14
c    18
Name: A, dtype: int64

df.loc['b':]['A'] # 시리즈 반환
b    14
c    18
Name: A, dtype: int64

df.loc['b':][['A']] # df 반환
    A
b	14
c	18

df.loc['b':,['A']] # df 반환
    A
b	14
c	18

df.loc['b':, 'A':'A'] # df 반환
    A
b	14
c	18
```

<br>
<br>

## iloc 인덱서(위치 인덱스)
라벨(name)이 아닌 위치를 나타내는 정수 인덱스만 받는다.  
위치 정수값은 0부터 시작  
`데이터프레임.iloc[행번호, 열번호]`

iloc는 숫자롤 이용해 작성하여 한눈에 어떤 데이터를 가리키는지 알아보기 어렵기 때문에, 행의 이름을 잘 모르거나 간단하게 사용하고 싶을때만 쓴다.  
보통 loc를 쓴다.  

```python
df
	A	B	C	D
a	10	11	12	13
b	14	15	16	17
c	18	19	20	21

df.iloc[0, 1]
# 11

df.iloc[0:2, 1:2] # df 반환
	B
a	11
b	15

df.iloc[0:2] # df 반환
	A	B	C	D
a	10	11	12	13
b	14	15	16	17

df.iloc[0:2, 1] # 시리즈 반환
a    11
b    15
Name: B, dtype: int64

df.iloc[0:1, -2:]
    C	D
a	12	13

df.iloc[0, -2:]
C    12
D    13
Name: a, dtype: int64
```

#### 시리즈, 데이터프레임 반환
행과 열 둘 다 슬라이싱하면 데이터프레임을 반환한다.  
시리즈를 가지고 작업하면 시리즈 반환하고, 슬라이싱을 쓰지 않으면 시리즈를 반환한다.  

### value_counts()
`value_counts()`는 원소들을 분류하여 갯수를 세는 함수이다.  
```python
df = pd.DataFrame({'num_legs': [2, 4, 4, 6],
                   'num_wings': [2, 0, 0, 0]},
                  index=['falcon', 'dog', 'cat', 'ant'])
df
	    num_legs	num_wings
falcon	    2	        2
dog	        4	        0
cat	        4	        0
ant	        6	        0

# df에 value_counts() 적용: 동일한 값을 갖는 행의 수를 반환
df.value_counts() 
num_legs  num_wings
4         0            2
2         2            1
6         0            1
dtype: int64

df.num_legs.value_counts()
4    2
2    1
6    1
Name: num_legs, dtype: int64
```

<br>
