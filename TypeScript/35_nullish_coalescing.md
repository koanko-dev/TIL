# Nullish Coalescing

Nullish Coalescing 오퍼레이터는 `null` 이거나 `undefined` 값을 가진 데이터 처리를 돕습니다.

사용자의 데이터 입력이 있는데 그게 `null`인지, 정의되지 않았는지, 정확히 알 수 없다고 가정해보겠습니다.  
우리가 원하는 건 `null` 이거나 `undefined`일 때만 `"DEFAULT"`를 출력하게 하는 것입니다.  
그런데 입력 값이 빈 문자열 `""` 이면 어떻게 될까요?

```typescript
const userInput = "";

const storedData = userInput || "DEFAULT";

console.log(storedData);
// DEFAULT
```

`||` 이 연산자로는 불가합니다.

타입스크립트에는 `??` 연산자가 있습니다. 이걸 Nullish Coalescing 오퍼레이터라고 부릅니다.  
`0`도 아니고, 빈 문자열 `""` 도 아닐 때, 그때만 fallback 을 사용합니다.

```typescript
const userInput = "";

const storedData = userInput ?? "DEFAULT";

console.log(storedData);
// ""
```

Nullish Coalescing 오퍼레이터를 사용하고 빈 문자열 `""` 을 넣으니 fallback 값이 나오지 않습니다.

```typescript
const userInput = undefined;

const storedData = userInput ?? "DEFAULT";

console.log(storedData);
// DEFAULT
```

값을 `undefined`으로 변경하고 Nullish Coalescing 오퍼레이터를 사용하니 fallback 값이 나오게 됩니다.

<br/>