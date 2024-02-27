# 인덱스 타입 사용하기

만약 HTML에 여러 `input` 요소를 가지고 있고, 유효성 검사를 한 다음 에러 메세지를 만든다고 생각해보겠습니다.

어떤 에러메세지는 이런 형태를 가질 겁니다.

```typescript
// e.g. 1
{
  email: "Not a valid email!",
  username: "Must start with a capital character!",
}

// e.g. 2
{
  email: "Not a valid email!",
  location: "Not a valid location!",
}
```

key값이 매번 바뀌는 이 경우 타입이나 인터페이스를 어떻게 정의해야 할까요?  
이 때, 대괄호 `[]` 를 이용해 인덱스 타입을 사용하면 됩니다.

```typescript
interface ErrorContainer {
  // index 타입
  [prop: string]: string;
}

const errorBag: ErrorContainer = {
  email: "Not a valid email!",
  username: "Must start with a capital character!",
};
```

위의 코드를 정리해 말하자면, 어떤 속성 이름(key)이 올지는 모르겠지만 그건 string 일거고, 그 값은 string일 것이라 이야기 하는 것입니다.

인덱스 타입 사용 방법을 나열하면 이렇습니다.

1. 먼저 대괄호를 쓰고,
2. 그 안에 아무 이름이나 지정해도 됩니다. 보통 key나 prop을 씁니다.
3. 그리고 그에 대한 타입을 콜론 뒤에 씁니다.
4. 대괄호 밖에 콜론을 입력하고 value로 들어올 값의 타입을 씁니다.

인터페이스 대신 타입을 정의할 때도 똑같이 작동합니다.

```typescript
type ErrorContainer = {
  // index 타입
  [prop: string]: string;
}
```

<br/>