> 참조
> [React 파이버 아키텍처 분석](https://d2.naver.com/helloworld/2690975)
> [Lin Clark - A Cartoon Intro to Fiber - React Conf 2017](https://www.youtube.com/watch?v=ZCuYPiUIONs&t=1334s)


React는 16버전부터 파이버라 불리는 새로운 코어 아키텍처를 채택했다. 이는 기존의 스택 기반 알고리즘을 완전히 새롭게 작성한 것이며 리액트18.2 버전까지 지속적으로 업데이트 및 리팩터링되고 있다.

### 복습

- JSX 선언된 표현식은 React 엘리먼트 객체로 치환된다.
	- React 엘리먼트는 객체이다.
- JSX 표현식은 createElement 함수(_JSX) 함수를 통해 React 엘리먼트로 치환된다.
	- 참조 [[createElement]]
- 변경된 부분만 업데이트 하기
	- React DOM은 업데이트 시 변경된 부분만 native DOM에 반영한다.
	- 이는 내부 diff 알고리즘을 통해 변경된 부분만 업데이트 후 실제 DOM에 반영한다.
		- 참조 [[diffing]]
	- 이전 React Element와 타입,키,Props가 다르면 업데이트가 진행된다.
		- 부모 내 자식의 형제 중 key가 동일할때는 이전 React Element를 사용한다.
- 재조정(Reconciliation)
	- 
	- 루트 엘리먼트의 타입이 다르면 이전 트리를 버리고 완전히 새로운 트리를 구축한다.