# Optional 속성과 파라미터

모든 클래스에서 특정한 속성을 가지지 않게 하고 싶다면, 인터페이스에서 속성 이름 뒤에 물음표 `?` 를 붙여주면 됩니다.

```typescript
interface Named {
  readonly name: string;
  outputName?: string;
}

interface Greetable extends Named {
  greet(phrase: string): void;
}

class Person implements Greetable {
  name: string;
  age = 300;

  constructor(n: string) {
		this.name = n;
  }

  greet(phrase: string) {
    console.log(phrase + " " + this.name);
  }
}
```
당연히 속성뿐만 아니라 메서드도 이렇게 가능합니다.  

이런 optional을 나타내는 `?`는 클래스 내에서도 쓸 수 있습니다. optional 파라미터에 사용할 수도 있습니다.  
`Person` 클래스의 `constructor`에서 `n` 값을 얻지 못했을 때를 대비해,

- `n` 뒤에 `?` 를 붙입니다.
- `Person` 클래스 내부의 `name` 속성에 값이 들어가지 않을 수 있으니 `?`를 붙입니다.
- 그럼 자연스럽게 인터페이스의 `name` 속성도 `?`를 붙여줘야겠죠.

이 3가지의 변화에 따라 `if`문도 적절하게 작성하면 아래와 같이 코드가 변경됩니다.

```typescript
interface Named {
  readonly name?: string;
  outputName?: string;
}

interface Greetable extends Named {
  greet(phrase: string): void;
}

class Person implements Greetable {
  name?: string;
  age = 300;

  constructor(n?: string) {
    if (n) {
      this.name = n;
    }
  }

  greet(phrase: string) {
    if (this.name) {
      console.log(phrase + " " + this.name);
    } else {
      console.log("Hi!");
    }
  }
}
```

<br/>