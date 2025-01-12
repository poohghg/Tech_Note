>참조
>
>[New Suspense SSR Architecture in React 18](https://github.com/reactwg/react-18/discussions/37)
  [RFC: React Server Components](https://github.com/reactjs/rfcs/blob/main/text/0188-server-components.md)
>[RSC From Scratch](https://github.com/reactwg/server-components/discussions/5)

- 브라우저에는 리액트 서버컴포넌트가 없다.
- 서버 환경에서 리액트 컴퍼넌트를 개발할 수 있는 환경을 만든는 것
- renderJSXToHTML 함수에서 만 비동기 처리 한다.
- 상위 router 함수는 동기식이다.
	- 내부 renderJSXToHTML 함수의 컴포넌트 반환은 비동기다
	- 서버 컴포넌트면 비동기
#### React의 SSR 렌더링 방식

Server Side Rendering를 사용하면 서버의 리액트 구성 요소에서 HTML을 생성하고 해당 HTML을 클라이언트에 보낼 수 있다. SSR을 사용하면 자바스크립트 번들이 로드되고 실행되기 전에 사용자가 페이지의 콘텐츠를 볼 수 있다.

리액트의 SSR은 여러 단계로 발생한다.

- 서버에서 전체 앱의 데이터를 가져온다
- 서버에서 전체 앱을 HTML로 렌더링하고 응답을 보낸다.
- 클라이언트에서 전체 앱에 대한 자바스크립트 번들 코드를 로드한다.
- 클라이언트에서 서버에서 생성된 HTML에 자바스크립트 로직을 연결한다.
	- 하이드레이션 과정이다.

문제는 다음 단계를 시작하기 전에 전체 앱에 대해 각 단계를 완료해야 한다는 것이다.
- SSR은 **waterfall** 현상이 있다.
- SSR은 "All or Nothing"










