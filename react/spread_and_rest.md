# spread 와 rest

## spread
`spread 기본`
```js
const slime = {
  name: '슬라임'
};

const cuteSlime = {
  name: '슬라임',
  attribute: 'cute'
};

const purpleCuteSlime = {
  name: '슬라임',
  attribute: 'cute',
  color: 'purple'
};

👇👇👇

const slime = {
  name: '슬라임'
};

const cuteSlime = {
  ...slime,
  attribute: 'cute'
};

const purpleCuteSlime = {
  ...cuteSlime,
  color: 'purple'
};
```

`배열에서의 spread`
```js
const animals = ['개', '고양이', '참새'];
const anotherAnimals = [...animals, '비둘기'];
console.log(animals);
console.log(anotherAnimals);

// 또는 여러번 사용도 가능!

const numbers = [1, 2, 3, 4, 5];

const spreadNumbers = [...numbers, 1000, ...numbers];
console.log(spreadNumbers); // [1, 2, 3, 4, 5, 1000, 1, 2, 3, 4, 5]
```

<br>

## rest

`객체에서의 rest`
```js
const purpleCuteSlime = {
  name: '슬라임',
  attribute: 'cute',
  color: 'purple'
};

const { color, ...rest } = purpleCuteSlime;
console.log(color); // 슬라임
console.log(rest); // {name: "슬라임", attribute: "cute"}
```
rest 안에 첫번 째 인자인 name을 제외한 값이 들어있습니다.

rest는 객체와 배열에서 사용 할 때는 이렇게 비구조화 할당 문법과 함께 사용됩니다. 주로 사용 할때는 위와 같이 rest 라는 키워드를 사용하게 되는데요. 추출한 값의 이름이 꼭 rest 일 필요는 없습니다.

```js
const purpleCuteSlime = {
  name: '슬라임',
  attribute: 'cute',
  color: 'purple'
};

const { color, ...cuteSlime } = purpleCuteSlime;
console.log(color);
console.log(cuteSlime);
```


`배열에서의 rest`

```js
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [one, ...rest] = numbers;

console.log(one); // 0
console.log(rest); // [1, 2, 3, 4, 5, 6]
```

`근데 이건 안댐`
```js
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [..rest, last] = numbers;

===> Syntax ERROR
```


`활용`
```js
function sum(...rest) {
  return rest.reduce((acc, current) => acc + current, 0);
}
// reduce에서 acc는 누적값, current는 현재값, 0은 acc의 초기값

const numbers = [1, 2, 3, 4, 5, 6];
const result = sum(...numbers);
console.log(result); //21

// 여기서 sum(numbers)하믄 배열이 전달되고
// sum함수에서 ...rest로 받았기 때문에 rest가 [Array[6]]으로 들어가기 때문에 값이 제대로 안나옴
// 그래서 ...numbers로 전해주고 ...rest로 받아야
// console.log(rest)의 값이 [1,2,3,4,5,6]이 나와서 reduce가 제대로 작동한다!
```