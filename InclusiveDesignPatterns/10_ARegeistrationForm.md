# 오늘 읽은 내용

등록 폼

## TIL 3줄 요약

- 첫번째로 읽어야 할 콘텐츠를 마지막에 위치 시키지 말아라.
- 폼에 시각적인 특징 뿐만 아니라 다른 요소도 추가하여 알려주어야 한다.
- 폼에 레이블을 불필요하다고 생각하고 생략하지 않기.

## TIL (Today I Learned) 날짜

2024.05.05

## 오늘 읽은 범위

### 등록 폼

웹 품은 입력 부분에 신중을 기해야 한다. 특히 폼이 인클루시브해야 한다는 점이 가장 중요하다.
모든 사람은 단순히 웹을 소비만 하는 것이 아니라 기여도 하기 때문이다.

폼요소는 OS 기능에 편승해야 하며 표준적인 방법으로 키보드와 스크린 리더에 대한 접근성을 갖추어야 한다.

### 현재 맥락에서의 폼

보통 로그인 화면에서 회원가입 링크가 로그인 버튼 아래 쪽에 있는 경우가 많은데
모든 사용자를 위해 명쾌한 해법은 처음 위쪽에서부터 로그인과 회원가입을 탭으로 구분해주는 것이 좋다.

```html
<h1>Welcome</h1>
<div role="toolbar" aroa-label="Login or register">
  <button aria-pressed="true">Log in</button>
  <button aria-pressed="false">Register</button>
</div>
<div id="forms">
  <div id="login">
    <form>
      <!-- 로그인 폼 -->
    </form>
  </div>
  <div id="register">
    <form>
      <!-- 등록 폼 -->
    </form>
  </div>
</div>
```

- 버튼을 누르면 그 버튼이 aria-pressed="true"로 바뀌며 해당 폼이 나타남.
- 스크린 리더 사용자가 첫번째 버튼에 포커스를 주면 "Login or Register toolbar, login toggle button, selected" 처럼 낭독된다.

### 기본 폼
```html
<form id="register">
  <label for="email">Your email address</label>
  <input type="text" id="email" name="email" />
  <label for="username">Choose a username</label>
  <input type="text" id="username" name="username" placeholder="e.g. HotStuff666" />
  <label for="password">Choose a password</label>
  <input type="password" id="password" name="password" />
  <button type="submit">Register</button>
</form>
```

#### 레이블

인클루시브 폼의 기본
모든 상호작용 가능한 요소에는 접근성이 있는 레이블이 있어야 한다.
for 속성을 갖는 `<label>` 요소를 사용 for 속성은 id 값을 통해 레이블 `<input>` 과 연결시킨다.

스크린 리더 사용자가 폼을 탐색하는 방법은 하나의 필드에서 다른 필드로 곧장 이동한다.
`<label>` 같은 요소는 건너뛰게 된다. 상호작용 요소에 명시적으로 연결시키지 않는다면 레이블은 놓치게 된다는 의미.

WAI-ARIA는 오직 시멘틱에만 영향을 미치고 일반적인 기능에 관여하지 않는다.

#### 플레이스홀더

사용자가 입력할 콘텐츠 유형에 대한 힌트를 사용자에게 미리 제공하기 위해 만들어짐.
기본적으로 placeholder는 회색 텍스트로 표시되는데 색대비가 아닌 이텔릭체 처럼 다른 방법으로 구별하는게 좋다.

```css
::placeholder {
  color: #000;
  font-style: italic;
}

::-webkit-input-placeholder {
  color: #000;
  font-style: italic;
}

::-moz-placeholder {
  color: #000;
  font-style: italic;
}
```

placeholder를 label로 대체하지말자.

### 필수항목

```html
<label for="email">
  Your email address
  <strong class="red" aria-hidden="true">*</strong>
</label>
<input type="text" id="email" name="email" aria-required="true" />
```

aria-arequired="true"로 인해 Required로 읽힌다. 그리고 * 표시는 숨기기 위해 aria-hidden="true"를 사용했다. 이는 음성 측면으로 display:none과 같다.

html에 required를 쓰지 않은 이유 브라우저마다 일관되게 구현되지 않았기 때문에

### 패스워드 보이기
```html
<label for="password">Choose a password</label>
<input type="password" id="password" name="password" />
<label>
  <input type="checkbox" id="showPassword" />
  showPassword
</label>
```

```javascript
var password = document.getElementById('password');
var showPassword = document.getElementById('showPassword');

showPassword.addEventListener('change', function() {
  var type = this.checked ? 'text' : 'password';
  password.setAttribute('type', type);
});
```

마이크로소프트 엣지는 눈모양의 아이콘을 통해 패스워드 보기 기능을 제공하는데, ::ms-reveal이라는 가상 클래스와 연결되어 있으므로 이 기능을 숨길 필요가 있다.

```css
input[type=password]::ms-reveal {
  display:none;
}
```

### 유효성 검증

유효성 검증 과정에서 관점을 분리시켜 두가지 핵심 메시지를 전달

1. 무엇이 잘못되었는가(폼에 어떤 오류가 있는가)?
2. 바로잡기 위해 무엇이 필요한가(유효한 폼으로 만들어 주는 것은 무엇인가)?

#### 무엇이 잘못되었는가

오류가 있다면 일시적으로 폼 제출을 막아야 한다.
클릭 핸들러만 막는 것이 아니라 폼 제출 자체도 막는다.

```javascript
var form = document.getElementById('register');
form.addEventListener('submit', function(event) {
  if(errors) {
    event.preventDefault();
  }
});
```

위에 것 + 실시간 영역에 에러임을 나타내주기

```javascript
var form = document.getElementById('register');
form.addEventListener('submit', function(event) {
  if(errors) {
    event.preventDefault();
    document.getElementById('error').textContent = "Please make sure all your registration information is correct";
  }
});
```

#### 시각적 효과

초기 상태에서 빈 박스를 보이지 않기 위해서 :empty라는 가상 클래스를 사용.

```css
#error:empty{
  display:none;
}
```

보통 에러는 관례대로 빨간색을 사용한다. 하지만 빨간색으로 표시하는 것은 시각적 특징일 뿐이라서 대체텍스트와 아이콘을 덧붙여 스크린 리더 사용자와 색을 구분하지 못하는 사용자에게 정보를 제공한다.

```html
<div id="error" aria-live="assertive" role="alert">
  <p>
    <svg role="img" aria-label="error:"><use xlink:href="#error"></use></svg>
    Please make sure your registration information is correct.
  </p>
</div>
```

img ARIA 역할과 aira-label이 `<svg>`요소를 'error:' 라는 값의 alt속성이 딸린 표준 `<img/>`요소와 비슷하게 만든다.

#### 바로잡기 위해 무엇이 필요한가

각 필드에 대해서 두가지 정보가 필요하다.

1. 필드가 유효하지 않다는 것.
2. 필드를 유효하게 만들 수 있는 것.

```html
<label for="password">Choose a password</label>
<input type="password" id="password" name="password" aria-invalid="true" />
<label>
  <input type="checkbox" id="showPassword" />
  showPassword
</label>
```

사용자가 폼으로 돌아가 입력 필드에 포커스를 주면 스크린리더가 invalid와 같은 식으로 읽는다.

```css
[aria-invalid="true"] {
  border-color: red;
  background: url() center right;
}
```

border-color로 변화를 감지하지 못하는 사용자를 위해 배경 아이콘을 추가 시킨다.

```html
<label for="password">Choose a password</label>
<input type="password" id="password" name="password" aria-invalid="true" aria-describedby="password-hint" />
<div id="password-hint">너 비밀번호 최소 6자리 이상이어야 해</div>
<label>
  <input type="checkbox" id="showPassword" />
  showPassword
</label>
```

#password-hint 요소는 aria-describedby 속성과 password-hint라는 id를 사용해 입력 필드와 연결된다.

#### 검증의 반복 실행

유효하디 않다고 표시된 필드에 대해 매 input 이벤트마다 검증을 실시해 aria-invalid 값을 false 또는 true로 전환할 수 있다.

```javascript
var password = document.getElementById('password');
password.addEventListener('input', function() {
  validate(this)
});
```

#### 디바운싱

validate함수를 계속 호출하는 것이 아니라 사용자가 잠깐의 시간을 가질 때만 호출되는 것이 더 좋다. 이를 디바운싱 기법을 사용하여 키 누름 이벤트를 블록화해 블록당 한 번만 validate()를 실행하게 만든다.

### 마이크로카피

코드와 시각디자인 뿐만 아니라 어휘의 선택에도 고심해야 한다.

나쁜 마이크로카피를 방지하는 다섯 가지 방법에서 빌 비어드가 핵심을제시

1. 머릿속에 있던 것은 지워버리고 사용자를 알도록 하라.
2. 사용자는 사람이다. 사람처럼 대화하라.
3. 다른 사람의 작품은 지침으로만 참고하고 의존하거나 모방하지 마라.
4. 실제로는 그렇지 않더라도 모든 모멘트를 브랜딩 모멘트처럼 여겨라.
5. 콘텐츠가 왕이라면 문맥은 여왕처럼 대하라.

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

폼에 시각적인 특징만을 넣어놨던 것 같다. 아이콘을 삽입해주어야 하고
보통 다 그렇게 사용하는 것들이어서 나도 그렇게 사용했던 점이지 않나 싶다.
한번 더 생각하고 만들어야 됨을 다시 한번 느꼈다.

## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

[나쁜 마이크로카피를 방지하는 다섯 가지 방법](http://smashed.by/bad-microcopy)

## 해시태그: 키워드

#인클루시브디자인 #등록폼