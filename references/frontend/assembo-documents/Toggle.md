# Toggle

`Toggle` 컴포넌트는 On/Off 스위치 기능을 제공하는 UI 요소입니다. 다양한 크기와 상태를 지원합니다.

## 설치

```jsx
import { Toggle } from "@wemeetdev/assembo";
```

## 기본 사용법

```jsx
// 기본 사용법
const [isOn, setIsOn] = useState(false);

<Toggle
  value={isOn}
  onChange={(e, newValue) => setIsOn(newValue)}
/>

// 초기값을 true로 설정
const [isActive, setIsActive] = useState(true);

<Toggle
  value={isActive}
  onChange={(e, newValue) => setIsActive(newValue)}
/>

// 비활성화 토글
<Toggle
  value={true}
  onChange={() => {}}
  disabled
/>
```

## 크기 설정

Toggle 컴포넌트는 세 가지 크기를 지원합니다.

```jsx
// 소형
<Toggle
  size="small"
  value={isOn}
  onChange={(e, newValue) => setIsOn(newValue)}
/>

// 중형 (기본값)
<Toggle
  size="medium"
  value={isOn}
  onChange={(e, newValue) => setIsOn(newValue)}
/>

// 대형
<Toggle
  size="large"
  value={isOn}
  onChange={(e, newValue) => setIsOn(newValue)}
/>
```

## Props

| 속성        | 타입                                                   | 기본값     | 설명                                       |
| ----------- | ------------------------------------------------------ | ---------- | ------------------------------------------ |
| `value`     | `boolean`                                              | 필수       | 토글의 현재 상태 (true: 켜짐, false: 꺼짐) |
| `onChange`  | `(event: React.MouseEvent, newValue: boolean) => void` | 필수       | 토글 상태가 변경될 때 호출되는 콜백 함수   |
| `size`      | `'small'` \| `'medium'` \| `'large'`                   | `'medium'` | 토글의 크기                                |
| `disabled`  | `boolean`                                              | `false`    | 비활성화 상태 여부                         |
| `className` | `string`                                               | -          | 추가 CSS 클래스                            |

## 크기별 스타일

- **small**: 높이 16px, 너비 28px
- **medium**: 높이 20px, 너비 36px (기본값)
- **large**: 높이 24px, 너비 44px

## 접근성

- 내부적으로 `role="switch"` 속성을 사용하여 스크린 리더에 토글 스위치임을 알립니다.
- `aria-checked` 속성을 통해 현재 토글 상태를 전달합니다.
- 항상 Toggle과 함께 라벨을 연결하는 것을 권장합니다.

```jsx
<div className="flex items-center gap-2">
  <label htmlFor="notifications" className="mr-2">
    알림 수신
  </label>
  <Toggle
    id="notifications"
    value={isOn}
    onChange={(e, newValue) => setIsOn(newValue)}
  />
</div>
```
