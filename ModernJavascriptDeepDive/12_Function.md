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

## 12.4 함수 정의

### 12.4.1 함수 선언문

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

![그림 12-6 foo는 자바스크립트 엔진이 암묵적으로 생성한 식별자다](../images/12-6.png)

자바스크립트 엔진은 생성된 함수를 호출하기 위해함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다.

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

**함수 호이스팅** 

- 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바 스크립트 고유의 특징.
- 함수를 호출하기 전에 반드시 함수를 선언해야 한다는 당연한 규칙을 무시하기 때문에 함수 표현식을 사용할 것을 권장.

변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.

함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생한다.

### 12.4.4 Function 생성자 함수

[생성자 함수: 객체를 생성하는 함수를 말한다.](https://github.com/juhee-playground/reading/blob/main/ModernJavascriptDeepDivee/17_Object%20Created%20By%20ConstructorFunction.md)

[예제 12-13]

```javascript
var add = new Function('x', 'y', 'return x + y');

console.log(add(2, 5));
```

함수 선언문이나 함수 표현식으로 생성한 함수와 Function 생성자 함수로 생성한 함수가 동일하게 작동하지 않는다.
클로저를 생성하지 않는다 등. 다르게 작동한다.

### 12.4.5 ([화살표 함수](https://github.com/juhee-playground/reading/blob/main/ModernJavascriptDeepDive/26_ESGFunction.md#263-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98))

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
  
## 12.5 함수 호출

### 12.5.1 매개변수와 인수

[예제 12-16]

```javascript
// 함수 선언문
function add(x, y) {
    return x + y;
}

// 함수 호출
// 인수 1과 2가 매개변수 x  y에 순서대로 할당되고 함수 몸체의 문들이 실행된다.
var result = add(1, 2);
```

매개변수: 함수 안에서만 사용
인수: 함수를 호출할 때 사용

![그림 12-9](../images/12-9.png)

[예제 12-17]

```javascript
function add(x, y) {
    console.log(x, y); // 2, 5
    return x + y;
}

add(2, 5);

// add 함수의 매개변수 x, y는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined
```

## 12.6 참조에 의한 전달과 외부 상태의 변경

[예제 12-33]

```javascript
// 매개변수 primitive는 원시 값을 전달받고, 매개변수 obj는 객체를 전달받는다.
function changeVal(primitive, obj) {
    primitive += 100;
    obj.name = "Kim";
}

// 외부상태
var num = 100;
var person = { name: "Baek" };

console.log(num);       // 100
console.log(person);    // { name: "Baek" }

// 원시 값은 값 자체가 복사되어 전달되고 객체는 참조 값이 복사되어 전달된다.
changeVal(num, person);

// 원시 값은 원본이 훼손되지 않는다.
console.log(num);       // 100
// 객체는 원본이 훼손된다.
console.log(person);    // { name: "Kim" }
```

## 12.7 다양한 함수 형태

### 12.7.2 재귀함수

재귀 호출: 함수가 자기 자신을 호출하는 것
재귀 함수: 자기자신을 호출하는 행위

```javascript
// [예제 12-43]

function countdown(n) {
     for (var i = n; i >= 0; i--) {
        console.log(i)
     }
}

countdown(10);
```

```javascript
// [예제 12-44]

function countdown(n) {
    if(n < 0) return;
    console.log(n);
    countdown(n - 1); // 재귀 호출
}

countdown(10);
```

### 12.7.3 중첩함수

내부 함수(중첩함수): 함수 내부에 정의된 함수. 자신을 포함하는 외부 함수를 돕는 헬퍼 함수의 역활을 한다.
외부함수: 중첩 함수를 포함하는 함수

```javascript
// [예제 12-48]

function outer() {
     var x = 1;

     // 중첩함수
     function inner() {
        var y = 2;
        // 외부 함수의 변수를 참조할 수 있다.
        console.log(x + y);
     }

     inner();
}

outer();
```

### 12.7.4 콜백함수

[예제 12-51]

```javascript
// 외부에서 전달받은 f를 n만큼 반복 호출한다.
function repeat(n, f) {
    // i를 출력한다.
    for (var i = 0; i < n; i++) {
        f(i);
    }
}

var logAll = function(i) {
    console.log(i);
}

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logAll); // 0 1 2 3 4

var logOdds = function(i) {
    if (i % 2) console.log(i);
}

repeat(5, logOdds); // 1 3
```

콜백함수(예제 12-51의 log All)

- 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수
- 고차 함수에 의해 호출된다.

고차함수(예제 12-51의 repeat)
// TODO:(27.9절 연결)

- 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수  
- 매개변수를 통해 전달받은 콜백함수의 호출 시점을 결정해서 호출한다.
- 필요에 따라 콜백 함수에 인수를 전달할 수 있다.

### 12.7.5 순수 함수와 비순수 함수

순수함수: 함수형 프로그래밍에서 어떤 외부 상태에 의존하지도 않고 변경하지도 않는, 즉 부수효과가 없는 함수
비순수 함수: 외부상태에 의존하거나 변경하는, 즉 부수효과가 있는 함수

순수함수는 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수이다. 
하나 이상의 인수를 전달받는다.

```javascript
// [예제 12-56]
var count = 0;  // 현재 카운트를 나타내는 상태

// 순수 함수 increase는 동일한 인수가 전달되면 언제나 동일한 값을 반환한다.
function increase(n) {
    return ++n;
}

// 순수 함수기 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count);
console.log(count);     // 1

count = increase(count);
console.log(count);     // 2
```

```javascript
// [예제 12-57]
var count = 0;  // 현재 카운트를 나타내는 상태, increase 함수에 의해 변화한다.

// 비순수 함수
function increase() {
    return ++count;
}

// 비순수 함수는 외부 상태(count)를 변경하므로 상태 변화를 추적하기 어려워진다.
increase();
console.log(count);     // 1

increase();
console.log(count);     // 2
```

함수형 프로그래밍

- 외부상태를 변경하는 부수 효과를 최소화해서 **불변성**을 지향하는 프로그래밍 패러다임이다.

함수형 프로그래밍 목표

- 조건문, 반복문을 제거해서 복잡성을 낮추고
- 변수 사용을 억제하고 생명주기를 최소화해서 상태 변경을 피해 오류를 최소화