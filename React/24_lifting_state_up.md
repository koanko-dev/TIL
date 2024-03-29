# State를 들어올리기 (Lifiting State Up)

나눠진 두개의 컴포넌트에서 하나의 state가 둘 다 필요하다면 어떻게 해야 할까요?

두 개의 컴포넌트의 가까운 조상 컴포넌트에서 state를 관리하면 됩니다. 정보가 필요한 양쪽 컴포넌트에서 엑세스할 수 있으니까요.

이렇게 여러 컴포넌트가 필요로 하는 state를 그들의 조상 컴포넌트로 옮겨 관리하는 컨셉이 Lifiting State Up 입니다.

조상 컴포넌트는 state를 변경하도록 트리거하는 함수를 props를 통해 자식 컴포넌트로 보냅니다. 이 함수가 자식 컴포넌트로 하여금 state를 변경할 수 있게 해줍니다. 함수뿐만 아니라 state값도 props를 통해서 보내기 때문에 자식 컴포넌트들은 똑같은 state를 공유할 수 있게 됩니다.

이런 Lifiting State Up 컨셉은 리액트에서 아주 중요한 개념입니다. 앱을 만들 때 많이 할 일이기도 하니까요.

<br/>

### State를 들어올리지 말아야 할 때?

컴포넌트가 공통으로 필요한 state라고 해서 언제나 무조건 state를 상위로 들어올리는 것은 좋지 않습니다.

state를 상위로 들어올리는 것의 좋지 않은 예를 들어보겠습니다.

자식 컴포넌트에 state가 있는데 이 state는 input의 값을 저장하는 state입니다. input에 onChange 이벤트 마다 state는 값이 업데이트 됩니다. Two-Way-Binding 되어 있는 상태인 input인 겁니다.  
이 state 값이 다른 컴포넌트에도 공통으로 필요해서 상위 부모 컴포넌트로 들어올리면 어떻게 될까요?

input이 변경 될 때마다, 즉 키가 입력 되는 순간순간 마다 부모 컴포넌트가 불필요하게 재랜더링 될 겁니다.

이렇게 **불필요한 재랜더링이 일어나는 경우에는 state를 들어올리면 안됩니다.**

위의 예시에 해결 방법은 부모 컴포넌트에 새로운 state를 생성해 관리하는 겁니다.

input이 변경될 때마다가 아닌, 모든 값이 입력되고 저장될 때 부모 컴포넌트의 새로운 state를 업데이트하면 간단하게 해결됩니다.

<br/>