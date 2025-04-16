# Progress

`Progress` 컴포넌트는 진행 상태를 시각적으로 표시하는 UI 요소입니다. 기본 진행 바, 제목과 퍼센트를 표시하는 헤더, 아이콘과 함께 사용할 수 있는 레이아웃 등 다양한 구성을 지원합니다.

## 설치

```jsx
import {
  Progress,
  ProgressHeader,
  ProgressIconLayout
} from "@wemeetdev/assembo";
```

## 컴포넌트 구성

Progress는 다음과 같은 컴포넌트로 구성됩니다:

1. **Progress**: 기본 진행 바 컴포넌트
2. **ProgressHeader**: 제목과 퍼센트를 표시하는 헤더 컴포넌트
3. **ProgressIconLayout**: 아이콘과 진행 바를 함께 배치하는 레이아웃 컴포넌트

## 기본 사용법

### 기본 진행 바

```jsx
// 기본 진행 바 (50% 진행)
<Progress value={0.5} />

// 75% 진행
<Progress value={0.75} />

// 완료된 진행 바
<Progress value={1} />
```

### 제목과 퍼센트가 있는 진행 바

```jsx
<ProgressHeader title="다운로드 중" percent={50}>
  <Progress value={0.5} />
</ProgressHeader>
```

### 아이콘과 함께 사용

```jsx
import { IconDownload } from "@wemeetdev/assembo";

<ProgressIconLayout>
  <IconDownload size={24} />
  <ProgressHeader title="다운로드 중" percent={75}>
    <Progress value={0.75} />
  </ProgressHeader>
</ProgressIconLayout>
```

## 진행 값 설정

Progress 컴포넌트의 value는 0부터 1 사이의 값으로 설정합니다.

```jsx
// 0% 진행
<Progress value={0} />

// 25% 진행
<Progress value={0.25} />

// 50% 진행
<Progress value={0.5} />

// 75% 진행
<Progress value={0.75} />

// 100% 진행
<Progress value={1} />
```

## 동적 진행 상태 표시

상태에 따라 진행 바를 동적으로 업데이트할 수 있습니다.

```jsx
import { useState, useEffect } from "react";

function ProgressExample() {
  const [progress, setProgress] = useState(0);
  
  useEffect(() => {
    const timer = setInterval(() => {
      setProgress(prevProgress => {
        if (prevProgress >= 1) {
          clearInterval(timer);
          return 1;
        }
        return prevProgress + 0.1;
      });
    }, 1000);
    
    return () => {
      clearInterval(timer);
    };
  }, []);
  
  return (
    <ProgressHeader
      title="업로드 중"
      percent={Math.round(progress * 100)}
    >
      <Progress value={progress} />
    </ProgressHeader>
  );
}
```

## 커스텀 스타일링

```jsx
// 커스텀 너비 설정
<Progress
  value={0.5}
  className="w-64"
/>

// 커스텀 스타일링
<ProgressHeader
  title="커스텀 진행 바"
  percent={80}
  className="bg-gray-100 p-4 rounded"
>
  <Progress
    value={0.8}
    className="h-2"
  />
</ProgressHeader>
```

## Props

### Progress Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `number` | 필수 | 진행 상태 (0부터 1 사이의 값) |
| `className` | `string` | - | 추가 CSS 클래스 |

Progress 컴포넌트는 HTML의 `<progress>` 요소의 모든 속성을 지원합니다.

### ProgressHeader Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `title` | `string` | - | 진행 바 위에 표시될 제목 |
| `percent` | `number` | - | 진행 바 위에 표시될 퍼센트 값 |
| `children` | `ReactNode` | 필수 | 진행 바 컴포넌트 |
| `className` | `string` | - | 추가 CSS 클래스 |

### ProgressIconLayout Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `children` | `ReactNode` | 필수 | 아이콘과 진행 바 컴포넌트 |
| `className` | `string` | - | 추가 CSS 클래스 |

## 스타일 가이드

- Progress 컴포넌트는 기본적으로 1px 높이의 얇은 진행 바로 표시됩니다.
- 배경색은 회색(gray-400)이고, 진행 부분은 기본 주 색상(primary-500)입니다.
- ProgressHeader의 제목과 퍼센트는 "captionSB12" 텍스트 스타일로 표시됩니다.
- ProgressIconLayout은 아이콘과 진행 바 사이에 3단위의 간격을 제공합니다.

## 주의사항

- Progress 컴포넌트의 value는 0부터 1 사이의 값이어야 합니다.
- ProgressHeader의 percent 값은 0부터 100 사이의 정수 값을 사용합니다.
- ProgressIconLayout을 사용할 때는 첫 번째 자식으로 아이콘, 두 번째 자식으로 ProgressHeader를 배치해야 합니다.