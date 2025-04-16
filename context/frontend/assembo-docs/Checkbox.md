# Checkbox

`Checkbox` 컴포넌트는 사용자가 여러 항목 중에서 선택할 수 있는 UI 요소입니다. 단독 체크박스, 체크박스 그룹, 전체 선택 체크박스, 중첩 체크박스 등 다양한 형태를 지원합니다.

## 설치

```jsx
import {
  Checkbox,
  CheckboxGroup,
  CheckboxAll,
  CheckboxNested,
} from "@wemeetdev/assembo";
```

## 기본 사용법

### 단독 체크박스

```jsx
import { useState } from "react";
import { Checkbox } from "@wemeetdev/assembo";

function Example() {
  const [checked, setChecked] = useState(false);
  const indeterminated = ...

  return (
    <Checkbox
      standalone
      indeterminate={indeterminated}
      checked={checked}
      onChange={(e) => setChecked(e.target.checked)}
      text="이용약관에 동의합니다"
    />
  );
}
```

### 체크박스 그룹

```jsx
import { useState } from "react";
import { Checkbox, CheckboxGroup } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState(["apple"]);

  return (
    <CheckboxGroup value={selected} onChange={setSelected}>
      <Checkbox value="apple" text="사과" />
      <Checkbox value="banana" text="바나나" />
      <Checkbox value="orange" text="오렌지" />
    </CheckboxGroup>
  );
}
```

### 전체 선택 체크박스

```jsx
import { useState } from "react";
import { Checkbox, CheckboxGroup, CheckboxAll } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState([]);
  const options = ["item1", "item2", "item3"];

  return (
    <CheckboxGroup value={selected} onChange={setSelected}>
      <CheckboxAll text="전체 선택" />
      {options.map((option) => (
        <Checkbox key={option} value={option} text={option} />
      ))}
    </CheckboxGroup>
  );
}
```

### 중첩 체크박스

```jsx
import { useState } from "react";
import { Checkbox, CheckboxGroup, CheckboxNested } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState(["parent"]);

  return (
    <CheckboxGroup value={selected} onChange={setSelected}>
      <Checkbox value="parent" text="부모 항목" />
      <CheckboxNested>
        <Checkbox value="child1" text="자식 항목 1" />
        <Checkbox value="child2" text="자식 항목 2" />
      </CheckboxNested>
    </CheckboxGroup>
  );
}
```

## 크기 설정

체크박스는 세 가지 크기를 지원합니다.

```jsx
<Checkbox size="small" text="작은 체크박스" />
<Checkbox size="medium" text="중간 체크박스" /> {/* 기본값 */}
<Checkbox size="large" text="큰 체크박스" />
```

## 방향 설정

체크박스 그룹의 방향을 설정할 수 있습니다.

```jsx
// 세로 방향 (기본값)
<CheckboxGroup direction="vertical">
  <Checkbox value="1" text="항목 1" />
  <Checkbox value="2" text="항목 2" />
</CheckboxGroup>

// 가로 방향
<CheckboxGroup direction="horizontal">
  <Checkbox value="1" text="항목 1" />
  <Checkbox value="2" text="항목 2" />
</CheckboxGroup>
```

## 비활성화 상태

```jsx
// 단일 체크박스 비활성화
<Checkbox disabled text="비활성화된 체크박스" />

// 그룹 전체 비활성화
<CheckboxGroup disabled>
  <Checkbox value="1" text="항목 1" />
  <Checkbox value="2" text="항목 2" />
</CheckboxGroup>

// 특정 항목만 비활성화
<CheckboxGroup>
  <Checkbox value="1" text="항목 1" />
  <Checkbox value="2" text="항목 2" disabled />
</CheckboxGroup>
```

## Props

### Checkbox Props

| 속성            | 타입                                               | 기본값     | 설명                                         |
| --------------- | -------------------------------------------------- | ---------- | -------------------------------------------- |
| `text`          | `string`                                           | -          | 체크박스 옆에 표시할 텍스트                  |
| `size`          | `'small'` \| `'medium'` \| `'large'`               | `'medium'` | 체크박스의 크기                              |
| `standalone`    | `boolean`                                          | `false`    | 단독 사용 여부 (true일 경우 value 불필요)    |
| `value`         | `string` \| `number`                               | -          | 체크박스의 값 (standalone이 false일 때 필수) |
| `indeterminate` | `boolean`                                          | `false`    | 부분 선택 상태 표시                          |
| `disabled`      | `boolean`                                          | `false`    | 비활성화 상태                                |
| `checked`       | `boolean`                                          | -          | 체크 상태 (standalone일 때 사용)             |
| `onChange`      | `(e: React.ChangeEvent<HTMLInputElement>) => void` | -          | 체크 상태 변경 시 호출되는 콜백              |
| `className`     | `string`                                           | -          | 추가 CSS 클래스                              |

### CheckboxGroup Props

| 속성        | 타입                                       | 기본값       | 설명                            |
| ----------- | ------------------------------------------ | ------------ | ------------------------------- |
| `value`     | `Array<string \| number>`                  | `[]`         | 선택된 체크박스 값들의 배열     |
| `onChange`  | `(value: Array<string \| number>) => void` | -            | 선택 항목 변경 시 호출되는 콜백 |
| `direction` | `'vertical'` \| `'horizontal'`             | `'vertical'` | 체크박스 배치 방향              |
| `disabled`  | `boolean`                                  | `false`      | 그룹 전체 비활성화              |
| `children`  | `ReactNode`                                | -            | 체크박스 항목들                 |
| `className` | `string`                                   | -            | 추가 CSS 클래스                 |

### CheckboxAll Props

| 속성        | 타입                                 | 기본값     | 설명                        |
| ----------- | ------------------------------------ | ---------- | --------------------------- |
| `text`      | `string`                             | -          | 체크박스 옆에 표시할 텍스트 |
| `size`      | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 체크박스의 크기             |
| `disabled`  | `boolean`                            | `false`    | 비활성화 상태               |
| `className` | `string`                             | -          | 추가 CSS 클래스             |

### CheckboxNested Props

| 속성        | 타입                           | 기본값       | 설명                   |
| ----------- | ------------------------------ | ------------ | ---------------------- |
| `direction` | `'vertical'` \| `'horizontal'` | `'vertical'` | 체크박스 배치 방향     |
| `disabled`  | `boolean`                      | `false`      | 중첩 그룹 비활성화     |
| `children`  | `ReactNode`                    | -            | 중첩된 체크박스 항목들 |
| `className` | `string`                       | -            | 추가 CSS 클래스        |

## 접근성

- 체크박스는 키보드를 통한 접근 및 조작이 가능합니다.
- 적절한 aria 속성을 사용하여 스크린 리더 호환성을 제공합니다.
- 항상 체크박스에 의미 있는 텍스트 레이블을 제공하는 것이 좋습니다.

## 주의사항

- `CheckboxGroup` 내부의 `Checkbox` 컴포넌트는 반드시 `value` 속성을 가져야 합니다.
- 단독 체크박스를 사용할 때는 `standalone` 속성을 `true`로 설정하세요.
- 중첩 체크박스를 사용할 때는 부모 항목 다음에 `CheckboxNested` 컴포넌트를 배치하세요.
- `CheckboxAll` 컴포넌트는 `CheckboxGroup` 내부의 첫 번째 자식으로 배치하는 것이 일반적입니다.
