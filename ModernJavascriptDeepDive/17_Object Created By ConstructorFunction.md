# 17. 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

[예제 17-01]

```javascript
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Juhee';
person.sayHello = function () {
  console.log('Hi! My name is ' + this.name);
};

console.log(person); // {name: "Juhee", sayHello: ƒ}
person.sayHello(); // Hi! My name is Juhee
```

[예제 17-02]

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Baek');
console.log(typeof strObj); // object
console.log(strObj);        // String {"Baek"}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(numObj);        // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj);        // Boolean {true}

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x');
console.log(typeof func); // function
console.dir(func);        // ƒ anonymous(x)

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr); // object
console.log(arr);        // [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp);        // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date);        // Mon May 04 2020 08:36:33 GMT+0900 (대한민국 표준시)
```

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

[예제 17-03]

```javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter()); // 20
```

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

[예제 17-04]

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5);  // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

> #### this
> 
> // TODO: 22장에 연결하기 나중에....
> this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
> | 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
> |---------------|---------------------------------|
> |일반 함수로서의 호출|전역 객체|
> |메서드로 호출|메서드를 호출할 객체|
> |생성자 함수로서 호출|생성자 함수가 생성할 인스턴스|
>
> [예제 17-05]
>
> ```javascript
>    // 함수는 다양한 방식으로 호출될 수 있다.
>    function foo() {
>    console.log(this);
>    }
>
>    // 일반적인 함수로서 호출
>    // 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
>    foo(); // window
>
>    // 메서드로서 호출
>    const obj = { foo }; // ES6 프로퍼티 축약 표현
>    obj.foo(); // obj
>
>    // 생성자 함수로서 호출
>    const inst = new foo(); // inst
>    ```

new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.

[예제 17-06]

```javascript
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

**인스턴스를 생성**하는 것과 **생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기화 할당)**하는 것이다.
생성자 함수가 인스턴스를 생성하는 것은 필수이고 생성된 인스턴스를 초기화하는 것은 옵션이다.

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

[예제 17-07]

```javascript
// 생성자 함수
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);  // 반지름이 5인 Circle 객체를 생성
```

1. 인스턴스 생성과 this 바인딩
   > 바인딩
   > 식별자와 값을 연결하는 과정.
   > 예를 들어 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩 하는 것이다.
   > this 바인딩은 this(키워드로 분류되지만식별자 역활을 한다)와 this가 가리키는 객체를 바인딩하는 것이다.

   [예제 17-08]

    ```javascript
    function Circle(radius) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this); // Circle {}

        this.radius = radius;
        this.getDiameter = function () {
            return 2 * this.radius;
        };
    }
    ```

2. 인스턴스 초기화
    [예제 17-09]

    ```javascript
    function Circle(radius) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.radius = radius;
        this.getDiameter = function () {
            return 2 * this.radius;
        };
    }
    ```

3. 인스턴스 반환
   [예제 17-10]

    ```javascript
        function Circle(radius) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.radius = radius;
        this.getDiameter = function () {
            return 2 * this.radius;
        };

        // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다
        }

        // 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
        const circle = new Circle(1);
        console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
    ```

    만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

    [예제 17-11]

    ```javascript
        function Circle(radius) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.radius = radius;
        this.getDiameter = function () {
            return 2 * this.radius;
        };

        // 3. 암묵적으로 this를 반환한다.
        // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
        return {};
        }

        // 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
        const circle = new Circle(1);
        console.log(circle); // {}
    ```

    하지만 명시적으로 원시값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

    [예제 17-12]

    ```javascript
        function Circle(radius) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.radius = radius;
        this.getDiameter = function () {
            return 2 * this.radius;
        };

        // 3. 암묵적으로 this를 반환한다.
        // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.
        return 100;
        }

        // 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
        const circle = new Circle(1);
        console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
    ```

    생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본동작을 훼손한다. 따라서 **생성자 함수 내부에서 return 문은 반드시 생략해야 한다.**

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

[예제 17-13]

```javascript
// 함수는 객체다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop = 10;

// 함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function () {
  console.log(this.prop);
};

foo.method(); // 10
```

함수는 객체이지만 일반 객체와는 다르다. **일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.**

[예제 17-14]

```javascript
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

![그림 17-1 모든 함수 객체는 callable이지만 모든 함수 객체가 constructor인 것은 아니다](../images/17-1.png)

### 17.2.5 constructor와 non-constructor의 구분

- constructor: 함수 선언문, 함수 표현시그 클래스(클래스도 함수다)
- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

[예제 17-15]

```javascript
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo();   // -> foo {}
new bar();   // -> bar {}
new baz.x(); // -> x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만을 메서드로 인정한다.
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

[예제 17-16]

```javascript
function foo() {}

// 일반 함수로서 호출
// [[Call]]이 호출된다. 모든 함수 객체는 [[Call]]이 구현되어 있다.
foo();

// 생성자 함수로서 호출
// [[Construct]]가 호출된다. 이때 [[Construct]]를 갖지 않는다면 에러가 발생한다.
new foo();
```

### 17.2.6 new 연산자

[예제 17-17]

```javascript
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
  return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Kim', 'admin');
// 함수가 생성한 객체를 반환한다.
console.log(inst); // {name: "Kim", role: "admin"}
```

[예제 17-18]

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

### 17.2.7 new.target

new target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다. 

**new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.**
**new 연산자와 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.**

[예제 17-19]

```javascript
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());  // 10
```

> #### 스코프 세이프 생성자 패턴
>
> new.target을 사용할 수 없는 상황이라면 스코프 세이프 생성자 패턴을 사용할 수 있다.
> IE 에서는 new.target을 지원하지 않는다
> [예제 17-20]
>
> ```javascript
>    // Scope-Safe Constructor Pattern
>    function Circle(radius) {
>       // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고
>       // this에 바인딩한다. 이때 this와 Circle은 프로토타입에 의해 연결된다.
>
>       // 이 함수가 new 연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 window를 가리킨다.
>       // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
>       //TODO: 19장 instance of랑 연결 시키기
>       if (!(this instanceof Circle)) {
>           // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
>           return new Circle(radius);
>       }
>
>       this.radius = radius;
>       this.getDiameter = function () {
>           return 2 * this.radius;
>       };
>    }
>
>    // new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
>    const circle = Circle(5);
>    console.log(circle.getDiameter()); // 10
>```

참고로 대부분 빌트인 생성자 함수(Object, String, Number, Boolean, Function, Array, Date, RegExp, Promise 등)는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.

예를 들어 Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 떄와 동일하게 동작.

[예제 17-21]

```javascript
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function('x', 'return x ** x');
console.log(f); // ƒ anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f); // ƒ anonymous(x) { return x ** x }
```

하지만 String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때 String, Number, Boolean 객체를 생성하여 반환하지만 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다.

[예제 17-22]

```javascript
const str = String(123);
console.log(str, typeof str); // 123 string

const num = Number('123');
console.log(num, typeof num); // 123 number

const bool = Boolean('true');
console.log(bool, typeof bool); // true boolean
```

