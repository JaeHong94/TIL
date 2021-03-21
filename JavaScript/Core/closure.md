# 클로저(Closure)

클로저는 함수와 함수가 선언된 어휘적 환경의 조합입니다. 외부함수의 실행이 종료되어도, 외부함수의 범위(scope)에 접근할 수 있는 내부함수, 즉 클로저는 자신이 샐성될 때의 환경을 기억하는 함수입니다. 클로저는 자바스크립트 고유의 개념이 아니고 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어(하스켈(Haskell), 리스프(Lisp), 얼랭(Elang), 스칼라(Scala)등) 에서 사용되는 중요한 특성입니다. 

```js
const x = 1;

function outer() {
  const x = 10;
  function inner() {
    console.log(x);
  }
  return inner;
}

const innerFunc = outer(); // ⑴
innerFunc(); // 10 - ⑵
```
⑴번에서 outer() 함수를 호출하고 inner를 반환하면서 생명주기가 끝이 납니다. outer() 함수의 호출이 종료하면 실행 컨텍스트 스택에서 제거됩니다. outer() 함수가 제거되면서 지역 변수 x도 같이 생명주기는 마감하게 됩니다. 하지만 ⑵번에서 outer() 함수의 지역 변수 x를 참조하여 10을 출력합니다. 이를 보는 것처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조할 수 있습니다. 이러한 중첩 함수를 클로저(Closure)라고 부릅니다.**<br />

```js
const x = 1;

function outer() {
  const x = 10;
  function inner() {
    const y = 20;
    console.log(y);
  }
  return inner;
}

const innerFunc = outer();
innerFunc();
```
다음 예제는 클로저와 비슷하게 생겼지만 inner() 함수에서 상위 스코프 outer() 함수에 식별자를 참조하고 있지 않아서 클로저라고 부를 수 없습니다.

### 클로저 조건

- 함수 안에 내부함수를 정의
- 내부 함수가 상위 스코프 식별자를 참조하고 있어야 합니다.
