# 19. 프로토타입

//TODO: ch.25 클래스랑 연동하기
> **클래스(Class)**
> ES6에서 클래스가 도입되었다. 하지만 ES6의 클래스가 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새로운 객체지향 모델을 제공하는 것은 아니다. 사실 클래스도 함수이며, 기존 프로토타입 기반 패턴 문법적 설탕 이라고 볼 수 있다.
> 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하여 클래스는 생성자 함수에서는 제공하지 않는 기능도 제공한다.
> 따라서 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕으로 보기보다는 새로운 객체 생성 매커니즘으로 보는것이 좀 더 합당하다고 할 수 있다.

자바스크립트는 객체 기반의 프로그래밍 언어이며 **자바스크립트를 이루고 있는 거의 "모든 것"이 객체다.**

## 19.1 객체지향 프로그래밍

객체지향 프로그래밍은 실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작. 실체는 특징이나 성질을 나타내는 **속성**을 가지고 있다.

사람에게는 다양한 속성이 있으나 우리가 구현하려는 프로그램에서는 사람의 이름과 주소라는 속성만 관심이 있다. 다양한 속성 중에서 필요한 속성만 간추려 내어 표현 하는 것이 **추상화**라 한다.

[예제 19-01]

```javascript
// 이름과 주소 속성을 갖는 객체
const person = {
  name: 'Kim',
  address: 'Seoul'
};

console.log(person); // {name: "Kim", address: "Seoul"}
```

**속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**를 객체라 한다.

원이라는 개념을 만들고 원에는 반지름이라는 속성이 있다. 이 반지름을 가지고 지름, 둘레, 넓이를 구할 수 있다. 이 때 반지름은 원의 **상태를 나타내는 데이터**이며 원의 지름, 둘레, 넓이를 구하는 것은 **동작**이다.

[예제 19-02]

```javascript
const circle = {
  radius: 5, // 반지름

  // 원의 지름: 2r
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레: 2πr
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이: πrr
  getArea() {
    return Math.PI * this.radius ** 2;
  }
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter());  // 10
console.log(circle.getPerimeter()); // 31.41592653589793
console.log(circle.getArea());      // 78.53981633974483
```

객체지향 프로그래밍은 객체의 **상태**를 나타내는 데이터와 상태 데이터를 조작할 수 있는 **동작**을 묶어서 생각한다.
따라서 객체는 **상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조**라고 할 수 있다.

## 19.2 상속과 프로토타입

상속은 객체지향 프로그래밍의 핵심 개념이다. 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

[예제 19-03]

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

![그림 19-1 메서드 중복 생성](../images/19-1.png)

상속을 통해 불필요한 중복을 제거 하고 **자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

[예제 19-04]

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

![그림 19-2 상속에 의한 메서드 공유](../images/19-2.png)

## 19.3 프로토타입 객체

![그림 19-3 객체와 프로토타입과 생성자 함수는 서로 연결되어 있다.](../images/19-3.png)

### 19.3.1 __proto__ 접근자 프로퍼티

[예제 19-05]

```javascript
const person = { name: 'Lee' };
```

![그림 19-4 크롬 브라우저의 콘솔에서 출력한 객체의 프로퍼티](../images/19-4.png)

**__proto__는 접근자 프로퍼티다.**

![그림 19-5 Object.prototype.__proto__는 접근자 프로퍼티다.](../images/19-5.png)

[예제 19-06]

```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

**__proto__는 접근자 프로퍼티는 상속을 통해 사용된다.**

접근자 프로퍼티는 객체가 **직접 소유하는** 프로퍼티가 아니다.
Object.prototype의 프로퍼티다.
모든 객체는 상속을 통해 Object.prototype.__proto__접근자 프로퍼를 사용할 수 있다.

[예제 19-07]

```javascript
const person = { name: 'Lee' };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty('__proto__')); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

**__proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

[예제 19-08]


```javascript
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

![그림 19-6 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인](../images/19-6.png)

프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.

**__proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장 하지 않음**

[예제 19-09]

```javascript
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 __proto__보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

[예제 19-10]

```javascript
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 19.3.2 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

[예제 19-11]

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype'); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // -> false
```

prototype 프로퍼티는 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킨다.
non-constructor인 화살표 함수와 ES6 메서드 축약표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

[예제 19-12]

```javascript
// 화살표 함수는 non-constructor다.
const Person = name => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {}
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype으로부터 상속받은) __proto__ 접근자 프로퍼티와 
함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.

|구분|소유|값|사용주체|사용목적|
|---|---|-|------|-----------------------|
|__proto__ 접근자 프로퍼티|모든객체|프로토타입의 참조|모든 객체|객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용|
|prototype 프로퍼티|constructor|프로토타입의 참조|생성자 함수|생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용|

[예제 19-13]

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Baek');

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__);  // true

![그림 19-7 객체의 __proto__접근자 프로퍼티와 함수 객체의 prototype 프로퍼티는 결국 동일한 프로토타입을 가진다.](../images/19-7.png)

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

[예제 19-14]

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Baek');

// me 객체의 생성자 함수는 Person이다.
// me 객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용
console.log(me.constructor === Person);  // true
```

![그림 19-8 프로토타입의 constructor 프로퍼티](../images/19-8.png)

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

[예제 19-15]

```javascript
// obj 객체를 생성한 생성자 함수는 Object다.
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person('Baek');
console.log(me.constructor === Person); // true
```

[예제 19-16]

```javascript
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) { return a + b; };

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexp = /is/ig;
```

[예제 19-17]

```javascript
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

![그림 19-9 Object 생성자 함수](../images/19-9.png)
[참조](https://262.ecma-international.org/11.0/#sec-object-objects)


[예제 19-18]

```javascript
// 2. Object 생성자 함수에 의한 객체 생성
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Object();
console.log(obj); // {}

// 1. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 3. 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}

// String  객체 생성
obj = new Object('123');
console.log(obj); // String {"123"}
```

![그림 19-10 객체 리터럴의 평가](../images/19-10.png)
[참조](https://262.ecma-international.org/11.0/#sec-object-initializer-runtime-semantics-evaluation)

[예제 19-19]

```javascript
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```

프로토타입과 생성자함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

|리터럴 표기법|생성자 함수|프로토타입|
|----------|--------|-------|
|객체 리터럴|Object|Object.prototype|
|함수 리터럴|Function|Function.prototype|
|배열 리터럴|Array|Array.prototype|
|정규 표현식 리터럴|RegExp|RegExp.prototype|

## 19.5 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

[예제 19-20]

```javascript
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. 
따라서 함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다.
이 때 프로토타입도 더불어 생성된다.

[예제 19-21]

```javascript
// 화살표 함수는 non-constructor다.
const Person = name => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
// 에러 발생: Uncaught SyntaxError: Identifier 'Person' has already been declared
```

![그림 19-12 Person.prototype의 프로토타입](../images/19-12.png)

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

![그림 19-13 Object 생성자함수와 프로토타입](../images/19-13.png)

객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화가 되어 존재한다.
이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당된다.

## 19.6 객체 생성 방식과 프로토타입 결정

객체 생성 방법

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

[예제 19-23]

```javascript
const obj = { x: 1 };
```

![그림 19-14 객체 리터럴에 의해 생성된 객체의 프로토타입](../images/19-14.png)

[예제 19-24]

```javascript
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

[예제 19-25]

```javascript
const obj = new Object();
obj.x = 1;
```

![그림 19-15 Object 생성자 함수에 의해 생성된 객체의 프로토타입](../images/19-15.png)

[예제 19-26]

```javascript
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

[예제 19-27]

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Baek');
```

![그림 19-16 생성자 함수에 의해 생성된 객체의 프로토타입](../images/19-16.png)

[예제 19-28]

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();  // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

![그림 19-17 프로토타입의 확장과 상속](../images/19-17.png)

## 19.7 프로토타입 체인

