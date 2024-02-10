# 인터페이스 readonly 속성

인터페이스 안에서는 private나 public은 사용할 수 없지만, `readonly`는 추가할 수 있습니다.

이 `Greetable` 인터페이스를 기반으로 하는 클래스로 구축된 객체는, `name` 속성이 `readonly`로 초기화 됩니다.

```typescript
interface Greetable {
  // readonly 추가
  readonly name: string;
  greet(phrase: string): void;
}

class Person implements Greetable {
  // ...
}

user1 = new Person("Anko");
// readonly 속성을 변경하려고 하니
user1.name = 'Coucou'; // error
```

인터페이스 뿐만 아니라 `type`에서도 `readonly`를 사용할 수 있습니다.

```typescript
type Greetable = {
  // readonly 추가
  readonly name: string;
  greet(phrase: string): void;
}
```

<br/>