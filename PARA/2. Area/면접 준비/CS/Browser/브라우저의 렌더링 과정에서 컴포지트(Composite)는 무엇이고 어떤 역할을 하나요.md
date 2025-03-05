---
sticker: emoji//2b50
tags:
  - browser
  - rerendering
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
	- 영향 범위 : `width`, `height`, `margin`, `padding`, `border`, `position`, `font-size` 등
	- 레이아웃 속성 변경 시 전체 문서의 레이아웃을 다시 계산해서 그럼
- **레이어 수 관리** : 과도한 레이어는 메모리 사용량 증가
	- 영향 범위 : `will-change`, `transform: translateZ()`, `position: fixed` 등
	- 각 레이어는 별도의 메모리 공간 필요
- **`transform`/`opacity` 활용** : GPU 가속 활용
	- GPU 처리 가능해 Reflow나 Repaint 발생하지 않음
- **`will-change` 속성 전략적 사용** : 변경이 예상되는 속성 미리 알리기
	- 변경이 예상되는 CSS 속성을 지정해 필요한 리소스를 미리 할당, 브라우저가 변경을 미리 준비 가능
	- 애니메이션 시작 시 지연 감소

GPU 처리는 렌더링의 마지막 단계이지만, 현대 웹에서 렌더링은 연속적인 프로세스입니다. 한 프레임의 렌더링이 끝나더라도, 다음과 같은 상황에서 새로운 렌더링 사이클이 시작됩니다.

1. **변경 트리거 요인**
	- 사용자의 마우스/키보드 입력
	- JavaScript 코드 실행으로 인한 DOM 변경
	- CSS 애니메이션 실행
	- AJAX 등으로 새로운 데이터 수신
2. **렌더링 프레임 관리**
	- 브라우저는 60fps(프레임당 16.67ms) 성능을 목표로 함
	- requestAnimationFrame API를 통해 다음 프레임 렌더링을 스케줄링
	- 성능 최적화를 위해 변경된 부분만 선택적으로 렌더링
3. **최적화된 렌더링 경로**
	- 모든 변경사항이 전체 렌더링 파이프라인을 실행하지는 않음
	- CSS transform과 같은 특정 속성은 컴포지팅(compositing) 단계만 실행
	- 브라우저가 자동으로 가장 효율적인 렌더링 경로를 선택

따라서 웹 페이지의 렌더링은 한 번의 GPU 처리로 끝나는 것이 아니라, 사용자 상호작용과 콘텐츠 변경에 따라 지속적으로 반복되는 프로세스라고 할 수 있습니다.

\+ 리액트 리렌더링이랑 같이 생각하면 좋음

[직관적인 유튜브](https://youtu.be/R23JmhbPnVo?si=tDNr3i1_8FH3Pi-d)