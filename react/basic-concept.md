# 기본 개념 모음

## 모듈 import & export
```js
export const hello = 'hello';

export default DefaultHello;
``` 
위 두 가지의 차이는 import시 발생한다.

```js
import { hello } from '~';
// 구조 분해 필요
import DefaultHello from '~';
// 구조분해 필요 X
```