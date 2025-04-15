# Text

`Text` 컴포넌트는 다양한 스타일과 색상을 적용할 수 있는 텍스트 표시 요소입니다.

## 설치

```jsx
import { Text } from "@wemeetdev/assembo";
```

## 사용법

```jsx
// 기본 텍스트
<Text variant="bodyM14">기본 텍스트입니다.</Text>

// 다양한 크기와 스타일
<Text variant="headM24">제목 텍스트입니다.</Text>
<Text variant="subM18">부제목 텍스트입니다.</Text>
<Text variant="bodyR16">본문 텍스트입니다.</Text>
<Text variant="captionR12">캡션 텍스트입니다.</Text>

// 다양한 색상
<Text variant="bodyM14" color="primary700">Primary 색상 텍스트</Text>
<Text variant="bodyM14" color="gray500">Gray 색상 텍스트</Text>
<Text variant="bodyM14" color="red200">Red 색상 텍스트</Text>

// 다양한 HTML 요소로 렌더링
<Text variant="headM24" as="h1">H1 제목</Text>
<Text variant="bodyR16" as="p">P 태그 본문</Text>
<Text variant="bodyM14" as="label">Label 텍스트</Text>
```

## 변형 이름 규칙

텍스트 변형(variant) 이름은 다음 패턴을 따릅니다: `[타입][굵기][크기]`

- **타입**: `head` (제목), `sub` (부제목), `body` (본문), `caption` (캡션)
- **굵기**: `SB` (Semi-Bold), `M` (Medium), `R` (Regular)
- **크기**: 픽셀 단위 크기 (12, 14, 16, 18, 20, 24)

예시: `headM24`, `bodySB16`, `captionR12`

## Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `variant` | `TextVariant` | 필수 | 텍스트 스타일 변형 |
| `color` | `TextColor` | `'default'` | 텍스트 색상 |
| `as` | `TextTag` | `'span'` | 렌더링될 HTML 요소 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `children` | `ReactNode` | - | 텍스트 내용 |

## TextVariant 타입

```tsx
type TextVariant =
  // 제목 스타일
  | "headSB24" | "headM24" | "headR24"
  | "headSB20" | "headM20" | "headR20"
  // 부제목 스타일
  | "subSB18" | "subM18" | "subR18"
  // 본문 스타일
  | "bodySB16" | "bodyM16" | "bodyR16"
  | "bodySB14" | "bodyM14" | "bodyR14"
  // 캡션 스타일
  | "captionSB12" | "captionM12" | "captionR12";
```

## TextColor 타입

```tsx
type TextColor =
  // Primary 색상
  | "primary100" | "primary200" | "primary300" | "primary400" | "primary500"
  | "primary600" | "primary700" | "primary800" | "primary900"
  // Green 색상
  | "green100" | "green200" | "green300"
  // Red 색상
  | "red100" | "red200"
  // Purple 색상
  | "purple100" | "purple200"
  // Orange 색상
  | "orange100" | "orange200"
  // Gray 색상
  | "gray100" | "gray200" | "gray300" | "gray400" | "gray500"
  | "gray600" | "gray700" | "gray800" | "gray900"
  // Divider 색상
  | "divider1" | "divider2" | "divider3"
  // 기본 텍스트 색상
  | "default" | "main" | "sub" | "disabled" | "white";
```

## TextTag 타입

```tsx
type TextTag = "span" | "p" | "label" | "h1" | "h2" | "h3" | "h4" | "h5" | "h6" | "div";
```

## 추가 정보

- 모든 텍스트는 기본적으로 `span` 요소로 렌더링됩니다.
- `as` prop을 통해 의미에 맞는 HTML 태그로 렌더링할 수 있습니다.
- 폰트 크기, 굵기, 행간 등의 속성은 variant에 따라 미리 정의되어 있습니다.
- 색상은 테마의 색상 변수에 맵핑되어 있어 일관된 디자인을 유지합니다.