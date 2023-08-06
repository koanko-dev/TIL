# Pandas - 데이터 조작, 개수, 정렬 등

<br>

## DataFrame 행 추가
pd의 데이터프레임 인덱서(loc) 사용.  
pd.concat() 사용 - 추가하고자 하는 행 data를 df로 새로 생성 후 결합.
```python
data = {
    "2015": [9904312, 3448737, 2890451, 2466052],
    "2010": [9631482, 3393191, 2632035, 2000002],
    "2005": [9762546, 3512547, 2517680, 2456016],
    "2000": [9853972, 3655437, 2466338, 2473990],
    "지역": ["수도권", "경상권", "수도권", "경상권"],
    "2010-2015 증가율":[0.0283, 0.0163, 0.0982,0.0141]
}
columns =['지역','2000','2005','2010','2015', '2010-2015 증가율']
index = ['서울','부산','인천','대구']
df3 = pd.DataFrame(data, index=index, columns=columns)

df3
	    지역	2000	2005	2010	2015	2010-2015 증가율
서울	수도권	9853972	9762546	9631482	9904312	    0.0283
부산	경상권	3655437	3512547	3393191	3448737	    0.0163
인천	수도권	2466338	2517680	2632035	2890451	    0.0982
대구	경상권	2473990	2456016	2000002	2466052	    0.0141

# 행 추가
df3.loc['광주'] = ['호남권',2470000,2456000,2453000,2460000,1.00]
```

<br>
<br>

## 행/열 삭제
`drop()` 함수 사용.  
`drop(index=[삭제할 행 인덱스])`  
`drop(columns=[삭제할 컬럼명])`  
기본은 원본반영 되지 않음.  
완전히 삭제하려면 `drop(inplace=True)`  
```python
# 행 삭제
df3.drop(index=['광주','대구'],inplace=True)

# 열 삭제
df3.drop(columns=['2010-2015 증가율']) # 원본반영 없고 결과반환

# axis 사용. drop(axis=0/1) : 0(행), 1(열)
df3.drop(['서울'],axis=0) # 원본 반영 안됨, 삭제 결과 반환
df3.drop(['지역'],axis=1) # 원본 반영 안됨, 삭제 결과 반환
```

<br>
<br>

## pandas 데이터처리 및 변환관련 함수
### count()
데이터 개수 세기.  
가장 간단한 분석은 개수를 세기 이다. count()함수 이용.  
NaN값은 세지 않는다.  
각 열마다 데이터 개수를 세기때문에 누락된 부분을 찾을 때 유용.  

#### 시리즈에서 count()
```python
s = pd.Series(range(10))
s[3] = np.nan
s
0    0.0
1    1.0
2    2.0
3    NaN
4    4.0
5    5.0
6    6.0
7    7.0
8    8.0
9    9.0
dtype: float64

s.count() # nan은 제외하고  count (결측치 제외)
# 9
```

<br>

#### 데이터프레임에서 count()
**각 열마다 데이터 개수**를 세기때문에 누락된 부분을 찾을 때 유용

##### 난수 발생시켜 dataframe 생성
난수 seed(값)라는 함수를 사용할 수 있음.  
seed의 의미 : 난수 알고리즘에서 사용하는 기본 값으로 시드값이 같으면 동일한 난수가 발생함.  
계속 변경되는 난수를 받고 싶으면 함수등을 이용해서 시드값이 매번 변하게 작업해야 함. `Time.tiem()`  
```python
np.random.randint(5) #정수 0-4사이에서 난수 발생
np.random.randint(5, size=4)
# array([0, 3, 4, 3])

np.random.seed(3)
np.random.randint(5, size=4)
# array([2, 0, 1, 3])

# 계속 변경되는 난수
import time
np.random.seed(int(time.time()))
np.random.randint(5, size=4)
# array([2, 2, 2, 1])

np.random.seed(3) #동일한 난수 발생
df1 = pd.DataFrame(np.random.randint(5,size=(4,4)))
df1.iloc[2,3] = np.nan
df1
	0	1	2	3
0	2	0	1	3.0
1	0	0	0	3.0
2	2	3	1	NaN
3	2	0	4	4.0

# 각 열의 data 개수 반환
# 3열은 3개의 data반환되었으므로 결측치가 있음
df1.count()
0    4
1    4
2    4
3    3
dtype: int64
```

<br>

### value_counts()
카테고리 값 세기.  
시리즈의 값이 정수, 문자열 등 카테고리 값인 경우에 시리즈.value_counts()메서드를 사용해 각각의 값이 나온 횟수를 셀 수 있음.  
파라미터 normalize=True 를 사용하면 각 값 및 범주형 데이터의 비율을 계산.  
`시리즈.value_counts(normalize=True)`  

#### 시리즈에 적용
```python
np.random.seed(1)
s2 = pd.Series(np.random.randint(6,size=100))

s2.value_counts() # 0,1,2,3,45의 값이 몇번 나왔는지
1    22
0    18
4    17
5    16
3    14
2    13
Name: count, dtype: int64

s2.value_counts(normalize=True) # 빈도의 비율 반환
1    0.22
0    0.18
4    0.17
5    0.16
3    0.14
2    0.13
Name: proportion, dtype: float64
```

범주형 데이터에 value_counts() 적용
```python
# 제공되는 타이타닉 승객 중 생존자수와 사망자수를 확인
# alive열은 생존여부가 yes/no로 표시되어있음

# 사망자/생존자 수
titanic['alive'].value_counts()
alive
no     549
yes    342
Name: count, dtype: int64

# 비율을 확인
titanic['alive'].value_counts(normalize=True) * 100
alive
no     61.616162
yes    38.383838
Name: proportion, dtype: float64

# 승객 중 남여 수 확인
titanic['sex'].value_counts()
sex
male      577
female    314
Name: count, dtype: int64

# 승객 중 남여 수의 비율을 확인
titanic['sex'].value_counts(normalize=True) * 100
sex
male      64.758698
female    35.241302
Name: proportion, dtype: float64
```

#### 데이터프레임에 적용
행을 하나의 value로 설정하고 동일한 행이 몇번 나타났는지 반환.  
행의 경우가 인덱스로 개수된 값이 value로 표시되는 Series 반환.  
```python
titanic[['sex','alive']].value_counts()
sex     alive
male    no       468
female  yes      233
male    yes      109
female  no        81
dtype: int64

type(titanic[['sex','alive']].value_counts())
# pandas.core.series.Series

titanic[['sex','alive']].value_counts().index
# MultiIndex([(  'male',  'no'),
#             ('female', 'yes'),
#             (  'male', 'yes'),
#             ('female',  'no')],
#            names=['sex', 'alive'])

titanic[['sex','alive']].value_counts().values
# array([468, 233, 109,  81])

pd.DataFrame(titanic[['sex','alive']].value_counts())
		        count
sex	    alive	
male	no	    468
female	yes	    233
male	yes	    109
female	no	    81
```

<br>

### sort_index(), sort_value()
정렬함수 - 데이터 정렬 시 사용  
sort_index() : 인덱스를 기준으로 정렬(default: 오름차순)  
sort_value() : 데이터 값을 기준으로 정렬(default: 오름차순)  
파라미터 : ascending = True/False (오름차순정렬 적용 유/무)  

#### 시리즈 정렬
sort_index()
```python
s2.value_counts() # 반환 결과는 빈도값을 기준으로 내림차순 정렬된 결과
1    22
0    18
4    17
5    16
3    14
2    13
Name: count, dtype: int64

s2.value_counts().sort_index() # index 값을 기준으로 오름차순 정렬
0    18
1    22
2    13
3    14
4    17
5    16
Name: count, dtype: int64

s2.value_counts().sort_index(ascending=False) # index 기준 내림차순 정렬
5    16
4    17
3    14
2    13
1    22
0    18
Name: count, dtype: int64
```

sort_values()
```python
s2.value_counts().sort_values() # 빈도값을 기준으로 오름차순 정렬
2    13
3    14
5    16
4    17
0    18
1    22
Name: count, dtype: int64

s2.value_counts().sort_values(ascending=False) # 빈도값을 기준으로 내림차순 정렬
1    22
0    18
4    17
5    16
3    14
2    13
Name: count, dtype: int64
```



#### 데이터프레임 정렬
df.sort_values() : 특정열 값 기준 정렬  
데이터프레임은 2차원 배열과 동일하기 때문에 정렬시 기준열을 줘야 한다. by 인수 사용(생략 불가)  
`by=기준열, by=[기준열1, 기준열2]`  
오름차순/내림차순 : ascending = True/False (default 오름차순)  

df.sort_index() : df의 index 기준 정렬  
오름차순/내림차순 : ascending = True/False (default 오름차순)  

sort_values()
```python
df1
	0	1	2	3
0	2	0	1	3.0
1	0	0	0	3.0
2	2	3	1	NaN
3	2	0	4	4.0

df1.sort_values(by=0)
	0	1	2	3
1	0	0	0	3.0
0	2	0	1	3.0
2	2	3	1	NaN
3	2	0	4	4.0

df1.sort_values(by=[0, 1])
	0	1	2	3
1	0	0	0	3.0
0	2	0	1	3.0
3	2	0	4	4.0
2	2	3	1	NaN

df1.sort_values(by=[0,1],ascending=False)
    0	1	2	3
2	2	3	1	NaN
0	2	0	1	3.0
3	2	0	4	4.0
1	0	0	0	3.0
```

sort_index()
```python
df
	    num_legs	num_wings
falcon	2	        2
dog	    4	        0
cat	    4	        0
ant	    6	        0

df.sort_index()
        num_legs	num_wings
ant	    6	        0
cat	    4	        0
dog	    4	        0
falcon	2	        2

df.sort_index(ascending=False)
	    num_legs	num_wings
falcon	2	        2
dog	    4	        0
cat	    4	        0
ant	    6	        0
```

<br>

### sum()
행과 열의 합계를 구할때는 df.sum() 함수 사용  
`sum(axis=0/1)` - axis default는 0  
각 열의 모든 행의 합계를 구할때: sum(axis=0)  
각 행의 모든 열의 합계를 구할때: sum(axis=1)  

```python
df2
	0	1	2	3	4	5	6	7
0	5	8	9	5	0	0	1	7
1	6	9	2	4	5	2	4	2
2	4	7	7	9	1	7	0	6
3	9	9	7	6	9	1	0	1

# df2의 각 행의 합계를 구하기
# 4행이므로 4개의 값을 시리즈로 반환
df2.sum(axis=1)
0    35
1    34
2    41
3    42
dtype: int64

# df2의 각 열의 합계를 구하기
# 8열이므로 8개의 값을 시리즈로 반환
df2.sum(axis=0) # or df2.sum()
0    24
1    33
2    25
3    24
4    15
5    10
6     5
7    16
dtype: int64
```

#### df의 기본 함수
mean(axis=0/1): 열, 행의 평균  
min(axis=0/1): 열, 행의 최소값  
max(axis=0/1): 열, 행의 최대값  

mean()
```python
df2.mean(axis=0) # 각 열의 평균
0    6.00
1    8.25
2    6.25
3    6.00
4    3.75
5    2.50
6    1.25
7    4.00
dtype: float64

df2.mean(axis=1) # 각 행의 평균
0    4.375
1    4.250
2    5.125
3    5.250
dtype: float64
```

min(), max()
```python
df2.min(axis=0) # 각 열의 최소값
0    4
1    7
2    2
3    4
4    0
5    0
6    0
7    1
dtype: int32

df2.max(axis=1) # 각 행의 최대값
0    9
1    9
2    9
3    9
dtype: int32
```

<br>

### drop()
행/열 삭제는 drop() 함수를 이용  
`df.drop('행이름', axis=0)` : 행 삭제. 행삭제 후 df로 결과를 반환  
`df.drop('행이름', axis=1)` : 열 삭제. 행삭제 후 df로 결과를 반환  
원본에 반영되지 않으므로 원본수정하려면 저장 해야 함  

열 삭제
```python
df2.drop(7, axis=1) # inplace=True 값 주면 원본에 반영 됨
    0	1	2	3	4	5	6
0	5	8	9	5	0	0	1
1	6	9	2	4	5	2	4
2	4	7	7	9	1	7	0
3	9	9	7	6	9	1	0

df2.drop(columns=7)
# 위와 같은 결과
```

행 삭제
```python
df2.drop(3, axis=0) # inplace=True 값 주면 원본에 반영 됨
    0	1	2	3	4	5	6	7
0	5	8	9	5	0	0	1	7
1	6	9	2	4	5	2	4	2
2	4	7	7	9	1	7	0	6

df2.drop(index=3)
# 위와 같은 결과
```

<br>

### dropna(), fillna()
NaN 값 처리(결측치 처리) 함수  

`df.dropna(axis=0/1)`  
- NaN값이 있는 열 또는 행을 삭제(axis=0 행삭제, axis=1 열삭제)
- 원본 반영되지 않음(inplace=True로 원본 반영)
- 행/열 단위 삭제가 일어남
- 결측치 수가 행이나 열단위로 상대적으로 많을 때 사용

<br>

`df.fillna(0)`
- NaN값을 정해진 숫자로 채움
- 아주 중요한 컬럼이거나 결측치 수가 상대적으로 적을 때 사용
- 시계열 data(시간의 흐름에 따라 변화량을 나타내는 데이터)인 경우 예측하기가 쉬워서 채워 사용하기도 함. (예를 들어, 오늘 시간별 날씨 data에서 오전 8시 데이터가 결측이면 7시나 9시 날씨 데이터로 채워서 사용)
- 원본 반영 되지 않음(inplace=True로 원본 반영)

dropna()
```python
df2
    0	1	2	3	4	5	6	7
0	NaN	8	9	5	0	0	1	7
1	6.0	9	2	4	5	2	4	2
2	4.0	7	7	9	1	7	0	6
3	9.0	9	7	6	9	1	0	1

# dropna() NaN 행을 삭제
df2.dropna(axis=0)
	0	1	2	3	4	5	6	7
1	6.0	9	2	4	5	2	4	2
2	4.0	7	7	9	1	7	0	6
3	9.0	9	7	6	9	1	0	1

# dropna() NaN 열을 삭제
df2.dropna(axis=1)
	1	2	3	4	5	6	7
0	8	9	5	0	0	1	7
1	9	2	4	5	2	4	2
2	7	7	9	1	7	0	6
3	9	7	6	9	1	0	1
```
df2의 결측치는 1개 결측치의 수가 적으므로 결측치를 임의의 data로 채워서 사용.  
보통 대표값인 평균값을 이용, 0이나 1도 이용하여 채움.  
시계열인 경우 결측데이터의 시간적으로 앞이나 뒤 data를 이용하여 채움.  
정해진 값은 없으므로 최소값/최대값을 이용하기도 하고 다른 유효한 데이터를 이용해서 예측해서 채우기도 함.  
머신러닝/딥러닝 전처리에서 위 내용을 다시 정리.  

fillna()
```python
df2.fillna(0) # 모든 NaN값은 0으로 채움
    0	1	2	3	4	5	6	7
0	0.0	8	9	5	0	0	1	7
1	6.0	9	2	4	5	2	4	2
2	4.0	7	7	9	1	7	0	6
3	9.0	9	7	6	9	1	0	1

df2.fillna(df2[0].mean()) # df2의 0번열의 평균값으로 채움
    0        	1	2	3	4	5	6	7
0	6.333333	8	9	5	0	0	1	7
1	6.000000	9	2	4	5	2	4	2
2	4.000000	7	7	9	1	7	0	6
3	9.000000	9	7	6	9	1	0	1

df2.astype(int) # df의 value의 type을 변경하는 함수, 원본에 영향 없음
df2 = df2.astype(int) # 원본 변경
	0	1	2	3	4	5	6	7
0	6	8	9	5	0	0	1	7
1	6	9	2	4	5	2	4	2
2	4	7	7	9	1	7	0	6
3	9	9	7	6	9	1	0	1
```

<br>

### apply()
열 또는 행에 동일한 연산 반복 적용할 때 `apply()` 함수 사용.  
`apply()` 함수는 DataFrame의 행이나 열에 복잡한 연산을 vectorizing할 수 있게 해주는 함수로 매우 많이 활용되는 함수임.  
동일한 연산을 모든열에 혹은 모든 행에 반복 적용하고자 할때 사용.  
`apply(반복적용할 함수, axis=0/1)`  
- 0 : 열마다 반복(default)
- 1 : 행마다 반복

```python
df3 = pd.DataFrame({
    'a':[1,3,4,3,4],
    'b':[2,3,1,4,5],
    'c':[1,5,2,4,4]
})
df3
	a	b	c
0	1	2	1
1	3	3	5
2	4	1	2
3	3	4	4
4	4	5	4

df3.apply(np.sum, axis=0)
a    15
b    15
c    16
dtype: int64
```
데이터프레임의 기본 집계함수(sum, min, max, mean 등)들은 행/열 단위 벡터화 연산을 수행함 => apply() 함수를 사용할 필요가 없음.  
일반적으로 apply() 함수 사용은 복잡한 연산을 해결하기 위한 lambda 함수나 사용자 정의 함수를 각 열 또는 행에 일괄 적용시키기 위해 사용.  
apply 함수를 사용해서 np.sum() 함수를 df3의 각 열에 적용해서 연산을 진행.  


titanic 데이터프레임의 'alive', 'sex', 'class' 각각의 열에 대해 value_counts 적용.
```python
titanic[['alive', 'sex', 'class']].apply(pd.value_counts)
	    alive	sex	    class
First	NaN	    NaN	    216.0
Second	NaN	    NaN	    184.0
Third	NaN	    NaN	    491.0
female	NaN	    314.0	NaN
male	NaN	    577.0	NaN
no	    549.0	NaN	    NaN
yes	    342.0	NaN	    NaN
```

함수로 apply 사용
```python
# 시리즈를 전달받아 해당 시리즈의 최대값과 최소값의 차이를 계산해서 반환하는 함수 diff(x) 정의
def diff(x):
    return x.max() - x.min()

df3.apply(diff, axis=0)
a    3
b    4
c    4
dtype: int64
```

lambda함수로 apply 사용
```python
# 시리즈를 전달받아 해당 시리즈의 최대값과 최소값의 차이를 계산해서 반환하는 lambda 함수
(lambda x: x.max() - x.min())(df3['a'])
# 3

df3.apply((lambda x: x.max() - x.min()), axis=0)
a    3
b    4
c    4
dtype: int64
```

<br>

### cut(), qcut()
데이터값을 카테고리 값으로 변환하는 함수  
값의 크기를 기준으로하여 카테고리 값으로 변환하고 싶을때 사용  

`cut(data, bins, label)`
- 기준값을 직접 설정할 수 있다
- data: 구간 나눌 실제값, bins: 구간 경계값, label: 카테고리값

<br>

`qcut(data, 구간수, labels=[d1,d2....])`
- 구간 경계선을 지정하지 않고 구간 수를 설정함. 데이터 개수가 같도록 지정한 수의 구간으로 분할.
- 구간수와 라벨의 수가 같아야 한다.
- ex. 1000개의 데이터를 4구간으로 나누려고 한다면
  - qcut 명령어를 사용 한 구간마다 250개씩 나누게 된다.
  - 예외로, 같은 숫자인 경우에는 같은 구간으로 처리한다. 그래서 구간마다의 개수가 정확하게 같지 않을 수도 있다.

cut()
```python
data = [0,0.5,4,6,4,5,2,10,21,23,37,15,38,31,61,20,41,31,100]

# 구간 경계값
# 구간 최소값 < 구간 <= 구간 최대값 (1~4, 5~18,...)
bins = [0, 4, 18, 25, 35, 60, 100]

# 각 구간의 이름: labels - 카테고리명
# bins 구간 순서와 동일하게 만들어야 함
labels = ['영유아', '미성년자', '청소년', '청년', '중년', '노년']

cats = pd.cut(data, bins, labels=labels)
cats
# [NaN, '영유아', '영유아', '미성년자', '영유아', ..., '노년', '청소년', '중년', '청년', '노년']
# Length: 19
# Categories (6, object): ['영유아' < '미성년자' < '청소년' < '청년' < '중년' < '노년']

type(cats)
# pandas.core.arrays.categorical.Categorical
```

#### Categorical 클래스 객체
`Categorical.categories`: 카테고리명 저장  
`Categorical.codes`: 코드 속성. 인코딩 된 결과값을 정수로 저장하고 있음
```python
cats.categories
# Index(['영유아', '미성년자', '청소년', '청년', '중년', '노년'], dtype='object')

cats.codes
# array([-1,  0,  0,  1,  0,  1,  0,  1,  2,  2,  4,  1,  4,  3,  5,  2,  4,  3,  5], dtype=int8)
# codes 값이 -1이면 범주화 실패(결측치)
```

qcut()
```python
np.random.seed(2)
data = np.random.randint(20, size=20)
data
# array([ 8, 15, 13,  8, 11, 18, 11,  8,  7,  2, 17, 11, 15,  5,  7,  3,  6,  4, 10, 11])

qcat = pd.qcut(data, 4, labels=['Q1', 'Q2', 'Q3', 'Q4'])
qcat
# ['Q2', 'Q4', 'Q4', 'Q2', 'Q3', ..., 'Q1', 'Q1', 'Q1', 'Q3', 'Q3']
# Length: 20
# Categories (4, object): ['Q1' < 'Q2' < 'Q3' < 'Q4']

qcat.value_counts()
Q1    5
Q2    5
Q3    5
Q4    5
dtype: int64
```

<br>

### set_index(), reset_index()
인덱스 설정 함수  
데이터 프레임 인덱스 설정을 위해 `set_index()`, `reset_index()`  

`set_index()`  
기존 행 인덱스를 제거하고 데이터 열 중 하나를 인덱스로 설정해주는 함수  

`reset_index()`  
기존 행인덱스를 제거하고 기본인덱스로 변경  
기본인덱스: 0부터 1씩 증가하는 정수 인덱스(따로 설정하지 않으면 기존 인덱스는 데이터열로 추가 됨)  
기존 인덱스를 데이터열로 추가하고 싶지 않다면, `reset_index(drop=True)`  

set_index()
```python
df3 = pd.DataFrame({
    'a':[1,3,4,3,4],
    'b':[2,3,1,4,5],
    'c':[1,5,2,4,4]
})
df3
    a	b	c
0	1	2	1
1	3	3	5
2	4	1	2
3	3	4	4
4	4	5	4

# a를 인덱스 열로 지정. inplace=True 해야 원본에 반영됨.
df3.set_index('a', inplace=True)
	b	c
a		
1	2	1
3	3	5
4	1	2
3	4	4
4	5	4
```

reset_index()
```python
# df3에 새로운 0 base 인덱스 추가. 기존 인덱스는 컬럼 데이터로 추가됨, 원본 반영되지 않음
df3.reset_index()
	a	b	c
0	1	2	1
1	3	3	5
2	4	1	2
3	3	4	4
4	4	5	4

# df3에 새로운 0 base 인덱스 추가. 기존 인덱스 열은 삭제. inplace=True 해야 원본에 반영됨.
df3.reset_index(drop=True, inplace=True)
df3
	b	c
0	2	1
1	3	5
2	1	2
3	4	4
4	5	4
```

<br>

### rename()
index명과 columns명 변경(부분변경)  
`rename(index={현재index: 바꿀index})` : 행인덱스 변경  
`rename(columns={현재컬럼명: 바꿀컬럼명})` : 컬럼명 변경  

```python
df3.rename(columns={'b': '학생수', 'c': '책상수'}, inplace=True)
df3
	학생수	책상수
0	2	   1
1	3	   5
2	1	   2
3	4	   4
4	5	   4

df3.rename(index={0: '1반', 1: '2반'}, inplace=True)
df3
	학생수	책상수
1반	 2	   1
2반	 3	   5
2	 1	   2
3	 4	   4
4	 5	   4
```