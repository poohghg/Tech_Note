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
### # React의 SSR 렌더링 방식

Server Side Rendering를 사용하면 서버의 리액트 구성 요소에서 HTML을 생성하고 해당 HTML을 클라이언트에 보낼 수 있다. SSR을 사용하면 자바스크립트 번들이 로드되고 실행되기 전에 사용자가 페이지의 콘텐츠를 볼 수 있다.

#### 리액트의 SSR은 여러 단계로 발생한다.

- 서버에서 전체 앱의 데이터를 가져온다
- 서버에서 전체 앱을 HTML로 렌더링하고 응답을 보낸다.
	- SSR 과정에서 HTML 렌더링하는 시점에 서버에서 컴포넌트 렌더를 위한 데이터가 모두 준비 되어 있어야한다.
- 클라이언트에서 전체 앱에 대한 자바스크립트 번들 코드를 로드한다.
- 클라이언트에서 서버에서 생성된 HTML에 자바스크립트 로직을 연결한다.
	- 이는 하이드레이션 과정이다.
	- 하이드레이션 과정은 전체 렌더 트리에서 발생한다. 즉 루트에서 시작해 모든 리액트 컴포넌트에서 적용된다.

문제는 다음 단계를 시작하기 전에 전체 앱에 대해 각 단계를 완료해야 한다는 것이다.
- SSR은 **waterfall** 현상이 있다.
- SSR은 "All or Nothing"

#### 기존 리액트 SSR의 문제점

- You have to fetch everything before you can show anything
	- 서버에서 HTML을 렌더하는 시점에 컴포넌트를 렌더링하기 위한 모든 데이터가 준비되어야 한다.
- You have to load everything before you can hydrate anything
	- 클라이언트의 HTML 하이드레이션 과정에서 하이드레이션을 하기위해 모든 컴포넌트의 자바스크립트 파일이 로드되어야 한다.
- You have to hydrate everything before you can interact with anything
	- 사용자와 상호작용을 하기위해서는 모든 컴포넌트가 하이드레이션 과정을 끝내야한다.

#### What is SSR?

SSR을 사용하지 않는다면 자바스크립트가 로딩되는 동안 유저는 빈 페이지를 보게 된다.
- 이것은 좋은 않은 사용자 경험이다.
- 특히 자바스크립트 로딩 시간이 오래 걸리는 저사양 디바이스에서 큰 문제가 될 수 있다.

SSR은 리액트 컴포넌트를 서버에서 HTML로 렌더링해서 유저에거 내려주는 방식으로 동작한다. 이렇게 내려온 HTML은 브라우저에서 기본적으로 제공하는 link, form, input 등과 같은 상호작용 요소를 제외하고는 interactive하지 않지만, 유저들로 하여금 자바스크립트가 로딩되는 동안 컨텐츠를 볼 수 있도록 한다.

#### 하이드레이션(Hydration) 과정

SSR 과정에서 리액트는 메모리상에 컴포넌트 렌더링하지만(Virtual DOM 생성), DOM 노드를 생성하지 않는다(이미 서버에서 HTML은 생성). 대신 이미 존재하는 HTML에 이벤트 핸들러와 같은 자바스크크립트 로직을 첨부 한다. 

#### 하이드레이션 이란?

React가 로드되고 나서 컴포넌트를 메모리에 렌더링하고 이벤트 핸들러와 같은 자바스크립트 로직을 첨부하는 과정이 하이드레이션 과정이다. 
- 위 과정이 동작하려면 브라우저에서 컴포넌트를 기반으로 생성된 트리 와 서버에서 생성된 트리가 일치해야 한다.
- 하이드레이션 과정이 시작하려면 클라이언트에 모든 자바스크립트가 로드 되어야 한다.
- 물론 Code Splitting을 통해 컴포넌트를 따로 로드 할 수 있지만, 분리된 컴포넌트는 최초 하이드레이션 과정에 포함되지 않는다.
- 하이드레이션 과정의 완료는 전체 컴포넌트 트리에서 하이드레이션 과정이 완료되어야 종료 된다.
	- 부분적으로 불가하다.

SSR 과정 자체로는 사용자와의 상호작용을 더 빠르게 만들지 않는다.
- 대신 상호작용없는 애플리케이션 컨텐츠를 빠르게 볼 수 있다.
	- 유저들은 자바스크립트 번들이 로도되는 동안 정적 컨텐츠를 볼 수 있다.
- 특히 네트워크 상황이 좋지 않은 유저들에게 유저가 인식하는 성능 향상을 가져온다.
- 쉬운 인덱싱과 빠른 속도는 Search Engine Optimization에도 도움이 된다.

---
### # React 18: Streaming HTML and Selective Hydration

- **Streaming HTML** on the server. To opt into it, you’ll need to switch from renderToString to the new renderToPipeableStream method, as [described here](https://github.com/reactwg/react-18/discussions/22).
- **Selective Hydration** on the client. To opt into it, you’ll need to [switch to hydrateRoot](https://github.com/reactwg/react-18/discussions/5) on the client and then start wrapping parts of your app with Suspense.

``` ts
<Layout>  
	<NavBar />  
	<Sidebar />  
	<RightPane>  
		<Post />  
		<Suspense fallback={<Spinner />}>  
			<Comments />  
		</Suspense>  
	</RightPane>  
</Layout>
```

![[Pasted image 20250113003645.png]]

React 18은 페이지의 부분을 Suspense로 감싸서 특정 컴포넌트가 준비되기 전까지 fallback UI를 보여주도록 할 수 있다. 최초 렌더링된 HTML에는 댓글 컴포넌트 대신  fallback UI가 생성 된다.

이때 서버 쪽에서 댓글 데이터가 준비되면, 리액트는 동일한 Stream에 추가되는 HTML과, 해당 HTML을 올바른 “위치”에 주입하기 위한 작은 inline “script” 태그를 보내준다.

이는 "You have to fetch everything before you can show anything" 문제를 해결 한다. 
- 서버에서 데이터 패칭 시간을 기다릴 필요가 없다. 
- 해당 부분만 HTML 스트리밍 상에 나중에 들어오게 할 수 있다.

또한 React 18에서 Suspense는 댓글 위젯이 로드되기 전에 애플리케이션을 하이드레이션 할 수 있게 해준다. Selective Hydration을 통해 스트리밍(Suspense의 컴포넌트) 되는 부분을 제외하고 하이드리에션을 수행한다.

Suspense로 부분적으로 컴포넌트를 묶음으로써 리액트가 Streaming과 Hydration이 렌더링을 Block 하는 것을 막아준다.
- 이는 하이드레이션 시작하기 위해 모든 코드가 로드되는 것을 기다릴 필요가 없다.
- 코드의 부분부분이 로드될 때 마다 하이드레이션을 진행 할 수 있다.

 Suspense와 non-blocking Hydration에 대한 것을 리액트에서 알아서 처리한다. 이로 인해 작업이 예상하지 못한 순서로 진행되는 것에 대해 걱정할 필요가 없다. 
 -  만약 Straming HTML보다도 나머지 부분의 자바스크립트 코드가 더 빨리 로드되었다면 리액트는 나머지 페이지를 먼저 Hydration 한다. 
 - Straming HTML이 먼저 도착하든, 자바스크립트 코드가 먼저 도착하든 Suspense는 일관적으로 non-blocking 하게 동작하며 리액트는 그저 먼저 도착한 것을 먼저 처리할 뿐이다.















   