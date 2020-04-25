# hook에 대해 알아보자아
hook은?
기존 함수형 컴포넌트에서의 한계를 극복
- state의 사용
- lifecycle을 다룰 수 있게 지원

class방식을 지양하고 hook을 통한 함수형 코딩을 지향하려고 하기 때문에 대문을 hook으로 시작해보겠슴.

## hook의 기본 useState

기존 functional component에서 사용할 수 없었던 state를 useState로 사용할 수 있게됨

```js
import React, {useState} from 'react';

function FuncComp(props){
    // 초기값을 선언
    const numberState = useState(props.initNumber);
    // useState를 찍어보면 첫번째로 현재상태의 값이 나오고
    const number = numberState[0];
    // 두번째로는 상태를 변화시킬 수 있는 함수가 등록된다.
    const setNumber = numberState[1];

    // 축약형
    const [number, setNumber] = useState(props.initNumber);

    return (
        <div>
            <p>Number : {number}</p>
            <input type="button" value="random" onClick={
                function(){
                    setNumber(Math.random());
                }
            }></input>
        </div>
    )
}
```

## hook의 기본 useEffect
functional Component에서 class Component의 LifeCycle을 다룰 수 있게 해주는 친구

#### 1. componentDidMount, componentDidUpdate

```js
import React, {useState, useEffect} from 'react';

function FuncComp(props){
    const [number, setNumber] = useState(props.initNumber);

    // useEffect의 첫 번째 인자 -> 함수
    useEffect(function(){
        console.log("요친구는 componentDidMount와 componentDidUpdate와 같이 실행됨")
        // 첫 render()직후, 그 다음 render()직후
    })

    // useEffect(function(){
    //     console.log("여러개 설치가능!")
    // })

    return (
        <div>
            <p>Number : {number}</p>
            <input type="button" value="random" onClick={
                function(){
                    setNumber(Math.random());
                }
            }></input>
        </div>
    )
}
```
렌더링 순서 
1. render() -> useEffect() -> render() -> useEffect()

<br>

#### 2. CleanUp(componentWillUnmount)
컴포넌트를 rerendering시킬 때 정리하는 용도로 쓸 수 있음
```js
useEffect(function(){
        console.log("요친구는 componentDidMount와 componentDidUpdate와 같이 실행됨")
        // useEffect의 함수안에 return과 함께 함수를 써준다.
        return function(){
            console.log("요친구는 componentWillUnMount와 같이 작동한다.)
        }
    })
```

렌더링 순서
1. init : render() -> useEffect()
2. rerender : render() -> useEffect() return -> useEffect() 


#### Skipping Effect

성능을 위해서 effect가 호출되는 것을 생략할 수 있게함

componentDidUpdate는 state값이 바뀔 때만 호출이 되는데, 단순한 useEffect는 이전 state와 비교하여 반영하지 않기 때문에 불필요한 effect호출이 생길 수 있다.

```js
...
const [number, setNumber] = useState(props.initNumber);

useEffect(function(){
        console.log("useEffect 실행!")
        document.title = number;
    },[number])
    // 배열안 인자의 상태가 바뀌었을 때만 해당 useEffect의 첫번 째 인자인 콜백호출!
    // 뒤의 배열안의 값의 설정을 통해 target Effect를 지정하는 용도로 쓰일 수도 있다!

return (
        <div>
            <p>Number : {number}</p>
            <input type="button" value="random" onClick={
                function(){
                    setNumber(Math.random());
                }
            }></input>
        </div>
        )
...
```
`componentDidMount만 쓰기`

단순히 useEffect의 2번째 인자로써 빈 배열[]을 전달하면 최초 1회 실행되며, return값은 부모컴포넌트에 의해 없어질 때 실행(componentWillUnMount)된다.
```js
useEffect(function(){
        console.log("useEffect 실행!")
        document.title = number;
    },[])
```