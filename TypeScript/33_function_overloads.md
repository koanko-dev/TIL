# Function Overloads

함수 오버로드는 다중 함수의 특징을 정의할 수 있는 기능입니다.  
이 말은 다른 파라미터로 함수를 호출할 수 있는 다양한 방법이 있다는 것입니다.

숫자를 받으면 숫자끼리 더해 `number`를 반환하고, 문자열을 받으면 문자열을 합쳐 `string`을 반환하는 함수를 작동시키겠습니다.

```typescript
type Combinable = string | number;

function add(a: Combinable, b: Combinable) {
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString();
  }
  return a + b;
}

// 문자열을 전달
const result = add("Developer", " Anko");
result.split(" "); // error
```

우리는 `result`가 `string`으로 나올 것을 확실하게 압니다. 하지만 타입스크립트는 `result`가 `number | string` 이기에 `split()`를 하지 못한다고 오류를 보냅니다.

이 경우는 타입 캐스팅 구문을 사용해 해결할 수 있습니다.
```typescript
const result = add("Developer", " Anko") as string;
result.split(" ");
```
하지만 이 해결법은 최선이 아닙니다. 매번 작성해야 하니까요.  
타입스크립트가 알아서 해결하도록 할 수는 없을까요?

이때 함수 오버로드를 사용하면 좋습니다.

함수 오버로드를 작성하는 방법은, 함수를 정의한 맨 위 부분을 복사하듯 한 줄 더 쓰면 됩니다. 중괄호를 열지 말고요. 특정한 파라미터 타입값과 반환값을 씁니다.

```typescript
// 4개의 funciton overloads 추가
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Combinable, b: Combinable) {
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString();
  }
  return a + b;
}

const result = add("Developer", " Anko"); // (a: string, b: string): string; 를 사용
result.split(" ");
```

참고로 이 부분은 자바스크립트에서 작동하는 구문이 아닙니다. 컴파일 과정에서 제거될 겁니다.

이렇게 미리 어떤 값이 들어 왔을 때, 어떤 값이 반환되는지 미리 지정하는 것이 function overload 입니다.

<br/>