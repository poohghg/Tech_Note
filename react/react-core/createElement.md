> createElement를 사용하면 React 엘레먼트를 생성할 수 있다. JSX를 작성하는 대신 사용할 수 있다.

``` js
const element = createElement(type, props, ...children)
```

> JSX
> JavaScript + HTML 유형의 새로운 문법이다. -> JSON 트리 형태로 변환된다.
> 
> _JSX_는 JavaScript를 확장한 문법으로, JavaScript 파일을 HTML과 비슷하게 마크업을 작성할 수 있도록 해준다.
> React 컴포넌트는 JSX라는 확장된 문법을 사용하여 마크업을 나타낸다.
> 
> JSX는 자바스크립트로 HTML을 표현할 수 있는 문법이다.

`type`, `prop`, `children`를 인수로 제공하고 `createElement`을 호출하여 React 엘리먼트를 생성

``` ts
// DOM Elements  
// TODO: generalize this to everything in `keyof ReactHTML`, not just "input"  
function createElement(  
type: "input",  
props?: InputHTMLAttributes<HTMLInputElement> & ClassAttributes<HTMLInputElement> | null,  
...children: ReactNode[]): DetailedReactHTMLElement<InputHTMLAttributes<HTMLInputElement>, HTMLInputElement>;  

function createElement<P extends HTMLAttributes<T>, T extends HTMLElement>(  
type: keyof ReactHTML,  
props?: ClassAttributes<T> & P | null,  
...children: ReactNode[]): DetailedReactHTMLElement<P, T>;  

function createElement<P extends SVGAttributes<T>, T extends SVGElement>(  
type: keyof ReactSVG,  
props?: ClassAttributes<T> & P | null,  
...children: ReactNode[]): ReactSVGElement;  

function createElement<P extends DOMAttributes<T>, T extends Element>(  
type: string,  
props?: ClassAttributes<T> & P | null,  
...children: ReactNode[]): DOMElement<P, T>;  
  
// Custom components  
  
function createElement<P extends {}>(  
type: FunctionComponent<P>,  
props?: Attributes & P | null,  
...children: ReactNode[]): FunctionComponentElement<P>;  

function createElement<P extends {}>(  
type: ClassType<P, ClassicComponent<P, ComponentState>, ClassicComponentClass<P>>,  
props?: ClassAttributes<ClassicComponent<P, ComponentState>> & P | null,  
...children: ReactNode[]): CElement<P, ClassicComponent<P, ComponentState>>;  

function createElement<P extends {}, T extends Component<P, ComponentState>, C extends ComponentClass<P>>(  
type: ClassType<P, T, C>,  
props?: ClassAttributes<T> & P | null,  
...children: ReactNode[]): CElement<P, T>;  

function createElement<P extends {}>(  
type: FunctionComponent<P> | ComponentClass<P> | string,  
props?: Attributes & P | null,  
...children: ReactNode[]): ReactElement<P>;


```

#### 매개변수

- type:  type 인수는 유효한 React 컴포넌트여야 한다. 태그 이름 또는 React 컴포넌트가 올 수 있다.
- props: props는 객체 또는 선택적 속성이다. React는 전달한 props와 일치하는 프로퍼티를 가진 엘레멘트를 생성한다. props 객체의 key와 ref는 특수하기 때문에 생성한 element에서 props 프로퍼티로 사용할 수 없다. 대신 element.ref 또는 element.key로 사용가능 하다.
- 선택사항: `...children`: 0개 이상의 자식 노드. React 엘리먼트, 문자열, 숫자, [포탈](https://ko.react.dev/reference/react-dom/createPortal), 빈 노드(`null`, `undefined`, `true`, `false`) 그리고 React 노드 배열을 포함한 모든 React 노드가 될 수 있다.

#### 반환값

createElement는 아래 프로퍼티를 가지는 React Element객체를 반환한다.

- type: 전달받은 type
- props: ref와 key를 제외한 전달받은 props type이 type.defaultProps를 가지는 컴포넌트라면, 누락되거나, 정의되지 않은 props는 type.defaulteProps 값을 가져온다.
- ref: 전달받은 `ref`. 누락된 경우 `null`.
- key: 전달받은 `key`를 강제 변환한 문자열. 누락된 경우 `null`.

#### 주의사항

- 반드시  React 엘리먼트와 그 프로퍼티는 불변하게 취급해야한다. 엘리먼트 생성 후 에는 그 내용이 변경되어선 안된다. 개발환경에서 React는 이를 강제하기 위해 반환된 엘리먼트와 그 프로퍼티를 얕게 freeze 한다.
- 사용자 컴포넌트를 렌더링하기위해선 태그를 반드시 대문자로 시작해야한다.
- children의 경우 정적인 경우에만 여러 인수로 전달해야한다. 그렇지고 않고 매 렌더링마다 동적으로 변환다면 ReactNodeList 형태로 전달해주고 key를 설정해주어야 한다.

>  React 엘리먼트란?
>  
>  엘리먼트는 사용자 인터페이스의 일부에 대한 표현이다. . 예를 들어 `<Greeting name="Taylor" />`와 `createElement(Greeting, { name: 'Taylor' })`는 모두 다음과 같은 객체를 생성한다.

``` js
// 약간 단순화됨  

{  

	type: Greeting,  
	props: {  
		name: 'Taylor'  
	},  
	key: null,  
	ref: null,  

}
```

> 리액트 엘리먼트는 React가 컴포넌트를 렌더링하도록 지시하는 설명서와 비슷하다. App 컴포넌트에서 이 객체를 반환함으로써 React에게 다음 할 일을 지시할 수 있다.
> 
> 엘리먼트 생성 비용은 매우 저렴하므로 엘리먼트 생성을 최적화하거나 피하려고 노력할 필요가 없다.
#### 바벨은 jsx 코드를 js코드로 변환한다.

> **`runtime: 'automatic'`**  
> React 17+에서는 JSX가 `React.createElement`를 명시적으로 호출하지 않고 `jsx`, `jsxs` 함수를 사용합니다. 이 옵션을 활성화하면 `import React from 'react'`를 생략할 수 있습니다.

- **Babel**과 **ESBuild**는 모두 JSX를 컴파일하여 React 코드가 동작할 수 있도록 준비합니다.
- **React 16 이하**: `React.createElement` 스타일을 지원하는 ESBuild 설정을 사용할 수 있습니다.
- **React 17 이상**: Babel과 `runtime: 'automatic'` 설정을 사용하면 더 깔끔하고 효율적인 JSX 트랜스폼이 가능합니다.
- ESBuild는 간단한 환경에 적합하고, Babel은 복잡하거나 유연한 설정이 필요한 프로젝트에 적합합니다.

`vite.config.ts` 파일은 Vite가 제공하는 **ESBuild의 설정을 래핑**하고, 이를 통해 JSX를 트랜스파일하는 과정을 제어할 수 있습니다. 따라서 **ESBuild와 관련된 설정**은 `vite.config.ts`의 `esbuild` 옵션에서 직접 처리하면 됩니다.

```ts
export default defineConfig({ plugins: [ tsconfigPaths(), ], esbuild: { jsx: 'transform', jsxDev: false, jsxImportSource: '@/libs/jsx', jsxInject: `import { createElement } from '@/libs/jsx/jsx-runtime'`, jsxFactory: 'createElement', }, });

```

- [https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html](https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
- `esbuild` 옵션에서 `jsx`를 `'transform'`으로 설정하여 JSX를 트랜스파일링합니다.
- `jsxImportSource`를 설정하여 사용자 정의 JSX 런타임을 지정합니다.
- `jsxInject`를 통해 모든 JSX 파일에 자동으로 `import { jsx } from '@/libs/jsx/jsx-runtime'`를 추가합니다.
- `jsxFactory`를 `'jsx.createElement'`으로 설정하여 JSX가 `jsx.createElement` 함수로 변환되도록 합니다.

- Vite와 Babel을 사용하여 개발 환경을 어떻게 구축하셨나요?
    - Vite를 선택한 이유와 그 장점은 무엇인가요?
        **Webpack** ↔ **Vite**는 **빠른 개발 서버**와 **효율적인 번들링**을 제공하는 도구로, 특히 모듈 핫 리로딩(HMR)이 매우 빠릅니다. 다음과 같은 이유로 선택했습니다:
        - **ES 모듈 기반 빌드**: Vite는 최신 브라우저의 ES 모듈 지원을 활용하여 빠르게 파일을 제공.
        - **빠른 스타트업 시간**: Webpack과 비교하여 초기 번들링 시간이 훨씬 짧음.
        - **플러그인 에코시스템**: Rollup 기반으로 확장성이 뛰어남.
- JSX란 무엇이며, 왜 React에서 사용하는지 설명해 주세요.
    
    - 의견
        **JSX란 무엇인가요?**
        JSX(JavaScript XML)는 **JavaScript의 확장 문법**으로, UI를 선언적으로 작성할 수 있게 해줍니다. HTML과 유사한 문법으로 작성할 수 있어 직관적이고 가독성이 높습니다.
        
        **React에서 사용하는 이유**
        
        - **가독성 향상**: UI 구조를 한눈에 이해 가능.
        - **컴포넌트 기반 개발**: HTML-like 코드를 JavaScript에서 직접 작성 가능.
        - **Virtual DOM과의 통합**: JSX는 내부적으로 React의 createElement 함수로 변환되어 Virtual DOM 객체를 생성합니다.
    - JSX는 어떻게 자바스크립트 코드로 변환되나요?
        
        JSX는 Babel과 같은 트랜스파일러를 통해 **React의** createElement **함수 호출**로 변환됩니다.
        
- 프로젝트에서 Babel을 어떻게 설정하여 JSX를 트랜스파일링했나요? Babel이란 것은 무엇인가요?
    
    - **Babel이란?**
        - Babel은 **JavaScript 트랜스파일러**로, 최신 JavaScript 코드를 오래된 브라우저에서도 실행할 수 있도록 트랜스파일링합니다.
    - 설정하신 옵션들의 역할은 무엇인가요?
        - jsx 문법을 js 코드로 변환하는 역할
- `createElement` 함수는 어떻게 구현하셨나요?
    
    ```jsx
    function createElement(type, props, ...children) {
      return {
        type,
        props: {
          ...props,
          children: children.map(child =>
            typeof child === "object" ? child : { type: "TEXT_ELEMENT", props: { nodeValue: child, children: [] } }
          )
        }
      };
    }
    ```
    
- Virtual DOM이란 무엇이며, 실제 DOM과의 차이점은 무엇인가요?
    - Virtual DOM은 **메모리 내에서 가상으로 생성되는 DOM 객체 트리**입니다. UI 상태를 추적하고 업데이트를 효율적으로 수행하기 위해 사용됩니다.
    - **Virtual DOM**: 메모리에서 동작하며, 업데이트 시 빠른 비교(diffing) 후 실제 DOM에 최소한의 변경을 적용. → React Batching, useState의 setState를 이용해서 상태를 변경할 때 한번에 모아서 업데이트 하는 것
    - **실제 DOM**: 브라우저가 렌더링하는 실제 UI 구조. DOM 업데이트는 상대적으로 느림.
    - 브라우저 렌더링
        - 레이아웃 → 페인팅
        - **reflow** → **repaint**
- JSX 코드가 어떻게 Virtual DOM 객체로 변환되는지 설명해 주세요.
- Virtual DOM과 실제 DOM의 차이점을 설명해 주세요. 프레임워크에서 Virtual DOM을 사용하는 이유는 무엇인가요?
    - **성능 최적화**: 전체 DOM 업데이트 대신 필요한 부분만 변경.
    - **추상화**: 개발자는 DOM 조작 대신 React의 선언적 UI를 작성
