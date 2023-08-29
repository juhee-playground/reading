# 47. 에러처리

## 47.2 try... catch... finally 문

```javascript
try {
    // 실행할 코드(에러가 발생할 가능성이 있는 코드)
}catch(err) {
    // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
    // err 에는 try 코드 블록에서 발생한 Error 객체가 전달된다. 
}finally {
    // 에러 발생과 상관 없이 반드시 한 번 실행된다.
}
```

[예제 47-06]

```javascript
console.log('[Start]');

try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
  foo();
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
  console.error(err); // ReferenceError: foo is not defined
} finally {
  // 에러 발생과 상관없이 반드시 한 번 실행된다.
  console.log('finally');
}

// try...catch...finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.
console.log('[End]');
```

## 47.3 Error 객체

[예제 47-07]

```javascript
const error = new Error('invalid');
```

|생성자 함수|인스턴스|
|-----------|--------|
|Error|일반적 에러 발생|
|SyntaxError|자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체|
|ReferenceError|참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체|
|TypeError|피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체|
|RangeError|숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체|
|URIError|encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체|
|EvalError|eval 함수에서 발생하는 에러 객체|

[예제 47-08]

```javascript
1 @ 1;    // SyntaxError: Invalid or unexpected token
foo();    // ReferenceError: foo is not defined
null.foo; // TypeError: Cannot read property 'foo' of null
new Array(-1); // RangeError: Invalid array length
decodeURIComponent('%'); // URIError: URI malformed
```

## 47.4 throw 문

[예제 47-09]

```javascript
try {
  // 에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
  new Error('something wrong');
} catch (error) {
  console.log(error);
}
```

```javascript
throw 표현식;
```

[예제 47-10]

```javascript
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
  throw new Error('something wrong');
} catch (error) {
  console.log(error);
}
```

## 47.5 에러의 전파

에러는 호출자 방향으로 전파된다. 즉 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

[예제 47-12]

```javascript
const foo = () => {
  throw Error('foo에서 발생한 에러'); // ④
};

const bar = () => {
  foo(); // ③
};

const baz = () => {
  bar(); // ②
};

try {
  baz(); // ①
} catch (err) {
  console.error(err);
}
```
