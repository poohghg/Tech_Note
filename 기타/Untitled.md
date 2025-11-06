## 사용한 기술 스택

|**분류**|**기술 스택**|**설명**|
|---|---|---|
|**프레임워크**|**Next.js 15 (App Router)**|서버 컴포넌트(Server Component)를 적극 활용하여 데이터 페칭 및 초기 렌더링 성능 최적화.|
|**언어/타입**|**TypeScript**|정적 타입 시스템을 도입하여 코드 안정성 및 개발 생산성 확보.|
|**상태 관리**|**Zustand**|가볍고 유연한 클라이언트 상태 관리 (즐겨찾기, 검색어, 정렬 상태).|
|**스타일링**|**Tailwind CSS**|빠르고 반응성이 뛰어난 유틸리티 기반 스타일링.|
|**패키지 관리**|**pnpm**|효율적인 의존성 관리 및 설치 속도 향상.|

---

## 아키텍처 및 설계 원칙

본 프로젝트는 요구사항에서 중요하게 언급된 **구조 설계**에 초점을 맞추어 다음 두 가지 핵심 아키텍처를 결합했습니다.

### 1. FSD (Feature-Sliced Design) 기반 컴포넌트 구성

React 컴포넌트를 **`entities`**, **`features`**, **`widgets`** 등 계층(Layer)과 슬라이스(Slice)로 분리하여 **의존성 규칙(Layer Isolation)**을 준수했습니다.

- **`entities/coin`**: 핵심 도메인 데이터(Coin)의 모델, DTO, API, 리포지토리 등의 클린 아키텍처 요소 포함.
    
- **`features/coin`**: 특정 비즈니스 로직(검색, 정렬, 즐겨찾기 상태 관리)을 캡슐화한 훅 및 스토어 포함.
    
- **`widgets/CoinListTable`**: `features`와 `entities`를 조합하여 테이블 UI를 구성하는 복합 컴포넌트.
    

### 2. 클린 아키텍처(Clean Architecture) 차용

**`entities/coin`** 내부에서 데이터 계층을 세분화하여 **관심사 분리(Separation of Concerns)**를 구현했습니다.

- **`api/service.ts`**: 외부 데이터(CoinGecko 등)와의 통신 책임 및 도메인 로직 적용.
    
- **`api/repository.ts`**: 외부 데이터 통신 및 도메인 객체로 변환하는 책임(매핑) 및 데이터 접근 추상화.
    
- **`model/domain/BaseCoin.ts`**: 순수한 비즈니스 로직 및 도메인 규칙 포함.
    
- **DTO 및 Type 분리**: 외부 데이터 규격(DTO)과 내부 도메인 규격(Type)을 분리하여 외부 변화에 유연하게 대응합니다.
    

---

## 구현된 주요 요소 설명

### 1. 효율적인 서버 컴포넌트 활용 및 SSR 최적화

- **Server Component**: 코인 목록 페칭을 포함한 대부분의 데이터 로딩을 서버 컴포넌트(`CoinListFetcher`)에서 처리하여 초기 로딩 속도와 SEO를 최적화했습니다.
    
- **동적 임포트 (Dynamic Import)**: 사용자의 즐겨찾기 상태(`useFavoriteCoinStore`)는 `localStorage`와 연동되는 클라이언트 전용 상태입니다. 이 상태를 사용하는 **`CoinTableRow`**와 **`useFavoriteCoinStore`**는 `next/dynamic`을 통해 클라이언트에서만 렌더링되도록 처리하여, SSR 시 발생할 수 있는 데이터 불일치(Hydration Mismatch) 문제를 방지했습니다.
    

### 2. UX/성능 최적화

- **가상 스크롤 (Virtual Scrolling)**: **만 개 이상의 데이터**가 로드될 수 있는 상황을 대비하여 가상 스크롤 라이브러리(미구현 시, 구현 계획 명시)를 도입할 수 있도록 설계했습니다. 현재는 성능 저하 없이 렌더링하도록 일반 테이블로 구성하였으며, 데이터가 늘어날 경우 즉시 적용 가능합니다.
    
- **검색 디바운스 (Search Debounce)**: 사용자가 검색어를 입력할 때마다 API 호출이나 상태 변경이 발생하는 것을 막기 위해 디바운스(Debounce) 로직을 적용했습니다.
    

### 3. 디자인 컴포넌트

- **SwitchCase 컴포넌트**: 복잡한 조건부 렌더링(예: `sortedCoins.length === 0`일 때 'No coins found' 메시지 표시)을 선언적으로 처리하기 위해 커스텀 `SwitchCase` 유틸리티 컴포넌트를 구현하여 가독성을 높였습니다.    
- Tabs 컴포넌트: 즐겨찾기/전체 코인 목록 전환을 위한 탭 UI를 구현하여 사용자 경험을 향상시켰습니다.

---
##  AI 활용 여부

- **Gemini/ChatGPT**: 특정 설계 패턴(FSD, 클린 아키텍처)의 개념을 명확히 하고, . **최종 코드 작성 및 구조 결정은 전적으로 직접 진행했습니다.**
    

---

## 📦 프로젝트 실행 방법

Bash

# 1. 의존성 설치  
pnpm install  
​  
# 2. 개발 서버 실행  
pnpm dev  
​  
# 3. 브라우저에서 접속  
# http://localhost:3000  
```<ctrl63>