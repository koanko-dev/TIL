# 리액트 앱 디버깅하기
코드를 작성하다보면 어쩔 수 없이 에러와 버그를 포함하는 코드를 작성하게 될 겁니다.

리액트는 오류 메세지가 친절하게 나와서 오류를 찾아서 고칠 수 있도록 합니다.  
하지만 때때로 오류 메시지가 만들어지지 않는 경우도 있습니다.  
그런 오류를 어떻게 찾아 고칠 수 있을까요?

- 브라우저의 개발자 도구, 디버거
- 리액트 strict 모드
- React DevTool

<br/>

## 리액트 에러 메세지 이해하기
먼저 리액트에서 에러가 일어 났을 때, 개발자 도구에서 오류 메시지를 볼 수 있습니다.  
이런 오류 메시지와 마주하면 순식간에 압도될 수 있습니다. 붉은 색이 많고 장황하게 써있는 느낌이 드니까요.  

하지만 오류메세지는 오류를 해결하기에 통찰력 있고 도움됩니다. 특히 오류 메세지의 가장 앞부분에 보통 중요한 말이 나옵니다.  
그리고 그 밑에는 코드가 실행된 목록이 나옵니다. 오류가 난 부분에 대한 발자국들인 셈입니다.  
적혀있는 해당 함수에 들어가서 몇번째 줄인지 말해준 대로 찾으면 오류를 쉽게 잡을 수 있습니다.

<br/>

## 브라우저 디버거 쓰기
모든 오류가 오류 메시지를 보여주진 않습니다.
코드에 논리적 오류도 있을 수 있거든요. 이런 경우 코드가 정상적으로 작동하지 않지만, 오류 메세지가 뜨지 않습니다.  

이럴 땐 항상 논리적으로 생각해야 합니다. 일단, 오류가 어느 시점에서 일어나는지 생각해보고 브라우저 개발자도구를 켭니다.

Sources 탭에 들어가서 실행된 개발서버 localhost:xxxx 를 클릭합니다.  
그 안에는 작업한 코드의 폴더 구조와 동일한 폴더 구조가 있는 것을 볼 수 있습니다.  
의심되는 코드 부분을 찾아 들어가 라인 넘버를 클릭해 breakpoint를 만듭니다. 

문제로 의심 되는 작업을 실행하면, breakpoint에서 코드 실행이 멈출 것입니다. 원래대로 쭉 더 진행되지 않고요.  
그 다음부터는, 밑의 넘어가는 화살표모양을 클릭하면 한 단계씩 코드를 진행시켜보며 어느 부분에서 문제가 일어났는지 확인해볼 수 있습니다.  

<br/>

## 스트릭 모드 사용하기
스트릭 모드는 보통 index.js 에서 사용합니다.  
스트릭 모드를 사용하려면, 리액트로부터 `StrictMode` 컴포넌트를 가져와 `<App />`을 랜더링하는 부분을 감싸줍니다.

스트릭 모드를 사용하고 싶은 특정한 컴포넌트를 `StrictMode` 컴포넌트로 감싸줘도 되지만, 보통 스트릭 모드를 사용하면 이렇게 루트에서 감싸주는 경우가 많습니다.
```javascript
import { StrictMode } from 'react';

// ...

ReactDOM.createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```
스트릭 모드는 앱 뒤편에서 특정한 문제를 잡아내도록 도와주는 역할을 합니다.  
스트릭 모드는 컴포넌트가 한번이 아니라 두번 실행되게 합니다. 따라서, 이 자체가 문제를 해결하지는 않지만 문제가 있다는 걸 바로 드러내기 때문에 유용합니다.  
이렇게 두번 실행되는 것은 개발할 때만 그렇고, 실제 프로덕션에서는 한번만 실행됩니다.

<br/>

## React DevTools 사용하기
크롬 확장프로그램인 React Developer Tools를 사용하면 더 편하게 디버깅할 수 있습니다.  

React Developer Tools를 추가하고 개발자도구를 켜보면 Components 탭이 새로 추가된 것을 볼 수 있습니다.  
앱에서 사용하는 컴포넌트들이 뜰 텐데, hover해서 어떤 컴포넌트인지 화면에서 바로 볼 수도 있고, 어떤 컴포넌트가 어떤 props를 가지고 있는지도 바로 볼 수 있습니다.  

만약, useState를 사용한다면 hooks 부분에 state 값이 저장되어 있는 것을 볼 수 있습니다.

React Developer Tools은 props와 state가 어떻게 UI에 반영되는지 빨리 파악할 수 있는 점이 장점입니다.  
이 부분을 이용하면 디버깅하기에도 용이합니다.

<br/>