# ai-context
> 📢 해당 레포지토리는 AI 도구(MCP)를 "중앙 집중식"으로 관리하여, 컨텍스트 공유 비용을 줄이고 생산성을 높이는 것을 목표로 합니다.

회사 개발자들이 Claude Code, Cursor AI와 같은 도구를 일관되게 잘 활용할 수 있도록 돕기 위한 중앙 컨텍스트 저장소입니다.

## 목적

- 프로젝트와 독립적으로 관리되는 공통 컨텍스트 저장소
- AI 도구와의 협업 효율을 높이기 위한 참고 문서 구조 제공
- 팀 별, 계층 별로 활용 가능한 예시와 가이드 수록

## 디렉토리 구조

```
ai-context/
├── README.md
├── context/
│   ├── backend/
│   │   ├── domain-overview.md
│   │   ├── service-patterns.md
│   │   └── test-guidelines.md
│   ├── frontend/
│   │   ├── component-structure.md
│   │   └── naming-convention.md
│   ├── mobile/
│   │   └── network-layer.md
│   └── shared/
│       ├── business-terms.md
│       └── naming-style.md
├── prompts/
│   ├── refactoring.md
│   ├── test-generation.md
│   └── code-review.md
└── examples/
    ├── cursor-snippets.md
    └── claude-usage.md
```

## 활용 방법

### 1. 프로젝트에서의 활용(예시)
- 프로젝트 내 `.cloderc` 또는 Cursor 설정에서 `ai-context/`를 context path로 지정
- Claude Code 프롬프트 시, "다음 가이드를 참고해주세요: [경로]" 와 같이 명시적으로 언급
- 예: `context/backend/service-patterns.md` 에 기반해 리팩토링 요청

### 2. 문서 기여 가이드
- Pull Request로 기여
- 컨벤션 문서는 Markdown으로 작성
- 컨텍스트 구조 변경은 이슈로 먼저 제안

### 3. 프롬프트 작성 예시
```md
# prompts/test-generation.md

## 목적
서비스 단위 테스트 자동화를 위한 Claude용 프롬프트 예시

## 프롬프트
서비스 클래스의 메서드에 대해 단위 테스트 코드를 작성해주세요.
- 테스트 프레임워크: JUnit5
- 모킹: MockK
- Given-When-Then 형식 사용
- 테스트 대상 메서드:
```kotlin
fun getUserById(id: Long): User
```
```

## 관련 프로젝트 예시
- 루티 시뮬레이터
- 엔진 내부 툴
- 전처리 서버

---
