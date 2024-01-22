# 리액트 컴포넌트 스타일링

리액트 컴포넌트를 스타일링 하는 방법은 다양하지만, 크게 4가지를 간단하게 정리해보겠습니다.
1. vanilla CSS로 스타일링
2. CSS Module로 스코핑된 스타일링
3. 'Styled-components' 모듈을 활용한 CSS-in-JS 스타일링
4. 'Tailwind CSS'로 스타일링

<br/>

## 1. vanilla CSS로 스타일링
vanilla CSS를 이용하는 가장 기본적인 방법입니다.  
컴포넌트 옆에 같은 이름을 가진 .css 파일을 더합니다. 예를 들어, index.jsx 파일이라면 index.css를 더해서 그 안에 CSS를 작성합니다.  

이 파일을 index.jsx에서 import 합니다.
```javascript
import './index.css';
```

그럼 빌드툴이 웹페이지에 CSS가 삽입되도록 도와줍니다.  
index.jsx 뿐만 아니라, 컴포넌트별로 따로 CSS 파일을 분리해서 만들수도 있습니다. 그럼 각각의 컴포넌트 바로 옆에 해당하는 CSS 파일이 있는 구조가 만들어질 것이고, 그럼 스타일을 쉽게 찾아볼 수 있을 것입니다.

**주의해야 할 점은, 스타일이 그 컴포넌트에만 제한되는 건 아니라는 점입니다.**

CSS 파일을 나누고 아무리 컴포넌트마다 나눠서 불러오더라도 그 컴포넌트마다 범위가 생기지 않습니다. 실제 웹 페이지엔 글로벌하게 적용되기 때문입니다. 

### vanilla CSS로 스타일링 장단점
장점
- jsx코드와 분리 가능
- CSS를 안다면 쉽게 사용 가능
- CSS를 사용하기에, 다른 사람들이 쉽게 참여 가능

단점
- CSS에 대한 이해 필요
- **컴포넌트로 범위가 정해지지 않음** (컴포넌트들끼리 스타일 충돌이 있을 수 있음)

<br/>

범위를 잡기 위해선, CSS 파일에 스타일을 정의하는 대신 jsx에 직접 인라인 스타일을 적용하는 방법도 있습니다.

### Inline 스타일
요소마다 스타일을 지정해줄 수 있는 것이 인라인 스타일입니다. 리액트에서 style prop은 스타일 속성값을 기대하기에 style prop을 사용하면 됩니다.  
이 값은 `<p style="color: red;">` 이런 식의 스트링값이 아닌, `<p style={}>` 이런 식의 동적 값을 가져야 합니다. 그 다음 객체 안에 key, value 형태로 스타일을 정의해서 넘겨줘야 합니다.

```javascript
<p style={{
    color: "red"
}}></p>
```

key를 쓸 때, `-` 를 사용한다면 `""` 로 감싸주거나 카멜케이스를 써서 입력해야 합니다.
```javascript
<p style={{
	"text-align": "center",
    // or
	textAlign: "center",
}}></p>
```

인라인 스타일에서 변수나 state 값을 받아서 동적으로 스타일을 적용할 수도 있습니다.
```javascript
<input style={{
	backgroundColor: emailNotValid ? 'red' : 'gray'
}}/>
```

또한 className도 동적으로 줄 수 있습니다.  
label이라는 클래스는 항상 적용하고, invalid 클래스는 동적으로 주고 싶을 때 아래와 같이 코드를 쓸 수 있습니다.
```javascript
<label className='label invalid'>Email</label>
// label, invalid 클래스가 항상 적용됩니다.

<label className={`label ${emailNotValid ? 'invalid' : ''}`}>Email</label>
// label 클래스는 항상 적용되고, emailNotValid 값이 true일 때만 invalid 클래스가 적용됩니다.
```

이런 인라인 스타일의 접근방법은 장단점이 있습니다.

장점
- 쉽고 빠르게 jsx에 추가 가능
- 추가한 요소에만 영향을 줌
- 동적으로, 조건부로 스타일링하기에 간단함 (Dynamic/Conditional 스타일링)

단점
- 모든 요소들을 일일히 스타일링 해야 한다는 것 (p에 공통으로 스타일 불가)
- css와 jsx코드에 구분이 없음

<br/>

범위가 적용되지 않는 가장 큰 단점을 극복하기 위해서, CSS Module을 사용할 수 있습니다.

<br/>

## 2. CSS Module로 스코핑된 스타일링
CSS Module은 범위를 적용해서 바닐라 CSS를 쓸 수 있도록 도와줍니다.  

CSS Module은 브라우저나 자바스크립트 기본 기능이 아닙니다. 빌드툴이 클래스 이름과 각각의 파일에 보증하여 변환해주도록 하는 겁니다.  

스타일을 적용하고자 하는 컴포넌트 옆에 `컴포넌트명.module.css` 이름으로 파일을 생성하면, CSS Module 파일이 만들어진 겁니다. 이제 이 파일 내부에 CSS 스타일 코드를 작성하면 됩니다.  
빌드툴은 `.module.css` 확장자를 가진 파일을 다르게 해석합니다.

이 파일을 불러와 사용하는 건 아래 코드와 같이 하면 됩니다.  
`Header.module.css` 파일 내부에는 para 클래스에 대한 스타일이 적혀있을 겁니다.
```javascript
import classes from './Header.module.css'

<p className={classes.para}></p>
```
저장한 뒤 개발자도구를 켜보면, class 이름이 유니크한 값을 가지게 된 것을 볼 수 있습니다. 이 유니크한 class 이름은 빌드툴에 의해 자동적으로 생성된 이름입니다.

### CSS Module 사용 장단점
장점
- CSS, JSX 분리 가능
- 결국 스타일은 CSS로 작성되기에, 다른 사람들이 쉽게 참여 가능
- **CSS 클래스의 범위가 컴포넌트로 단위로 적용**

단점
- CSS에 대한 이해 필요
- 많은, 작은 CSS 파일을 가지게 될 가능성

<br/>

## 3. 'Styled-components' 모듈을 활용한 CSS-in-JS 스타일링

Styled-components를 사용하면, 수많은 CSS 파일을 가지지 않아도 된다는 장점이 있습니다. 또한 인라인 스타일을 쓸 일도 없게 됩니다.

먼저 npm으로 Styled-components를 설치해줍니다.
```bash
npm install styled-components
```

컴포넌트 파일에 styled를 import 하고, 사용하고자 하는 요소를 꺼냅니다.
```javascript
import { styled } from 'styled-components';

const ControlContainer = styled.div``
```
사용하고자 하는 요소 뒤에 백틱을 두개 붙입니다.  
백틱을 두개 붙이는게 어색할 수도 있는데, 이건 tagged templates로 템플릿리터럴의 좀 더 발전한 형태입니다. 리액트건 아니고 자바스크립트의 기능입니다.

이 백틱 사이에 원하는 스타일을 쓰면 됩니다.  
그리고 그 값 자체를 컴포넌트처럼 사용하면 됩니다.
```javascript
// 컴포넌트 외부
const ControlContainer = styled.div`
	display: flex;
	flex-direction: column; -> 카멜케이스 안써도 됩니다.
`

const Label = styled.label`
	display: block;
	margin-bottom: 0.5rem;
`

// 컴포넌트 내부 return 부분
<ControlContainer>
	<Lable>
	 ...
	</Lable>
</ControlContainer> 
```

원래 input 요소는 type, ClassName, onClick같은 속성이 있는데, Styled-components로 만든 요소들에서 그 속성을 그대로 붙여서 써도 됩니다.  
`styled.div` 와 같이 `styled.`뒤에 쓴 요소는 jsx요소로 전환되는 것이기 때문에 다 물려받습니다.

Styled-components는 유니크한 CSS 클래스 이름을 만들어서, HTML 헤더에 그 클래스(스타일)를 삽입하고, 요소에 그 클래스를 적용합니다.  


### Styled-components의 동적 스타일링
백틱을 쓰기 때문에 `${}`로 변수를 받아서 쓸 수 있습니다. 
```javascript
// 컴포넌트 외부
const ControlContainer = styled.div`
	display: flex;
	flex-direction: column; -> 카멜케이스 안써도 됩니다.
`

const Label = styled.label`
	display: block;
	margin-bottom: 0.5rem;
	color: ${(props) => props.invalid ? 'red' : 'gray'}; 
	color: ${({invalid}) => invalid ? 'red' : 'gray'}; -> 또는 이렇게
`

// 컴포넌트 내부 return 부분
<ControlContainer>
	<Lable invalid={emailNotValid}>
	 ...
	</Lable>
</ControlContainer> 
```
원래는 이 방식으로 prop을 넘겨줄 수 있지만, 이 경우에는 콘솔창에 워닝이 뜨게 됩니다. invalid는 빌트인 prop과 이름이 동일하기 때문입니다.

빌트인 props와 충돌되지 않게 하기 위한 컨벤션은 앞에 `$` 사인을 붙이는 것입니다. 그럼 워닝이 사라지게 될겁니다.
```javascript
// 컴포넌트 외부
const ControlContainer = styled.div`
	display: flex;
	flex-direction: column; -> 카멜케이스 안써도 됩니다.
`

const Label = styled.label`
	display: block;
	margin-bottom: 0.5rem;
	color: ${({$invalid}) => $invalid ? 'red' : 'gray'}; -> 또는 이렇게
`

// 컴포넌트 내부 return 부분
<ControlContainer>
	<Lable $invalid={emailNotValid}>
	 ...
	</Lable>
</ControlContainer> 
```

### Pseudo Selectors, Nested Rules & Media Queries
Styled-components에서 가상선택자(Pseudo Selectors)와 중첩 룰(Nested Rules), 미디어쿼리는 어떻게 다뤄야 할까요?  
보통의 vanilla CSS와 비교해서 코드를 살펴보겠습니다.

vanilla CSS 사용 시 (JSX 코드, CSS 코드)
```javascript
// 보통 jsx
<header>
	<img />
	<h1></h1>
</header>
<div className='button'></div>
```
```css
/* 보통 vanilla CSS */
header {
	display: flex;
	align-items: center;
	justify-content: center;
}

header img {
	object-fit: contain;
}

header h1 {
	font-size: 1.5rem;
	font-weight: 600;
}

.button {
	color: black;
	border: none;
	padding: 1rem; 2rem;
	background-color: yellow;
}

.button:hover {
	background-color: orange;
}

@media (min-width: 768px) {
	header {
		margin-bottom: 4rem;
	}

	header h1 {
		font-size: 2.25rem;
	}
}
```

Styled Components로 변환 시
```javascript
// Styled Components 사용

// 컴포넌트 내부 return 부분
<StyledHeader>
	<img />
	<h1></h1>
</StyledHeader>
<Button></Button>

// 컴포넌트 외부
const StyledHeader = styled.header`
	display: flex;
	align-items: center;
	justify-content: center;

	& img {
		object-fit: contain;
	}

	& h1 {
		font-size: 1.5rem;
		font-weight: 600;
	}

	@media (min-width: 768px) {
		header
	}

	@media (min-width: 768px) {
		margin-bottom: 4rem;

		& h1 {
			font-size: 2.25rem;
		}
	}
`

const Button = styled.div`
	color: black;
	border: none;
	padding: 1rem; 2rem;
	background-color: yellow;

	&:hover {
		background-color: orange;
	}
`
```

<br/>

Styled-components 사용 장단점을 비교해보면 이렇습니다.

장점
- 쉽게 빠르게 추가 가능
- 자동으로 범위가 정해짐

단점
- CSS에 대한 이해 필요
- 리액트와 CSS코드가 깨끗하게 분리되지 않음

<br/>

## 4. 'Tailwind CSS'로 스타일링
Tailwind CSS는 CSS 프레임워크입니다.  
부트스트랩처럼 스타일이 미리 정해진 클래스를 가져다가 쓰는 방식입니다. 리액트 외에서도 사용 가능하며 리액트와 사용하기도 좋습니다.

장점
- CSS를 몰라도 사용 가능
- 빠르게 앱 개발 가능
- CSS룰을 정의하지 않기 때문에 컴포넌트간에 스타일이 충돌되지 않음
- 구성도가 높고 확장성이 좋음

단점
- 클래스 이름이 길어질 가능성
- 스타일을 편집하려면 jsx코드를 편집해야 함 (스타일 코드의 뚜렷한 구분이 없음)

<br/>

## 정리
어떤 하나의 스타일링 방법이 가장 최고의 방법이라고 할 수는 없습니다.  
스코핑을 제공하는 부분은 중요기에 그 부분은 고려하여 결정해야 하지만, 그 외에서는 필요나 취향에 맞게 사용하면 됩니다.  

보통은 여러 방법을 섞어서 쓰지 않고 하나의 방법을 선택해서 씁니다. 각 스타일링의 장단점을 비교해보고 적절한 스타일링을 선택하는 것이 중요합니다.

<br/>