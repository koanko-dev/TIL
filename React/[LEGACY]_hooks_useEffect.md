# React Hooks - useEffect
리액트는 사용자의 입력에 반응하고 필요할 때 UI를 랜더링하는 것이 주된 메인 job입니다.  
뭔가를 화면에 띄워 사용자가 그 무언가와 상호작용 할 수 있도록 하고 특정 이벤트에 따라 바뀔 수 있도록 하는 것입니다.  
이를 위해 리액트는 JSX 코드와 DOM을 평가하고 렌더링 합니다. 유저 인풋을 반영해서 모든 컴포넌트가 적절하게 데이터를 가지고 있도록 state와 props를 관리합니다.  
진짜 DOM을 필요에 따라 조작하기 위해 state와 props가 업데이트됨에 따라 재평가합니다.  

Side Effect는 이런 리액트의 메인 job 외에 일어나는 모든 것들입니다.  
HTTP Request를 보내거나 브라우저 storage에 무언가를 저장하는 경우가 그 예 입니다.  
HTTP Request를 보내는 것은 화면에 무언가를 가져오는 것과는 관련이 없습니다.  
따라서 이런 작업들은 기본적인 컴포넌트 기능 밖에서 이루어져야 합니다. 왜냐하면 예를 들어 HTTP 요청을 하는 함수를 가진 컴포넌트가 있다면, 이 컴포넌트는 재실행 될 때마다 HTTP 요청을 보내게 됩니다.  
예를 들어 HTTP 요청에 응답해 최종적으로 state를 바꾸면 무한루프가 생성될 수도 있습니다. state가 바뀌어 재실행 될 때마다 요청을 보낼 것이기 때문입니다.  

그러므로, Side Effect는 컴포넌트의 함수로 직접 만들지 말아야 합니다.  
대신 이러한 Side Effect를 다루기 위해서는 useEffect hook을 사용하면 됩니다.   

<br>
<br>

## useEffect
useEffect는 두개의 인자를 가집니다.
```javascript
useEffect(() => {}, [dependencies]);
```
첫번째 인자는 **종속성이 변경되는 경우에** 모든 컴포넌트가 평가된 다음 실행되는 함수입니다.
두번째 인자가 바로 지정된 종속성입니다. 종속성을 가질 것들을 배열로 만들어 두번째 인자로 지정하면 됩니다.

<br>

### 종속성이 빈 배열인 경우
```javascript
useEffect(() => {
    console.log('effect');
}, []);
```
컴포넌트가 처음 mount 된 후, 함수는 한번만 실행됩니다. 예로, 컴포넌트가 처음 렌더링 될 때 HTTP 요청을 보내는 경우에 사용될 수 있습니다.  
이후 컴포넌트가 재평가 되더라도 다시 실행되지 않습니다.  

<br>

### 종속성이 있는 경우
```javascript
useEffect(() => {
    console.log('effect');
}, [dependencies]);
```
종속성이 있는 경후 또한 컴포넌트가 처음 mount 된 후 실행되며, 이후 **종속성이 변경되는 경우에** 컴포넌트가 평가되고 그 다음 실행됩니다.

<br>

### useEffect 클린업 함수
```javascript
useEffect(() => {
    console.log('effect');

    return () => {
        console.log('cleanup');
    }
}, [dependencies]);
```
클린업 함수는 useEffect 함수 부분에 return 값을 만들어 작성할 수 있습니다. 이때, return 값은 무조건 함수여야 합니다.  
이 cleanup 함수는 클린업 프로세스로서, useEffect가 다음에 함수를 실행하기 직전에 실행될겁니다.  
한 마디로, cleanup 함수는 매번 새로운 Side Effect 함수가 실행되기 전에 실행됩니다.

<br>

cleanup 함수는 유용하게 사용할 수 있습니다.  
Side Effect 함수에 setTimeout을 지정한 것을 클린업 함수에서 삭제할 수 있습니다. 예를 들어 이렇게 사용하는 것은, 유저가 타이핑 할 때마다 유효성을 체크하거나 HTTP 요청을 보낼 때 불필요한 너무 많은 작업을 하지 않게 할 수 있습니다.
```javascript
// ...

const [enteredEmail, setEnteredEmail] = useState('');
const [enteredPassword, setEnteredPassword] = useState('');
const [formIsValid, setFormIsValid] = useState(false);

useEffect(() => {
    const identifier = setTimeout(() => {
        console.log('Checking form validity');
        setFormIsValid(
            enteredEmail.includes('@') && enteredPassword.trim().length > 6
        );
    }, 500);

    return () => {
        console.log('cleanup');
        clearTimeout(identifier);
    }
}, [enteredEmail, enteredPassword]);

// ...
```
위의 코드에서 useEffect를 사용할 때 이메일과 비밀번호 값인 `enteredEmail`, `enteredPassword`를 종속성으로 두고 있습니다. 종속성이 변할 때마다 cleanup 함수가 먼저 실행 된 후 useEffect의 함수가 실행됩니다.  

이메일과 비밀번호를 타이핑 할 경우, 텍스트 한개가 추가되면 useEffect의 함수는 계속 불려져 유효성을 검사할 겁니다. 타이핑이 마지막으로 멈춘 부분에서 유효성 검사를 하게 된다면, 불필요하게 많은 작업을 하지 않아도 됩니다.  
useEffect의 cleanup 함수는 타이핑이 멈추고 500ms가 지나지 않은 것들은 모두 지워지도록 쉽게 설정할 수 있습니다. 500ms가 넘은 경우만 유효성 검사를 할 수 있어 보다 효율적입니다. (위의 코드는 cleanup 함수의 예시이기에 적절한 유효성 검사를 하고 있지 않습니다.)

<br>
<br>

## 요약
```javascript
useEffect(() => {
    console.log('run after every render cycle');
});
// 두번째 인자가 없는 경우: 컴포넌트가 처음 mount 됐을 때와, 컴포넌트가 재실행 될 때마다 실행됩니다.

useEffect(() => {
    console.log('only execute once');
}, []);
// 두번째 인자가 빈 array일 경우: 컴포넌트가 처음 mount 되었을 때만 실행됩니다. 이후 re-render cycle에서도 실행되지 않습니다.

useEffect(() => {
    console.log('password dependencies');
}, [enteredPassword]);
// 두번째 인자에 종속성이 있는 경우: 컴포넌트가 처음 mount 됐을 때와, 종속성이 변경되었을 때 이 함수는 컴포넌트가 평가된 다음 실행됩니다.

useEffect(() => {
    console.log('running');

    return () => {
        console.log('cleanup');
    }
}, [enteredPassword]);
// 종속성 있음(클린업 함수 내포): 컴포넌트가 처음 mount 됐을 때 'running'이 실행되고, 이후 종속성이 변경될 때마다 'cleanup'이 실행된 후 re-render 되어 다시 'running'이 실행됩니다.

useEffect(() => {
    console.log('running');

    return () => {
        console.log('cleanup');
    }
}, []);
// 종속성 없음(클린업 함수 내포):  컴포넌트가 처음 mount 됐을 때 'running'이 실행되고, 컴포넌트가 DOM에서 제거되면 'cleanup'이 실행됩니다.
```

<br>
