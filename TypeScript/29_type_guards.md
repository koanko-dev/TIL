# 타입 가드(Type Guards)

타입 가드는 union 타입에 도움을 줍니다.

union 타입은 유연하지만, 런타임에 어떤 유형이 필요한지 알아야 하기 때문에 타입 가드를 사용합니다.

### type 사용시 타입가드

```typescript
// union 타입 생성
type Combinable = string | number;

function add(a: Combinable, b: Combinable) {
  // 바로 아래 라인이 typeof를 사용한 '타입가드' 입니다.
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString();
  }
  return a + b;
}
```
`if` 문에 `typeof`를 사용하여 타입가드를 만들었습니다.  

다른 타입 가드 예시를 들어보겠습니다.  
union 타입에 커스텀타입 객체가 들어가는 경우입니다.
```typescript
type Admin = {
  name: string;
  privileges: string[];
};

type Employee = {
  name: string;
  startDate: Date;
};

// union 타입 생성
type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee) {
  console.log("Name: " + emp.name);
  // 타입가드 필요
  // if (emp.privileges) {       => error
  // if (typeof emp === Admin) { => error
  if ("privileges" in emp) { // 자바스크립트의 in 키워드를 사용하면 가능
    console.log("Privileges: " + emp.privileges);
  }
	if ("startDate" in emp) {
    console.log("StartDate: " + emp.startDate);
  }
}
```
자바스크립트 `in` 키워드를 사용해 타입가드를 만들었습니다.

이제 타입 가드가 잘 작동하는지 확인해보겠습니다.
```typescript
type ElevatedEmployee = Admin & Employee;

const e1: ElevatedEmployee = {
  name: "Anko",
  privileges: ["create-server"],
  startDate: new Date(),
};

printEmployeeInformation(e1);
// 두개의 콘솔로그가 모두 출력됨
```

### class 사용시 타입가드

클래스로 작업할 때는 `instanceof` 라는 타입가드를 사용할 수 있습니다.  
먼저 사용하지 않은 예시입니다.
```typescript
class Car {
  drive() {
    console.log("Driving a car...");
  }
}

class Truck {
  drive() {
    console.log("Driving a truck...");
  }

  loadCargo(amount: number) {
    console.log("Loading cargo..." + amount);
  }
}

// union 타입 생성
type Vehicle = Car | Truck;

const v1 = new Car();
const v2 = new Truck();

function useVehicle(vehicle: Vehicle) {
  vehicle.drive();
  // in 키워드로 확인
  if ("loadCargo" in vehicle) {
    vehicle.loadCargo(1000);
  }
}

useVehicle(v1);
// Driving a car...
useVehicle(v2);
// Driving a truck...
// Loading cargo...1000
```

클래스에서도 마찬가지로 `in` 키워드를 사용해 클래스 내부에 속성이나 메서드가 있는지 확인합니다.

`in` 키워드로 타입가드가 작동은 하지만, 여기에서 위험한 부분은 `"loadCargo"`를 타이핑 실수 할 수 있다는 점입니다.

`instanceof`를 써서 방지하겠습니다.

```typescript
// ...

function useVehicle(vehicle: Vehicle) {
  vehicle.drive();
  // instanceof 사용
  if (vehicle instanceof Truck) {
    vehicle.loadCargo(1000);
  }
}

// ...
```

`instanceof`는 바닐라 자바스크립트 기능입니다. `typeof` 와 마찬가지로 런타임에 실행됩니다.

클래스는 자바스크립트가 클래스 및 생성자 함수를 지원하기 때문에 이렇게 할 수 있지만, 클래스 대신 인터페이스를 사용한다면 당연히 이렇게 구현할 수는 없습니다.  
인터페이스는 자바스크립트 코드로 컴파일되지 않기 때문에 런타임에는 인스턴스를 사용할 수 없어서 그렇습니다. 따라서 타입 가드에서 사용할 수 없습니다.

<br/>