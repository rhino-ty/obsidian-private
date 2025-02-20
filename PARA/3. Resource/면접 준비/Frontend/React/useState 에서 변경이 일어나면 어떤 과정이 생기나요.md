---
sticker: emoji//1faf5
tags:
  - react
  - useState
---
### 핵심 답변
useState의 상태 변경 시에는 다음 프로세스가 발생합니다.

1. setState 호출 → 새로운 상태값 큐잉
2. React가 리렌더링 스케줄링
3. 컴포넌트 함수 재실행 
4. Virtual DOM 생성/비교
5. 필요한 부분만 실제 DOM 업데이트
#### 주요 특징
- 상태 업데이트는 비동기적
- 배치(batch) 처리로 여러 업데이트 최적화
- 이전 상태 값을 기반으로 업데이트 시 함수형 업데이트 권장
  ```javascript
  setCount(prev => prev + 1) // 안전한 업데이트
  ```