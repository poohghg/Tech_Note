<u>**자바스크립트는 명령형,함수형,프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어이다.**</u>

**<u>자바스크립트는 객체기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 원식타입을 제외한 모든값들은 객체로 구성되어 있다.</u>**



자바스크립트의 모든 객체는 객체간 상속을 위해 object라는 내부의 prototype 가지고 있다.

- 자바스크립트의 내장된 기본 상태 및 함수이다.
- 직접 접근은 불가하지만, 내부슬롯의 경우 `__proto__` 접근자 프로퍼티를 통해 간접적으로 접근할 수 있다.

자바스크립트에서 객체간 상속의 연결고리는 프로토타입 체인으로 연결되어 있다

- 배열 -> array -> object



자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 속성을 기본값으로 자동정의한다.

#### 데이터 프로퍼티: 키와값으로 구성된 일반적인 프로퍼티.

- value: 객체속성의 값
- writable: 값을 변경할수 있을지의 유무(값의 갱신 가능 여부)
- enumerable: 속성을 열거가능한지
  - -false 인경우 in, Object 문의 열거함수로 열거 할 수없다.

- configurable: 속성이 삭제될수 있는지유무(재정의 가능 여부)

```javascript
const obj1 = {
  name: 'kwon',
  age: 31,
  desc() {
    return `age:${this.age},name:${this.name}`;
  },
};
// 객체의 권한을 수정해서 객체를 관리 할 수 있다.
Object.defineProperties(obj1, {
  name: {
    writable: false,
  },
  age: {
    value: 29,
    enumerable: false,
  },    
}); 
/**
{
  name: {
    value: 'kwon',
    writable: false,
    enumerable: true,
    configurable: true
  },
  age: { value: 29, writable: true, enumerable: false, configurable: true },
  desc: {
    value: [Function: desc],
    writable: true,
    enumerable: true,
    configurable: true
  }
*/
console.log(Object.getOwnPropertyDescriptors(obj1));


```

#### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할때 사용하는 접근자 함수로 구성된다.

- ```javascript
  // 접근자 프로퍼티
  function accesoorProtp() {
    const user = {
      name: 'kwon',
      age: 31,
      get ageInfo() {
        return `${this.age}`;
      },
      set ageInfo(age) {
        this.age = age;
      },
    };
  
    // setter 호출
    user.ageInfo = 33;
    // getter 호출
    console.log(user.ageInfo);
  }
  ```

#### 프로퍼타입

프로퍼타입은 어떤 객체의 상위 객체의 역할을 하는 객체다. 프로퍼타입은 하위 객체에게 자신의 프로퍼티와 메서드를 상속한다. 하위 객체는 상위 프로퍼타입의 프로퍼티 또는 메서드를 사용 할 수 있다. 프로퍼타입 체인은 단방향 링크드 리스트 형태로 구성되어 있다. 객체나 메서드에 접근하려 할때 해당 객체의 프로퍼티 또는 메서드가 없다면, 프로퍼타입 체인을 따라 상위 프로퍼타입을 검색한다.

프로토타입 확인해보기

- ```javascript
  function checkProtoLevel(params) {
    function Info(name, age) {
      this.name = name;
      this.age = age;
      // 인스턴스 레벨의 함수
      // 각각의 객체마다 함수를 생성해서 가지고 았다.
      // 메모리상 비효율적.
      // this.printinfo = function () {
      //   return `name:${this.name}/age:${this.age}`;
      // };
    }
    // 프로토타입의 함수를 생성하면, 프로타타입레벨의 함수를 가질수 있다.
    Info.prototype.printUser = function () {
      return `name:${this.name}/age:${this.age}`;
    };
    const kwon = new Info('kwon', 31);
    const kim = new Info('kim', 27);
    console.log(kwon);
  }
  ```

  

- <img src="/Users/khg/Library/Application Support/typora-user-images/image-20220607230608685.png" alt="image-20220607230608685"  />

  #### 객체변경방지

  - ```javascript
    // 오브제트 프리징 -> 값의 읽기만 가능 나머지는 모두불가.
    // 객체를 동결시킨다.
    function immutabilityObj() {
      // 오브젝트 동결시키기
      const obj = {
        name: 'kwon',
        age: 31,
        inner: {
          a: 'a',
          b: 'b',
        },
      };
      // Prevents the modification of existing property attributes and values, and prevents the addition of new properties.
      // 기존 프로퍼티의 속성(키)과 값을 수정(삭제,쓰기,재정의)을 방지하고, 새로운 속성을 추가하는것을 방지한다.
    
      Object.freeze(obj);
      // age: { value: 31, writable: false, enumerable: true, configurable: false }
      // console.log(Object.getOwnPropertyDescriptors(obj));
      // 무시!!!!
      delete obj.name;
      obj.name = 'hi!';
      obj.plus = '+';
      // freeze도 얕은복사이다. 내부객체는 변경가능하다.
      // freeze시 내부 객체의 경우 같은 참조값을 보고있다. 참조 객체의 값을 수정하면 같이 변경된다.
      obj.inner.a = 'AA';
      // -> { name: 'kwon', age: 31, inner: { a: 'AA', b: 'b' } }
      console.log(obj.iss);
      console.log(obj);
    }
    ```

  - ```javascript
    // 오브제트 seal -> 기본 오브젝트의 형태를 유지하지만, 값의 수정은 가능하다.
    function sealObj() {
      const obj = {
        name: 'kwon',
        age: 31,
        inner: {
          a: 'a',
          b: 'b',
        },
      };
      //Prevents the modification of attributes of existing properties, and prevents the addition 	of new properties.
      // 속성은 변경불가 !! 값의 수정은 가능하다.
      Object.seal(obj);
      obj.name = 'HI!';
      delete obj.name;
      // -> { name: 'HI!', age: 31, inner: { a: 'a', b: 'b' } }
      console.log(obj);
      // seal되었는지 확인하다.
      // console.log(Object.isSealed(obj));
    }
    ```

  - 중첩객체(얕은방지)로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다. 재귀적으로 호출하여 freeze하자. 



#### 생성자 함수

객체 리털리에 의한 객체생성 방식은 단 하나의 객체만 생성한다. 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야하기에 비효울적이다.

생성자함수 : 객체(인스턴스)를 생성하는 함수.

일반함수와 같이 정의하고, new 연산자와 함께 생성자 함수를 호출한다.

- 인스턴스 생성과 this바인딩: 암묵적으로 빈 객체를 생성한다. -> 인스턴스는 this에 바인딩된다.
- 다음 과정은 함수실행 이전 런타임에 실행된다.

인스턴스 초기화 

- this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 할당하여 초기화하거나 고정값을 할당한다.

인스턴스 반환

- 생성자 함수의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
- 만약 this가 아닌 다른 객체를 반환하면 this가 반환되지 못하고 retun문에 명시한 객체가 반환된다.
- `생성자 함수에 명시적으로 return문을 사용하는것은 기본 동작을 훼손하다. 지양해야할 부분이다.` 

```javascript
// 기본 생성자 함수.
function constructorFunction() {
  function Info(name, age) {
    const constant = 'constant';
    this.name = name;
    this.age = age;
    this.greeting = '안녕하세요';
    this.printInfo = function () {
      return `${greeting} \n 이름은:${this.name}이고, 나이는:${this.age}입니다.`;
    };
    
    // return 문을 명시하면 명시된 값이 출력된다.
    // ->{ name: 'kwon', age: 31 }
    // return { name, age };
  }
  const myInfo = new Info('kwon', 31);
  // this에 바인딩한 값들이 출력된다.
  //   Info {
  //   name: 'kwon',
  //   age: 31,
  //   greeting: '안녕하세요',
  //   printInfo: [Function (anonymous)]
  // }
  console.log(myInfo);
}
```

##### this 란?

|        함수 호출 방식        |                 this가 가르키는것                 |
| :--------------------------: | :-----------------------------------------------: |
|      일반 함수로서 호출      | 전역 객체 브라우저 환경: window/ node환경: global |
| 메서드로서 호출(객체 메서드) |               메서드를 호출한 객체                |
|     생성자 함수로서 호출     |           생성자 함수가 생성할 인스턴스           |

#### 내부 메서드 call과 construct

- 함수는 객체이지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출 할 수 있다.

- 함수가 일반 함수로서 호출되면 함수객체의 내부 메서드 call이 호출된다.

  - new 연산자와 함께 생성자 함수로써 호출되면 내부 메서드 construct가 호출된다.

- 모든 함수는 callable이지만 모든 함수 객체가 constructor인 것은 아니다.

- ```javascript
  function isConstructorFunction() {
    //  위 세가지 표현식만이 생성자 함수이다.
    function foo() {}
    const constFoo = function () {};
    // 프로퍼티의 값으로 할당된 함수는, 메서드가 아니다. 일반함수로 정의된 것이다.
    // 함수
    const obj = {
      x: function () {},
    };
    const newObj = new obj.x();
  
    // 생성자 함수가 아닌 함수
    // 생성자 함수가 아닌 함수를 new 연산자와 함께 사용하면 에러가 발생한다.
    const obj1 = {
      // 메서드
      met() {},
    };
    // 애로우 함수
    const t = () => console.log('t');
  }
  
  ```

- 일반 함수를 new 연산자와 사용하여 this또는 객체를 반환하지 않는다면 빈객체가 반환된다.

- New 연산자 없이 생성자 함수로 호출하면 일반 함수로 호출된다.
 
```javascript
  function newTarget() {
    function Info(name, age) {
      //  new 연산자없이 호출되었는지 확인하여 없이 호출하였다면, 재귀 호출한다.
      if (!new.target) {
        return new Info(name, age);
      }
      // 스코프 세이프 생성자 팬턴
      // new연산자와 함께 호출되면 this와 샘성자함수 객체는 프로토타입에 의해 연결된다.
      // 만약 일반함수처럼 호출된다면, this는 전역객체가 소유하고 있다.
      if (!(this instanceof Info)) {
        return new Info(name, age);
      }
      
      this.name = name;
      this.age = age;
      this.print = function () {
        return `이름은:${this.name}이고, 나이는:${this.age}입니다.`;
      };
    }
    // new연산자 없이 사용
    const user1 = Info('kim', 31);
    console.log(user1);
  }
  ```

#### 상속

상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 그대로 상속받아 사용할 수 잇는 것을 말한다.자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

모든 객체는 prototype 이라는 내부슬롯이 있고,이 내부 슬롯의 값은 프로토타입의 참조값이다.

```javascript
function checkProtoLevel(params) {
  function Info(name, age) {
    this.name = name;
    this.age = age;
    // 인스턴스 레벨의 함수
    // 각각의 객체마다 함수를 생성해서 가지고 았다. -> 메모리상 동일한 함수가 각각의 인스턴스에서 사용된다.
    this.printinfo = function () {
      return `name:${this.name}/age:${this.age}`;
    };
  }
  
  // 프로토타입의 함수를 생성하면, 프로타타입객체 레벨의 함수를 생성할 수 있다.
  // 이는 프로토타입 프토퍼티에 바인딩되어 사용할 수 있다.
  Info.prototype.printUser = function () {
    return `name:${this.name}/age:${this.age}`;
  };
  const kwon = new Info('kwon', 31);
  const kim = new Info('kim', 27);
  // 각각의 인스턴스는 다른 객체이다.
  // -> false
  console.log(kwon === kim);
  // 그러나 프로토타입 레벨의 함수는 공유되어 사용된다.
  // -> true
  console.log(kwon.printUser === kim.printUser);
  // 오버라이딩또한 가능하다.
  // -> 안녕하세요!! name:kwon/age:31
  kwon.printUser = function () {
    return `안녕하세요!! name:${this.name}/age:${this.age}`;
  };
}
```

각각의 인스턴스는 생성자 함수에 의해 prototype 속성을 상속받는다. 

![image-20220612224510412](/Users/khg/Library/Application Support/typora-user-images/image-20220612224510412.png)

##### 프로토타입 객체

프로토타입 객체란 객체지행 프로그래밍의 근간을 이루는 객첵 간 상속을 구현하기 위해 사용된다. 프로토타입은 어떤 객체의 상위(부모)객체의 역할을 하는 객체로서 다른객체에 공유 프로퍼티를 제공한다. 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

- 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다



접근자 프로퍼티 `__proto__ `는 object.prototype의 프로퍼티다. 모든 객체는 상속을 통해 접근자 프로퍼티를 사용할 수 있다.

- get/set으로 이루어줘 있음

- Object.setPrototypeOf(obj, 상속받을 obj)을 통해 프로터타입을 교체 할 수 있다.

  

prototype 프로퍼티(속성)는 생성자 함수가 생성할 인스턴스 객체의 프로토타입을 가리킨다. **<u>따라서 생성자 함수만이 prototype을 소유한다.</u>**

|             구분             | 소유        | 값                | 사용주체    | 사용 목적                                                    |
| :--------------------------: | ----------- | ----------------- | ----------- | ------------------------------------------------------------ |
| `__proto__ ` 접근자 프로퍼티 | 모든객체    | 프로토타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용      |
|      prototype 프로퍼티      | Constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체 인스턴스의 프로토타입을 할당하기 위해 사용 |

모든 프로토타입은 constructor프로퍼티를 갖는다. 이 constructor 프로퍼티는 프로토타입 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.이 연결은 생성장 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄진다.

- constructor가 가리키는 것은 생성자 함수이고, 생성자 함수는 인스턴스를 생성한 생성자 함수이다.

- 생성자 함수에는 자기자신의 프로퍼티가 정의되어 있고, 생성자함수에 의해 생성된 인스턴스는 prototype 프로퍼티을 통해 부모 속성을 사용할 수 있다.

리터럴 표기법으로 의해 생성된 객체는 생성자 함수로 생성된게 아닌다. 하지만 constructor와 연결되어 있다.

프로토타입 체인: 자바스크립트는 객체의 프로퍼티에 접근하려 할 때 해당 객체에 접근하려는 프로퍼타가 없다면, 내부슬롯 참조에 따라 자신의 부모 역할을 하는 프로퍼타입의 프로퍼티를 순차적으로 검색한다.

#### 정적 프로퍼티

생성자 함수 또한 객체임으로 자기자신의 속성값을 가질수 있다 이를 정적프로퍼티라 한다.인스턴스는 프로토타입 체인에 속하지 않는 정적메서드에는 접근이 불가하다.

```javascript
function testStatic(params) {
  function Obj(name) {
    const staticA = 'A';
    this.name = name;
  }
  Obj.staticB = 'B';
  const obj1 = new Obj('test1');
  // -> obj1.staticA:"undefined
  console.log(`obj1.staticA:"${obj1.staticA}`);
  // -> obj1.staticA:"undefined
  console.log(`obj1.staticb:"${obj1.staticB}`);
  // -> test1
  console.log(`obj1.staticb:"${obj1.name}`);
}
```

 