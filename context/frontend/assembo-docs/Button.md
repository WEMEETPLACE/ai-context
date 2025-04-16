# Button

`Button` 컴포넌트는 사용자의 액션을 유도하는 인터랙티브 요소입니다.

## 설치

```jsx
import { Button } from "@wemeetdev/assembo";
```

## 사용법

```jsx
// 기본 버튼
<Button>기본 버튼</Button>

// 크기 설정
<Button size="small">작은 버튼</Button>
<Button size="medium">중간 버튼</Button>
<Button size="large">큰 버튼</Button>

// 변형 스타일
<Button variant="primary">Primary</Button>
<Button variant="secondary">Secondary</Button>
<Button variant="tertiary">Tertiary</Button>
<Button variant="primary-line">Primary Line</Button>
<Button variant="ghost">Ghost</Button>

// 비활성화
<Button disabled>비활성화 버튼</Button>

// 아이콘과 함께 사용
import { Button, IconPlus } from "@wemeetdev/assembo";

<Button>
  <IconPlus size={20} />
  추가하기
</Button>

// 아이콘만 사용
<Button variant="secondary">
  <IconPlus />
</Button>

// 로딩 상태
import { Button, MoonSpinner } from "@wemeetdev/assembo";

<Button variant="secondary">
  <div className="flex gap-1 items-center">
    <MoonSpinner size={18} />
    <span>처리 중</span>
  </div>
</Button>
```

## Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `variant` | `'primary'` \| `'secondary'` \| `'tertiary'` \| `'primary-line'` \| `'ghost'` | `'primary'` | 버튼의 스타일 변형 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 버튼의 크기 |
| `disabled` | `boolean` | `false` | 버튼의 비활성화 여부 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 버튼 내용 |

## 추가 정보

- `small` 크기: 높이 32px (8 * 4), 텍스트 14px
- `medium` 크기: 높이 40px (10 * 4), 텍스트 16px
- `large` 크기: 높이 48px (12 * 4), 텍스트 18px
- 모든 버튼은 `disabled` 상태에서 시각적으로 비활성화됩니다.
- `children`을 통해 텍스트, 아이콘 또는 둘의 조합을 자유롭게 사용할 수 있습니다.