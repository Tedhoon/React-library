# 비구조화 할당

`기본`
```js
const object = { a: 1, b: 2 };

const { a, b } = object;

console.log(a); // 1
console.log(b); // 2
```

`함수의 파라미터에서의 비구조화 할당`
```js
const object = { a: 1, b: 2 };

function print({ a, b }) {
  console.log(a); // 1
  console.log(b); // 2
}

print(object);
```
`b가 주어지지 않았을 때`
```js
const object = { a: 1 };

function print({ a, b }) {
  console.log(a);
  console.log(b);
}

print(object);
// 1
// undefined
```
`그러면 기본값 설정은?`
```js
const object = { a: 1 };

function print({ a, b = 2 }) {
  console.log(a);
  console.log(b);
}

print(object);
// 1
// 2
```
`비구조화 할당시 이름 바꾸기`
> : 문자를 사용
```js
const animal = {
  name: '멍멍이',
  type: '개'
};

const { name: nickname } = animal
console.log(nickname); // 멍멍이
```

`배열 비구조화 할당`
```js
const array = [1, 2];
const [one, two] = array;

console.log(one); // 1
console.log(two); // 2
```

`배열 비구조화 할당 기본값`
```js
const array = [1];
const [one, two = 2] = array;

console.log(one);
console.log(two);
```

## 숙련됐으면 다음 레벨!

`깊은 값 비구조화 할당`
> 객체의 깊숙한 곳에 들어있는 값을 꺼내는 방법
```js
const deepObject = {
  state: {
    information: {
      name: 'velopert',
      languages: ['korean', 'english', 'chinese']
    }
  },
  value: 5
};

const { state : { information : { name, languages } }, value } = deepObject; // name과 languages가 추출

// const {
//   state: {
//     information: { name, languages }
//   },
//   value
// } = deepObject;

const extracted = {
  name,
  languages,
  value
};

console.log(extracted)
```





