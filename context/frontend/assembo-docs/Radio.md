# Radio

`Radio` 컴포넌트는 여러 옵션 중 하나를 선택할 수 있는 라디오 버튼 UI 요소입니다. 기본 라디오 버튼과 탭 형태의 라디오 선택 컴포넌트를 제공합니다.

## 설치

```jsx
import {
  Radio,
  RadioGroup,
  RadioTab
} from "@wemeetdev/assembo";
```

## 컴포넌트 구성

Radio는 다음과 같은 컴포넌트로 구성됩니다:

1. **Radio**: 기본 라디오 버튼 컴포넌트
2. **RadioGroup**: 여러 라디오 버튼을 그룹화하여 상태를 관리하는 컨테이너
3. **RadioTab**: 탭 형태의 라디오 선택 컴포넌트

## 기본 사용법

### 기본 라디오 버튼 그룹

```jsx
import { useState } from "react";
import { Radio, RadioGroup } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState("option1");

  return (
    <RadioGroup value={selected} onChange={setSelected}>
      <Radio value="option1" text="옵션 1" />
      <Radio value="option2" text="옵션 2" />
      <Radio value="option3" text="옵션 3" />
    </RadioGroup>
  );
}
```

### 탭 형태의 라디오 그룹

```jsx
import { useState } from "react";
import { RadioTab, RadioGroup } from "@wemeetdev/assembo";

function Example() {
  const [selected, setSelected] = useState("tab1");

  return (
    <RadioGroup value={selected} onChange={setSelected}>
      <RadioTab value="tab1">탭 1</RadioTab>
      <RadioTab value="tab2">탭 2</RadioTab>
      <RadioTab value="tab3">탭 3</RadioTab>
    </RadioGroup>
  );
}
```

## 방향 설정

RadioGroup 컴포넌트는 세로 또는 가로 방향으로 자식 요소를 배치할 수 있습니다.

```jsx
// 세로 방향 (기본값)
<RadioGroup value={selected} onChange={setSelected} direction="vertical">
  <Radio value="option1" text="옵션 1" />
  <Radio value="option2" text="옵션 2" />
  <Radio value="option3" text="옵션 3" />
</RadioGroup>

// 가로 방향
<RadioGroup value={selected} onChange={setSelected} direction="horizontal">
  <Radio value="option1" text="옵션 1" />
  <Radio value="option2" text="옵션 2" />
  <Radio value="option3" text="옵션 3" />
</RadioGroup>
```

## 크기 설정

Radio와 RadioTab은 세 가지 크기를 지원합니다. 크기는 개별 컴포넌트 또는 RadioGroup을 통해 설정할 수 있습니다.

```jsx
// RadioGroup에서 크기 설정 (모든 자식에 적용)
<RadioGroup value={selected} onChange={setSelected} size="medium">
  <Radio value="option1" text="옵션 1" />
  <Radio value="option2" text="옵션 2" />
  <Radio value="option3" text="옵션 3" />
</RadioGroup>

// 개별 라디오 버튼 크기 설정
<RadioGroup value={selected} onChange={setSelected}>
  <Radio value="option1" text="옵션 1" size="small" />
  <Radio value="option2" text="옵션 2" size="medium" />
  <Radio value="option3" text="옵션 3" size="large" />
</RadioGroup>
```

RadioTab의 크기 설정:

```jsx
<RadioGroup value={selected} onChange={setSelected}>
  <RadioTab value="tab1" size="small">작은 탭</RadioTab>
  <RadioTab value="tab2" size="medium">중간 탭</RadioTab>
  <RadioTab value="tab3" size="large">큰 탭</RadioTab>
</RadioGroup>
```

## 비활성화 상태

Radio와 RadioTab 컴포넌트는 개별적으로 또는 그룹 전체를 비활성화할 수 있습니다.

```jsx
// RadioGroup 전체 비활성화
<RadioGroup value={selected} onChange={setSelected} disabled>
  <Radio value="option1" text="옵션 1" />
  <Radio value="option2" text="옵션 2" />
  <Radio value="option3" text="옵션 3" />
</RadioGroup>

// 개별 라디오 버튼 비활성화
<RadioGroup value={selected} onChange={setSelected}>
  <Radio value="option1" text="옵션 1" />
  <Radio value="option2" text="옵션 2" disabled />
  <Radio value="option3" text="옵션 3" />
</RadioGroup>
```

RadioTab 비활성화:

```jsx
<RadioGroup value={selected} onChange={setSelected}>
  <RadioTab value="tab1">활성화 탭</RadioTab>
  <RadioTab value="tab2" disabled>비활성화 탭</RadioTab>
  <RadioTab value="tab3">활성화 탭</RadioTab>
</RadioGroup>
```

## 커스텀 내용이 있는 RadioTab

RadioTab은 단순 텍스트뿐만 아니라 복잡한 내용도 지원합니다.

```jsx
import { IconHome, IconSetting, IconPerson } from "@wemeetdev/assembo";

<RadioGroup value={selected} onChange={setSelected}>
  <RadioTab value="home">
    <div className="flex items-center gap-2">
      <IconHome size={16} />
      <span>홈</span>
    </div>
  </RadioTab>
  <RadioTab value="settings">
    <div className="flex items-center gap-2">
      <IconSetting size={16} />
      <span>설정</span>
    </div>
  </RadioTab>
  <RadioTab value="profile">
    <div className="flex items-center gap-2">
      <IconPerson size={16} />
      <span>프로필</span>
    </div>
  </RadioTab>
</RadioGroup>
```

## Props

### Radio Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `string \| number` | 필수 | 라디오 버튼의 값 |
| `text` | `string` | - | 라디오 버튼 옆에 표시될 텍스트 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 라디오 버튼의 크기 |
| `disabled` | `boolean` | `false` | 비활성화 여부 |
| `className` | `string` | - | 추가 CSS 클래스 |

### RadioGroup Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `string \| number` | 필수 | 현재 선택된 값 |
| `onChange` | `(value: string \| number) => void` | 필수 | 선택 변경 시 호출되는 콜백 함수 |
| `direction` | `'vertical'` \| `'horizontal'` | `'vertical'` | 라디오 버튼 배치 방향 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 그룹 내 라디오 버튼의 기본 크기 |
| `disabled` | `boolean` | `false` | 그룹 전체 비활성화 여부 |
| `children` | `ReactNode` | 필수 | Radio 또는 RadioTab 컴포넌트들 |
| `className` | `string` | - | 추가 CSS 클래스 |

### RadioTab Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `string \| number` | 필수 | 라디오 탭의 값 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 라디오 탭의 크기 |
| `disabled` | `boolean` | `false` | 비활성화 여부 |
| `children` | `ReactNode` | 필수 | 탭 내용 |
| `className` | `string` | - | 추가 CSS 클래스 |

## 크기별 스타일

### Radio 크기

- **small**: 작은 원형 버튼, 간격이 좁음
- **medium**: 중간 크기 원형 버튼 (기본값)
- **large**: 큰 원형 버튼, 간격이 넓음

### RadioTab 크기

- **small**: 작은 텍스트와 패딩이 있는 탭
- **medium**: 중간 크기 텍스트와 패딩 (기본값)
- **large**: 큰 텍스트와 넓은 패딩이 있는 탭

## 접근성

- 모든 Radio 및 RadioTab 컴포넌트는 적절한 ARIA 속성을 포함합니다.
- 키보드 접근성을 지원하여 키보드로 선택 변경이 가능합니다.
- 비활성화 상태는 시각적으로 표시되고 상호작용이 불가능합니다.

## 주의사항

- RadioGroup 내의 모든 Radio 또는 RadioTab은 고유한 value 값을 가져야 합니다.
- 다른 타입의 컴포넌트(Radio와 RadioTab)를 같은 RadioGroup 내에 혼합하는 것은 권장되지 않습니다.
- RadioGroup의 value와 onChange props는 필수적으로 제공해야 합니다.