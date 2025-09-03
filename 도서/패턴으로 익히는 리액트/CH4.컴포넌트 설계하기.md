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

위 코드는 단일 책임 원칙을 위반하고 있습니다. `BlogPost` 컴포넌트는 데이터 페칭과 UI 렌더링, 그리고 좋아요 상태 관리 까지 3가지 동작을 하나의 컴포넌트에서 수행하기 때문이다.

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
  <button onClick={onToggle}>
    {isLiked ? 'Unlike' : 'Like'}
  </button>
);


```

