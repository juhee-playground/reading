# 44. REST API

REST는 HTTP 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
REST API는 REST를 기반으로 서비스 API를 구현하는 것을 말한다.

## 44.1 REST API의 구성

REST API 구성요소

|구성요소|내용|표현방법|
|--------|----|--------|
|자원|자원|URI|
|행위|자원에 대한 행위|HTTP 요청 메서드|
|표현|자원에 대한 행위의 구체적 내용|페이로드|

## 44.2 REST API 설계 원칙

가장 중요한 기본적인 원칙 두가지

1. URI는 리소스를 표현
2. 행위에 대한 정의는 HTTP 요청 메서드를 통해 표현

### 1. URI는 리소스를 표현

```javascript
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다

```javascript
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```
