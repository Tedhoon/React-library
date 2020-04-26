# setState

## setState에서 prevState받기
setState안에 함수를 넣어주고, return값을 주게되면 이전 state를 사용할 수 있다.
```js
...
this.setState((prevState)=>{
    return {
        prevValue : prevState.value;
    }
})
```

<br>

> 정리 나중에 할려고 일단 여기에 씁니다.
## DOM 접근하기[Ref]
1. class기반
```js
onChange = (e) => {
    this.targetInput.focus();
}
...
targetInput;
render() {
    return (
        <input ref={(c)=>{this.targetInput = c;}}>
        // 이런식으로 ref를 넣어주게 되면 this."~"의 "~"를 선언하고 사용할 수 있음.
    )
}
```


2. hooks
```js
const targetInput = React.useRef('초기값');
// useRef 사용해서 비구조화 할당(구조분해)

const onChangeSubmit = (e) => {
    targetInput.current.focus();
    // current를 써줘야한다.
}

...

render() {
    return (
        <input ref={targetInput} onChange={onChangeSubmit}>
    )
}
```