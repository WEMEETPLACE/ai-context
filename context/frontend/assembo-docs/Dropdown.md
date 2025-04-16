# Dropdown

`Dropdown` 컴포넌트는 사용자가 여러 옵션 중 하나를 선택할 수 있는 드롭다운 메뉴를 제공합니다. 이 컴포넌트는 여러 하위 컴포넌트의 조합으로 구성되어 다양한 커스터마이징을 지원합니다.

## 설치

```jsx
import {
  Dropdown,
  DropdownTrigger,
  DropdownInput,
  DropdownContent,
  DropdownItem
} from "@wemeetdev/assembo";
```

## 컴포넌트 구성

Dropdown은 다음과 같은 하위 컴포넌트로 구성됩니다:

1. **Dropdown**: 상태 관리 및 컨텍스트를 제공하는 루트 컴포넌트
2. **DropdownTrigger**: 드롭다운 메뉴를 열고 닫는 트리거 컴포넌트
3. **DropdownInput**: 기본 제공되는 입력 형태의 트리거 컴포넌트
4. **DropdownContent**: 드롭다운 메뉴 항목들을 포함하는 컨테이너
5. **DropdownItem**: 개별 드롭다운 항목을 나타내는 컴포넌트

## 기본 사용법

```jsx
import { useState } from "react";
import {
  Dropdown,
  DropdownTrigger,
  DropdownInput,
  DropdownContent,
  DropdownItem
} from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState(null);
  
  const options = [
    { label: "옵션 1", value: "option1" },
    { label: "옵션 2", value: "option2" },
    { label: "옵션 3", value: "option3" }
  ];

  return (
    <Dropdown value={selected} onChange={setSelected}>
      <DropdownTrigger>
        <DropdownInput placeHolder="선택하세요" />
      </DropdownTrigger>
      <DropdownContent>
        {options.map((option) => (
          <DropdownItem 
            key={option.value} 
            value={option}
          />
        ))}
      </DropdownContent>
    </Dropdown>
  );
}
```

## 아이콘이 있는 항목

```jsx
import { IconHome, IconSetting, IconPerson } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState(null);
  
  const options = [
    { label: "홈", value: "home", icon: <IconHome size={16} /> },
    { label: "설정", value: "settings", icon: <IconSetting size={16} /> },
    { label: "프로필", value: "profile", icon: <IconPerson size={16} /> }
  ];

  return (
    <Dropdown value={selected} onChange={setSelected}>
      <DropdownTrigger>
        <DropdownInput placeHolder="선택하세요" />
      </DropdownTrigger>
      <DropdownContent>
        {options.map((option) => (
          <DropdownItem 
            key={option.value} 
            value={option}
          />
        ))}
      </DropdownContent>
    </Dropdown>
  );
}
```

## 크기 설정

Dropdown 컴포넌트는 세 가지 크기를 지원합니다.

```jsx
// 작은 크기
<DropdownInput size="small" placeHolder="작은 드롭다운" />

// 중간 크기 (기본값)
<DropdownInput size="medium" placeHolder="중간 드롭다운" />

// 큰 크기
<DropdownInput size="large" placeHolder="큰 드롭다운" />
```

## 커스텀 트리거

기본 제공되는 `DropdownInput` 대신 커스텀 트리거를 사용할 수 있습니다.

```jsx
<Dropdown value={selected} onChange={setSelected}>
  <DropdownTrigger>
    <button className="custom-button">
      {selected ? selected.label : "선택하세요"} ▼
    </button>
  </DropdownTrigger>
  <DropdownContent>
    {/* ... 드롭다운 항목들 ... */}
  </DropdownContent>
</Dropdown>
```

## 상태에 따른 트리거 변경

드롭다운의 열림/닫힘 상태에 따라 트리거의 모양을 바꿀 수 있습니다.

```jsx
<Dropdown value={selected} onChange={setSelected}>
  <DropdownTrigger>
    {(isOpen) => (
      <button className={`custom-button ${isOpen ? "active" : ""}`}>
        {selected ? selected.label : "선택하세요"}
        {isOpen ? " ▲" : " ▼"}
      </button>
    )}
  </DropdownTrigger>
  <DropdownContent>
    {/* ... 드롭다운 항목들 ... */}
  </DropdownContent>
</Dropdown>
```

## 비활성화 상태

드롭다운 전체 또는 특정 항목을 비활성화할 수 있습니다.

```jsx
// 드롭다운 전체 비활성화
<Dropdown value={selected} onChange={setSelected} disabled>
  {/* ... */}
</Dropdown>

// 특정 항목만 비활성화
<DropdownItem 
  value={{ label: "비활성화 항목", value: "disabled-option" }}
  disabled
/>
```

## Props

### Dropdown Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `Object \| null` | `null` | 선택된 항목 값 |
| `onChange` | `(value: Object \| null) => void` | - | 항목 선택 시 호출되는 콜백 함수 |
| `disabled` | `boolean` | `false` | 드롭다운 비활성화 여부 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `style` | `CSSProperties` | - | 인라인 스타일 |
| `children` | `ReactNode` | - | 드롭다운 구성 요소 |

### DropdownTrigger Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `children` | `ReactNode \| (isOpen: boolean) => ReactNode` | - | 트리거로 사용할 요소 또는 함수형 자식 |
| `className` | `string` | - | 추가 CSS 클래스 |

### DropdownInput Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 입력 필드의 크기 |
| `icon` | `ReactNode` | - | 커스텀 아이콘 |
| `placeHolder` | `string` | - | 값이 없을 때 표시할 텍스트 |
| `className` | `string` | - | 추가 CSS 클래스 |

### DropdownContent Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 드롭다운 항목들 |

### DropdownItem Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `Object` | 필수 | 항목 데이터 (label, value, icon 등) |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 항목의 크기 |
| `disabled` | `boolean` | `false` | 항목 비활성화 여부 |
| `showChevron` | `boolean` | `true` | 선택 표시 아이콘 표시 여부 |
| `className` | `string` | - | 추가 CSS 클래스 |

## 동작 원리

Dropdown 컴포넌트는 다음과 같은 방식으로 작동합니다:

1. **상태 관리**: Context API를 사용하여 선택된 항목과 열림/닫힘 상태를 관리
2. **포지셔닝**: 드롭다운 메뉴는 트리거 요소를 기준으로 자동으로 위치가 결정됨
3. **상호작용**: 외부 클릭 및 ESC 키 감지로 드롭다운 자동 닫기
4. **접근성**: 적절한 ARIA 속성 및 키보드 상호작용 지원

## 접근성

- 키보드 네비게이션을 통해 드롭다운을 조작할 수 있습니다.
- ESC 키를 통해 드롭다운을 닫을 수 있습니다.
- 외부 클릭 시 드롭다운이 자동으로 닫힙니다.
- 적절한 ARIA 속성을 통해 스크린 리더 접근성을 지원합니다.

## 주의사항

- `DropdownItem`의 `value` 속성에는 최소한 `label`과 `value` 필드가 있어야 합니다.
- Dropdown 컴포넌트는 컴포넌트 구조(Dropdown > DropdownTrigger > DropdownContent)를 따라야 합니다.
- DropdownContent는 포털을 사용하여 렌더링되므로, z-index 관련 스타일 이슈가 발생할 수 있습니다.