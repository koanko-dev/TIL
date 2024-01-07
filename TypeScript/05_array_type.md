# Array 타입

배열의 타입을 만드는 방법은 배열 내부에 무엇을 담고있는지에 따라 크게 두가지로 나뉩니다.

1. 한 타입만을 담고있는 배열 (e.g. `[1, 2, 3]`)
2. 두 타입 이상이 섞여있는 배열 (e.g. `[’HI’, true, 5]`)

<br/>

## 1. 한 타입만을 담고있는 배열
```typescript
let favoriteActivities: string[];
favoriteActivities = ['Sports'];
```
담고 있는 타입을 명시한뒤 `[]`으로 배열 표시를 해주면 됩니다.

<br/>

## 2. 두 타입 이상이 섞여있는 배열
홉합된 배열을 지원하고 싶다면 할 수 있는 방법 중 하나는 any 타입을 사용하는 것입니다.
```typescript
let favoriteActivities: any[];
favoriteActivities = ['Sports', 2];
```
any는 특별한 타입으로, 기본적으로 원하는 대로 하라는 뜻입니다.  
자주 쓰이는 타입은 아닙니다. 모든 타입이 any로 이루어진다면 타입스크립트를 사용하는 의미가 없으니까요.

<br/>