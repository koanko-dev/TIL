# ReactDOM - Potal
## Potals
모달 창과 같은 컴포넌트는 구조적으로 어느 곳에 위치해야 할까요?  
모달 창을 사용하는 컴포넌트 안에 위치한다면, 화면 전체를 덮는 모달의 구조로 보았을 때 적절하지 않은 위치로 보여집니다.  
이런 케이스에 사용하기 좋은 것이 Potal입니다.

ReactDOM의 `createPortal()`을 사용해 Potal을 생성할 수 있습니다.  
`createPortal()`의 첫번째 요소로는 사용할 컴포넌트를 넣고, 두번째 요소로는 포탈이 위치할 자리를 넣으면 됩니다.  
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

html에서 포탈이 띄워질 자리를 만듭니다.  
이 케이스에서는 루트와 같은 위치에 포탈을 띄우는 것이 적절한 방법으로 여겨집니다.  
```html
<!-- ... -->
<body>
    <div id='backdrop-root'></div>
    <div id='overlay-root'></div>
    <div id='root'></div>
</body>
<!-- ... -->
```
 