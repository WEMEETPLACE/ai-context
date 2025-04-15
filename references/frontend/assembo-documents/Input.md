# Input

`Input` 컴포넌트는 사용자로부터 텍스트 입력을 받기 위한 요소입니다. 에러 상태 표시와 도움말 메시지 표시 기능을 제공합니다.

## 설치

```jsx
import { Input, InputMessage, InputMessageLayout } from "@wemeetdev/assembo";
```

## 기본 사용법

```jsx
// 기본 입력 필드
<Input placeholder="입력해주세요" />

// 크기 설정
<Input size="small" placeholder="작은 입력" />
<Input size="medium" placeholder="중간 입력" />
<Input size="large" placeholder="큰 입력" />

// 비활성화 상태
<Input disabled placeholder="비활성화된 입력" />

// 에러 상태
<Input error placeholder="에러 상태 입력" />
```

## 아이콘 사용하기

```jsx
import { Input, IconCircleCloseFill } from "@wemeetdev/assembo";

// 아이콘 표시
<Input
  placeholder="아이콘이 있는 입력"
  icon={<IconCircleCloseFill size={15} />}
/>

// 아이콘과 함께 클리어 기능 추가
import { Input, IconCircleCloseFill, useInput } from "@wemeetdev/assembo";

function InputWithClear() {
  const { value, onChange, clear } = useInput();

  return (
    <Input
      value={value}
      onChange={onChange}
      placeholder="입력 후 클리어 가능"
      icon={
        <IconCircleCloseFill
          size={15}
          onClick={clear}
          className="cursor-pointer"
        />
      }
    />
  );
}
```

## InputMessage와 InputMessageLayout 사용하기

`InputMessage`와 `InputMessageLayout`을 사용하여 입력 필드 아래에 에러 또는 성공 메시지를 표시할 수 있습니다.

```jsx
// 에러 메시지 표시
<InputMessageLayout>
  <Input 
    placeholder="이메일 입력" 
    error 
    value="invalid-email"
  />
  <InputMessage error>
    이메일 형식이 올바르지 않습니다.
  </InputMessage>
</InputMessageLayout>

// 성공 메시지 표시
<InputMessageLayout>
  <Input 
    placeholder="이메일 입력"
    value="example@domain.com"
  />
  <InputMessage>
    올바른 이메일 형식입니다.
  </InputMessage>
</InputMessageLayout>

// 커스텀 아이콘 사용
import { Input, InputMessage, InputMessageLayout, IconInfo } from "@wemeetdev/assembo";

<InputMessageLayout>
  <Input placeholder="사용자 이름 입력" />
  <InputMessage icon={<IconInfo size={15} />}>
    영문, 숫자를 포함하여 4~20자로 입력해주세요.
  </InputMessage>
</InputMessageLayout>
```

## useInput 훅

`useInput` 훅은 입력 필드 상태 관리를 위한 편리한 도구입니다.

```jsx
import { useInput } from "@wemeetdev/assembo";

function MyComponent() {
  const { value, onChange, clear } = useInput("초기값");
  
  return (
    <div>
      <Input
        value={value}
        onChange={onChange}
        placeholder="입력해주세요"
      />
      <button onClick={clear}>입력 지우기</button>
    </div>
  );
}
```

## Input Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 입력 필드 크기 |
| `error` | `boolean` | `false` | 에러 상태 표시 여부 |
| `icon` | `ReactNode` | - | 입력 필드 우측에 표시할 아이콘 |
| `showIcon` | `boolean` | `true` | 아이콘 표시 여부 |
| `wrapperClassName` | `string` | - | 아이콘이 있을 때 래퍼 요소에 적용할 CSS 클래스 |
| `wrapperStyle` | `CSSProperties` | - | 아이콘이 있을 때 래퍼 요소에 적용할 스타일 |
| `className` | `string` | - | 입력 필드에 적용할 추가 CSS 클래스 |

## InputMessage Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `error` | `boolean` | `false` | 에러 메시지 여부 (true: 빨간색, false: 녹색) |
| `icon` | `ReactNode` | `error ? <IconAlertFill /> : <IconCircleCheckFill />` | 메시지 왼쪽에 표시할 아이콘 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 메시지 내용 |

## InputMessageLayout Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `children` | `ReactNode` | - | 레이아웃 내부에 렌더링할 컴포넌트들 |

## 추가 정보

- `small` 크기: 높이 32px (8 * 4), 텍스트 14px
- `medium` 크기: 높이 40px (10 * 4), 텍스트 14px
- `large` 크기: 높이 48px (12 * 4), 텍스트 16px
- `error` 상태에서는 빨간색 테두리와 배경으로 표시됩니다.
- `InputMessageLayout`은 입력 필드와 메시지 사이에 적절한 간격을 제공합니다.
- `InputMessage`는 `error` prop에 따라 빨간색(에러) 또는 녹색(성공) 메시지를 표시합니다.
- 메시지가 비어있거나 공백만 있는 경우 `InputMessage`는 렌더링되지 않습니다.