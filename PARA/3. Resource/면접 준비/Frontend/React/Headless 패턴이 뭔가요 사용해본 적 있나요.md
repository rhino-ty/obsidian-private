---
aliases: []
---
- **응높결낮**
	- 응집도는 모듈 내의 요소들이 얼마나 밀접하게 관련
	- 결합도는 모듈 간의 상호 의존성을 나타내며, 낮은 결합도가 이상적

"Headless 패턴은 UI 컴포넌트의 로직과 스타일을 분리하는 설계 방식입니다. 저는 실시간 드로잉 프로젝트에서 Canvas 도구를 구현할 때 이 패턴을 활용했습니다.

예를 들어, 펜 도구의 경우 선 그리기 로직을 useDrawingTool이라는 훅으로 분리했습니다. 이 훅은 마우스 이벤트 처리, 좌표 계산, Canvas 조작 같은 핵심 로직을 담당하고, 실제 UI 렌더링과 스타일링은 각 컴포넌트에서 자유롭게 구현했죠.

이렇게 분리함으로써 얻은 장점이 있었는데요. 먼저 동일한 드로잉 로직을 펜, 형광펜 등 다양한 도구에 재사용할 수 있었고, 새로운 도구를 추가할 때도 기존 로직을 변경하지 않고 UI만 구현하면 되어 확장이 쉬웠습니다. 또한 로직을 분리해서 테스트하기도 수월했죠.

이 경험을 통해 Headless 패턴이 복잡한 인터랙션을 다루는 컴포넌트를 설계할 때 특히 유용하다는 것을 배웠습니다."
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