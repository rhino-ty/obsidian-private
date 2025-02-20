---
aliases: []
sticker: emoji//1fae5
---
- **응높결낮**
	- 응집도는 모듈 내의 요소들이 얼마나 밀접하게 관련
		- UI 컴포넌트, Container 컴포넌트, Hook, 상수 등으로 나누는 게 아닌 도메인별로, 그 속에서 하면 응집도 높아지는데?!
	- 결합도는 모듈 간의 상호 의존성을 나타내며, 낮은 결합도가 이상적 => 단일 책임

"Headless 패턴은 UI 컴포넌트의 로직과 스타일을 분리하는 설계 방식입니다. 프로젝트에서도 사용한 적이 있는데, 토스트나 모달 같은 컴포넌트를 만들 때 사용했었습니다.

스타일을 포함해 UI 렌더링과 실행되는 비즈니스 로직 등을 분리했었습니다.
```tsx
// Modal 로직
const useModal = (autoCloseDelay?: number) => {
  const [isModalOpened, setModalOpened] = useState(false);

  const openModal = () => {
    setModalOpened(true);
    if (autoCloseDelay) {
      return timer({ 
        handleComplete: closeModal, 
        delay: autoCloseDelay 
      });
    }
  };

  const handleKeyDown = (e: KeyboardEvent) => {
    if (e.key === 'Escape') closeModal();
  };

  return { openModal, closeModal, handleKeyDown, isModalOpened };
};

// Modal UI
const Modal = ({ title, isModalOpened, handleKeyDown, children }) => (
  <div
    className={cn(
      'fixed left-0 top-0 flex h-full w-full items-center justify-center',
      isModalOpened ? 'pointer-events-auto' : 'pointer-events-none'
    )}
    onKeyDown={handleKeyDown}
    tabIndex={0}
  >
    <div className="relative m-3 bg-violet-100">
      <h2>{title}</h2>
      {children}
    </div>
  </div>
);

// 사용 예시
const GameResult = () => {
  const { isModalOpened, openModal } = useModal(5000); // 5초 후 자동 닫힘

  return (
    <Modal 
      title="게임 결과" 
      isModalOpened={isModalOpened}
    >
      <div>결과 내용...</div>
    </Modal>
  );
};
```

1. **중앙 집중화된 상태 관리**: Toast나 Modal의 상태를 전역적으로 관리하면서도, UI는 유연하게 커스터마이징 가능했습니다.
2. **일관된 동작 보장**: 키보드 접근성이나 자동 닫힘 같은 공통 기능을 로직 레벨에서 보장할 수 있었습니다.
3. **쉬운 기능 확장**: 새로운 스타일의 Toast나 Modal이 필요할 때 로직을 수정하지 않고 UI만 추가하면 됐습니다.
4. **코드 재사용**: 동일한 useModal 훅으로 게임 결과, 역할 공개, 도움말 등 다양한 모달을 구현할 수 있었습니다.


이 경험을 통해 Headless 패턴이 복잡한 인터랙션을 다루는 컴포넌트를 설계할 때 특히 유용하다는 것을 배웠습니다.
"

1. **결합도 감소**
	- UI와 로직의 분리로 직접적인 의존성이 줄어듭니다
	- 예를 들어 Canvas 로직(`useDrawing`)과 실제 캔버스 컴포넌트가 분리되어, 캔버스 UI를 변경해도 로직에는 영향이 없습니다
2. **응집도 증가**
	- 로직 계층에서는 상태 관리와 비즈니스 로직만 담당
	- UI 계층에서는 렌더링과 스타일링만 담당
	- 각 계층이 단일 책임을 가지게 되어 응집도가 높아집니다

```tsx
// 높은 응집도: 드로잉 상태 관리만 담당
const useDrawingState = () => {
  const [currentColor, setCurrentColor] = useState('#000000');
  const [brushSize, setBrushSize] = useState(4);
  const [inkRemaining, setInkRemaining] = useState(1000);
  // ...
};

// 높은 응집도: Canvas API 조작만 담당
const useDrawingOperation = () => {
  const drawStroke = useCallback(() => { /*...*/ }, []);
  const floodFill = useCallback(() => { /*...*/ }, []);
  // ...
};

// 높은 응집도: UI 렌더링과 이벤트 처리만 담당
const Canvas = () => {
  const { currentColor, brushSize } = useDrawing();
  return (
    <canvas 
      onMouseDown={handleDrawStart}
      // ...
    />
  );
};
```

---

- **테스트 용이성**: 로직이 분리되어 있어 Canvas API 모킹 없이도 상태 변화나 이벤트 처리를 테스트할 수 있었습니다.
- **재사용성**: 동일한 드로잉 로직을 일반 캔버스, 미니맵 등 다양한 UI에서 재사용할 수 있었습니다.
- **관심사 분리**: 실시간 동기화나 상태 관리 같은 복잡한 로직을 UI와 분리함으로써 코드의 가독성과 유지보수성이 향상됐습니다.
- **선언적 인터페이스**: 토스의 Slash 디자인 시스템처럼, 복잡한 내부 동작을 추상화하고 선언적인 API를 제공할 수 있었습니다.
## 기본 정의
Headless 패턴은 UI 컴포넌트의 로직(동작)과 표현(스타일)을 분리하는 디자인 패턴입니다. 'head'는 UI를, 'body'는 로직을 의미합니다.

```tsx
// 1. 기본적인 Toggle 컴포넌트
function BasicToggle() {
  const [isOn, setIsOn] = useState(false);

  return (
    <button 
      className={`toggle ${isOn ? 'active' : ''}`}
      onClick={() => setIsOn(!isOn)}
    >
      {isOn ? 'ON' : 'OFF'}
    </button>
  );
}

// 2. Headless 패턴을 적용한 Toggle
function useToggle(initialState = false) {
  const [isOn, setIsOn] = useState(initialState);
  
  return {
    isOn,
    toggle: () => setIsOn(!isOn),
    setOn: () => setIsOn(true),
    setOff: () => setIsOn(false)
  };
}

// 다양한 UI로 구현 가능
function SwitchToggle() {
  const { isOn, toggle } = useToggle();
  
  return (
    <div 
      className={`switch ${isOn ? 'bg-green' : 'bg-gray'}`}
      onClick={toggle}
    >
      <div className={`knob ${isOn ? 'translate-x-full' : ''}`} />
    </div>
  );
}

function CheckboxToggle() {
  const { isOn, toggle } = useToggle();
  
  return (
    <label className="checkbox">
      <input 
        type="checkbox"
        checked={isOn}
        onChange={toggle}
      />
      <span>{isOn ? '✓' : ''}</span>
    </label>
  );
}

// 3. 더 복잡한 Headless 컴포넌트 예시 - Select
function useSelect(items) {
  const [selectedItem, setSelectedItem] = useState(null);
  const [isOpen, setIsOpen] = useState(false);

  return {
    selectedItem,
    isOpen,
    select: (item) => {
      setSelectedItem(item);
      setIsOpen(false);
    },
    toggle: () => setIsOpen(!isOpen),
    getItemProps: (item) => ({
      onClick: () => select(item),
      'aria-selected': item === selectedItem
    }),
    getMenuProps: () => ({
      'aria-expanded': isOpen,
      'role': 'listbox'
    })
  };
}

// 다양한 스타일의 Select 구현
function DropdownSelect({ items }) {
  const {
    selectedItem,
    isOpen,
    select,
    toggle,
    getItemProps,
    getMenuProps
  } = useSelect(items);

  return (
    <div className="dropdown">
      <button onClick={toggle}>
        {selectedItem?.label ?? 'Select an item'}
      </button>
      {isOpen && (
        <ul {...getMenuProps()}>
          {items.map(item => (
            <li 
              key={item.id}
              {...getItemProps(item)}
            >
              {item.label}
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## 장점

1. **재사용성**
   - 동일한 로직을 다양한 UI로 구현 가능
   - 비즈니스 로직 중복 제거
   - 일관된 동작 보장

2. **유지보수성**
   - 로직과 UI 분리로 독립적인 수정 가능
   - 테스트 용이성
   - 관심사 분리 명확

3. **유연성**
   - 다양한 디자인 시스템과 호환
   - 커스텀 스타일링 자유도
   - 접근성 구현 용이

## 사용 사례

1. **Form 컴포넌트**
   - 유효성 검증
   - 상태 관리
   - 제출 처리

2. **UI 컨트롤**
   - Modal
   - Dropdown
   - Tabs
   - Accordion

3. **복잡한 인터랙션**
   - 드래그 앤 드롭
   - 무한 스크롤
   - 자동완성

더 자세히 알고 싶으신 특정 부분이 있으시다면 말씀해 주세요!