# React Hooks - useState
## useState()
`useState()`는 state를 관리하는 hook 입니다.  
default 값을 할당해주면 `useState()`는 그 값에 대한 포인터와 그 값을 업데이트 할 수 있는 함수를 반환합니다.
한 컴포넌트 안에 여러 state를 사용할 수도 있습니다.
```javascript
const ExpenseForm = () => {
    const [enteredTitle, setEnteredTitle] = useState('');
    const [enteredAmount, setEnteredAmount] = useState('');
    const [enteredDate, setEnteredDate] = useState('');
    
    const titleChangeHandler = (event) => {
        setEnteredTitle(event.target.value);
    };
    const amountChangeHandler = (event) => {
        setEnteredAmount(event.target.value);
    };
    const dateChangeHandler = (event) => {
        setEnteredDate(event.target.value);
    };

    return (
        // ...
        <input type='text' value={enteredTitle} onChange={titleChangeHandler} />
        <input type='number' value={enteredAmount} onChange={amountChangeHandler} />
        <input type='date' value={enteredDate} onChange={dateChangeHandler} />
    )
}
// ...
```
대안으로 다음과 같이 다중 state를 관리할 수 있습니다.
```javascript
const ExpenseForm = () => {
    const [enteredInput, setEnteredInput] = useState({
        enteredTitle: '',
        enteredAmount: '',
        enteredDate: '',
    });
    
    const titleChangeHandler = (event) => {
        setEnteredInput((prevState) => {
            return { ...prevState, enteredTitle: event.target.value }
        });
    };
    const amountChangeHandler = (event) => {
        setEnteredInput((prevState) => {
            return { ...prevState, enteredAmount: event.target.value }
        });
    };
    const dateChangeHandler = (event) => {
        setEnteredInput((prevState) => {
            return { ...prevState, enteredDate: event.target.value }
        });
    };

    return (
        // ...
        <input type='text' value={enteredTitle} onChange={titleChangeHandler} />
        <input type='number' value={enteredAmount} onChange={amountChangeHandler} />
        <input type='date' value={enteredDate} onChange={dateChangeHandler} />
    )
}
// ...
```
state를 통해 관리되는 컴포넌트를 smart component, stateful component 라고 부릅니다.  
이와 반대로 state가 없는 컴포넌트를 state less component, presentational component, dumb component 라고 부릅니다.  
말에서 느껴지는 의미를 생각했을 때 어떤 것이 좋고 나쁘다를 떠올릴 수 있지만, 그런 건 없습니다.  
단지, state를 관리하는 컴포넌트와 그것을 보여주는 컴포넌트의 차이일 뿐입니다.  

## Potals
```javascript
import ReactDOM from 'react-dom';

// ...

const Backdrop = () => {
    return <div onClick={props.onConfirm}/>
};
const ModalOverlay = (props) => {
    return // ...
};

const ErrorModal = (props) => {
    

    return (
        <React.Fragment>
            {ReactDOM.createPortal(<Backdrop onConfirm={props.onConfirm} />, document.getElementById('backdrop-root'))}
            {ReactDOM.createPortal(<ModalOverlay onConfirm={props.onConfirm} />, document.getElementById('overlay-root'))}
        </React.Fragment>
    )
}
```
```html
<!-- ... -->
<body>
    <div id='backdrop-root'></div>
    <div id='overlay-root'></div>
    <div id='root'></div>
</body>
<!-- ... -->
```
## useRef()


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
요소에 저장되어있는 값에 state 없이 접근할 수 있기 때문에 간단합니다.
state를 삭제하고 change handler을 사용하지 않아도 됩니다.
코드가 조금 줄 수 있고 값을 읽는 것은 ref에 의존하기 때문에 이대로 사용할 수도 있습니다.
하지만 input 값을 초기화하는 로직은 사용할 수 없게 됩니다.
물론 직접 DOM을 조작해서 값을 초기화할 수도 있지만, 직접 조작하는 것을 좋아하지 않을 수도 있습니다.
만약 값만 읽기를 원하고 어떤 것도 바꾸지 않을 계획이라면 state를 필요로 할 필요가 없습니다.
불필요한 코드와 작업이 많기 때문입니다. 그냥 빨리 읽기만 원한다면 그럴 땐, Refs가 나은 선택일 수 있습니다. 요소에 접근하게 하는 건 상당히 편리하기 때문입니다.

## useEffect()
```javascript

```
## useEffect()
```javascript

```
## useEffect()
```javascript

```

