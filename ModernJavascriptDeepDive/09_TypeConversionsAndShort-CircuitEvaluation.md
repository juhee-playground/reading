# 타입변환과 단축평가

## 9.2 암묵적 타입 변환

[예제 09-09]

```javascript
+''       // -> 0
+false    // -> 0
+null     // -> 0
+[]       // -> 0

// 불리언 타입
+true     // -> 1


+'string'   // -> NaN
+{}         // -> NaN
+[10, 20]   // -> NaN
+undefined  // -> NaN
```

[예제 09-11]

```javascript
if ('')    console.log('1');
if (true)  console.log('2');
if (0)     console.log('3');
if ('str') console.log('4');
if (null)  console.log('5');

// 2 4
```

## 9.4 단축평가

객체에 프로퍼티 없는거 했을 때 에러 안내는 법

[예제 09-24]
```javascript
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```

###  9.4.2 옵셔널 체이닝 연산자 ?.

왼쪽이 null 또는 undefined 이면 undefined 반환 그렇지 않으면 오른쪽 반환

[에제 09-26]

```javascript
var elem = null;

// elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```

[에제 09-27]

```javascript
var elem = null;

// elem이 Falsy 값이면 elem으로 평가되고 elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value;
console.log(value); // null
```

###  9.4.3 null 병합 연산자 ??

[예제 09-31]

```javascript
// Falsy 값인 0이나 ''도 기본값으로서 유효하다면 예기치 않은 동작이 발생할 수 있다.
var foo = '' || 'default string';
console.log(foo); // "default string"
```
[예제 09-32]

```javascript
// 좌항의 피연산자가 Falsy 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 반환한다.
var foo = '' ?? 'default string';
console.log(foo); // ""
```