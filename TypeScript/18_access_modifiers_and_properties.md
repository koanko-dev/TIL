# private, public, readonly, static

타입스크립트 클래스에서 사용할 수 잇는 접근제어자와 속성은 아래와 같이 나뉩니다.  
여기에서는 protected를 제외하고 살펴봅니다. protected는 상속과 관련이 있어, 상속을 다룬 뒤에 함께 다루겠습니다.

**접근 제어자 (Access Modifier)**
- `private` (타입스크립트 지원)
- `public` (타입스크립트 지원)
- `protected` (타입스크립트 지원)

<br/>

**속성 (Properties)**
- `readonly` (타입스크립트 지원)
- `static` (모던자바스크립트 지원)

<br/>

## private
`private`는 클래스 안에서만 접근할 수 있는 속성입니다.

```typescript
class Department {
  name: string;
  employees: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log("Department: " + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}
```

위의 클래스에서 `employee`를 추가할 때 `addEmployee` 메서드를 사용합니다.

하지만 `employees` 배열 자체에 바로 추가하려고 하는 사람도 있을 수 있습니다. 아래 코드처럼요.

```typescript
const accounting = new Department("Accounting");

accounting.addEmployee("Anko");
accounting.addEmployee("Coucou");
// addEmployee 메서드 사용하지 않고, 바로 추가
accounting.employees[2] = "Someone";

accounting.describe();
accounting.printEmployeeInformation();
// 3
// ['Anko', 'Coucou', 'Someone']
```

이를 방지하고 `addEmployee` 메서드로만 추가하게 하려면, `employees` 속성에 이렇게 `private` 키워드를 추가하면 됩니다.

```typescript
class Department {
  name: string;
  // private 키워드 추가
  private employees: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log("Department: " + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

const accounting = new Department("Accounting");

accounting.addEmployee("Anko");
accounting.addEmployee("Coucou");
accounting.employees[2] = "Someone"; // error
```

즉, **클래스 안에서만 접근 가능하게 하고 싶은 속성 앞에 `private` 키워드를 붙이면 됩니다.**

<br/>

## public
위에 나와있는 코드에서 `name` 속성은 `public` 속성입니다.  
**`private`를 붙이지 않은 것들은 모두 `public`이고, 이 키워드를 붙이든 안 붙이든 모두 `public` 속성입니다.**

`public` 속성은 클래스 외부에서 값에 접근은 물론, 변경도 가능합니다.

<br/>

## readonly
`readonly` 속성은 말 그대로 읽기만 가능하고 변경은 불가능한 속성을 지정합니다.  
이 속성을 변경하려고 하면 에러가 날 겁니다.

```typescript
class Department {
	// readonly 사용
  private readonly name: string;
  private employees: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log("Department: " + this.name);
		this.name = "Something"; // error
  }
}
```

<br/>

## static

`static`은 위에 소개한 것들과 다르게, 모던 자바스크립트에도 있는 기능입니다.  
`static`은 속성과 메서드에 쓸 수 있는데, **`static`을 쓰면 클래스를 new 키워드로 생성하거나 하지 않아도 바로 속성이나 메서드에 접근할 수 있도록 합니다.**

`static`은 전역으로 사용하고 싶은 속성이나 메서드에 많이 쓰입니다.  
예로, `Math` 내부에 있는 메서드 `pow`는 객체 생성 없이 바로 실행 가능합니다.
```typescript
Math.pow()
// new Math()로 Math 객체를 생성하지 않아도 사용 가능함을 볼 수 있다!
```

클래스에서의 예시를 들어보겠습니다.

`static`을 `fiscalYear` 속성과 `createEmployee` 메서드에게 주었습니다.  
이렇게 하면 new 키워드로 `Department` 객체를 생성하지 않아도 외부에서 사용 가능하게 됩니다.
```typescript
class Department {
	// static 속성
  static fiscalYear = 2024;
  protected employees: string[] = [];

  constructor(private id: string, private readonly name: string) {}

	// static 메서드
  static createEmployee(name: string) {
    return { name: name };
  }

	// ...
}

// static 속성, 메서드에 접근
const employee = Department.createEmployee("Anko");
console.log(employee, Department.fiscalYear);
// {name: "Anko"} 2024
```

**주의해야 할 점은, `static` 속성은 클래스 내부의 `static` 부분이 아닌 곳에서 사용할 수 없습니다!**

`static` 부분이 아닌 곳인 `constructor`에서 사용해보겠습니다.

```typescript
class Department {
  static fiscalYear = 2024;
  protected employees: string[] = [];

  constructor(private id: string, private readonly name: string) {
		// static 아닌 곳에서 static 속성을 사용
    console.log(this.fiscalYear) // error
  }

  static createEmployee(name: string) {
    return { name: name };
  }

	// ...
}
```

`static` 속성과 메서드는 인스턴스와 분리되는 원리를 가지고 있습니다. 따라서 `this` 키워드를 사용할 수 없기 때문에 당연하게 액세스 불가능 합니다.

에러메세지는, `fiscalYear` 속성은 `Department` 타입에 존재하지 않는다고 뜹니다.

`static` 속성이나 메서드를 클래스 내에서 사용하고 싶다면, 클래스 이름을 써서 접근할 수 있습니다.

```typescript
class Department {
  static fiscalYear = 2024;
  protected employees: string[] = [];

  constructor(private id: string, private readonly name: string) {
		// static 아닌 곳에서 static 속성을 사용
    console.log(Department.fiscalYear)
  }

  static createEmployee(name: string) {
    return { name: name };
  }

	// ...
}
```

<br/>