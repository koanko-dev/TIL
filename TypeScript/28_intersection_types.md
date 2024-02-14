# Intersection 타입

intersection 타입은 다른 타입과 결합한 타입입니다.  
`&` 연산자를 사용해서 타입을 합칠 수 있습니다.  

객체 타입인 경우, 객체 속성을 모두 조합합니다.

```typescript
type Admin = {
  name: string;
  privileges: string[];
};

type Employee = {
  name: string;
  startDate: Date;
};

type ElevatedEmployee = Admin & Employee;

const e1: ElevatedEmployee = {
  name: "Anko",
  privileges: ["create-server"],
  startDate: new Date(),
};
```

union 타입의 경우, 공통된 타입만 가져옵니다.

```typescript
type Combinable = string | number;
type Numeric = number | boolean;

type Universal = Combinable & Numeric; // type Universal = number
let n: Universal;
n = true; // error
```

number가 유일한 교집합이기 때문에, 타입은 number가 됩니다.

<br/>