# 오늘 읽은 내용

패턴에 의한 평가

## TIL 3줄 요약

- 망가진 웹사이트와 어플리케이션을 수정하는 일은 불편한 작업이다. 처음부터 인클루시브 디자인 방법을 교육하고 훈련하는 것이 더 가치 있고 효과적이다.
- 패턴에 의한 개선은 디자인 싱킹을 개선 과정에 포함시킴으로 제품 수정에도 도움을 주고 미래의 인클루시브 디자인 의사결정에 신뢰를 불어넣는다.
- 커스텀 요소를 만들 때 인클루시브 디자인의 관점을 추가하자.

## TIL (Today I Learned) 날짜

2024.05.01

## 오늘 읽은 범위

### 패턴에 의한 평가

인터페이스를 디자인할 때 추상적인 원칙이 아니라 실제로 작동하는 어떤 것을 만든다.
상황에 따라 모듈이나 컴포넌트라 부른다. 덜 엄격하게는 **패턴**이라고도 한다.
본질적으로 인터펭스를 구성하는 부품을 의미한다.
어떠한 원칙만을 따르는 패턴을 만들지는 않는다.

#### 원칙에 의한 평가의 문제점

필자는 감사를 할 때 WCAG를 따른다.
WCAG 인식성(Perceivable), 운용성(Operable), 이해성(Understandable), 호환성(Robust) 4가지 원칙으로 이루어진다.

우리는 원칙에 따라 접근성 결함을 보고하는 것을 무척 고집한다.
대부분의 이슈는 하나의 원칙이나 주제에 속한 이슈는 인과적이든 상호적이든 상관 없읻 다른 이슈와 관련이 있다. 잘못된 디자인에서 둘 이상의 이슈로 나누어서 티켓을 만드는 것은 개발자가 문제의 실제 원인을 파악하는데 아무 도움이 되지 않는다.

```html
<div class='upvote' data-action='upvote'></div>
````

위에 처럼 생긴 버튼이 있다고 가정해보자.

이 버튼의 문제점은 아래와 같다.

- 인식성 원칙에서 : 텍스트가 아닌 콘텐츠 조항에 어긋난다. 이 버튼은 이미지로 되어 있다.
- 운용성 원칙에서 : 키보드 조항을 위반한다. 포커스를 받을 수 없는 `<div>` 요소를 사용한다.
- 호환성 원칙에서 : 이름 ,역할, 값 조항을 위반한다. 실제 버튼이라는 정보를 보조기술에 알리는 어떤 마크업도 없다.

버튼을 개선 한 코드
```html
<button data-action="upvote" aria-label="upvote">
  <svg> <use xlink:href="#upvote"></use> </svg>
</button>
```


## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

커스텀으로 무언가를 만들 때 고려해야 할 것들이 많다. 되도록이면 div가 아닌 원래의 용도에 맞는 태그를 써야지 추가적인 작업들을 안하게 되는 것 같다.

## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

섀도DOM
https://developer.mozilla.org/ko/docs/Web/API/Web_components/Using_shadow_DOM 참고

## 해시태그: 키워드

#인클루시브디자인 #패턴에의한평가