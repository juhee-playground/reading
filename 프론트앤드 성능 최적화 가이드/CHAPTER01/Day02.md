# DAY 2

## 🔖 오늘 읽은 범위 : 1장 블로그 서비스 최적화

- 병목 코드 최적화
- 코드 분할 & 지연 로딩
- 텍스트 압축

---

<aside>
😃 **기억하고 싶은 내용**

</aside>

### 병목 코드 최적화

- Performance 패널 살펴보기

![CPU 차트, Network차트, 스크린샷](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f3752bb-8143-476c-940b-c4fa6f186387/Untitled.png)

CPU 차트, Network차트, 스크린샷

- CPU 차트

CPU가 어떤 작업에 리소스를 사용하고 있는지 비율을 보여 준다. 

자바스크립트 ⇒ 노란색

렌더링/레이아웃 작업 ⇒ 보라색

페인팅 작업 ⇒ 초록색

기타 ⇒ 회색

- Network 차트

CPU 차트 밑에 막대형태로 표현. 대략적인 네트워크의 상태를 표시. 

위쪽의 진한 막대는 우선순위가 높은 막대를 표시.

![Network 타임라인](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5aac5358-d2fb-4047-856d-044429570a38/Untitled.png)

Network 타임라인

- Network 타임라인

네트워크 패널과 유사하게 서비스 로드 과정에서 네트워크 요청을 시간 순서에 따라 보여 준다.
    - 왼쪽 회색 선: 초기 연결 시간
    - 막대의 옅은 색 영역: 요청을 보낸 시점부터 응답을 기다리는 시점까지의 시간(TTFB, Time to First Byte)
    - 막대의 짙은 색 영역: 콘텐츠 다운로드 시간
    - 오른쪽 회색 선: 해당 요청에 대한 메인 스레드의 작업 시간

![Frames, Timings, Main](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/01522468-000a-4d43-ab8c-9bfc839a2a8f/Untitled.png)

Frames, Timings, Main

- Frames
    화면의 변화가 있을 때마다 스크린샷을 찍어 보여준다.

- Timings
    User Timings API를 통해 기록된 정보를 기록.
    여기서 표시된 막대들은 리액트에서 각 컴포넌트의 렌더링 시간을 측정.

- Main 섹션은 브라우저의 메인 스레드에서 실행되는 작업을 플레임 차트로 보여준다. 이를 통해 어떤 작업이 오래 걸리는지 파악 가능하다.

![하단 탭](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/feb1b5e6-369e-47b9-b31f-0d9ad5d3c5e5/Untitled.png)

하단 탭

- summary
    선택 영역에서 발생한 작업 시간의 총합과 각 작업이 차지하는 비중을 보여준다.

- Bottom up
    가장 최하위에 있는 작업부터 상위 작업까지 역순으로 보여준다.

- Call Tree
    Bottom up 과 반대로 가장 상위 작업부터 하위 작업 순으로 작업 내용을 트리뷰로 보여준다.

- Event Log
    발생한 이벤트를 보여준다.

⇒ Reduce JavaScript execution time: 자바스크립트 실행 시간 줄이기 위해 제안 하는 것.

### 코드 분할 & 지연 로딩

- 번들 파일 분석 webpack을 통해 번들링된 파일을 분석하고 최적화
- 코드분할: 필요한 코드만 따로 로드하면 불필요한 코드를 로드하지 않아 더욱 빨라진다.
- 페이지별로 코드를 분리. 하나의 번들 파일을 여러 개의 파일로 쪼개는 방법.
- 코드 분할을 하는 가장 좋은 방법은 **동적 import** 를 사용하는 방법.

⇒ import 문을 사용하면 빌드할 때가 아닌 런타임에 해당 모듈을 로드합니다.

```jsx
import { add } from "./math"

console.log("1 + 4 =" add(1,4))

// 빌드시에 함께 번들링 됨.
```

```jsx
import ( add ).then((module) => {
    const { add } = module 
    console.log("1 + 4 =" add(1,4))
}
// 빌드할 때가 아닌 런타임에 해당 모듈을 로드한다.
```

- 동적 import 의 문제점: Promise 형태로 모듈을 반환해 준다.
import 하려는 모듈은 컴포넌트 이기 때문에 Promise 내부에서 로드된 컴포넌트를 Promise 밖으로 빼내야 한다. ⇒ 리액트 이를 해결 하기 위해 lazy와 Sus-pense 제공
- 지연로딩: 분할된 코드는 사용자가 서비스를 이용하는 중 해당 코드가 필요해지는 시점에 로드되어 실행.

### 텍스트 압축

- 압축방식
- Gzip

블록화, 휴리스틱 필터링, 헤더와 체크섬과 함께 내부적으로 De-flate를 사용하는 압축 방식

- Deflate

LZ77이라는 알고리즘과 허프먼 코딩을 사용하여 데이터를 감싸는 매우 인기 있는 압축방식

- npm serve 옵션값
- -s 옵션은 SPA 서비스를 위해 매칭되지 않는 주소는 모두 index.html로 보낸다는 옵션
- -u 텍스트 압축을 하지 않겠다는 옵션

<aside>
🤔 **오늘의 파트에 대한 소감**

</aside>

- Network 패널과 performance 패널이 익숙치 않아서 항상 기피했는데 하나하나 살펴보니 그래도 이젠 막 저건 안봐 이런 느낌은 들지 않는다. 다만 더 자주 저 패널들을 보는 연습이 필요한 것 같다.

<aside>
🔎 앞으로 할일 및 궁금한점.

</aside>

빌드할 때랑 런타임 할 때의 차이는?

쉽게 설명해주시오.

**토이프로젝트에선 (숙제)**

- serve가 없고 start만 있고 craco??를 쓴다?? 물어보기 텍
- main.js 텍스트 압축 시도해보기.
- 라우터 관련 동적 import 로 바꿔주기 lazy와 Sus-pense