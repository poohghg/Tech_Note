> 자바스크립트의 과도한 연산으 한 프레임에 주언지 16ms의 시간을 초과하면 프레임 드랍을 발생시킨다. 브라우저는 하나의 메인 콜 스택에서 렌더링과 관련된 대부분의 작업을 처리하므로 자바스크립트가 메인 콜 스택에 실행되는 동안에는 다른 작업을 할 수가 없다. 따라서 하나의 프레임에서 처리되는 자바스크립트의 양을 적절하게 조절하는 방법이 필요하다.

![[Pasted image 20260804152958.png|506]]

- 메인 스레드에서 실행되는 연산들을 적절히 나누어 효율적으로 실행하는 겨우
	- setTimeout, setInterval, requestAnimationFrame, requestIdleCallback 등을 활용하여 연산을 나누어 실행
- 무거운 연산을 별도의 스레드에서 실행하게 한 후 결과를 메인 스레드로 넘겨주는 경우 
	- 서비스 워커(Service Worker), 웹 워커(Web Worker)를 활용할 수 있다.

#### requestAnimationFrame

> window.requestAnimationFrame() 메서드는 브라우저에게 애니메이션을 업데이트할 준비가 되었음을 알려주는 메서드이다. ==다음 리페인트 바로 전에 브라우저가 애니메이션을 업데이트할 지정된 함수를 호출하도록 요청==한다.

![[Pasted image 20260804153348.png|477]]

```javascript
function animate() {
  // 애니메이션 로직 수행
  // ...

  // 다음 프레임에 animate 함수를 호출
  requestAnimationFrame(animate);
}

```

> 브라우저는 다음 리페인트를 수행하기 전에 rAF에 등록된 콜백 함수를 실행한다. 메인 스레드에서 현재 실행되는 프레임 작업을 방해하지 않아 rFA를 사용하면 메인 스레드의 작업을 방해하지 않고 간단한