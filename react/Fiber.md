> 참조
> [React 파이버 아키텍처 분석](https://d2.naver.com/helloworld/2690975)
> [Lin Clark - A Cartoon Intro to Fiber - React Conf 2017](https://www.youtube.com/watch?v=ZCuYPiUIONs&t=1334s)


React는 16버전부터 파이버라 불리는 새로운 코어 아키텍처를 채택했다. 이는 기존의 스택 기반 알고리즘을 완전히 새롭게 작성한 것이며 리액트18.2 버전까지 지속적으로 업데이트 및 리팩터링되고 있다.

### 복습

- JSX 선언된 표현식은 React 엘리먼트 객체로 치환된다.
	- React 엘리먼트는 객체이다.
- JSX 표현식은 createElement 함수(_JSX) 함수를 통해 React 엘리먼트로 치환된다.
	- 참조 [[createElement]]
- qus