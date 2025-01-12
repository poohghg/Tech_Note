>참조ffff
>
>[New Suspense SSR Architecture in React 18](https://github.com/reactwg/react-18/discussions/37)
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

- 서버에서 전체 









