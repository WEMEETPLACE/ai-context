# MultiDrop

`MultiDrop` 컴포넌트는 체크박스 형태의 다중 선택이 가능한 드롭다운 메뉴를 제공합니다. 여러 하위 컴포넌트로 구성되어 있으며, 전체 선택 기능도 지원합니다.

## 설치

```jsx
import {
  MultiDrop,
  MultiDropTrigger,
  MultiDropInput,
  MultiDropContent,
  MultiDropCheckbox,
  MultiDropCheckboxAll
} from "@wemeetdev/assembo";
```

## 컴포넌트 구성

MultiDrop은 다음과 같은 하위 컴포넌트로 구성됩니다:

1. **MultiDrop**: 상태 관리 및 컨텍스트를 제공하는 루트 컴포넌트
2. **MultiDropTrigger**: 드롭다운 메뉴를 열고 닫는 트리거 컴포넌트
3. **MultiDropInput**: 기본 제공되는 입력 형태의 트리거 컴포넌트
4. **MultiDropContent**: 드롭다운 메뉴 항목들을 포함하는 컨테이너
5. **MultiDropCheckbox**: 개별 체크박스 항목을 나타내는 컴포넌트
6. **MultiDropCheckboxAll**: 모든 항목을 선택/해제할 수 있는 전체 선택 체크박스

## 기본 사용법

```jsx
import { useState } from "react";
import {
  MultiDrop,
  MultiDropTrigger,
  MultiDropInput,
  MultiDropContent,
  MultiDropCheckbox,
  MultiDropCheckboxAll
} from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState([]);
  
  const options = [
    { label: "옵션 1", value: "option1" },
    { label: "옵션 2", value: "option2" },
    { label: "옵션 3", value: "option3" }
  ];

  return (
    <MultiDrop value={selected} onChange={setSelected}>
      <MultiDropTrigger>
        <MultiDropInput placeHolder="선택하세요" />
      </MultiDropTrigger>
      <MultiDropContent>
        <MultiDropCheckboxAll />
        {options.map((option) => (
          <MultiDropCheckbox 
            key={option.value} 
            value={option}
          />
        ))}
      </MultiDropContent>
    </MultiDrop>
  );
}
```

## 선택 항목 표시 방식

MultiDropInput은 선택된 항목에 따라 자동으로 표시 텍스트를 변경합니다:

- 아무것도 선택되지 않은 경우: 플레이스홀더 표시
- 모든 항목이 선택된 경우: "전체"
- 일부 항목만 선택된 경우: "첫 번째 선택 항목 외 N건"

```jsx
// 선택된 항목 없음: "선택하세요" 표시
<MultiDropInput placeHolder="선택하세요" />

// 모든 항목 선택: "전체" 표시
// 일부 선택: "옵션 1 외 2건" 표시
```

## 크기 설정

MultiDrop 컴포넌트는 세 가지 크기를 지원합니다.

```jsx
// 작은 크기
<MultiDropInput size="small" placeHolder="작은 드롭다운" />

// 중간 크기 (기본값)
<MultiDropInput size="medium" placeHolder="중간 드롭다운" />

// 큰 크기
<MultiDropInput size="large" placeHolder="큰 드롭다운" />
```

## 커스텀 트리거

기본 제공되는 `MultiDropInput` 대신 커스텀 트리거를 사용할 수 있습니다.

```jsx
<MultiDrop value={selected} onChange={setSelected}>
  <MultiDropTrigger>
    <button className="custom-button">
      {selected.length > 0 ? `${selected.length}개 선택됨` : "선택하세요"}
    </button>
  </MultiDropTrigger>
  <MultiDropContent>
    {/* ... 체크박스 항목들 ... */}
  </MultiDropContent>
</MultiDrop>
```

## 상태에 따른 트리거 변경

드롭다운의 열림/닫힘 상태에 따라 트리거의 모양을 바꿀 수 있습니다.

```jsx
<MultiDrop value={selected} onChange={setSelected}>
  <MultiDropTrigger>
    {({ isOpen }) => (
      <button className={`custom-button ${isOpen ? "active" : ""}`}>
        {selected.length > 0 ? `${selected.length}개 선택됨` : "선택하세요"}
        {isOpen ? " ▲" : " ▼"}
      </button>
    )}
  </MultiDropTrigger>
  <MultiDropContent>
    {/* ... 체크박스 항목들 ... */}
  </MultiDropContent>
</MultiDrop>
```

## 비활성화 상태

MultiDrop 전체 또는 특정 체크박스를 비활성화할 수 있습니다.

```jsx
// MultiDrop 전체 비활성화
<MultiDrop value={selected} onChange={setSelected} disabled>
  {/* ... */}
</MultiDrop>

// 특정 체크박스만 비활성화
<MultiDropCheckbox 
  value={{ label: "비활성화 항목", value: "disabled-option" }}
  disabled
/>

// 전체 선택 체크박스 비활성화
<MultiDropCheckboxAll disabled />
```

## 전체 선택 텍스트 변경

전체 선택 체크박스의 텍스트를 변경할 수 있습니다.

```jsx
<MultiDropCheckboxAll label="모두 선택" />
```

## Props

### MultiDrop Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `Array<string>` | 필수 | 선택된 항목들의 값 배열 |
| `onChange` | `(value: Array<string>) => void` | 필수 | 선택 항목 변경 시 호출되는 콜백 함수 |
| `disabled` | `boolean` | `false` | 드롭다운 비활성화 여부 |
| `children` | `ReactNode` | 필수 | 드롭다운 구성 요소 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `style` | `React.CSSProperties` | - | 인라인 스타일 객체 |

### MultiDropTrigger Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `children` | `ReactNode \| (props: { isOpen: boolean }) => ReactNode` | 필수 | 트리거로 사용할 요소 또는 함수형 자식 |
| `className` | `string` | - | 추가 CSS 클래스 |

### MultiDropInput Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 입력 필드의 크기 |
| `icon` | `ReactNode` | - | 커스텀 아이콘 |
| `placeHolder` | `string` | - | 값이 없을 때 표시할 텍스트 |
| `className` | `string` | - | 추가 CSS 클래스 |

### MultiDropContent Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | 필수 | 드롭다운 항목들 |

### MultiDropCheckbox Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `{ label: string, value: string }` | 필수 | 체크박스 항목 데이터 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 체크박스의 크기 |
| `disabled` | `boolean` | `false` | 체크박스 비활성화 여부 |
| `className` | `string` | - | 추가 CSS 클래스 |

### MultiDropCheckboxAll Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 체크박스의 크기 |
| `disabled` | `boolean` | `false` | 체크박스 비활성화 여부 |
| `label` | `string` | `'전체'` | 체크박스 라벨 |
| `className` | `string` | - | 추가 CSS 클래스 |

## 동작 원리

MultiDrop 컴포넌트는 다음과 같은 방식으로 작동합니다:

1. **상태 관리**: Context API를 사용하여 선택된 항목과 열림/닫힘 상태를 관리
2. **체크박스 등록**: 각 `MultiDropCheckbox`는 렌더링 시 자동으로 등록됨
3. **전체 선택**: `MultiDropCheckboxAll`은 등록된 모든 체크박스 항목을 선택/해제
4. **포지셔닝**: 드롭다운 메뉴는 트리거 요소를 기준으로 자동으로 위치가 결정됨
5. **선택 표시**: 선택된 항목에 따라 표시 텍스트가 동적으로 변경됨

## 접근성

- 키보드 네비게이션을 통해 드롭다운을 조작할 수 있습니다.
- ESC 키를 통해 드롭다운을 닫을 수 있습니다.
- 외부 클릭 시 드롭다운이 자동으로 닫힙니다.
- 적절한 ARIA 속성을 통해 스크린 리더 접근성을 지원합니다.

## 주의사항

- MultiDrop은 컴포넌트 구조(MultiDrop > MultiDropTrigger > MultiDropContent)를 따라야 합니다.
- MultiDropContent는 포털을 사용하여 렌더링되므로, z-index 관련 스타일 이슈가 발생할 수 있습니다.
- value 배열과 MultiDropCheckbox의 value 속성은 일치해야 선택 상태가 올바르게 표시됩니다.