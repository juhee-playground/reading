# 오늘 읽은 내용

필터 위젯

## TIL 3줄 요약

- HTML은 우리가 다시 만들고 있는 것들을 이미 제공하고 있을 수 있다.
- 시맨틱 HTML을 사용할 때 css는 점진적 향상역활을 할 수 있다.
- 콘텐츠를 정리할 수 있는 선택권과 제어권을 사용자에게 제공하는 것은 중요하다.

## TIL (Today I Learned) 날짜

2024.05.04

## 오늘 읽은 범위

### 필터위젯

- 사용자가 정보의 우선순위를 정할 수 있도록 검색을 제어하는 부가적인 차원을 제시하는 중요한 도구 이다.
- 페이지의 콘텐츠를 변경하는 메타 콘텐츠에 해당한다.

헤니스완의 기본원친 중 두번째 **사용자에게 제어권을 둘것**

### 마크업

ARIA 첫번째 규칙

요소의 용도를 변경하면 접근성을 위해 ARIA역할, 상태, 속성을 추가하는 대신 이미 내장된 시맨틱과 동작이 있는 네이티브 HTML이나 속성을 사용할 수 있다면 그렇게 해야 한다.

정렬위젯은 `<filedset>`, `<legend>` radio button으로 만들 수 있다.

```html
<form role="form" class="sorter" method="get">
  <fieldset>
    <legend>Sort by</legend>
    <input type="radio" name="sort-method" id="most-recent" value="most-recent" checked />
    <label for="most-recent">most recent</label>
    <input type="radio" name="sort-method" id="popularity" value="popularity" />
    <label for="popularity">popularity</label>
    <input type="radio" name="sort-method" id="price-lowhigh" value="price-low-high" />
    <label for="popularity">price(low to high)</label>
    <input type="radio" name="sort-method" id="price-highlow" value="price-high-low" />
    <label for="popularity">price(high to low)</label>
  </fieldset>
  <button type="submit">sort</button>
</form>
```

1. `<form>`이 네비게이션 영역에 포함되어 스크린 리더에서 단축키로 탐색할 수 있게 한다.
2. `<input>`이 포커스 받으면 `<legend>` 콘텐츠가 낭독되며 그 다음에 `<input>`의 `<label>`이 읽힌다. 체크된 `<input>`만이 탭을 통해 포커스를 받을 수 있다.
3. 방향키를 사용하여 다른 옵션을 선택할 수 있다. 어떤 라디오 버튼을 선택해도 sort by가 같이 읽힌다. 다른 페이지에 갔다 오더라도 이를 통해 정렬 기능을 사용하고 읽었다는 것을 떠올릴 수 있다.
4. 자바스크립트에 의존하지 않고 서버에서 페이지를 재구성하기 위해 이 단계에서는 폼에 GET 메서드 사용.

### CSS 향상

버튼이 제공하는 스타일이 제한적이기 때문에 버튼 대신 완전한 비접근성의 `<div>`와 `<span>`기반의 해법 또는 상대적으로 깨지기 쉽고 복잡한 WRI-ARIA기반의 구현을 하는 이유다.

for, id의 관계 `<label>`을 클릭할 때 연결된 `<input>`에 작동함. 이는 클릭영역을 확장함을 의미한다.

```html
<input type="radio" name="sort-method" id="most-recent" value="most-recent" checked />
<label for="most-recent">most recent</label>
```

:focus, :checked t상태를 나타내기 위해 css를 사용하여 못생긴 radio 버튼을 숨긴다.

```css
[type="radio"] + label {
  cursor: pointer;
  /* 기본스타일 */
}

[type="radio"]:focus + label {
  /* 포커스 받았을 때 스타일 */
}

[type="radio"]:checked + label {
  /* 선택되었을 때 스타일  */
}
```

HTML과 CSS로 할 수 있는 일을 자바스크립트와 WAI-ARIA로 대체하지 말자.

### 자바스크립트의 향상

필터옵션을 선택하고 정렬을 누르면 페이지는 처음부터 다시 로딩된다.
키보드 사용자, 스크린 리더 사용자는 콘텐츠의 처음부터 다시 읽어야 한다는 말이다.
따라서 XHR를 통해 콘텐츠를 다시 채워지게 해야 한다. 이렇게 하면 페이지는 새로고침 되지 않고 콘텐츠만 추가되고 포커스도 그대로 남아 있게 된다.

#### 대기

로딩상태를 이미지로 표시하는 방법은 좋지 않다.
WAI-ARIA에서 이런 문제를 해결하기 위해 실시간 영역이라는 개념을 제공.

스크린 리더가 콘텐츠를 낭돋하는 경우

- 사용자나 프로그램이 요소에 포커스를 주는경우
- 사용자가 스크린 리더의 내비게이션 명령을 사용해 요소를 탐색하는 경우

실시간 영역은 콘텐츠가 변경되면 콘텐츠를 읽는다.

```html
<div aria-live="assertive" role="alert">
  Please wait. Loading products
</div>
```

```html
<div aria-live="assertive" role="alert">
  Loading complete. 23 products listed.
</div>
```

- aria-live="polite" 속성은 status 역할과 똑같다.
- DOM 조작 대신에 실시간 영역을 메시지로 채울 수 있다. liveElement.text-Contnent = 'messsage'
- assertive와 alert는 실시간 영역에서 메시지를 알리기 위해 스크린 리더의 현재 낭독이 중단될 수 있다는 뜻.
- 이와는 다르게 polite와 status는 스크린 리더의 현재 낭독이 완료된 후에 실시간 영역의 콘텐츠를 읽는다는 의미이다.

#### 제출 버튼의 퇴출

기본 폼 제출 기능을 억제함으로 페이지 새로고침을 예방할 필요가 있다.
```javascript
var sortForm = document.querySelector('.sorter');
sortForm.addEventListener('submit', function(event) {
  // 폼 제출을 XHR로 처리할 것이므로
  // 브라우저의 폼 제출을 잠시 미뤄둔다.
  event.prevntDefault();

  // XHR 처리 부분
})
```
제출 버튼을 제거하면 이벤트 발생할 때 XHR기능을 사용하게 만들 수 있다.

```javascript
var sortForm = document.querySelector('.sorter');
sortForm.addEventListener('change', function(event) {
  if(event.target.type !== 'radio') {
    return;
  }
  this.submit();

}, true);
```

이렇게 제출 행위를 없앴다면 사용자들은 의도치 않게 제어권을 박탈당한다고 느낄수 있다.
이는 옵션 사이에서 방향키를 움직일 때마다 change 이벤트를 발생시킨다는 의미이다.

```javascript
var sortForm = document.querySelector('.sorter');
sortForm.addEventListener('click', function(event) {
  if(event.target.type !== 'radio') {
    return;
  }
  this.submit();

}, true);
```

change 이벤트를 click 이벤트로 바꿈으로 마우스와 터치 사용자로 한정시킬 수 있다.
버튼이 없기 때문에 키보드 사용자는 엔터키를 사용하여 폼을 제출할 것임을 확신하는 의미밖에 안된다. ios 플랫폼에서는 제출 버튼이 없으면 제출을 할 수 없기 때문에 또 다른 처리를 해줘야 한다.

인클루시브 디자인에 대한 결정을 올바로 확정하기 위한 가장 좋은 방법은 더 다양한 테스트 그룹을 모집하는 것이다.

### 더 많은 결과의 로딩

무한스크롤: 인클루시브 디자인에서는 심각한 문제가 있다.
키보드 사용자에게는 포저스 가능한 요소가 자동으로 계속 추가되므로 shift+tab키를 사용해 메인콘텐츠를 향해 다시 거꾸로 올라가는 것이 불가능하다.

웬만하면 무한스크롤 기능을 안주는 것이 좋다.
정 구현하고 싶으면 더보기 버튼 기능을 통해 넣어주고 몇 번 반복하고 추가해주는 것이 좋다.

#### 더보기 버튼 넣기

data-load-more 버튼에서 새로 로딩된 첫 아이템으로 포커스가 이동하는 것이 중요하다.
많은 경우 버튼에서 포커스만 없애고 다른 곳으로 보내지 않는다. 이걸 정해주지 않으면 첫번째 콘텐츠로 올라가 버리게 된다.
더 보기 버튼 클릭 후 버튼 로딩으로 바꾸고 로딩이 다되면 다시 더보기 버튼으로 바꾼다.
실시간영역에 상태값을 표시해준다.

### 보기 옵션

리스트 대 그리드 2가지 보기를 설정해 줄 수 있도록 한다.

보강된 필터 위젯

```html
<form role="form" class="sorter" method="get">
  <fieldset>
    <legend>Sort by</legend>
    <input type="radio" name="sort-method" id="most-recent" value="most-recent" checked />
    <label for="most-recent">most recent</label>
    <input type="radio" name="sort-method" id="popularity" value="popularity" />
    <label for="popularity">popularity</label>
    <input type="radio" name="sort-method" id="price-lowhigh" value="price-low-high" />
    <label for="popularity">price(low to high)</label>
    <input type="radio" name="sort-method" id="price-highlow" value="price-high-low" />
    <label for="popularity">price(high to low)</label>
  </fieldset>

  <fieldset>
    <legend>Display as</legend>
    <label for="list">
      <svg><use xlink:href="#list-icon"></use><text class="visually-hidden">a list</text></svg>
    </label>
    <input type="radio" name="display-as" id="list" value="list" checked />
    <label for="grid">
      <svg><use xlink:href="#grid-icon"></use><text class="visually-hidden">a grid</text></svg>
    </label>
    <input type="radio" name="display-as" id="grid" value="grid" />
  </fieldset>
  <button type="submit">sort</button>
</form>
```

#### 자동조절 그리드

플렉스 박스를 사용하여 flex-basis를 통해 요소 수준에서 이상적인 너비를 정의
flex-grow와 flex-shrink로 그리드 요소의 이상적인 너비단위로 확장되거나 축소될 수 있다.

```css
.grid-display {
  display: flex;
  flex-wrap: wrap;
}

.grid-display li {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 10em;
}
```

- flex-basis로 각 아이템이 10em의 너비를 가지만 flex-grow가 설정되어 남은 공간을 완전히 나누어 갖게 된다.
- flex-basis값을 em 단위로 설정함으로 폰트 크기가 상대적이 되게 했다.
- flex-grow가 1이라는 것은 각 아이템 너비가 10em이 넘더라도 모든 공간을 채우도록 팽창한다는 의미.

#### 왼쪽 방향 그리드

언어를 설정했다면 아랍어 같은 일부 언어는 오른쪽으로 왼쪽으로 읽는 방식을 설정해 줄 수 있다.
`<html lang="ar" dir="rtl">`

플로트 기반 CSS 레이아웃은 이 장치에 영향을 받지 않는다. 플로트 콘텐츠의 레이아웃은 수동으로 뒤집어줘야 한다.

```css
.content {
  float: left;
  width: 60%;
}

[dir="rtl"] .content {
  float: right;
}

.sidebar { 
  float: right;
  width: 40%;
}

[dir="rtl"] .sidebar {
  float: left;
}

```

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

라이브러리들을 사용하는일이 많아서 그런지 컴포넌트를 따로 만드는 일을 잘하지 않게 되면서 이런 고민들을 잘 안하게 된 것 같다. 기존에 만든 라이브러리들이 이런것들을 잘 준수하고 만드는지 코드를 유심히 보고 컴포넌트를 만들 때 참고해야할 것 같다. 생각보다 신경써야 할 것들이 너무 많다.

## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

[빌어먹을 폼](http://wtfforms.com)

## 해시태그: 키워드

#인클루시브디자인 #필터위젯
