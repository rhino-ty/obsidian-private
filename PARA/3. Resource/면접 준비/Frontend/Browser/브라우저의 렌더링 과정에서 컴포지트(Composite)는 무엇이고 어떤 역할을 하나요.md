---
sticker: emoji//2b50
tags:
  - browser
---
"브라우저에서 웹 페이지가 그려지는 과정을 설명해주세요.", "Critical Rendering Path에 대해 설명해주세요.", "브라우저에서 컴포지트(Composite)란 무엇이고 어떤 과정을 거치나요?"

### 컴포지트까지의 렌더링 과정
#### 1. DOM 처리 단계
- **JavaScript 실행 & DOM/CSSOM 생성**
  - JavaScript가 실행되며 DOM 트리와 CSSOM 트리 생성
  - DOM은 HTML 파싱 결과, CSSOM은 CSS 파싱 결과
- **스타일 계산**
  - DOM과 CSSOM을 결합하여 계산된 스타일 결정
  - CSS 상속과 cascading 규칙 적용
#### 2. 레이아웃 단계
- **레이아웃 계산 (Reflow)**
  - 뷰포트 내 각 요소의 정확한 위치와 크기 계산
  - `width`, `height`, `margin` 등의 변경은 Reflow 발생
- **렌더 트리 생성**
  - 실제 화면에 표시될 요소만 포함
  - `display: none` 요소는 제외됨
#### 3. 페인트 & 레이어 단계
- **페인트 (Repaint)**
  - 픽셀 단위로 실제 시각적 요소 그리기
  - `color`, `background`, `shadow` 등의 변경은 Repaint만 발생
- **레이어화**
  - 성능 최적화를 위해 요소들을 개별 레이어로 분리
  - `will-change`, `transform: translateZ(0)` 등으로 레이어 생성 가능
#### 4. GPU 처리 단계
- **래스터화**
  - 각 레이어를 비트맵으로 변환
  - GPU에서 하드웨어 가속으로 처리 가능
- **컴포지트**
  - 모든 레이어를 올바른 순서로 합성
  - `transform`, `opacity` 변경은 이 단계에서만 처리돼 성능상 유리
### 성능 최적화 Tips
- **Reflow를 최소화** : `layout` 속성 변경 피하기
	- 영향 범위: `width`, `height`, `margin`, `padding`, `border`, `position`, `font-size` 등
- **레이어 수 관리** : 과도한 레이어는 메모리 사용량 증가
- **`transform`/`opacity` 활용** : GPU 가속 활용
- **`will-change` 속성 전략적 사용** : 변경이 예상되는 속성 미리 알리기