# 20. Strict Mode

## 20.1 strict mode란?

strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용.
오류를 발생시킬 코드나 
최적화 작업에 문제를 일으킬 수 있는 코드에 명시적인 에러를 발생시킨다.

## 20.2 strict mode의 적용

[예제 20-02]

```javascript
'use strict';

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

코드의 선두에 'use strict'를 위치시키지 않으면 strict mode가 제대로 동작하지 않는다.

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
