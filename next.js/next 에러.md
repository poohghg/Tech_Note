###  hydration mismatch

##### 원인

- 리액트를 통해 렌더링 될 최초의 결과물이 hydration할 html과 일치 하지 않아 발생한다.
- https://blog.hwahae.co.kr/all/tech/13604

##### 주로 발생하는 케이스

- 리액트에서 생성된 html에 whirespace가 있는 경우
- typeof window !== undefined 또는 브라우저 전용 스토리지의 값을 사용하는 경우
  - 서버에서 만들어지 html 결과물과 클라이언트에서 첫 하이드레이션 후 만들어진 렌더링 결과물이 달라서 발생

##### 해결 방법

- useEffect 사용
  - 렌더링 후 트리거 되어 느리게 업데이트됨
- 다이나믹 임폴트 사용
  - 클라이언트에서 해당 청크 파일을 불러와 UI에 적용
  - 프리렌더링이 되지 않아 CLS을 발생시 킬 수 있음
    - 대체 컴포넌트 반드시 사용
- suppressHydrationWarning 사용
  - 이 방법은 text content 대상으로만 가능하다. nested 된 dom 구조에 suppressHydrationWarning을 적용한다면 hydration mismatch 에러가 발생한다.
- 일반적인 [하이드레이션 불일치 해결방법](https://nextjs.org/docs/messages/react-hydration-error) 참조
