---
sticker: emoji//2b50
tags:
  - react
  - useState
  - rerendering
---
리액트 컴포넌트 라이프사이클은 마운트, 업데이트, 언마운트로 나뉨.
### 핵심 답변
리액트 렌더링 과정은 초기 렌더링과 리렌더링으로 나누어볼 수 있음.
![[Pasted image 20250220234019.png]]

리렌더링) useState의 상태 변경 시에는 다음 프로세스가 발생합니다.

1. `setState` 호출 → 새로운 상태값 큐잉
2. React가 리렌더링 스케줄링
3. 컴포넌트 함수 재실행 
4. Virtual DOM 생성/비교
5. 필요한 부분만 실제 DOM 업데이트
#### 주요 특징
- 상태 업데이트는 비동기적
- 배치(batch) 처리로 여러 업데이트 최적화
- 이전 상태 값을 기반으로 업데이트 시 함수형 업데이트 권장

### 자세한 답변
[fiber + 리액트 리렌더링](https://velog.io/@yiseungyun/리액트의-Fiber를-모르는-Chill-guy일-때)

1. **상태 변경 감지**
   * `useState` 훅을 사용하면, 리액트는 내부적으로 해당 컴포넌트의 상태를 추적합니다.
   * 상태 업데이트 함수(`setState`)가 호출되면, 리액트는 해당 컴포넌트를 "`더티`(dirty)"로 표시합니다.
   * Closure를 통한 상태 값 보존 메커니즘
2. **렌더링 큐**
   * 리액트는 상태 변경이 발생한 컴포넌트들을 렌더링 큐에 추가합니다.
   * 동시성(Concurrent) 모드 : 렌더링 작업의 우선순위를 동적으로 조정
	   * Concurrent Rendering의 작동 방식
	   * `startTransition`의 동작 방식
	   - `Suspense`와의 연계
   * 이 과정은 비동기적으로 이루어져, 여러 상태 변경을 배치 처리할 수 있습니다.
3. **재조정(Reconciliation)**
   * 렌더링 단계에서 리액트는 "`더티`" 컴포넌트들을 새로 렌더링합니다.
   * 이 때 가상 DOM 트리를 새로 생성하고, 이전 트리와 비교합니다(Diffing).
   * Render Phase : 가상 DOM 트리 생성 및 비교
   * Commit Phase : 실제 DOM 업데이트 수행
   - 작업 우선순위에 따른 렌더링 작업을 중단하고 재개 가능(인터럽트)
4. **부분 렌더링**
   * Diffing 과정에서 변경된 부분만 식별됩니다.
   * 리액트는 이 정보를 사용해 실제 DOM에서 필요한 부분만 업데이트합니다.
5. **최적화**
   * `React.memo`, `useMemo`, `useCallback` 등을 통해 불필요한 리렌더링을 방지할 수 있습니다.
   * 이들은 `props`나 의존성이 변경되지 않았을 때 컴포넌트나 값의 재계산을 막습니다.
6. **Fiber 아키텍처**
   * 리액트의 Fiber 아키텍처는 렌더링 작업을 작은 단위로 나누어 우선순위를 관리합니다.
   * 이를 통해 중요한 업데이트를 먼저 처리하고, 렌더링 과정을 중단하거나 재개할 수 있습니다.
   - Double Buffering으로 current tree와 workInProgress tree를 관리
   - `child`, `sibling`, `return` 포인터로 fiber 노드들을 연결하는 구조

먼저 상태 변경 감지 단계를 살펴보겠습니다. React에서 useState 훅을 사용하면, React는 해당 컴포넌트의 상태를 내부적으로 추적합니다. 컴포넌트에서 setState 함수를 호출하면, React는 해당 컴포넌트를 '더티(dirty)'로 표시합니다. 이때 클로저(Closure)를 사용하여 이전 상태 값을 안전하게 보존합니다.
![[Pasted image 20250220233550.png]]
그 다음은 렌더링 큐 단계입니다. React는 상태가 변경된 컴포넌트들을 렌더링 큐에 추가합니다. 동시성(Concurrent) 모드에서는 렌더링 작업의 우선순위를 동적으로 조정할 수 있습니다. startTransition API를 사용하면 우선순위가 낮은 업데이트를 표시할 수 있고, Suspense와 함께 사용하면 로딩 상태를 더 세련되게 처리할 수 있습니다. 이 모든 과정은 비동기적으로 처리되어 여러 상태 변경을 효율적으로 배치 처리합니다.

재조정(Reconciliation) 단계는 두 개의 주요 페이즈로 나뉩니다. Render Phase에서는 더티로 표시된 컴포넌트들을 새로 렌더링하고 가상 DOM 트리를 생성하여 이전 트리와 비교(Diffing)합니다. Commit Phase에서는 실제 DOM을 업데이트합니다. React는 작업 우선순위에 따라 이 렌더링 작업을 필요한 경우 중단하고 나중에 재개할 수 있습니다.

부분 렌더링은 성능 최적화의 핵심입니다. Diffing 과정에서 변경된 부분만 정확히 식별하여, 실제 DOM에서도 필요한 부분만 업데이트합니다. 이는 불필요한 DOM 조작을 최소화하여 성능을 향상시킵니다.

React는 여러 최적화 도구도 제공합니다. React.memo는 props가 변경되지 않은 컴포넌트의 리렌더링을 방지합니다. useMemo와 useCallback은 각각 계산 비용이 큰 값과 함수의 불필요한 재생성을 방지합니다.

마지막으로 Fiber 아키텍처는 이 모든 과정의 기반이 됩니다. 렌더링 작업을 작은 단위(fiber)로 나누어 우선순위를 관리하며, Double Buffering 기법으로 current tree와 workInProgress tree를 관리합니다. 각 fiber 노드는 `child`, `sibling`, `return` 포인터로 연결되어 있어 효율적인 트리 순회가 가능합니다.
current 트리는 현재 화면에 보이는 UI를 나타내는 Fiber 트리다.
workInProgress 트리는 업데이트를 준비하는 작업용 트리다.
