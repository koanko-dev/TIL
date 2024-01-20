# 사용자 Input에 대한 Two-Way-Binding

인풋의 값은 state의 값을 가리키고, 그 값이 변경될 때마다 state 값을 업데이트하고 싶다면 Two-Way-Binding을 이용할 수 있습니다.

아래는 Two-Way-Binding을 이용한 코드입니다.  
코드를 보면 input이 변경될 때마다(onChange) `handleChange`가 실행됩니다.

```javascript
export default function UserInput() {
    const [playerName, setPlayerName] = useState(initialName);

    function handleChange(event) {
        setPlayerName(event.target.value);
    }

    return (
        <input type="text" required value={playerName} onChange={handleChange} />
    )
}
```

onChange는 모든 키 입력에 트리거되며, 입력한 값을 포함해서 이벤트 객체를 제공합니다. 그래서 `handleChange` 함수에서 `event`를 받아서 사용할 수 있는 겁니다.  

`event` 객체는 이벤트가 일어난 요소를 참조하는 `target` 속성을 가집니다. 그 요소에 입력된 값이 필요하니까 `event.target.value` 값을 사용해서 state를 업데이트 할 수 있습니다.

이렇게 input에서 값을 얻어서 input 값을 다시 넣는 방식을 Two-Way-Binding이라 합니다.

<br/>