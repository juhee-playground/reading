# 20. Strict Mode

## 20.1 strict mode란?

[예제 20-1]

```javascript
function foo() {
  x = 10;
}
foo();

console.log(x); // ?
```

전역스코프에 x변수의 선언을 하지 않았기 때문에 Reference error가 날 것 같지만 자바스크립트 엔진은 암묵적으로 전역 객체에 x프로퍼티를 동적 생성한다.
이러한 현상을 **암묵적 전역**이라고 한다.

strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 코드나 최적화 작업에 문제를 일으킬 수 있는 코드에 명시적인 에러를 발생시킨다.

## 20.2 strict mode의 적용

[예제 20-02]

```javascript
'use strict';

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

[예제 20-03]

```javascript
function foo() {
  'use strict';

  x = 10; // ReferenceError: x is not defined
}
foo();
```

코드의 선두에 'use strict'를 위치시키지 않으면 strict mode가 제대로 동작하지 않는다.

[예제 20-04]

```javascript
function foo() {
  x = 10; // 에러를 발생시키지 않는다.
  'use strict';
}
foo();
```

## 20.3 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용된다.

[예제 20-05]

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict';
  </script>
  <script>
    x = 1; // 에러가 발생하지 않는다.
    console.log(x); // 1
  </script>
  <script>
    'use strict';

    y = 1; // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```

위 예제처럼 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에만 한정되어 적용된다.
하지만 strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.
전역에 strict mode를 두는 것은 바람직하지 않다.
즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용.

[예제 20-06]

```javascript
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
```

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

[예제 20-07]

```javascript
(function () {
  // non-strict mode
  var lеt = 10; // 에러가 발생하지 않는다.

  function foo() {
    'use strict';

    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
}());
```

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

[예제 20-08]

```javascript
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
}());
```

### 20.5.2 변수, 함수 매개변수의 삭제

[예제 20-09]

```javascript
(function () {
  'use strict';

  var x = 1;
  delete x;
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 20.5.3 매개변수 이름의 중복

[예제 20-10]

```javascript
(function () {
  'use strict';

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```

### 20.5.4 with 문의 사용

[예제 20-11]

```javascript
(function () {
  'use strict';

  // SyntaxError: Strict mode code may not include a with statement
  with({ x: 1 }) {
    console.log(x);
  }
}());
```

## 20.6 strict mode 적용에 의한 변화

[예제 20-12]

```javascript
(function () {
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
}());
```

[예제 20-13]

```javascript
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
}(1));
```
