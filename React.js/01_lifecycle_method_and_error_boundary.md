# Lifecycle Methods & Error Boundary

리액트 16.8 버전부터 Hook이라는 개념이 정식으로 도입되었습니다.  
Hook 사용 이전에는 side effect를 어떻게 관리했을까요?  
클래스 기반 컴포넌트에서는 리액트 Hook을 사용할 수 없습니다. 따라서 side effect를 사용할 수 없습니다.  
하지만 클래스 기반 컴포넌트에는 component life cycle(컴포넌트 수명 주기)라는 개념이 있습니다.  
클래스 기반 컴포넌트에 추가할 수 있는 가장 중요한 수명주기 메서드는 아래와 같습니다.  
- `componentDidMount()`
- `componentDidUpdate()`
- `componentWillUnmount()`
  
<br>
<br>

## `componentDidMount()`
`componentDidMount()`는 컴포넌트가 mount 되었을 때 불려집니다.  
evaluate 되거나 DOM에 렌더될 때 불려진다는 것입니다.  
Hook에서 `useEffect(..., [])`를 사용할 때의 효과와 같습니다.  
Effect 함수는 종속성이 없으면 즉, 빈 종속성 배열이라면 mount 되었을 때만 실행됩니다.  
따라서 빈 종속성 배열을 추가한 useEffect는 componentDidMount()와 같습니다.  
> `componentDidMount()` = `useEffect(..., [])`

<br>
<br>

## `componentDidUpdate()`
`componentDidUpdate()`는 컴포넌트가 업데이트되면 불려집니다.  
무언가 바뀌면 state가 바뀌고 컴포넌트가 재평가되고 렌더링됩니다.  
이것은 어떤 종속성을 가진 useEffect와 같습니다. `useEffect(..., [someValue])`와 같이 종속성 배열을 가진 경우입니다.  
dependency가 바뀌면 useEffect 함수는 다시 실행됩니다.  
> `componentDidUpdate()` = `useEffect(..., [someValue])`

<br>
<br>

## `componentWillUnmount()`
`componentWillUnmount()`는 구성 요소가 DOM에서 제거되기 직전에 호출됩니다.  
이것과 동등한 것은 useEffect에서 클린업 함수를 사용하는 것입니다.  
클린업 함수는 항상 컴포넌트가 DOM에서 제거되기 직전에 호출됩니다.  
> `componentWillUnmount()` = `useEffect(return () => {...}, [])`

<br>
<br>

## Class-based Components vs. Functional Components
원한다면 클래스 기반 컴포넌트로 프로그램 전체를 빌드할 수 있습니다.  
하지만 현대식 리액트 앱에서는 Functional Components를 고수합니다. 단순하고 더 유연하기 때문입니다.  
클래스 기반 컴포넌트를 사용할지 Functional 컴포넌트를 사용할지 고민이라면 (사실 기호에 따라 사용하면 됩니다만) 단순히 다음과 같이 생각해보면 좋습니다.  
- 단순히 클래스 기반 컴포넌트를 선호한다거나
- 클래스 기반 컴포넌트를 이미 사용하고 있는 프로젝트 또는 팀에서 일하거나
- Error Boundary를 구축하려고 한다면

클래스 기반 컴포넌트를 사용하기로 결정할 수 있습니다.  
Error Boundary를 구축하려면 반드시 클래스 기반 컴포넌트를 사용해야만 합니다.  

<br>
<br>

## Error Boundary
에러가 일어났을 때 자바스크립트에서는 일반적으로 `try/catch`로 에러를 처리합니다.  
어떤 컴포넌트 안에서 오류가 발생했는데 그 컴포넌트 안에서 처리하지 않고 부모 컴포넌트에서 처리한다고 합시다.  
그럼 `try/catch`문을 사용할 수 없습니다. `try/catch`는 자바스크립트를 작성하는 곳에서만 작동하기 때문입니다.  
부모 컴포넌트에서 자식 컴포넌트의 에러가 발생하는 곳은 JSX 코드 부분인데, 이 JSX 코드에서는 `try/catch`로 래핑할 수 없기에 그렇습니다.  
이런 경우 Error Boundary를 구축하고 활용할 수 있습니다.  

새로운 Error Boundary 컴포넌트를 만들어서 사용하면 됩니다.  
```javascript
import { Component } from 'react';

class ErrorBoundary extends Component {
    constructor() {
        super();
        this.state = { hasError: false };
    }

    componentDidCatch(error) {
        this.setState({ hasError: true });
    }

    render() {
        if (this.state.hasError) {
            return <p>Something went wrong!</p>
        }
        return this.props.children;
    }
}

export default ErrorBoundary
```

`componentDidCatch()` 메서드는 어떤 클래스 기반 컴포넌트에서든지 추가할 수 있습니다.  
클래스 기반 컴포넌트에 추가할 때마다 그 클래스 기반 컴포넌트를 Error Boundary로 만듭니다.  
그래서 Error Boundary로 만들려면 클래스 기반 컴포넌트를 사용해야 한다는 것입니다.  

Error Boundary를 사용할 컴포넌트에서 에러가 나는 컴포넌트를 Error Boundary 컴포넌트로 감싸주면 됩니다.  
```javascript
import ErrorBoundary from './ErrorBoundary';

// ...

class UserFinder extends Component {
    // ...

    render() {
        return (
            <ErrorBoundary>
                <Users>
            </ErrorBoundary>
        )
    }
}
```

개발 단계에서는 오류가 발생했다는 것을 확실히 알려주기 위해서 여전히 에러 메세지가 나옵니다.  
프로덕션을 위해 응용 프로그램을 만들어 배포하는 경우, 이 메세지는 보이지 않을 것입니다. 실제 프로덕션 앱에서는 개발 오류 메세지가 나오지 않습니다.  

이 Error Boundary를 설정하는 이유는, 무언가 잘못돼도 앱 전체가 다운되지 않게 하려는 것입니다.  
다운되지 않는 대신 오류를 잡아 우아하게 처리할 수 있습니다.  
다시 말하자면, Error Boundary를 추가하려면 클래스 기반 컴포넌트가 필요합니다. Functional 컴포넌트로는 불가합니다.  

<br>
