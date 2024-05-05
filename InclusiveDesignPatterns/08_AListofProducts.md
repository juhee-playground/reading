# 오늘 읽은 내용

제품 목록

## TIL 3줄 요약

- 콘텐츠의 구성과 구조가 가장 중요하다.
- 이미지 최적화는 어디에서나 해야하는 일이다.
- 데이터 구조화를 잘하면 검색결과 페이지에 이점이 생긴다.

## TIL (Today I Learned) 날짜

2024.05.03

## 오늘 읽은 범위

### 제품 목록

보통 회사나 개발자들은 흥미가 없을 것으로 생각하는 사용자들을 위한 지원은 의미가 없다고 생각한다. 사진을 주제로 한 사이트에서 시각장애인이 사진에 관심 없다고 생각하며 스크린 리더 신경을 쓰는 것을 의아해할지도 모른다.

이런식으로 하나 둘 만드는 사람의 편의만 생각한다면 제품의 질이 떨어진다.
품질이란 값비싼 추가 기능이 아니라 인클루시브함을 말한다.

마우스 잡는 손으로 무언가를 먹고 있는 모든 사람은 키보드 사용자다.

### 리스트의 장점

콘텐츠 접근성을 아이템 별로 나누고 주제별로 그룹화 했으며 분해헀다.
간단하게 정의했지만 이와 같은 것들을 정의하고 맥락과 소속 관계를 확인한다면 좀 더 복잡한 구조 정의에 응용하는 것도 문제가 되지 않는다.

```html
<li>
  <h3>
    Naked Man In Garage Forecourt
    <a href="/photographer/kenny-mulbarton">by Kenny Mulbarton</a>
  </h3>
</li>
```

주력 제품에 h2 태그를 붙이는 것보다 일부 텍스트를 강조하는 것이 더 좋다.

```html
<li>
  <h3>
    <strong class="hightlight">Featured: </strong>
    Naked Man In Garage Forecourt
    <a href="/photographer/kenny-mulbarton">by Kenny Mulbarton</a>
  </h3>
</li>
```

강조된 텍스트는 시각적으로 눈에 쉽게 띄며 보조 기술에도 Featured라는 정보를 제공한다.
스크린 리더 사용자는 헤딩을 통해 제품 목록을 탐색할 것이다. 

헤니스완의 BBC에서 만든 접근성 있는 UX 기본 원칙

1. **사용자에게 선택권**을 줄 것.
2. 사용자에게 제어권을 줄 것.
3. 친금함을 염두에 둔 디자인을 할 것.
4. 부가가치가 있는 기능들은 우선순위를 정할 것.

사용자가 특정 페이지로 이동하기 위해 헤딩의 링크를 클릭한다면 그 작가의 사진 목록이 그 자리에서 실행되고 보여야 한다.

같은 것을 같은 방식으로 보여주어야 한다.
사이트의 요소의 표현 방식과 레이블 방식의 일관성을 유지하는 것은 아주 중요하다.

### 주요 정보

```html
<li>
  <h3>
    <strong class="hightlight">Featured: </strong>
    Naked Man In Garage Forecourt
    <a href="/photographer/kenny-mulbarton">by Kenny Mulbarton</a>
  </h3>
  <dl>
    <dt>Size:</dt>
    <dd>90cm × 30cm</dd>
    <dt>Price:</dt>
    <dd>€35.95</dd>
    <dt>Rating:</dt>
    <dd><img src="/images/rating_4_5.svg" alt="">4 out of 5 stars</dd>
  </dl>
</li>
```

- 90cm × 30cm 여기에서 곱하기를 제대로된 기호로 쓰지 않으면 스크린 리더 사용자가 다른 의미로 읽힐 수 있다.
- alt 텍스트 맥락에 의해 전달되어야 한다.
- 단어 역시 크기 보다 원래는 면적이 더 정확한 표현이지만 딱딱한 단어보다 쉽고 인간적인 단어를 사용하는 것이 좋다.

### '바로 구매' 기능

바로구매 기능은 버튼의 형태를 하고 있지만 사실 페이지를 이동하는 링크이다.
`<a>`태그에 `role="button"`을 한다면 스크린리더 사용자를 속이고 혼란스럽게 할 수 있다.
버튼을 닮지는 않았지만 표준 링크보다는 시각적으로 강조된 별도의 콜투 액션 스타일의 링크를 만드는게 좋다.

- 이런 요소는 각기 다르게 취급되지만 상호작용성의 품질을 공유한다는 의미로 같은 색 사용을 권장. 링크가 파란색이라면 버튼도 파란색일 때 쉽게 인지하기 좋다. 파란색은 클릭 가능하다는 의미가 있기 떄문이다.
- a에 .call-to-action 클래스를 부여했는데 이렇게 하면 중복을 일으킨다고 할수 도 있다. 하지만 .call-to-action 스타일을 a 요소로 한정하면 해결된다.
- 버튼이 다른 링크와 구별되는 또 다른 사항은 손 모양 커서 등과 같은 pointer 커서를 갖지 않는다는 것. 커서 스타일을 적용한 `<button>`은 표준 도앚ㄱ을 하지 않게 되며 이는 오래 유지된 관례를 깨는 일이다.


#### 표준동작의 수용

```html
<!-- 안정적이며 가능한 방법 -->
<a href="/product/naked-man-in-garage" class="call-to-action">Buy Now</a>

<!-- 안정적이거나 가능적이지 않은 방법 -->
<a href="javascript:;" data-action="naked-man-in-garage" class="call-to-action">Buy Now</a>
```

#### 잘 서술된 고유의 링크 텍스트

```html
<a href="/product/naked-man-in-garage" class="call-to-action">
  Buy <span class="visuallt-hidden">Naked Man In Garage Forecourt</span> Now
</a>
```

링크 텍스트에 제품이름을 넣는 경우 부가적인 이득이 있다.
검색 엔진의 가시성을 높인다는 점. 구글은 잘 서술된 링크 텍스트의 사용을 권장한다.
그래야만 크롤러가 링크 텍스트와 대상 페이지의 `<title>`이나 `<h1>`이 같다는 것을 인식해서 검색어와의 높은 관련성을 부여하기 떄문이다.

#### 블록 수준의 링크

제품 콘텐츠 전체를 링크로 감쌀 수 있는데 클릭 영역이 넓어진다는 이점이 있다.

```html
<li>
  <a href="/product/naked-man-in-garage">
    <h3> Naked Man In Garage Forecourt</h3>
    <img src="/images/naked-forecourt-man.jpg" alt="high contrast black and white image of a naked man nonchalantly leaning against a pertol pump" />
    <dl>
      <dt>Size:</dt>
      <dd>90cm × 30cm</dd>
      <dt>Price:</dt>
      <dd>€35.95</dd>
      <dt>Rating:</dt>
      <dd><img src="/images/rating_4_5.svg" alt="">4 out of 5 stars</dd>
    </dl>
  </a>
</li>
```

위의 마크업은 기술적인 규범을 완전히 지키고 있으며 UX와 관련한 몇가지 중여한 사항을 제시한다.

1. 보조기술에 의해 탐지되거나 시각적으로 보일 전용 레이블이 없다. 실제로 상호작용 하기 전까지 상호작용이 가능한지 알지 못한다.
2. 링크가 전체를 둘러싸고 있는 구조로 포커스를 링크가 받기 때문에 h3 헤딩이 낭독되지 않음.
3. 링크가 포커스를 받을 경우 텍스트로 된 콘텐츠 전부를 읽게 됨.
4. 터치 디바이스 사용자는 상호작용이 없어보이는 스크린의 일부를 터치하여 뜻하지 않게 페이지 이동이 될 수 있음.

이와 같이 표준에 따른 기술적으로 유효한 마크업이라는 점이 반드시 일정 수준의 사용자 경험을 만들 수 있다는 것을 의미하지는 않는다.

규정을 100%로 준수하는 인터페이스도 항상 완벽하게 사용할 수 없다.
마찬가지로 이상해보이는 에러가 있더라도 단순하고 잘 구조화된 명료한 인터페이스라면 즐겁게 사용할 수 있다.

인클루시브 디자인은 시맨틱하고 안정적인 인터페이스를 만드는 마크업의 중요성을 인정한다.
목적을 달성하기 위해 실제로 일을 완수하는 것은 사용자의 능력이다.
마크업의 품질은 UX의 측면에서 측정되기 때문이다.

### SERP

SERP는 검색 결과 페이지
찾아보기가 잘된 사이트는 SERP에도 제품을 노출시킬 수 있다.

`site:shop.the-photography-site.com [search term]`

구글콘텐츠 목록의 외형을 바꿀 수 없지만 문구를 통제할 수는 있다.
`<title>`의 텍스트를 쉽게 이해할 수 있게 만드는 것이 엄청 중요하다.
구조화된 데이터는 구글봇 같은 프로그램이 파싱할 수 있는 메타 정보를 증가시킨다.

#### 제품단어집

구조화된 데이터는 단어로 분할되어 서로 다른 각종 콘텐츠에 적용될 수 있다.

평범한 마크업

```html
<main id="main">
  <h1>
    Naked Man In Garage Forecourt
    <a href="/artist/kenny-mulbarton">by Kenny Mulbarton</a>
  </h1>
  <img src="/images/naked-forecourt-man.jpg" alt="high contrast black and white image of a naked man nonchalantly leaning against a pertol pump" />
  <dl>
    <dt>Size:</dt>
    <dd>90cm × 30cm</dd>
    <dt>Price:</dt>
    <dd>€35.95</dd>
    <dt>Rating:</dt>
    <dd><img src="/images/rating_4_5.svg" alt="">4 out of 5 stars</dd>
  </dl>
  <h2>Choose a payment method</h2>
  <!-- 구매 위젯 -->
</main>
```

구조화된 데이터를 포함시킨 코드

```html
<main id="main" itemscope itemtype="http://schema.org/Product">
  <h1>
    <span itemprop="name">Naked Man In Garage Forecourt</span>
    <a href="/artist/kenny-mulbarton">by Kenny Mulbarton</a>
  </h1>
  <img itemprop="image" src="/images/naked-forecourt-man.jpg" alt="high contrast black and white image of a naked man nonchalantly leaning against a pertol pump" />
  <dl>
    <dt>Size:</dt>
    <dd>90cm × 30cm</dd>
    <dt>Price:</dt>
    <dd>
      <span itemprop="offers" itemscope itemtype="http://schema.org/Offer">
        <meta itemprop="priceCurrency" content="EUR" />
        €<span itemprop="price">35.95</span>
      </span>
    </dd>
    <dt>Rating:</dt>
    <dd>
      <img src="/images/rating_4_5.svg" alt="">
      <span itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
        <span itemprop="ratingValue">4</span> start, based on <span itemprop="reviewCount">13</span> reviews
      </span>
    </dd>
  </dl>
  <h2>Choose a payment method</h2>
  <!-- 구매 위젯 -->
</main>
```

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

특정 사용자를 위해서가 아니라 잠재적으로 혹은 어떤 상황때문에 내가 특정 사용자가 될 수도 있다.

귀찮아서 보통 사이즈나 X를 써야 할 때 소문자 x나 대문자 X 를 많이 썼는데 이렇게 쓰면 스크린 리더가 읽을 때 곱하기가 아닌 엑스라고 읽는다니;;; 충격이다. 귀찮더라도 알맞은 기호를 써야 할 것 같다.
책을 읽다보면 하지 말아야할 것들을 상당부분 내가 하고 있는 부분들이 보인다.

나는 책에 한 챕터를 읽은 것 뿐인데 왜케 읽을거리가 많아지는건지;;;
마지막 부분은 정말 흥미로웠다.

데이터 구조화 하는 부분은 더 알아봐야 겠다. 그래서 제대로 써먹어봐야지!


## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

[콘텐츠와 프레젠테이션을 분리](http://webaim.org/techniques/css/#sep)

[더 나은 접근성을 위한 쉬운 말의 채택](http://www.hand.org/techniques/css/#sep)

[접근성을 의무화 하는 CSS 프레임워크](http://smashed.by/enforceally)

[버튼에서 손 모양 커서가 없어야 한다](http://smashed.by/handcursor)

[구글은 잘 서술된 링크 텍스트의 사용을 권장한다](http://smashed.by/linkarchitecture)

[구글의 구조화된 데이터 테스팅 도구](http://search.google.com/structured-data/testing-tool)

 `<meta>` 태그 중간에쓰는 법
  `itemprop` `itemscope` `itemtype` 사용법?

## 해시태그: 키워드
#인클루시브디자인 #제품목록
