## Recoil

Recoil은 Atomic구조를 통해 만드어진 페이스북에 개발한 상태관리 라이브러리다.

- 오직 React만을 위해 React처럼  내부상태만 이용
- 작은 Atom단위로 관리
- 순수함수 Seletor
- Re-Render 최소화
  - 아톰이 변경되면 아톰을 구독하고 있는 컴퍼넌트만 리렌더링됨.
- 곧 새로운 React기능과 결합의 가능성
### Atom

atom은 상태(state)의 일부를 나타낸다. Atoms는 어떤 컴퍼넌트에서 읽고 쓸 수 있다. atom의 값을 읽는 컴퍼넌트들은 암묵적으로 atom을 구독한다.그래서 atom을 구독하는 모든 컴퍼넌트들이 재런디링 되는 결과가 발생한다.

```tsx
const textState = atom({
  key: 'textState', // unique ID (with respect to other atoms/selectors)
  default: '', // default value (aka initial value)
});

// 컴퍼넌트가 아톰을 쓰고 읽기 위해서 useRecoilState휵울 사용한다.
const [text, setText] = useRecoilState(textState);
```

### Selector

selector는 파생된 상태(**derived state**)의 일부를 나타낸다. 파생된 상태는 상태의 변화다. 파생된 상태를 어떤 방법으로든 주어진 상태를 수정하는 순수 함수에 전달된 상태의 결과물로 생각할 수 있다.