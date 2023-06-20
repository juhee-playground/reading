# 6. 데이터 타입

## 6.1 숫자타입(Number)

```javascript
// 모두 숫자 타입이다.
var integer = 10;   // 정수
var double = 10.12; // 실수
var negative = -20; // 음의 정수

var binary = 0b01000001; // 2진수
var octal = 0o101;       // 8진수
var hex = 0x41;          // 16진수

// 표기법만 다를 뿐 모두 값은 값이다.
console.log(binary);    // 65
console.log(octal);     // 65
console.log(hex);       // 65
console.log(binary === octal);  // true
console.log(octal === hex);     // true

// 숫자타입은 모두 실수로 처리된다.
console.log(1 === 0.1); // true
console.log(4 / 2);     // 2
console.log(3 / 2);     // 1.5

// 숫자타입은 특별한 값도 표현 가능하다.
/*
- Infinity : 양의 무한대
- -Infinity : 음의 무한대
- NaN : 산술 연산 불가(not-a-number)
*/

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

```javascript
var foo;

console.log(foo); // undefined
```

자바스크립트 엔진이 변수를 초기화 할 때 사용하는 값.
변수를 참조했을 때 undefined가 반환된다면 **초기화되지 않은 변수라는 것을 의미**한다.

## 6.6 null 타입

```javascript
var foo = 'Lee';

// 이전 참조를 제거. foo변수는 더 이상 'Lee'를 참조하지 않는다.
// 유용해 보이지 않는다. 변수의 스코프를 좁게 만들어 변수 자체를 서멸시키는 편이 낫다.
foo = null;

console.log(foo); // false
```

## 6.7 심벌타입

```javascript
var key = Symbol('key');

console.log(typeof key); // symbol

var obj = {};

// 이름이 충돌할 위험이 없는 유일무이한 값인 심벌을 프로퍼티 키로 사용한다.
obj[key] = 'value';
console.log(obj[key]); // value
```

//TODO: ch33에 링크 연결하기
심벌타입은 추후 33장에서 자세히 다룰 예정

## 6.8 객체타입

자바스크립트를 이루고 ㅇ있는 거의 모든 것이 객체다.

## 6.9 데이터타입의 필요성

- 값을 저장할 때 확보해야 하는 **메모리 공간의 크기**를 결정하기 위해
- 값을 참조할 때 한 번에 읽어 들여야 할 **메모리 공간의 크기**를 결정하기 위해
- 메모리에서 읽어 들인 **2진수를 어떻게 해석**할지 결정하기 위해

## 6.10 동적타이핑

자바스크립트

- 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)된다.
- 동적타입 언어(파이썬, php, 루비, 리스프, 펄)
- 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.(동적타이핑)
