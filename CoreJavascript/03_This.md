# This

This는 함수가 호출되는 방식에 따라 this 바인딩이 동적으로 결정됩니다.

## 상황에 따라 달라지는 this

1. 전역 공간에서의 this

전역객체(브라우저: window, Node.js: global)를 참조.

```javascript
console.log(this); // window
console.log(window); // window
console.log(this === window);
```

2. 메서드로 호출할 때 그 메서드 내부에서의 this

메서드 호출 주체(메서드 명 앞의 객체)를 참조.

```javascript
const obj = {
  methodA: function() { console.log(this); },
  inner: {
    methodB: function() { console.log(this); }
  }
}
obj.methodA(); // {inner: {…}, methodA: ƒ} === obj
obj.inner['methodB'](); // {methodB: ƒ} === obj.inner
```

```javascript
const person = {
  name: 'Baek',
  getName() {
    console.log(this);
    return this.name;
  }
}

console.log(person.getName); // {name: 'Baek', getName: ƒ}
```

3. 함수로서 호출할 때 그 함수 내부에서의 this

전역 객체를 참조. 메서드의 내부 함수도 동일.

```javascript
function square(number) {
  console.log(this); // window
  return number * number;
}

console.log(square(2)); // window 나오고 그 다음에 4

```

```javascript
// this를 바인딩하지 않는 화살표 함수
const person = {
  acting: function() {
    console.log(this); // {acting: ƒ}
    var eat = () => {
      console.log(this); // {acting: ƒ}
    };
    eat();

    var walk = function () {
      console.log(this); // window
    }
    walk();
  }
};

person.acting();

```

4. 콜백 함수 호출 시 그 함수 내부에서의 this

해당 콜백 함수의 제어권을 넘겨받은 함수가 정의한 바에 따름.
정의하지 않으면 전역객체를 참조.

```javascript
setTimeout(function() { console.log(this); }, 300); // window

[1,2,3,4,5].forEach(function (x) {
  console.log(this, x);  // window
});

document.body.innerHtml += '<button id="a">click</button>';
document.body.querySelector('#a').addEventListener('click', function(e) {
  console.log(this, e);  // id가 a인 element 호출
});
```

5. 생성자 함수 내부에서의 this

생성될 인스턴스를 참조.

```javascript
const Cat = function (name, age) {
  this.bark = '야옹';
  this.name = name;
  this.age = age;
};

const choco = new Cat('초코', 3);
const nabi = new Cat('나비', 1);

console.log(choco); // Cat {bark: '야옹', name: '초코', age: 3}
console.log(nabi); // Cat {bark: '야옹', name: '나비', age: 1}
```

## 명시적으로 this를 바인딩하는 방법

1. call 메서드

`Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])`

메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령.
첫번째 인자는 this를 바인딩, 이후의 인자들은 ,로 받아서 호출할 함수의 매개변수로 지정.

2. apply 메서드

`Function.prototype.apply(thisArg[, argArray])`

call과 기능적으로 완전히 동일.
첫번째 인자는 this를 바인딩, 두번째 인자를 배열로 받아서 그 배열의 요소들을 호출할 함수의 매개변수로 지정.

```javascript
function getThisBinding() {
  return this;
}
// this로 사용할 객체
const thisArg = { a :1 };

console.log(getThisBinding()); // window

console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

3. bind 메서드

`Function.prototype.bind(thisArg[, arg1[, arg2[, ...]]])`

call과 비슷하지만 즉시 호출하지 않고 넘겨 받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 하는 메서드.

```javascript
function getThisBinding() {
  return this;
}
// this로 사용할 객체
const thisArg = { a :1 };

console.log(getThisBinding.bind(thisArg)); // ƒ getThisBinding() {}
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

```javascript
const person = {
  name: 'Baek',
  foo(callback) {
    // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
    setTimeout(callback, 100);
  }
}

// window.name은 브라우저 창의 이름을 나타낸는 빌트인 프로퍼티이며 기본값은 ''이다.
person.foo(function() {
  console.log(`Hi! my name is ${this.name}`); // Hi! my name is 
});

const person2 = {
  name: 'Baek',
  foo(callback) {
    // 콜백 함수 내부의 this를 외부 함수 내부의 this와 일치시키기 위해 bind메서드를 사용한다.
    setTimeout(callback.bind(this), 100);
  }
}

person2.foo(function() {
  console.log(`Hi! my name is ${this.name}`); // Hi! my name is Baek
});
```
