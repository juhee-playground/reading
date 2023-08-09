# 26. ES6 함수의 추가 기능

## 26.1 함수의 구분

[예제 26-01]

```javascript
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수호서 호출할 수 있다.

[얘재 26-02]

```javascript
var foo = function () {};

// ES6 이전의 모든 함수는 callable이면서 constructor다.
foo(); // -> undefined
new foo(); // -> foo {}
```

> **callable과 constructor/non-constructor**
> 내부 메서드에서 살펴 본 것 다시 정리
> callable: 호출할 수 있는 함수 객체
> constructor: 인스턴스를 생성할 수 있는 함수 객체
> non-constructor: 인스턴스를 생성할 수 없는 함숙 객체

[얘제 26-03]

```javascript
// 프로퍼티 f에 바인딩된 함수는 callable이며 constructor다.
var obj = {
  x: 10,
  f: function () { return this.x; }
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

[예제 26-04]

```javascript
// 콜백 함수를 사용하는 고차 함수 map. 콜백 함수도 constructor이며 프로토타입을 생성한다.
[1, 2, 3].map(function (item) {
  return item * 2;
}); // -> [ 2, 4, 6 ]
```

|ES6 함수의 구분|constructor|prototype|super|arguments|
|------------|-----------|---------|-----|---------|
|일반 함수(Normal)|O|O|X|O|
|메서드(Method)|X|X|O|O|
|화살표 함수(Arrow)|X|X|X|X|

## 26.2 메서드

ES6 사양에서 메서드는 축약 표현으로 정의도니 함수만을 의미한다.

[예제 26-05]

```javascript
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다.

[예제 26-06]

```javascript
new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

[예제 26-07]

```javascript
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // -> true
```

[예제 26-08]

```javascript
String.prototype.toUpperCase.prototype; // -> undefined
String.fromCharCode.prototype           // -> undefined

Number.prototype.toFixed.prototype; // -> undefined
Number.isFinite.prototype;          // -> undefined

Array.prototype.map.prototype; // -> undefined
Array.from.prototype;          // -> undefined
```

ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.

[예제 26-09]

```javascript
const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  }
};

const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  }
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다. ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject를 갖지 않기 때문이다.

[예제 26-10]

```javascript
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드가 아니다.
  // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError: 'super' keyword unexpected here
    return `${super.sayHi()}. how are you doing?`;
  }
};
```

## 26.3 화살표 함수

function 키워드대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정으ㅣ방식보다 간략하게 함수를 정의할 수 있다.
함수 표현 뿐만 아니라 내부 동작도 기존의 함수보다 간략하다.
콜백 함수 내부에서 this가 **전역 객체를 가리키는 문제**를 해결하기 위한 대안으로 유용하다.

### 26.3.1 화살표 함수 정의

#### 함수 정의

함숫 선언문으로 정의할 수 없고 함수 표현식으로 정의.
호출방식은 기존 함수와 동일.

[예제 26-11]

```javascript
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

#### 메게뱐수 선언

[예제 26-12]

```javascript
// 매개변수가 여러 개인 경우 소괄호()안에 매개변수를 선언한다.
const arrow = (x, y) => { ... };
```

[예제 26-13]

```javascript
// 매개변수가 한 개인 경우 소괄호()를 생략할 수 있다.
const arrow = x => { ... };
```

[예제 26-14]

```javascript
// 매개변수가 없다면 소괄호()를 생략할 수 없다.
const arrow = () => { ... };
```

#### 함수 몸체 정의

함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략 가능하다.
내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.

[예제 26-15]

```javascript
// concise body
const power = x => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
// block body
const power = x => { return x ** 2; };
```

[예제 26-16]

```javascript
// 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.
// 표현식이 아닌 문은 반환할 수 없기 때문이다.
const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

[예제 26-17]

```javascript
// 함수 몸체가 하나의 문으로 구성된다 하더라도 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.
const arrow = () => { const x = 1; };
```

[예제 26-18]

```javascript
// 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 ()로 감싸줘야 한다.
const create = (id, content) => ({ id, content });
create(1, 'JavaScript'); // -> {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => { return { id, content }; };
```

[예제 26-19]

```javascript
// 객체 리터럴을 소괄호 ()로 감싸지 않으면
// 객체 리터럴을 중괄호 {}를 함수 몸체를 감싸는 중괄호 {}로 잘못 해석한다.
// { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => { id, content };
create(1, 'JavaScript'); // -> undefined
```

[예제 26-20]

```javascript
// 함수 몸체가 여러개의 문으로 구성된다면
// 험수 몸채룰 중괄호 {}룰 생략할 수 없다.
// 이 때 반환값이 있다면 명시적으로 반환해야 한다.
const sum = (a, b) => {
  const result = a + b;
  return result;
};
```

[예제 26-21]

```javascript
// 화살표 함수도 즉시 실행 함수로 사용할 수 있다.
const person = (name => ({
  sayHi() { return `Hi? My name is ${name}.`; }
}))('Lee');

console.log(person.sayHi()); // Hi? My name is Lee.
```

[예제 26-22]
```javascript
// 화살표 함수도 일급 객체 이므로 Array.prototype.map, Array.prototype.filter, Array.prototype.reduce 같은 고차 함수에 인수로 전달할 수 있다.
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map(v => v * 2); // -> [ 2, 4, 6 ]
```

### 26.3.2 화살표 함수와 일반 함수의 차이

01. **화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.**

    [예제 26-23]

    ```javascript
      const Foo = () => {};
      // 화살표 함수는 생성자 함수로서 호출할 수 없다.
      new Foo(); // TypeError: Foo is not a constructor
    ```

    [예제 26-24]
    
    ```javascript
      const Foo = () => {};
      // 화살표 함수는 prototype 프로퍼티가 없다.
      Foo.hasOwnProperty('prototype'); // -> false
    ```

02. **중복된 매개변수 이름을 선언할 수 없다**

    [예제 26-25]

    ```javascript
      function normal(a, a) { return a + a; }
      console.log(normal(1, 2)); // 4
    ```

    [예제 26-26]

    ```javascript
    // 일반 함수도 strict 모드에서 중복된 매개변수 이름 사용할 수 없다.
      'use strict';
      
      function normal(a, a) { return a + a; }
      // SyntaxError: Duplicate parameter name not allowed in this context
    ```
    
    [에제 26-27]
    
    ```javascript
    // 화살표 함수에서 중복된 매개변수 이름을 사용하면 에러 발생.
      const arrow = (a, a) => a + a;
      // SyntaxError: Duplicate parameter name not allowed in this context
    ```
03. **화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
    띠리사 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면
    스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.

    만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this, arguments, super, new.target 바인딩이 없으므로
    스코프 체인 상의 가장 가까운 상위 함수 중에서 화살효 함수가 아닌 함수의 this, arguments, super, new.target을 참조한다.

### 26.3.3 this

화살표 함수기 일반 함수와 구별되는 가장 큰 특징이 바로 this다.
콜백 함수 내부의 this 문제. 콜백 함수의 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것이다.

this 바인딩은 함수의 호출 방식, 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.
함수를 호출할 때 함수가 어떻게 호출되엇는지에 따라 this 바인딩할 객체가 동적으로 결정된다.

[예제 26-28]

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    // ①
    return arr.map(function (item) {
      return this.prefix + item; // ②
      // -> TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

프로토타입 메서드 내부인 ①에서 this는 메서드를 호출한 객체(prefixrer 객체)를 가리킨다.
그런데 Array.prototype.map의 인수로 전달한 콜백 함수의 내부인 ②에서 this는 undefined를 가리킨다.
Array.prototype.map 메서드가 콜백 함수를 일반 함수로서 호출하기 때문이다.

클래슨 내부의 모든 코드는 strict mode가 암묵적으로 적용된다. 따라서 Array.prototype.map 메서드의 콜백 함수에도 strict mode가 적용된다.
strict mode에서 일반 함수로서 호출된 모든 함수 내부의 this에는 전역 객체가 아니라 undefined가 바인딩되므로
일반 함수로서 호출되는 Array.prototype.map 메서드의 콜백 함수 내부의 this에는 undeifned가 바인딩된다.

이 때 발생하는 문제가 콜백 함수 내부의 this 문제다. ①과 ②가 서로 다른 값을 가리키고 있어서 TypeError가 발생한다.

**이를 해결하기 위한 방법들**

01. **add 메서드를 호출한 prefixer 객체를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다.**
   [예제 26-29]
    ```javascript
      ...
      add(arr) {
        // this를 일단 회피시킨다.
        const that = this;
        return arr.map(function (item) {
          // this 대신 that을 참조한다.
          return that.prefix + ' ' + item;
        });
      }
      ...
    ```
02. ** Array.prototpye.map의 두번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다.**
    [예제 26-30]

    ```javascript
      ...
      add(arr) {
        return arr.map(function (item) {
          return this.prefix + ' ' + item;
        }, this); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
      }
      ...
    ```
03. **Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩 한다.**
   [에제 26-31]
  ```javascript
    ...
    add(arr) {
      return arr.map(function (item) {
        return this.prefix + ' ' + item;
      }.bind(this)); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
    }
    ...
  ```

ES6에서는 화삻표 함수를 사용하여 "콜백 함수 내부의 this 문제"를 해결할 수 있다.

[예젲 26-32]

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(item => this.prefix + item);
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```

**화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라 한다.**

