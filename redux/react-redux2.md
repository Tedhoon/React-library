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

## conenct
```js
import { connect } from 'react-redux';
```
- subscribe형태로 connect를 쓰는 것 같다..!
- mapStateToProp과 action함수를 인자로 받아서 등록해준다. 
- mapStateToProp에는 관련 global state에 기인해서 작성한다.
- 또한 hook기준 함수형 컴포넌트에도 global state 또는 action함수를 받아서 사용한다.
- 위의 사항은 왜그런진 모르겠음.. 그냥 하다보니까 저렇게 외웠다.. 나중에 고수가 되면 다시 써야겠다

> connet() 사용 형식
```js
import { connect } from 'react-redux'
import TodoApp from "./components/TodoApp"
 
 // connect 함수를 실행시키고 
 // TodoApp컴포넌트에서 store에 접근하게 만든다.
 const Todo = connect();
 export default Todo(TodoApp);
 
 // 위의 코드를 간단하게 만들면 아래와 같다.
 export default connect()(TodoApp);
```



## 🔹mapStateToProps

mapStateToProps는 connect함수에 첫번째 인수로 들어가는 함수 혹은 객체다.

mapStateToProps는 기본적으로 store가 업데이트가 될때 마다 자동적으로 호출이 된다. 이를 원하지 않는다면 null 혹은 undefined값을 제공해야한다.

```js
// mapStateToProps는 안쓰고 mapDispatchToProps만 쓰고 싶을 때!

export default connect(null, mapDispatchToProps)((Component));
// 일케 null 값을 줘서 작성합니당
```

store.getState()를 하게 되면 reducer에서 정의해준 global state가 뜬다.

mapStateToProps함수는 store에서 state를 가져와서 해당 컴포넌트에 props로 불여넣어주는 기능을 한다.

```js
// mapStateToProps는 기본적으로 state가 첫번째 인자로 사용된다.
// 그로인해 우리는 state를 다룰수 있게된다.
const mapStateToProps = state => ({ todos: state.todos })
```

```js
// mapStateToProps의 두번째 요소로는 우리가 원하는 객체를 인자로 넘겨주면된다.
`state와 ownProps모두 순수 객체여야 한다.` 
// return object를 해주라는 말!
const mapStateToProps = (state, ownProps) => ({
  return { todo: state.todos[ownProps.id] }
})
```

> mapStateToProps를 connect함수에 사용하기
```js
import { connect } from 'react-redux';
import TodoApp from "./components/TodoApp";
 
 const mapStateToProps = (state, ownProps) => ({
    return { todo: state.todos[ownProps.id] }
  })
 
 //connect 첫번째 인자로 mapStateToProps 함수를 제공했다.
 export default connect(mapStateToProps)(TodoApp);
 ```

### 순서 정리 해준다!! 😎
1. mapStateToProps 함수 작성
2. 함수내에서 store의 state를 가져와서 return 해줄 수 있음
3. connet함수를 이용해서 해당 컴포넌트와 연결
4. 해당 컴포넌트에서 쓰고 싶으면 구조분해해서 props로 가져온다.


## 🔹 mapDispatchToProps 

mapDispatchToProps는 connect함수의 두번째 인자로 사용된다.

간단하게 정리하면 store에 접근한 컴포넌트가 store의 상태를 바꾸기 위해
`dispatch`를 사용할수 있게 만들어준다!


Ex) mapDispatchToProps의 dispatch
```js
/* 
mapDispatchToProps는 첫번째 인자로
redux의 dispatch를 인자로 사용한다. (TMI : 2번째 인자로 ownProps도 받을 수 있음)
이를 통해 우리는 store의 상태를 변경할수있다.
*/

const mapDispatchToProps = dispatch => {
  // 순수 객체를 반환해줘야한다.
  return { dispatch }
  // 이렇게 return 해주고 props처럼 받아서 dispatch 쓸 수 있음!
}

// 그런데! 함수로 작성해서 보내주면 깔끔하다!

const mapDispatchToProps = dispatch => {
  // 순수 객체를 반환해줘야한다.
  return {
    // 순수 action객체를 dispatch 해준다.
    increment: () => dispatch({ type: 'INCREMENT' }),
    decrement: () => dispatch({ type: 'DECREMENT' }),
    reset: () => dispatch({ type: 'RESET' })
  }
}

```

Ex) mapDispatchToProps로 dispatch하기


```js
import toggleTodo from "./actions/toggleTodo";

const TodoApp = () => {
	const { toggleTodo, todo } = this.props;
	return (
    	<div>
        	// mapStateToProps로 반환한 todo에 접근할수있다.
        	{todo}
            
            // mapDispatchToProps가 반환한 toggleTodo를 사용해
            // button을 눌러 dispatch하게 만들어줄수있다.
            
            <button onClick={() => toggleTodo()} />
        </div>
    )
}

const mapStateToProps = (state, ownProps) => ({
  todo: state.todos[ownProps.id]
  })
  
const mapDispatchToProps = (dispatch) => {
  toggleTodo: action => dispatch(toggleTodo(action))
  }
  
// 반드시 connect로 mapStateToProps, mapDispatchToProps를 넘겨주어야
// mapStateToProps와 mapDispatchToProps가 반환한 객체에 TodoApp컴포넌트가
// this.props로 접근할수있다.
 
 export default connect(mapStateToProps, mapispatchToProps)(TodoApp);
 ```