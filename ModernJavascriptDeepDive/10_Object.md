# 10. 객체 리터럴

## 10.1 객체란?

- 원시 타입의 값. 즉 원시 값은 변경 불가능한 값이지만 객체 타입의 값. 즉 객체는 변경 가능한 값.

![그림 10-1](../images/10-1.png)

- 프로퍼티 : 객체의 상태를 나타내는 값(data)
- 메서드 : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

![그림 10-1](../images/10-2.png)

## 10.2 객체 리터럴에 의한 객체 생성

객체 생성 방법

- 객체 리터럴
- Object 생성 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)



## 10.3 프로퍼티

객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
프로퍼티를 나열할 때 ,로 구분한다.

[예제 10-3]

```javascript
var person = {
    // 프로퍼티 키는 name, 프로퍼티 값은 'Baek'
    name: 'Baek',
    // 프로퍼티 키는 age, 프로퍼티 값은 20
    age: 20
};
```

## 10.4 메서드

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메소드라 부른다.
객체에 묶여 있는 함수를 의마한다.

[예제 10-11]

```javascript
var circle = {
    radius: 5, // 프로퍼티

    // 원의 지름
    getDiameter: function() {       // 메서드
        return 2 * this.radius;     // this는 circle을 가리킨다.
    }
};

console.log(circle.getDiameter());  // 10
```

## 10.5 프로퍼티 접근

- 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법
- 대괄호 프로퍼티 접근 연산자([...])를 사용하는 대괄호 표기법

[예제 10-12]

```javascript
var person = {
    name: 'Baek'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name);
// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']);
// 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.
console.log(peron.age);     // undefined
```

## 10.6 프로퍼티 값 갱신

[예제 10-16]

```javascript
var person = {
    name: 'Baek'
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person);
```

## 10.7 프로퍼티 동적 생성

[예제 10-17]

```javascript
var person = {
    name: 'Baek'
};

// person 객체에는 age 프로퍼티가 존재하지 않는다.
// 따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person);    // { name: 'Baek', age: 20 }
```

## 10.7 프로퍼티 삭제

[예제 10-18]

```javascript
var person = {
    name: 'Baek'
};
// 프로퍼티 동적 생성
person.age = 20;
// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자 address 프로퍼티를 삭제할 수 없다. 이떄 에러가 발생하지 않는다.
delete person.address;

console.log(person);    // { name: 'Baek' }
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 10.9.1 프로퍼티 축약 표현

[예제 10-19]

```javascript
var x = 1, y = 2;

var obj = {
    x: x,
    y: y
};

console.log(obj);    // {  x: 1, y: 2 }
```

[예제 10-20]

```javascript
let x = 1, y = 2;

// 프로퍼티 축약 표현
var obj = { x, y };

console.log(obj);    // {  x: 1, y: 2 }
```

### 10.9.2 계산된 프로퍼티 이름

[예제 10-21]

```javascript
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj);    // {prop-1: 1, prop-2: 2, prop-3: 3}
```

[예제 10-22]

```javascript
const prefix = 'prop';
let i = 0;

const obj = {
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i
};

console.log(obj);    // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

[예제 10-23]

```javascript
var obj = {
    name: 'Baek',
    sayHi: function() {
        console.log("Hi! " + name);
    }
};

console.log(obj.sayHi);    // Hi! Baek
```

[예제 10-24]

```javascript
const obj = {
    name: 'Baek',
    sayHi() {
        console.log("Hi! " + name);
    }
};

console.log(obj.sayHi);    // Hi! Baek
```
