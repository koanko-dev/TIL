# 타입스크립트의 Class

## Class 란?
타입스크립트의 클래스를 보기에 앞서, 클래스에 대해 간단하게 짚고 넘어갑니다.  

클래스는 **객체를 찍어내는 공장**이라고 생각하면 됩니다.  
객체와 클래스를 비교해보면 이렇습니다.

### Objects
- 코드에서 작업하는 것들
- 클래스의 인스턴스

### Classes
- object의 청사진
- object의 속성이나 메서드에 대해 담고 있음
- 클래스는 여러개의 비슷한 object를 쉽게 만들 수 있음

<br/>

## 첫번째 타입스크립트 클래스 만들기

`Department` 라는 클래스를 만들어보겠습니다.  
`name` 속성을 가지고 있는 객체를 만들기 위해 클래스에 `name` 속성의 타입을 먼저 명시합니다.
```typescript
class Department {
  name: string;
}
```

`constructor` 는 object가 생성될때 기본적으로 실행되는 예약 메서드입니다.  
여기서 `name` 속성의 값을 설정해줍니다.  
```typescript
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }
}
```

클래스를 이용해 객체를 생성하려면, `new` 키워드를 사용해 생성하면 됩니다.  
`constructor` 에서 string을 받고 있으니 생성 시에 string 타입의 값을 넘겨줘야 합니다.
```typescript
const accounting = new Department("Accounting");

console.log(accounting);
// Department {name: 'Accounting'}
```

생성된 `accounting` 인스턴스는 key, value 값으로 `name`, `"Accounting"`을 가지고 있는 것을 볼 수 있습니다.

클래스에 `describe` 메서드도 추가해보겠습니다.  
그리고 두개의 다른 방식으로 `describe` 메서드를 호출해보겠습니다.
```typescript
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }

  describe() {
    console.log('Department: ' + this.name);
  }
}

const accounting = new Department("Accounting");
accounting.describe(); // Department: Accounting

const accountingCopy = { describe: accounting.describe };
accountingCopy.describe(); // Department: undefined
```

클래스로 만든 오브젝트가 아니라, 직접 오브젝트를 만든 `accountingCopy`의 경우 `this.name` 값으로 `undefined`이 출력 됩니다.

`this`를 `Department` 클래스가 아닌, `accountingCopy`로 인식하기에 발생하는 문제입니다.  
`accountingCopy` 안에는 당연히 `name` 요소가 없습니다.

이 부분을 미리 방지하고자 한다면, 메서드에 `this`의 타입을 지정해주면 됩니다.
```typescript
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }

  // this 타입 추가
  describe(this: Department) {
	console.log('Department: ' + this.name);
  }
}

const accounting = new Department("Accounting");

const accountingCopy = { describe: accounting.describe };
accountingCopy.describe(); // error
```
그럼 컴파일 하기도 전에 에러를 잡을 수 있습니다.

<br/>