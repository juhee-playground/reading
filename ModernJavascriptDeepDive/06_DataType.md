# 6. 데이터 타입

원시타입: Number, string, boolean, undefined, null, symbol
객체타입: 객체, 함수, 배열 등 위의 여섯가지 빼고 다

## 6.1 숫자타입(Number)

숫자, 정수와 실수 구분없이 하나의 타입만이 존재한다.
64비트 부동 소수점 형식을 따른다. 즉 모든 수를 실수로 처리한다.

```javascript
// 숫자타입은 모두 실수로 처리된다.
console.log(1 === 0.1); // true
console.log(4 / 2);     // 2
console.log(3 / 2);     // 1.5
```

숫자타입은 특별한 값도 표현 가능하다.

- Infinity : 양의 무한대
- -Infinity : 음의 무한대
- NaN : 산술 연산 불가(not-a-number)

```javascript
// 숫자 타입의 세 가지 특별한 값
console.log(10 / 0);    // Infinity
console.log(10 / -0);   // -Infinity
console.log(10 * "1");  // NaN
```

## 6.2 문자타입(String)

```javascript
var string;

string = '문자열';  // 작은 따옴표
string = "문자열";  // 큰 따옴표
string = `문자열`;  // 백틱(ES6)
```

## 6.4 불리언 타입

```javascript
var foo = true;

console.log(foo); // true

foo = false;

console.log(foo); // false
```

## 6.5 undefined 타입

undefined 타입은 값으로 undefined가 유일한 값이다. 

```javascript
var foo;

console.log(foo); // undefined
```

자바스크립트 엔진이 변수를 초기화 할 때 사용하는 값.
변수를 참조했을 때 undefined가 반환된다면 **초기화되지 않은 변수라는 것을 의미**한다.

## 6.6 null 타입

null 타입은 null 이 유일한 값이다.
변수에 값이 없다는 것을 의도적으로 명시.

## 6.7 심벌타입

다른 값과 중복되지 않는 유일무이한 값.

```javascript
var key = Symbol('key');

console.log(typeof key); // symbol

var obj = {};

// 이름이 충돌할 위험이 없는 유일무이한 값인 심벌을 프로퍼티 키로 사용한다.
obj[key] = 'value';
console.log(obj[key]); // value
```

## 6.8 객체타입

자바스크립트를 이루고 있는 거의 모든 것이 객체다.

## 6.9 데이터타입의 필요성

- 값을 저장할 때 확보해야 하는 **메모리 공간의 크기**를 결정하기 위해
- 값을 참조할 때 한 번에 읽어 들여야 할 **메모리 공간의 크기**를 결정하기 위해
- 메모리에서 읽어 들인 **2진수를 어떻게 해석**할지 결정하기 위해

## 6.10 동적타이핑

자바스크립트

- 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)된다.
- 동적타입 언어(파이썬, php, 루비, 리스프, 펄)
- 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.(동적타이핑)
