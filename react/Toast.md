`react-hot-toast`에서 `import { toast } from 'react-hot-toast';`를 사용하여 전역에서 상태를 업데이트하는 원리를 구체적인 코드로 보여드리겠습니다.

이 라이브러리의 핵심은 **일반 JavaScript 모듈**을 사용하여 **전역 상태**를 관리하고, **Observable(관찰 가능한 객체)** 패턴을 통해 리액트 컴포넌트와 통신하는 것입니다.

---

## 1. 전역 상태 관리 및 발행 (Publisher)

다음 코드는 리액트 컴포넌트 외부에 있는 순수한 JavaScript 파일(`toast-store.js`)을 모방합니다. 이 파일은 토스트 메시지 배열을 저장하고, 컴포넌트가 구독할 수 있는 **발행자(Publisher)** 역할을 합니다.

### `toast-store.js` (가상의 상태 관리 모듈)

JavaScript

```
// 토스트 메시지 배열을 저장하는 전역 변수
let toasts = [];

// 구독자 함수 목록 (상태가 변경될 때 호출될 함수들)
const subscribers = new Set();

// 구독자에게 상태 변경을 알리는 함수 (발행)
function notify() {
  // 모든 구독자에게 현재 토스트 배열을 전달하여 상태가 변경되었음을 알립니다.
  subscribers.forEach(callback => callback(toasts));
}

// 💡 toast 객체의 API 함수 (상태를 변경)
export const toast = {
  // 새로운 토스트를 추가하고 알림을 발행
  success: (message) => {
    const newToast = { id: Date.now(), message, type: 'success' };
    toasts = [...toasts, newToast];
    notify(); // 상태 변경을 구독자에게 알림
  },
  error: (message) => {
    const newToast = { id: Date.now(), message, type: 'error' };
    toasts = [...toasts, newToast];
    notify();
  },
  // 토스트를 닫고 알림을 발행
  dismiss: (id) => {
    toasts = toasts.filter(t => t.id !== id);
    notify();
  }
};

// 💡 구독 관련 함수 (컴포넌트가 상태를 받을 수 있도록 함)
export function subscribe(callback) {
  subscribers.add(callback);
  // 구독 해지 함수를 반환
  return () => subscribers.delete(callback);
}

// 이 파일에서 'toast' 객체와 'subscribe' 함수가 export 됩니다.
```

이 파일에서 `export const toast`를 가져오는 것이 바로 `import { toast } from 'react-hot-toast';`에 해당합니다. **이 객체는 리액트 컴포넌트가 아니므로 어디서든 호출 가능합니다.**

---

## 2. 구독 및 렌더링 (Subscriber)

다음 코드는 리액트 컴포넌트(`Toaster`와 `useToasts`)를 모방합니다. 이들은 위에서 정의한 `toast-store.js`의 상태를 **구독(Subscribe)**하고, 상태 변경이 있을 때마다 자신을 **리렌더링**하여 화면을 업데이트합니다.

### `useToasts.js` (가상의 사용자 정의 훅)

JavaScript

```
import React, { useState, useEffect } from 'react';
import { subscribe } from './toast-store'; // 💡 발행자의 subscribe 함수 가져오기

// 토스트 배열을 구독하고 반환하는 사용자 정의 훅
function useToasts() {
  // 컴포넌트 내부의 리액트 상태 (화면을 렌더링하는 주체)
  const [currentToasts, setCurrentToasts] = useState([]);

  useEffect(() => {
    // 💡 1. 상태 저장소에 구독자 함수(setCurrentToasts)를 등록
    // 상태 저장소가 변경될 때마다 이 함수가 호출되어 리액트 상태를 업데이트합니다.
    const unsubscribe = subscribe(newToasts => {
      setCurrentToasts(newToasts); // 리액트 상태 업데이트 -> <Toaster> 리렌더링 발생
    });

    // 💡 2. 컴포넌트 언마운트 시 구독 해지
    return () => unsubscribe();
  }, []); // 컴포넌트 마운트 시 한 번만 실행

  return currentToasts;
}

// 💡 <Toaster /> 컴포넌트 (UI 렌더링 담당)
export const Toaster = () => {
  const toasts = useToasts(); // 훅을 사용하여 최신 토스트 배열을 가져옴

  return (
    <div style={{ position: 'fixed', top: 20, right: 20 }}>
      {toasts.map(t => (
        <div 
          key={t.id} 
          style={{ 
            padding: '10px', 
            margin: '5px', 
            background: t.type === 'error' ? 'red' : 'green',
            color: 'white'
          }}
        >
          {t.message}
          <button onClick={() => toast.dismiss(t.id)}>X</button>
        </div>
      ))}
    </div>
  );
};
```

---

## 3. 요약 및 핵심

1. **전역 JavaScript 모듈:** `toast` 함수는 리액트의 라이프사이클과 독립적인 **순수 JavaScript 모듈**에 존재합니다. 이는 어디서든 호출 가능하게 만듭니다.
    
2. **상태 변경 및 알림:** `toast.success()` 호출 시, 이 모듈 내부의 `toasts` 배열을 변경하고 **`notify()` 함수를 호출**하여 상태 변화를 '발행(Publish)'합니다.
    
3. **구독과 리렌더링:** 애플리케이션에 배치된 `<Toaster />` 컴포넌트 내부의 `useToasts` 훅은 **`subscribe()` 함수를 통해 이 알림을 '구독(Subscribe)'**합니다. 알림을 받으면 훅 내부의 `useState`가 업데이트되어 `<Toaster />` 컴포넌트가 **리렌더링**되고, 화면에 토스트가 나타납니다.
    

이 구조 덕분에 개발자는 **컴포넌트 트리에 얽매이지 않고** `toast` API를 호출하는 것만으로 전역 UI를 업데이트할 수 있습니다.