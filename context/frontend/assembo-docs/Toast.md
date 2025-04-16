# Toast

`Toast` 컴포넌트는 사용자에게 간단한 알림 메시지를 표시하는 기능을 제공합니다.

## 설치

```jsx
import { toast, Toaster } from "@wemeetdev/assembo";
```

## 기본 설정

Toast를 사용하기 위해서는 애플리케이션의 루트 컴포넌트에 `Toaster` 컴포넌트를 추가해야 합니다.

```jsx
// App.tsx 또는 layout.tsx
import { Toaster } from "@wemeetdev/assembo";

function App() {
  return (
    <>
      {/* 애플리케이션 컨텐츠 */}
      <Toaster />
    </>
  );
}
```

## 기본 사용법

`toast` 함수를 호출하여 알림 메시지를 표시할 수 있습니다.

```jsx
// 기본 토스트 메시지
toast("안녕하세요!");

// 지속 시간 설정 (밀리초 단위)
toast("3초 동안 표시됩니다.", { duration: 3000 });

// 줄바꿈 포함
toast("안녕하세요!\n반갑습니다.");
```

## 토스트 위치 설정

`Toaster` 컴포넌트의 `position` prop을 통해 토스트 메시지가 표시될 위치를 설정할 수 있습니다.

```jsx
// 하단 중앙 (기본값)
<Toaster position="bottom-center" />

// 상단 우측
<Toaster position="top-right" />

// 상단 중앙
<Toaster position="top-center" />

// 상단 좌측
<Toaster position="top-left" />

// 하단 우측
<Toaster position="bottom-right" />

// 하단 좌측
<Toaster position="bottom-left" />
```

## 오프셋 설정

토스트 메시지의 화면 가장자리로부터의 거리를 설정할 수 있습니다.

```jsx
// 기본 오프셋 (24px)
<Toaster offset="24px" />

// 커스텀 오프셋
<Toaster offset="32px" />
```

## 사용 사례

```jsx
// 폼 제출 성공 알림
function handleSubmit(e) {
  e.preventDefault();
  // 폼 데이터 처리...
  toast("정보가 성공적으로 저장되었습니다.");
}

// 실패 알림
function handleDelete() {
  try {
    // 데이터 삭제 처리...
    toast("항목이 삭제되었습니다.");
  } catch (error) {
    toast("삭제 중 오류가 발생했습니다.\n다시 시도해주세요.");
  }
}

// 복사 확인
function handleCopy() {
  navigator.clipboard.writeText("복사할 텍스트");
  toast("클립보드에 복사되었습니다.");
}
```

## Props

### Toaster Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `position` | `'top-left'` \| `'top-right'` \| `'top-center'` \| `'bottom-left'` \| `'bottom-right'` \| `'bottom-center'` | `'bottom-center'` | 토스트 메시지가 표시될 위치 |
| `offset` | `string` | `'24px'` | 화면 가장자리로부터의 거리 |

### toast 함수 옵션

| 옵션 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `message` | `string` | 필수 | 표시할 토스트 메시지 (줄바꿈 가능: `\n`) |
| `duration` | `number` | `3000` | 토스트 메시지 표시 시간 (밀리초) |

## 특징

- 마우스가 토스트 위에 있을 때 자동으로 타이머가 일시 정지됩니다.
- 마우스가 떠나면 타이머가 재시작됩니다.
- 새로운 토스트 메시지가 표시되면 이전 메시지는 사라집니다.
- 스케일 및 페이드 애니메이션으로 부드러운 표시/숨김 효과를 제공합니다.

## 고려사항

- 토스트 메시지는 짧고 간결하게 작성하는 것이 좋습니다.
- 중요한 정보나 사용자의 즉각적인 반응이 필요한 경우에는 토스트 대신 모달이나 다른 알림 방식을 사용하는 것이 좋습니다.
- 토스트 메시지는 자동으로 사라지므로 필수적인 정보는 토스트로만 제공하지 마세요.