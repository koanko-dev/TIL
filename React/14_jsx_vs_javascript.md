# 리액트에서 꼭 JSX를 사용해야 할까?

리액트에서 JSX 코드를 사용하지 않고도 컴포넌트를 만들 수도 있습니다.  

JSX는 리액트 프로젝트에서 필수는 아니지만 편리합니다. 그래서 대부분의 리액트 프로젝트에서는 JSX를 씁니다.

JSX를 사용하지 않고 어떻게 컴포넌트를 만들 수 있을까요?

```jsx
<div id='content'>
	<p>Hello World!</p>
</div>
```

이 JSX 코드를 변환해보겠습니다.

```javascript
React.createElement(
	'div',
	{ id: 'content' },
	React.createElement(
		'p',
		null,
		'Hello World!'
	)
)
```

이 코드는 표준 자바스크립트의 기능만 사용합니다. `createElement` 메서드를 이용해 동일한 구조와, 동일한 HTML 코드를 생성합니다.

빌드 프로세스가 없는 프로젝트를 원한다면 이렇게 할 수도 있습니다. JSX는 빌드 프로세스가 필요하니까요.  
하지만 그런 프로세스가 같이 딸려 있는 프로젝트를 만드는 건 어렵지 않습니다. 혼자 작성할 필요도 없고요.  
그렇기 때문에 JSX 접근법이 일반적으로 사용하기 쉽고 읽고 이해하기도 쉽습니다.

JSX를 사용하지 않는 접근법은 너무 장황하고 직관적이지 않습니다.  
실제 코드에서 JSX를 자바스크립트로 대체하는 테스트를 해보겠습니다.

리액트 프로젝트의 가장 루트에 위치하는 `Index` 컴포넌트입니다.

```jsx
import ReactDOM from "react-dom/client";

import App from "./App.jsx";
import "./index.css";

const entryPoint = document.getElementById("root");
ReactDOM.createRoot(entryPoint).render(<App />);
```

`<App />` 을 랜더링하는 부분을 변경해보면 이렇습니다.

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import App from "./App.jsx";
import "./index.css";

const entryPoint = document.getElementById("root");
ReactDOM.createRoot(entryPoint).render(React.createElement(App));
```

`createElement` 를 이용했습니다. 저장해보면 오류없이 똑같이 잘 작동됩니다.  

<br/>