# Optional Chaining

서비스를 다루다 보면 서버쪽에서 데이터를 받아올 때가 있습니다.

서버로부터 사용자 정보를 받아와 `fetchedUserData`에 저장했다고 가정해보겠습니다.

그런데 어떤 이유로 기존에 받던 데이터와 조금 다르게 데이터가 전달 되어 에러가 발생할 수 있습니다.

```typescript
const fetchedUserData = {
  id: "u1",
  name: "Anko",
  // job: { title: "CEO", description: "My own company" },
};

console.log(fetchedUserData.job.title); // error
```

자바스크립트에서는 이런 경우 `&&` 연산자로 속성이 있는지 확인하고 불러올 수 있습니다.

```typescript
console.log(fetchedUserData.job && fetchedUserData.job.title);
```

타입스크립트에는 좀 더 나은 방법이 있습니다. Optional Chaining 오퍼레이터를 사용하는 방법입니다.

```typescript
console.log(fetchedUserData?.job?.title);
```

정의되었는지 안 되었는지 잘 모르겠는 것 뒤에 `?` 를 붙이면 됩니다.  
IDE에서 지원이 안 될 수도 있지만 지원되는 구문입니다.

**Optional Chaining 오퍼레이터는 객체 데이터의 중첩된 속성과 중첩된 객체들에 안전하게 접근할 수 있도록 합니다.**

물음표 앞에 있는 것들이 정의되지 않았다면 그 이후에 것들에는 접근하지 않을 겁니다.  
참고로 이 구문은 정의되지 않은게 있는지 확인하고 실행하는 `if` 문 체크로 컴파일될 겁니다.

<br/>