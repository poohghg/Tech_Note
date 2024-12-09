createElement를 사용하면 React 엘레먼트를 생성할 수 있다. JSX를 작성하는 대신 사용할 수 있다.

``` js
const element = createElement(type, props, ...children)
```

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

createElement는 아래 프로퍼티를 가지는 React 엘레멘트 객체를 반환한다.

- type: 전달받은 type
- props: ref와 key를 제외한 전달받은 props type이 type.defaultProps를 가지는 컴포넌트라면, 누락되거나, 정의되지 않은 props는 type.defaulteProps 값을 가져온다.
- ref: 전달받은 `ref`. 누락된 경우 `null`.
- key: 전달받은 `key`를 강제 변환한 문자열. 누락된 경우 `null`.

#### 주의사항

- 반드시  React 엘리먼트와 그 프로퍼티는 불변하게 취급해야한다. 엘리먼트 생성 후 에는 그 내용이 변경되어선 안된다. 개발환경에서 React는 이를 강제하기 위해 반환된 엘리먼트와 그 프로퍼티를 얕게 freeze 한다.
- 사용자 컴포넌트를 렌더링하기위해선 태그를 반드시 대문자로 시작해야한다.
- children의 경우 정적인 경우에만 여러 인수로 전달해야한다. 그렇지고 않고 매 렌더링마다 동적으로 변환다면 ReactNodeList 형태로 전달해주고 key를 설정해주어야 한다.

>  React 엘레먼트란?
>  
>  엘리먼트는 사용자 인터페이스의 일부에 대한 표현이다. . 예를 들어 `<Greeting name="Taylor" />`와 `createElement(Greeting, { name: 'Taylor' })`는 모두 다음과 같은 객체를 생성합니다.

```
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