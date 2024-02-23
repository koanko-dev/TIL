# Discriminated Unions

Discriminated Union으로 union 타입과 작업할 때, 타입가드를 더 쉽게 구현할 수 있습니다.

차별화된 union으로 개선이 왜 필요한지에 대해 먼저 보겠습니다.

```typescript
interface Bird {
  flyingSpeed: number;
}

interface Horse {
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  // 타입가드 작성
  if ("flyingSpeed" in animal) {  // error 가능성이 있음
	console.log("Moving with speed: " + animal.flyingSpeed);
  }
}
```

이렇게 코드를 쓴다면, 타입가드를 작성할 때 글자가 틀릴 위험이 있습니다. 자동 완성이 되지 않기에 철자를 조금 다르게 쓸 가능성이 있으니까요.

인터페이스는 컴파일되지 않기 때문에 `instanceof`를 사용할 수는 없습니다.  
이럴 때는 인터페이스에 공통된 속성을 넣어줌으로서 해결할 수 있습니다.

```typescript
interface Bird {
  // 공통된 속성 type 추가
  type: "bird";
  flyingSpeed: number;
}

interface Horse {
  // 공통된 속성 type 추가
  type: "horse";
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  let speed;
  // 타입가드 작성
  switch (animal.type) {
	// 자동완성 됨
    case "bird":
      speed = animal.flyingSpeed;
      break;
	// 자동완성 됨
    case "horse":
      speed = animal.runningSpeed;
      break;
  }

  console.log("Moving at speed: " + speed);
}

moveAnimal({ type: "bird", flyingSpeed: 10 });
// Moving at speed: 10
```

공통된 속성은 보통 `type`이나 `kind`를 사용합니다.  
타입가드는 `switch case` 문을 사용하는데, 이전과 달리 자동완성이 되어 오타의 위험이 줄어들었습니다.

이런 방식으로 union을 사용하는 것을 Discriminated Union을 사용한다고 합니다.

<br/>