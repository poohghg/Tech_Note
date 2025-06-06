> 참조
> 
> https://testing-library.com/docs/
> https://github.com/practical-fe-testing/test-example-shopping-mall
### # 테스트란 ?

애플리케이션의 품질과 안정성을 높이기 위해 사전에 결함을 찾아내고 수정하기 위한 행위이다.
- 주로 특정 모듈이 사양에 잘 동작하는지 자동화된 테스트로 검증한다.  
- 효과
	- 좋은 설계에 대한 사고를 도와준다.
		- 결합도가 높을 경우 테스트 복잡도는 향상된다.
		- 이는 특정 기능만 따로 검증하기 어려워진다.
			- 복잡한 구조로 필요한 테스트가 누락
			- 결합도가 높기 때문에 여러 테스트 코드를 계속 수정해야함
			- 통합 테스트를 잘 작성하기 위해서는 좋은 설계가 기반이 되어야 한다.
	- 테스트 코드를 기반으로 빠르고 안정적이게 리팩토링을 할 수 있다.
		- 리팩토링은 결과의 변경없이 코드의 구조를 재조정하는 행위이다.
			- 문제가 발생했을 때 범위가 커 발견하기 어렵다
			- 개발자는 안정성있게 리팩토링을 할 수 있다.
	- 좋은 테스트 코드는 애플리케이션의 이해를 돕는 문서가 된다.
		- 앱의 시나리오를 파악할 수 있다
- 올바른 테스트 작성을 위한 규칙
	- 인터페이스를 기준으로 테스트를 작성하자.
		- 인터페이스의 의미는 서로 다른 클래스 또는 모듈이 상호작용하는 시스템이다.
		- 캡슐화된 부분만 테스트 하는게 좋다.
			- 내부 구현에 대한 테스트는 캡슐화를 위반 이는 깨지기 쉬운 테스트이다.
			- 내부 구현과 종속성이 없으면 캡슐화에 위반되지 않는다.
			- 어떤 행위를 하는지 명확해진다.
			- 테스트를 위한 불필요한 주석이나 설명이 필요 없다.
	- 커버리지 보다는 의미있는 테스트인지 고민하는게 좋다.
		- 커버리지를 생각하다보면 큰 유지 보수 비용이 발생한다.
		- 의미있고 중요한 부분에 대한 테스트가 더 좋다.
			- 로직 처리가 없는 UI의 경우 검증하지 않는다.
			- 해당 검증은 스토리북과 같은 도구를 통해 검증한다.
	- 테스트 코드도 유지 보수의 대상으로 가독성을 높여야한다.
		- 테스트 하고자 하는 내용을 명확하게 적는게 중요하다.
		- 하나의 테스트에서는 가급적 하나의 동작만 검증하는게 중요하다.
	- 모든 테스트는 독립적으로 실행되어야 한다.
- 테스팅 방식
	- 유저가 실제 사용하는 방식으로 테스트 해라
		- 내부 구현에 초점을 맞춤 테스트가 아니다.
			- React 컴포넌트의 state같은 내부 데이터를 직접 조작하는것보다 사용자가 실제로 하는 방식으로 테스트 하는게 좋다. 
		- 코드 작성 방식은 변경될 수 있으며, 소프트웨어가 사양에 따라 변화한다.
	- 요소를 접근성 표시로 찾는것이다.
- React Testing Library
	- 테스트를 위한 시뮬레이터된 DOM을 제공한다.
	- 시뮬레이터된 DOM을 조작하고, 평가하는 방식을 제공한다.
		- 테스트를 위한 가상 DOM을 생성한다.
	- DOM과 상호작용하기 위한 유틸리티 함수를 제공한다.
	- 브라우저 없이도 테스트 가능하다.
- Jest/Vitest
	- 테스트를 위한 테스트 러너이다.
		- 테스트를 찾고
		- 테스트를 실행하고
		- 테스트가 통과 했는지 여부를 결정한다.
	- Jest/Vitest 간의 구문 차이가 거의 없다.
	- 차이점은 사용하는 파일을 지정하는 설정과 구성 구문이다.
- Vitest
	- Jest 보다 더 빠르다
	- vite 환경에서 호환성이 더 좋다.
	- Jest와의 약간의 Syntex 차이가 있다.
- TDD(Test-Driven Development)
	- 코드를 작성하기전에 테스트틑 작성하고 테스트에 통과하도록 코드를 작성하는 것이다.
	- 레드-그린 테스트
		- 코드를 작성하기전에 테스트에 실패하는 레드 테스트를 먼저 실행한다.
		- 코드 작성 후에는 통과하는 테스트로 그린 테스트를 확인한다.
		- 테스트가 통과하면 리팩토링을 진행한다.
	- 코딩 프로세스의 일부이다.
	- 더 효율적이다
	- 개발하면서 모든 테스트를 작성해두면 변경에 자유롭다.
- 테스트 타입
	- 유닛(단위) 테스트
		- 단일 한수 또는 단일 컴포넌트 등과 같이 앱에서 테스트 가능한 가장 작은 소프트웨어를 실행해 예상대로 동작하는지 확인하는 테스트 이다.
			- 공통 컴포넌트, 리액트 훅, 유틸 함수 등
			- 결괏값 또는 상태(UI), 행위를 검증
		- 독립적인 코드의 조각을 테스트 한다.
		- 테스트를 최대한 격리하여 실행한다.
			- 단위 테스트에서 외부 모듈에 대한 검증은 분리한다. (외부 의존성의 격리)
			- 모듈의 특정 기능을 제대로 호출되는지만 검증한다.
		- 격리된 유닛에서 실패를 쉽고 정확하게 파악할 수 있다.
		- 사용자가 소프트웨어와 상호작용하는 방식과는 덜 밀접하게 연결되어 있다.
		- 리팩토링은 동작을 변경하지 않고 소프트웨어 작성 방식을 변경하는것
		- 보통 유닛 테스트로 소프트웨어가 어떻게 작성 되었는지 테스트한다.
		- 공통 컴포넌트는 단위 테스트에 적합하다.
			- 다른 컴포넌트와의 상호작용이 없으며
			- 컴포넌트의 비지니스 로직을 기능 단위로 나눠 검증하기 좋다.
			- 범용적으로 재사용되기 때문에 검증을 통해 안정성을 높여야한다.
		- 단점 
			- 여러 모듈이 도합되었을때 발생하는 이슈는 찾을 수 없다.
			- 앱의 전반적인 기능이 비지니스 요구사항에 맞게 동작하는지 보장할 수 없다.
	- 통합 테스트
		- 여러 유닛이 함께 작동하는 방식을 테스트한다.
			- 주로 API, 상태 관리 스토어, 리액트 컨텍스트 등 다양한 요소들이 결합된 컴포넌트가 특정 비지니스 로직을 올바르게 수행하는지 검증한다.
				- ​이는 유닛(컴포넌트)간의 상호작용을 테스트한다.
			- 주로, 컴포넌트간의 상호작용 API 호출 및 상태 변경에 따른 UI 변경 사항 검증이다.
			- 비지니스 로직 기준으로 여러 컴포넌트들 간의 상호 작용을 한번에 검증할 수 있다.
		- 항목
			- 특정 상태를 기준으로 동작하는 컴포넌트 조합
			- API와 함께 상호작용 하는 컴포넌트 조합
			- 단순 UI 렌더링 및 간단한 로직을 실행하는 컴포넌트까지 한번에 효율적으로 검증 가능하다.
			- 상태나 데이터를 관리하는 특정 컴포넌트를 기준으로 하위 컴포넌트가 제대로 렌더링 되는지 검증한다.
				- 데이터를 관리하는 로직이 산재 -> 통합 테스트 작성이 어렵다
				- 앱의 상태를 어디서 어떻게 관리하고 변경할지 구조적인 설계가 중요하다.
					- 통합 테스트를 통해 상태 관리의 효율성을 검증할 수도 있다.
				- 좋은 설계를 위해 매개체가 된다.
		- 장점
			- 여러 개의 모듈이 동시에 상호 작용 하는 것을 테스트 하기 때문에 단위 테스트에 비해 모킹의 비중이 적으며, 모듈 간의 발생하는 에러를 검증할 수 있다.
			- 실제 앱이 동작하는 비지니스 로직에 가깝게 기능을 검증할 수 있다.
			- 하위 모듈의 단위 테스트에서 검증할수 있는 부분까지 한 번에 효율적으로 검증할 수 있다.
		- 비지니스 로직을 기준으로 통합 테스트를 작성하면
			- 비지니스 로직(도메인)이란?
				- 프로그램의 핵심 기능을 수행하는 코드
				- 사용자가 원하는 결과를 얻기 위한 계산,처리,의사 결정을 수행하는 코드이다.
			- 서비스의 핵심 비지니스 로직을 독립적인 기능 관점에서 효율적으로 검증할 수 있다.
			- 테스트를 명세 자체로 볼 수 있어 앱을 이해하는데 큰 도움을 받을 수 있다.
			- 불필요한 단위 테스트를 줄여 유지 보수 측면에서도 좋다.
		- 비지니스 로직을 기준으로 통합 테스트를 진행 할 때 고려사항
			- 가능한 한 모킹을 하지 않고 실제와 유사하게 검증한다.
			- 비지니스 로직을 처리하는 상태 관리나 API호출은 상위 컴포넌트로 응집한다.
			- 변경 가능성을 고려해 여러 도메인의 기능이 조합된 비지니스 로직은 나눠서 검증한다.
	- 기능 테스트
		- 특정 기능을 테스트한다.
			- 코드가 아닌 동작을 테스트 한다.
			- 예를 들어 데이터를 폼에 입력하고 제출하는 행위를 테스트 하는 것이다. 
		- 주로 유저 플로우와 연관된 모든 단위를 포함한다.
			- 사용자가 소프트웨어와 상호 작용하는 방식과 밀접하게 연결되어 있다.
			- 테스트에 통과하면 사용자에게 문제가 발생할 가능성이 적다.
			- 견고한 테스트가 가능하다.
		- 하지만 실패한 테스트의 경우 디버깅하기 어려운 단점이 있다.
			- 어떠한 부분의 코드가 테스트 실패의 원인인지 정확히 알 수 없다.
	- E2E
		- 실제 브라우저에서 환경에 통합테스트를 진행한다.
- 테스트 작성 패턴
	- Arrange: 테스트를 위한 환경 만들기: 컴포넌트 렌더링
	- Act: 테스트할 동작 발생: 테스트할 동작 시뮬레이션(클릭, 타이핑, 메서드 호출 등)
	- Assert: 올바른 동작이 실행 되었는지 또는 변경사항 검증하기

### # 테스트 프레임워크

테스트 러너, 어설션 라이브러리, 플러그인 등 테스트 실행 및 검증에 필요한 환경을 제공하는 도구들
- E2E 테스트를 제외하고, 대부분 node.js 환경에서 구동된다.

### # vitest

- https://vitest.dev/
- 별다른 설정 없이 vite 기반에서 테스트 구종
- 추가 설정 없이 ESM, TypeScript, JSX 사용 가능하다.
- Jest 호환 API 및 가이드 제공
- jsDOM
	- 웹 사이트 표준을 자바스크립트로 Node.js 환경에 구축한 도구이다.
	- 일반적으로 Node.js에서 브라우저 환경 테스트시 설정이 복잡하고, 시간 소요가 많다.
	- 브라우저 구현과는 다른 부분이 있다.

#### it 함수

- 테스트의 실행 단위로서 테스트 디스크립션, 기대 결과에 대한 코드를 작성한다
- it 함수는 test 함수의 alias로서 동일한 열학을 수행한다.
- 기대결과와 실제 결과가 같다면 테스트는 성공적으로 통과한다.

#### 단언(assertion)

- 테스트가 통과하기 위한 조건을 기술하여 검증을 실행한다.
#### 매처

- 기대 결과를 검증하기 위해 사용되는 일종의 API 집합이다.
- vitest에서는 다양한 기본 매처를 제공하며, 이를 확장하여 단언을 실행할 수 있다.
- https://vitest.dev/api/expect.html
- https://github.com/testing-library/jest-dom?tab=readme-ov-file#custom-matchers
#### setup 과 teardown
- setup
	- 테스트를 실행하기 전 수행해야 하는 작업
		- 테스트 컨텍스마다 반복되는 작업을 정의한다.
		- 테스트가 독립적으로 실행되지 않는 변수 및 조건을 주는건 좋지않다. -> 테스트가 깨질 수 있다.
	- https://vitest.dev/api/#setup-and-teardown
	- `beforEach` : 스코프 및 파일 내에 각각의 테스트가 실행되기 전 실행된다.
	- `beforAll` :  스코프 및 파일 내에 한번만 호출된다. `beforEach` 보다 우선순위가 높게 실행된다.
- teardown
	- 테스트를 실행한 뒤 수행해야 하는 작업
	- 보통 테스트에 의해 생성된 상태를 초기화하는 경우에 사용된다.
	- `afterEach` : 스코프 및 파일 내에 각각의 테스트가 실행된 후 실행된다.
	- `afterAll` :  스코프 및 파일 내에 한번만 호출된다. 모든 테스트가 완료된 후 실행된다.
- setup 과 teardown 를 통해 테스트 실행 전, 후 실행되어야 할 반복 작업을 관리할 수 있다.
- 테스트 별로 별도의 스코프로 동작하기 때문에 독립적인 테스트를 구성하는데 도움을 받을 수 있다.
- setup 과 teardown에서 전역 변수를 사용한 조건 처리는 독립성을 보장하지 못하고, 신뢰성이 낮아지므로 지양해야한다.

---
### # Queries

쿼리는 Testing Library에서 페이지의 요소를 찾는 데 제공하는 방법이다. [쿼리에는 여러 유형이](https://testing-library.com/docs/queries/about#types-of-queries) 있다 ("get", "find", "query").

#### 쿼리의 종류

- `getBy`: 쿼리에 대한 일치하는 노드를 반환하고, 일치하는 요소가 없거나 두 개 이상의 일치 항목이 있는 경우 오류를 발생시킨다.
- `queryBy`: 쿼리에 대한 일치하는 노드를 반환하고, 일치하는 요소가 없는 경우 `null`을 반환한다.
	- 두 개 이상의 일치 항목이 있는 오류를 발생시킨다.
	- `queryAllBy` : 일치하는 모든 노드를 반환한다.
- `findBy`: 주어진 쿼리와 일치하는 요소가 발견되면 해결된 프로미스를 반환한다.
	- 요소가 발견되지 않거나 1000ms의 기본 시간 초과 후에 두 개 이상의 일치 항목이 있는 경우 오류를 발생시킨다.

| 질의 유형        | 0개의 일치   | 1 매치  | >1개 일치  | 재시도(비동기/대기) |
| ------------ | -------- | ----- | ------- | :---------: |
| **단일 요소**    |          |       |         |             |
| `getBy...`   | 오류를 던지다  | 반환 요소 | 오류를 던지다 |     아니요     |
| `queryBy...` | 반품`null` | 반환 요소 | 오류를 던지다 |     아니요     |
| `findBy...`  | 오류를 던지다  | 반환 요소 | 오류를 던지다 |      예      |

###  # Testing Library

- 특정 프레임워크나 라이브러리에 종속되지 않는다.
- UI 컴포넌트를 사용자가 사용하는 방식으로 테스트 하는게 핵심 철학이다.
	- 테스트 라이브러리는 컴포넌트 내부 구현이나 상태에 직접 접근 하는것 아니라 DOM 노드를 쿼리 하고 사용자와 비슷한 방식으로 이벤트를 발생 시킨다.
	- 이는 내부 구현과 종속성이 없으면 캡슐화에 위반되지 않는다.
- DOM, 이벤트 인터페이스 기반으로 요소를 검증을 하고, 다양한 동작을 시뮬레이션 할 수 있다.
	- 요소 조회를 위한 퀴리는 다양하며, 우선 순위가 존재한다.
- 사용자 중심의 검증을 위해 DOM 이벤트 인터페이스 기반으로 테스트를 진행한다.

#### spy 함수

- 테스트 코드에서 특정 함수가 호출되었는지, 함수의 인자로 어떤 것이 넘어왔는지 어떤 값을 반환하는지 확인
- 콜백 함수나 이벤트 핸들러가 올바르게 호출 되었는지 검증 할 수 있다.

#### 모킹

-  실제 모듈,객체와 동일한 동작을 하도록 만든 모의 모듈 객체(Mock)로 실제 객체를 대체하는 것이다.
- `vi.mock()` 함수를 사용하여 모킹을 진행한다.
	- 모킹된 모듈의 구현을 제공하거나, 특정 함수의 호출 히스토리를 추적할 수 있다.
	- 모킹된 모듈은 실제 모듈과 동일한 인터페이스를 제공한다.
- 외부 모듈의 검증은 완전히 배제하고, 대상이 되는 컴포넌트의 테스트만 독립적으로 작성할 수 있다.
- 테스트 실행 뒤에는 명시적으로 모킹 초기화를 진행해야 한다.
	- vitest의 resetMocks(), clearMocks(), restoreMocks() 함수를 사용하여 초기화를 진행한다.
	- `vi.clearAllMocks()`
		- 모킹된 모의 객체 호출에 대한 히스토리 초기화
		- 모킹된 모듈의 구현을 초기화하지는 않는다 -> 모킹된 상태로 유지됨
		- 반면, 모킹 히스토리가 계속 쌓임 -> 다른 테스트에 영향을 줄 수 있다.
	- `vi.resetMocks()`
	    - 모킹된 모듈의 구현을 초기화한다.
	- 참고 : https://vitest.dev/api/mock.html
- 모킹의 단점
	- 모킹은 테스트를 독립적으로 분리하여 효과적으로 검증할 수 있게 도와주지만, 지나치게 많은 모킹은 테스트 신뢰성을 저하시키며 변경에 취약하다.

#### 리액트 훅

- 리액트 렌더링 메커니즘을 따른 순수 함수 이기 때문에 독립적인 단위 테스트를 작성하기 적합하다.
- React Testing Library는 리액트 훅을 테스트하기 위한 유틸리티 함수(renderHook API)를 제공한다.

#### act

- `act()`함수는 상호 작용(렌더링, 이펙트 등)을 함께 그룹화하고 실행해 렌더링과 업데이트가 실제 앱이 동작하는 것과 유사한 방식으로 동작함
- 즉 테스트 환경에서 act를 사용하면 가상의 돔(jsdom)에 제대로 반영되었다는 가정하에 테스트가 가능해짐
- ==컴포넌트를 렌더링한 뒤 업데이트 하는 코드의 결과를 검증하고자 할 때 사용한다.==
- `@testing-library/react, @testing-library/user-envet` 컴포넌트 렌더링, 이벤트 핸들러 실행
	- `render()`, `user event` 모듈을 사용하지 않을 경우 act 함수를 사용해야한다. 
	- 내부적으로 act 함수를 호출하여 상태를 반영한다.

#### 타이머 테스트

- 타이머를 원하는대로 제어하기 위해서 타이머 모킹
	- `useFakeTimers()` 함수를 사용하여 타이머를 모킹한다.
	- `advanceTimersByTime()` 함수를 사용하여 타이머를 직접 제어할 수 있다.
	- `setSystemTime()` 함수를 통해 테스트가 실행되는 시간을 변경할 수 있다.
- 타이머 복원
	- 테스트가 실행 된 후 다른 테스트에 영향을 주지 않기 위해 타이머 모킹을 해제해야한다.
	- `useRealTimers()` 함수를 사용하여 타이머를 복원한다.

``` ts

import { useFakeTimers, useRealTimers } from 'vitest'

describe('타이머 테스트', () => {
  let clock

  beforeEach(() => {
    clock = useFakeTimers()
  })

  afterEach(() => {
    useRealTimers()
  })

  it('타이머 테스트', () => {
    const callback = vi.fn()
    setTimeout(callback, 1000)
    clock.advanceTimersByTime(1000)
    expect(callback).toHaveBeenCalled()
  })
})

```
#### userEvent

- 실제 사용자가 사용하는 것처럼 이벤트가 연쇄적으로 발생
	- 테스트의 신뢰성을 향상시킨다.
- `disabled` 속성이 있는 요소를 클릭하거나, `readOnly` 속성이 있는 요소를 수정하려고 시도하면 에러가 발생한다.
	- 이는 사용자가 실제로 할 수 없는 행동을 시도하는 것을 방지하기 위함이다.
- `userEvent` 모듈은 `@testing-library/user-event` 패키지를 사용하여 제공된다.
- `fireEvent` 함수를 사용하여 이벤트를 발생시키는 것과 다르게, `userEvent` 모듈은 사용자가 요소와 상호작용하는 것처럼 이벤트를 발생시킨다.

#### fireEvnet

- 특정 요소에서 원하는 이벤트만 쉽게 발생시킬 수 있다.
	- DOM 이벤트만 발생시킨다.
- fireEvnet의 클릭은 단순하게 클릭 이벤트만 발생 시킨다.
	- 실제 브라우저에서는 사용자가 요소 클릭시 pointerdown, pointerup, click 등의 이벤트가 연쇄적으로 발생한다.
	- 유저 이벤트의 경우 위 상황이 고려되어 있다.
- `userEvent` 로 불가능한 테스트의 경우 `fireEvent`를 사용한다.
	- `scroll`, `focus`, `blur` 등의 이벤트는 `fireEvent`를 사용한다.`

#### 상태 관리와 통합 테스트

- 원하는 상태로 통합 테스트를 하기 위해 zustand 모킹이 필요하다.
- 웹의 전역 상태를 모킹해 테스트 전역 값을 변경하고 초기화해야 한다.
	- `__mocks__` 하위에 모킹 파일을 생성한다. ->  자동 모킹을 적용해 스토어를 초기화한다.
- state,api에 대한 제어 코드를 통합 테스트 대상 컴포넌트로 응집 -> 유지보수성 향상, 테스트의 단위를 나뉘기 좋음
	- 상태 관리 코드 산재 -> 로직 파악 및 테스트 파악이 어렵다.

#### API 호출 테스트

- 통합 테스트에서 API를 호출하는 컴포넌트를 시뮬레이션 하기 위해서는 프로젝트에서 사용하는 Tanstack-query 설정과 API에 대한 모킹이 필요하다.
- Tanstack-query
	- API 호출에 따른 로딩, 에러 상태 처리 및 페이지네이션 캐싱 등의 편의성을 위해 사용
- MSW
	- Node.js, 브라우저 환경을 위한 API 모킹 라이브러리
	- 브라우저는 서비스 워커를 사용, Node.js는 http 요청을 가로채는 인턱셉터를 사용
	- setup 과 teardown을 통해 API 모킹을 초기화하고 제거한다.

```ts

import { setupServer } from 'msw/node'

const server = setupServer(
  rest.get('https://api.example.com/posts', (req, res, ctx) => {
    return res(ctx.json({ title: 'test' }))
  })
)

// 테스트 실행 전 서버를 시작한다.
beforeAll(() => server.listen())

// 테스트 실행 후 서버를 초기화한다.
afterEach(() => server.resetHandlers())

// 테스트 실행 후 서버를 종료한다.
afterAll(() => server.close())
```

- server.use() 
	- API에 대해 상황에 따라 다른 모킹을 하기 위해 사용한다.
	- Teardown(afterEach)에서 `server.resetHandlers()`를 사용하여 동적인 API 모킹 초기화
	- 일관된 모킹 데이터로 안정성 있는 테스트 작성

#### 통합 테스트의 한계

- 전체 워크플로우를 검증하는 경우 지나치게 모킹에 의존하게 된다.
- 모킹 사 시나리오를 누락하거나, 잘못된 모킹이 있을수 있어 신뢰가 낮아짐
- 모킹 유지 비용도 증가
- E2E 테스트를 사용하면 앱을 구동시켜 모킹에 의존하지 않고 워크플로우를 검증 할 수 있음
- 개발 단계에서 단위/통합 테스트로 도메인 별 비지니스 로직을 검증하고 앱의 완성 단계에 전체 워크플로우를 E22E 테스트로 검증하는 것이 좋다.







