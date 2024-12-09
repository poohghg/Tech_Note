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
- props: props는 객체 또는 선택적 속성이다. React는 전달한 props와 일치하는 프로퍼티를 가진 엘레멘트를 생성한다. props 객체의 key와 ref 특수하기 때문에 생성한 element에서 props 프로퍼티로 사용할 수 없다. 대신 element.ref 또는 element.key로 사용가능 하다.
- 선택사항: `...children`: 0개 이상의 자식 노드. React 엘리먼트, 문자열, 숫자, [포탈](https://ko.react.dev/reference/react-dom/createPortal), 빈 노드(`null`, `undefined`, `true`, `false`) 그리고 React 노드 배열을 포함한 모든 React 노드가 될 수 있다.

#### 반환값

createElement는 아래 프로퍼티를 가지는 React 엘레멘트 객체를 반환한다.

- type: 전달받은 type
- props: ref와 key를 제외한 전달받은 props type이 type.defaultProps를 가지는 컴포넌트라면, 누락되거나, 정의되지 않은 props는 t