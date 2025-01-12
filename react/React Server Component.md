>참조
>
>[RSC From Scratch](https://github.com/reactwg/server-components/discussions/5)
>

- 브라우저에는 리액트 서버컴포넌트가 없다.
- 서버 환경에서 리액트 컴퍼넌트를 개발할 수 있는 환경을 만든는 것
- renderJSXToHTML 함수에서 만 비동기 처리 한다.
- 상위 router 함수는 동기식이다.
	- 내부 renderJSXToHTML 함수의 컴포넌트 반환은 비동기다
	- 서버 컴포넌트면 비동기




