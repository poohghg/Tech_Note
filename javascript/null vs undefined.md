### null

**`null`** 은 JavaScript의 [원시 값](https://developer.mozilla.org/ko/docs/Glossary/Primitive) 중 하나로, 어떤 값이 의도적으로 비어있음을 표현하며 불리언 연산에서는 [거짓](https://developer.mozilla.org/ko/docs/Glossary/Falsy)으로 취급합니다.

null은 리터럴로써 null이라 쓰인다. null은 undefined과 같이 글로벌 객체의 속성에 대한 식별자가 아니다. 대신 null은 식별되지 않은 것을 표현한다. 즉 변수가 아무런 객체를 가르키지 않음을 표현한다. null은 종종 관련된 객체가 존재하지 않을 때 그 객체 대신 사용된다.

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


> https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/null