# Type Aliases로 커스텀 타입 만들기

매번 특정한 타입을 반복해서 쓰는 건 귀찮을 수 있습니다.  
그럴 땐, 타입 키워드를 사용해서 Type Aliases(타입 별칭)을 만들 수 있습니다.
```typescript
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: "as-number" | "as-string"
) {
	// ...
}
```

이 코드를 Type Aliase를 사용해 다시 작성해보겠습니다.
```typescript
type Combinable = number | string;
type ConversionDescriptor = "as-number" | "as-string";

function combine(
  input1: Combinable,
  input2: Combinable,
  resultConversion: ConversionDescriptor
) {
	// ...
}
```

Type Aliase를 사용하면 오타를 줄일 수 있고 코드도 빨리 쓸 수 있는 이점이 있습니다. 의도도 명확하게 할 수 있고요.

참고로 Type Aliase를 만들 때 쓰는 맨 앞의 `type`은 자바스크립트에서 허용되지 않습니다. 타입스크립트가 제공해주는 겁니다.  

Type Aliases는 union 타입만을 저장하는데 그치지 않습니다. 내 마음대로 원하는 타입을 만드는 데 쓸 수도 있습니다.  
객체타입도 가능합니다. 예를 들어 아래와 같은 코드가 있다면,
```typescript
function greet(user: { name: string; age: number }) {
  console.log(user.name);
}
 
function isOlder(user: { name: string; age: number }, checkAge: number) {
  return checkAge > user.age;
}
```

아래와 같이 변경할 수 있습니다.
```typescript
type User = { name: string; age: number };
 
function greet(user: User) {
  console.log(user.name);
}
 
function isOlder(user: User, checkAge: number) {
  return checkAge > user.age;
}
```
이렇게 만든 타입들을 커스텀 타입 이라고 합니다.  
Type Aliases을 이용하면, 즉 커스텀 타입을 사용하면 불필요한 반복을 없앨 수 있고, 관리하기에 좋습니다.

<br/>