# React Hooks - useRef

## useRef()
`useRef()`를 사용하면 DOM 노드의 값을 저장할 수 있습니다.  
```javascript
import { useRef } from 'react';

const AddUser = (props) => {
    const nameInputRef = useRef();
    const ageInputRef = useRef();

    return (
        <input type='text' value={} onChange={} ref={nameInputRef}/>
        <input type='number' value={} onChange={} ref={ageInputRef}/>
    )
}
```
`useRef()`을 사용하는 이점은 무엇일까요?  

console.log로 useRef를 살펴보면, useRef는 항상 객체를 출력합니다.  
이 객체는 현재 속성을 가지고 있습니다. useRef에 저장되는 건 실제 DOM 노드입니다.  
이제 DOM 노드를 조작해 온갖 작업을 할 수도 있지만, 일반적으로는 그렇게 하지 않는 것이 좋습니다. DOM은 오로지 리액트로만 다루는 것이 좋습니다.  
하지만 데이터를 읽는 것은 상관 없습니다. 아무것도 바꾸지 않고 그냥 데이터만 읽는 것이기 때문입니다.  

`useRef()`는 요소에 저장되어있는 값에 state 없이 접근할 수 있기 때문에 비교적 간단합니다.  
state를 삭제하고 change handler을 사용하지 않아도 됩니다.  
값을 읽는 것은 ref에 의존하기 때문에 이대로 사용할 수도 있습니다.  

하지만 input 값을 초기화하는 로직을 원한다면 사용할 수 없게 됩니다.  
물론 직접 DOM을 조작해서 값을 초기화할 수도 있지만, 직접 조작하는 것을 좋아하지 않을 수도 있습니다.  
만약 값만 읽기를 원하고 어떤 것도 바꾸지 않을 계획이라면 state를 필요로 할 필요가 없습니다.  
불필요한 코드와 작업이 많기 때문입니다. 그냥 빨리 읽기만 원한다면 그럴 땐, `useRef()`가 나은 선택일 수 있습니다. 요소에 접근하게 하는 건 상당히 편리하기 때문입니다.  

