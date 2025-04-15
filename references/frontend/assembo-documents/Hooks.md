# Assembo Hooks

Assembo는 React 애플리케이션 개발을 위한 유용한 훅(Hook)을 제공합니다. 이 문서에서는 각 훅의 기능과 사용법을 설명합니다.

## 목차

- [useDrawer](#usedrawer) - 드로어 컴포넌트 제어
- [useInput](#useinput) - 입력 필드 상태 관리
- [useInputNumber](#useinputnumber) - 숫자 입력 필드 상태 관리
- [useModal](#usemodal) - 모달 컴포넌트 제어
- [useSpinner](#usespinner) - 전역 로딩 스피너 제어
- [useTimer](#usetimer) - 타이머 기능 제어

## useDrawer

드로어(Drawer) 컴포넌트를 제어하기 위한 훅입니다.

### 사용법

```tsx
import { useDrawer } from 'assembo';

function MyComponent() {
  const drawer = useDrawer({
    autoClose: true,     // ESC 키 누를 때 자동으로 닫힘 (기본값: false)
    position: "right",   // 드로어 위치 - "right" 또는 "left" (기본값: "right")
    width: "400px",      // 드로어 너비 (기본값: "600px")
  });

  const handleOpenDrawer = () => {
    drawer.open(({ close, isOpen, isRemoving, drawerId }) => (
      <div>
        <h2>드로어 컨텐츠</h2>
        <button onClick={close}>닫기</button>
      </div>
    ));
  };

  return (
    <button onClick={handleOpenDrawer}>드로어 열기</button>
  );
}
```

### 반환값

| 속성 | 타입 | 설명 |
|------|------|------|
| `open` | `function` | 드로어를 열기 위한 함수. 컨텐츠 렌더링 함수를 인자로 받음 |
| `close` | `function` | 현재 열린 드로어를 닫는 함수 |
| `closeAll` | `function` | 모든 오버레이(드로어 포함)를 닫는 함수 |

## useInput

입력 필드의 상태를 관리하기 위한 훅입니다.

### 사용법

```tsx
import { useInput } from 'assembo';

function MyForm() {
  const name = useInput('초기값'); // 초기값은 선택 사항입니다

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('제출된 이름:', name.value);
    name.clear(); // 입력 필드 초기화
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={name.value}
        onChange={name.onChange}
        placeholder="이름 입력"
      />
      <button type="submit">제출</button>
    </form>
  );
}
```

### 반환값

| 속성 | 타입 | 설명 |
|------|------|------|
| `value` | `string` | 현재 입력 필드의 값 |
| `onChange` | `function` | 입력 필드의 변경을 처리하는 이벤트 핸들러 |
| `clear` | `function` | 입력 필드를 초기화하는 함수 |

## useInputNumber

숫자 입력 필드의 상태를 관리하기 위한 훅입니다. 숫자만 입력 가능하도록 제한합니다.

### 사용법

```tsx
import { useInputNumber } from 'assembo';

function MyNumberForm() {
  const amount = useInputNumber({
    initialValue: 100,   // 초기값 (선택 사항)
    decimal: true,       // 소수점 입력 허용 (기본값: false)
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (amount.isNotValid) {
      alert('유효한 숫자를 입력해주세요');
      return;
    }
    
    console.log('제출된 금액:', Number(amount.value));
    amount.clear(); // 입력 필드 초기화
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={amount.value}
        onChange={amount.onChange}
        placeholder="금액 입력"
      />
      {amount.isNotValid && <p>유효한 숫자를 입력해주세요</p>}
      <button type="submit">제출</button>
    </form>
  );
}
```

### 반환값

| 속성 | 타입 | 설명 |
|------|------|------|
| `value` | `string` | 현재 입력 필드의 값 (문자열 형태) |
| `isNotValid` | `boolean` | 현재 입력이 유효하지 않은지 여부 |
| `onChange` | `function` | 입력 필드의 변경을 처리하는 이벤트 핸들러 |
| `clear` | `function` | 입력 필드를 초기화하는 함수 |

## useModal

모달(Modal) 컴포넌트를 제어하기 위한 훅입니다.

### 사용법

```tsx
import { useModal } from 'assembo';

function MyComponent() {
  const modal = useModal({
    autoClose: true,             // ESC 키 누를 때 자동으로 닫힘 (기본값: false)
    anchorElement: buttonRef,    // 모달이 위치할 기준 요소 (선택 사항)
    anchorSelector: "#button",   // 위의 anchorElement 대신 CSS 선택자로 지정 가능
    anchorOffset: { x: 0, y: 10 }, // 앵커 요소로부터의 오프셋 (선택 사항)
  });

  const handleOpenModal = () => {
    modal.open(({ close, isOpen, isRemoving, modalId }) => (
      <div>
        <h2>모달 컨텐츠</h2>
        <button onClick={close}>닫기</button>
      </div>
    ));
  };

  return (
    <button onClick={handleOpenModal}>모달 열기</button>
  );
}
```

### 반환값

| 속성 | 타입 | 설명 |
|------|------|------|
| `open` | `function` | 모달을 열기 위한 함수. 컨텐츠 렌더링 함수를 인자로 받음 |
| `close` | `function` | 현재 열린 모달을 닫는 함수 |
| `closeAll` | `function` | 모든 오버레이(모달 포함)를 닫는 함수 |

## useSpinner

전역 로딩 스피너를 제어하기 위한 훅입니다.

### 사용법

```tsx
import { useSpinner } from 'assembo';

function MyComponent() {
  const spinner = useSpinner();

  const handleLoadData = async () => {
    spinner.open(); // 기본 "로딩 중.." 메시지와 함께 스피너 표시
    // 또는 커스텀 컨텐츠 사용:
    // spinner.open(<div>데이터 로딩 중...</div>);

    try {
      await fetchData();
    } finally {
      spinner.close();
    }
  };

  return (
    <button onClick={handleLoadData}>데이터 로드</button>
  );
}
```

### 반환값

| 속성 | 타입 | 설명 |
|------|------|------|
| `open` | `function` | 스피너를 화면에 표시하는 함수. 선택적으로 커스텀 내용을 인자로 받음 |
| `close` | `function` | 현재 표시된 스피너를 닫는 함수 |
| `closeAll` | `function` | 모든 오버레이(스피너 포함)를 닫는 함수 |

## useTimer

타이머 기능을 제공하는 훅입니다. 카운트다운 타이머를 쉽게 구현할 수 있습니다.

### 사용법

```tsx
import { useTimer } from 'assembo';

function MyTimer() {
  const timer = useTimer({
    initialSeconds: 60,  // 타이머 초기 시간(초)
    onExpire: () => {    // 타이머가 만료되었을 때 실행할 콜백 함수
      alert('시간이 종료되었습니다!');
    }
  });

  return (
    <div>
      <p>남은 시간: {timer.formattedTime}</p>
      <button onClick={timer.pause} disabled={!timer.isRunning}>
        일시정지
      </button>
      <button onClick={timer.resume} disabled={timer.isRunning || timer.isExpired}>
        재개
      </button>
      <button onClick={timer.reset}>
        재설정
      </button>
    </div>
  );
}
```

### 반환값

| 속성 | 타입 | 설명 |
|------|------|------|
| `seconds` | `number` | 현재 남은 시간(초) |
| `isRunning` | `boolean` | 타이머가 실행 중인지 여부 |
| `isExpired` | `boolean` | 타이머가 만료되었는지 여부 |
| `formattedTime` | `string` | "MM:SS" 형식의 포맷팅된 시간 |
| `reset` | `function` | 타이머를 초기 시간으로 재설정하는 함수 |
| `pause` | `function` | 타이머를 일시 정지하는 함수 |
| `resume` | `function` | 일시 정지된 타이머를 다시 시작하는 함수 |