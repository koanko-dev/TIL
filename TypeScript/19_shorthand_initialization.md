# 초기화 짧게 줄이기

초기화를 줄이는 방법은 이렇습니다.

원래 아래와 같이 초기화 했다면,
```typescript
class Department {
  private id: string;
  private name: string;

  constructor(id: string, n: string) {
    this.id = id;
    this.name = n;
  }
}
```

아래와 같이 단축할 수 있습니다.

```typescript
class Department {
  constructor(private id: string, private name: string) {

  }
}
```

원래는 먼저 정의를 하고 constructor 내부에서 값을 저장했어야 했는데, 받아오면서 한번에 할 수 있게 됐습니다.

주의할 점은, 속성의 이름은 인수와 동일하게 생성된다는 것입니다. 따라서 인수의 이름을 정할 때 염두해두어야 합니다.

<br/>