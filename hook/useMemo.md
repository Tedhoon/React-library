# useMemo

Memo 는 "memoized" 를 의미하는데, 이는, 이전에 계산 한 값을 재사용한다는 의미를 가지고 있습니다.

활성 사용자 수를 세는 기능은, users 에 변화가 있을때만 세야되는건데, input 값이 바뀔 때에도 컴포넌트가 리렌더링 되므로 이렇게 불필요할때에도 호출하여서 자원이 낭비되고 있습니다.

이러한 상황에는 useMemo 라는 Hooks를 사용하면 성능을 최적화 할 수 있습니다.

useMemo는 `반환값`을 기억한다!

```js

// 생략

function App () {
    
    // 생략
    // const count = countActiveUsers(users) <<= 일케 쓰지말구!
    const count = useMemo(() => countActiveUsers(users), [users]);

    return (
        ...
    )

}
```
useMemo 의 첫번째 파라미터에는 어떻게 연산할지 정의하는 함수를 넣어주면 되고 두번째 파라미터에는 deps 배열을 넣어주면 되는데, 이 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 호출해서 값을 연산해주고, 만약에 내용이 바뀌지 않았다면 이전에 연산한 값을 재사용하게 됩니다.