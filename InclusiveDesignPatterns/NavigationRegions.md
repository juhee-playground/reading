# 오늘 읽은 내용

네비게이션 영역

## TIL 3줄 요약

- 키보드 사용자들도 사이트를 쉽게 이용할 수 있도록 포커싱 체크 잘하기.
- 색상으로만 구분되게 하지마라.
- 현재위치를 사용자들에게 잘 알려주어야 한다.

## TIL (Today I Learned) 날짜

2024.05.01

## 오늘 읽은 범위

### 네비게이션 영역

콘텐츠를 다루는 마지막 두가지 패턴으로 콘텐츠관리와 작성방법에 관한 패턴이 있다.
네비게이션 랜드마크 = 메타와도 같다.
네비게이션은 콘텐츠를 위한 콘텐츠이다.

#### 네비게이션 랜드마크

웹페이지 개별 구역 영역, 블록, 모듈 등으로 불리며 페이지 안의 경계를 정하는 시각적인 박스를 나타낸다.

#### css 포지셔닝

소스 순서가 읽는 순서와 일치되다면 상관 없지만 우리에게는 어떤 것에 과잉 기술을 적용하거나 지난친 복잡성을 부여하는 버릇이 있다.

일반적으로 아주 보기 드문 상황이 아니라면 포지셔닝을 하지 말아야 한다.
특정이유 때문에 포지셔닝을 해야 한다면 매우 주의 하여야 하며 뷰포트 크기와 확대 설정 등 모든 범위에 대해 테스트 해야한다.

#### 현재위치

어디로 갈 것인지를 아는 것의 반은 지금 어디에 있는지를 아는 것이다.
nav 영역에서 현재 페이지를 식별하도록 하면 이용성이 더욱 높아진다.

#### 색상으로만 구분되게 하지마라

색상을 정보 전달, 행동지시, 반응 유발, 시각적 요소 식별을 위한 유일한 시각적 수단으로 사용하면 안된다.

색으로 nav를 구분하는 경우가 많다. 색맹 사용자는 구별하지 못할 수 있다. 밑줄 같은 또 다른 수단과 함께 사용하는 것이 좋다.

모호함을 완화하는 방법으로 대응요소와 aria-describedby 사용하기

> aria-describedby
> 일시 정지 후에 설명을 덧붙인다. 관계형 속성의 하나로 id를 통해 서술될 요소와 서술 요소를 연결한다. 서술 내용은 서술 요소의 텍스트 노드이다.

#### 중복링크를 건너뛰기 링크로

싱글페이지에서는 aria-describedby 사용이 더 어울린다.

#### 중복제거

Aria 역활을 주목하자

#### 차례

드롭다운 하위메뉴 보다 차례가 더 좋다.

두가지 이점

- 긴 형식의 콘텐츠 요약을 할 수 있다.
- 특정 절로 이동할 수 있는 내비게이션이 제공된다.

드롭다운이 안 좋은 이유

- 상호작용이 활성화된 불안정한 메뉴에서 내비게이션 콘텐츠를 가려버린다.
- 정보 설계자가 관련 절의 그룹화와 명확한 계층화 작업 없이 다수의 독립된 콘텐츠 페이지를 만들어내게 할 수 있다.

#### 순차 포커스 네비게이션

대상 내부 또는 다음에 있는 첫번째 포커스 가능 요소가 다음 순서의 포커스 가능요소가 된다.

#### 링크 재킹

키보드 인클루시브한 자바스크립트 인터페이스를 만들 때 키보드 사용자가 원하는 위치로 이동하도록 포커스 관리가 확실히 되어야 한다.

## 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

색맹을 가진 사용자에 대해선 전혀 고려해보지 못했던 것 같다.
보통 회사에서나 개인 프로젝트나 네비게이션에 선택됨을 색으로만 구분했음이 생각 나서 수정해야되는데 어떻게 해야 시각적으로 이상하지 않으면서 색맹 사용자에게 선택됨을 알려줄 수 있을까 더 생각해봐야 할 것 같다.

## 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

스키마타: 인간의 뇌는 감각자료를 이해하기 위해 현재 경험의 해석을 위해 과거 경험이 구조화된 것으로 여겨진다. 프로그래밍 용어로 이해를 위한 일종의 캐시라도 생각하면 된다.

색맹 사용자가 웹페이지를 어떻게 보이는지 확인하려면 
맥 os: 시스템 환경설정 => 손쉬운 사용 => 디스플레이 => 흑백 음영 사용 체크

책 닐슨노먼그룹의 드롭다운 디자인 지침

## 해시태그: 키워드
#인클루시브디자인 #네비게이션영역