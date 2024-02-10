# Abstract 클래스

부모와 자식 모든 클래스가 같은 메서드를 가지도록 강요하고 싶다면 `abstract`를 쓰면 됩니다.  
사용하기 위해서는 몇가지 규칙이 있습니다.

- 같은 메서드를 가지도록 강요하고 싶은 메서드 앞에 `abstract` 키워드를 붙입니다.
- `abstract` 메서드를 가지려면, `abstract` 클래스 내부에서만 가능하므로, 클래스 이름 앞에도 `abstract`을 붙여줘야 합니다.
- `abstract` 메서드는 어떤 코드도 가져서는 안됩니다.
- 반환하는 타입 어노테이션이 필요합니다.

이제 이 클래스를 물려받는 자식 클래스들은 모두 `abstract` 메서드를 가져야 합니다.

```typescript
// 부모 클래스
abstract class Department {
  static fiscalYear = 2024;
  protected employees: string[] = [];

  constructor(protected id: string, private readonly name: string) {
    console.log(Department.fiscalYear);
  }

  // abstract 메서드
  abstract describe(this: Department): void;

  // ...
}

// 자식 클래스
class ITDepartment extends Department {
  constructor(id: string) {
    super(id, "IT");
  }

  // 이 메서드를 작성하지 않으면 오류 발생
  describe() {
    console.log("IT Department - ID: " + this.id);
  }
}
```

<br/>