> 참조
> 
> [React 파이버 아키텍처 분석](https://d2.naver.com/helloworld/2690975)
> [Lin Clark - A Cartoon Intro to Fiber - React Conf 2017](https://www.youtube.com/watch?v=ZCuYPiUIONs&t=1334s)


React는 16버전부터 파이버라 불리는 새로운 코어 아키텍처를 채택했다. 이는 기존의 스택 기반 알고리즘을 완전히 새롭게 작성한 것이며 리액트18.2 버전까지 지속적으로 업데이트 및 리팩터링되고 있다.

## # 복습

- JSX 선언된 표현식은 React 엘리먼트 객체로 치환된다.
	- React 엘리먼트는 객체이다.
- JSX 표현식은 createElement 함수(_JSX) 함수를 통해 React 엘리먼트로 치환된다.
	- 참조 [[createElement]]
- 변경된 부분만 업데이트 하기
	- React DOM은 업데이트 시 변경된 부분만 native DOM에 반영한다.
	- 이는 내부 diff 알고리즘을 통해 변경된 부분만 업데이트 후 실제 DOM에 반영한다.
		- 참조 [[diffing]]
	- 이전 React Element와 타입,키,Props가 다르면 업데이트가 진행된다.
		- 부모 내 자식의 형제 중 key가 동일할때는 이전 React Element를 사용한다.
- 재조정(Reconciliation)
	- 복잡도가 O(n)인 휴리스틱 알고리즘 사용하여 변경된 리액트 엘리멘트 생성한다.
	- 타입이 다르면 이전 트리를 버리고 완전히 새로운 트리를 구축한다.
		- 루트 엘리먼트의 하위 컴포넌트 모두 언마운트 된다.
	- 이전 엘레민트와 키가 다르다면 컴포넌트를 언마운트 한다.
	- 타입 및 키과 동일하고 props가 변경되었다면 변경된 부분만 업데이트 한다.
- Ref 와 DOM
	- 포커스, 텍스트 선택 영역, 혹은 미디어의 재생을 관리할 때
	- 애니메이션을 직접 실행시킬 때
	- 서드 파티 DOM 라이브러리를 React와 같이 사용할 때
		- 셋 모두 React에서 컨트롤 가능한 영역인 태그 이름, 어트리뷰트, 자식이 아니라 native DOM API 등을 직접 컨트롤해야 하는 경우임을 알 수 있다. 반대로 말하자면 React가 제어 가능한 부분은 선언형 API를 통해 조작하라는 것인데 왜 이를 권고할까? React는 JSX 표현식이 치환된 React 엘리먼트의 `props`, `states`를 통해 React DOM을 갱신하므로, Ref를 통해 native DOM을 직접 조작하면 이 변경 사항을 React가 유지할 수 없을 것이라 추정해볼 수 있다.

### #Fiber

- React는 컴포넌트트리에 대한 추가 정보를 포함하기 위해 “fibers”라는 내부 객체를 사용한다.
- 브라우저에서 일반적인 UI 업데이트 주기는 초당 60회이다. 약 16ms가 소요된다. 잦은 업데이트로 인해 코드가 연속적으로 16ms 이상의 시간을 소비하며 실행된다면, 화면은 갱신 되지 못하고 끊기는 현상이 발생한다.
- React 파이버 구조에서는 UI 갱신 작업은 작은 단위로 나누어 내부적으로 스케줄링 함으로써 React 사용자가 신경쓰지 않더라도 대규모 UI 갱신에도 16ms를 초과하지 않도록 작업한다.

### 동시성 스케줄링

React 개발팀은 기존의 알고리즘으로는 대규모 앱 컴포넌트를 빠르게 업데이트 하는것이 어렵다고 판단했다.이에 파이버에 동시성이라는 개념을 도입했다. DOM 업데이트, 렌더링 로직을 작업 단위로 구분하고 이를 비동기로 실행하여 최대 실행 시간이 16ms가 넘지 않도록 제어한다.

파이버는 단순히 작업을 청크로 분리하여 실행 시간만을 관리하는 것이 아니라 작업의 유형에 따라 우선순위를 부여하고, 기존에 수행중인 작업보다 더 우선순위가 높은 작업이 인입 될 경우 작업을 일시 중단하고 인입된 작업을 처리 후 기존 작업을 실행한다.

### React의 동작 단계

1. Render 단계: JSX 선언 또는 React.createElement()를 통해 JSX를 일반 객체인 ReactElemen를 생성한다.
	1. ReactElement트리 생성
2. Reconcile 단계: 이전에 렌더링된 ReactElement트리와 새로 렌더링할 ReactElement트리를 비교하여 변경점을 적용한다.
3. Commit 단계: 새로운 DOM 엘리먼트를 브라우저 뷰에 커밋한다.
4. Update 단계: props, state 변경 시 해당 컴포넌트와 하위 컴포넌트에 대해 위 과정을 반복한다.

#### Render 단계

 `ReatRootDOM.render()`는 JSX 등으로 작성된 코드가 실제 DOM으로 커밋되기까지 모든 과정을 의미한다.
 - JSX 또는 React.createElement()로 작성된 코드를 ReactElement로 변경하는 작업이다.

#### Reconcile 단계

재조정자는 FiberNode를 하나의 작업 단위(`unitOfWork`)로 취급한다. 즉, FiberNode는 자체로 렌더링에 필요한 정보를 담고 있는 객체이자 재조정 작업 단위인 것이다.

 `Reconcile`이라는 작업은 좁은 의미로 FiberNode를 생성하고 기존 생성된 FiberNode를 보면 `alternate`, `child`, `memoizedProps`, `memoizedState` 모두 `null`이다. FiberNode만 생성했을 뿐 `SimpleComp()`는 호출하지 않았다.



