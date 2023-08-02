# 14. 전역변수의 문제점

## 14.1 변수의 생명 주기

### 14.1.1 지역변수의 생명 주기

[예제 14-01]

```javascript
function foo() {
  var x = 'local';
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined
```

지역변수의 생명주기는 함수의 생명 주기와 일치한다.

[예제 14-01]

```javascript
var x = 'global';

function foo() {
  console.log(x); // ① 
  var x = 'local';
}

foo();
console.log(x); // global
```

① 의 x는 지역변수의 x를 참조해 출력한다. 변수 할당문이 실행되기 이전까지는 undefined 값을 갖는다.

호이스팅

- 스코프 단위로 동작한다.
- 변수 선언이 스코프의 선두로 끌어 올려진 것처럼 동작하는 것.

### 14.1.2 전역 변수의 생명주기

var 키워드로 선언한 전역 변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

## 14.2 전역변수의 문제점

- 암묵적 결합
- 긴 생명 주기
- 스코프 체인 상에서 종점에 존재
- 네임스페이스 오염
  
## 14.3 전역 변수의 사용을 억제하는 방법

전역 변수를 반드시 사용해야 할 이유를 찾지 못한다면 지역 변수를 사용해야 한다.
변수의 스코프는 좁을 수록 좋다.

### 14.3.1 즉시 실행 함수

모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

[예제 14-04]

```javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
}());

console.log(foo); // ReferenceError: foo is not defined
```

### 14.3.2 네임스페이스 객체

[예제 14-05]

```javascript
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = 'Baek';

console.log(MYAPP.name); // Baek
```

[예제 14-06]

```javascript
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.person = {
  name: 'Baek',
  address: 'Seoul'
};

console.log(MYAPP.person.name); // Baek
```

### 14.3.3 모듈 패턴

```javascript
var Counter = (function () {
    // private 변수
    var num = 0;

    // 외부로 공개할 데이터나 메서드를 프로퍼티에 추가한 객체를 반환한다.
    return {
        increase() {
            return ++num;
        },
        decrease() {
            return --num;
        }
    };
}());

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

### 14.3.4 ES6 모듈

ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.
