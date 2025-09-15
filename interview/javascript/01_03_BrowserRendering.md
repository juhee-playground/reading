### **1. 정의**

브라우저가 HTML, CSS, JavaScript를 받아서 화면에 실제 픽셀로 그리기까지 거치는 과정을 **Critical Rendering Path**라고 한다.

---

### **2. 중요한 이유**

- 이 과정을 이해하면 **성능 최적화** 포인트(CSS/JS 로딩 차단, 리플로우 최소화 등)를 알 수 있다.
- CSS나 JS 변경이 렌더링에 어떤 영향을 주는지 파악할 수 있어 **불필요한 Layout, Paint**를 줄이는 데 도움이 된다.

---

### **3. 단계별 과정**

1. **HTML 파싱 → DOM 생성**
   - HTML 태그를 해석해서 트리 구조(DOM)를 만든다.
2. **CSS 파싱 → CSSOM 생성**
   - CSS를 해석해서 CSSOM을 만든다.
3. **Render Tree 생성**
   - DOM + CSSOM을 결합해 실제 렌더링에 필요한 트리를 만든다.
   - display: none 같은 요소는 제외됨.
4. **Layout (Reflow)**
   - 각 요소의 위치와 크기를 계산한다.
5. **Paint**
   - 색상, 글꼴, 그림자 등 스타일을 픽셀로 그린다.
6. **Composite (합성)**
   - 레이어를 GPU에서 합성해 최종 화면을 만든다.

※ JS 실행이나 CSS 변경이 있으면 Layout → Paint → Composite 과정이 반복적으로 일어날 수 있다.
