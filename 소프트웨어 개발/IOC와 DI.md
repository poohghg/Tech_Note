## IOC(제어의 역전)

- 전통적인 방식은 컴포넌트가 직접 인스턴스를 생성하고 제어함.
- IOC는 제어의 흐름을 외부로 위임함, 컴포넌트는 동작만 정의하고, 의존선 주입(DI)을 통해 필요한 객체를 외부에서 전달받음
- IOC의 대표적인 구현방식이 DI (Dependency Injection)임

### DI(의존성 주입)

- 컴포넌트가 직접 의존성을 생성하지 않고, 외부에서 주입받는 패턴
- 즉 동작과 책임을 외부로 두어 설계하는게 핵심이다.
	- eg. 모달의 경우 들어갈 동작, UI, 컨텐츠를 모두 외부에서 주입한다.
- 기존 컴포넌트의 역할은 컨텍스트와 구조를 제공한다.
	- UI의 레이아웃 구조를 정의한다.
	- 행위의 컨테스트와 상태를 제공한다.
	- 외부에서 주입된 동작과 내용을 표현할 자리를 제공한다.
- 장점: 테스트 용이성, 유연한 모듈 교체, 느슨한 결합

#### 컴포넌트의 핵심 책임

| 구분        | 설명                                            |
| --------- | --------------------------------------------- |
| 구조 제공     | UI의 배치를 정의하고, 어디에 무엇이 들어갈지 정함                 |
| 컨텍스트 제공   | `useContext`, `Provider`로 내부 컴포넌트에게 상태와 제어 제공 |
| 동작 제어     | 열기/닫기 등 제어 흐름만 책임지고, 구체 로직은 외부에 위임            |
| 주입된 기능 수용 | 외부에서 전달된 `children`, `render`, `action` 등을 표현 |

``` tsx 
//컨텍스트 제공

import { createContext, ReactNode, useContext, useState } from "react";

interface ModalContextType {
  isOpen: boolean;
  openModal: () => void;
  closeModal: () => void;
}


const ModalContext = createContext<ModalContextType | undefined>(undefined);


export function ModalProvider({ children }: { children: ReactNode }) {
  const [isOpen, setIsOpen] = useState(false);

  const openModal = () => setIsOpen(true);
  const closeModal = () => setIsOpen(false);

  return (
    <ModalContext.Provider value={{ isOpen, openModal, closeModal }}>
      {children}
    </ModalContext.Provider>
  );
}
export function useModal() {
  const context = useContext(ModalContext);
  if (!context) {
    throw new Error("useModal must be used within a ModalProvider");
  }
  return context;
}

// 사용

import { useModal, ModalProvider } from "./ModalContext";


function ModalTrigger() {
  const { openModal } = useModal();
  return <button onClick={openModal}>Open Modal</button>;
}
```


```


``` tsx
// components/ListLayout.tsx
import { ReactNode } from "react";

interface ListLayoutProps<T> {
  items: T[];
  children: (item: T) => ReactNode; // 👈 렌더링 전략을 주입
}

export function ListLayout<T>({ items, children }: ListLayoutProps<T>) {
  return (
    <ul className="grid grid-cols-2 gap-4">
      {items.map((item, index) => (
        <li key={index} className="p-4 border rounded-xl shadow-sm">
          {children(item)}
        </li>
      ))}
    </ul>
  );
}

```

``` tsx
// components/UserList.tsx
import { useEffect, useState } from "react";
import { fetchUsers } from "../service/UserService";
import { ListLayout } from "./ListLayout";

export function UserList() {
  const [users, setUsers] = useState<{ id: number; name: string }[]>([]);

  useEffect(() => {
    fetchUsers().then(setUsers);
  }, []);

  return (
    <ListLayout items={users}>
      {(user) => <div className="text-blue-600">👤 {user.name}</div>}
    </ListLayout>
  );
}


```






