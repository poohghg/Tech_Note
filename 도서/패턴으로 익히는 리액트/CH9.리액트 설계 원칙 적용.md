> 설계 원칙은 소프트웨어 개발에서 시간이 지나도 코드베이스의 유지보수성, 확장성, 그리고 가동성을 보장하기 위한 기본 원칙이다. 끊임없이 변화하는 기술 환경 속에서도 설계 원칙을 준수한다면 프로젝트의 장기적인 성공을 이끌어 낼 수 있으며, 버그가 빈번하게 발생하는 `코드 지옥` 도 피할 수 있다.

> 리액트 애플리케이션은 라이브러리의 선언전 특성과 컴포너트 기반의 구조로 인해 설계 원칙이 더욱 중요하다.
> `리액트는 작고 독립된 컴포넌트로 복잡한 UI를 만들 수 있다. 이러한 모듈화된 접근 방식은 리액트의 강점`이지만, 설계 원칙을 지키지 않으면 관리하기 어려운 코드가 되기 쉽다.

> 설계 원칙을 따르지 않으면 복잡한 의존관계로 인해 코드를 변경하기 어렵거나 읽기 어려워진다.
> 예를 들어 SRP(단일 책임 원칙)를 무시하면 컴포넌트가 너무 많은 역할을 하게 되어 유지보수가 어려워진다.
> ISP(인터페이스 분리 원칙)를 무시하면 불필요한 props가 컴포넌트에 전달되어 성능 저하와 혼란을 초래할 수 있다.

> 소프트웨어 개발에서 중요한것은 기술 부채를 해결하는 것보다 기능 개발, 버그 수정, 비지니스 가치 전달 등 본질에 집중할 수 있는 환경을 만드는 것이다.
> 
> 리액트에서 설계 원칙의 적용은 단순한 방법론을 넘어 필수적인 요소이다. 이는 복잡성에 대응하기 위한 사전적인 조치로, 코드를 읽고 테스트하고 유지보수하기 쉽게 만든다.

### 1. 단일 책임 원칙 (SRP: Single Responsibility Principle)

> 단일 책인 원칙의 핵심은 컴포넌트의 주요 역할을 정확히 파악하는 것이다. 컴포넌트가 수행해야 할 핵심 기능을 분리하면, 부가적인 기능들을 리팩터링하고 추상화하기 쉬워진다.

> 가장 많이 사용되는 2가지 기술은 render prop과 합성이다. render prop은 리액트 컴포넌트 간 코드 공유를 위해 함수 prop을 이용하는 기술입니다. render prop으로 구현된 컴포넌트는 자체적으로 렌더링 로직을 구현하는 대신 리액트 요소를 반환하고 이를 호출하는 함수를 사용합니다. 반면에 합성은 작고 재사용 가능한 컴포넌트들을 만들어 이들을 조합해 더 복잡한 UI 요소를 만드는 기술입니다.

#### Render Prop 패턴

> 리액트에서 render Prop 패턴은 prop으로 함수를 컴포넌트에 전달한다. 이 함수는 컴포넌트가 렌더링할 내용을 결정하는 데 사용된다. 이를 통해 컴포넌트는 자신의 상태나 로직을 캡슐화하면서도, 렌더링되는 내용을 외부에서 제어할 수 있다.
> 
> 위 함수는 JSX를 반환하며, 컴포넌트 일부 영역의 렌더링을 맡는다. 이패턴은 부모 컴포넌트가 자식 컴포넌트의 렌더링 로직을 제어할 수 있게 하므로 컴포너틑를 더욱 유연하게 재사용할 수 있다.


```tsx

const Title = ({
 title,
 render,
}: {
 title: string;
 render: (s: string) => React.ReactNode;
}) => <div>{render("TILTE" | title)}</div>;


<Title
 title="This is a title"
 render={(s: string) => {
 const formatted = s.toUpperCase();
 return <h3>{formatted}</h3>;
 }}
/>
```

``` tsx

const Title = ({
 title,
 children,
}: {
 title: string;
 children: (s: string) => React.ReactNode;
}) => <div>{children("TILTE" | title)}</div>;


<Title title="This is a title">
 {(s: string) => {
 const formatted = s.toUpperCase();
 return <h3>{formatted}</h3>;
 }}
</Title>

// 결과

<div>
 <h3>TILTE | THIS IS A TITLE</h3>
</div>
```

> 중요한것은 추상화이다. render prop 패턴을 사용하면 컴포넌트의 핵심 기능을 유지하면서도, 렌더링 로직을 외부에서 제어할 수 있다. 이를 통해 컴포넌트는 단일 책임 원칙을 준수하면서도 다양한 상황에 대응할 수 있다.

> render prop을 통한 합성은 핵심 로직을 변경하지 않고 컴포넌트 동작을 확장하거나 사용자에 맞게 지정할 수 있는 방법이다. 각 컴포넌트가 하나의 역할을 잘 수행하기 때문에 컴포넌트를 깔금하게 모듈화하며 테스트 하게 쉽게 유지한다.

#### 합성을 통한 단일 책임 원칙 저용

> 합성은 여러 작은 컴포넌트를 조합하여 더 복잡한 UI를 만드는 리액트의 강력한 기능이다. 합성을 통해 각 컴포넌트가 단일 책임 원칙을 준수하도록 설계할 수 있다. 시스템은 각 부분이 제 역할을 잘 수행할 때 잘 작동한다.
> 
> 합성은 컴포넌트의 재사용성을 높이고, 유지보수를 쉽게 하며, 코드의 가독성을 향상시킨다. 각 컴포넌트가 하나의 역할에 집중할 수 있기 때문에, 시스템 전체의 복잡성을 줄이는 데 도움이 된다.

``` tsx
type AvatarProps = {
 name?: string;
 role?: string;
 url: string;
};
const Avatar = ({ name, role, url }: AvatarProps) => {
	 if (name) {
		 return (
			 <Tooltip name={name} role={role}>
				 <div className="rounded">
					 <img src={url} alt={`${name}'s profile`} />
				 </div>
			 </Tooltip>
		 );
	 }
	 
	 return (
		 <div className="rounded">
			 <img src={url} alt="" />
		 </div>
	 );
};


```

> Avatar 컴포넌트의 원래 코드는 Tooltip 기능과 밀접하게 연관되어 있다. 사용자가 보다 세부적인 툴팁의 기능을 원한다면, 이렇게 강하게 연결되어 있는 코드를 유지하기 쉽지 않다. prop을 추가하여 툴팁의 기능을 수정하기 위해서는 Avatar 컴포넌트 코드를 변경해야 하므로 관리하기 까다로워진다.

``` tsx
const Avatar = ({name = "", url}: AvatarProps) => (  
    <div className="rounded">  
      <img src={url} alt={name} title={name}/>  
    </div>  
);  
  
const ToolTip = ({text, children}: { text: string; children: React.ReactNode }) => (  
    <div className="group relative inline-block">  
      {children}  
      <div  
          className="pointer-events-none mt-0.5 m-auto w-max rounded text-center bg-gray-900 px-2 py-1 text-xs text-white opacity-0 transition-opacity group-hover:block group-hover:opacity-100"  
      >        {text}  
      </div>  
    </div>  
);
```


``` tsx
const MyAvatar = () => (
 <Tooltip name="Juntao Qiu" role="Software Engineer">
	 <Avatar
	 name="Juntao Qiu"
	 url="https://avatars.githubusercontent.com/u/122324"
	 />
 </Tooltip>
);

```

> 위 예제에서 `MyAvatar` 컴포넌트는 `Tooltip`과 `Avatar` 두 개의 컴포넌트를 합성하여 만든다. 각 컴포넌트는 자신의 역할에 충실하며, `MyAvatar`는 이들을 조합하는 역할만 한다. 이를 통해 각 컴포넌트가 단일 책임 원칙을 준수하면서도, 복잡한 UI를 구성할 수 있다.

### 2. 의존성 역전 원칙 (DIP: Dependency Inversion Principle)

> 의존성 역전 원칙은 고수준 모듈이 저수준 모듈에 의존하지 않도록 하는 것이다. 대신, 둘 다 추상화에 의존해야 한다. 리액트에서 이 원칙을 적용하는 한 가지 방법은 컨텍스트 API를 사용하는 것이다. 컨텍스트는 컴포넌트 트리 전체에 데이터를 전달할 수 있는 방법을 제공하여, props를 통해 여러 단계로 데이터를 전달하는 것을 피할 수 있다.
> 
> DIP는 유지보수 가능하며 유연하고 확장 가능한 소프트웨어를 만들기 위해 필요한 원칙중 하나이다. 이 원칙은 구체적인 구현보다는 추상화에 초점을 맞춘다.
> 
> DIP는 대규모 시스템을 구축하고 유지 관리할 때 직면하는 문제들을 해결한다. 그중 하는는 결합도가 높은 모듈로 인해 발생하는 문제이다. 상위 레블의 모듈이 하위 레벨의 모듈에 의존관계를 맺게 되면, 하위 모듈을 변경할 때 상위 모듈도 영향을 받게 된다. 이는 시스템의 복잡성을 증가시키고 유지보수를 어렵게 만든다.

#### 의존관계 역전 원칙의 원리

> DIP는 두 가지 주요 원칙으로 구성된다.
> 
> 1. 고수준 모듈은 저수준 모듈에 의존해서는 안 된다. 둘 다 추상화에 의존해야 한다.
> 2. 추상화는 구체적인 사항에 의존해서는 안 된다. 구체적인 사항은 추상화에 의존해야 한다.


``` tsx

interface NotificationAPI {  
  send(message: string, severity: "info" | "warning" | "error"): void;  
}  
  
class EmailNotification implements NotificationAPI {  
  // 이메일 관련 설정 및 초기화 코드  
  send(message: string, severity: "info" | "warning" | "error"): void {  
    console.log(`Email sent with ${severity} severity: ${message}`);  
  }  
}  
  
class Application {  
  private notifier: NotificationAPI[]  
  
  constructor(notifier: NotificationAPI[]) {  
    this.notifier = notifier;  
  }  
  
  process(  
      message: string,  
      severity: "info" | "warning" | "error" = "info"  
  ) {  
    this.notifier.forEach(n => n.send(message, severity));  
  }  
}  
  
const emailNotifier = new EmailNotification();  
const app = new Application([emailNotifier]);  
app.process(  
    "This is a warning message",  
    "warning"  
)
```

> send 메서드를 가지는 Notification 인터페이스를 정의하고, 이를 준수하는 EmailNotification 클래스를 구현한다. Application 클래스는 Notification 인터페이스를 준수하는 클래스를 통해 인스턴스를 생성할 수 있다. process 메서드에서 Application은 이 인스턴스 객체를 사용하여 알림을 보냔다. 이러한 설정은 Application 클래스를 특정 알림 동작으로부터 분리하여, 유연하게 기능을 확장하거나 변경할 수 있게 해준다.

``` d
classDiagram
    %% 추상화 (슈퍼클래스 역할)
    class NotificationAPI {
      <<interface>>
      +send(message: string, severity: "info" | "warning" | "error"): void
    }

    %% 구체 클래스 (저수준 모듈)
    class EmailNotification {
      +send(message: string, severity: "info" | "warning" | "error"): void
    }

    %% 고수준 모듈
    class Application {
      -notifier: NotificationAPI[]
      +process(message: string, severity: "info" | "warning" | "error"): void
    }

    %% 관계 정의
    NotificationAPI <|.. EmailNotification : implements
    Application --> NotificationAPI : depends on
```

#### 명령과 조회 책임 분리 원칙 ( CQRS: Command Query Responsibility Segregation)

> 명령과 조회 책임 분리 원칙(CQRS)는 소프트웨어 설계에서 ==메서드나 함수는 시스템의 상태를 수정하는 명령이거나 시스템 상태에 대한 정보를 조회하여 반환하는 쿼리 둘 중에 하나여야 하며, 2가지가 동시에 수행되지 않았야== 한다는 원칙이다.

1. 명령(Command): 시스템의 상태를 변경하는 작업을 수행한다. 예를 들어, 상태를 업데이트하거나 새로운 데이터를 추가하는 작업이 이에 해당한다.
	 - 명령 또는 수정 메서드는 액션 또는 객체의 상태 변경을 수행하며 값을 반환하지 않는다.
2. 조회(Query): 시스템의 상태를 변경하지 않고, 현재 상태에 대한 정보를 반환하는 작업을 수행한다. 예를 들어, 상태 및 데이터를 읽어오는 작업이 이에 해당한다.
	-  상태를 변경 없이 읽기만 하며, 상태에 대한 정보를 반환한다.

> CQRS 원칙은 시스템의 복잡성을 줄이고, 유지보수성을 향상시키며, 성능을 최적화하는 데 도움이 된다. 명령과 조회를 분리함으로써 각 작업에 대한 최적화가 가능해지고, 시스템의 확장성도 향상된다. 명령과 조회 메서드는 서로 독립적으로 진화할 수 있다.

``` tsx
class UserService {
 private users: { id: number; name: string }[] = [];

 // 명령 메서드: 사용자 추가
 addUser(id: number, name: string): void {
   this.users.push({ id, name });
 }

 // 조회 메서드: 사용자 목록 반환
 getUsers(): { id: number; name: string }[] {
   return this.users;
 }
}
```

> 위 예제에서 `addUser` 메서드는 사용자를 추가하는 명령 메서드이고, `getUsers` 메서드는 사용자 목록을 반환하는 조회 메서드이다. 이 두 메서드는 서로 독립적으로 동작하며, 각각의 책임이 명확하게 구분되어 있다.


