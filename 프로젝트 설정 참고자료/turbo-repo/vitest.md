#### test: turbo run test 

```
루트
 └─ turbo가 각 패키지를 병렬로 실행
     ├─ apps/web        → vitest run (자체 vitest.config)
     └─ packages/ui     → vitest run (자체 vitest.config)
```

특징
- 실행 단위 :각 패키지 내부의 test 스크립트 개별 실행
- 병렬 실행:  패키지들을 동시에 실행
- 캐싱: 변경 없으면 이전 결과 재사용 (outputs: coverage/**)
- 의존성 순서:@repo/vitest-config#build 먼저 실행 후 테스트
- 커버리지 위치:각 패키지 폴더 안 (apps/web/coverage, packages/ui/coverage)
 
#### test:projects: vitest run — 단일 프로세스 (루트 vitest)

```
루트
 └─ 루트 vitest.config.ts 하나로 모든 프로젝트를 단일 프로세스에서 실행
     ├─ projects[0]: apps/web
     └─ projects[1]: packages/ui
```

특징
- 실행 단위: 루트 vitest.config.ts의 projects 배열로 통합 실행
- 병렬 실행: vitest 내부 워커로 병렬 처리
- 캐싱: Turborepo 캐시 없음
- 의존성 순서: @repo/vitest-config 빌드 여부 직접 확인 안 함
- 커버리지 위치: 루트 기준 한 곳으로 통합 가능
- 결과 출력: 단일 리포트로 통합되어 보기 편함
  
언제 어떤 걸 쓰나?

> CI / 정식 빌드   → turbo run test    (캐싱 + 의존성 순서 보장)
> 로컬 빠른 확인   → vitest run        (단순, 통합 리포트)
>
 핵심 차이는 "Turborepo가 각 패키지를 독립적으로 실행하느냐" vs "Vitest가 모든 패키지를 하나의 프로세스로 묶어 실행하느냐" 이다.