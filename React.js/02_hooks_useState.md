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
여러 `useState()`를 사용해 state를 관리하는 것의 대안으로, 다음과 같이 다중 state를 관리할 수 있습니다.
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
