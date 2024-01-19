# Props 속성 전달에 대해

Props으로 지정한 것들은 자동으로 JSX 코드로 전달되지 않습니다.  

아래의 코드를 보면 이해할 수 있습니다.  
Section 컴포넌트는 title 값과 id 값을 내려보냅니다.
```javascript
<Section title="Examples" id="examples">
	{/* JSX code */}
</Section>
```

Section 컴포넌트는 section 요소로 래핑된 코드를 반환합니다.  
```javascript
export default function Section({ title, children }) {
  return (
    <section>
      <h2>{title}</h2>
      {children}
    </section>
  );
}
```
이 때 section 요소가 `id="examples"` 값을 가지고 있을 거라고 기대해서는 안된다는 것입니다. 자동으로 속성들이 넘어가지 않거든요.

하지만 이렇게 안쪽에 있는 요소에게 직접 속성을 붙여주고 싶을 때는 어떻게 해야 할까요?  
그 갯수가 지금처럼 1개라면 직접 붙여줘도 되지만, 매번 붙여주는 속성이 달라지고 몇개로 늘어나든 자동으로 요소에 넘겨주는 방법은 없을까요?  

자바스크립트 rest를 사용하면 쉽게 가능합니다. title과 children을 제외한 props을 rest로 가져와서 바로 속성에 spread 오퍼레이터로 넣을 수 있습니다.

```javascript
export default function Section({ title, children, ...props }) { // rest로 나머지 가져오기
  return (
    <section {...props}> {/* spread 연산자로 그대로 속성 넣기 */}
      <h2>{title}</h2>
      {children}
    </section>
  );
}
```

이렇게 하면 동적으로 속성을 받아와 사용할 수 있다는 장점이 있습니다.

<br/>