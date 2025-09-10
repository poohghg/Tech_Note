### 1. 단일 책임 원칙

  > 단일 책임 원칙(Single Responsibility Principle, SRP)은 소프트웨어 설계 원칙 중 하나로, 각 클래스나 모듈이 하나의 책임만을 가져야 한다는 개념이다. 즉, 하나의 클래스나 모듈이 변경되어야 하는 이유가 하나뿐이어야 한다는 것이다.
  > 
  > 리액트 컴포넌트는 변경해야 할 이유가 단 하나여야 한다. 예를 들어, 컴포넌트가 사용자 인터페이스(UI)를 렌더링하는 책임을 가지고 있다면, 그 컴포넌트는 UI와 관련된 변경에만 영향을 받아야 한다. 데이터 처리나 상태 관리와 같은 다른 책임은 별도의 컴포넌트나 훅으로 분리하는 것이 좋다.
  > 
  > 하나의 컴포넌트는 하나의 작업이나 기능만을 수행하는 것이 이상적이다. 
  > 이 원칙을 따르면 코드 가독성이 높아지고, 유지보수와 테스트 및 디버깅도 쉬워진다.
  
``` tsx

const BlogPost = ({id}:{id:ing}) => {
  const [post, setPost] = useState(null);
  const [isLiked, setIsLiked] = useState(false);
  
  useEffect(() => {
    fetchPost(id).then(data => setPost(data));
  }, [id]);

  if (!post) return <div>Loading...</div>;

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
      <button onClick={() => setIsLiked(!isLiked)}>
        {isLiked ? 'Unlike' : 'Like'}
    </div>
  );
};
})
  
```

위 코드는 단일 책임 원칙을 위반하고 있다. `BlogPost` 컴포넌트는 데이터 페칭과 UI 렌더링, 그리고 좋아요 상태 관리 까지 3가지 동작을 하나의 컴포넌트에서 수행하기 때문이다.

```tsx

const userFetchPost = async (id: string) => {
  const response = await fetch(`/api/posts/${id}`);
  const data = await response.json();
  return data;
};

const usePost = (id: string) => {
  const [post, setPost] = useState(null);

  useEffect(() => {
    userFetchPost(id).then(data => setPost(data));
  }, [id]);

  return post;
};

const LikeButton = () => (
  const [isLiked, setIsLiked] = useState(false);
  const onToggle = () => setIsLiked(!isLiked);
  
  return <button onClick={onToggle}>{isLiked ? 'Unlike' : 'Like'}</button

);

const BlogPost = ({id}:{id:ing}) => {
  const post = usePost(id);

  if (!post) return <div>Loading...</div>;

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
      <LikeButton />
    </div>
  );
};

```

### 2. 중복 배제 원칙

> 중복 배제 원칙 (Don't Repeat Yourself, DRY)은 소프트웨어 개발에서 동일한 코드나 로직을 여러 번 반복하지 않는다는 원칙이다. 중복된 코드는 유지보수와 확장성을 어렵게 만들고, 버그 발생 가능성을 높인다.
> 
> 코드 안에서 중복을 줄이는 것이 목적이다.
> 이 원칙을 지키면 유지보수성과 가독성이 높아지고 테스트하기 쉬워지며 로직 중복으로 인해 발생하는 버그를 방지 할 수 있다.
> 
> 유지보수하기 쉽고 재사용성을 높인 접근법이다.
> 변경은 한 곳에서만 일어나므로 버그가 발생할 가능성이 줄어든다. 기능 변경을 한 곳에서 해결할 수 있기 때문에 유지보수가 단순해질 수 있다.
> 
> 동일한 기능과 역할을 하는 컴포넌트를 하나의 컴포넌트로 통합하는 것이 좋다.

### 3. 합성 활용하기

> 합성(Composition)은 여러 개의 작은 컴포넌트를 조합하여 더 큰 컴포넌트를 만드는 방법이다. 합성을 활용하면 코드의 재사용성이 높아지고, 유지보수와 확장성이 좋아진다.

``` tsx
// 원본 코드 뭉쳐있는 패턴
function UserDashboard({ user, posts }: UserDashboardProps) {
  return (
    <div>
	    <h1> {user.name} </h1>
	    <img src={user.avatar} alt="profile" />
	    <h2> Friends </h2>
	    <ul>
        {user.friends.map((friend) => (
          <li key={friend}>{friend}</li>
        ))}
        </ul>
        <h2> Latest Posts </h2>
        {posts.map((post) => (
          <div key={post.author}>
            <h3>{post.author}</h3>
            <p>{post.summary}</p>
          </div>
        ))}
    </div>
  );
}


 
export default UserDashboard;

```

```tsx

import { UserDashboardProps } from "./types";
import { UserProfile } from "./UserProfile";
import { FriendList } from "./FriendList";
import { PostList } from "./PostList";

function UserDashboard({ user, posts }: UserDashboardProps) {
  return (
    <div>
      <UserProfile user={user} />
      <FriendList friends={user.friends} />
      <PostList posts={posts} />
    </div>
  );
}

export default UserDashboard;
```

- 관심사 분리: 컴포넌트의 여러 다른 영역들을 각각의 별도의 컴포넌트로 분리함으로써 각각의 컴포넌트가 하나의 역할을 당담하게 된다. 덕분에 각 컴포넌트가 더 명확해지고 이해하기 쉬워지고 유지보수도 쉬워진다.
- 더 나은 가독성: 컴포넌트가 더 작고 집중되어 있기 때문에 코드의 가독성이 향상된다. 각 컴포넌트가 무엇을 하는지 쉽게 파악할 수 있다.
- 높은 재사용성: 각 컴포넌트가 독립적으로 동작하기 때문에 다른 곳에서도 재사용하기 쉽다. 예를 들어, `UserProfile` 컴포넌트는 다른 페이지나 애플리케이션에서도 사용할 수 있다.
- 테스트의 용이함: 작고 하나의 택임을 가진 컴포넌트는 상호작용이 복잡하지 않고 의존하는 코드가 적어 테스트하기 쉽다.

>  내부에 상태와 로직을 가진 컴포넌트를 다루다 보면 합성은 더 복잡해질 것이다.
>  하지만 재사용성이 가능한 작은 컴포넌트를 결합하여 큰 컴포넌트를 만든다는 핵심 원칙을 동일하다.
>  
>  단순하고 하나의 기능을 가진 컴포넌트들의 합성을 통해 복잡한 사용자 인터페이스를 효과적으로 구조활 할 수 있다. 합성을 사용해서 단일 책임 원치과 중복 배제 원칙을 온전하게 구현할 수있다. 또한 이해와 유지보수가 쉽고 테스트가 용이한 정교한 UI를 만들수 있다.

> 컴포넌트를 분리한 뒤에, 합성을 통해 컴포넌트가 분리한 서브 컴포넌트를 prop으로 전달하면 컴포넌트를 사용하는 측에서 서브 컴포넌트를 prop을 통해 직정 제공하도록 하여 Prop Drilling 문제를 해결할 수 있다.

``` tsx
const MySimplePage = () => {
 return (
	 <Page
		 header={
		 <h1>A simple header</h1>
	 }
	 sidebar={
		 <aside>
			 <ul>
				 <li>Home</li>
				 <li>About</li>
			 </ul>
		 </aside>
	 }
	 main={<div>The main content</div>}
	 />
 );
```