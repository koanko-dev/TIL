# Python Class (Advanced)

## 상속(Inheritance)
클래스는 상속받아 사용할 수도 있습니다.  
부모 클래스의 모든 속성을 자식 클래스에게 상속하는 것이기 때문에 코드 재사용성이 높아집니다.  

클래스를 정의할 때, 다음과 같이 클래스명 옆 괄호에 상속받을 클래스명을 작성하면 됩니다.  
```python
class Person:
    def __init__(self, name, age, number, email):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        
    def greeting(self):
        print(f'Hello, {self.name}')
      
    
class Student(Person):
    def __init__(self, name, age, number, email, student_id):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        self.student_id = student_id
        
p = Person('master', 300, '0101231234', 'master@gmail.com')
s = Student('anko', 200, '12312312', 'koanko.dev@gmail.com', '190000')

s.greeting()
# Hello, anko

p.greeting()
s.greeting()
# Hello, master
# Hello, anko
```
상속을 받았지만 `__init__`으로 초기화하는 부분에서 부모클래스와 중복된 부분이 많습니다.  
이 부분은 아래에서 `super`로 개선해보겠습니다.

<br>

### `issubclass(sub_class, parent_class)`
`sub_class`가 `parent_class`의 subclass인지 확인할 수 있습니다.  
subclass인 경우 `True`를 반환합니다.
```python
issubclass(bool, int)
# True
issubclass(int, float)
# False
```

<br>

### `isinstance(object, class)`
`object`가 `class`의 인스턴스인지 확인합니다.  
인스턴스인 경우 `True`를 반환합니다.
```python
isinstance(p, Person)
# True
isinstance(s1, Person)
# True
isinstance(p1, Student)
# False
```

<br>
<br>

## `super()`
부모 클래스의 내용을 사용하고자 할 때, `super()`를 사용하면 됩니다.  
```python
class Person:
    def __init__(self, name, age, number, email):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        
    def greeting(self):
        print(f'Hi, {self.name}')
      
    
class Student(Person):
    def __init__(self, name, age, number, email, student_id):
        super().__init__(name, age, number, email)
        self.student_id = student_id
    
        
p = Person('master', 300, '0101231234', 'master@gmail.com')
s = Student('anko', 200, '12312312', 'koanko.dev@gmail.com', '190000')
p.greeting()
s.greeting()
s.student_id
# Hi, master
# Hi, anko
# 190000
```

<br>
<br>

## 다형성(Polymorphism)
Polymorphism 그 자체는 '여러 모양'을 뜻합니다. 여기에서는 동일한 메서드가 클래스에 따라 다르게 행동할 수 있음을 뜻합니다.  
다른 클래스에 속한 객체들이 동일한 메서드에 다른 방식으로 반응한다는 것입니다.  

<br>

### 메서드 오버라이딩(Method Overriding)
부모와 자식 클래스의 메서드 이름이 똑같을 경우, 자식의 메서드 이름으로 덮어씌우는 것을 말합니다. 즉, **자식 클래스에서 부모 클래스의 메서드를 재정의하는 것**입니다.  
이렇게 되면 상위의 부모 클래스의 메서드가 쓰여지는 것이 아니라, 자식의 메서드가 사용됩니다.  
이것을 메서드 오버라이딩이라고 말합니다.  
```python
class Animal:
    def __init__(self, name, greeting_msg):
        self.name = name
        self.greeting_msg = greeting_msg
        
    def talk(self):
        print(f'{self.name} cries {self.greeting_msg}')
        

class Person(Animal):
    def __init__(self, name, email, greeting_msg):
        super().__init__(name, greeting_msg)
        self.email = email
    
    def talk(self):
        print(f'{self.name} says {self.greeting_msg}')
        
coucou = Animal('coucou', 'Meowwww')
anko = Person('anko', 'koanko.dev@gmail.com', 'Hello')

coucou.talk()
anko.talk()
# coucou cries Meowwww
# anko says Hello
```
똑같은 `talk()` 메서드를 호출했음에도, Person 인스턴스는 자신의 클래스에 있는 `talk()` 메서드를 사용했습니다.  
`talk()` 메서드가 오버라이딩 됐기 때문입니다.  

+추가  
파이썬에서는 오버로딩을 볼 수 없습니다.  
오버로딩은 한 클래스 안에 같은 이름의 메서드가 있을 때, 어떤 값이(타입이) 매개변수로 들어올지에 따라 골라 사용할 수 있는 것을 말합니다.  
파이썬에서는 데이터 타입이 없기 때문입니다.  

<br>
<br>

## 캡슐화(Encapsulation)
외부로부터의 직접적인 접근을 차단하는 것을 말합니다.  
다른 언어와 달리 파이썬에서 캡슐화는 언어적으로 존재하지 않습니다. 암묵적으로만 존재합니다.  

접근제어자 종류
- Public Access Modifier
- Protected Access Modifier
- Private Access Modifier

<br>

### Public Member
언더바 없이 시작하는 메서드나 속성들이 Public Member에 해당됩니다.  
어디서든 호출 가능하며, 하위 클래스에서 메서드 오버라이딩을 허용합니다.  
일반적으로 작성되는 메서드와 속성은 대부분 Public Member 입니다.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def talk(self):
        print('hi')

p = Person('anko', 200)
```
인스턴스 `p`는 `name, age, talk()`에 모두 접근할 수 있습니다.

<br>

### Protected Member
언더바 1개(`_`)로 시작하는 메서드나 속성들이 Protected Member에 해당됩니다.  
암묵적인 규칙으로 부모 클래스 내부와 자식 클래스에서만 호출할 수 있습니다.  
하위클래스 오버라이딩도 허용합니다.  

아래 코드에서는 `age`를 Protected Member로 지정했습니다.
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

p = Person('anko', 200)
p.name, p._age
# ('anko', 200)
```
위와 같이 Protected Member인 `_age`에 접근하게 되면, 에러는 발생하지 않지만 암묵적으로 바깥에서 접근하거나 바꾸지 않기로 되어 있습니다.  
**Protected Member는 클래스 코드 안에서만 활용해야 합니다. 상속받는 클래스 안에서도 접근하거나 바꿀 수 있지만, 클래스 밖에서는 하지 말아야 합니다.**  

Protected Member에 접근하고 수정하는 예시는 다음과 같습니다.
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age
        
    # 접근하는 메서드
    def get_age(self):
        return self._age
    
    # 재할당하는 메서드
    def set_age(self, age):
        # 이런식으로 로직을 걸어줄 수 있기 때문에 Protected Member를 사용합니다.
        if age > 0:
            self._age = age

p = Person('anko', 200)
p.get_age()
# 200
p.set_age(201)
p.get_age()
# 201
```

<br>

### Private Member
언더바 2개(`__`)로 시작하는 메서드나 속성들이 Private Member에 해당됩니다.  
해당 클래스 내부에서만 사용 가능하며, 하위 클래스 상속 및 호출이 불가능합니다. 당연히 외부 호출도 불가능합니다.  

Protected Member와 다르게 호출 자체가 불가하며 에러가 발생합니다.  
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.__age = age
        
    def get_age(self):
        return self.__age
    
    def set_age(self, age):
        self.__age_set_msg()
        self.__age = age
        
    def __age_set_msg(self):
        print('age is setted')

p = Person('anko', 200)
p.__age
# 에러 발생. 외부에서는 접근할 수 없습니다.
p.get_age()
# 200
p.set_age(201)
# age is setted
p.get_age()
# 201
```

<br>
<br>

## 다중상속
상속은 한 부모로부터가 아닌, 여러 부모로부터 받을 수 있습니다. 두개 이상의 클래스를 상속받는 경우를 다중상속이라고 합니다.  
상속 받은 모든 클래스의 요소를 사용할 수 있으며, 중복된 속성 또는 메서드가 있다면 상속 순서에 의해 사용하는 요소가 결정됩니다.  

<br>

### 상속관계에서 이름공간과 MRO(Method Resolution Order)
이름공간을 탐색하는 방식은 아래와 같습니다.  
`인스턴스 -> 클래스 -> 부모클래스`

MRO는 해당 인스턴스의 클래스가 부모 클래스로 어떤 클래스를 가지는지 확인하는 속성 또는 메서드입니다.  
```python
[Class_name].__mro__
# or
[Class_name].mro()
```

상속관계가 다중으로 이루어져 있으면, 상속받는 순서에 따라 우위에 있는 부모가 있습니다. MRO 속성을 이용해 쉽게 확인해 볼 수 있습니다.
```python
class Parent1:
    def greeting(self):
        print('Hiya!')   
        
class Parent2:
    def greeting(self):
        print('Yo!')


class Child1(Parent1, Parent2):
    pass

class Child2(Parent2, Parent1):
    pass


c1 = Child1()
c2 = Child2()

c1.greeting()
c2.greeting()
# Hiya!
# Yo!
```
```python
print(Child1.__mro__)
# (<class '__main__.Child1'>, <class '__main__.Parent1'>, <class '__main__.Parent2'>, <class 'object'>)
# Child1 -> Parent1 -> Parent2 -> object

print(Child2.__mro__)
# (<class '__main__.Child2'>, <class '__main__.Parent2'>, <class '__main__.Parent1'>, <class 'object'>)
# Child1 -> Parent2 -> Parent1 -> object
```

<br>
