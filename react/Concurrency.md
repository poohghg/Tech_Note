> React 18의 동시성 기능은 기존 렌더링 모델을 개선해 더 유연하고 사용자 경험을 해치지 않는 UI업데이트를 가능하게 한다.
### 동시성이란 ?

동시성은 여러 작업을 동시에 처리하는 것처럼 보이도록 스케줄링하는 개념이다. `React18` 에서는 이를 통해 **렌더링 작업을 더 세분화하고, 중단** 및 재개할 수 있게 되었다.

예전에는 하나의 렌더링 작업이 시작되면 끝날 때가지 멈출 수 없었지만, React 18에서는 중간에 끊고 우선순위가 높은 작업을 먼저 처리 할 수 있다.

- 사용자 입력 등을 먼저 처리 할 수 있다.
- 렌더링 작업을 나누어 여러 번에 걸쳐 처리 할 수 있다.
- 동시성은 UI가 끊김 없이 부드럽게 반응하도록 만드는 핵심기술이다.

### React 18에서 도입된 주요 동시성 기능

- **Automatic Batching**: 여러 개의 상태 업데이트를 하나의 렌더링으로 묶어 처리한다.
- **Concurrent Rendering**: 렌더링 작업을 중단하고 우선순위가 높은 작업을 먼저 처리할 수 있다.
- **Transitions**: 상태 업데이트가 UI에 미치는 영향을 제어할 수 있다.
- **Suspense**: 비동기 작업을 처리할 때 UI를 일시적으로 대체할 수 있다.
- **Selective Hydration**: 서버에서 렌더링된 HTML을 클라이언트에서 하이드레이션 할 때, 필요한 부분만 하이드레이션 할 수 있다.
- **Streaming**: 서버에서 렌더링된 HTML을 클라이언트로 스트리밍할 수 있다.

#### Automatic Batching

``` jsx

function handleClick() {
  setCount(c => c + 1);
  setFlag(f => !f);
  // 이전 버전: 각각 re-render 발생
  // React 18: 한 번만 render!
}

```

- React 18은 여러 상태 업데이트를 자동으로 하나로 묶어 렌더링을 최적화한다.
- 비동기 함수 내의 setState 호출도 자동으로 배치 처리된다.
- 이전 버전에서는 각각의 setState 호출이 별도의 렌더링을 발생시켰지만, React 18에서는 하나의 렌더링으로 처리된다.
- 이는 성능을 개선하고 불필요한 렌더링을 줄여준다.
#### CreateRoot

``` jsx
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(<App />);

```

- React 18부터는 `ReactDOM.render()` 대신 `createRoot()`를 사용해야 **동시성 기능이 활성화**된다.
- React 내부적으로 렌더링 작업을 스케줄링 하고 중단 가능하게 만든다.

#### Transitions

``` jsx

import { startTransition, useState } from 'react';

const Example = () => {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  const handleChange = (e) => {
	// 바로 반영
    const value = e.target.value;
    setQuery(value);

   // 비긴급, 나중에 반영
    startTransition(() => {
      const filtered = heavySearch(value);
      setResults(filtered);
    });
  };

  return (
    <input value={query} onChange={handleChange} />
  );
};

```

``` jsx

const [isPending, startTransition] = useTransition();

const handleClick = () => {
  startTransition(() => {
    fetchSomething().then(setData);
  });
};

return (
  <>
    <button onClick={handleClick}>불러오기</button>
    {isPending ? <LoadingSkeleton /> : <ActualDataComponent />}
  </>
);



```

- `startTransition`은 **비긴급 작업**(예: 검색 결과 필터링, 리스트 정렬 등)을 낮은 우선순위로 실행하게 한다.
- 사용자 입력과 같이 긴급한 업데이트를 막지 않고 부드럽게 실행할 수 있게 돕는다.

#### useTransition

``` jsx
import { useTransition, useState } from 'react';

const Example = () => {
  const [isPending, startTransition] = useTransition();
  const [query, setQuery] = useState('');

  const handleChange = (e) => {
    const value = e.target.value;
    startTransition(() => {
      setQuery(value);
    });
  };

  return (
    <div>
      <input value={query} onChange={handleChange} />
      {isPending && <span>Loading...</span>}
    </div>
  );
};
```

- `useTransition`은 **비긴급 상태 업데이트**를 처리하는 Hook이다.
- `isPending`은 상태 업데이트가 진행 중인지 여부를 나타낸다.
- `startTransition`으로 감싼 업데이트가 실행 중일 때 `isPending`이 `true`가 된다.

### 기존과의 차이점

|         | 17 이전   | 18 이후                 |
| ------- | ------- | --------------------- |
| 렌더링     | 블로킹 렌더링 | 비블로킹, 중단 가능           |
| 우선순위 조절 | 불가능     | 가능                    |
| 자동 배치   | 제한적     | 더 넓은 범위에서 자동          |
| 유연성     | 낮음      | 높음 (진입 애니메이션, 점진적 로딩) |
- 동시성 모드에 완벽히 대응하려면, 순수 함수형 컴포넌트로 작성하는것이 좋다.
- Suspense와의 조합이 핵심 (예: SSR, Lazy loading)

| 기능                       | 설명                       | Fiber와의 관계                  |
| ------------------------ | ------------------------ | --------------------------- |
| **Automatic Batching**   | 여러 상태 업데이트를 하나로 묶음       | Fiber가 렌더링을 모아서 처리          |
| **Concurrent Rendering** | 렌더링 중 우선순위 높은 작업을 먼저 처리  | Fiber가 작업을 쪼개고 중단/재개 가능하게 함 |
| **Transitions**          | 어떤 상태 업데이트는 "덜 중요"하다고 표시 | Fiber가 우선순위를 다르게 두고 처리      |