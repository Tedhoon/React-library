# hooks에 대해 알아보자아

## hooks의 기본 useState

기존 functional에서 사용할 수 없었던 state를 useState로 사용할 수 있게됨

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