# SelectButtons

`SelectButtons` 컴포넌트는 여러 옵션 중에서 선택할 수 있는 버튼 그룹을 제공합니다. 단일 선택과 다중 선택을 모두 지원하며, 버튼 형태의 직관적인 UI를 제공합니다.

## 설치

```jsx
import {
  SelectButton,
  SelectButtonGroup
} from "@wemeetdev/assembo";
```

## 컴포넌트 구성

SelectButtons는 다음과 같은 컴포넌트로 구성됩니다:

1. **SelectButtonGroup**: 여러 SelectButton을 그룹화하고 선택 상태를 관리하는 컨테이너
2. **SelectButton**: 개별 선택 버튼 컴포넌트

## 기본 사용법

### 다중 선택 모드 (기본)

```jsx
import { useState } from "react";
import { SelectButton, SelectButtonGroup } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState(["option1"]);

  return (
    <SelectButtonGroup value={selected} onChange={setSelected}>
      <SelectButton value="option1">옵션 1</SelectButton>
      <SelectButton value="option2">옵션 2</SelectButton>
      <SelectButton value="option3">옵션 3</SelectButton>
    </SelectButtonGroup>
  );
}
```

### 단일 선택 모드

```jsx
import { useState } from "react";
import { SelectButton, SelectButtonGroup } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState(["option1"]);

  return (
    <SelectButtonGroup 
      value={selected} 
      onChange={setSelected} 
      multiple={false}
    >
      <SelectButton value="option1">옵션 1</SelectButton>
      <SelectButton value="option2">옵션 2</SelectButton>
      <SelectButton value="option3">옵션 3</SelectButton>
    </SelectButtonGroup>
  );
}
```

## 크기 설정

SelectButton은 다양한 크기를 지원합니다.

```jsx
<SelectButtonGroup value={selected} onChange={setSelected}>
  <SelectButton value="option1" size="small">작은 버튼</SelectButton>
  <SelectButton value="option2" size="medium">중간 버튼</SelectButton>
  <SelectButton value="option3" size="large">큰 버튼</SelectButton>
</SelectButtonGroup>
```

## 비활성화 상태

전체 그룹 또는 개별 버튼을 비활성화할 수 있습니다.

```jsx
// 그룹 전체 비활성화
<SelectButtonGroup value={selected} onChange={setSelected} disabled>
  <SelectButton value="option1">옵션 1</SelectButton>
  <SelectButton value="option2">옵션 2</SelectButton>
  <SelectButton value="option3">옵션 3</SelectButton>
</SelectButtonGroup>

// 개별 버튼 비활성화
<SelectButtonGroup value={selected} onChange={setSelected}>
  <SelectButton value="option1">옵션 1</SelectButton>
  <SelectButton value="option2" disabled>옵션 2</SelectButton>
  <SelectButton value="option3">옵션 3</SelectButton>
</SelectButtonGroup>
```

## 커스텀 스타일링

```jsx
<SelectButtonGroup 
  value={selected} 
  onChange={setSelected}
  className="p-2 bg-gray-100 rounded"
>
  <SelectButton 
    value="option1"
    className="font-bold"
  >
    옵션 1
  </SelectButton>
  <SelectButton value="option2">옵션 2</SelectButton>
  <SelectButton value="option3">옵션 3</SelectButton>
</SelectButtonGroup>
```

## 아이콘과 함께 사용

```jsx
import { IconHome, IconSetting, IconPerson } from "@wemeetdev/assembo";

<SelectButtonGroup value={selected} onChange={setSelected}>
  <SelectButton value="home">
    <div className="flex items-center gap-2">
      <IconHome size={16} />
      <span>홈</span>
    </div>
  </SelectButton>
  <SelectButton value="settings">
    <div className="flex items-center gap-2">
      <IconSetting size={16} />
      <span>설정</span>
    </div>
  </SelectButton>
  <SelectButton value="profile">
    <div className="flex items-center gap-2">
      <IconPerson size={16} />
      <span>프로필</span>
    </div>
  </SelectButton>
</SelectButtonGroup>
```

## 클릭 이벤트 처리

개별 버튼에 추가 클릭 이벤트 핸들러를 설정할 수 있습니다.

```jsx
<SelectButtonGroup value={selected} onChange={setSelected}>
  <SelectButton 
    value="option1"
    onClick={() => console.log("옵션 1 클릭")}
  >
    옵션 1
  </SelectButton>
  <SelectButton 
    value="option2"
    onClick={() => console.log("옵션 2 클릭")}
  >
    옵션 2
  </SelectButton>
  <SelectButton 
    value="option3"
    onClick={() => console.log("옵션 3 클릭")}
  >
    옵션 3
  </SelectButton>
</SelectButtonGroup>
```

## Props

### SelectButtonGroup Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `Array<string \| number>` | 필수 | 선택된 값들의 배열 |
| `onChange` | `(value: Array<string \| number>) => void` | 필수 | 선택 항목 변경 시 호출되는 콜백 함수 |
| `multiple` | `boolean` | `true` | 다중 선택 가능 여부 |
| `disabled` | `boolean` | `false` | 그룹 전체 비활성화 여부 |
| `children` | `ReactNode` | 필수 | SelectButton 컴포넌트들 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `style` | `CSSProperties` | - | 인라인 스타일 |

### SelectButton Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `string \| number` | 필수 | 버튼의 값 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 버튼의 크기 |
| `disabled` | `boolean` | `false` | 버튼 비활성화 여부 |
| `onClick` | `(e: React.MouseEvent) => void` | - | 버튼 클릭 시 호출되는 추가 콜백 함수 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 버튼 내용 |

또한 HTML 버튼 요소의 모든 속성을 지원합니다.

## 선택 동작

- **다중 선택 모드(`multiple={true}`)**: 선택된 버튼을 다시 클릭하면 선택이 해제됩니다.
- **단일 선택 모드(`multiple={false}`)**: 이미 선택된 버튼을 다시 클릭하면 선택이 해제됩니다.

## 스타일 가이드

- 선택되지 않은 버튼: 기본 버튼 스타일
- 선택된 버튼: 파란색 배경과 흰색 텍스트
- 비활성화된 버튼: 회색 배경과 비활성화된 텍스트 색상
- 버튼 크기(size)에 따라 내부 패딩과 텍스트 크기가 자동으로 조정됩니다.

## 접근성

- 적절한 ARIA 속성을 사용하여 선택 상태를 스크린 리더에 전달합니다.
- `aria-checked` 속성을 통해 현재 선택 상태를 표시합니다.
- `aria-disabled` 속성을 통해 비활성화 상태를 표시합니다.
- 키보드로 접근 및 조작이 가능합니다.

## 주의사항

- SelectButtonGroup 내의 모든 SelectButton은 고유한 value 값을 가져야 합니다.
- value 배열과 onChange 함수는 필수적으로 제공해야 합니다.
- 단일 선택 모드에서는 value 배열의 길이가 항상 0 또는 1이 됩니다.