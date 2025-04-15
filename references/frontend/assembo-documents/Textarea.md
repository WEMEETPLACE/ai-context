# Textarea

`Textarea` 컴포넌트는 여러 줄의 텍스트 입력을 받기 위한 요소입니다. 기본 텍스트 영역 기능과 글자 수 제한 기능을 제공합니다.

## 설치

```jsx
import { Textarea, TextareaCounter } from "@wemeetdev/assembo";
```

## 기본 사용법

```jsx
// 기본 텍스트 영역
<Textarea placeholder="내용을 입력해주세요" />

// 비활성화 상태
<Textarea disabled placeholder="비활성화된 텍스트 영역" />

// 에러 상태
<Textarea error placeholder="오류 상태의 텍스트 영역" />

// 초기값 설정
<Textarea defaultValue="이미 입력된 내용입니다" />

// 높이 설정
<Textarea rows={5} placeholder="5줄 높이의 텍스트 영역" />

// 리사이즈 제어
<Textarea style={{ resize: "none" }} placeholder="리사이즈 불가능" />
<Textarea style={{ resize: "vertical" }} placeholder="수직 리사이즈만 가능" />
```

## TextareaCounter로 글자 수 제한하기

`TextareaCounter` 컴포넌트를 사용하여 최대 글자 수를 제한하고 현재 입력한 글자 수를 표시할 수 있습니다.

```jsx
// 최대 100자 제한
<TextareaCounter max={100}>
  <Textarea placeholder="최대 100자까지 입력 가능합니다" />
</TextareaCounter>

// 에러 상태와 함께 사용
const [value, setValue] = useState("너무 긴 텍스트입니다...");
const maxLength = 10;
const isError = value.length > maxLength;

<TextareaCounter max={maxLength}>
  <Textarea
    error={isError}
    value={value}
    onChange={(e) => setValue(e.target.value)}
    placeholder="최대 10자까지 입력 가능합니다"
  />
</TextareaCounter>
```

## 폼 내에서 사용

```jsx
function TextareaForm() {
  const [formData, setFormData] = useState({
    comment: "",
  });

  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value,
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    // 폼 제출 로직...
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="comment">의견:</label>
      <Textarea
        id="comment"
        name="comment"
        value={formData.comment}
        onChange={handleChange}
        placeholder="의견을 입력해주세요"
        rows={4}
      />
      <button type="submit">제출</button>
    </form>
  );
}
```

## Props

### Textarea Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `error` | `boolean` | `false` | 에러 상태 표시 여부 |
| `className` | `string` | - | 추가 CSS 클래스 |

추가로 HTML의 `textarea` 요소에서 지원하는 모든 속성을 사용할 수 있습니다 (placeholder, disabled, rows, cols, value, onChange 등).

### TextareaCounter Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `max` | `number` | 필수 | 최대 글자 수 제한 |
| `children` | `ReactElement` | 필수 | Textarea 컴포넌트 (단일 자식) |
| `className` | `string` | - | 추가 CSS 클래스 |

## 스타일 가이드

- Textarea의 기본 높이는 auto이며, rows 속성으로 조절할 수 있습니다.
- 기본적으로 리사이즈가 가능하며, style 속성으로 리사이즈 동작을 제어할 수 있습니다.
- error 상태에서는 빨간색 테두리와 배경으로 표시됩니다.
- TextareaCounter는 Textarea 아래에 현재 글자 수와 최대 글자 수를 표시합니다.
- 글자 수가 최대 제한을 초과하면 자동으로 잘립니다.

## 접근성

- 항상 Textarea와 함께 label을 사용하는 것을 권장합니다.
- 에러 메시지가 있는 경우 aria-describedby 속성으로 에러 메시지 요소를 연결하세요.

```jsx
<div>
  <label htmlFor="comment">의견:</label>
  <Textarea
    id="comment"
    aria-describedby="comment-error"
    error={hasError}
  />
  {hasError && <div id="comment-error">올바른 형식으로 입력해주세요.</div>}
</div>
```