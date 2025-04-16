# DatePicker

`DatePicker` 컴포넌트는 사용자가 날짜를 선택할 수 있는 복합 UI 컴포넌트입니다. 단일 날짜 선택과 날짜 범위 선택을 지원하며, 여러 하위 컴포넌트를 조합하여 유연하게 사용할 수 있습니다.

## 설치

```jsx
import {
  DatePicker,
  DatePickerTrigger,
  DatePickerInput,
  DatePickerContent,
  DatePickerCalendar,
} from "@wemeetdev/assembo";
import "@wemeetdev/assembo/dist/index.css"; // 캘린더 스타일을 위해 필요
```

## 컴포넌트 구성

DatePicker는 다음과 같은 하위 컴포넌트로 구성됩니다:

1. **DatePicker**: 전체 DatePicker의 상태를 관리하는 루트 컴포넌트
2. **DatePickerTrigger**: 클릭 시 날짜 선택 팝업을 열고 닫는 트리거 컴포넌트
3. **DatePickerInput**: 기본 제공되는 날짜 입력 필드 컴포넌트
4. **DatePickerContent**: 팝업 컨텐츠를 포지셔닝하고 렌더링하는 컴포넌트
5. **DatePickerCalendar**: 실제 달력 UI를 제공하는 컴포넌트

## 기본 사용법

### 단일 날짜 선택

```jsx
import { useState } from "react";
import {
  DatePicker,
  DatePickerTrigger,
  DatePickerInput,
  DatePickerContent,
  DatePickerCalendar,
} from "@wemeetdev/assembo";
import dayjs from "dayjs"; // 날짜 포맷팅을 위해 사용 (선택사항)

function Example() {
  const [value, setValue] = useState(new Date());
  const dateString = dayjs(value).format("YYYY-MM-DD"); // 선택한 날짜 포맷팅

  return (
    <DatePicker value={value} onChange={setValue}>
      <DatePickerTrigger>
        <DatePickerInput>{dateString}</DatePickerInput>
      </DatePickerTrigger>
      <DatePickerContent>
        <DatePickerCalendar />
      </DatePickerContent>
    </DatePicker>
  );
}
```

### 날짜 범위 선택

```jsx
import { useState } from "react";
import {
  DatePicker,
  DatePickerTrigger,
  DatePickerInput,
  DatePickerContent,
  DatePickerCalendar,
} from "@wemeetdev/assembo";
import dayjs from "dayjs";

function Example() {
  const [rangeValue, setRangeValue] = useState([
    new Date(),
    dayjs().add(7, "day").toDate(),
  ]);

  const startDateString = dayjs(rangeValue[0]).format("YYYY-MM-DD");
  const endDateString = dayjs(rangeValue[1]).format("YYYY-MM-DD");

  return (
    <DatePicker value={rangeValue} onChange={setRangeValue}>
      <DatePickerTrigger>
        <DatePickerInput>
          {`${startDateString} - ${endDateString}`}
        </DatePickerInput>
      </DatePickerTrigger>
      <DatePickerContent>
        <DatePickerCalendar type="range" />
      </DatePickerContent>
    </DatePicker>
  );
}
```

## 커스텀 트리거

DatePicker는 기본 제공되는 `DatePickerInput` 대신 커스텀 트리거를 사용할 수 있습니다.

```jsx
import {
  DatePicker,
  DatePickerTrigger,
  DatePickerContent,
  DatePickerCalendar,
  IconCalenderLine,
  Text,
} from "@wemeetdev/assembo";

function Example() {
  const [value, setValue] = useState(new Date());
  const dateString = dayjs(value).format("YYYY-MM-DD");

  return (
    <DatePicker value={value} onChange={setValue}>
      <DatePickerTrigger>
        <div className="flex items-center gap-2 border rounded p-2">
          <Text variant="bodySB16">{dateString}</Text>
          <IconCalenderLine size={20} />
        </div>
      </DatePickerTrigger>
      <DatePickerContent>
        <DatePickerCalendar />
      </DatePickerContent>
    </DatePicker>
  );
}
```

## 이벤트가 있는 날짜 표시

특정 날짜에 이벤트를 표시할 수 있습니다.

```jsx
function Example() {
  const [value, setValue] = useState(new Date());

  // 이벤트가 있는 날짜 배열
  const eventDates = [new Date(2025, 3, 10), new Date(2025, 3, 15)];

  return (
    <DatePicker value={value} onChange={setValue} eventDates={eventDates}>
      {/* ... */}
    </DatePicker>
  );
}
```

## 날짜 선택 제한

최소 및 최대 선택 가능 날짜를 설정할 수 있습니다.

```jsx
function Example() {
  const [value, setValue] = useState(new Date());

  // 오늘부터 한 달 후까지만 선택 가능
  const minDate = new Date();
  const maxDate = new Date();
  maxDate.setMonth(maxDate.getMonth() + 1);

  return (
    <DatePicker
      value={value}
      onChange={setValue}
      minDate={minDate}
      maxDate={maxDate}
    >
      {/* ... */}
    </DatePicker>
  );
}
```

## DatePickerInput 크기 설정

DatePickerInput은 세 가지 크기를 지원합니다.

```jsx
// 작은 크기
<DatePickerInput size="small">
  2025-04-15
</DatePickerInput>

// 중간 크기 (기본값)
<DatePickerInput size="medium">
  2025-04-15
</DatePickerInput>

// 큰 크기
<DatePickerInput size="large">
  2025-04-15
</DatePickerInput>
```

## 프로그래매틱 제어

ref를 사용하여 DatePicker를 프로그래매틱하게 제어할 수 있습니다.

```jsx
import { useRef } from "react";
import { DatePicker } from "@wemeetdev/assembo";

function Example() {
  const [value, setValue] = useState(new Date());
  const datePickerRef = useRef(null);

  const openDatePicker = () => {
    datePickerRef.current?.open();
  };

  const closeDatePicker = () => {
    datePickerRef.current?.close();
  };

  return (
    <div>
      <DatePicker ref={datePickerRef} value={value} onChange={setValue}>
        {/* ... */}
      </DatePicker>
      <div className="mt-4">
        <button onClick={openDatePicker}>열기</button>
        <button onClick={closeDatePicker}>닫기</button>
      </div>
    </div>
  );
}
```

## Props

### DatePicker Props

| 속성         | 타입                                                            | 기본값 | 설명                                  |
| ------------ | --------------------------------------------------------------- | ------ | ------------------------------------- |
| `value`      | `Date \| null \| [Date \| null, Date \| null]`                  | 필수   | 선택된 날짜 또는 날짜 범위            |
| `onChange`   | `(value: Date \| null \| [Date \| null, Date \| null]) => void` | 필수   | 날짜 선택 시 호출되는 콜백 함수       |
| `minDate`    | `Date`                                                          | -      | 선택 가능한 최소 날짜                 |
| `maxDate`    | `Date`                                                          | -      | 선택 가능한 최대 날짜                 |
| `eventDates` | `Date[]`                                                        | `[]`   | 이벤트가 있는 날짜 배열               |
| `style`      | `CSSProperties`                                                 | -      | DatePicker 컨테이너의 인라인 스타일   |
| `className`  | `string`                                                        | -      | DatePicker 컨테이너의 추가 CSS 클래스 |
| `children`   | `ReactNode`                                                     | 필수   | DatePicker 구성 요소                  |

### DatePickerTrigger Props

| 속성       | 타입        | 기본값 | 설명                 |
| ---------- | ----------- | ------ | -------------------- |
| `children` | `ReactNode` | 필수   | 트리거로 사용할 요소 |

### DatePickerInput Props

| 속성        | 타입                                 | 기본값     | 설명                    |
| ----------- | ------------------------------------ | ---------- | ----------------------- |
| `size`      | `'small'` \| `'medium'` \| `'large'` | `'medium'` | 입력 필드의 크기        |
| `className` | `string`                             | -          | 추가 CSS 클래스         |
| `children`  | `ReactNode \| string`                | -          | 표시할 텍스트 또는 내용 |

### DatePickerContent Props

| 속성        | 타입                  | 기본값          | 설명                     |
| ----------- | --------------------- | --------------- | ------------------------ |
| `children`  | `ReactNode`           | 필수            | 팝업 내용                |
| `container` | `HTMLElement \| null` | `document.body` | 팝업을 렌더링할 컨테이너 |

### DatePickerCalendar Props

| 속성        | 타입                    | 기본값     | 설명                                 |
| ----------- | ----------------------- | ---------- | ------------------------------------ |
| `type`      | `'single'` \| `'range'` | `'single'` | 선택 모드 (단일 날짜 또는 날짜 범위) |
| `nextLabel` | `ReactNode`             | -          | 다음 달 버튼의 커스텀 레이블         |
| `prevLabel` | `ReactNode`             | -          | 이전 달 버튼의 커스텀 레이블         |

추가로 Calendar 컴포넌트의 모든 props를 지원합니다.

## DatePicker 메서드

DatePicker 컴포넌트의 ref를 통해 다음 메서드에 접근할 수 있습니다:

| 메서드    | 설명                        |
| --------- | --------------------------- |
| `open()`  | DatePicker 팝업을 엽니다.   |
| `close()` | DatePicker 팝업을 닫습니다. |

## 포지셔닝 시스템

DatePicker는 스마트 포지셔닝 시스템을 사용하여 팝업 위치를 자동으로 조정합니다:

- 뷰포트 공간에 따라 위/아래, 좌/우 방향을 자동 결정
- 스크롤 및 리사이즈 이벤트에 반응하여 위치 재계산
- 중첩된 스크롤 컨테이너 내에서도 올바르게 작동

## 접근성

- 키보드 네비게이션을 통해 캘린더를 조작할 수 있습니다.
- Esc 키를 통해 캘린더를 닫을 수 있습니다.
- 외부 클릭 시 팝업이 자동으로 닫힙니다.
- 적절한 aria 속성을 통해 스크린 리더 접근성을 지원합니다.

## 주의사항

- DatePicker를 사용하려면 반드시 컴포넌트 구조(DatePicker > DatePickerTrigger > DatePickerContent)를 따라야 합니다.
- 날짜 범위 선택 시, value는 `[startDate, endDate]` 형태의 배열이어야 합니다.
- 날짜 포맷팅은 컴포넌트에서 자동으로 제공하지 않으므로, dayjs와 같은 라이브러리를 사용하여 직접 포맷팅해야 합니다.
- DatePickerContent는 Portal을 사용하여 렌더링되므로, 스타일 관련 이슈가 발생할 수 있습니다.
