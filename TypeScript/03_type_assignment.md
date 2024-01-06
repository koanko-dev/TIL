# 타입 할당과 추론

타입스크립트에서 타입을 할당할 땐 이렇게 합니다.

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}
```
값 뒤에 `:` 콜론을 적고 그 다음에 타입을 소문자로 작성합니다.

```typescript
const number1 = 5;
const number2 = 2.8;
```
상수의 경우에는 변경될 일이 없으니, 타입과 값 모두 적힌 그대로가 될 겁니다. 그래서 따로 타입을 지정할 필요가 없습니다.  

변수는 어떨까요?  
변수는 값이 다른 값으로 재할당될 수 있으니 타입을 써줘야 할까요?
```typescript
// ?
let number3: number = 10;
```
이건 좋은 방법이 아닙니다. 불필요하게 타입을 지정하고 있는 겁니다. 왜냐하면 **타입스크립트는 값으로부터 타입을 완벽하게 추론할 수 있으니까요.**  
**할당되지 않은 변수를 생성하는 경우**에만 이렇게 해주면 좋습니다.  
```typescript
let number3: number;
number3 = 10;
```

정확히 이해하기 위해서 다른 예를 하나 더 들자면 아래와 같습니다.
```typescript
let someText = 'Hello World!';
someText = 10; // error: type 'number' is not assignable to type 'string'
```
먼저 `someText`는 선언과 동시에 할당되면서 타입이 string으로 정해졌습니다. 그 이후에 number 타입 값을 할당하려고 하니 에러가 난 것입니다.

<br/>