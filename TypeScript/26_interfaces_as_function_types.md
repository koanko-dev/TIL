# 함수 타입으로서 인터페이스

인터페이스는 객체의 구조만을 정의할 수 있는 게 아니라, 함수의 구조도 정의할 수 있습니다.  

타입으로 함수 구조를 정의하는 방법을 먼저 복습해보자면 아래와 같습니다.

```typescript
type AddFn = (a: number, b: number) => number;

let add: AddFn;

add = (n1: number, n2: number) => {
  return n1 + n2;
};
```

인터페이스는 객체의 구조를 정의하지만, 자바스크립트 함수는 결국 객체이기 때문에 인터페이스를 쓸 수 있습니다.

인터페이스를 사용하면 이렇게 할 수 있습니다.

```typescript
interface AddFn {
  (a: number, b: number): number;
}

let add: AddFn;

add = (n1: number, n2: number) => {
  return n1 + n2;
};
```

<br/>