# 오늘 읽은 내용

테스트 주도 마크업

## TIL 2줄 요약

- css 테스트는 시각적인 표현으로 프런트개발자에게 알려주기 위함이다.
- 추후에 에러가 발견된다면 동료들에게 인클루시브한 마크업에 대해 설명할 수 있는 좋은 기회가 될 것이다.

## TIL (Today I Learned) 날짜

2024.05.06

## 오늘 읽은 범위

### 테스트 주도 마크업

TDD는 개발자가 잦은 반복작업을 통해 확신을 갖고 진행할 수 있게 만드는 방법이다.
HTML과 같은 선언형 언어에 그와 같은 방식의 테스트를 작성한다면 마크업 구조가 올바른지 확인할 수 있다.

잘 구성된 마크업은 웹 전근성에 매우 큰 역활을 하므로 TDD같은 방법으로 인클루시브 하지 않은 패턴 구조를 막을 수 있다.

### 선택자의 로직

활성화된 모든 버튼 매칭

`button:not(:disabled) { ...}`

자바스크립트 코드로 바꾸면

`if(element nodeName === "button" && !element.disabled) { .... }`

깨졌거나 이상한 패턴을 대상으로 선택자를 사용할 수 있다.
문제점을 강조하는 에러스타일을 만들 수 있다.

### 테스트 주도 탭 인터페이스

```html
<div class="tab-interface">
  <ul role="tablist">
    <li role="presentation">
      <a href="#panel1" id="tab1" role="tab" aria-selected="true">First Tab</a>
    </li>
    <li role="presentation">
      <a href="#panel1" id="tab1" role="tab" aria-selected="true">First Tab</a>
    </li>
    <li role="presentation">
      <a href="#panel2" id="tab2" role="tab" tabindex="-1">Second Tab</a>
    </li>
    <li role="presentation">
      <a href="#panel3" id="tab3" role="tab" tabindex="-1">Third Tab</a>
    </li>
  </ul>
  <div role="tabpanel1" id="panel1" aria-labelledby="tab1">
    <!-- 탬패널 1의 콘텐츠 -->
  </div>
  <div role="tabpanel1" id="panel2" aria-labelledby="tab2">
    <!-- 탬패널 2의 콘텐츠 -->
  </div>
  <div role="tabpanel1" id="panel3" aria-labelledby="tab3">
    <!-- 탬패널 3의 콘텐츠 -->
  </div>
</div>
```

ul 요소에 role="tablist" 속성이 없다면 강조 표시를 해야한다.

```css
.tab-interface ul:not([role="tablist"]) {
  outline: 0.5em solid red;
}
```

tablist요소 안에 `<li>`요소 안에 직접 넣는 경우도 있지만 링크의 리스트를 쉽게 분해될 수 있는 구조로 만들기 위해 `<a>`요소에 넣는 방법을 선호.

```css
.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]) {
  outline: 0.5em solid red;
}
```

자바스크립트가 탭 인터페이스 시맨틱을 추가하면 리스트 시맨틱은 중복이된다. 그래서 `<li>` 요소에 role="presentation"을 넣었다.

```css
.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]),
[role="tablist"] li:not([role="presentation"]) {
  outline: 0.5em solid red;
}
```

탭 인터페이스에 aria-selected 상태를 사용해 접근 가능하도록 정의된 탭 하나가 항상 있어야 한다. 그리고 나머지 탭은 모두 tabindex = -1 속성을 취해야 한다.

```css
.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]),
[role="tablist"] li:not([role="presentation"]),
[role="tablist"] a[aria-selected][tabindex="-1"],
[role="tablist"] a:not([aria-selected]):not([tabindex="-1"]) {
  outline: 0.5em solid red;
}
```

탭 패널이 올바른 역활을 하는지 확인하면서 aria-labelledby 값이 tab으로 시작되지 않는지 테스트 한다. 물론 이는 aria-labelledby 가 아예 없는 탭도 짚어낸다.
퍼지 매칭은 속성선택자인 [aria-labelledby^="tab"]에 시작문자(^)를 사용함으로 가능하다.

```css
.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]),
[role="tablist"] li:not([role="presentation"]),
[role="tablist"] a[aria-selected][tabindex="-1"],
[role="tablist"] a:not([aria-selected]):not([tabindex="-1"]),
[role="tabpanel"]:not([aria-labelledby^="tab"]) {
  outline: 0.5em solid red;
}
```

마지막으로 모든 탭 패널에 tabpanel역할이 주어졌는지 검증하도록 하기.
형제조합자 **~**를 사용해 tabpanel이 아닌 `<div>`가 tablist 다음에 있는지 확인할 수 있다.

```css
.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]),
[role="tablist"] li:not([role="presentation"]),
[role="tablist"] a[aria-selected][tabindex="-1"],
[role="tablist"] a:not([aria-selected]):not([tabindex="-1"]),
[role="tabpanel"]:not([aria-labelledby^="tab"]),
[role="tablist"] ~ div:not([role="tabpanel"]) {
  outline: 0.5em solid red;
}
```

탭 리스트와 탭 패널 사이에 콘텐츠가 존재하면 인터페이스가 손상될 수 있고 혼란을 일으킬 수 있다. tabpanel역할의 `<div>`가 tablist 다음에 처음 나타나는 형제 요소인지 확인하는 테스트
인접 형제 조합자인 **+**를 사용한다.

```css
.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]),
[role="tablist"] li:not([role="presentation"]),
[role="tablist"] a[aria-selected][tabindex="-1"],
[role="tablist"] a:not([aria-selected]):not([tabindex="-1"]),
[role="tabpanel"]:not([aria-labelledby^="tab"]),
[role="tablist"] ~ div:not([role="tabpanel"]),
[role="tablist"] + div:not([role="tabpanel"]) {
  outline: 0.5em solid red;
}
```

#### 에러메시지

지금까지는 원하지 않는 패턴을 사용한 요소에 모두 빨간테두리를 표시했다.
에러에 대한 설명 또한 제공되어야 한다.
화면에 에러를 표시하는 방법은 좋지못하며 에러를 페이지 안이 아닌 자바스크립트 콘솔과 같은 개발자 도구에 에러메시지를 출력하게 할 수 있다.

여기에도 두가지 문제가 있다.

1. 한 요소에 둘 이상의 에러가 있을 경우 CSS 인스펙터에서 마지막을 제외한 에러에 취소선을 긋는다는 점.
2. 불필요한 사항이라 할 수 있는 display:none을 에러메시지에 추가해야 한다는 점.

#### ERROR 속성

css 인스펙터에 에러를 기록할 때 비공식 속성을 사용할 때 이득

1. 가상콘텐츠는 display:none;과 함께 완전히 퇴출되며 개별 css 에러메시지는 다음과 같이 간략해진다.

```css
.tab-interface ul:not([role="tablist"]) {
  ERROR: The tab interface `<ul>` must have the tablist WAI-ARIA role
}
```

2. 비공식 속성은 CSS 명시도 계산에서 빠진다.
브라우저에 따라 취소선을 표시하지 않거나 ERROR 선언에 회색 스타일을 적용한다는 의미.

```css
.tab-interface ul:not([role="tablist"]) {
  ERROR: The tab interface `<ul>` must have the tablist WAI-ARIA role to be recongnized in assistvie technologies.;
}

[role="tablist"] a:not([role="tab"]) {
   ERROR: `<a>` elements within the tablist need to each have the WAI-ARIA tab role to be counted as tabs in assistvie technologies.;
}

[role="tablist"] li:not([role="presentation"]) {
   ERROR: Remove the `<li>` segmeantics with the WAI-ARIA presentation role. Where the tab interface is instated. these semantics are irrelevant.;
}

[role="tablist"] a[aria-selected][tabindex="-1"] {
   ERROR: Remove the -1 tabindex value on the aria-selected tab to make it focusable by the user. They should be able to move yo this tab only.;
}

[role="tablist"] a:not([aria-selected]):not([tabindex="-1"]) {
   ERROR: All unselected tabs should habe the -1 tabindex value and only be focusable using the left and right arrow keys.;
}

[role="tabpanel"]:not([aria-labelledby^="tab"]){
   ERROR: Each tabpanel should have an aria-labelledby attribute starting with "tab" followed by the corresponding tab's number. This is the convention of tab systems in our interface.;
}

[role="tablist"] ~ div:not([role="tabpanel"]){
   ERROR: Each tabpanel needs to have the explict tabpanel WAI-ARIA role to be correctly associated with the tablist that controls it.;
}

[role="tablist"] + div:not([role="tabpanel"]){
   ERROR: The first element after the tablist should be a tab panel with the tabpanel WAI-ARIA role. Screen reader users must be able to move directly into the open tab panel from the selected tab.;
}

.tab-interface ul:not([role="tablist"]),
[role="tablist"] a:not([role="tab"]),
[role="tablist"] li:not([role="presentation"]),
[role="tablist"] a[aria-selected][tabindex="-1"],
[role="tablist"] a:not([aria-selected]):not([tabindex="-1"]),
[role="tabpanel"]:not([aria-labelledby^="tab"]),
[role="tablist"] ~ div:not([role="tabpanel"]),
[role="tablist"] + div:not([role="tabpanel"]) {
  outline: 0.5em solid red;
}
```

프런트 개발자에게만 보이도록 시각적으로 에러를 강조할 뿐이다.
운영환경의 빌드에 파일을 복사할 때 .test.css라는 패턴을 사용하면 쉽게 제외시킬 수 있다.

테스트 주도 마크업은 일반적인 접근성 테스트 방식과는 다르다.
css 기반의 북마클릿이나 tenon.io와 같은 고급 API를 사용하면 포괄적인 의미의 에러도 나타나게 해준다.
자신만의 패턴과 구조를 위해 자신만의 테스트를 작성하는것은 기대하는 형태에 대해 더욱 세밇하고 구체적으로 접근할 수 있다.

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

내가 기대한 테스트 방법은 아니지만 내 관점에서 이건 꼭 빠뜨리지 않고 작성해야겠다라는 기준이 생긴다면 코드에 넣고 그걸 지키기 위해서 테스트를 작성할 것 같긴 하다.
하지만 아직까지는 어떤 것은 꼭 해야지라는 기준점이 없는것 같다.
명확한 기준을 가지기 위해 더 많은 코드를 만져봐야 할 것 같다.
아직은 연습이 필요하다. 시맨틱 태그를 사용하는 연습과
코드를 짤 때 더 많이 생각하고 더 많은 사용자를 고려하여 코드를 짜야 겠다.

## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

퍼지 매칭

CSS후크

## 해시태그: 키워드

#인클루시브디자인 #테스트주도마크업