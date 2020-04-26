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
