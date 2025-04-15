# FilterChip

`FilterChip` 컴포넌트는 여러 옵션 중에서 필터링 조건을 선택하는 데 사용되는 UI 요소입니다. 다중 선택이 가능하며, 선택 상태에 따라 시각적으로 구분됩니다.

## 설치

```jsx
import { FilterChip, FilterChipGroup } from "@wemeetdev/assembo";
```

## 컴포넌트 구성

FilterChip은 다음과 같은 컴포넌트로 구성됩니다:

1. **FilterChipGroup**: 여러 FilterChip을 그룹화하고 선택 상태를 관리
2. **FilterChip**: 개별 필터 옵션을 나타내는 칩 컴포넌트

## 기본 사용법

```jsx
import { useState } from "react";
import { FilterChip, FilterChipGroup } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState([]);

  return (
    <FilterChipGroup value={selected} onChange={setSelected}>
      <FilterChip value="option1">옵션 1</FilterChip>
      <FilterChip value="option2">옵션 2</FilterChip>
      <FilterChip value="option3">옵션 3</FilterChip>
      <FilterChip value="option4">옵션 4</FilterChip>
    </FilterChipGroup>
  );
}
```

## 크기 설정

FilterChip은 두 가지 크기를 지원합니다.

```jsx
// 작은 크기 (기본값)
<FilterChip size="small" value="option1">
  작은 칩
</FilterChip>

// 중간 크기
<FilterChip size="medium" value="option2">
  중간 칩
</FilterChip>
```

## 초기 선택값 설정

특정 칩을 초기에 선택된 상태로 설정할 수 있습니다.

```jsx
function Example() {
  // option1과 option3이 초기에 선택됨
  const [selected, setSelected] = useState(["option1", "option3"]);

  return (
    <FilterChipGroup value={selected} onChange={setSelected}>
      <FilterChip value="option1">옵션 1</FilterChip>
      <FilterChip value="option2">옵션 2</FilterChip>
      <FilterChip value="option3">옵션 3</FilterChip>
      <FilterChip value="option4">옵션 4</FilterChip>
    </FilterChipGroup>
  );
}
```

## 아이콘과 함께 사용

```jsx
import { FilterChip, FilterChipGroup, IconHome } from "@wemeetdev/assembo";

<FilterChipGroup value={selected} onChange={setSelected}>
  <FilterChip value="home">
    <div className="flex items-center gap-1">
      <IconHome size={14} />
      <span>홈</span>
    </div>
  </FilterChip>
  {/* 다른 필터 칩들... */}
</FilterChipGroup>
```

## 커스텀 스타일링

```jsx
<FilterChip 
  value="custom" 
  className="border-green-500 hover:bg-green-100"
>
  커스텀 스타일
</FilterChip>
```

## 필터 변경 처리

필터 선택 변경 시 특정 동작을 수행할 수 있습니다.

```jsx
function Example() {
  const [selected, setSelected] = useState([]);

  const handleFilterChange = (newSelected) => {
    setSelected(newSelected);
    
    // 필터 변경 시 추가 작업 수행
    console.log("선택된 필터:", newSelected);
    
    // 예: API 호출로 데이터 필터링
    fetchFilteredData(newSelected);
  };

  return (
    <FilterChipGroup value={selected} onChange={handleFilterChange}>
      <FilterChip value="category1">카테고리 1</FilterChip>
      <FilterChip value="category2">카테고리 2</FilterChip>
      <FilterChip value="category3">카테고리 3</FilterChip>
    </FilterChipGroup>
  );
}
```

## Props

### FilterChipGroup Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `Array<string \| number>` | 필수 | 선택된 칩의 value 값 배열 |
| `onChange` | `(value: Array<string \| number>) => void` | 필수 | 선택 변경 시 호출되는 콜백 함수 |
| `children` | `ReactNode` | 필수 | FilterChip 컴포넌트들 |
| `className` | `string` | - | 추가 CSS 클래스 |

### FilterChip Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `string \| number` | 필수 | 칩의 고유 식별자 |
| `size` | `'small'` \| `'medium'` | `'small'` | 칩의 크기 |
| `children` | `ReactNode` | - | 칩 내부 콘텐츠 |
| `className` | `string` | - | 추가 CSS 클래스 |

또한 HTML 버튼 요소의 모든 속성(onClick, disabled 등)을 지원합니다.

## 스타일 가이드

- **미선택 상태**: 회색 배경, 기본 텍스트 색상
- **선택 상태**: 연한 파란색 배경, 파란색 테두리, 파란색 텍스트
- **크기**:
  - small: 14px 텍스트, 28px 높이
  - medium: 18px 텍스트, 36px 높이
- FilterChipGroup은 내부 FilterChip들 사이에 간격(gap)을 제공합니다.

## 접근성

- 키보드 포커스가 가능하며 Enter 또는 Space 키로 선택 가능합니다.
- 모든 FilterChip은 버튼 요소로 구현되어 적절한 키보드 접근성을 제공합니다.
- 시각적으로 선택 상태가 명확하게 구분됩니다.

## 주의사항

- FilterChip은 반드시 FilterChipGroup 내에서 사용해야 합니다.
- 각 FilterChip의 value는 그룹 내에서 고유해야 합니다.
- FilterChipGroup의 value 배열에 있는 값과 일치하는 value를 가진 FilterChip은 선택된 상태로 표시됩니다.