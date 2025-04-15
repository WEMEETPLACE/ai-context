# Calendar

`Calendar` 컴포넌트는 날짜 선택을 위한 UI 요소로, 단일 날짜 선택과 날짜 범위 선택을 지원합니다.

## 설치

```jsx
import { Calendar } from "@wemeetdev/assembo";
import "@wemeetdev/assembo/dist/index.css"; // 캘린더 스타일을 위해 필요
```

## 기본 사용법

### 단일 날짜 선택

```jsx
import { useState } from "react";
import { Calendar } from "@wemeetdev/assembo";

function Example() {
  const [value, setValue] = useState(new Date());

  return (
    <Calendar
      value={value}
      onChange={setValue}
      type="single"
    />
  );
}
```

### 날짜 범위 선택

```jsx
import { useState } from "react";
import { Calendar } from "@wemeetdev/assembo";

function Example() {
  const [dateRange, setDateRange] = useState([new Date(), null]);

  return (
    <Calendar
      value={dateRange}
      onChange={setDateRange}
      type="range"
    />
  );
}
```

## 이벤트가 있는 날짜 표시

```jsx
function Example() {
  const [value, setValue] = useState(new Date());
  
  // 이벤트가 있는 날짜 배열
  const eventDates = [
    new Date(2025, 3, 10),
    new Date(2025, 3, 15),
    new Date(2025, 3, 20)
  ];

  return (
    <Calendar
      value={value}
      onChange={setValue}
      eventDates={eventDates}
    />
  );
}
```

## 최대 선택 가능 날짜 설정

```jsx
function Example() {
  const [value, setValue] = useState(new Date());
  const maxDate = new Date();
  maxDate.setMonth(maxDate.getMonth() + 2); // 2개월 후

  return (
    <Calendar
      value={value}
      onChange={setValue}
      maxDate={maxDate}
    />
  );
}
```

## 버튼 이벤트 처리

```jsx
function Example() {
  const [value, setValue] = useState(new Date());
  const [selectedDate, setSelectedDate] = useState(null);

  const handleApply = (newValue) => {
    setSelectedDate(newValue);
    // 적용 버튼을 클릭했을 때 추가 처리
  };

  const handleCancel = () => {
    // 취소 버튼을 클릭했을 때 처리
  };

  return (
    <div>
      <Calendar
        value={value}
        onChange={setValue}
        onApply={handleApply}
        onCancel={handleCancel}
      />
      {selectedDate && (
        <div>선택된 날짜: {selectedDate.toLocaleDateString()}</div>
      )}
    </div>
  );
}
```

## 날짜 선택기 컴포넌트와 함께 사용

```jsx
import { useState } from "react";
import { Calendar, Input, IconCalendarLine } from "@wemeetdev/assembo";

function DatePickerExample() {
  const [showCalendar, setShowCalendar] = useState(false);
  const [selectedDate, setSelectedDate] = useState(null);

  const handleDateChange = (date) => {
    setSelectedDate(date);
    setShowCalendar(false);
  };

  return (
    <div className="relative">
      <Input
        value={selectedDate ? selectedDate.toLocaleDateString() : ''}
        placeholder="날짜 선택"
        onClick={() => setShowCalendar(true)}
        readOnly
        icon={<IconCalendarLine />}
      />
      
      {showCalendar && (
        <div className="absolute z-10 mt-1">
          <Calendar
            value={selectedDate}
            onChange={handleDateChange}
            onCancel={() => setShowCalendar(false)}
          />
        </div>
      )}
    </div>
  );
}
```

## Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `value` | `Date \| null \| [Date \| null, Date \| null]` | - | 선택된 날짜 또는 날짜 범위 |
| `onChange` | `(value: Date \| null \| [Date \| null, Date \| null]) => void` | - | 날짜 선택 시 호출되는 콜백 함수 |
| `eventDates` | `Date[]` | `[]` | 이벤트가 있는 날짜 배열 (점으로 표시됨) |
| `onCancel` | `() => void` | - | 취소 버튼 클릭 시 호출되는 콜백 함수 |
| `onApply` | `(value: Date \| null \| [Date \| null, Date \| null]) => void` | - | 적용 버튼 클릭 시 호출되는 콜백 함수 |
| `type` | `'single'` \| `'range'` | `'single'` | 캘린더 타입 (단일 날짜 또는 날짜 범위) |
| `maxDate` | `Date` | - | 선택 가능한 최대 날짜 |
| `onClickDay` | `(value: Date, event: React.MouseEvent) => void` | - | 날짜 클릭 시 호출되는 콜백 함수 |

## 스타일 가이드

Calendar 컴포넌트의 스타일은 CSS 파일을 통해 정의됩니다. 주요 스타일 특징:

- 오늘 날짜는 특별한 스타일로 표시됩니다.
- 선택된 날짜는 강조 표시됩니다.
- 주말은 다른 색상으로 표시됩니다.
- 범위 선택 시 시작일, 종료일, 그 사이의 날짜가 다르게 스타일링됩니다.
- 이벤트가 있는 날짜는 점으로 표시됩니다.

## 접근성

- 날짜 선택 UI는 키보드 네비게이션을 지원합니다.
- 적절한 ARIA 속성을 사용하여 스크린 리더와의 호환성을 제공합니다.
- 날짜 형식은 현지화를 지원합니다.

## 주의사항

- Calendar 컴포넌트를 사용하려면 CSS 파일을 반드시 임포트해야 합니다.
- 타입이 "range"일 때는 value가 [시작일, 종료일] 형태의 배열이어야 합니다.
- maxDate 속성을 설정하면 해당 날짜 이후로 탐색이 제한됩니다.