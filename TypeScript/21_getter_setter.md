# Getters & Setters

## Getter
`private` 속성은 바깥에서 접근할 수 없습니다. 그런 값을 접근할 수 있도록 도와주는 것이 `getter` 속성입니다.

`getter`는 `private` 속성값을 겁색할 때 함수나 메서드를 실행하는 속성입니다.

```typescript
class AccountingDepartment extends Department {
  private lastReport: string;

  // getter 메서드 추가
  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error("No report found.");
  }

  constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
    this.lastReport = reports[0];
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }
}

const accounting = new AccountingDepartment("d1", []);
accounting.addReport("Something went wrong...");

// getter 메서드 사용
console.log(accounting.mostRecentReport);
// "Something went wrong..."
```

`getter` 메서드는 무언가를 꼭 반환해야 합니다. 여기에서는 `lastReport` 값입니다.

`getter` 메서드를 사용하고 싶다면, 실행시키지 말고 속성에 접근하듯 하면 됩니다.

<br/>

## Setter
`setter` 메서드는 바깥에서 속성 값을 조작하도록 하는 메서드입니다.

```typescript
class AccountingDepartment extends Department {
  private lastReport: string;

  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error("No report found.");
  }

  // setter 메서드 추가
  set mostRecentReport(value: string) {
    if (!value) {
      throw new Error("Please pass in a valid value!");
    }
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
    this.lastReport = reports[0];
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }

  printReport() {
    console.log(this.reports);
  }
}

const accounting = new AccountingDepartment("d1", []);

// setter 메서드 사용
accounting.mostRecentReport = "Year End Report";
accounting.printReport();
// ['Year End Report']
```

`getter` 메서드와 마찬가지로 실행하지 않고 속성에 접근하든 해야 합니다.  
할당을 사용해서 값을 넣어줄 수 있습니다.

<br/>