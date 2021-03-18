# this

JavaScript에서 가장 혼란스러운 개념중에 하나인 this입니다. this의 값은 함수를 호출하는 방법에 의해 결정됩니다.<br>

## 상황에 따라 달라지는 this

this는 실행 컨텍스트가 생성될 때 함께 결정되고, 함수를 호출할 때 결정됩니다. 또한 함수를 어떤 방식으로 호출하느냐에 따라 값이 달라지는 것입니다.

### 전역 공간에서의 this, 일반 함수 호출 this

``` js
var num = 1;
console.log(num);         // 1
console.log(window.num);  // 1
console.log(this.num);    // 1

window.num = 2;
console.log(num);         // 2
console.log(window.num);  // 2
console.log(this.num);    // 2
```
전역 공간에서의 this는 **전역변수를 선언하면 자바스크립트 엔진은 이를 전역객체의 프로퍼티로 할당합니다.**<br>

```js
var book = 'JS';
console.log(this.book);  // JS

function whatBook() {
this.book2 = 'Java';
console.log(this);
}

console.log(this.book);  // JS
console.log(this.book2); // undefined

window.book = 'C';

console.log(this.book);  // C
console.log(this.book2); // undefined

whatBook();              // window

console.log(this.book);  // C
console.log(this.book2); // Java
```
여기서 알 수 있는 점은 일반 함수로 호출하면 함수 내부의 this는 전역 객체로 바인딩하는 것을 알 수 있다.<br>
하지만 'use strict'모드(엄격한 모드)를 적용하게되면 일반 함수 내부의 this는 undefinde가 바인딩됩니다.

```js
function foo() {
  'use strict';

  console.log("foo's this: ", this);
  function bar() {
    console.log("bar's this: ", this);
  }
  bar(); // undefined
}

foo();   // undefined
```
<br>
다음으로 살펴볼 것은 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this도 전역 객체로 바인딩 되는가이다.

```js
var book = 'JS';

const list = {
  book: 'Java',
  foo() {
    console.log("foo's this: ", this);
    console.log("foo's this.book: ", this.book);

    function bar() {
      console.log("bar's this: ", this);
      console.log("bar's this.book: ", this.book);
    }

    bar();
  },
};

list.foo();
```
위와 같이 this는 어떠한 함수라도 일반 함수로 호출되면 전역 객체가 바인딩됩니다.

### 메소드를 이용한 방식, .(Dot Notation) 방식

메서드 호출 시 메서드 내부에 this는 호출한 객체에 바인딩됩니다.

```js
const library = {
  book: 'JS',
  getBook() {
    return this.book;
  },
};

console.log(library.getBook()); // JS
```
library 객체에서 getBook() 함수를 정의하고 출력했다. 출력할 때 .(Dot Notation) 방식을 이용해서 호출하였기 때문에 library 객체에 바인딩되고 this.book은 library.book과 똑같은 의미가된다.<br><br>

다음 예제에서는 foo라는 변수에 getBook메서드를 할당하고 일반 함수로 호출했을 경우이다.

```js
function getBook() {
  console.log(this.book);
}

const library = {
  book: 'JS',
  getBook: getBook,
};

library.getBook(); // JS

const foo = library.getBook;

foo();             // undefined
```
foo() 함수는 .(Dot Notation) 방식이 아닌 일반 함수로 호출하였기 대문에 전역 변수 값으로 출력됩니다.

```js
function Library(book) {
  this.book = book;
}

Library.prototype.getBook = function () {
  return this.book;
};

const list = new Library('React');

console.log(list.getBook());              // React

Library.prototype.book = 'Vue.js';

console.log(Library.prototype.getBook()); // Vue.js
```
.(Dot Notation) 방식, 메소드를 이용한 방식은 어떤객체를 호출하느냐에 따라 바인딩 장소가 결정됩니다.

### 생성자 함수 호출, new 키워드

생성자는 new를 사용하여 객체를 만들어 사용하는 방식입니다. 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩됩니다.

```js
function library(book, page) {
  this.book = book;
  this.page = page;
  this.bookInformation = function () {
    return `책의 제목은 ${this.book}이고, 페이지수는 ${this.page} 입니다.`;
  };
}

const book1 = new library('javascript', 255);
const book2 = library('java', 255);

console.log(book1.bookInformation()); // 책의 제목은 javascript이고, 페이지수는 255 입니다.
console.log(book1);                   // library { book: 'javascript', page: 255, bookInformation: [function (anonymous)]
console.log(book2);                   // undefined
```
book2는 생성자 함수로 호출하지 않았기 때문에 일반 함수로 동작하게 됩니다. 

### apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드입니다. 정리하자면 이들 메서드는 모든 함수가 상속받아 사용할 수 있습니다.

```js
Function.prototype.apply(thisArg,[ argsArray]);
```
- thisArg: this로 사용할 객체입니다.
- argsArray: 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체입니다.

```js
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]]);
```
- thisArg: this로 사용할 객체입니다.
- arg1, arg2, ...: 함수에게 전달할 인수 리스트입니다.

aplly와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작합니다.
```js
function getThisBinding() {
  console.log(arguments);
  return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}
console.log(getThisBinding.call(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}
```

apply와 call 메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우입니다.
```js
function convertArgsToArray() {
  console.log(arguments); // [Arguments] { '0': 1, '1': 2, '2': 3 }

  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}

convertArgsToArray(1, 2, 3); // [ 1, 2, 3]
```

Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않고 this로 사용햘 객체만 전달합니다.
```js
function getThisBinding() {
  return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.bind(thisArg));   // f getThisBinding
console.log(getThisBinding.bind(thisArg)()); // { a: 1 }
```
bind 메서드는 함수를 호출하지 않기때문에 명시적으로 호출을 해주어야 합니다.<br><br>

bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용됩니다.
```js
const book = {
  title: 'JS',
  foo(callback) {
    // ⑴
    setTimeout(callback, 100);
  },
};

book.foo(function () {
  console.log(`The title of this book is ${this.title}`); // ⑵
});
```
⑴번의 시점에서는 this가 book 객체를 가르키지만 ⑵번의 시점에서는 book.foo가 일반 함수로 호출되었기 때문에 this는 전역 객체 window를 가르키게됩니다. 이를 해결하기 위해서 bind 메서드를 사용하여 this를 일치시킬 수 있습니다.
```js
const book = {
  title: 'JS',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달합니다.
    setTimeout(callback.bind(this), 100);
  },
};

book.foo(function () {
  console.log(`The title of this book is ${this.title}`); // The title of this book is JS
});
```
