# 36 디스트럭처링 할당

## 36.1 배열 디스트럭처링 할당

```javascript
const arr = [1, 2, 3];

// ES6 배열 디스트럭처링 할당
// 변수 one, two, three를 선언하고 배열 arr을 디스트럭처링하여 할당한다.
// 이때 할당 기준은 배열의 인덱스다.
const [one, two, three] = arr;

console.log(one, two, three); // 1 2 3

const [x, y] = [1, 2];

const [x1, y1]; // SyntaxError: Missing initializer in destructuring declaration

const [x2, y2] = {}; // TypeError: {} is not iterable

// 요소의 개수가 반드시 일치할 필요는 없다.
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3

// 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
// 기본값
const [i, j, k = 3] = [1, 2];
console.log(i, j, k); // 1 2 3

// 기본값보다 할당된 값이 우선한다.
const [l, m = 10, n = 3] = [1, 2];
console.log(l, m, n); // 1 2 3

// Rest 요소를 사용할 수 있다. Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 한다.
// Rest 요소
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [ 2, 3 ]
```

## 36.2 객체 디스트럭처링 할당

```javascript
// 36-10
// ES5
var person = { firstName: 'Jisoo', lastName: 'Yoon' };

var firstName = person.firstName;
var lastName  = person.lastName;

console.log(firstName, lastName); // Jisoo Yoon

// 36-11
const user = { firstName: 'Jisoo', lastName: 'Yoon' };
// ES6 객체 디스트럭처링 할당
// 변수 lastName, firstName을 선언하고 user 객체를 디스트럭처링하여 할당한다.
// 이때 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
const { lastName, firstName } = user;

console.log(firstName, lastName); // Jisoo Yoon

// 36-12
const { lastName, firstName } = { firstName: 'Jisoo', lastName: 'Yoon' };

// 36-13
// 우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러가 발생한다.
const { lastName, firstName };
// SyntaxError: Missing initializer in destructuring declaration

const { lastName, firstName } = null;
// TypeError: Cannot destructure property 'lastName' of 'null' as it is null.

// 36-14
const { lastName, firstName } = user;
// 위와 아래는 동치다.
const { lastName: lastName, firstName: firstName } = user;

// 36-15
const user = { firstName: 'Jisoo', lastName: 'Yoon' };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
// 프로퍼티 키가 lastName인 프로퍼티 값을 ln에 할당하고,
// 프로퍼티 키가 firstName인 프로퍼티 값을 fn에 할당한다.
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); // Jisoo Yoon

// 36-16 변수에 기본값을 설정할 수 있다.
const { firstName = 'Jisoo', lastName } = { lastName: 'Yoon' };
console.log(firstName, lastName); // Jisoo Yoon

const { firstName: fn = 'Jisoo', lastName: ln } = { lastName: 'Yoon' };
console.log(fn, ln); // Jisoo Yoon
```
 