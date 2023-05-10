# 제어문(Control Statement)
순차적인 흐름을 제어하는 제어문은 크게 두가지로 나뉩니다.  
- 조건문
- 반복문

<br>
<br>

## 조건문(Conditional Statement)
### if, elif
참/거짓을 판단할 수 있는 조건과 함께 사용합니다.  
JavaScript에서 `{}` 블록으로 경계를 만드는 것과 달리, 파이썬에서는 조건문 뒤에 `:` 을 붙이고 코드블럭에서 tab(4 spaces) 만큼의 들여쓰기를 해 경계를 나눕니다.
```python
if <expression>:
    <code>
elif <expression>:
    <code>
else:
    <code>
```
파이썬의 `if` 또한 중첩으로 `if` 조건문이 가능합니다.

<br>

### 삼항 연산자(Ternary Operator)
파이썬 또한 삼항 연산자를 사용할 수 있습니다. 조건 표현식(Conditional Expression)이라고도 합니다.
```python
true_value if <expression> else false_value

num = 8
result_num = 'odd number' if num % 2 else 'even number'
print(result_num)
# 'even number'
```

<br>
<br>

## 반복문(Loop Statement)
- while
- for(in)

<br>

### while loop
`while` 루프는 조건식이 참인 경우 반복적으로 코드가 실행됩니다.  
**원하는 실행이 끝난 뒤 조건식이 거짓으로 설정되도록, 반드시 종료 조건을 설정해야 합니다.**  
```python
while <expression>:
    <code>
```

<br>

### for loop
`for` 루프는 시퀀스를 포함한 순회가능한 iterable 요소들을 순회합니다.  
```python
for <el> in <iterable data>:
    <code>
```
파이썬의 `while` 루프와 `for` 루프 또한 중첩으로 사용 가능합니다.  

반복문에서 임시변수가 쓰이지 않으면, 그 임시변수는 `_` 로 쓰는 것이 컨벤션입니다. 해당 `for` 루프에서 나오지 않을 것이라는 뜻을 암묵적으로 내포하고 있습니다.  

<br>

#### `for loop` - 딕셔너리 순회
```python
ex_dict = {'id': 124421, 'name': 'koanko'}

for key in ex_dict:     # 임시변수 자리에 key
    print(ex_dict[key]) # value 접근
# 124421
# 'koanko'
    
for k_v in ex_dict.items():
    print(k_v, type(k_v))
# ('id', 124421) <class 'tuple'>
# ('name', 'koanko') <class 'tuple'>
    
for k, v in ex_dict.items():
    print(f'{k} => {v}')
# id => 124421
# name => koanko
```

<br>

#### `enumerate()`
list를 가지고 있는 요소의 값들과 index 값을 함께 활용할 수 있게 합니다.  
for 루프에서 인덱스가 필요할 때 사용합니다.
```python
colors = ['red', 'blue']
print(enumerate(colors))
# <enumerate object at 0x11301fc80>

for e_color in enumerate(colors):
    print(e_color)
    print(f'index {e_color[0]} => value {e_color[1]}')
# (0, 'red')
# index 0 => value red
# (1, 'blue')
# index 1 => value blue

for idx, color in enumerate(colors):
    print(f'idx {idx} => {color}')
# idx 0 => red
# idx 1 => blue
```

<br>

#### List Comprehension
표현식과 제어문을 통해 리스트를 생성합니다.  
복잡하지 않은 코드의 경우 한 줄로 간단하게 줄일 수 있는 것이 장점입니다.
```python
[<expression> for <el> in <iterable>]
list(<expression> for <el> in <iterable>)

# 1~3 까지의 세제곱 리스트 생성
print([num**3 for num in range(1, 4)])
# [1, 8, 27]
```
중첩도 가능합니다.

<br>

#### Dictionary comprehension
comprehension을 사용해 딕셔너리도 생성할 수 있습니다.
```python
{<key: val> for <el> in <iterable>}
dict({<key: val> for <el> in <iterable>})

# 1~3 까지의 세제곱 딕셔너리 생성
print{{num: num**3 for num in range(1, 4)}}
# {1: 1, 2: 8, 3: 27}
```

<br>
<br>

### 반복제어(`break`, `continue`, `for-else`)
`for` 또는 `while` 루프에서 활용합니다.

<br>

#### `break`
`break` 를 사용하면, 코드는 완전히 `for` 루프 밖으로 빠져나가게 됩니다.
```python
for num in range(10):
    print(num)
    
    if num == 3:
        print('3 break')
        break
# 0
# 1
# 2
# 3
# 3 break
```

<br>

#### `continue`
`continue` 는 이후의 코드는 실행하지 않고, 다시 루프를 시작합니다.  
```python
nums = [10, 2, 8, 13, 21, 11]

for num in nums:
    if num <= 10:
        continue
    print(f'{num} is more than 10')
# 13 is more than 10
# 21 is more than 10
# 11 is more than 10
```

<br>

#### `pass`
`pass` 는 아무 기능을 하지 않습니다.  
`for` 루프 안, 들여쓰기 이후 코드를 작성해야 오류가 나지 않습니다. 임시적으로 작성해야 할 코드가 없을 때 자리를 채우는 용도로 사용할 수 있습니다.
```python
for num in range(10):
    pass
# 아무 일도 일어나지 않습니다. 오류 또한 없습니다.
```

<br>

#### `else`
반복문을 끝까지 반복하고 난 뒤에 실행됩니다.  
`break` 를 사용할 때에만 `else` 는 의미가 생깁니다. `break` 로 중간에 종료되지 않고 모든 반복이 끝난 경우에만 실행되기 때문입니다.  
**반복문이 `break` 로 종료될 때는 실행되지 않습니다.**
```python
random_string = 'ASDASDKFK:NAA:'

for char in random_string:
    if char == 'c':
        print('found C')
        break
else:
    print('C doesn\'t exist')
# 'C doesn't exist'
```

<br>