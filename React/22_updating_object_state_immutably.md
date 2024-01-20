# 객체 State를 불변하게 업데이트하기

객체형태의 state가 있다면 어떻게 업데이트게 가장 좋은 방법일까요?

```javascript
const initialGameBoard = [
  [null, null, null],
  [null, null, null],
  [null, null, null],
];

export default function GameBoard() {
    const [gameBoard, setGameBoard] = useState(initialGameBoard);

    function handleSelectSquare(rowIndex, colIndex) {
		// 이전 state 기반으로 업데이트?
        setGameBoard((prevGameBoard) => {
			prevGameBoard[rowIndex][colIndex] = 'X';
            return prevGameBoard;
        });
    }

	// ...
```

이 방법을 사용할 수는 있지만, 이 접근방법은 절대 권장되는 방법이 아닙니다.

**state가 객체나 배열같은 레퍼런스 타입인 경우 이렇게 하는 대신, 불변의 방식으로 업데이트하는 것을 강력히 권장합니다.** 복사본을 만드는 방법이요.

**먼저 새 객체나 새 배열 카피본을 만들고, 그걸 변경해서 return해 업데이트 해야 합니다.**

이런 방법을 권장하는 이유는, state가 객체나 배열이면 reference value이기 때문에 이렇게 업데이트할 경우 메모리 값이 즉시 업데이트 될 것이기 때문입니다. 리액트가 state를 업데이트 시키기도 전에요. 같은 state에 대해 여러 곳에서 업데이트 한다면 이상한 버그나 사이드이팩트가 일어날 수 있습니다.

따라서 아래와 같이 업데이트해야 합니다.

```javascript
const initialGameBoard = [
  [null, null, null],
  [null, null, null],
  [null, null, null],
];

export default function GameBoard() {
  const [gameBoard, setGameBoard] = useState(initialGameBoard);

	function handleSelectSquare(rowIndex, colIndex) {
		// 올바른 업데이트 방식
    setGameBoard((prevGameBoard) => {
      const updatedBoard = [...prevGameBoard.map(innerArray => [...innerArray])];
      updatedBoard[rowIndex][colIndex] = 'X';
      return updatedBoard;
    });
  }

	// ...
```

<br/>