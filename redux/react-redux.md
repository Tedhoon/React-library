# react-redux
```js
// 액션 타입을 정의해줍니다.
const INCREMENT = 'counter/INCREMENT';
const DECREMENT = 'counter/DECREMENT';

// 액션 생성 함수를 만듭니다.
// 이 함수들은 나중에 다른 파일에서 불러와야 하므로 내보내줍니다.
export const increment = () => ({ type: INCREMENT });
export const decrement = () => ({ type: DECREMENT });

// 모듈의 초기 상태를 정의합니다.
const initialState = {
  number: 0
};

// 리듀서를 만들어서 내보내줍니다.
export default function reducer(state = initialState, action) {
  // 리듀서 함수에서는 액션의 타입에 따라 변화된 상태를 정의하여 반환합니다.
  // state = initialState 이렇게 하면 initialState 가 기본 값으로 사용됩니다.
  switch(action.type) {
    case INCREMENT:
      return { number: state.number + 1 };
    case DECREMENT:
      return { number: state.number - 1 };
    default:
      return state; // 아무 일도 일어나지 않으면 현재 상태를 그대로 반환합니다.
  }
}
```

#### store
store는 글로벌 state 즈음으로 생각하면 됩니다.

#### action type
action에 대한 type을 지정해줍니다.
ex) INCREMENT라는 타입을 지정해줌
```js
const increment = () => {
    return {
      type: 'INCREMENT'
    }
}
const decrement = () => {
    return {
      type: 'DECREMENT'
    }
}
```

#### reducer
state를 받아서 action type을 읽고 그에 정의되어 가공된 state를 재가공해서 뿌려줌
```js
// 당연히 state와 action을 받는다.
const counter = (state = initialState, action) => {
    switch(action.type){
        case "INCREMENT":
            return state + 1;
        case "DECREMENT":
            return state = 1;
    }
}

// 그리고 store에 등록해준다.
import { createStore } from 'redux';
let store createStore(counter);

// 그리고 console에 등록 (redux-devtool 사용 안할시)

store.subscribe(()=> console.log(store.getState()));
```

#### dispatch
```js
store.dispatch(increment());
store.dispatch(decrement());

```






# 실질적인 React 적용방식
리액트에서는 보통 type을 따로 지정해주고
type을 임포팅해와서 관련 action과 reducer를 정의해줍니다아
```js
// type.js
export const INCREMENT =  'INCREMENT';
export const DECREMENT =  'DECREMENT';
```

```js
// counterAction.js
export const increment = () => {
    return {
      type: "INCREMENT"
    }
}
export const decrement = () => {
    return {
      type: "DECREMENT"
    }
}

// 이 안에서 dispatch를 수행해주거나 할수도 있숨


export const increment = () => (dispatch, getState) => {
    dispatch({type: INCREMENT})

    const foo = getState().~;

    ...

    // api요청 후에 받은 데이터를 넘겨줌
}
```


```js
// counterReducer.js
const initialState = {
  state: 0,
};

// const counterReducer = (state = initialState, action) => {} 당근 가능
export default function counterReducer(state = initialState, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
}
```

스토어에 등록전에 묶어준다(스토어에도 콤마도 굳이 적기 가능)

```js
// reducer/index.js
import {combineReducer} from 'redux';
import couterReducer from './counter';
import loggerReducer from './isLogged';

const rootReducer = combineReducer({
  // myname : Reducer 
  counter: counterReducer,
  isLogged: loggedReducer

  //es6부터는 밑의 두개가 같은 뜻이다.
  // counterReducer
  // counterReducer : counterReducer
   
})

export default rootReducer;

```
```js
// app.js
import rootReducer from 'reducer/index';

const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
  // devtool추가
  );
```
그리고 스토어 등록
```js
import {Provider} from 'react-redux';

<Provider store={store}>
  <App />
<Provider>
```

store에 접근해서 state 건드리기!
```js
import { useSelector } from 'react-redux';
function App() {
  
  const counter = useSelector(state=>state.counter);
  return (
    <div>
      {counter}
    </div>
  )
} 
```
그리고 action을 이용해서 reducer에 접근하여 state를 조정!
```js
import { useSelector, useDispatch } from 'react-redux';
import { increment } from './actions';
function App() {
  
  const counter = useSelector(state=>state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      {counter}
      <button onClick={()=> dispatch(increment())}>+</button>
    </div>
  )
} 
```

### 마지막!! payload이용하기! 중-요

```js
<button onClick={()=> dispatch(increment(5))}>+</button>
```
인자를 넣어줬을 때

```js
export const increment = (hello) => {
    return {
      type: "INCREMENT",
      payload: hello
    }
}
```
action에서 인자를 받아서 payload에 넘겨준 줄수있고

```js
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return state + action.payload;

    default:
      return state;
  }
}
```
reducer에서는 action.payload로 받아서 사용한다!


