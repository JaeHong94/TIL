# export

export문은 JavaScript모듈에서 함수, 객체, 원시 값을 내보낼 때 사용합니다. 내보낸 값은 다른 프로그램에서 import문으로 가져가 사용할 수 있습니다.

## 사용법

```js
export default function () {
  return Math.floor(Math.random() * 10);
}
```

default로 작성하여 내보낼 때는 function에 이름을 작성하지 않아도 됩니다. import 할 때 원하는 이름으로 작성해서 사용 가능합니다.  
단, default는 한 파일 안에 한 번만 작성 가능합니다.

```js 
export function random() {
  return Math.floor(Math.random() * 10);
}
export const user = {
  name: 'JaeHong',
  age: 28
}
export default function() {
  console.log('Hello World');
}
```

export에 default가 없을 경우에는 여러 번 내보낼 수 있습니다.  
default가 없는 것과 있는 것은 같이 사용할 수 있습니다.
