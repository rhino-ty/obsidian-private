---
sticker: emoji//2b50
tags:
  - network
  - graphql
  - RESTfulAPI
aliases:
  - GraphQL vs RESTful API
---
"GraphQL이 뭔가요? RESTful API 와 비교해 장단점은 무엇인가요?"

### GraphQL 정의
- Facebook에서 개발한 API 쿼리 언어 및 런타임
- 클라이언트가 필요한 데이터를 정확하게 요청 가능
- 단일 엔드포인트에서 여러 리소스 처리

### 주요 특징 
#### 선언적 데이터 요청
- 클라이언트가 스키마를 통해 원하는 데이터 구조 정의
- 필요한 필드만 선택적으로 요청 가능
- Over/Under-fetching 문제 해결

#### 강력한 타입 시스템
- 스키마 기반의 타입 시스템 제공
- 런타임 이전에 타입 검증 가능
- API 문서 자동 생성

### REST와의 비교

#### GraphQL 장점
- 필요한 데이터만 정확히 요청 (Over-fetching 방지)
- 여러 리소스를 단일 요청으로 처리 (Under-fetching 방지)
- 강력한 타입 시스템으로 안정성 확보
- 실시간 업데이트(Subscriptions) 지원
- API 버저닝 불필요

#### GraphQL 단점
- 학습 곡선이 높음
- 캐싱 구현이 복잡
- 파일 업로드 처리가 번거로움
- 단순한 API에서는 오버엔지니어링 될 수 있음

#### REST 장점
- 단순하고 직관적인 구조
- HTTP 캐싱 용이
- 브라우저에서 바로 테스트 가능
- 파일 전송 처리 용이
- 더 폭넓은 생태계

#### REST 단점
- Over-fetching / Under-fetching 문제
- 엔드포인트 관리 복잡성
- 필요한 데이터만 선택적 요청 어려움
- API 버저닝 필요

### 사용 케이스
#### GraphQL 적합
- 복잡한 데이터 요구사항
- 다양한 프론트엔드 클라이언트
- 빈번한 API 변경
- 모바일 앱 (데이터 효율성 중요)

#### REST 적합
- 단순한 CRUD 작업
- 캐싱이 중요한 경우
- 파일 업로드가 많은 경우
- 공개 API 제공