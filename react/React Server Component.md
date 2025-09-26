> 참조
> 
> https://github.com/reactwg/server-components/discussions/5
> https://github.com/reactjs/rfcs/blob/main/text/0188-server-components.md
> https://tech.kakaopay.com/post/react-server-components/
> https://velog.io/@seeh_h/React-Server-Component-Streaming-SSR%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0

## # 복습

### 클라이언트 사이드 렌더링

![Pasted image 20250330044203.png](../img/Pasted%20image%2020250330044203.png)

- 클라이언트 사이드 렌더링을 사용하는 경우 페이지 진입 시, HTML, 자바스크립트, 그리고 모든 데이터가 로드되고 컴포넌트 렌더링이 끝나기전 까지 사용자는 아무런 기능이 없는 빈 화면만 보게 된다. 자바스크립트 번들의 용량이 크거나 사용자의 네트워크 속도가 느릴수록 사용자는 오랜 시간 빈 화면을 보게 되고 이는 사용자 경험을 저하하게 된다.

### 서버 사이드 렌더링

![Pasted image 20250330044359.png](../img/Pasted%20image%2020250330044359.png)

- 서버에서 먼저 HTML로 렌더링한다. 페이지가 클라이언트에서 동작하기 위해서 자바스크립트 번들이 모두 다운되고  [hydration](https://github.com/reactwg/react-18/discussions/46#discussioncomment-846714)이 완료되어야 하지만 빈 화면 대신 데이터가 존재하는 HTML을 제공함으로써 무거운 자바스크립트 파일이 다운로드되는 동안 사용자에게 의미 있는 콘텐츠를 제공할 수 있다.
- 서버 사이드 렌더링의 목적은 non-interactive한 버전의 클라이언트 컴포넌트를 최대한 빠르게 브라우저에 전달하여 초기 페이지의 First Contentful Paint 또는 Largest Contentful Paint 속도를 향상시는 것이다.

### 서버 컴포넌트와 서버 사이드 렌더링의 차이

서버 컴포넌트와 서버 사이드 렌더링은 서버에서 렌더링 된다는 유사점이 있지만 해결하고자 하는 문제점은 다르다.

- 서버 컴포넌트의 코드는 클라이언틑로 전달되지 않는다. 하지만 서버 사이드 렌더링의 모든 컴포넌트의 코드는 자바스크립트 번들에 포함된다.
- 서버 컴포넌트는 페이지 레벨에 상관없이 모든 컴포넌트에서 서버에 접근 가능하다. Next.js의 경우 가장 top level의 페이지에서만 `getServerProps()`나 `getInitialProps()`로 서버에 접근 가능하다.
- 서버 컴포넌트는 클라이언트 상태를 유지하면서 refetch 될 수 있다. 서버 컴포넌트는 HTML이 아닌 특별한 형태로 컴포넌트를 전달하기 때문에 필요한 경우 포커스, 인풋 입력값 같은 클라이언트 상태를 유지하며 여러 번 데이터를 가져오고 리렌더링하여 전달 할 수 있다. 하지만 SSR의 경우 HTML로 전달되기 때문에 새로운 refetch가 필료한 경우 HTML 전체를 리렌더링 해야한다.
	- Next.js의 경우 router.refresh() 사용

---
## RSC(React Server Component)

### Streaming SSR도 해결하지 못한 이슈

- 리액트는 여전히 클라이언트 중심으로 동작한다. 즉 서버를 충분히 활용하지 못하고 있다.
- Streaming 방식을 사용한다고 해도 결국 서버는 초기 HTML을 만드는 과정에만 관여한다.
- 라우팅을 비롯한 모든 작업은 클라이언트 사이드에서 진행된다.
	- 이는 서버 자원을 최대한 활용한다고 보기 어렵다.
- RSC는 서버 환경에서만 실행되는 컴포넌트를 의미한다. 즉 서버에서만 실행되는 컴포넌트이다.

### 추가 개념

- Streaming HTML
	- 데이터를 한번에 로드하지 않고 일정한 부분만 나누어 점진적으로 처리
	- 서버 컴포넌트는 streamingHTML을 지원한다. 서버 컴포넌트는 서버에서 렌더링되어 클라이언트로 전송되는 컴포넌트이다. 서버 컴포넌트는 클라이언트에 의해 렌더링되지 않으며, 클라이언트에 의해 실행되지 않는다. 서버 컴포넌트는 서버에서 렌더링되어 클라이언트로 전송되는 컴포넌트이다.
- 동기 컴포넌트의 문제
	- 클라이언트 관점 : 컴포넌트는 브라우저에서 실행 -> 서버 API 불가
	- 브라우저에서 컴포넌트 함수가 await을 사용할 수 없다.
	- 만약 지연 된다면 그사이에 다른 코드가 실행되어 브라우저가 멈추게 된다.
	- 리액트팀은 컴포넌트 렌더 함수는 즉시 JSX 반환 규칙을 고수하고 있음
		- 비동기 로직이 있다면?
			- useEffect나 다른 컴포넌트에서 처리해야함
			- Promise를 반환하고 Sususpense로 처리해야함
			- fallback + Promise 반환된 값을 렌더링
####  New Mental Model

![Pasted image 20250407040403.png](../img/Pasted%20image%2020250407040403.png)

- 서버 컴포넌트로 구성된 서버 트리가 추가되고, 서버 트리는 서버 컴포넌트로만 구성된다. 이 트리는 리액트 트리보다 먼저 실행되며 실행한 결과값을 리액트 트리로 넘겨준다.

![Pasted image 20250407040452.png](../img/Pasted%20image%2020250407040452.png)

> 이것이 클라이언트 구성 요소가 여전히 HTML로 SSR되는 이유입니다. 클라이언트 구성 요소는 항상 알고 있는 구성 요소입니다. 이들은 이전에 SSR될 수 있었고(첫 번째 로드 중에 더 빠르게 무언가를 보여주기 위해), 여전히 HTML로 SSR됩니다.
> 
> 이에 대해 생각하는 한 가지 방법은 RSC에서 "서버"와 "클라이언트"가 물리적 서버와 클라이언트에 직접 대응하지 않는다는 것입니다. "React 서버"와 "React 클라이언트"로 생각할 수 있습니다. Props는 항상 React 서버에서 React 클라이언트로 흐르고, 그 사이에 직렬화 경계가 있습니다. React 서버는 일반적으로 빌드 시간(기본값) 또는 실제 서버에서 실행됩니다. React 클라이언트는 일반적으로 두 환경 모두에서 실행됩니다(브라우저에서는 DOM을 관리하고, 다른 환경에서는 초기 HTML을 생성합니다).

- 서버 트리가 새롭게 추가되면서 서버 컴포넌트는 클라이언트 컴포넌트와는 다르게 서버에서만 실행된다. 즉 서버에서만 실행되는 컴포넌트이다.
- 기존의 리액트 모델은 유지된다. 
###  Benefits of RSC

#### 1.파일시스템, API, 데이터베이스에 직접 접근 가능

- 서버 컴포넌트는 서버에서만 실행되기 때문에 파일 시스템, 서버 API와 데이터베이스에 직접 접근할 수 있다.

#### 2.제로 번들 사이즈

- 서버 컴포넌트는 클라이언트로 전달되지 않기 때문에 서버 컴포넌트의 코드는 클라이언트 번들에 포함되지 않는다. 즉 서버 컴포넌트는 제로 번들 사이즈를 가진다.
- 서버 컴포넌트는 정확히 RSC Payload 라는 특별한 포멧으로 클라이언트로 전달된다.
	- RSC Payload는 서버 컴포넌트의 결과물이다.

##### RSC Payload

RSC Payload에는 **서버 구성 요소에서 클라이언트 구성 요소로 전달된 모든 props가** 포함된다. 그런 다음 [props가 직렬화된다.](https://github.com/reactjs/rfcs/blob/main/text/0188-server-components.md#simplified-loading-sequence) .


#### 3. 페이지 레벨이 아닌 컴포넌트 레벨

![Pasted image 20250407042008.png](../img/Pasted%20image%2020250407042008.png)

- 가장 크고 중요한 차이점은 **해당 로직을 페이지 레벨이 아닌 컴포넌트 레벨로 내렸다는 것입니다.** 그리고 이런 서버 컴포넌트는 전체 컴포넌트 트리 어디에서나 삽입될 수 있다.

#### 4. 자동 코드 분할

- 일반적인 접근 방식은 경로(라우터)당 번들을 지연 로드하거나 런타임에 몇 가지 기준에 따라 다른 모듈을 지연 로드하는 것입니다. 예를 들어, 애플리케이션은 사용자, 콘텐츠, 기능 플래그 등에 따라 다른 코드를(다른 번들에서) 지연 로드할 수 있다.
- 서버 구성 요소는 클라이언트 구성 요소의 모든 가져오기를 잠재적인 코드 분할 지점으로 처리한다.

``` jsx

// PhotoRenderer.js - Server Component

// one of these will start loading *once rendered and streamed to the client*:
import OldPhotoRenderer from './OldPhotoRenderer.js';
import NewPhotoRenderer from './NewPhotoRenderer.js';

function Photo(props) {
  // Switch on feature flags, logged in/out, type of content, etc:
  if (FeatureFlags.useNewPhotoRenderer) {
    return <NewPhotoRenderer {...props} />;
  } else {
    return <OldPhotoRenderer {...props} />;
  }
}

```