# Python Function
## Python 함수 형식
함수는 기능을 재사용 가능하게 하는 이점을 줍니다. 이를 통해 반복되는 코드를 없앨 수 있습니다.  
때문에 가독성과 유지보수가 편합니다.  

파이썬의 함수 또한 JavaScript의 함수와 유사합니다.  
매개변수(parameter)를 받거나 받지 않을 수도 있고, 값을 return 할 수도 하지 않을 수도 있습니다.    
함수에서 값이 return 되거나 종료되면, 함수를 호출한 곳으로 돌아가 다시 작업을 수행합니다.

값을 반환한다면, 오직 한개의 객체만 반환됩니다.  
따라서, return 값이 2개 이상 콤마(,)로 묶여 반환되면, 결과값은 해당 값들을 묶은 하나의 튜플로 반환되게 됩니다.
```python
# 함수 정의
def <function_name>(<parameter1>, <parameter2>, ...):
    <code block>
    return <return_value>

# 함수 호출
<function_name>(<argument>)
```

<br>
<br>

## 함수는 결국 변수다
`a = 1` 코드는 `a` 라는 변수에 `1` 이라는 값이 저장되는 것처럼, 함수도 마찬가지입니다.  
`multiply` 라는 함수가 있다면, `multiply` 변수에 multiply 함수의 내용이 저장되는 것입니다. 실제로 메모리에서 값을 저장할 때에도 변수를 저장할 때와 같은 일이 일어납니다.

따라서, 두번 같은 함수를 선언한다면 변수의 값이 재할당 되는 것과 같은 원리이기 때문에, 더 아래쪽에 있는 함수가 호출됩니다.  
아래 작성한 코드를 통해 확인해 볼 수 있습니다.  
```python
def multiply(a, b):
    print('first multiply')
    return a * b

def multiply(a, b):
    print('second multiply')
    return a * b

print(multiply(2, 4))
# 'second multiply'
# 8
```

<br>
<br>

## 기본 인자 값(Default Argument Values)
기본 인자 값이란, `argument` 로 입력된 값이 없을 때 `argument` 로 사용할 값을 미리 설정하는 것을 말합니다.
`parameter` 에서 기본 인자값을 설정할 수 있습니다.
```python
def <function_name>(<parameter>=<default_argument>):
    <code block>
    return <return_value>

def sum_num(a, b=5):
    return a + b
print(sum_num(3))
# 8
```

<br>

기본 인자 값을 사용할 때, 주의해야 할 점이 있습니다.
**파라미터 가장 끝 부분 영역에서만 기본 인자 값을 설정할 수 있습니다. 첫 파라미터나 중간 파라미터에서는 설정할 수 없습니다.**

아래 예시를 보면 명확하게 이해할 수 있을 것입니다.  
첫 파라미터에 기본 인자 값을 설정해 테스트 해보겠습니다.  
```python
def say_hello(greeting='hello', name):
    return greeting + name

print(say_hello(None, 'koanko'))
#     def say_hello(greeting='hello', name):
#                                     ^
# SyntaxError: non-default argument follows default argument
```

<br>
<br>

## 키워드 인자(Keyword Arguments)
앞서 보았듯, 함수를 정의할 때 파라미터 자리에 `=` 을 사용해 기본값을 정의하는 것은 '기본 인자 값' 설정입니다.  
**키워드 인자는 함수를 정의할 때가 아니라, 호출할 때 인자에 `=` 을 사용하는 것을 말합니다.**  

호출할 때 인자의 순서는 파라미터의 순서와 일치하게 작성해야 하지만, 키워드 인자를 활용하면 파라미터와 인자를 호출단계에 미리 묶어주는 것이기 때문에 인자의 위치를 바꿔도 상관 없이 제대로 호출할 수 있습니다.
```python
def greeting(age, name, major, address):
    return f'이름: {name}, 나이: {age}세, 전공: {major}, 주소: {address}'

print(greeting(200, address='서울', major='cs', name='koanko'))
# '이름: koanko, 나이: 200세, 전공: cs, 주소: 서울'
```

<br>

기본적으로 사용하는, 순서대로 파라미터에 들어가는 인자를 위치인자 라고 합니다.  
**키워드 인자를 사용하며 주의해야 할 것은, 원칙적으로 키워드 인자 뒤에 위치인자를 사용할 수는 없다는 것입니다.**
```python
print(greeting(address='서울', major='cs', name='koanko', 20))
#     greeting(address='서울', major='cs', name='koanko', 20)
#                                                         ^
# SyntaxError: positional argument follows keyword argument
```

<br>

파이썬 표준 라이브러리의 내장함수 중 하나인 `print()` 함수는 사실 내부적으로 이렇게 생겼습니다.
![](img/print().png)
`print()` 함수에 여러 인자들을 넣으면 사이에 공백 한 칸씩 띄고 출력되는 것과, 줄 단위로 출력이 되는 것은 사실 '키워드 인자'가 이렇게 설정되어 있었기 때문입니다.  
따라서 우리는 `sep`, `end` 를 활용해 출력되는 형태를 직접 바꿀 수 있습니다.  
마법이 일어난 것이 아니라, 내부가 이런 원리로 작동하기 때문에 가능하다는 것을 키워드 인자를 통해 이해할 수 있습니다.  

<br>
<br>

## 가변(임의) 인자 리스트(Arbitrary Argument Lists)
가변 인자 리스트를 사용하면, 개수가 정해지지 않은 여러 개의 인자를 함수에서 개수에 관계없이 무한으로 받을 수 있습니다.  
함수를 정의할 때, 파라미터 앞에 애스터리스크(*) 마크를 붙이면 됩니다.  
호출할 때 여러 개수의 인자를 넣으면, 이 값들은 가변 인자 리스트에 튜플로 묶여서 들어가게 됩니다.  

어떤 타입의 인자들이 들어올지 함수 입장에서는 모르기 때문에, 파라미터 자리에 `*args`라고 적는 것이 컨벤션입니다.

```python
def search_max(*args):
    max_arg = args[0]
    
    for arg in args:
        if arg > max_arg:
            max_arg = arg
    
    return max_arg

print(search_max(1, 2, 3, 4, 5, 6, 7))
# 7
```

<br>
<br>

## 가변(임의) 키워드 인자(Arbitrary Keyword Arguments)
가변 키워드 인자는 정해지지 않은 키워드 인자들은 함수를 정의할 때 사용합니다.  
가변 키워드 인자는 dict 형태로 처리됩니다. 사용할 땐, 매개변수에 `**`로 표현하면 됩니다.
```python
def kwargs_func(a, b=1, *args, **kwargs):
    print(a, b, args, kwargs)
    
kwargs_func(1, 2, True, False, 'a', x=1, y=2, z=3)
# 1 2 (True, False, 'a') {'x': 1, 'y': 2, 'z': 3}
```

<br>