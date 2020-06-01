# Uncaught TypeError: Cannot read property

```js
class Quiz extends Component {
  componentWillMount() {
    axios.get('/thedata').then(res => {
      this.setState({items: res.data});
    });
  }

  render() {
    return (
      <ul>
        {this.state.items.map(item =>
          <li key={item.id}>{item.name}</li>
        )}
      </ul>
    );
  } 
}
```
1. Component의 state(여기서는 this.state)는 undefined 인 상태로 시작합니다.

2. 비동기적으로 데이터를 가지고 올 때, constructor(여기서는 componentWillMount 또는 componentDidMount)에서 데이터를 불러오는 것과 관계없이 component는 데이터를 불러오기 전에 최소한 한번은 렌더링을 수행합니다. Quiz가 처음 렌더링될 때, this.state.items은 undefined입니다. 결국 ItemList가 undefined인 items를 가졌다는 뜻이기 때문에 "Uncaught TypeError: Cannot read property 'map' of undefined" 에러가 발생하게 되는거죠!


이 에러는 굉장히 쉬운 방법으로 고칠 수 있습니다. 가장 간단한 방법은 constructor에서 적절한 초기값을 설정해주는 거에요!

```js
class Quiz extends Component {
  // 여기에 추가합니다.
  constructor(props) {
    super(props);

    // Quiz 자체에 state를 할당하고, items에 기본값을 줍니다.
    this.state = {
      items: []
    };
  }

  componentWillMount() {
    axios.get('/thedata').then(res => {
      this.setState({items: res.data});
    });
  }

  render() {
    return (
      <ul>
        {this.state.items.map(item =>
          <li key={item.id}>{item.name}</li>
        )}
      </ul>
    );
  }
}
```


## 그러면 Hook에서는?

Class로 작성한 Component 는 constructor 가 실행되고 나서
render 가 실행되기 때문에 재 랜더링시 render 함수에 있는 내용이 재 실행되지만,
Function(Hook)으로 작성한 Component 는 재 랜더링시, 함수 내의 모든 코드를 다시 실행한다.

하지만 이때 useState에 들어가는 초기값 재 랜더링시에 다시 0으로 갱신되지 않는다.

```js
const [state, setState] = useState(initialState);
// setState 를 호출할 때마다, 재 렌더링 된다.
```

그러면 `useEffect`에 대해 알아보자.

useEffect 는 렌더링된 이후 호출된다!(중요)
> useEffect(didUpdateFunction, [비교 Values]?);
componentDidMount, componentWillUnmount, componentWillUnmount 와 비슷하다.