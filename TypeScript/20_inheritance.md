# 클래스의 상속

속성이나 메서드를 물려받는 상속을 하기 위해서는, 아래 코드처럼 `extends` 키워드를 사용할 수 있습니다.

```typescript
class Department {
  private employees: string[] = [];

  constructor(private id: string, private readonly name: string) {}
}

// extends 키워드 사용
class ITDepartment extends Department {
  // empty
}
```

`ITDepartment` 클래스로 생성한 객체는 항상 `name` 속성 값이 ‘IT’이길 원할 땐, `ITDepartment` 클래스 `constructor`에서 `name`값은 고정해두고 `id` 값만 받을 수 있습니다.  
상위 클래스인 `Department`에는 `id`와 `name` 값을 모두 전달해줘야 합니다. `super` 키워드를 사용해 전달하면 됩니다.

```typescript
class Department {
  private employees: string[] = [];

  constructor(private id: string, private readonly name: string) {}
}

// extends 키워드 사용
class ITDepartment extends Department {
  constructor(id: string) {
      // super 키워드 사용
      super(id, 'IT')
  }
}

const it = new ITDepartment("d1");
```

**주의할 점은, `constructor`에서 다른 속성을 할당할 계획이라면, `super`를 호출한 다음에 할당해야 합니다.**

<br/>

## 중복된 속성 작성(Overriding Properties)
동일한 속성을 부모와 자식 클래스가 가지고 있다면, 자식 클래스에서 호출했을 때, 자식 클래스에서 먼저 속성을 찾고 부모 클래스로 올라가는 순서로 진행됩니다.  
한 마디로, 자식클래스의 속성이 불려지게 됩니다. 

```typescript
class AccountingDepartment extends Department {
  constructor(id: string) {
    super(id, "Accounting");
  }

  addEmployee(name: string) {
    if (name === "Anko") {
      return;
    }
		// console.log 출력
    console.log('added!')
    this.employees.push(name);
  }
}

const accounting = new AccountingDepartment("d1");

accounting.addEmployee("Anko"); // no console log
accounting.addEmployee("Coucou"); // 'added!'
```

`Department` 클래스에서 `addEmployee`라는 똑같은 메서드가 있지만, 모두 자식 클래스인 `AccountingDepartment` 클래스에서 불려졌습니다.

<br/>

## "protected" Modifier 

`protected` 속성은 `private`처럼 외부에서 접근할 수 없지만, 클래스와 그 클래스로 확장하는 모든 클래스에서 사용할 수 있습니다.

```typescript
class Department {
  protected employees: string[] = [];

	constructor(private id: string, private readonly name: string) {}

	printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
	}

	// ...
}

class AccountingDepartment extends Department {
  constructor(id: string) {
    super(id, "Accounting");
  }

  addEmployee(name: string) {
    if (name === "Anko") {
      return;
    }
	// 상위 클래스 employees 속성에 추가
    this.employees.push(name);
  }
}

const accounting = new AccountingDepartment("d1");

accounting.addEmployee("Anko");
accounting.addEmployee("Coucou");

accounting.printEmployeeInformation();
// 1
// ['Coucou']
```

<br/>