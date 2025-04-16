# Tabs

`Tabs` 컴포넌트는 콘텐츠를 탭으로 구분하여 표시하는 UI 요소입니다. 두 가지 스타일(기본 탭, 패널 탭)을 지원합니다.

## 설치

```jsx
import {
  TabsRoot,
  TabsList,
  TabsTrigger,
  TabsPanelTrigger,
  TabsContent,
} from "@wemeetdev/assembo";
```

## 기본 탭 사용법

기본 탭은 하단에 밑줄이 있는 스타일입니다.

```jsx
<TabsRoot defaultValue="tab1">
  <TabsList>
    <TabsTrigger value="tab1">탭 1</TabsTrigger>
    <TabsTrigger value="tab2">탭 2</TabsTrigger>
    <TabsTrigger value="tab3">탭 3</TabsTrigger>
  </TabsList>

  <TabsContent value="tab1">
    <div className="p-4">탭 1의 내용입니다.</div>
  </TabsContent>

  <TabsContent value="tab2">
    <div className="p-4">탭 2의 내용입니다.</div>
  </TabsContent>

  <TabsContent value="tab3">
    <div className="p-4">탭 3의 내용입니다.</div>
  </TabsContent>
</TabsRoot>
```

## 패널 탭 사용법

패널 탭은 상단이 둥근 패널 형태의 스타일입니다.

```jsx
<TabsRoot defaultValue="tab1">
  <TabsList>
    <TabsPanelTrigger value="tab1">탭 1</TabsPanelTrigger>
    <TabsPanelTrigger value="tab2">탭 2</TabsPanelTrigger>
    <TabsPanelTrigger value="tab3">탭 3</TabsPanelTrigger>
  </TabsList>

  <TabsContent value="tab1">
    <div className="p-4 border border-t-0 rounded-b-md">탭 1의 내용입니다.</div>
  </TabsContent>

  <TabsContent value="tab2">
    <div className="p-4 border border-t-0 rounded-b-md">탭 2의 내용입니다.</div>
  </TabsContent>

  <TabsContent value="tab3">
    <div className="p-4 border border-t-0 rounded-b-md">탭 3의 내용입니다.</div>
  </TabsContent>
</TabsRoot>
```

## 크기 설정

탭의 크기를 medium(기본값) 또는 large로 설정할 수 있습니다.

```jsx
// 중간 크기 (기본값)
<TabsRoot defaultValue="tab1" size="medium">
  {/* ... */}
</TabsRoot>

// 큰 크기
<TabsRoot defaultValue="tab1" size="large">
  {/* ... */}
</TabsRoot>
```

## 아이콘과 함께 사용

```jsx
import {
  TabsRoot,
  TabsList,
  TabsTrigger,
  TabsContent,
  IconHome,
  IconSettings,
  IconPerson,
} from "@wemeetdev/assembo";

<TabsRoot defaultValue="home">
  <TabsList>
    <TabsTrigger value="home">
      <div className="flex items-center gap-1">
        <IconHome size={16} />
        <span>홈</span>
      </div>
    </TabsTrigger>
    <TabsTrigger value="settings">
      <div className="flex items-center gap-1">
        <IconSettings size={16} />
        <span>설정</span>
      </div>
    </TabsTrigger>
    <TabsTrigger value="profile">
      <div className="flex items-center gap-1">
        <IconPerson size={16} />
        <span>프로필</span>
      </div>
    </TabsTrigger>
  </TabsList>

  <TabsContent value="home">
    <div className="p-4">홈 페이지 내용</div>
  </TabsContent>

  <TabsContent value="settings">
    <div className="p-4">설정 페이지 내용</div>
  </TabsContent>

  <TabsContent value="profile">
    <div className="p-4">프로필 페이지 내용</div>
  </TabsContent>
</TabsRoot>;
```

## Props

### TabsRoot Props

| 속성           | 타입                    | 기본값     | 설명                |
| -------------- | ----------------------- | ---------- | ------------------- |
| `defaultValue` | `string`                | 필수       | 초기 선택될 탭의 값 |
| `size`         | `'medium'` \| `'large'` | `'medium'` | 탭의 크기           |
| `children`     | `ReactNode`             | 필수       | 탭 컴포넌트들       |

### TabsList Props

| 속성        | 타입        | 기본값 | 설명                 |
| ----------- | ----------- | ------ | -------------------- |
| `className` | `string`    | -      | 추가 CSS 클래스      |
| `children`  | `ReactNode` | 필수   | 탭 트리거 컴포넌트들 |

### TabsTrigger Props

| 속성        | 타입        | 기본값 | 설명                |
| ----------- | ----------- | ------ | ------------------- |
| `value`     | `string`    | 필수   | 이 탭의 고유 식별자 |
| `className` | `string`    | -      | 추가 CSS 클래스     |
| `children`  | `ReactNode` | 필수   | 탭 제목 내용        |

### TabsPanelTrigger Props

| 속성        | 타입        | 기본값 | 설명                |
| ----------- | ----------- | ------ | ------------------- |
| `value`     | `string`    | 필수   | 이 탭의 고유 식별자 |
| `className` | `string`    | -      | 추가 CSS 클래스     |
| `children`  | `ReactNode` | 필수   | 탭 제목 내용        |

### TabsContent Props

| 속성        | 타입        | 기본값 | 설명                       |
| ----------- | ----------- | ------ | -------------------------- |
| `value`     | `string`    | 필수   | 이 컨텐츠와 연결된 탭의 값 |
| `className` | `string`    | -      | 추가 CSS 클래스            |
| `children`  | `ReactNode` | 필수   | 탭 내용                    |

## 접근성

- 탭 컴포넌트는 WAI-ARIA 탭 패턴을 준수합니다.
- 키보드 탐색을 지원합니다.
- 적절한 aria 속성을 사용하여 스크린 리더 사용자에게 접근성을 제공합니다.

## 주의사항

- 각 탭의 `value`는 고유해야 합니다.
- 탭 컨텐츠의 `value`는 해당 탭 트리거의 `value`와 일치해야 합니다.
- 탭 제목은 간결하고 명확하게 작성하는 것이 좋습니다.
- 모든 탭 컨텐츠는 비슷한 성격의 정보를 포함하도록 구성하는 것이 좋습니다.
