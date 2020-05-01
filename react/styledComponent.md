# Styled Component

## install
```bash
$ npm install --save styled-components
$ yarn add styled-components
```

## 사용법

### 기본사용법
> styled를 임포트한 후 변수에 html, css를 담아줍니다.
```js
import React, { Component, Fragment } from 'react';
import styled from 'styled-components';
    
class App extends Component {
  render() {
    return (
      <Fragment>
		<Button>Hello</Button>
		<Button danger>Hello</Button>
      </Fragment>
    )
  }
}

const Button = styled.button`
	border-radius: 50px;
    padding: 5px;
    min-width: 120px;
    color: white;
    font-weight: 600;
    -webkit-appearance: none;
    cursor: pointer;
    &:active,
    &:focus {
      outline: none;
    }
    background-color: ${props => (props.danger ? 'red' : 'purple')}
`;

export default App;
```

### 다른 속성값을 조정하고 싶을 땐?

> createGlobalStyle를 사용합니다.
```js
...
import styled, { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
	body {
		padding: 0;
		margin: 0;
	}
`;

...

<React.Fragment>
  <GlobalStyle>	//	<-- 추가
  <Button>Hello</Button>
  <Button danger>Hello</Button>
</React.Fragment>

```

### style재사용?
> withComponent 사용하여 Buttom에서 사용한 style을 a태그에 적용시켜서 사용할 수 있습니다.
```js
const Anchor = Button.withComponent('a');
```

> style추가하고 싶을 땐 styled메소드로 오버라이딩
```js
const Anchor = styled(Button.withComponent('a'))`
	text-decoration: none;
`;
```

### animation은..?
> keyframes도 만들어놨다!
```js

import styled, { keyframes } from "styled-components";

...
const toggleShow = keyframes`
from {
  opacity: 0;
}
to {
 opacity: 1;
}
`;


const Click = styled.div`
  animation: ${toggleShow} 1.5s linear infinite;
`;
```
> props를 이용해서 css바꿔보기
```js
import styled, { createGlobalStyle, css, keyframes } from 'styled-components';

...

const Button = styled.button`
	${props => {
		if (props.danger) {
            return css `animation: ${rotation} 2s linear infinite `;
		}
	}}
`;

const rotation = keyframes`
	from {
		transform: rotate(0deg);
	}
	to {
		transform: rotate(360deg);
	}
`
```
### 태그 속성 변경
> attr 오버라이딩
```js
const InputCheckbox = styled.input.attrs({
	type: 'checkbox',
  	checked: true,
})`
	border-radius: 5px;
	color: red;
`;
```
### base 속성 적용하기(mixin)
> 변수에 css 메서드를 이용해서 만들어두고 적용시킨다!
```js
import styled, { css } from 'styled-components';

const awesomeCard = css`
	box-shadow: 0 4px 6px blue, 0 1px 3px blue;
	background-color: white;
	border-radius: 10px;
	padding: 20px;
`;

const InputCheckbox = styled.input.attrs({
	type: 'checkbox',
  	checked: true,
})`
	border-radius: 5px;
	color: red;
	${awesomeCard} // <== 추가
`;
```
### Child Component에 스타일 적용
> ThemeProvider와 theme를 import 한뒤, 컴포넌트 최상단 ThemeProvider 컴포넌트에 theme를 props로 넣어준다.
```js
// theme.js
const theme = {
  mainColor: 'red',
  dangerColor: 'blue',
  successColor: 'gray',
}

export default theme;
```
```js
// app.js
import styled, { createGlobalStyle, ThemeProvider } from 'styled-components';
import themes from './themes';

const Card = styled.div`
	background-color: white;
`;

const Button = styled.button`
  border-radius: 30px;
  padding: 25px 15px;
  background-color: ${props => props.theme.dangerColor} // <== props.theme의 값을 설정할 수 있음
`;

class App extends Component {
  render() {
    return (
      <ThemeProvider theme={themes}>	// <== props
        <React.Fragment>
          <GlobalStyle />
          <Container>
            <Form />
          </Container>
        </React.Fragment>
      </ThemeProvider>
    );
  }
}

const Form = () => (
  <Card>
    <Button>Hello</Button>
  </Card>
)
```
