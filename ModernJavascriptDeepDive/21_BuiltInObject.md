# 21. 빌트인 객체

## 21.1 자바스크립트 객체의 분류

자바스크립트의 객체

- 표준 빌트인 객체
    ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공한다. 자바스크립트 실행 환경과 관계없이 언제나 사용가능하다.
    전역 객체의 프로퍼티로서 제공된다. 따라서 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.
- 호스트 객체
    ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체를 말한다.
    브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API를 호스트 객체로 사용하고 Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.
- 사용자 정의 객체
    표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

## 21.2 표준 빌트인 객체

40여개의 표준 빌트인 객체를 제공
ex) Object, String, Boolean, Number, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, Json, Error

[예제 21-01]

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj);       // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj);     // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true); // Boolean {true}
console.log(typeof boolObj);      // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // ƒ anonymous(x )
console.log(typeof func);                       // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr);        // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp);         // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();  // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object
```

[에제 21-02]

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

[예제 21-03]

```javascript
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

## 21.3 원시값과 래퍼 객체

[예제 21-04]

```javascript
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

이처럼 **문자열, 숫자, 불리언 값에 대해 객체처럼 접근하여 생성되는 임시 객체를 래퍼 객체**라 한다.

[예제 21-05]

```javascript
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

[예제 21-06]

```javascript
// ① 식별자 str은 문자열을 값으로 가지고 있다.
const str = 'hello';

// ② 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = 'Lee';

// ③ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ②에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.

// ④ 식별자 str은 새롭게 암묵적으로 생성된(②에서 생성된 래퍼 객체와는 다른) 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name); // undefined

// ⑤ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ④에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
console.log(typeof str, str); // string hello
```

[예제 21-07]

```javascript
const num = 1.5;

// 원시 타입인 숫자가 래퍼 객체인 Number 객체로 변환된다.
console.log(num.toFixed()); // 2

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof num, num); // number 1.5
```

## 21.4 전역 객체

전역 객체: 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다 먼저 생성되는 특수한 객체 어떤 객체에도 속하지 않는 최상위 객체다.

특징

1. 개발자가 의도적으로 생성할 수 없다.
2. window(또는 global)를 생략할 수 있다.

    [예제 21-09]

    ```javascript
    // 문자열 'F'를 16진수로 해석하여 10진수로 변환하여 반환한다.
    window.parseInt('F', 16); // -> 15
    // window.parseInt는 parseInt로 호출할 수 있다.
    parseInt('F', 16); // -> 15

    window.parseInt === parseInt; // -> true
    ```

3. 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
4. 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
5. var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.

    [예제 21-10]

    ```javascript
    // var 키워드로 선언한 전역 변수
    var foo = 1;
    console.log(window.foo); // 1

    // 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
    bar = 2; // window.bar = 2
    console.log(window.bar); // 2

    // 전역 함수
    function baz() { return 3; }
    console.log(window.baz()); // 3
    ```

6. let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 
    즉 .window.foo와 같이 접근할 수 없다. 

    [예제 21-11]

    ```javascript
    let foo = 123;
    console.log(window.foo); // undefined
    ```

### 21.4.1 빌트인 전역 프로퍼티

#### Infinity

무한대를 나타내는 숫자값 Infinity를 갖는다.

[예제 21-12]

```javascript
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

#### NAN

NAN프로퍼티는 숫자가 아님(Not-a-Number)을 나타내는 숫자값 NaN을 갖는다. NaN 프로퍼티는 Number, NaN 프로퍼티와 같다.

[예제 21-13]

```javascript
console.log(window.NaN); // NaN

console.log(typeof NaN);    // number
```

#### undefined

undefined 프로퍼티는 원시타입 undefined를 값으로 갖는다.

[예제 21-14]

```javascript
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

### 21.4.2 빌트인 전역 함수

#### isFinite

```javascript
/**
 * 전달받은 인수가 유한수인지 확인하고 그 결과를 반환한다.
 * @param {number} testValue - 검사 대상 값
 * @returns {boolean} 유한수 여부 확인 결과
 */

isFinite(testValue)
```

[예제 21-20]

```javascript
// 인수가 유한수이면 true를 반환한다.
isFinite(0);    // -> true
isFinite(2e64); // -> true
isFinite('10'); // -> true: '10' → 10
isFinite(null); // -> true: null → 0

// 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(Infinity);  // -> false
isFinite(-Infinity); // -> false

// 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(NaN);     // -> false
isFinite('Hello'); // -> false
isFinite('2005/12/12'); // -> false
```

#### isNaN

```javascript
/**
 * 주어진 숫자가 NaN인지 확인하고 그 결과를 반환한다.
 * @param {number} testValue - 검사 대상 값
 * @returns {boolean} NaN 여부 확인 결과
 */

isNaN(testValue)
```

[예제 21-22]

```javascript
// 숫자
isNaN(NaN); // -> true
isNaN(10);  // -> false

// 문자열
isNaN('blabla'); // -> true: 'blabla' => NaN
isNaN('10');     // -> false: '10' => 10
isNaN('10.12');  // -> false: '10.12' => 10.12
isNaN('');       // -> false: '' => 0
isNaN(' ');      // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date());            // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```

#### parseFloat

```javascript
/**
 * 전달받은 문자열 인수를 실수로 해석하여 반환한다.
 * @param {string} string - 변환 대상 값
 * @returns {number} 변환 결과
 */

parseFloat(string)
```

[예제 21-23]

```javascript
// 문자열을 실수로 해석하여 반환한다.
parseFloat('3.14');  // -> 3.14
parseFloat('10.00'); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat('34 45 66'); // -> 34
parseFloat('40 years'); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat('He was 40'); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(' 60 '); // -> 60
```

#### parseInt

```javascript
/**
 * 전달받은 문자열 인수를 정수로 해석하여 반환한다.
 * @param {string} string - 변환 대상 값
 * @param {number} [radix] - 진법을 나타내는 기수(2 ~ 36, 기본값 10)
 * @returns {number} 변환 결과
 */

parseInt(string, radix)
```

[예제 21-24]

```javascript
// 문자열을 정수로 해석하여 반환한다.
parseInt('10');     // -> 10
parseInt('10.123'); // -> 10
```

[예제 21-25]

```javascript
parseInt(10);     // -> 10
parseInt(10.123); // -> 10
```

[예제 21-26]

```javascript
// 10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10'); // -> 10
// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 2); // -> 2
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 8); // -> 8
// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 16); // -> 16
```

### 21.4.3 암묵적 전역

[예제 21-37]

```javascript
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

[예제 21-38]

```javascript
// 전역 변수 x는 호이스팅이 발생한다.
console.log(x); // undefined
// 전역 변수가 아니라 단지 전역 객체의 프로퍼티인 y는 호이스팅이 발생하지 않는다.
console.log(y); // ReferenceError: y is not defined

var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

[예제 21-39]

```javascript
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
  console.log(x + y);
}

foo(); // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않는다.
delete y; // 프로퍼티는 삭제된다.

console.log(window.x); // 10
console.log(window.y); // undefined
```