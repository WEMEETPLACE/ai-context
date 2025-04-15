# Flex

`Flex` 컴포넌트는 Flexbox 레이아웃을 손쉽게 구현할 수 있는 유틸리티 컴포넌트입니다. 직관적인 props를 통해 Flexbox의 다양한 속성을 쉽게 제어할 수 있습니다.

## 설치

```jsx
import { Flex } from "@wemeetdev/assembo";
```

## 기본 사용법

```jsx
// 기본 가로 배치
<Flex gap={16}>
  <div>항목 1</div>
  <div>항목 2</div>
  <div>항목 3</div>
</Flex>

// 세로 배치
<Flex direction="column" gap={8}>
  <div>항목 1</div>
  <div>항목 2</div>
  <div>항목 3</div>
</Flex>
```

## 정렬 설정

```jsx
// 가운데 정렬
<Flex justify="center" align="center">
  <div>가운데 정렬된 콘텐츠</div>
</Flex>

// 양쪽 정렬
<Flex justify="space-between" align="center">
  <div>왼쪽 항목</div>
  <div>오른쪽 항목</div>
</Flex>

// 끝 정렬
<Flex justify="flex-end" align="flex-end">
  <div>오른쪽 하단에 정렬된 항목</div>
</Flex>
```

## 간격 설정

gap 속성을 사용하여 자식 요소 간의 간격을 설정할 수 있습니다. 값은 픽셀 단위의 숫자로 전달합니다.

```jsx
// 8픽셀 간격
<Flex gap={8}>
  <div>항목 1</div>
  <div>항목 2</div>
  <div>항목 3</div>
</Flex>

// 16픽셀 간격
<Flex gap={16}>
  <div>항목 1</div>
  <div>항목 2</div>
  <div>항목 3</div>
</Flex>

// 20픽셀 간격
<Flex gap={20}>
  <div>항목 1</div>
  <div>항목 2</div>
  <div>항목 3</div>
</Flex>
```

## 래핑 설정

flex-wrap 속성을 설정하여 아이템이 넘칠 경우의 동작을 제어할 수 있습니다.

```jsx
// 래핑 활성화
<Flex wrap={true}>
  {/* 많은 항목들... */}
</Flex>

// 래핑 비활성화 (기본값)
<Flex wrap={false}>
  {/* 항목들... */}
</Flex>

// 직접 값 지정
<Flex wrap="wrap-reverse">
  {/* 항목들... */}
</Flex>
```

## 다양한 HTML 요소로 렌더링

`as` prop을 사용하여 div 이외의 다른 HTML 요소로 렌더링할 수 있습니다.

```jsx
// 목록으로 렌더링
<Flex as="ul" gap={8}>
  <li>목록 항목 1</li>
  <li>목록 항목 2</li>
  <li>목록 항목 3</li>
</Flex>

// 네비게이션으로 렌더링
<Flex as="nav" justify="space-between">
  <a href="/">홈</a>
  <a href="/about">소개</a>
  <a href="/contact">연락처</a>
</Flex>

// 섹션으로 렌더링
<Flex as="section" direction="column" gap={16}>
  <h2>섹션 제목</h2>
  <p>섹션 내용</p>
</Flex>
```

## 중첩 사용

Flex 컴포넌트는 중첩해서 사용할 수 있으며, 복잡한 레이아웃을 구성할 수 있습니다.

```jsx
<Flex direction="column" gap={16}>
  <h2>제목</h2>
  
  <Flex gap={8}>
    <div>좌측 콘텐츠</div>
    
    <Flex direction="column" gap={8} flex={1}>
      <div>우측 상단</div>
      <div>우측 하단</div>
    </Flex>
  </Flex>
  
  <Flex justify="flex-end">
    <button>제출</button>
  </Flex>
</Flex>
```

## Props

| 속성 | 타입 | 기본값 | 설명 |
|------|------|-------|------|
| `gap` | `number` | - | 자식 요소 간의 간격 (픽셀) |
| `direction` | `'row'` \| `'column'` \| `'row-reverse'` \| `'column-reverse'` | - | flex-direction 속성 |
| `flex` | `number` \| `string` | - | flex 속성 값 |
| `justify` | `'flex-start'` \| `'flex-end'` \| `'center'` \| `'space-between'` \| `'space-around'` \| `'space-evenly'` | - | justify-content 속성 |
| `align` | `'flex-start'` \| `'flex-end'` \| `'center'` \| `'stretch'` \| `'baseline'` | - | align-items 속성 |
| `wrap` | `boolean` \| `'nowrap'` \| `'wrap'` \| `'wrap-reverse'` | `false` | flex-wrap 속성 |
| `as` | `ElementType` | `'div'` | 렌더링할 HTML 요소 |
| `className` | `string` | - | 추가 CSS 클래스 |
| `style` | `CSSProperties` | - | 인라인 스타일 |
| `children` | `ReactNode` | - | 자식 요소 |

기타 HTML 요소의 모든 기본 속성을 지원합니다.

## 실용적인 패턴

### 1. 카드 레이아웃

```jsx
<Flex direction="column" gap={16}>
  {cards.map(card => (
    <div key={card.id} className="card">
      <h3>{card.title}</h3>
      <p>{card.description}</p>
    </div>
  ))}
</Flex>
```

### 2. 폼 필드 그룹

```jsx
<Flex direction="column" gap={16}>
  <Flex direction="column" gap={4}>
    <label htmlFor="name">이름</label>
    <input id="name" type="text" />
  </Flex>
  
  <Flex direction="column" gap={4}>
    <label htmlFor="email">이메일</label>
    <input id="email" type="email" />
  </Flex>
  
  <Flex justify="flex-end">
    <button type="submit">제출</button>
  </Flex>
</Flex>
```

### 3. 헤더 레이아웃

```jsx
<Flex as="header" justify="space-between" align="center">
  <div className="logo">로고</div>
  
  <Flex as="nav" gap={16}>
    <a href="/">홈</a>
    <a href="/about">소개</a>
    <a href="/contact">연락처</a>
  </Flex>
  
  <Flex gap={8}>
    <button>로그인</button>
    <button>회원가입</button>
  </Flex>
</Flex>
```

## 주의사항

- Flex 컴포넌트는 기본적으로 display: flex가 적용됩니다.
- 자식 요소에 flex 비율을 적용하려면 해당 요소에 적절한 className을 부여하거나 직접 스타일을 설정해야 합니다.
- 브라우저 호환성을 위해 최신 Flexbox 기능 일부는 지원하지 않을 수 있습니다.