# Badge 컴포넌트

Badge 컴포넌트는 상태 표시, 알림, 카테고리 구분 등을 위한 시각적 요소를 제공합니다. 세 가지 유형의 뱃지를 지원합니다:

- **Dot**: 작은 원형 표시기, 상태나 알림을 나타냄
- **RoundBadge**: 둥근 형태의 뱃지, 주로 카운터나 짧은 텍스트에 사용
- **LabelBadge**: 레이블 형태의 뱃지, 상태나 카테고리 표시에 사용

## 설치

```jsx
import { Dot, RoundBadge, LabelBadge } from "@wemeetdev/assembo";
```

## Dot 컴포넌트

작은 원형 표시기로, 알림이나 상태를 시각적으로 나타냅니다.

```jsx
// 기본 사용법
<Dot /> // 기본 색상 (blue)

// 다른 색상 사용
<Dot color="red" />
<Dot color="gray" />
<Dot color="green" />
<Dot color="orange" />
<Dot color="purple" />

// 텍스트와 함께 사용
<Dot text="처리" />
<Dot color="red" text="실패" />
<Dot color="green" text="완료" />

// 아이콘과 함께 사용 (알림 표시)
import { Dot, IconFilledBell } from "@wemeetdev/assembo";

<div className="relative w-fit">
  <Dot className="absolute right-[-2px] top-[-2px]" />
  <IconFilledBell />
</div>
```

### Dot Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `color` | `'green'` \| `'red'` \| `'gray'` \| `'blue'` \| `'orange'` \| `'purple'` | `'blue'` | 도트의 색상 |
| `text` | `string` | - | 도트 옆에 표시할 텍스트 |
| `className` | `string` | - | 추가 CSS 클래스 |

## RoundBadge 컴포넌트

둥근 형태의 뱃지로, 주로 카운터나 짧은 텍스트 표시에 사용합니다.

```jsx
// 기본 사용법
<RoundBadge>99+</RoundBadge>

// 다양한 색상
<RoundBadge color="blue">99+</RoundBadge>
<RoundBadge color="gray">99+</RoundBadge>
<RoundBadge color="green">99+</RoundBadge>
<RoundBadge color="orange">99+</RoundBadge>
<RoundBadge color="purple">99+</RoundBadge>
<RoundBadge color="red">99+</RoundBadge>

// 다양한 스타일 변형
<RoundBadge variant="solid" color="blue">99+</RoundBadge> // 기본값
<RoundBadge variant="light" color="blue">99+</RoundBadge>
<RoundBadge variant="ghost" color="blue">99+</RoundBadge>

// 아이콘과 함께 사용
import { RoundBadge, IconSearch } from "@wemeetdev/assembo";

<RoundBadge className="h-6">
  <IconSearch size={14} />
</RoundBadge>
```

### RoundBadge Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `variant` | `'solid'` \| `'light'` \| `'ghost'` | `'solid'` | 뱃지의 스타일 변형 |
| `color` | `'red'` \| `'gray'` \| `'green'` \| `'blue'` \| `'orange'` \| `'purple'` | `'blue'` | 뱃지의 색상 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 뱃지 내부 콘텐츠 |

## LabelBadge 컴포넌트

레이블 형태의 뱃지로, 상태나 카테고리를 표시하는 데 사용합니다.

```jsx
// 기본 사용법
<LabelBadge>배송완료</LabelBadge>

// 다양한 색상
<LabelBadge color="blue">배송완료</LabelBadge>
<LabelBadge color="gray">배송완료</LabelBadge>
<LabelBadge color="green">배송완료</LabelBadge>
<LabelBadge color="orange">배송완료</LabelBadge>
<LabelBadge color="purple">배송완료</LabelBadge>
<LabelBadge color="red">배송완료</LabelBadge>

// 다양한 크기
<LabelBadge size="small">배송완료</LabelBadge>
<LabelBadge size="medium">배송완료</LabelBadge> // 기본값
<LabelBadge size="large">배송완료</LabelBadge>

// 다양한 스타일 변형
<LabelBadge variant="solid">배송완료</LabelBadge> // 기본값
<LabelBadge variant="light">배송완료</LabelBadge>
<LabelBadge variant="ghost">배송완료</LabelBadge>

// 아이콘과 함께 사용
import { LabelBadge, IconSearch } from "@wemeetdev/assembo";

<LabelBadge>
  <div className="flex items-center gap-1">
    <IconSearch size={14} /> 검색
  </div>
</LabelBadge>
```

### LabelBadge Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `variant` | `'solid'` \| `'light'` \| `'ghost'` | `'solid'` | 뱃지의 스타일 변형 |
| `color` | `'red'` \| `'gray'` \| `'green'` \| `'blue'` \| `'orange'` \| `'purple'` | `'blue'` | 뱃지의 색상 |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 뱃지의 크기 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 뱃지 내부 콘텐츠 |

## 스타일 변형 (Variants)

Badge 컴포넌트는 세 가지 스타일 변형을 지원합니다:

- **solid**: 단색 배경과 흰색 텍스트 (강조)
- **light**: 밝은 배경색과 메인 색상의 텍스트 (부드러운 강조)
- **ghost**: 회색 배경과 테두리, 메인 색상의 텍스트 (약한 강조)

## 색상 (Colors)

Badge 컴포넌트는 다음과 같은 색상을 지원합니다:

- **blue**: 기본 색상, 파란색 계열
- **gray**: 회색 계열
- **green**: 녹색 계열
- **red**: 빨간색 계열
- **orange**: 주황색 계열
- **purple**: 보라색 계열

## 사용 사례

### 상태 표시

```jsx
<LabelBadge color="green">완료</LabelBadge>
<LabelBadge color="blue">진행중</LabelBadge>
<LabelBadge color="orange">대기중</LabelBadge>
<LabelBadge color="red">실패</LabelBadge>
```

### 알림 표시

```jsx
// 아이콘에 알림 표시
<div className="relative w-fit">
  <Dot color="red" className="absolute right-[-2px] top-[-2px]" />
  <IconFilledBell />
</div>

// 카운터 표시
<div className="relative w-fit">
  <RoundBadge color="red" className="absolute right-[-8px] top-[-8px]">
    5
  </RoundBadge>
  <IconFilledBell size={24} />
</div>
```

### 카테고리 구분

```jsx
<div className="flex gap-2">
  <LabelBadge variant="light" color="blue">일반</LabelBadge>
  <LabelBadge variant="light" color="green">할인</LabelBadge>
  <LabelBadge variant="light" color="purple">신규</LabelBadge>
</div>
```

## 조합 예시

Badge 컴포넌트는 다음과 같이 다양한 요소와 조합하여 사용할 수 있습니다:

```jsx
// 메뉴 아이템과 함께 사용
<div className="flex justify-between items-center">
  <span>알림</span>
  <RoundBadge color="red">23</RoundBadge>
</div>

// 목록 항목과 함께 사용
<div className="flex items-center gap-2">
  <Dot color="green" />
  <span>작업 완료됨</span>
</div>
```