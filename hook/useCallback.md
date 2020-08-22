# useCallback

useMemo는 특정 `결과값`을 재사용 할 때 사용하는 반면, useCallback 은 특정 `함수`를 새로 만들지 않고 재사용하고 싶을때 사용합니다.

useCallback은 `함수자체`를 기억한다!


✔ 자식에게 함수를 넘겨줄때는 useCallback 필수!
- 리렌더링이 발생할 때마다 자식 컴포넌트에 넘어가는 함수 역시 리렌더링 되면서 자식 컴포넌트는 넘어오는 props가 바뀐다고 생각해서 자기 자신도 쭈-욱 리렌더링을 시키기 때문이다!!! 

```js

import React, { useRef, useState, useMemo, useCallback } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

function countActiveUsers(users) {
  console.log('활성 사용자 수를 세는중...');
  return users.filter(user => user.active).length;
}

function App() {
  const [inputs, setInputs] = useState({
    username: '',
    email: ''
  });
  const { username, email } = inputs;
  const onChange = useCallback(
    e => {
      const { name, value } = e.target;
      setInputs({
        ...inputs,
        [name]: value
      });
    },
    [inputs]
  );
  const [users, setUsers] = useState([
    {
      id: 1,
      username: 'tedhoon',
      email: 'public.tedhoon@gmail.com',
      active: true
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com',
      active: false
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com',
      active: false
    }
  ]);

  const nextId = useRef(4);
  const onCreate = useCallback(() => {
    const user = {
      id: nextId.current,
      username,
      email
    };
    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: ''
    });
    nextId.current += 1;
  }, [users, username, email]);

  const onRemove = useCallback(
    id => {
      // user.id 가 파라미터로 일치하지 않는 원소만 추출해서 새로운 배열을 만듬
      // = user.id 가 id 인 것을 제거함
      setUsers(users.filter(user => user.id !== id));
    },
    [users]
  );
  const onToggle = useCallback(
    id => {
      setUsers(
        users.map(user =>
          user.id === id ? { ...user, active: !user.active } : user
        )
      );
    },
    [users]
  );
  const count = useMemo(() => countActiveUsers(users), [users]);
  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} onToggle={onToggle} />
      <div>활성사용자 수 : {count}</div>
    </>
  );
}

export default App;

```
이 함수들은 컴포넌트가 리렌더링 될 때 마다 새로 만들어집니다. 함수를 선언하는 것 자체는 사실 메모리도, CPU 도 리소스를 많이 차지 하는 작업은 아니기 때문에 함수를 새로 선언한다고 해서 그 자체 만으로 큰 부하가 생길일은 없지만, 한번 만든 함수를 필요할때만 새로 만들고 재사용하는 것은 여전히 중요합니다.

주의 하실 점은, 함수 안에서 사용하는 상태 혹은 props 가 있다면 꼭, deps 배열안에 포함시켜야 된다는 것 입니다. 만약에 deps 배열 안에 함수에서 사용하는 값을 넣지 않게 된다면, 함수 내에서 해당 값들을 참조할때 가장 최신 값을 참조 할 것이라고 보장 할 수 없습니다. (의존값이 바뀌어도 함수는 리렌더링되지 않기 때문에 그 전값을 반환한다!!) props 로 받아온 함수가 있다면, 이 또한 deps 에 넣어주어야 해요.

`어떤 컴포넌트가 렌더링되고 있는지 확인`
React Devtools의 'Highlight Updates' 체크!

그런데 지금 보면, input 이 바뀔 때에도 UserList 컴포넌트가 리렌더링이 되고 있음!

이 부분은 `React.memo`를 사용해서 리소스를 줄일 수 있습니다!



## 주의사항
어떤 컴포넌트에 b 와 button 에 onClick 으로 설정해준 함수들은, 해당 함수들을 useCallback 으로 재사용한다고 해서 리렌더링을 막을 수 있는것은 아니므로, 굳이 그렇게 할 필요 없습니다.
=> 해보니까.. onClick에 들어있는 것들은 useCallback말고 React.memo를 사용해서 메모이제이션을 하는게 맞다!