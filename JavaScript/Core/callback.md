# 콜백 함수(Callback Function)

콜백 함수는 다른 코드에게 파라미터로 넘겨줌으로써 제어권도 같이 위임하는것을 말합니다. 콜백 함수는 **비동기**처리를 위해서 사용합니다. 비동기 처리는 콜백 함수 뿐만 아니라 Promise, Async/Await도 사용하지만 이번 장에서는 콜백함수만 다루겠습니다.

```js
const add = function (num1, num2, callback) {
  const sum = num1 + num2;
  callback(sum);
};

add(2, 5, function (result) {
  console.log(result);
}); // 7
```
add 함수를 선언하고 파라미터로 num1, num2와 callback을 받습니다. callback은 함수로 받습니다. 후에 add함수를 호출하고 2와 5을 더한 후에 sum을 전달하면서 callback 함수를 호출합니다. 그러면 6번째줄에 function (result)는 받아온 sum을 출력하게됩니다.

## 동기
- Request를 보낸 후 Response를 받아야지만 다음 동작이 이루어지는 방식입니다.
- 설계가 매우 간단하고 직관적이지만 결과가 주어질 때까지 아무것도 못하고 대기해야 하는 단점이 있습니다.
<img src="https://user-images.githubusercontent.com/74242937/111780911-61911900-88fb-11eb-885a-c3f067db3017.png" width="400px" height="300px" /><br />

## 비동기
- Request를 보낸 후 Response와는 상관없이 다음 동작이 이루어지는 방식입니다.
- 설계가 동기보다는 복잡하지만 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업을 진행할 수 있습니다. 자월을 효율적으로 사용할 수 있는 장점이 있습니다.

<img src="https://user-images.githubusercontent.com/74242937/111782405-54c0f500-88fc-11eb-8974-0f3124076ff0.png" width="400px" height="300px" /><br />

```js
function printImmediately(print) {
  print();
}

function printWithDelay(print, timeout) {
  setTimeout(print, timeout);
}

console.log('1');                             // 동기
setTimeout(() => console.log('2'), 1000);     // 비동기
console.log('3');                             // 동기
printImmediately(() => console.log('4'));     // 동기
printWithDelay(() => console.log('5'), 2000); // 비동기
// 결과 : 1 3 4 2 5
```
결과값 2와 5는 비동기처리기 때문에 요청을 한 후에 1, 3, 4를 출력한 후에 1000ms 시간이 지난후에 2가 출력하고 2000ms 시간이 지난후에 5가 출력됩니다.

## 콜백 지옥

콜백 지옥이란 비동기 함수를 중첩으로 이용할 때 콜백 함수를 중첩해서 계속 쓰게 될 때를 가리키는 말입니다. 콜백 지옥은 가독성이 매우 좋지 않습니다.

```js
setTimeout(
  (title) => {
    let bookeList = title;
    console.log(bookeList);

    setTimeout(
      (title) => {
        bookeList += ', ' + title;
        console.log(bookeList);

        setTimeout(
          (title) => {
            bookeList += ', ' + title;
            console.log(bookeList);

            setTimeout(
              (title) => {
                bookeList += ', ' + title;
                console.log(bookeList);
              },
              500,
              'Python'
            );
          },
          500,
          'Java'
        );
      },
      500,
      'C++'
    );
  },
  500,
  'Js'
);
```
책 제목을 수집하고 출력하는 코드입니다. 콜백 지옥은 실행은 정상적으로 작동하지만 들여쓰기가 과도하게 깊어지고 가독성이 떨어지므로 피해야할 코딩 입니다. 이를 해결하기 위해서 Promise,  Async/Await이 있습니다.

