createElement를 사용하면 React 엘레먼트를 생성할 수 있다. JSX를 작성하는 대신 사용할 수 있다.

``` js
const element = createElement(type, props, ...children)
```

`type`, `prop`, `children`를 인수로 제공하고 `createElement`을 호출하여 React 엘리먼트를 생성

#### 매개변수
- type:  type 