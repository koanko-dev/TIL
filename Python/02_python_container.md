# Python Container
컨테이너는 여러 값을 저장할 수 있는 객체를 의미합니다.  
크게 두 가지로 나뉩니다.  
- 시퀀스(Sequence)형: 순서가 있는 ordered 데이터
- 비시퀀(Non-sequence)형: 순서가 없는 unordered 데이터

<br>
<br>

## 시퀀스(sequence)형 컨테이너
순서대로 나열(ordered)된 데이터입니다. 정렬(sorted)된 것은 아닙니다.

- 리스트(list)
- 튜플(tuple)
- 레인지(range)
- 문자열(string)

<br>

### 리스트(list)
리스트는 index로 접근 및 변경(Mutate) 가능합니다.
```python
num_list = [1, 2, 3, 4, 5] # 리스트, 튜플의 변수명은 반드시 복수형 단어로 지어야 합니다.
print(num_list[2])
# 3

# mutate
num_list[2] = 'three'
print(num_list)
print(id(num_list))
# [1, 2, 'three', 4, 5]
# 4402326784

# re-assignment
num_list = [1, 2, 'three', 4, 5]
print(num_list)
print(id(num_list))
# [1, 2, 'three', 4, 5]
# 4402840384 => 재할당으로, 내용은 같아도 id 값이 다른 것을 볼 수 있습니다.
```

<br>

### 튜플(tuple)
리스트와 유사한 튜플은 `()`로 묶어 표현합니다.  
튜플은 재할당(re-assignment)은 가능해도, 수정(mutate)이 불가능(불변, immutable)합니다.  

```python
new_tuple = (1, 2)
```

마지막 항목에 붙은 쉼표는 생략할 수 있지만, 단일 항목 생성의 경우 값 뒤에 쉼표를 붙여야 합니다.
```python
one_el_tuple = (30, )
print(type(one_el_tuple))
# <class 'tuple'>
```

파이썬은 빈 괄호를 튜플로 처리합니다.
```python
empty_tuple = ()
print(type(empty_tuple), len(empty_tuple))
# <class 'tuple'> 0
```

튜플 또한 순서가 있기 때문에 index로 접근 가능합니다.  
```python
new_tuple = (1, 2, 3, 4, 5)
print(new_tuple[3])
# 4
```

아래에서 다룰 딕셔너리는 key, value 값을 튜플로 나타냅니다.
```python
user = {'id': 124421, 'name': 'koanko', 'user_name': 'koanko.dev'}
print(user.items())
# dict_items([('id', 124421), ('name', 'koanko'), ('user_name', 'koanko.dev')])
```

<br>

### 레인지(range)
정수의 범위를 쉽게 나타낼 수 있습니다.  
```python
range(n) # 0 ~ n-1 값을 가집니다.
range(10) # 0 ~ 9

range(n, m) # n ~ m-1 값을 가집니다. (n이상, m미만)
range(2, 12) # 2 ~ 11

range(n, m, s) # n ~ m-1 까지 s(스텝)만큼 증가하는 값을 가집니다.
range(10, 20, 2) # 10, 12, 14, 16, 18 범위의 값을 가집니다.
```

`list()`로 형변환하여 사용할 수도 있습니다.  
```python
r1 = range(0, -10)
print(list(r1))
# []
# 기본 스텝이 1 이기에 음수 방향으로 아무것도 읽지 못해 아무것도 나오지 않았습니다.

r2 = range(0, -10, -1)
print(list(r2))
# [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
# 스텝을 음수로 지정하면 시작숫자 0에서, 끝 숫자 -10 방향으로 -1씩 읽습니다.
```

range 또한 index로 접근 가능합니다.
```python
r = range(0, 10)
print(r[1], r[-1])
# 1, 9
```

<br>

### ❓ 패킹 / 언패킹 연산자 (Packing / Unpacking Operator)
시퀀스형은 패킹, 언패킹이 가능합니다.  
연산자 `*` 를 사용하여 패킹, 언패킹 합니다.

#### 패킹
```python
a, b, *c = 1, 2, 3, 4, 5
print(a, b, c)
# 1 2 [3, 4, 5]

a, *b, c = 1, 2, 3, 4, 5
print(a, b, c)
# 1 [2, 3, 4] 5

*a, b, c = 1, 2, 3, 4, 5
print(a, b, c)
# [1, 2, 3] 4 5
```

#### 언패킹
```python
numbers = [2, 4, 6]

def multiply(x, y, z):
    return x * y * z

print(multiply(*numbers))
# 48
```

<br>
<br>

## 비시퀀(Non-sequence)형 컨테이너
순서가 없는(unordered) 데이터를 말합니다.  

- 세트(set)
- 딕셔너리 (dictionary)

<br>

### 세트(set)
세트는 순서가 없고 중복된 값이 없는 자료구조입니다.  
`{}`(중괄호)로 만들지만, 빈 중괄호는 밑에 설명할 딕셔너리를 뜻하기에 `{}` 만으로는 빈 세트를 생성할 수 없습니다. 세트를 생성하기 위해서는 `set()`로 생성해야 합니다.  
수학의 집합과 동일하게 처리되며, 순서가 없고 중복된 값이 없습니다.


```python
set_a = {2, 4, 5, 6}
set_b = {3, 5, 6, 9}

# 차집합은 - 연산자를 사용합니다.
print(set_a - set_b)
# {2, 4}

# 합집합은 | 연산자를 사용합니다.
print(set_a | set_b)
# {2, 3, 4, 5, 6, 9}

# 교집합은 & 연산자를 사용합니다.
print(set_a & set_b)
# {5, 6}

# set는 중복된 값이 있을 수 없습니다.
set_c = {0, 0, 1, 2, 2, 3, 4, 4}
print(set_c)
# {0, 1, 2, 3, 4}
# 따라서, list를 set으로 형변환하여 중복된 값을 쉽게 제거할 수 있습니다.
```

<br>

### 딕셔너리 (dictionary)
`key`, `value`가 쌍으로 이루어져 있으며, JavaScript의 Object와 유사합니다.  
`key`는 중복될 수 없으며, 변경불가한 데이터로만 가능합니다. (string, integer, float, boolean, tuple, range)  
`value`는 list, dictionary를 포함한 모든 것이 값으로 가능합니다.  

```python
user = {'id': 124421, 'name': 'koanko', 'user_name': 'koanko.dev'}

# .keys() 메서드로 key를 확인할 수 있습니다.
print(user.keys())
# dict_keys(['id', 'name', 'user_name'])

# .values() 메서드로 value를 확인할 수 있습니다.
print(user.values())
# dict_values([124421, 'koanko', 'koanko.dev'])

# .items() 메서드로 key와 value를 모두 확인할 수 있습니다.
print(user.items())
# dict_items([('id', 124421), ('name', 'koanko'), ('user_name', 'koanko.dev')])
```

key로 접근하여 추출과 변경을 할 수 있습니다.
```python
user = {'id': 124421, 'name': 'koanko', 'user_name': 'koanko.dev'}

# 추출
print(user['id'])
# 124421

# 변경
user['name']: 'anko'
print(user)
# {'id': 124421, 'name': 'koanko', 'user_name': 'koanko.dev'}
```

<br>
<br>

## 컨테이너형 형변환

- `str()` : list, tuple, range, set, dictionary => str 변환
- `list()` : string, tuple, range, set, dictionary(key) => list 변환
- `tuple()` : string, list, range, set, dictionary(key) => tuple 변환
- `set()` : string, list, range, tuple, dictionary(key) => set 변환
  
어떤 타입도 range와 dictionary로는 형변환이 불가합니다.

<br>
<br>

## Sequence/Unordered & Mutable/Immutable
시퀀스형과 비시퀀스형에 대해 정리해봅시다.  
|Sequence|Unordered|
|:-:|:-:|
|string|set|
|list|dictionary|
|tuple||
|range||


가변과 불변에 대해 정리해봅시다.  
|Mutable|Immutable|
|:-:|:-:|
|list|string|
|set|tuple|
|dictionary|range|

<br>
<br>

## 시퀀스형 연산자(Sqeuence Type Operator)

### 산술연산자 (+)
```python
# string
12 + 'a'
# '12a'

# list
[1, 2] + ['a']
# [1, 2, 'a']

# tuple
(1, 2) + ('a', )
# (1, 2, 'a')

# range 불가능
```

<br>

### 반복연산자 (*)
```python
# string
'hello' * 3
# 'hellohellohello'

# list
[2] * 5
# [2, 2, 2, 2, 2]

# tuple
(1, 2) * 3
# (1, 2, 1, 2, 1, 2)

# range 불가능
```

<br>
<br>

## 슬라이싱(Slicing)
시퀀스를 특정 단위로 슬라이싱 할 수 있습니다.  

`[n:m:s]`  
n: 시작점을 의미합니다. 범위 밖이어도 작동합니다.  
m: 끝점-1 을 의미합니다. 범위 밖이어도 작동합니다.  
s(스텝): 움직일방향 + 칸수를 의미합니다. default 값은 1이며, -1 입력 시 역방향으로 갑니다.


```python
s = 'abcdefghi'
print(s[:3])   # 처음 ~ 2 => 'abc'
print(s[5:])   # 5 ~ 마지막 => 'fghi'
print(s[::])   # abcdefghi
print(s[::-1]) # ihgfedcba

num_list = [1, 2, 3, 4, 5, 6, 7, 8]
print(num_list[::2]) # 2스탭씩 띄어서 => [1, 3, 5, 7]
print(num_list[2:5:-1]) # 2의 역방향으로 가기 때문에 출력할 것 없음 => []
print(num_list[5:2:-1]) # 시작 5 ~ 끝 2 전까지 =>  [6, 5, 4]

print('abcd'[1:2])
# 'b'

print([1, 2, 3, 4][2:10])
# [3, 4]

print((1, 2, 3, 4)[:2])
# (1, 2)

print((range(10)[5:8]))
# range(5, 8)
```

<br>