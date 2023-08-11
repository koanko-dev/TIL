# Pandas - pivot, pivot_table, groupby

<br>

## 피봇테이블
가지고 있는 데이터원본을 원하는 형태의 가공된 정보를 보여주는 것  
자료의 형태를 변경하기 위해 많이 사용하는 방법  

`pivot_table(data, values=None, index=None, columns=None, aggfunc='mean', margins=False, margins_name='All')`
- data : 분석할 데이터 프레임. 메서드 형식일때는 필요하지 않음 (ex. `df1.pivot_table()`)
- values : 분석할 데이터 프레임에서 분석할 열
- index : 행 인덱스로 들어갈 키열 또는 키열의 리스트
- columns : 열 인덱스로 들어갈 키열 또는 키열의 리스트
- fill_value : NaN이 표출될 때 대체값 지정
- margins : 모든 데이터를 분석한 결과를 행으로 표출할 지 여부
- margins_name : margins가 표출될 때 그 열(행)의 이름

<br>

### 피봇테이블을 작성할 때 반드시 설정해야 되는 인수
- data : 사용 데이터 프레임
- index : 행 인덱스로 사용할 필드
- columns : 열 인덱스로 사용할 필드
나머지 값(data)은 수치 data만 사용함  
기본 함수가 평균(mean)함수 이기 때문에 각 데이터의 평균값이 반환  

```python
data = {
    "도시": ["서울", "서울", "서울", "부산", "부산", "부산", "인천", "인천"],
    "연도": ["2015", "2010", "2005", "2015", "2010", "2005", "2015", "2010"],
    "인구": [9904312, 9631482, 9762546, 3448737, 3393191, 3512547, 2890451, 263203],
    "지역": ["수도권", "수도권", "수도권", "경상권", "경상권", "경상권", "수도권", "수도권"]
}
columns = ["도시", "연도", "인구", "지역"]
df1 = pd.DataFrame(data, columns=columns)
df1
	도시	연도	 인구	  지역
0	서울	2015	9904312	수도권
1	서울	2010	9631482	수도권
2	서울	2005	9762546	수도권
3	부산	2015	3448737	경상권
4	부산	2010	3393191	경상권
5	부산	2005	3512547	경상권
6	인천	2015	2890451	수도권
7	인천	2010	263203	수도권

# 각 도시에 대한 연도별 인구 평균
df1.pivot(index="도시", columns="연도", values="인구")
연도	2005	    2010	    2015
도시			
부산	3512547.0	3393191.0	3448737.0
서울	9762546.0	9631482.0	9904312.0
인천	NaN	        263203.0	2890451.0

df1.pivot_table(index="도시", columns="연도", values="인구")
# 위와 같은 결과가 나온다.

pd.pivot_table(data=df1[['도시','연도','인구']], index="도시", columns="연도", values="인구")
# 위와 같은 결과가 나온다.

# 각 지역별 도시에 대한 연도별 인구 평균
df1.pivot(index=["지역", "도시"], columns="연도", values="인구")
	    연도	    2005	    2010	    2015
지역     도시			
경상권    부산	    3512547.0	3393191.0	3448737.0
수도권	  서울	    9762546.0	9631482.0	9904312.0
        인천	    NaN	       263203.0	  2890451.0

df1.pivot(index=["지역", "도시"], columns="연도")
# 위와 같은 결과가 나온다.

df1.pivot_table(index=["지역", "도시"], columns="연도")
# 위와 같은 결과가 나온다.

pd.pivot_table(df1, index=["지역", "도시"], columns="연도")
# 위와 같은 결과가 나온다.
```

```python
import pandas as pd
import seaborn as sns

df = sns.load_dataset('titanic')[['age','sex','class','fare','survived']]
df.head()
    age	    sex	    class	fare	survived
0	22.0	male	Third	7.2500	0
1	38.0	female	First	71.2833	1
2	26.0	female	Third	7.9250	1
3	35.0	female	First	53.1000	1
4	35.0	male	Third	8.0500	0

# 선실 등급별로 숙박객의 성별 평균 나이
df1 = pd.pivot_table(df, # 피벗할 data
                     index = 'class', # 행인덱스
                     columns = 'sex', # 컬럼
                     values = 'age', # 계산열/요약열/분석열
                     aggfunc = 'mean' # 데이터 집계 함수/요약함수
                    )
df1
sex	    female	    male
class		
First	34.611765	41.281386
Second	28.722973	30.740707
Third	21.750000	26.507589
```

<br>
<br>

## 그룹 분석
요약 통계의 목적.

<br>

### groupby 메서드
groupby 메서드는 데이터를 그룹 별로 분류하는 역할을 함  
groupby 메서드의 인수  
- 열 또는 열의 리스트
- 행 인덱스
  
연산 결과로 그룹 데이터를 나타내는 GroupBy 클래스 객체를 반환  
- 이 객체에는 그룹별로 연산을 할 수 있는 그룹연산 메서드가 있음

<br>

### GroupBy 클래스 객체의 그룹연산 메서드
- size, count: 그룹 데이터의 갯수
- mean, median, min, max: 그룹 데이터의 평균, 중앙값, 최소, 최대
- sum, prod, std, var, quantile : 그룹 데이터의 합계, 곱, 표준편차, 분산, 사분위수
- first, last: 그룹 데이터 중 가장 첫번째 데이터와 가장 나중 데이터

<br>

### 이 외에도 많이 사용되는 그룹 연산
- agg, aggregate
    - 만약 원하는 그룹연산이 없는 경우 함수를 만들고 이 함수를 agg에 전달한다.
    - 또는 여러가지 그룹연산을 동시에 하고 싶은 경우 함수 이름 문자열의 리스트를 전달한다.
- describe
    - 하나의 그룹 대표값이 아니라 여러개의 값을 데이터프레임으로 구한다.
- apply
    - describe 처럼 하나의 대표값이 아닌 데이터프레임을 출력하지만 원하는 그룹연산이 없는 경우에 사용한다.
- transform
    - 그룹에 대한 대표값을 만드는 것이 아니라 그룹별 계산을 통해 데이터 자체를 변형한다.

<br>

### 그룹핑된 요약을 확인: `그룹객체변수.groups(<나눌 열 or 행>)`
```python
np.random.seed(0)
df2 = pd.DataFrame({
    'key1': ['A', 'A', 'B', 'B', 'A'],
    'key2': ['one', 'two', 'one', 'two', 'one'],
    'data1': [1, 2, 3, 4, 5],
    'data2': [10, 20, 30, 40, 50]
})
df2
	key1	key2	data1	data2
0	A	    one	    1	    10
1	A	    two	    2	    20
2	B	    one	    3	    30
3	B	    two	    4	    40
4	A	    one	    5	    50

groups = df2.groupby(df2.key1) # 데이터프레임이 A, B로 두개로 쪼개진 것과 같다.
groups
# <pandas.core.groupby.generic.DataFrameGroupBy object at 0x128b18d10>

groups.groups # 딕셔너리 형태 반환
# {'A': [0, 1, 4], 'B': [2, 3]}

groups.groups.keys()
# dict_keys(['A', 'B'])

groups.groups['A'] # key A로 분류된 그룹에 속하는 행은 0, 1, 4행임
# Int64Index([0, 1, 4], dtype='int64')
```

굳이 내용을 확인해보자면,
```python
pd.DataFrame(groups).loc[0].values[1]
    key1	key2	data1	data2
0	A	    one	    1	    10
1	A	    two	    2	    20
4	A	    one	    5	    50

pd.DataFrame(groups).loc[1].values[1]
	key1	key2	data1	data2
2	B	    one	    3	    30
3	B	    two	    4	    40
```

groups.sum 같은 연산을 하면, 분류기준열인 key1은 연산에서 제외되고 나머지 key2, data1, data2에 대해 연산이 적용됨
```python
groups.sum()
	key2        data1	data2
key1		
A	onetwoone    8	    80
B	onetwo       7	    70
```

특정 컬럼만 뽑아서 연산 진행
```python
groups['data1'].sum() # groupby객체에서 특정컬럼을 추출한 후 합계연산 진행
key1
A    8
B    7
Name: data1, dtype: int64

groups.sum()['data1'] # groupby객체에서 전체컬럼에 대해 합계연산 진행 후 특정컬럼 추출
key1
A    8
B    7
Name: data1, dtype: int64
```

<br>

#### 그룹 함수 예제
apply()/agg()

DF 등에 벡터라이징 연산을 적용하는 함수(axis = 0/1 이용하여 행/열 적용가능)  
agg 함수는 숫자 타입의 스칼라만 리턴하는 함수를 적용하는 apply의 특수한 경우  
스칼라 : 하나의 수치(數値)만으로 완전히 표시되는 양. 방향의 구별이 없는 물리적 수량임. 질량·에너지·밀도(密度)·전기량(電氣量) 따위.  