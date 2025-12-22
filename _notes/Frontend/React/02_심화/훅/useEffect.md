## useEffect란?
리액트에서 컴포넌트가 렌더링 될 때마다 특정 작업을 실행하는 Hook으로, 주로 API 요청 후 데이터를 저장하거나, 외부 라이브러리 초기화, 이벤트 리스너 등록 및 제거 등에 사용된다.

---
## 용어
* `Mount` : 화면에 처음 렌더링되는 것
* `Update` : 다시 렌더링 되는 것
* `Unmount` : 화면에서 사라지는 것

## 특징
- **useEffect는 화면이 `렌더링`이 된 이후에 실행된다.**
- **의존성 배열`[]`을 통해 실행 시점을 제어할 수 있다.**

## 왜 사용해야 하는가?
JSON Placeholder API를 통해 요청을 했을 때, useEffect를 사용하지 않고 코드를 작성하면 React에서는 [[Axios]]로 데이터를 요청해서 화면을 렌더링하게 된다. 이때, useEffect를 쓰지 않으면 상태가 바뀔 때마다 다시 렌더링이 일어나고, 그때마다 또 요청을 보내서 결국 무한루프에 빠져 계속해서 API 요청을 보내게 될 것이다.
## 사용법
- **1. 마운트 시 한 번만 실행**
```jsx
useEffect(() => {
  // 실행할 코드
}, [의존성 배열]);
```
의존성 배열을 빈 배열로 설정하면 컴포넌트가 처음 마운트될 때만 실행된다.

- **2. 특정 값이 바뀔 때만 사용**
```jsx
const [count, setCount] = useState(0);

useEffect(() => {
  console.log("count가 바뀔 때마다 실행:", count);
}, [count]);
```
count가 변경될 때마다 useEffect가 실행된다. 의존성 배열에 있는 값이 바뀔 때만 작동한다.

- **3. 클린업 함수 사용 (언마운트 또는 재실행 전에 실행됨)**
```jsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log("1초마다 실행");
  }, 1000);

  // 정리(clean-up) 함수
  return () => {
    clearInterval(interval);
    console.log("컴포넌트가 언마운트되거나 의존성 변경 시 정리");
  };
}, []);
```
코드에서 setInterval이 1초마다 로그를 찍게 되는데, 이때 컴포넌트가 사라지거나(unmount) 다시 실행되기 전에 clearInterval로 타이머를 정리한다. 이를 [[클린업 함수]]라고 한다.

# 예시
아래는 JsonPlaceHolder에 요청을 보내 user 데이터의 모든 이름을 받는 코드이다.
```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

export default function JsonFetcher() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        setUsers(res.data);
      })
      .catch((err) => {
        console.log("오류 발생 :", err);
      });
  }, []);

  return (
    <>
      {users.length > 0 ? (
        <ul>
          {users.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      ) : (
        <p>유저 정보를 불러오는 중...</p>
      )}
    </>
  );
}

```

보다시피 useEffect를 사용하는 모습인데 [[Axios]] 요청을 보내는 모습인데, 만약 useEffect를 사용하지 않으면 어떻게 될까?

아래는 useEffect를 사용하지 않고 GET 요청을 보내는 코드이다.
```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

export default function JsonFetcher() {
  const [users, setUsers] = useState([]);

    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        setUsers(res.data);
      })
      .catch((err) => {
        console.log("오류 발생 :", err);
      },[]);

  return (
    <>
      <h1>{count}</h1>
      {users.length > 0 ? (
        <ul>
          {users.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      ) : (
        <p>유저 정보를 불러오는 중...</p>
  );
}
```

만약 useEffect를 사용하지 않고 외부 API에 요청을 보내게 된다면, 컴포넌트가 무한히 렌더링되어, 수많은 요청을 보내게 될 것이다. 아래 사진은 그 예시를 다룬 것이다.
![[Pasted image 20250903142944.png]]
사진과 같이 JsonFetcher 함수가 478번이나 동작하는 모습을 볼 수 있다.