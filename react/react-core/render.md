Virtual DOM을 실제 DOM으로 렌더링하기
#### render 함수
- `render` 함수는 Virtual DOM 객체를 입력으로 받아 실제 DOM 노드를 생성
- 재귀적으로 VDOM 트리를 순회하며 각 노드에 대해 DOM 노드를 생성하고, 부모 노드에 추가
- 텍스트 노드, 함수형 컴포넌트, 일반 요소를 구분하여 처리

