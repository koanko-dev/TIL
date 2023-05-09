# Python 기초

## 기초문법
- `#`: 한 줄을 주석 처리할 때 사용합니다. `#` 뒤에 한 칸 띄어 쓰는 것이 컨벤션입니다.
- `"""` or `'''`: 여러 줄을 주석 처리할 때 사용합니다.
- 파이썬에서는 `;` 를 사용하지 않습니다. 한 줄로 표기할 때 작성할 수는 있지만, 기본 '1 line 1 statement'이 원칙입니다.
- `print()` 함수로 출력합니다. 여러 줄을 출력할 땐 `"""`를 사용하는 것이 컨벤션입니다.

<br>
<br>

## 변수(Variable)
변수를 할당할 때, `=` 을 통해 할당합니다.  
JavaScript의 let과 var와 같은 변수 선언은 따로 없습니다.
```python
greeting = 'hello'
print(greeting)
# 'hello'
```

`type()` 함수로 데이터 타입을 확인합니다.
```python
type(greeting)
# str
```

`id()` 함수로 메모리 주소 확인할 수 있습니다.
```python
id(greeting)
# 4352828976
```

파이썬에서는 아래와 같이 값을 동시에 할당할 수 있습니다.
```python
a = b = 100
print(a, b)
# 100 100

a, b = 10, 20
print(a, b)
# 10, 20

a = 1
b = 2
print(a, b)
# 1 2
a, b = b, a
print(a, b)
# 2 1
```

<br>

### 식별자(Identifiers)
파이썬의 식별자는 변수, 함수, 모듈, 클래스 등을 식별하는데 사용되는 이름입니다.  
파이썬은 스네이크케이스([snake case](https://www.freecodecamp.org/news/snake-case-vs-camel-case-vs-pascal-case-vs-kebab-case-whats-the-difference/))를 사용합니다.  
첫 글자에는 숫자가 올 수 없습니다.  
예약어는 사용할 수 없습니다.

<br>
<br>

## 데이터 타입(Data Type)
### Value Type
- 숫자(Number)
- 문자열(String)
- 참/거짓(Boolean)

`type()` 함수로 데이터 타입을 확인할 수 있습니다.

<br>

### Number Type
#### 1. `int` (정수, ingteger)
파이썬에서 정수는 `int`로 표현됩니다.

<br>

#### 2. `float` (부동소수점, 실수, floating point number)
파이썬에서 실수는 `float`로 표현됩니다.  
지수 표현 방식을 사용하면 float 자료형으로 변환됩니다.
```python
a = 100e2
b = 314e-2
print(a, b)
print(type(a), type(b))
# 10000.0 3.14
# <class 'float'> <class 'float'>
```

실수를 연산할 경우 원하는대로 값이 나오지 않는 경우가 많습니다.  
```python
print(4.6 - 3.34)
# 1.2599999999999998
```

때문에 값을 비교할 때 어려움을 겪을 수 있는데, 여러 방법이 있지만 다음과 같이 처리할 수도 있습니다.
```python
import math

a = 4.6 - 3.34
b = 1.26

math.isclose(a, b)
# True
```

<br>

#### 3. `complex` (복소수, complex number)
`complex` 복소수는 실수부와 허수부를 가집니다.  
복소수는 허수부를 `j`로 표현합니다.  
```python
a = 6 - 3j
type(a)
# complex

b = '6+8j' # 공백 포함 불가능
c = complex(b)
print(c, type(c))
# (6+8j) <class 'complex'>
```

<br>

### String Type
파이썬에서 string은 `'` 작은따옴표 또는 `"` 큰 따옴표로 묶어 표현합니다.  
다른 언어와 마찬가지로 `input()`을 통해 받는 값은 string 입니다.

#### Excape Sequence
|문자|의미|
|-|-|
|\n|줄 바꿈|
|\t|탭|
|\r|캐리지리턴|
|\0|Null|
|\\\\ |\ |
|\'|'|
|\"|"|
```python
print('hello') # end option default value = '\n'
print('world')
# hello
# world

print('hello', end='\t')
print('world')
# hello world
```

<br>

#### ❓ f-string
`f-string`을 활용하면 문자열 안에서 형식을 지정할 수 있습니다.  
JavaScript에서 백틱(\`)을 사용해 문자열을 표현하면서 내부에 표현식을 포함할 수 있듯이, 파이썬에서는 문자열 앞에 `f`를 붙여 문자열 내부에서 표현식을 쓸 수 있습니다.  
```python
import datetime

today = datetime.datetime.now()
print(today)
# 2023-05-09 21:43:37.056966

print(f'지금은 {today.year}년 {today.month}월 {today.day}일 {today.hour}시 입니다.')
# 지금은 2023년 5월 9일 21시 입니다.
```

<br>

### Boolean Type
`True` 와 `False` 로 이루어져 있습니다.  
파이썬은 보다시피 `True`와 `False`의 첫 글자를 대문자로 씁니다.

파이썬은 다음과 같은 값들을 `False`로 변환합니다.
- `0`
- `0.0`
- `()`
- `[]`
- `{}`
- `''`
- `None`(파이썬은 값이 없을 때 null 대신 `None`을 씁니다.)

<br>

### 형변환(Type conversion, Typecasting)

#### 1. 암시적 형변환(Implicit Type Conversion)
의도하지 않게 파이썬 내부적으로 자동으로 형변환 하는 경우를 말합니다.
```python
print(True + 2)
# 3

i = 2
f = 3.6
c = 1+4j
print(i + f, type(i + f))
# 5.6 <class 'float'>

print(i + c, type(i + c))
# (3+4j) <class 'complex'>
```

<br>

#### 2. 명시적 형변환(Explicit Type Conversion)
의도적으로 형변환 하는 경우를 말합니다. 암시적 형변환 외에 형변환을 원한다면, 모두 명시하여 형변환을 해줘야 합니다.
```python
int() # string, float => int 변환
float() # string, int => float 변환
str() # int, float, list, tuple, dictionary => str 변환
list() # string, tuple, range, set, dictionary(key) =>  list 변환
tuple() # string, list, range, set, dictionary(key) =>  list 변환
set() # string, list, range, tuple, dictionary(key) =>  list 변환
```

<br>
<br>

## 연산자(Operator)
### 산술 연산자
|연산자|내용|
|-|-|
|+|덧셈|
|-|뺄셈|
|\*|곱셈|
|/|나눗셈|
|//|몫|
|%|나머지(modulo)|
|\*\*|거듭제곱|

나누기를 하게 되면 `float`를 돌려줍니다.

<br>

### 비교 연산자
|연산자|내용|
|-|-|
|`<`|미만|
|`<=`|이하|
|`>`|초과|
|`>=`|이상|
|`==`|같음|
|`!=`|같지않음|
|`is`|객체 아이덴티티|
|`is not`|부정된 객체 아이덴티티|

<br>

### 논리 연산자
|연산자|내용|
|-|-|
|a and b|a와 b 모두 True시만 True|
|a or b|a 와 b 모두 False시만 False|
|not a|True -> False, False -> True|

<br>

#### ❓ 단축평가(Short Circuit Evaluation)
첫 번째 값이 확실할 때, 두 번째 값은 확인 하지 않는 것을 말합니다. 때문에 속도가 향상됩니다.
```python
print('a' and 'b')
# b

print(True and {} and 3 and '' and ())
# {}

print('a' or 'b')
# a

print(0 or '' or None or 0.0 or [] or 100 or True or 1 or 'hello')
# 100
```
`and` 는 둘 다 True일 경우만 True이기 때문에 첫번째 값이 True라도 두번째 값을 확인해야 하기 때문에 'b'가 반환됩니다.  
`or` 는 하나만 True라도 True이기 때문에 True를 만나면 해당 값을 바로 반환합니다.

```python
print((3 and 5) , (3 and 0), (0 and 3), (0 and 0))
# (5, 0, 0, 0)

print((3 or 5) , (3 or 0), (0 or 3), (0 or 0))
# (3, 3, 3, 0)
```

<br>

### 복합 연산자
|연산자|내용|
|-|-|
|a += b|a = a + b|
|a -= b|a = a - b|
|a \*= b|a = a \* b|
|a /= b|a = a / b|
|a //= b|a = a // b|
|a %= b|a = a % b|
|a \*\*= b|a = a ** b|

<br>

### 기타 연산자
`in`: `in` 연산자를 통해 요소가 속해있는지 여부를 확인할 수 있습니다.  
`is`: `is` 연산자를 통해 동일한 object인지 확인할 수 있습니다.

<br>

### 연산자 우선순위
0. `()`을 통한 grouping
1. Slicing
2. Indexing
3. 제곱연산자  
    `**`
4. 단항연산자   
    `+`, `-` (음수/양수 부호)
5. 산술연산자  
    `*`, `/`, `%`
6. 산술연산자  
    `+`, `-`
7. 비교연산자, `in`, `is`
8. `not`
9. `and` 
10. `or`

[연산자 우선순위 파이썬 문서](https://docs.python.org/ko/3/reference/expressions.html#operator-precedence)

<br>