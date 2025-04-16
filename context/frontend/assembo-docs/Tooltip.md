# Tooltip

`Tooltip` 컴포넌트는 요소에 마우스를 올렸을 때 추가 정보를 표시하는 UI 요소입니다.

## 설치

```jsx
import { Tooltip } from "@wemeetdev/assembo";
```

## 기본 사용법

```jsx
// 기본 사용법
<Tooltip content="추가 정보">
  <button>마우스를 올려보세요</button>
</Tooltip>

// 텍스트에 툴팁 적용
<Tooltip content="이 기능은 관리자만 사용할 수 있습니다.">
  <span className="underline cursor-help">관리자 전용</span>
</Tooltip>

// 아이콘에 툴팁 적용
import { IconInfo } from "@wemeetdev/assembo";

<Tooltip content="이 정보는 중요합니다.">
  <IconInfo size={16} className="cursor-help" />
</Tooltip>
```

## 툴팁 위치 설정

툴팁의 위치를 여섯 가지 방향으로 설정할 수 있습니다.

```jsx
// 상단 (기본값)
<Tooltip content="상단에 표시" placement="top">
  <button>버튼</button>
</Tooltip>

// 하단
<Tooltip content="하단에 표시" placement="bottom">
  <button>버튼</button>
</Tooltip>

// 좌측
<Tooltip content="좌측에 표시" placement="left">
  <button>버튼</button>
</Tooltip>

// 우측
<Tooltip content="우측에 표시" placement="right">
  <button>버튼</button>
</Tooltip>

// 좌측 상단
<Tooltip content="좌측 상단에 표시" placement="topLeft">
  <button>버튼</button>
</Tooltip>

// 우측 상단
<Tooltip content="우측 상단에 표시" placement="topRight">
  <button>버튼</button>
</Tooltip>
```

## 오프셋 설정

툴팁과 트리거 요소 사이의 간격을 설정할 수 있습니다.

```jsx
// 기본 간격 (4px)
<Tooltip content="기본 간격">
  <button>버튼</button>
</Tooltip>

// 간격 늘리기
<Tooltip content="더 넓은 간격" offset={8}>
  <button>버튼</button>
</Tooltip>
```

## 복잡한 콘텐츠

툴팁 내용으로 단순 텍스트뿐만 아니라 복잡한 요소도 표시할 수 있습니다.

```jsx
// 여러 줄 텍스트
<Tooltip
  content={
    <div>
      <p>첫 번째 줄</p>
      <p>두 번째 줄</p>
    </div>
  }
>
  <button>마우스를 올려보세요</button>
</Tooltip>

// 리스트 표시
<Tooltip
  content={
    <ul className="list-disc pl-4">
      <li>항목 1</li>
      <li>항목 2</li>
      <li>항목 3</li>
    </ul>
  }
>
  <button>참고 사항</button>
</Tooltip>

// 텍스트와 아이콘 조합
import { IconInfo } from "@wemeetdev/assembo";

<Tooltip
  content={
    <div className="flex items-center gap-1">
      <IconInfo size={14} />
      <span>중요한 정보입니다.</span>
    </div>
  }
>
  <button>자세히 보기</button>
</Tooltip>
```

## 사용 사례

```jsx
// 비활성화된 버튼에 이유 설명
<Tooltip content="권한이 없습니다.">
  <span>
    <button disabled>삭제</button>
  </span>
</Tooltip>

// 축약된 텍스트에 전체 내용 표시
<Tooltip content="이것은 매우 긴 텍스트의 전체 내용입니다. 화면에 모두 표시할 수 없어 일부분만 보여집니다.">
  <p className="truncate w-40">이것은 매우 긴 텍스트의 전체 내용입니다...</p>
</Tooltip>

// 기능 설명
<Tooltip content="사용자를 추가합니다.">
  <button>
    <IconPlus size={16} />
  </button>
</Tooltip>
```

## Props

| 속성        | 타입                                                                          | 기본값  | 설명                                   |
| ----------- | ----------------------------------------------------------------------------- | ------- | -------------------------------------- |
| `content`   | `ReactNode`                                                                   | 필수    | 툴팁에 표시할 내용                     |
| `children`  | `ReactElement`                                                                | 필수    | 툴팁을 트리거할 요소 (단일 자식)       |
| `placement` | `'top'` \| `'bottom'` \| `'left'` \| `'right'` \| `'topLeft'` \| `'topRight'` | `'top'` | 툴팁이 표시될 위치                     |
| `offset`    | `number`                                                                      | `4`     | 툴팁과 트리거 사이의 간격 (픽셀)       |
| `className` | `string`                                                                      | -       | 툴팁 컨테이너에 적용할 추가 CSS 클래스 |

## 접근성

- 툴팁은 hover와 focus 이벤트 모두에 반응하므로 키보드 사용자도 접근할 수 있습니다.
- 간결하고 명확한 내용을 제공하여 사용자의 이해를 돕습니다.
- 툴팁은 추가 정보 제공을 위해 사용되어야 하며, 필수 정보는 항상 가시적으로 표시해야 합니다.
