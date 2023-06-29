# 12. 함수

## 12.1 함수란?

함수: 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것.

```javascript
function add (x, y) {
    return x + y;
}
```

## 12.2 함수를 사용하는 이유

- 코드의 재사용할 수 있다.
- 유지보수의 편의성 높임.
- 코드의 신뢰성을 높임.
- 코드의 가독성 향상

## 12.3 함수 리터럴

```javascript
var f = function add (x, y) {
    return x + y;
}
```

함수는 객체다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.

## 12.4 함수 정의

### 12.4.1 함수 선언문

[예제 12-05]

```javascript
// 함수 선언문
function add(x, y) {
    return x + y;
}

// 함수 참조
// console.dir은 console.log와는 달리 객체의 프로퍼티까지 출력한다.
// 단, Node.js 환경에서는 console.log 와 같은 결과가 출력된다.
console.dir(add); // f add(x.y)

// 함수 호출
console.log(add(2, 5)); // 7
```

함수 선언문

- 함수이름을 생략할 수 없다.
- 표현식이 아닌 문이다.

[예제 12-07]

```javascript
// 함수 선언문은 표현식이 아닌 문이므로 변수에 할당할 수 없다.
// 하지만 함수 선언문이 변수에 할당되는 것처럼 보인다.
var add = function add(x, y) {
    return x + y;
}

// 함수 호출
console.log(add(2, 5)); // 7
```

[예제 12-08]

```javascript
// 기명함수 리터럴을 단독으로 사용하면 함수 선언문을 해석
// 함수 선언문에서는 함수 이름을 생략할 수 없다.
function foo() {
    console.log('foo');
}

foo();

// 함수 리터럴을 피연산자로 사용하면 함수 선언문이 아니라 함수 리터럴 표현식으로 해석된다.
// 함수 리터럴에서는 함수 이름을 생략할 수 있다.

(function bar() { console.log('bar'); });
bar(); // ReferenceError: bar is not defined
```

![그림 12-5 함수 이름 bar는 함수 몸체 내에서만 참조할 수 있는 식별자이므로 함수를 호출할 수 없다](../images/12-5.png)

![그림 12-6 foo는 자바스크립트 엔진이 암묵적으로 생성한 식별자다](../images/12-6.png)

자바스크립트 엔진은 생성된 함수를 호출하기 위해함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다.

[예제 12-09]

```javascript
// 기명함수 리터럴을 단독으로 사용하면 함수 선언문을 해석
// 함수 선언문에서는 함수 이름을 생략할 수 없다.
var add = function add(x, y) {
    return x + y;
}

console.log(add(2, 5)); // 7
```

함수는 함수이름으로 호출하는 것이 아니라함수 객체를 가리키는 식별자로 호출한다.
![그림 12-7](../images/12-7.png)

### 12.4.2 함수 표현식

지바스크립트의 함수는 일급 객체다.
일급 객체: 값의 성질을 갖는 객체.

[예제 12-10]

```javascript
// 함수 표현식
var add = function (x, y) {
    return x + y;
}

console.log(add(2, 5)); // 7
```

익명함수: 함수 리터럴은 함수 이름을 생략할 수 있다.

[예제 12-11]

```javascript
// 기명 함수 표현식
var add = function foo (x, y) {
    return x + y;
}

// 함수 객체를 가리키는 식별자로 호출
console.log(add(2, 5)); // 7

// 함수 이름으로 호출하면 ReferenceError가 발생한다.
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자다.

console.log(foo(2, 5)); // ReferenceError가: foo is not defined
```

### 12.4.3 함수 생성 시점과 호이스팅

[예제 12-12]

```javascript
// 함수 참조
console.dir(add); // f add (x,y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
    return x + y;
}

// 함수 표현식
var sub = function (x, y) {
    return x - y;
}
```

sub가 에러나는 이유:
함수 선언문으로 정의한 함수(add)와 함수 표현식으로 정의한 함수(sub)의 생성 시점이 다르기 때문이다.

함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바 스크립트 고유의 특징인 **함수 호이스팅**이라고 한다.
함수 호이스팅은 함수를 호출하기 전에 반드시 함수를 선언해야 한다는 당연한 규칙을 무시하기 때문에 함수 표현식을 사용할 것을 권장한다.

변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.

함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생한다.

### 12.4.4 Function 생성자 함수

생성자 함수: 객체를 생성하는 함수를 말한다.  17장 연동

[예제 12-13]

```javascript
var add = new Function('x', 'y', 'return x + y');

console.log(add(2, 5));
```

함수 선언문이나 함수 표현식으로 생성한 함수와 Function 생성자 함수로 생성한 함수가 동일하게 작동하지 않는다.
클로저를 생성하지 않는다 등. 다르게 작동한다.

### 12.4.5 화살표 함수

화살표를 사용하여 더 간략하게 함수를 선언 할 수 있다. 항상 익명함수로 정의한다.

```javascript
const add = (x, y) => x + y;
console.log(add(2, 5));
```

표현뿐 아니라 내부 동작 또한 간략하게 작동한다.

- 생성자 함수로 사용할 없다.
- 기존 함수와 this 바인딩이 다르다
- prototype 프로퍼티가 없고
- arguments 객체를 생성하지 않는다.
  
// TODO: 26.3 에서 더 자세히 다루니까 공부하고 연결시키기

## 12.5 함수 호출

### 12.5.1 매개변수와 인수

[예제 12-16]

```javascript
// 함수 선언문
function add(x, y) {
    return x + y;
}

var result = add(1, 2);



// 함수 호출
// 인수 1과 2가 매개변수 x와 y에 순서대로 할당되고 함수 몸체의 문들이 실행된다.
```

![그림 12-9](../images/12-9.png)