# Interface

인터페이스는 보통 타입 체크를 하기 위해서 사용됩니다.  
`Person` 인터페이스를 생성해서 변수에 적용해보겠습니다. 

```typescript
interface Person {
  name: string;
  age: number;
  greet(phrase: string): void;
}

let user1: Person;

user1 = {
  name: 'Anko',
  age: 300,
  greet(phrase: string) {
      console.log(phrase + ' ' + this.name)
  }
}

user1.greet('Hi there I am')
// Hi there I am Anko
```

## 인터페이스를 클래스와 함께 사용하기

인터페이스 부분을 이렇게 커스텀 타입으로 바꿔도 똑같이 동작하는데, 왜 인터페이스가 필요할까요?

```typescript
type Person = {
  name: string;
  age: number;
  greet(phrase: string): void;
}
```

인터페이스나 타입은 둘 다 객체의 구조를 설명하지만 확장하는 부분에서나 사용하는 부분에서나 약간의 차이가 있습니다.

`interface`를 클래스에서 더 자주 사용하는 이유는, 클래스가 준수해야 하는 부분을 설정하는 용도로서 사용할 수 있기 때문입니다.

클래스에서의 사용을 보기 전에 객체와 먼저 비교해보겠습니다.  
객체에서 아래 `Greetable` 인터페이스를 사용하면 오류가 납니다. `Greetable` 인터페이스에는 없는 `age` 속성을 가지고 있거든요.

```typescript
interface Greetable {
  name: string;
  greet(phrase: string): void;
}

let user1: Greetable;

user1 = {
  name: 'Anko',
  // interface에는 없는 age 속성 사용
  age: 300, // error
  greet(phrase: string) {
      console.log(phrase + ' ' + this.name)
  }
}
```

`age` 속성이 있는 클래스에서 이 `Greetable` 인터페이스를 사용하면 어떨까요?

```typescript
interface Greetable {
  name: string;
  greet(phrase: string): void;
}

class Person implements Greetable {
  name: string;
  // interface에는 없는 age 속성 사용
  age = 300;

  constructor(n: string) {
    this.name = n;
  }

  greet(phrase: string) {
    console.log(phrase + " " + this.name);
  }
}
```

오류가 나지 않습니다. 클래스에서 인터페이스는 필수로 가져야 하는 속성과 메서드일 뿐, 추가적으로 속성과 메서드를 가지는 것은 아무 문제 없습니다.

**즉, 인터페이스는 클래스들간에 구현과 관련되지 않은 여러 기능을 공유하는 역할을 합니다.**

아까 위에서 오류가 났던 `user1` 변수에, 직접 작성한 오브젝트가 아닌, 클래스로 만든 인스턴스를 넣으면 어떻게 될까요?

```typescript
let user1: Greetable;

// user1 = {
//   name: "Anko",
//   age: 300,  // error
//   greet(phrase: string) {
//     console.log(phrase + " " + this.name);
//   },
// };

user1 = new Person("Anko");
console.log(user1);
// age가 포함된 person 객체가 생성됨
```

직접 오브젝트를 작성했을 땐 오류가 났지만 이제는 오류가 나지 않습니다.

<br/>