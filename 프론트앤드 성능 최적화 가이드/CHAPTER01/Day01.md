# DAY 1

## 🔖 오늘 읽은 범위  1장 블로그 서비스

<http://github.com/performance-lecture/lecture-1>

- 실습 내용 소개
- 서비스 탐색 및 코드 분석
- Lighthouse 툴을 이용한 페이지 검사
- 이미지 사이즈 최적화

    <aside>

    😃 **기억하고 싶은 내용**
    </aside>

### 최적화 기법

- 이미지 사이즈 최적화
- 코드 분할
- 텍스트 압축
- 병목 코드 최적화

### 분석 툴 소개

- 크롬 개발자 도구
  - Network 패널
  - Performance 패널
  - Lighthouse 패널

### LightHouse

![lighthouse 패널 초기화면](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e19360f6-c6b0-4b51-94d8-d73cf46c376f/Untitled.png)

lighthouse 패널 초기화면

- Mode
  - Navigation: Lighthouse의 기본 값, 초기 페이지 로딩 시 발생하는 성능 문제를 분석
  - Timespan: 사용자가 정의한 시간 동안 발생한 성능 문제를 분석
  - Snapshot: 현재 상태의 성능 문제를 분석
- Categories
  - Performance: 웹 페이지의 로딩 과정에 발생하는 성능 문제를 분석
  - Accessibility: 서비스의 사용자 접근성 문제를 분석
  - Best practices:  웹 사이트의 보안 측면과 웹 개발의 최신 표준에 중점을 두고 분석
  - SEO: 검색 엔진에서 얼마나 잘 크롤링되고 검색 결과에 표시되는지 분석
  - Progressive Web App: 서비스 워커와 오프라인 동작 등, PWA와 관련된 문제를 분석
- Device
  - Mobile: 모바일 사이즈의 화면에서 좀 더 느린 CPU와 네트워크 환경으로  검사 진행
  - Desktop: 데스크톱 환경

![검사 결과 6가지 지표 표시 ⇒  웹 바이탈](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2d3a909c-8841-497b-9d36-287b4f30f2e3/Untitled.png)

검사 결과 6가지 지표 표시 ⇒  웹 바이탈

### Metrics

- FCP(First Contentful Paint)

    페이지가 로드 될 때 브라우저가 DOM 콘텐츠의 첫 번째 부분을 렌더링 하는데 걸리는 시간에 관한 지표.

    **총점 가중치 10%**

- SI(Speed Index)

    페이지 로드 중에 콘텐츠가 시각적으로 표시되는 속도를 나타내는 지표

    총점 가중치 10%

- LCP(Latest contentful Paint)

    페이지가 로드 중 화면 내의 가장 큰 이미지나 텍스트 요소가 렌더링 될 때까지 걸리는 시간을 나타내는 지표

    **총점 가중치 30%**

- TTI(Time to Interactive)

    사용자가 페이지와 상호작용이 가능한 시점까지 걸리는 시간을 측정한 지표

    **총점 가중치 10%**

- TBT(Total Blocking TIme)

    페이지가 클릭, 키보드 입력 등의 사용자의 입력에 응답하지 않도록 차단된 시간을 총합한 지표

    **총점 가중치 30%**

- CLS(Cumulative Layout Shift)

    페이지 로드 과정에서 발생하는 예기치 못한 레이아웃 이동을 측정한 지표.

    화면상에서 요소의 위치나 크기가 순간적으로 변하는 것을 말함.

    **총점 가중치 15%**

### 이미지 사이즈 최적화

- 비효율적인 이미지 분석

    ⇒ Properly size image : 이미지를 적절한 사이즈로 사용하도록 제안하는 것.

  - 서버에서 받아오는 이미지를 줄이는 방법은?

        이미지 CDN : 물리적 거리의 한계를 극복하기 위해 소비자가 가까운 곳에 콘텐츠 서버를 두는 기술

<aside>
🤔 **오늘의 파트에 대한 소감**

</aside>

- 어렵게 생각했던 부분들이 예제를 들어주면서 알려주니까 조금은 쉽게 느껴진다.
- 이 책을 잘 산 것 같다는 느낌이 든다.
- 실습을 해봐야 더 제대로 이해할 수 있을 것 같다.

<aside>
🔎 **추가로 알게 된 것 (책 내용 +알파로 궁금한 것, 이해가 가지 않는 것 등)**

</aside>

- 플레임 차트?

    소프트웨어의 작업(스택)을 손쉽게 추적하기 위해 개발된 계층형 데이터 시각화 기법.

- 내 토이 프로젝트 분석해보자!!

    [오버프라임 공략 사이트](https://forwin.info/hero)

    결과는?

    ![paragon info 검사 결과 ](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/21747adf-f895-4d89-a9ad-0ffe052f1eb7/Untitled.png)

    paragon info 검사 결과

이 책 읽으면서 요 점수를 높여보자!!!

- Enable text compression ⇒ 서버로부터 리소스를 받을 때, 텍스트를 압축해서 받아라
- Reduce unused JavaScript ⇒ 사용하지 않는 자바스크립트 줄이기
- Serve images in next-gen formats ⇒ 서버 이미지 포멧?
