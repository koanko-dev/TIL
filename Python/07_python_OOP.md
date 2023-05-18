# OOP

## 객체 지향 프로그래밍(Object-Oriented Programming)
말 그대로 객체(Object)가 중심이 되는 프로그래밍을 뜻합니다.  
프로그래밍에서 파생된 개념이라기보단, 세상에 존재하는 객체의 개념을 프로그래밍으로 가져온 것입니다.  
프로그램을 명령어 줄로 보는 게 아니라 독립된 여러 단위, 즉 객체의 모임과 객체의 상호작용(메서드)로 바라보고자 하는 것입니다.  

<br>
<br>

## 객체(Object)
파이썬에서 모든 것은 객체입니다.  
객체는 type, attribute, method를 가집니다.  

<br>
<br>

## 클래스(Class)
클래스는 공통된 속성(attribute)과 조작법(method)을 가진 객체들의 분류입니다.  
실체가 아닌 추상화된 개념을 뜻합니다. 예를 들어, '사람' 이라는 '말'과 같습니다. 사람 하나 하나는 존재해도, '사람' 이라는 그 '말 자체'는 실체가 없습니다. 분류라고 보면 됩니다.  

<br>
<br>

## 인스턴스(Instance)
인스턴스는 특정 클래스의 실제 데이터 예시(instance)입니다.  
파이썬에서 모든 것은 객체이고, **모든 객체는 특정 클래스의 인스턴스** 입니다.  
1을 예로 들어보겠습니다. 1의 type을 검색해보면 정수(int)가 나옵니다. 사실 1 또한 객체이고 정수 클래스의 인스턴스인 것입니다.  

<br>
<br>

## OOP 기초

### 클래스 만들기
인스턴스를 찍어내는 클래스를 만들어봅시다.  
클래스명은 파스칼케이스(PascalCase)로 써서 정의합니다. 파스칼케이스는 첫글자를 대문자로,lk 다음 단어가 있다면 띄어쓰기를 하지 않고 첫 글자를 대문자로 씁니다.  

```python
# 클래스 정의
class Person:
    pass

# 인스턴스 생성
p = Person()

isinstance(p, Person)
# True

isinstance([], list)
# True

type(p) # type은 사실 클래스를 물어보는 내장함수 입니다.
# __main__.Person

int(1) # 0이 출력되는 이유는 바로 int가 클래스이기 때문입니다. int는 함수명이 아니라 클래스 이름이었습니다.
# 1
```

<br>

### 인스턴스 변수
인스턴스 변수는 인스턴스가 가지는 속성(attribute)입니다.  
아래와 같이 생성할 수도 있고, 아래에서 설명할 생성자 메서드에서 `self.변수명`으로 생성할 수도 있습니다.

```python
class Person:
    pass

me = Person()

me.name = 'anko'
me.age = 200
```

위와 같이 입력하면 `name`, `age` 는 전역 변수에 생성되지 않고 Person 클래스의 인스턴스인 me 안에 변수가 생성됩니다.

<br>

### 인스턴스 메서드
메서드는 클래스의 객체에 공통적으로 적용 가능한 행위(behavior)를 의미합니다.  
인스턴스 메서드는 인스턴스가 사용할 메서드를 말합니다. 생성하는 방법은 클래스 내부에 함수를 만들 듯 `def` 선언으로 작성하면 됩니다.  
클래스 내부에 정의되는 메서드는 기본적으로 인스턴스 메서드로 생성됩니다.(다른 메서드 종류는 아래에서 다룹니다.)  

```python
class Person:
    def greeting():
        print('hi')

me = Person()
me.name = 'anko'
me.greeting()
# TypeError: Person.greeting() takes 0 positional arguments but 1 was given
```
위와 같이 입력하면, `greeting` 에 아무것도 인자를 넣지 않았는데 인자가 하나 있다고 오류가 뜹니다.  

파이썬은 독특하게도 첫번째 인자로 실행한 본인을 넣습니다. 위의 코드의 경우 me(Person의 인스턴스)가 `greeting()` 괄호 사이에 들어간다고 생각하면 됩니다.  

그럼 이렇게 작성해볼 수 있습니다.
```python
class Person:
    def greeting(self): # self라고 쓰는 것이 컨벤션입니다.
        print(f'hi {self.name}')

me = Person()
me.name = 'anko'
me.greeting()
# 'hi anko'
```
위와 같이 파라미터를 하나 설정해야 합니다. 이 본인을 가리키는 파라미터는 메서드 내부에서 활용해 사용할 수 있습니다. 

```python
class Person:
    def greeting(self):
        print(f'hi {self.name}')
        
    def eat(self, food='nothing'):
        print(f'eat {food}')

me = Person()
me.name = 'anko'
me.eat('sushi')
# 'eat sushi'
```

<br>

### `self`
`self` 를 좀 더 자세히 보자면, 아래 코드와 같이 테스트해볼 수 있습니다.
```python
class Person:
    def test(self):
        return self

p1 = Person()
p1.test()
print(p1 is p1.test())
# True
```
인스턴스인 p1과 스스로를 가리키는 `self`가 같음을 확인할 수 있습니다.

<br>

### 생성자(constructor) 메서드
인스턴스가 생성되면서 자동으로 호출되는 메서드입니다.  
기본적으로 인스턴스가 생성되면서 설정되는 메서드 중 하나인 `__init__`을 사용해서 조작할 수 있습니다.  
이 `__init__` 이라는 이름의 메서드가 인스턴스가 생성되면서 실행되도록 묶여있기 때문에, 꼭 `__init__` 이라는 이름으로 정의해야 합니다.  

`dir()`을 통해 `__init__` 에 대해 조금 더 자세하게 이해할 수 있습니다.
```python
class Person:
    pass

p = Person()

dir(p)
# 기본적으로 클래스가 생성되면서 설정되는 메서드들이 나옵니다.
# __init__도 여기서 확인할 수 있습니다.
```

`__init__`의 기본적인 사용 방법은 아래와 같습니다.  
```python
class Person:
    def __init__(self):
        print('created!')

p = Person()
# 'created!'
```

아래와 같이 하면 init을 하면서(인스턴스가 생성되면서) 이름을 설정할 수 있습니다.  
```python
class Person:
    def __init__(self, name_val):
        self.name = name_val

p = Person('anko')
p.name
# 'anko'
```
위 코드에서 한 작업은, **인스턴스의 변수**를 설정한 것입니다.

설정된 인스턴스의 변수를 활용해 코드를 작성할 수도 있습니다.  
```python
class Person:
    def __init__(self, name_val):
        self.name = name_val

    def greeting(self):
        print(f'hi {self.name}')

p = Person('anko')
p.greeting()
# 'hi anko'
```

<br>

### 소멸자(destructor) 메서드
인스턴스가 소멸되기 직전에 자동으로 호출되는 메서드입니다.  
인스턴스는 죽기 직전에 항상 `__del__` 메서드를 실행하고 죽습니다.  

실행되는 경우는 2가지로 볼 수 있습니다.
- 이름 값을 지우거나(`del [instance_name]`)
- 이름값에 다른 값을 할당하거나(re-assignment)

아래의 예시로 소멸자 메서드가 작동하는 방식을 쉽게 이해할 수 있습니다.
```python
class Person:
    def __init__(self, name):
        self.name = name
        print(f'{self.name} init')

    def __del__(self):
        print(f'{self.name} del')

p1 = Person('a')
p1 = Person('b')

# a init => a 생성
# b init => b 생성되면서 a에 연결되어 있던 Person('a') 값과 연결 끊김
# a del => 연결이 끊겼기 때문에 사라짐
```

<br>
<br>

## 매직(스페셜) 메서드
더블언더스코어(`__`)가 앞뒤로 붙어있는 메서드는 특별한 일을 하기 위해 만들어진 메서드라 '매직메서드' 또는 '스페셜메서드'라고 불립니다.  

- `__str__(self)`
- `__len__(self)`
- `__repr__(self)`
- `__lt__(self, other)`
- `__le__(self, other)`
- `__eq__(self, other)`
- `__ne__(self, other)`
- `__gt__(self, other)`
- `__ge__(self, other)`

<br>

### `__str__(self)`
이 설정을 하면 출력할 때 보여줄 내용의 모습을 바꿀 수 있습니다.  

인스턴스를 그대로 출력하면 아래와 같이 보여지는데,
```python
class Person:
    def __init__(self, name):
        self.name = name

p1 = Person('anko')
print(p1)
# <__main__.Person object at 0x10f0c95d0>
```

아래의 코드로 바꾸면 보기 복잡했던 출력값을 지정하여 변경할 수 있게 됩니다.  
```python
class Person:
    def __init__(self, name):
        self.name = name
        
    def __str__(self):
        return f'person object => name: {self.name}'

p1 = Person('anko')
print(p1)
# person object => name: anko
```

모든 객체는 메서드를 활용해 소통합니다. 정수와 정수를 더하는 아래의 코드도 사실 내부적으로는 이렇게 동작합니다.  
```python
# 1 + 2
(1).__add__(2) # 두 숫자가 arg로 들어간다고 생각하면 됩니다.
# 3
```

아래는 원래 사용하던 연산자로 비교하면 에러가 나는 코드입니다.  
매직 메서드를 이용해 비교연산이 가능하도록 다시 정의할 수 있습니다.  
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def __eq__(self, other):
        # 나이가 같으면 true, 다르면 false를 return
        return self.age == other.age
    
    def __gt__(self, other):
        return self.age > other.age
    
    def __lt__(self, other):
        return self.age < other.age
    
    def __add__(self, other):
        return f'{self.name} {other.name}'
    

print(p1 == p3)               # True
print(p1 == p2)               # False
print(p1 > p2, p1.__gt__(p2)) # False False
print(p1 < p2)                # True
print(p1 + p2)                # 'yu kim'
```

❗️ 추가로, `__lt__`나 `__gt__`를 사용하면 주의할 점이 있습니다.  
둘 중 하나만 정의하면(예를 들어 `__lt__`), 작성한 `__lt__`의 반대값으로 `__gt__`가 정의됩니다.  
한 가지를 다시 정의해야 한다면, 둘 다 정의하여 오류를 방지해야 합니다.

<br>
<br>

## 클래스의 클래스, 메타클래스(MetaClass)
클래스도 사실 오브젝트입니다. 클래스는 오브젝트를 만들기 위한 것이지만 클래스 그 자체도 오브젝트입니다.  
모든 객체는 특정 클래스의 인스턴스라고 했습니다. 그렇다면 클래스도 무언가의 인스턴스라는 뜻이 됩니다.  

클래스를 만들어낸 클래스를 메타클래스(MetaClass)라고 합니다. 가장 상위의 클래스는 type 입니다.

정수 클래스를 type에 넣어보면 그 클래스를 확인할 수 있습니다.
```python
type(int)
# type
```
메타클래스인 type이 출력되었습니다.

<br>
<br>

## 다시, 클래스
### 클래스 변수
클래스 변수는 클래스의 속성(attribute) 입니다.  
파생된 모든 인스턴스가 공유하며, 클래스 선언 내부에서 정의합니다. `클래스.변수명` 으로 접근 및 할당 가능합니다.  
```python
class Circle:
    pi = 3.14

Circle.pi
# 3.14

c1 = Circle()
c2 = Circle()

c1.pi, c2.pi
# (3.14, 3.14)

c1.pi = 3.141592
c1.pi, c2.pi
# (3.141592, 3.14)
```
c1은 따로 파이값을 작성했기 때문에, 인스턴스 변수 설정이 됩니다. 클래스에도 pi를 정의했지만 클래스의 pi를 가져오는 것이 아닙니다.  
반면에, c2는 pi 인스턴스 변수가 없기 때문에 부모클래스의 pi 값을 가져옵니다.

<br>

### 클래스 메서드(class method), 스태틱 메서드(static method)
클래스 메서드는 클래스가 사용할 메서드를 말합니다.  
클래스 메서드는 정의할 때 메서드 위에 `@classmethod` 데코레이터를 작성해야 정의됩니다.  
호출 시, 첫번째 인자로 클래스 `cls` 가 전달됩니다.

스태틱 메서드 또한 클래스가 사용할 메서드를 말합니다.  
인스턴스와 클래스의 속성과는 무관한 메서드이며, 메서드 위에 `@staticmethod` 데코레이터를 작성해야 정의됩니다.  
호출 시, 어떤 인자(`cls`,`self`)도 자동으로 전달되지 않습니다.  

```python
class MyClass:
    # 메서드 위에 아무 데코레이터를 작성하지 않으면 instance method 입니다.
    def im(self):
        return self
    
    @classmethod
    def cm(cls):
        return cls
    
    @staticmethod
    def sm():
        return 1

mc = MyClass()
mc.im() == mc
# True

MyClass.cm() == MyClass
# True

MyClass.sm()
# 1
```

<br>

### 인스턴스와 클래스 간의 이름 공간 (namespace)
클래스를 정의하면 클래스가 생성되고 이름 공간이 생성됩니다.  
인스턴스를 만들면 인스턴스 객체가 생성되고 이름 공간이 생성됩니다.  
인스턴스의 특정 속성에 접근하려 하면, 일단 인스턴스에 그 속성이 있는지 확인하고 없다면 인스턴스의 클래스로 올라가 클래스에 그 속성이 있는지 확인합니다.  
```python
class Person:
    name = 'unknown'
    
    def talk(self):
        return self.name

p1 = Person()
p2 = Person()

p1.name
# 'unknown'

p2.name = 'anko'
p1.name, p2.name, Person.name
# ('unknown', 'anko', 'unknown')
```

어떤 순서로 속성을 찾는지 이해하기 좋은 예시가 있습니다.
```python
a = 100

class Sample:
    a = 1

    def func(self):
        b = 2

        return a + b

s = Sample()
s.func()
# 102
```
`a` 를 함수 내부, 즉 로컬 스코프에서 찾을 수 없기 때문에 상위로 올라갑니다.  
상위 함수가 없기 때문에 로컬 스코프의 상위는 글로벌 스코프입니다.  
따라서, `a` 는 `100` 이 됩니다.

이름공간에서는 값을 찾을 때 LEGB Rule을 따라 순서대로 찾았지만, 값 공간에서는 인스턴스, 클래스, 상위클래스 순서로 찾습니다.

아래와 같이 코드가 수정되면 결과값은 `3` 이 나오게 됩니다.
```python
a = 100

class Sample:
    a = 1

    def func(self):
        b = 2

        return self.a + b

s = Sample()
s.func()
# 3
```

<br>
<br>

## 정리
- 인스턴스는 3가지 메서드(인스턴스, 클래스, 정적 메서드) 모두에 접근할 수 있습니다.
- 인스턴스에서 클래스 메서드와 스태틱 메서드는 호출하지 않습니다. (가능하다 != 사용한다)
- 인스턴스가 할 행동은 모두 인스턴스 메서드로 한정 지어서 설계합니다.

- 클래스는 3가지 메서드(인스턴스, 클래스, 정적 메서드) 모두에 접근할 수 있습니다.
- 클래스에서 인스턴스 메서드는 호출하지 않습니다. (가능하다 != 사용한다)
- 클래스가 할 행동은 다음 원칙에 따라 설계합니다. (클래스 메서드와 정적 메서드)
  - 클래스 자체(cls)와 그 속성에 접근할 필요가 있다면 클래스 메서드로 정의합니다.
  
  - 클래스와 클래스 속성에 접근할 필요가 없다면 정적 메서드로 정의합니다.(정적 메서드는 cls, self와 같이 묵시적인 첫번째 인자를 받지 않기 때문)

<br>
<br>

### 추가 노트) docstring 사용
`"""`로 문자열을 감싸는 것은 multiline string 용으로도 사용하지만, 클래스 내부에서도 사용할 수 있습니다.  
클래스를 설명하는 docstring으로 사용함으로서, 코드에디터에서 설명을 볼 때 나오는 문구를 설정할 수 있습니다.
```python
class Person:
    """This is Person class."""
Person.__doc__
# 'This is Person class.'
```

<br>
