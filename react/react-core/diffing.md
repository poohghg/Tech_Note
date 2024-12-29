#### diiffing 알고리즘이란?
- Virtual DOM에서는 실제 DOM과 Virtual DOM을 비교하여 변경된 부분만을 실제 DOM에 반영하는데,
이때 변경된 부분을 찾아내는 알고리즘이 Diffing 알고리즘이다.
- 엘레먼트의 태그 또는 키값이 변경된 경우라면 해당 노드를 포함한 하위의 모든 노드를 언마운트 한 다음 새로운 virtualDom으로 대체한다.


- diff 알고리즘 구현
	- 이전 Virtual DOM과 새로운 Virtual DOM의 변경된 부분만 업데이트 하도록 적용
	- 휴리스틱 알고리즘으로 구현( O(n^3))
		- 변경될 부분만 찾아서 변경
			- 엘레멘트 타입,키,props을 비교하여 변경된 부분을 업데이트
			-  **key** 는 부모 트리 내 자식 노드간 비교
	- diff 함수는 재귀적으로 동작 변경된 props를 수정,삭제 하기위해

- render 및 rerender 함수 변경
	- render 함수시 현재 커밋될 Virtual DOM을 저장
	- rerender 과정에서 Virtual DOM 트리구성 시 diff 알고리즘으로 변경될 부분만 업데이트 후 커밋 되도록 변경
	- 커밋 후 runEffects useEffect)
