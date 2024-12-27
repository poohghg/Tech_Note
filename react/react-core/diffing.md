#### diiffing 알고리즘이란?
- Virtual DOM에서는 실제 DOM과 Virtual DOM을 비교하여 변경된 부분만을 실제 DOM에 반영하는데,
이때 변경된 부분을 찾아내는 알고리즘이 Diffing 알고리즘이다.
- 엘레먼트의 태그 또는 키값이 변경된 경우라면 해당 노드를 포함한 하위의 모든 노드를 언마운트 한 다음 새로운 virtualDom으로 대체한다.


