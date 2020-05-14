# redux
redux는 react와 궁합이 잘맞는(혹은 이를 위해 만들어진) 상태관리 툴

## why redux?
컴포넌트는 local state를 가지고있지만 redux는 global state를 가지고있다.

## about more
- 모든 state는 javascript object형태로 저장된다.
- 이 object가 store라고 생각해도 문제없음
- state를 수정하기 위해서는 action를 reducer에 보내면된다.
- 이 action을 보내는 과정을 dispatch라고 한다.
- reducer는 rootReducer로 combine해서 store에 등록해준다!
- 즉 컴포넌트와 연결되서 소통하는 건 action이고 state(object)를 바꿔주는건 reducer임

## connenct
```js
import { connect } from 'react-redux';
```
- subscribe형태로 connect를 쓰는 것 같다..!
- mapStateToProp과 action함수를 인자로 받아서 등록해준다. 
- mapStateToProp에는 관련 global state에 기인해서 작성한다.
- 또한 hook기준 함수형 컴포넌트에도 global state 또는 action함수를 받아서 사용한다.
- 위의 사항은 왜그런진 모르겠음.. 그냥 하다보니까 저렇게 외웠다.. 나중에 고수가 되면 다시 써야겠다