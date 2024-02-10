# 인터페이스 확장하기

인터페이스를 상속받듯 확장해서 사용할 수도 있습니다.  
`extends` 키워드를 사용하면 됩니다.

```typescript
interface Named {
  readonly name: string;
}

interface Greetable extends Named {
  greet(phrase: string): void;
}
```

여러 인터페이스를 사용해 확장하고 싶다면, 컴마로 구분해 써주면 됩니다.

클래스에서는 단 하나의 클래스를 상속받을 수 있지만 인터페이스는 여러개를 상속받을 수 있습니다.

```typescript
interface Greetable extends Named, Something {
  greet(phrase: string): void;
}
```

<br/>