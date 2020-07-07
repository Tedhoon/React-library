# props.children

컴포넌트 태그 사이에 넣은 값을 조회하고 싶을 땐, props.children 을 조회하면 됩니다.

```js
// Hello.js
import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>안녕하세요 {name}</div>
}

Hello.defaultProps = {
  name: '이름없음'
} // defaultProps를 사용하여 props를 정해주지 않았을 때 default값 설정가능 !

export default Hello;
```

```js
// Wrapper.js
import React from 'react';

function Wrapper({ children }) {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>

    </div>
  )
}

export default Wrapper;
```


```js
// App.js
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
      // Wrapper Component안에 Hello Component가 있고, 
      // 이 Hello Component에서 props를 사용하고 싶으면 
      // 상위 Component(Wrapper)에 props.children을 전달해주어야 한다!
    <Wrapper>
      <Hello name="react" color="red"/>
      <Hello color="pink"/>
    </Wrapper>
  );
}

export default App;
```