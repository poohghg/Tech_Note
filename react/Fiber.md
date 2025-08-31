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

- React는 ==컴포넌트 트리에 대한 추가 정보를 포함하기 위해 “Fiber”==라는 내부 객체를 사용한다.
- `Fiber`는 React 내부의 렌더링 엔진이다. 렌더링 작업을 쪼개고, 중단하고, 다시 시작하는 게 가능하다.
- 브라우저에서 일반적인 UI 업데이트 주기는 초당 60회이다. 약 16ms가 소요된다. 잦은 업데이트로 인해 코드가 연속적으로 16ms 이상의 시간을 소비하며 실행 된다면, 화면은 갱신 되지 못하고 끊기는 현상이 발생한다.
- React 파이버 구조에서는 UI 갱신 작업은 작은 단위로 나누어 내부적으로 스케줄링 함으로써 React 사용자가 신경쓰지 않더라도 대규모 UI 갱신에도 16ms를 초과하지 않도록 작업한다.
	- 작업을 스케줄링하고, 언제 그걸 처리할지 Fiber가 결정한다.

### 동시성 스케줄링

React 개발팀은 기존의 알고리즘으로는 대규모 앱 컴포넌트를 빠르게 업데이트 하는것이 어렵다고 판단했다.이에 React18에서 ==파이버에 동시성==이라는 개념을 도입했다. DOM 업데이트, 렌더링 로직을 작업 단위로 구분하고 이를 비동기로 실행하여 최대 실행 시간이 16ms가 넘지 않도록 제어한다.

파이버는 단순히 작업을 청크로 분리하여 실행 시간만을 관리하는 것이 아니라 작업의 유형에 따라 우선순위를 부여하고, 기존에 수행중인 작업보다 더 우선순위가 높은 작업이 인입 될 경우 작업을 일시 중단하고 인입된 작업을 처리 후 기존 작업을 실행한다.

### React의 동작 단계

![Pasted image 20250224184823.png](../img/Pasted%20image%2020250224184823.png)


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

- UI의 업데이트 된 상태를 반영하기 위해서, 리액트 엘리먼트의 current tree에 적용되어야하는 모든 변경사항들은, 재조정 과정 동안 reconciler에 의해 결정된다.

#### Scheduling

Scheduling은 동작 처리(work) 들이 수행 될때를 결정하는 역할을 한다. 여기서 work들이 수행 될 때를 결정하는 역할을 한다. 여기서 work란 렌더링 작업, 이벤트 핸들러 실행, 데이터 가져오기 등을 의미한다. 즉 수행되어야 할 모든 계산들을 의미한다. work는 보통 업데이트의 결과이다(상태 변화, 라이프사이클 함수, DOM내에서의 변화)

---
## Fiber의 구조

하나의 fiber는 작업의 단위를 나타낸다. 리액트 fiber는 더 맞춤화된 스택의 재구현이기 때문에, 하나의 단일 fiber를 가상의 스택 프레임이이라고 생각할 수 있다. 이는 스택 프레임과 일치하지만 ==리액트 컴포넌트의 인스턴스와 일대일 관계==를 유지한다.

- fiber의 구현체는 객체이다. 이는 리액트 정보를 포함하고 있는 간단한 자바스크립트 객체인것이다.
- 실제 DOM노드, 컴포넌트의 인스턴스에 대응되며 렌더링 작업을 관리하는데 필요한 정보를 담고 있다.
- fiber객체는 fiber 노드들간의 관계와 정보를 추적할 수 있도록 하는 특정한 프로퍼티를 가지고 있다.

#### Fiber의 이점

1. 작업 분할과 중단 가능
	- 이전에는 재조정 과정이 한번에 완료 되어야 했다. 하지만 Fiber는 작업을 작은 단위로 나누어 중단하고 다시 시작하는게 가능하다.
	- 이때 스케줄러가 사용된다.
2. 우선 순위 기반 처리
	- 이전에는 모든 업데이트 동일한 우선순위로 처리되었다. Fiber는 다양한 우선순위 레벨을 지원한다. 
3. 더 나아진 에러 처리
	- Fiber는 에러 바운더리를 통해 실패 처리가 가능하다.

```javascript
fiberNode{  
   stateNode,
   child,
   sibling,
   return,
   type,
   alternate,
   key,
   updateQueue,
   memoizedState,
   pendingProps,
   memoizedProps,
   tag,
   effectTag,
   nextEffect
}
```

- stateNode는 해당 fiber가 속해 있는 컴포넌트 인스턴스를 나타낸다.
- child와 sibling은 현재 fiber노드와 관계 되어있는 다른 fiber들을 가리킨다.
- return : return 속성은 현재의 fiber에서 처리를 완료 한후, 돌아오는 상위 fiber를 말한다. 이를 부모 fiber라고 말할수 있다. 만약 하나의 fiber가 여러개의 자식 fiber를 가진다면, 각각 자식 fiber들의 return 속성 값은 부모 fiber가 됩니다.  
-  type: 타입은 컴포넌트의 구성요소이다. class가 될수도, function이 될수도,DOM element 가 될수도 있다.
- key : key는 재조정 과정에서 해당 fiber가 변화나, 요소 추가, 삭제 같은 것들을 식별함으로써 재사용 될수 있는지 아닌지를 결정한다.
- alternate : fiber를 flush 한다는 것은, 렌더링 된 결과물을 화면에 출력한다는 것을 의미이다.  어느 시점에서든, 컴포넌트 인스턴스는 2개의 동일한 fiber를 가지고 있다. current Fiber와 work-in-progress Fiber이다. 
	- work-in-progress Fiber는 아직 작업이 완료되지 않은 fiber이다. 개념적으로 아직 리턴되지 않은 스택프레임이다.  
	- current fiber의 alternate는 work-in-progress이며, 그 반대로 work-in-progress의 alternate는 current fiber이다.  
	- fiber의 alternate는 cloneFiber라고 불리는 함수에 의해서 필요에 따라 생성된다.  항상 새로운 객체를 만들기보다, cloneFiber는 객체 할당을 최소화하여 만약 fiber의 alternate가 존재한다면 이를 재사용하려고 시도한다.
- updateQueue : 이 큐들은, 모든 상태와 DOM 업데이트들, 그리고 다른 효과(effect)들을 큐에 추가하여 관리한다.
- memoizedState : 이전 렌더링때의 상태값을 참조
- memoizedProps and pendingProps : 개념적으로, props는 함수의 인자로 생각할 수 있다.  fiber의 pendingProps는 실행의 시작 부분에서 설정되며, memoizedProps는 끝 부분에서 설정된다. 들어오는 pendingProps가 memoizedProps와 같을 때, 이는 Fiber의 이전 출력을 재사용할 수 있다는 신호이며, 불필요한 작업을 방지한다.
- tag : 이것은 fiber의 타입을 명시합니다. 예를 들어, 클래스 컴포넌트, 함수 컴포넌트, 호스트 포털 등이 있다. (tag 속성이 type속성보다 더 넓은 범위의 의미를 가지고 있다.)
- effectTag : 적용되어야 할 부수 효과(side-effect)에 대한 정보를 담고 있다.
- nextEffect : 이펙트 리스트 안에 있는 업데이트 되어야할 다음 노드를 가리킨다.

#### 알아두면 좋은점

- 리액트 fiber는 리액트 엘리먼트와 비슷하다.
- fiber는 리액트 엘리먼트로부터 만들어지고 type, key 등 같은 몇몇 속성등을 공유한다.
	- 하지만 엄밀이 말하면 fiber는 리액트 엘리먼트가 아니다. ==fiber는 렌더링 작업을 추적하고 관리하기 위한 자바스크립트== 객체이다.
- 리액트 엘리먼트는 매번 다시 만들어지지만, fiber는 가능한 최대로 재사용되어진다.

### Fiber의 동작

React가 DOM에 어떤것을 랜더링하기 전에, 각각의 fiber(작업단위)를 처리하여 완성된 작업이라고 불리는 결과물을 만들어낸다. 그후 React는 이 완성된 작업을 커밋하여 DOM에서 시각적인 변화가 발생하게 된다. 해당 과정은 두 단계로  나뉘어진다.

#### 1.Fiber 트리 렌더링

![Pasted image 20250224182316.png](../img/Pasted%20image%2020250224182316.png)


하나는 current tree, 또 다른 하나는 workInProgress tree 이다. current tree는 현재 우리의 UI에 렌더된 트리이다.리액트는 일관되지 않은 UI를 나타낼수도 있기 때문에, 이 tree를 변경하지 못한다.대신에 리액트는 workInProgress라는 트리를 교체하고 모든 변경 사항들이 적용되면 포인터를 교체한다.모든 사항이 적용된 workInProgress트리는 current tree가 되고, 해당 current tree의 복제본을 만들어서 또 다른 workInProgress 트리를 생성한다.

#### 2.Render 와 Reconciliation Phase

이 단계에서 리액트는 아래의 단계를 따르는 workinProgress tree를 만든다.

1.  `setState()` 메서드 호출: 컴포넌트의 상태를 업데이트하기 위해 setState() 메서드가 호출되면, React는 requestIdleCallback()을 사용하여 작업을 예약한다. 이 함수는 메인 스레드에게 유휴(Idle) 시간이 생기면 해당 작업을 수행하라고 알린다.
2. `workInProgress`파이버 트리 생성: 이제 React는 현재의 파이버 트리에서 요소들을 복제하여 `workInProgress` 파이버 트리를 만들기 시작한다. 그리고 각 노드를 순회하면서 변경이 필요한지 결정한다.
3. 변경된 노드 처리: 특정 노드가 업데이트 된 경우 그 노드는 `effect list`에 추가된다. 이 리스트는 커밋 단계에서 실제 DOM에 반영되는 변경 사항을 추적한다.`
	- 이 리스트는 선형 연결 리스트 구조이다.
4. phase 완료: `workInProgress` 트리 전체를 순회하고, 모든 업데이트된 노드가 태그되면 첫 번째 phase가 완료된다.

#### 3.Commit Phase

 `effects list`에 있는 노드들의 모든 업데이트가 수행되고 DOM에 반영된다. 메인 스레드는 모든 변경사항을 한 번에 적용한다. 랜더링 단계와 달리 일시 중지되거나 재개될 수 있는 것이 아니라 동기적으로 진행된다.








