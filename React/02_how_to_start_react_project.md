# React 프로젝트 시작하기

리액트는 웹 환경과 로컬 환경으로 시작할 수 있습니다.

<br/>

## 웹 환경에서 시작하기
가장 간편하게 리액트 프로젝트를 시작하는 방법입니다.  
주소창에 [react.new](https://codesandbox.io/p/sandbox/react-new?utm_source=dotnew)를 입력하고 접속하면, 코드샌드박스에서 제공하는 새로운 리액트 프로젝트가 나타날 겁니다.  
만약 사용하는 컴퓨터의 관리자 권한이 없어 패키지를 설치할 수 없는 경우라면 사용하면 좋겠네요.  

<br/>

## 로컬 환경에서 실행하기
로컬로 실행하는 것의 장점은, 필요에 맞게 구성해서 쓸 수 있다는 점입니다.  

Visual Studio Code와 같은 본인 입맛에 맞는 코드에디터를 설치합니다.  
코드에디터 안에서 extensions를 설치해서 사용할 수 있으니 웹 에디터보다 구성할 수 있는 부분이 많습니다.  
Prettier와 같은 코드 포맷터를 설치해 함께 사용하면 편합니다.  

이런 리액트 로컬 프로젝트를 만들려면 node.js 또한 필요합니다. 리액트를 사용할 때 node.js를 사용하지 않지만 다양한 도구(예를 들어 Vite)를 사용할 때 필요하기 때문입니다.  

로컬에서 리액트 프로젝트를 만드는 데 사용할 수 있는 다양한 도구들이 있는데요,

- 'Vite'
- 'Create React App'

와 같은 툴들이 유명합니다.  
이 두 툴 모두 간단한 명령어로 새로운 리액트 프로젝트를 생성합니다.  

### Vite
```bash
# npm create vite@latest <react-project-name>
npm create vite@latest my-app
```
이 명령어를 입력하면 어떤 프레임워크를 사용한 건지, 타입스크립트를 사용할 건지 등 간단한 질문을 합니다. 선택을 하면 바로 프로젝트가 만들어집니다.  

Vite로 처음 프로젝트를 생성한 뒤에는 `npm install` 명령어를 입력하여 필요한 모든 패키지를 받아줘야 합니다.  
패키지가 모두 설치된 다음 `npm run dev` 명령어로 개발 서버를 실행하면 작업 중인 웹사이트를 방문하고 미리 볼 수 있습니다. 저장할 때마다 자동으로 변경사항이 적용되는 것을 볼 수 있습니다.  

### Create React App
```bash
# npx create-react-app <react-project-name>
npx create-react-app my-app
```
이 명령어를 입력하고 `npm start` 명령어로 개발 서버를 실행할 수 있습니다.  
타입스크립트를 사용하는 리액트 프로젝트를 시작하고 싶다면, 프로젝트를 생성하는 단계에서 아래 명령어로 타입스크립트 템플릿을 지정해 설치하면 됩니다.  
```bash
# npx create-react-app <react-project-name> --template <template-name>
npx create-react-app my-app --template typescript
```

<br/>

## 복잡한 과정이 필요한 이유
리액트 코드를 쓰려면 왜 이렇게 복잡한 과정이 필요할까요?  

그냥 새롭게 html 파일을 만들어서 그 안에 스크립트 파일을 참조하게 한 다음, 리액트 코드를 스크립트 파일 안에 넣으면 안되나요?  
안됩니다. 작동조차 않습니다.

왜냐면 리액트는 특별한 jsx라는 것을 사용하거든요. 안타깝게도 브라우저에선 작동되지 않습니다. 브라우저는 이게 뭔지도 모릅니다.

그래서 **리액트 코드는 브라우저에서 실행되는 코드로 바뀌어야 합니다.**  
코드를 변환하는 것과 함께 최적화 또한 진행합니다. 필요없는 공백 지우기 같은 거요. 그래야 웹 사이트를 방문한 유저가 최소한으로 줄여진 코드를 받고 향상된 웹사이트 성능을 경험할 수 있으니까요.  

이런 역할을 쉽게 하도록 돕는게 위에서 본 Vite와 Create React App과 같은 빌드 툴 입니다.  
더 복잡한 설정을 직접 하지 않고도 프로젝트를 쉽게 생성할 수 있도록 돕는 툴들인 겁니다.  

<br/>