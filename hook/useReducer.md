# useReducer

✔ 목적 : 관리할 state와 setState가 너무 많을 때 하나의 state, setState로 관리할 수 있게 해준다!

✔ 순서 : action 객체를 정의한다 -> dispatch로 실행 -> 실행된 action을 reducer에서 해석하여 state변경

```js
import React, { useReducer } from 'react';

const initialState = {
    a: '',
    b: '',
    c: '',
};

const SET_A = 'SET_A'; // action 이름(type)은 변수로 빼놓는게 좋습니당

const reducer = (state, action) => {
   switch (action.type) {
     case SET_A:
       // state.a = action.a; 이렇게 하면 불변성 유지가 안됨, 반드시 새로운 객체를 복사해서 만든 다음에 바뀌는 부분만 바꿔줘야 한다!
       return {
           ...state,
           a: action.a,
           // 기존 state를 복사하고, 바뀌는 부분만 수정
       }
   } 
};

function TestComponent () {
    const [state, dispatch] = useReducer(initialState, reducer);

    // 컴포넌트 안에 들어가는 함수는 모두 useCallback
    const onClickTable = useCallback(() => {
        dispatch({ type: SET_A, a: 'a' })
        // dispatch => action을 실행
        // { type: 'SET_A', a: 'a' } => action 객체
        // action을 실행한다고 해서 state를 바꿀 수 있는게 아니라, action 객체를 해석해서 state를 바꿔주는 reducer의 역할이 필요하다.
    }, [a])
    return (
        <Table onClick={onClickTable} />
        ...
    )
}
...
```