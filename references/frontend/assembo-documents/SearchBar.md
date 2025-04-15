# SearchBar

`SearchBar` 컴포넌트는 사용자가 텍스트를 입력하고 검색 기능을 사용할 수 있는 UI 요소입니다. 단독으로 사용하거나 다른 컴포넌트와 조합하여 고급 검색 인터페이스를 구성할 수 있습니다.

## 설치

```jsx
import {
  SearchBar,
  SearchBarComposer
} from "@wemeetdev/assembo";
```

## 컴포넌트 구성

SearchBar는 다음과 같은 컴포넌트로 구성됩니다:

1. **SearchBar**: 기본 검색 입력 필드 컴포넌트
2. **SearchBarComposer**: SearchBar와 다른 UI 요소를 조합하는 컨테이너 컴포넌트

## 기본 사용법

```jsx
import { useState } from "react";
import { SearchBar, useInput } from "@wemeetdev/assembo";

function Example() {
  const { value, onChange, clear } = useInput("");
  
  const handleSearch = (searchValue) => {
    console.log("검색어:", searchValue);
    // 검색 로직 실행
  };

  return (
    <SearchBar
      value={value}
      onChange={onChange}
      placeholder="검색어를 입력하세요"
      onSearch={handleSearch}
      onClear={clear}
    />
  );
}
```

## 크기 설정

SearchBar는 세 가지 크기를 지원합니다.

```jsx
// 작은 크기
<SearchBar
  size="small"
  value={value}
  onChange={onChange}
  onSearch={handleSearch}
  onClear={clear}
  placeholder="작은 검색창"
/>

// 중간 크기 (기본값)
<SearchBar
  size="medium"
  value={value}
  onChange={onChange}
  onSearch={handleSearch}
  onClear={clear}
  placeholder="중간 검색창"
/>

// 큰 크기
<SearchBar
  size="large"
  value={value}
  onChange={onChange}
  onSearch={handleSearch}
  onClear={clear}
  placeholder="큰 검색창"
/>
```

## 검색 아이콘 숨기기

검색 아이콘을 숨기고 입력 필드만 표시할 수 있습니다.

```jsx
<SearchBar
  value={value}
  onChange={onChange}
  onSearch={handleSearch}
  onClear={clear}
  hideSearch
  placeholder="검색 아이콘 없음"
/>
```

## 에러 상태 표시

```jsx
<SearchBar
  value={value}
  onChange={onChange}
  onSearch={handleSearch}
  onClear={clear}
  error
  placeholder="에러 상태"
/>
```

## 다른 컴포넌트와 조합하기

SearchBarComposer를 사용하여 SearchBar와 다른 UI 요소(예: 드롭다운)를 조합할 수 있습니다.

```jsx
import { 
  SearchBar, 
  SearchBarComposer, 
  Dropdown,
  DropdownTrigger,
  DropdownInput,
  DropdownContent,
  DropdownItem
} from "@wemeetdev/assembo";

function Example() {
  const { value, onChange, clear } = useInput("");
  const [selected, setSelected] = useState(null);
  
  const options = [
    { label: "전체", value: "all" },
    { label: "제목", value: "title" },
    { label: "내용", value: "content" }
  ];

  const handleSearch = (searchValue) => {
    console.log("검색어:", searchValue);
    console.log("검색 범위:", selected?.value);
    // 검색 로직 실행
  };

  return (
    <SearchBarComposer
      acc={
        <Dropdown value={selected} onChange={setSelected}>
          <DropdownTrigger>
            <DropdownInput placeHolder="전체" />
          </DropdownTrigger>
          <DropdownContent>
            {options.map((option) => (
              <DropdownItem key={option.value} value={option} />
            ))}
          </DropdownContent>
        </Dropdown>
      }
    >
      <SearchBar
        value={value}
        onChange={onChange}
        placeholder="검색어를 입력하세요"
        onSearch={handleSearch}
        onClear={clear}
      />
    </SearchBarComposer>
  );
}
```

## 이벤트 처리

SearchBar는 다양한 이벤트를 처리할 수 있습니다.

```jsx
// 검색 버튼 클릭 또는 엔터키 입력 시 검색 실행
<SearchBar
  value={value}
  onChange={onChange}
  onSearch={(searchValue) => {
    console.log("검색어:", searchValue);
    // 검색 실행
  }}
  onClear={clear}
  placeholder="검색어를 입력하세요"
/>

// 사용자 지정 핸들러 추가
<SearchBar
  value={value}
  onChange={(e) => {
    onChange(e);
    console.log("입력 중:", e.target.value);
  }}
  onSearch={handleSearch}
  onClear={() => {
    clear();
    console.log("검색어 지움");
  }}
  onFocus={() => console.log("포커스됨")}
  onBlur={() => console.log("포커스 해제됨")}
  placeholder="검색어를 입력하세요"
/>
```

## Props

### SearchBar Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `string` | 필수 | 입력 필드의 값 |
| `onChange` | `(e: React.ChangeEvent<HTMLInputElement>) => void` | 필수 | 입력값 변경 시 호출될 함수 |
| `onSearch` | `(value: string) => void` | - | 검색 실행 시 호출될 함수 |
| `onClear` | `() => void` | 필수 | 입력값 지우기 버튼 클릭 시 호출될 함수 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 검색창의 크기 |
| `acc` | `ReactNode` | - | 왼쪽에 배치할 추가 콘텐츠 |
| `error` | `boolean` | `false` | 에러 상태 표시 여부 |
| `hideSearch` | `boolean` | `false` | 검색 아이콘 숨김 여부 |
| `wrapperClassName` | `string` | - | 외부 래퍼에 적용할 CSS 클래스 |
| `wrapperStyle` | `CSSProperties` | - | 외부 래퍼에 적용할 인라인 스타일 |
| `placeholder` | `string` | - | 입력 필드의 플레이스홀더 |

HTML의 `<input>` 요소의 모든 속성을 지원합니다.

### SearchBarComposer Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `children` | `ReactElement` | 필수 | SearchBar 컴포넌트 |
| `acc` | `ReactNode` | 필수 | 검색창 왼쪽에 배치할 콘텐츠 |

## 스타일 가이드

- SearchBar는 기본적으로 둥근 모서리와 검색 아이콘을 가진 입력 필드로 표시됩니다.
- 입력 필드에 값이 있는 경우, 오른쪽에 지우기 버튼(X)이 표시됩니다.
- 에러 상태에서는 빨간색 테두리와 배경으로 표시됩니다.
- SearchBarComposer를 사용하면 왼쪽에 다른 UI 요소를 배치하여 검색 조건을 설정할 수 있습니다.

## 접근성

- 검색 입력 필드에는 적절한 레이블이나 플레이스홀더를 제공해야 합니다.
- 검색 기능은 엔터키를 통해서도 실행할 수 있어 키보드 사용자를 지원합니다.
- 지우기 버튼은 스크린 리더 사용자에게 적절한 역할을 전달합니다.

## 주의사항

- SearchBar는 반드시 `value`, `onChange`, `onClear` props를 제공해야 합니다.
- SearchBarComposer를 사용할 때는 자식으로 SearchBar 컴포넌트를 제공해야 합니다.
- 입력 필드의 높이는 size 속성에 따라 자동으로 조정됩니다.