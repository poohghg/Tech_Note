
> 출처
> 
> - [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/null)
> - https://inpa.tistory.com/entry/%F0%9F%93%9A-null-undefined-NaN

### null

**`null`** 은 JavaScript의 [원시 값](https://developer.mozilla.org/ko/docs/Glossary/Primitive) 중 하나로, 어떤 값이 의도적으로 비어 있음을 표현하며 불리언 연산에서는 [거짓](https://developer.mozilla.org/ko/docs/Glossary/Falsy)으로 취급한다.

null은 리터럴로써 null이라 쓰인다. null은 undefined과 같이 글로벌 객체의 속성에 대한 식별자가 아니다. 대신 null은 식별되지 않은 것을 표현한다. ==즉 변수가 아무런 객체를 가르키지 않음을 표현한다==. null은 종종 관련된 객체가 존재하지 않을 때 그 객체 대신 사용된다.

보통 null은 값이 없는 상태라고 표현되지만, 정확히 null은 어느 reference변수에 대해 주소값이 없는 것을 표현하기 위한 키워드이다. 프로그래밍적으로 명시적으로 부재를 나타내고 싶다면 항상 null을 사용하는 것이 좋다.

- 객체 속성값을 초기화
- 함수의 매개변수의 초기화

```
// 정의되지 않고 초기화된 적도 없는 foo
foo; //ReferenceError: foo is not defined

// 존재하지만 값이나 자료형이 존재하지 않는 foo
var foo = null;
foo; //null
```

### undefined

undefined은 javaScript의 원시 자료형 하나이며, 전역 객체의 속성이다. 즉 전역 범위에서의 변수이다. 값을 할당하지 않은 변수는 undefined 자료형이다. 메서드나 선언도 평가할 변수가 값을 할당받지 않은 경우에 undefined을 반환한다. 함수가 값을 명시적으로 반환하지 않으면 undefined을 반환한다.

- 반한 값이 없는 return문은 암시적으로 undefined을 반환
- 존재하지 않는 객체 속성에 접근 하면 undefined가 반환
- 초기화가 없는 변수 선언은 변수를 undefined로 암시적으로 초기화
- 내장 API 중 요소를 찾을 수 없을 때 undefined을 반환

개념적으로, `undefined`는 값이 없음을 의미하고, `null`은 객체가 없음을 의미한다. ([`typeof null === "object"`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/typeof#typeof_null)에 대한 변명이 될 수도 있다. 일반적으로 값이 없는 경우 언어의 기본값은 `undefined`다.

### null 과 undefined 의 차이

null 과 undefined 의 차이점은 메모리 측면에서 설명될 수 있다.
undefined은 변수가 초기화 되지 않았거나, 객체의 속성이 존재하지 않는 등의 경우에 자동으로 할당 되는 값으로, 이때의 변수는 메모리에 존재하지만 값이 없기 때문에 크기가 매우 작다.

반면 null은 개발자가 의도적으로 값이 없음을 할당한 경우에 사용 되는 값으로, 이때의 변수는 빈 객체를 가르키는 객체포인터 이기 때문에 주소값을 나중에라도 받기 위해 크기가 있다.


``` js
typeof null; // "object" (하위호환 유지를 위해 "null"이 아님)
typeof undefined; // "undefined"
null === undefined; // false
null == undefined; // true
null === null; // true
null == null; // true
!null; // true
isNaN(1 + null); // false
isNaN(1 + undefined); // true
```





