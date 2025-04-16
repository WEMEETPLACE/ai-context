# Tag

`Tag` 컴포넌트는 라벨이나 카테고리를 표시하고 필요에 따라 삭제할 수 있는 UI 요소입니다.

## 설치

```jsx
import { Tag } from "@wemeetdev/assembo";
```

## 기본 사용법

```jsx
// 기본 태그
<Tag value="example">기본 태그</Tag>

// 삭제 이벤트 처리
<Tag
  value="example"
  onClose={(value) => console.log(`태그 삭제: ${value}`)}
>
  삭제 가능한 태그
</Tag>

// 비활성화 태그
<Tag value="example" disabled>
  비활성화된 태그
</Tag>
```

## 태그 목록 관리

```jsx
function TagsExample() {
  const [tags, setTags] = useState(["React", "TypeScript", "Tailwind"]);

  const handleDelete = (value) => {
    setTags(tags.filter((tag) => tag !== value));
  };

  return (
    <div className="flex flex-wrap gap-2">
      {tags.map((tag) => (
        <Tag key={tag} value={tag} onClose={handleDelete}>
          {tag}
        </Tag>
      ))}
    </div>
  );
}
```

## 아이콘과 함께 사용

```jsx
import { Tag, IconClock } from "@wemeetdev/assembo";

<Tag value="time">
  <div className="flex items-center gap-1">
    <IconClock size={14} />
    <span>10분 전</span>
  </div>
</Tag>;
```

## 커스텀 스타일링

```jsx
// 색상 변경
<Tag
  value="custom"
  className="bg-blue-100 text-blue-800 hover:bg-blue-200"
  onClose={(value) => console.log(`태그 삭제: ${value}`)}
>
  커스텀 태그
</Tag>

// 크기 변경
<Tag
  value="small"
  className="text-xs py-0.5"
  onClose={(value) => console.log(`태그 삭제: ${value}`)}
>
  작은 태그
</Tag>

<Tag
  value="large"
  className="text-lg py-1.5 px-3"
  onClose={(value) => console.log(`태그 삭제: ${value}`)}
>
  큰 태그
</Tag>
```

## Props

| 속성        | 타입                                | 기본값  | 설명                                       |
| ----------- | ----------------------------------- | ------- | ------------------------------------------ |
| `value`     | `string` \| `number`                | 필수    | 태그의 고유 식별자 (onClose 콜백에 전달됨) |
| `onClose`   | `(value: string \| number) => void` | -       | 태그 삭제 버튼 클릭 시 호출되는 콜백 함수  |
| `disabled`  | `boolean`                           | `false` | 비활성화 상태 및 삭제 버튼 숨김 여부       |
| `className` | `string`                            | -       | 추가 CSS 클래스                            |
| `children`  | `ReactNode`                         | -       | 태그 내용                                  |

## 접근성

- 태그 삭제 버튼에 적절한 aria-label을 포함하는 것이 좋습니다.
- 태그 목록에서 키보드로 탐색 및 삭제할 수 있도록 설계하세요.

```jsx
<Tag
  value="example"
  onClose={(value) => handleDelete(value)}
  aria-label="태그 삭제: example"
>
  Example
</Tag>
```

## 주의사항

- 태그가 너무 많아지면 UI가 복잡해질 수 있으므로 적절한 수의 태그를 유지하세요.
- 태그 내용은 간결하게 유지하는 것이 좋습니다.
- `value` 속성은 고유해야 효과적인 관리가 가능합니다.
