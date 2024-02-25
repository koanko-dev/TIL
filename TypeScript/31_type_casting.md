# Type Casting

타입 캐스팅은 타입스크립트에게 감지할 수 없는 특정 타입을 알려주는 것입니다.

좋은 예로는 DOM 에서 무언가 얻는 경우에 볼 수 있습니다.

```typescript
const userInputElement = document.getElementById("user-input");

userInputElement.value = 'Hi there!'; // error
```

이렇게 코드를 쓰면 2가지 오류가 발생합니다.

1. null 값일 수도 있다
2. value 값이 없다

이 두 오류를 없애보겠습니다.

## 1. null 값 안나오도록 만들기

null 값이 아니라고 말하는 것은 맨 뒤에 느낌표를 붙여주면 됩니다.

```typescript
const userInputElement = document.getElementById("user-input")!;
```
`!` 를 붙이면 “이게 null이 아니라고 내가 보장해” 라고 타입스크립트에게 말하는 거니까요.

혹은 이 방식을 쓰지 않고 `if` 문으로 처리할 수도 있습니다.
```typescript
const userInputElement = document.getElementById("user-input");

if (userInputElement) {
  // as 키워드 사용하는 부분은 아래 타입 캐스팅에서 다룹니다.
  (userInputElement as HTMLInputElement).value = "Hi there!";
}
```

`if` 문에서 먼저 null 값인지 확인하면 null이 아닐 때만 내부의 코드가 돌아갈 겁니다.

## 2. 타입 캐스팅

null 값이 아니라고 말하는 것 외에도, 어떤 엘리먼트라고 알려줘야 에러가 해결됩니다. 이게 타입 캐스팅으로 할 수 있는 일입니다.

타입 캐스팅에는 2가지 구문이 있습니다.

### 첫번째 타입 캐스팅 구문
```typescript
const userInputElement = <HTMLInputElement>document.getElementById("user-input")!;

userInputElement.value = 'Hi there!';
```
앞에 태그를 붙이고, 태그 안에 어떤 HTML 요소인지 써주는 겁니다.

이게 가능한 이유는 tsconfig.json 설정 파일 안에, compilerOptions > lib > dom을 작성했기 때문에 가능한 겁니다. 이 설정으로 DOM에 전역으로 접근할 수 있게 되었으니까요.

### 두번째 타입 캐스팅 구문
```typescript
const userInputElement = document.getElementById("user-input")! as HTMLInputElement;

userInputElement.value = 'Hi there!';
```
맨 뒤에 `as` 키워드를 쓰고 그 뒤에 어떤 HTML 요소인지 써주는 겁니다.

똑같이 아무 에러 없이 잘 작동됩니다.

<br/>