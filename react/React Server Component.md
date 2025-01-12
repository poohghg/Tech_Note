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

##### 리액트의 SSR은 여러 단계로 발생한다.

- 서버에서 전체 앱의 데이터를 가져온다
- 서버에서 전체 앱을 HTML로 렌더링하고 응답을 보낸다.
- 클라이언트에서 전체 앱에 대한 자바스크립트 번들 코드를 로드한다.
- 클라이언트에서 서버에서 생성된 HTML에 자바스크립트 로직을 연결한다.
	- 이는 하이드레이션 과정이다.

문제는 다음 단계를 시작하기 전에 전체 앱에 대해 각 단계를 완료해야 한다는 것이다.
- SSR은 **waterfall** 현상이 있다.
- SSR은 "All or Nothing"

#### What is SSR?

SSR을 사용하지 않는다면 자바스크립트가 로딩되는 동안 유저는 빈 페이지를 보게 된다.
- 이것은 좋은 않은 사용자 경험이다.
- 특히 자바스크립트 로딩 시간이 오래 걸리는 저사양 디바이스에서 큰 문제가 될 수 있다.

SSR은 리액트 컴포넌트를 서버에서 HTMl로 렌더링해서 유저에거 내려주는 방식으로 동작한다. 이렇게 내려온 HTML은 브라우저에서 기본적으로 제공하는 link, form, input 등과 같은 상호작용 요소를 제외하고는 interactive하지 않지만, 유저들로 하여금 자바스크립트가 로딩되는 동안 컨텐츠를 볼 수 있도록 한다.

##### 하이드레이션(Hydration) 과정

SSR 과정에서 리액트는 메모리상에 컴포넌트 렌더링하지만(Virtual DOM 생성), DOM 노드를 생성하지 않는다(이미 서버에서 HTML은 생성). 대신 이미 존재하는 HTML에 이벤트 핸들러와 같은 자바스크크립트 로직을 첨부 한다. 

##### 하이드레이 이란?
React가 로드되고 나서 컴포넌트를 메모리에 렌더링하고 이벤트 핸들러와 같은 자바스크립트 로직을 첨부한는 과정이 하이드레이션 과정이다. 






















   