# 오늘 읽은 내용

The Document

## TIL 3줄 요약

- 인클루시브 디자인의 핵심은 특별한 집단을 대상으로 삼는 것이 아니라 어떤 집단이든 함부로 배제하지 않는다는 것.
- 더 완벽한 인클루시브 디자인은 혼자가 아니라 디자이너가 좋은 조력자가 되어줄 것이다.
- 서로 다른 설정을 사용해 나, 나와 같은 사람, 그리고 나와 다른 사용자에게 득이 된다.

## TIL (Today I Learned) 날짜

2024.04.29

## 오늘 읽은 범위

### lang 속성
lang='en' 라고 쓰여져 있던걸 지운적이 있다. 이 별것 아닌 속성이 번역API나 점자디스플레이에 도움을 준다는 것을 몰랐다.

### 반응형 디자인

#### 핀치줌 허용
줌을 하지 못하는 일은 인클루시브 디자인을 약화시킨다.
- 사용자가 읽기 힘든 정도로 텍스트가 작아질 수 있다.
- 사용자는 이미지를 더 자세히 보고 싶어할 수 있다.
- 텍스트가 클수록 단어를 선택해 복사나 붙여 넣기하기 편할 수 있다.
- 등

인클루시브 디자인을 잘 완성하려면 디자이너는 조력자가 되어야 한다.
사용자를 위한 결정을 적게할수록 사용자는 스스로 할 수 있는 것이 많아진다.

### 폰트크기

```css
html {
  /* 이렇게 하지 말 것 
  이유: 자신의 취향대로 폰트 크기를 조정할 수 있는 사용자의 권한을 훼손하는 것이다
  */
  font-size: 16px;
}
```

상대적인 크기 단위 rem, em 을 사용해야 한다.

**뷰포트 단위**
뷰 포트의 높이(vh)와 너비(vw)를 기준으로 텍스트 크기를 조정할 수 있게한다.

```css
html { font-size: calc(1em + 1vw); }
```

이와 같은 알고리즘을 사용하면 모든 것의 크기는 뷰포트에 대해 점진적이고 비례적으로 조절된다.

### 점진적 향상

수 많은 네트워크와 스크맂트 에러에도 견딜 수 있는 논리적이고 강력한 형식으로 콘텐츠의 탄탄한 토대를 만드는 일과 관련이 있다.

스크립트는 문서의 끝, 즉 </body> 태그 직전에 삽입되어여 한다.
스크립트가 실행되는 시점에는 이미 DOM렌더링이 끝난 후임을 보장할 수 있기 때문에

### 자원관리

페이지 콘텐츠를 향상시키기 위해 사용하는 자원이 그 콘텐츠에 방해가 되지 않게 해야 한다.
네트워크가 느린 환경이라도 콘텐츠는 가능한 한 빨리 사용자에게 도달해야 한다.
**웹 폰트** 점진적 차원에서 다루어야 하는 큰 자원이다.

### <title> 요소

웹 사이트 이름 | 검색 결과에 나타날 내용

### <main> 요소

```html
<main id="main">
  <!-- 이 페이지만의 콘텐츠 -->
</main>
```

main태그의 id 속성은 사용자가 건너뛰기 링크를 사용할 수 있도록 하기 위함이다.

<main>은 페이지의 핵심 내용을 담을 목적으로 설계 되었다.
싱글 페이지 어플레케이션이라면 한번만 사용되어야 한다.


### 모두 합치기
여기서 나온 것들을 모두 합친다면
```html
<!DOCTYPE html>
<!-- 이 페이지의 주요 언어 선언 -->
<html lang='en'>
  <head>
    <meta charset='utf-8'>
    <!-- 줌 기능ㅇ을 막지 않는 뷰포트 선언 -->
    <meta name='viewport' content='width=device-width, initial-scale=1.0'>
    <!-- 베이스64로 인코딩된 논블로킹 폰트 지원 -->
    <link rel='stylesheet' href='font.css' media='none' onload='if(media != "all")'>
    <noscript><link rel='stylesheet' href='main.css'></noscript>

    <!-- 페이지를 설명하는 레이블 -->
    <title>Inclusive Design Template | Heydon's Site</title>
  </head>
  <body>
    <!-- 키보드 사용자를 위한 건너뛰기 링크 -->
    <a href='#main'>skip to main content</a>

    <!-- 로고, 페이지 네비게이션 -->
    <main id='main'>
      <!-- 페이지 고유의 콘텐츠 -->
    </main>

    <!-- 논 블로킹 자바스크립트 자원 -->
    <script src='scripts.js'></script>
  </body>
```

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

내가 편하기 위해 개발 했던 것들은 분명히 누군가에게 불편함으로 작용하게 된다.
이 책은 생각해보지 못한 것들에 대해 계속 이야기 한다.
내가 단순하게 생각하면서 모르고 지나쳤던 것들이 아주 많다.
앞으로 책을 읽으면서 이런 것들을 하나씩 줄여야 겠다.

## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

-

## 해시태그

#인클루시브디자인 #문서 