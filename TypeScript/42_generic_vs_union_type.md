# Generic 타입 vs Union 타입

Generic 타입과 Union 타입은 어떻게 다를까요?

Generic 타입으로 만들어진 클래스를 Union 타입으로도 구현할 수 있습니다.

```typescript
// generic 타입
class DataStorage<T extends string | object> {
  private data: T[] = [];

  addItem(item: T) {
    this.data.push(item);
  }

  removeItem(item: T) {
    this.data.splice(this.data.indexOf(item), 1);
  }

  getItems() {
    return [...this.data];
  }
}

const textStorage = new DataStorage<string>();

// union 타입
class DataStorage {
  private data: (string | object)[] = [];

  addItem(item: string | object) {
    this.data.push(item);
  }

  removeItem(item: string | object) {
    this.data.splice(this.data.indexOf(item), 1);
  }

  getItems() {
    return [...this.data];
  }
}

const textStorage = new DataStorage();
```

얼핏 보면 비슷해 보일 수 있지만, 이 둘은 전혀 다릅니다.  
무엇이 다를까요?

### Generic 타입

```typescript
class DataStorage<T extends string | object> {
	private data: T[] = [];
  // ...
}

const textStorage = new DataStorage<string>();
```

Generic 타입은 `DataStorage` 클래스를 생성할 때 선택된 타입에 따라 `data` 배열이 어떤 타입으로 채워질지 정해집니다.  
즉, 인스턴스 생성 후 다른 타입 값을 `data`에 넣으려고 하면 실패합니다.

### union 타입

```typescript
class DataStorage {
	private data: (string | object)[] = [];

	// ...
}
```

Union 타입은 `DataStorage` 클래스의 `data` 배열에 string과 object가 섞여있을 수 있습니다.

섞여 있지 않은 배열을 가지고 싶어서 `private data: string[] | object[] = [];` 이렇게 할 수도 있지만, 그럼 메서드 부분에서 에러가 날 겁니다. 파라미터로 string과 object를 모두 받고 있으니까요.

<br/>

## 정리

Generic 타입은 특정 타입으로 제한하려는 경우에 사용합니다. Generic 타입을 사용하면, 생성한 클래스 인스턴스 전체에서 같은 타입을 사용하도록 합니다. 또한 함수에서도 같은 타입을 사용하도록 합니다.

Union 타입은 모든 메서드와 함수가 실행될 때마다 유연한 다른 타입을 가질 수 있습니다.

<br/>