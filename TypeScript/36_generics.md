# Generics

제네릭 타입은 다른 타입과 연결된 타입으로, 매우 유연합니다.

```typescript
const names: Array<string> = []; // string[]
```

위 코드의 `Array<string>` 부분이 제네릭 타입입니다. 배열 타입 안에 문자열 타입이 들어가 있다는 의미입니다.  
이렇게 타입을 작성하는 것은 `string[]` 으로 타입을 지정했을 때와 같은 역할을 합니다.

배열 제네릭 타입 말고도 다른 제네릭 타입도 있습니다.  
또 다른 제네릭 타입은 Promise 타입 입니다.

```typescript
// Promise<string> 제네릭 타입 지정
const promise: Promise<string> = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("This is done!");
  }, 2000);
});

promise.then((data) => {
  // split() 메서드 자동완성 가능
  data.split(" ");
});
```

`Promise<string>` 으로 제네릭 타입을 지정해줬기 때문에, 타입스크립트는 이 `promise`가 문자열을 생성할 것이라고 알 수 있게 됩니다.  
그래서 이 데이터를 사용할 때 `split()` 같은 메서드의 자동완성 기능을 제공해줍니다.

만약 `Promise<number>` 로 제네릭 타입을 지정했다면 어떻게 됐을까요?  
`data.split(” “)` 부분에 오류가 뜨게 됩니다.

제네릭타입으로 작업을 하면 타입스크립트의 더 나은 지원을 받을 수 있습니다.

<br/>