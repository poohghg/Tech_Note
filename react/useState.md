React는 useState를 포함한 훅들을 컴포넌트가 렌더링될 때 순서 기반으로 호출한다.
즉, 각 훅의 호출 순서에 따라 내부적으로 상태를 연결 리스트 형태로 저장하며, 이를 통해 이전 렌더링에서 저장된 상태와 현재 렌더링을 연결한다.

- `setState`는 단순히 값을 바꾸는 함수가 아니라
	- 업데이트 요청을 업데이트 큐에 등록하는 작업
	- 그리고 React가 한 번에 처리하면서 최신 상태를 적용하는 방식이다.
- React 18+에선 특히 `setState`가 **concurrent 모드에서 지연되거나 재정렬**될 수도 있다
	- **렌더 중에도 상태 변경이 생길 수 있기 때문이다.**
	- 이전 값을 기반으로 한 업데이트가 필요할 땐 **무조건 `prev =>` 형식을 써야 안전하다.**

### 렌더링과 useState의 위치

React는 컴포넌트가 렌더링될 때 훅들을 순서대로 호출 하면서 관리한다.
useState는 호출 순서에 따라 상태를 매핑 하는데, 이는 Fiber의 업데이트 과정과 직결된다.

- React가 state를 훅을 순서 기반으로 추적한다.
- useState의 호출 순서가 변경되면, React는 기존 상태와 새로운 상태를 올바르게 매핑하지 못한다.

### Fiber의 관점에서 

- React의 Fiber 구조에서 상태는 링크드 리스트 형태로 관리된다.
- 각 useState는 Fiber노드의 memoizedState에 저장되며, 훅을 호출할 때마다 이 리스르틑 따라가면서 값을 가져온다.
- React는 `extra` 상태가 없다고 생각하다가, 새로운 훅이 추가되면서 `useState`의 순서가 바뀜
- 업데이트 시
	- 리렌더링이 발생하면, React는 기존 Fiber 트리에서 훅이 호출된 순서대로 이전 상태를 찾아 연결한다.
	- 상태가 변경되면 동일한 순서의 훅이 이전 값을 참조하여 업데이트한다
- useState는 호출 선수를 기준으로 상태를 저장하므로 조건문 안에서 호출하면 순서가 바뀌면서 오류가 발생한다.
- Fiber는 연결 리스트로 관리하는데, 순서가 어긋나면 올바른 상태를 가져오지 못한다.
- 훅은 항상 최상위에서 호출하야 React가 에측 가능한 방식으로 상태를 관리할 수 있다.

### Fiber의 상태 업데이트 및 재조정

``` jsx

	const [count, setCount] = useState(0);
	
	const handleClick = () => {
		// count = 3
		setCount(count + 1);
		setCount(count + 2);
		setCount(count + 3);
	};

```

#### setState호출 시 마지막 상태만 적용되는 이유

React에서 setState를 여러 번 호출 해도 마지막 호출만 적용되는 것처럼 보이는 이유는 React의 상태 업데이트 배치 및 재조정 과정과 연관된다.

- React의 상태 업데이트 배치 처리
- Fiber의 상태 스케줄링 및 재조정
- 최신 상태 적용을 위한 상태 병합

#### 상태 업데이트 배치 처리

React는 여러 개의 setState 호출을 한 번의 렌더링으로 처리하기 위해 상태 업데이트를 배치 처리한다.

- 동일한 렌더 사이클 내에서 상태 업데이트를 병합하기 때문이다.
- setState는 비동기적으로 실행되며, React는 모든 setState를 배치하여 하나의 최종 값으로 병합한다.
	- 실제 setState는 비동기 함수는 아니다, 다만 비동기적으로 실행된다.
	- queueMicrotask를 사용?
- React는 최신 setState 호출만 나기고 이전 호출을 덮어씌우는 방식으로 동작한다.

#### Fiber의 상태 스케줄링 및 재조정

React의 fiber는 상태 업데이트를 스케줄링 하며, 가능한 경우 재조정 단계를 최적화하여 불필요한 렌더링을 방지한다.

##### 스케줄링 과정

1. setState가 호출되면, React는 업데이트 객체(enqueued update)를 생성하여 현재 컴포넌트의 업데이트 큐(update queue)에 추가한다.
2. React는 여러 setState 호출을 ==같은 렌더링 프레임 내에서 배치 처리== 하여, 불필요한 렌더링을 방지한다.
3. `setState`가 여러 번 호출 되어도, **마지막 업데이트만 적용**되도록 기존 상태를 덮어씁니다.
4. 즉, Fiber는 모든 `setState` 호출을 수집한 후 한 번의 렌더링만 수행하며, 같은 값이 여러 번 변경될 경우 마지막 값만 유지합니다.

#### 최신 상태 적용을 위한 상태 병합

React의 setState는 이전 상태를 기반으로 한 업데이트를 병합하는 방식으로 동작한다. 이때 이전 값을 기반으로 연속된 업데이트를 적용하려면 `prevState` 콜백을 사용해야 합니다.

``` jsx
function Example() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
	// 6
    setCount(prev => prev + 1);
    setCount(prev => prev + 2);
    setCount(prev => prev + 3); 
  };

  console.log("render", count);

  return <button onClick={handleClick}>Click Me</button>;
}

```

`prevState`를 사용하면 각 `setState` 호출이 독립적으로 처리되어, 상태가 예상대로 누적됩니다.

``` jsx

// React는 업데이트 큐에 등록.
updateQueue = [
  () => prev + 1,
  () => prev + 1
];

// 렌더링 사이클이 오면, React는 이 큐를 순서대로 실행하면서  
// 각 함수에 최신 prev 값을 넣어준다.

let state = 0;

// prev 예시
for (let update of updateQueue) {
  state = update(state); // prev => prev + 1
}

// 일반 state 예시
updateQueue = [
  1,2,3
];

for (let update of updateQueue) {
  state = update; // state = 3
}

```

### 결론

1. **React의 배치 처리 (Batching)**
    - 동일한 렌더 사이클 내에서 여러 `setState` 호출이 병합됨.
2. **Fiber의 상태 스케줄링**
    - `setState`는 업데이트 큐에 추가되고, React는 마지막 호출만 유지.
3. **최신 상태 적용 (State Merging)**
    - `setState(newValue)` 방식은 이전 상태를 무시하고 마지막 값으로 덮어씌움.
4. **이전 값을 기반으로 업데이트하려면 `prevState`를 사용해야 함**.
    

✅ **따라서, 여러 `setState`를 연속 호출할 때 누적된 업데이트가 필요하면, `setState(prev => prev + value)` 형태를 사용해야 한다.**