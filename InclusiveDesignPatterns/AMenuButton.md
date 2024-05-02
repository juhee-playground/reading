# 오늘 읽은 내용

메뉴

## TIL 3줄 요약

- 단순히 시각적으로 효과를 주기 위한 css 포지셔닝을 사용하는 애니메이션을 지양하자
- 접근 가능한 레이블을 누락하지 말자 
- 사용자들에게 최대한 빨리 방해없이 효율적인 방식으로 콘텐츠를 전달하자.


## TIL (Today I Learned) 날짜

2024.05.02

## 오늘 읽은 범위

### 메뉴 버튼

메뉴 항목이 4개 이하이면 그냥 그대로 두는 편이 낫다.
어떤 기능을 숨겼다가 그 기능이 필요할 때 사용자가 추가 작업을 하게 만드는 것은 언제나 최후의 수단이어야 한다.

### 모양

아이코노그래피는 시각장애가 없는 사용자의 이해를 증진시킨다는 점은 거의 정설이다.

인클루시브 디자인은 관례를 따르는 편이다. 흔히 메뉴 모양은 세 줄(햄버거모양)으로 한다. 아이콘의 모양은 바꾸지 않는 편이 좋다. 그래야 관례대로 이해하기 떄문이다.
기본적으로 모든 버튼은 버튼처럼 보여야 한다.

### 아이콘 렌더링

#### 배경이미지 

고대비 모드를 켰을 때 아미지만 있는 경우 시각적인 존재감이 사라진다.

#### 이미지

배경이 검은색으로 바뀌면 이미지 또한 사라진 것처럼 보인다. 만약 아이콘이 흰색일 수 도 있다. 그렇다면 아이콘은 보이겠지만 원래와 살짝 다른 제안이 된다.

#### 아이콘 폰트와 문자

아이콘 폰트는 텍스트이며 따라서 텍스트와 같이 동작한다. 색상도 잘 반전되고 품질 저하없이 크기 조절이 가능하다. 그러나 난독증이 있는 사용자가 자신이 선호하는 폰트로 텍스트를 바꾸게 된다면 아이콘 폰트는 문제가 된다.

> ☐MENU
정의되지 않은 문자라는 빈박스만 보일 수 있다.
이렇게 보이게 된다면 사용자 입장에서 부정적인 영향을 미칠 것이다.

#### 유니코드

유니코드 기호를 아이콘으로 표시할 수 있다. 유니코드에도 하늘괘라는 기호가 세 줄 모양이다.

두가지 이슈

- 어떤 디바이스는 하늘 괘를 포함할만큼 충분한 유니코드 확장을 지원하지 않을 수 있다.
- 사용자 영역을 사용하는 경우와는 달리 보조기술에 따라 해석이 된다. 메뉴가 아니라 하늘 괘라고 읽힐 수 있다. 그래서 스크린 리더 사용자에게 혼란을 줄 수 있다.

`aria-hidden = 'true'` 지정하여 스크린 리더가 읽지 못하게 렌더링된 css 가상 콘텐츠를 사용하여 막는 방법이 있다.

#### svg 스프라이트

아이콘 렌더링에서 매우 타당한 사실상의 표준 해법이다.
svg 스프라이트는 페이지에 내장될 때 최고의 크로스 브라우징이 가능하다 이는 매번 HTTP요청을 하지 않는다는 의미이다.

### 레이블

모든 상호작용 요소는 보조기술에서 해석하고 통신할 수 있는 접근 가능한 이름이 있어야 한다.

아이콘만 단독으로 제공해야 하는 상황에서 스크린 리더가 메뉴 버튼을 식별할 수 있게 하는 것이 중요하다.

#### 히든 `<span>`

`<span>`에 css 핵을 적용해 'Menu'텍스트 레이블을 스크린 리더가 인식하게 하면서 시각적으로는 숨기는 방법

```html
<button>
  <svg><use xlink:href='#navigation'></use></svg>
  <span class='visually-hidden'>menu</span>
</button>
```

#### aria-label 속성

아이콘에 레이블을 주기위한 또 다른 방법.
aria-label 이 전역 속성은 alt 처럼 대체 텍스트를 붙여주는 역활을 한다.

```html
<button aria-label='menu'>
  <svg><use xlink:href='#navigation'></use></svg>
</button>
```

aria-label의 장점
요소에 텍스트 노드가 있다면 무시한다는 점이다.
►, x 아이콘에 삼각형, 엑스가 아닌 aria-label로 설정해 놓은 재생 버튼, 닫기 버튼으로 원하는 레이블을 줄 수 있다.
따라서 가급적 렌더링에 svg를 사용하는 것이 좋다.

### 구식 브라우저

구식브라우저에서 svg대체기술이 필요하다.
`<switch>`, `<foreignObject>`

```html
<button aria-label='menu'>
  <svg><switch><use xlink:href='#navigation'></use><foreignObject><img src='path/to/navicon.png' alt='' /></foreignObject></switch></svg>
</button>
```

대체 이미지에는 `alt=""` 빈 값이나 null을 지정하면 스크린리더가 이미지로 인식하지 않는다.
MENU 레이블이 이미 제공되므로 이미지에 alt를 지정하지 않아 파일이름을 스크린리더가 인식한다면 사용자에게는 더 큰 혼란을 주게 된다. 따라서 `alt=""` 설정을 지정해줘야 한다.

### 작동

인클루시브한 UX를 만들기 위해 신경써야 하는 것으로 `<button>`과 메뉴의 위치이다.

사용자를 메뉴로 운송하기 위한 최선의 방법은 링크다.

```html
<a href="#nav-menu">navigation menu</a>
<!-- 다른 수많은 DOM 관련 코드가 들어올 자리 -->
<nav aria-label="site" id="nav-menu" tabIndex="-1">
  <ul>
    <li><a href="#main">home</a></li>
    <li><a href="/about">about</a></li>
    <li><a href="/products">products</a></li>
    <li><a href="/contact">contact</a></li>
    <li><a href="/login">login</a></li>
  </ul>
</nav>
```

사용자가 랜드마크에 도착하기 전까지 메뉴를 숨겨야 한다면 :target 이라는 가상 클래스를 사용하는 것이 좋다. hidden 속성을 없애고 css 사용하는 방법이 있다.

```css
#nav-menu ul {
  display: none;
}

#nav-menu:target ul {
  display: block;
}
```

#### 상태통신

웹 인터페이스에서 기능성요소의 상태와 통신 하는 일은 보조기술을 사용한다.
접근성트리에 의존하는 모두에 대한 인터페이스를 인클루시브하게 만드는 중요한 부분이다.

> 접근성 트리: 비시각적인 목적으로 마크업을 통해 제공하는 접근성 역화, 속성, 값, 상태정보를 나타내는 DOM의 한 버전.

인기있는 모든 스크린 리더는 변경이 있을 때마다 자신만의 버퍼를 갱신한다.
WAI-ARIA는 true와 false 값으로 상태의 존재여부를 알려주는 상태 속성을 제공한다.

aria-expanded를 사용하면 스크린 리더가 명확하게 접혀짐(false), 펼쳐짐(true)라고 낭독할 수 있게 해준다.

```html
<nav aria-label="site">
  <button aria-expanded="false">
    <svg><use xlink:href='#navigation'></use></svg>
    menu
  </button>
  <ul hidden>
    <li><a href="#main">home</a></li>
    <li><a href="/about">about</a></li>
    <li><a href="/products">products</a></li>
    <li><a href="/contact">contact</a></li>
    <li><a href="/login">login</a></li>
  </ul>
</nav>
```

#### 자바스크립트의 결정적 역할

```javascript
(function() {
  // 버튼과 메뉴 노드 취득
  var button = document.querySelector('[aria-label="site"]button');
  var menu = button.nextElementSibling;
  // 초기 상태(닫힌 메뉴)설정
  button.setAttribute('aria-expanded'. 'false');
  button.hidden = true;
  button.addEventListener('click', function() {
    // 메뉴 가시성 전환
    var expanded = this.getAttribute('aria-expanded') ==== 'true';
    this.setAttribute('aria-expanded', String(!expanded));
    menu.hidden = expanded;
  });
})();
```

사용자의 눈길을 끌기 위한 애니메이션 효과의 유혹에 빠지지 않기
ex) css 포지셔닝에 의존하는 애니메이션

대부분 실제 사용자는 일이 잘 처리되기만을 원한다. 인클루시브 디자이너가 최우선으로 가져야 할 덕목은 그들의 기대에 부응하는 일이다.

사용자들은 최대한 빨리 가급적 방해없이 가장 효율적인 방식으로 그 결과를 얻는데 관심이 있다.

### 터치 타깃

류머티즘성 관절염으로 손동작에 제한이 있는 사용자들도 포용해야 한다.
메뉴의 크기나 아이콘의 크기가 너무 작으면 사용하는데에 불편함을 초래한다.

권장요소

- 44 X 44 DIP, 48 X 48필셀을 권장한다.
- 모바일에서 메뉴의 크기를 성인 손가락 크기로 한줄에 한 링크씩 주는것 권장한다.

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

토이 프로젝트에다가 navigation에 menu 설정하는거 해봐야겠다.
navigation 컴포넌트 만들때 css position닝 사용하지 않고 만들어 봐야겠다.
이번주제는 은근히 흥미로웠다. 만들고 싶게 만드는 무언가가 있다.


## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

아이코노그래피: 도상학이라고 하며 단순 스타일로 접근하기 보다 기호학에 의거하여 최대한 직관적인 정보 전달에 우선순위를 둔다.

`<use>` 태그

다른 엘리먼트를 참조 할 때 사용한다.
<use> 요소는 문서 전반에 걸쳐 요소를 재사용 할 수 있습니다. 
x, y, width, height 속성을 사용해 설정이 가능하며, xlink:href 속성을 사용해 재사용 할 요소를 호출(식별자 ID 참조) 할 수 있습니다. 
요소를 재사용 함으로서 제작 시간을 아낄 수 있고, 코드를 최소화 할 수 있습니다.

## 해시태그: 키워드

#인클루시브디자인 #메뉴