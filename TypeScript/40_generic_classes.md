# 제네릭 클래스

함수 뿐만 아니라 클래스에서도 제네릭 타입을 더할 수 있습니다.

```typescript
class DataStorage<T> {
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

// 문자열을 저장하는 스토리지 생성
const textStorage = new DataStorage<string>();
textStorage.addItem("Anko");
textStorage.addItem("Coucou");
textStorage.removeItem("Anko");
console.log(textStorage.getItems());
// ['Coucou']

// 객체를 저장하는 스토리지 생성
const objectStorage = new DataStorage<object>();
const ankoObject = { name: "Anko" };
const coucouObject = { name: "Coucou" };
objectStorage.addItem(ankoObject);
objectStorage.addItem(coucouObject);
objectStorage.removeItem(ankoObject);
console.log(objectStorage.getItems());
// [{name: 'Coucou'}]
```

`DataStorage`가 특정 타입만 받길 원한다면 `extends`로 제한을 줘도 됩니다.

예를 들어 string과 object만 받는 `DataStorage`를 원한다면, 아래와 같이 작성하면 됩니다.

```typescript
class DataStorage<T extends string | object> {
	// ...
}
```

<br/>