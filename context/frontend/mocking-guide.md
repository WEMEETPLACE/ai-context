# MOCKING 가이드

## 1. 개요

이 문서는 프로젝트의 Mock 서비스 워커(MSW) 설정 및 사용법에 대한 가이드를 제공합니다. MSW를 통해 백엔드 API를 모킹하여 프론트엔드 개발 및 테스트를 더 효율적으로 진행할 수 있습니다.

## 2. 응답 형식 규칙

모든 API 응답은 다음 형식을 따라야 합니다:

### 성공 응답 형식

```javascript
{
  result: "SUCCESS",
  data: {
    // 실제 응답 데이터
  },
  error: null
}
```

### 실패 응답 형식

```javascript
{
  result: "FAIL",
  data: null,
  error: "ERROR_CODE" // MockingDevTool에서 설정 가능한 에러 코드
}
```

## 3. MockingDevTool 사용법

MockingDevTool은 브라우저에서 직접 API 응답을 제어할 수 있는 UI 도구입니다.

### 주요 기능

- **MSW 활성화/비활성화**: 모킹 기능 자체를 켜고 끌 수 있음 (비활성화 시 실제 API 요청 진행)
- **에러 응답 반환**: 모든 API 요청에 대해 성공 또는 실패 응답 전환
- **상태 코드**: 실패 시 반환할 HTTP 상태 코드 설정 (400, 401, 403, 404, 500, 503)
- **에러 코드**: 실패 시 반환할 에러 코드 문자열 설정
- **지연 시간**: API 응답 지연 시간 설정 (0ms ~ 5000ms)

### 설정 저장

- 모든 설정은 localStorage에 자동 저장되어 페이지 새로 고침 후에도 유지됩니다.
- 키: `mswDevConfig`
- 저장되는 설정 객체 구조:
  ```javascript
  {
    enabled: true,       // MSW 활성화 여부 (true: 모킹 활성화, false: 실제 API 요청 진행)
    shouldFail: false,   // 에러 응답 반환 여부
    statusCode: '500',   // 에러 응답 시 HTTP 상태 코드
    delayMs: 0,          // 응답 지연 시간 (밀리초)
    error: 'ERROR_CODE'  // 에러 응답 시 에러 코드
  }
  ```

### MSW 활성화/비활성화 동작 방식

- **활성화 상태 (enabled: true)**: 정의된 모든 API 엔드포인트가 모킹됩니다.
- **비활성화 상태 (enabled: false)**: MSW 핸들러가 모든 요청을 통과시켜 실제 네트워크 요청이 진행됩니다.

## 4. 새 MSW 핸들러 추가 방법

새로운 API 엔드포인트를 모킹하려면 다음 단계를 따르세요:

### 1. handlers.ts 파일에 핸들러 추가

```javascript
import { http, HttpResponse, delay } from "msw";

// 기존 핸들러 배열에 새 핸들러 추가
export const handlers = [
  // 기존 핸들러들...

  // 새 핸들러 추가
  http.METHOD("*/api/v1/your-endpoint", async ({ request }) => {
    // 현재 모킹 설정 가져오기
    const config = getMockConfig();

    // MSW가 비활성화된 경우, 요청을 통과시킴 (패스스루)
    if (config.enabled === false) {
      const response = await fetch(bypass(request));
      const realResponse = await response.json();
      return HttpResponse.json(realResponse);
    }

    // 설정된 지연 시간 적용
    if (Number(config.delayMs) > 0) {
      await delay(Number(config.delayMs));
    }

    // 요청 파라미터 추출 (필요한 경우)
    const url = new URL(request.url);
    const someParam = url.searchParams.get("paramName");

    // MockingDevTool 설정에 따라 실패 응답 반환
    if (config.shouldFail) {
      return HttpResponse.json(
        {
          result: "FAIL",
          data: null,
          error: config.error,
        },
        { status: parseInt(config.statusCode) || 500 }
      );
    }

    // 성공 응답 반환
    return HttpResponse.json(
      {
        result: "SUCCESS",
        data: {
          // 모의 응답 데이터
        },
        error: null,
      },
      { status: 200 }
    );
  }),
];
```

### 2. 인터페이스 정의

모킹하려는 API 엔드포인트의 요청 및 응답 인터페이스를 정의하세요:

```typescript
// API 요청 파라미터 인터페이스
export interface IYourRequestParams {
  id: string;
  // 기타 파라미터...
}

// API 응답 데이터 인터페이스
export interface IYourResponseData {
  someField: string;
  // 기타 필드...
}
```

## 5. 팁과 모범 사례

1. **일관된 응답 형식 유지**: 모든 모의 응답은 2번 섹션의 응답 형식을 따라야 합니다.
2. **실제 API 동작 모방**: 실제 API와 동일한 동작을 모방하여 개발 및 프로덕션 환경 간의 차이를 최소화하세요.
3. **에지 케이스 고려**: 실패 케이스 및 다양한 상황을 고려하여 모킹하세요.
4. **getMockConfig 활용**: 항상 `getMockConfig` 함수를 사용하여 현재 설정을 가져오세요.
5. **타입 안전성 확보**: TypeScript 인터페이스를 활용하여 타입 안전성을 확보하세요.

## 6. 문제 해결

- **MSW가 작동하지 않는 경우**: 브라우저 콘솔에서 관련 오류 확인
- **MockingDevTool이 표시되지 않는 경우**: 개발 환경에서만 작동하므로 NODE_ENV 확인
- **설정이 적용되지 않는 경우**: localStorage를 확인하고 필요시 초기화

## 7. 고급 사용법

### 조건부 응답

특정 조건에 따라 다른 응답을 반환하려면:

```javascript
// MockingDevTool 설정에 상관없이 특정 조건에 따른 응답
if (someCondition) {
  return HttpResponse.json({ ... }, { status: 403 });
}
```

### 요청 본문 검사

POST/PUT 요청의 본문을 검사하려면:

```javascript
const requestData = await request.json();
// requestData를 기반으로 응답 생성
```

### 페이지네이션 모킹

페이지네이션을 모킹하려면:

```javascript
const page = parseInt(url.searchParams.get("page") || "0");
const size = parseInt(url.searchParams.get("size") || "10");

// 페이지네이션 로직 구현
const totalItems = mockData.length;
const totalPages = Math.ceil(totalItems / size);
const paginatedData = mockData.slice(page * size, (page + 1) * size);

return HttpResponse.json({
  result: "SUCCESS",
  data: {
    content: paginatedData,
    currentPage: page,
    size: size,
    totalElements: totalItems,
    totalPages: totalPages,
  },
  error: null,
});
```
