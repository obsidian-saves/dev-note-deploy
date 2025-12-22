## useState란?
> React에서 가장 많이 사용되는 Hook 중 하나로, 함수형 컴포넌트에서 상태(State)를 관리할 때 사용된다.
> 여기서 상태는 컴포넌트 내에서 유지되어야 하는 데이터 정보를 의미하며, useState는 이 데이터를 저장하고 변경할 수 있도록 도와준다.

---

## 왜 사용해야할까?

React는 상태가 변경될 때 UI를 자동으로 업데이트하는 방식으로 동작한다.
하지만 함수형 컴포넌트는 기본적으로 렌더링될 때마다 함수가 다시 실행되기 때문에 내부 변수만으로는 상태를 유지할 수 없다.

## 특징 살펴보기 🔍

useState의 특징은 대표적으로 아래와 같다.

- **함수 컴포넌트에서 상태 관리**
  클래스 컴포넌트에서만 가능했던 상태 관리를 함수 컴포넌트에서도 가능하게 해준다.

- **자동 UI 업데이트**
  상태가 변경되면 React가 자동으로 해당 컴포넌트를 다시 렌더링하여 UI를 최신 상태로 유지해준다.

- **초기값 설정**
  useState를 호출할 때 초기 상태 값을 설정해줘야 한다.

---

## 기본 사용법

리액트에서 import를 통해 useState를 불러오고 컴포넌트 내부에서 사용하면 된다.

```jsx
import { useState } from "react";

const Component1 = () => {
  const [value, setValue] = useState(0); // ()안에는 0, "", null 등이 올 수 있다.
};
```

여기서

- `value`는 현재 상태 값
- `setValue`는 상태를 변경하는 함수
- `useState(0)`의 0은 초기값

**비유로 이해하기 (냉장고 비유)**

- `useState(0)`은 음식이 들어 있는 냉장고를 하나 갖는 것과 비슷하다.
- `value`는 현재 냉장고에 있는 음식의 수
- `setValue`는 음식을 넣거나 빼는 행위

초기값은 처음 렌더링 때만 사용되며 이후에는 무시된다.

---

## useState를 사용한 간단한 예시

아래는 useState를 활용하여 버튼을 클릭시 숫자 값이 바뀌는 코드이다.

```jsx
import { useState } from "react";
export default function Count() {
  const [count, setCount] = useState(0);
  return (
    <>
      <div>숫자 : {count}</div>
      <div>
        <button
          onClick={() => {
            setCount(count + 1);
          }}
        >
          증가
        </button>
        <button
          onClick={() => {
            setCount(count - 1);
          }}
        >
          감소
        </button>
        <button
          onClick={() => {
            setCount(0);
          }}
        >
          리셋
        </button>
      </div>
    </>
  );
}
```

---

## 주의점 ⚠️

아래는 아마도 당신이 useState를 사용하면서 다음과 같은 코드를 사용하거나, 문제점을 직면하게 될 것이다. 이 글을 통해 문제점을 예방하거나, 좀 더 보완된 코드를 작성하길 바란다.

### 너무 많은 useState 관리

아래의 코드처럼 여러가지 데이터를 관리하기 위해 useState를 사용한다 가정하자. 간단한 기능을 구현하는 것이라면 아래 코드처럼 작성해도 상관이 없겠지만, 만약 그 값이 10개, 100개가 된다고 생각해보자.

```jsx
function UserCard() {
  const [name, setName] = useState("");
  const [age, setAge] = useState(20);
  const [school, setSchool] = useState("");

  return (
    <div>
      <h1>이름: {name}</h1>
      <h2>나이: {age}</h2>
      <h3>학교: {school}</h3>
      <button
        onClick={() => {
          setName("rlaxogh76");
          setAge("20");
          setSchool("GBSW");
        }}
      >
        정보 변경
      </button>
    </div>
  );
}
```

코드처럼 유저에 대한 정보를 다루는 useState 같은 경우 setName 등을 통해 계속 바꿔줘야 한다. 나쁜 방식은 아니지만 그렇다고 추천하는 방식도 아니다.

이 코드를 아래와 같이 좀 더 깔끔하게 수정할 수 있다.

```jsx
function UserCardSquashed() {
  const [user, setUser] = useState({
    name: "",
    age: 20,
    school: "",
  });

  return (
    <div>
      <h1>이름: {user.name}</h1>
      <h2>나이: {user.age}</h2>
      <h3>학교: {user.school}</h3>
      <button
        onClick={() => {
          setUser({
            name: "rlaxogh76",
            age: 20,
            school: "GBSW",
          });
        }}
      >
        정보 변경
      </button>
    </div>
  );
}
```

useState 내에서 name, age, school를 다루면 한 번만 상태를 변경할 수 있다는 장점이 있다.

### 상태의 값을 직접 변경해서는 안된다.

useState의 반환 값의 첫 번째 요소는 `상태 값`이다. 그렇다면 만약 이 값을 직접 변경하게 된다면 어떤 변화가 생길까? 차례대로 알아보자.

먼저 리액트가 변경을 감지하지 못하여 리렌더링이 일어나지 않는다. 이는 useState만의 특징과 사용 이유를 아예 없애버리는 행위와 같다고 보면 된다.

또다른 문제점은 React는 상태 업데이트 시 불변성을 유지해야한다. 이는 기존 상태를 직접 수정하지 않고 새로운 상태 값을 반환해야 하는데, `객체(상태)나 배열`을 직접 수정하는 것은 이 규칙을 위반한다.

### 이전 상태를 기반으로 업데이트하여야 한다면 함수를 통해 업데이트하라.

```jsx
setNumber(number + 1);
```

위 코드처럼 `setCount`는 이전 상태인 `count`를 통해 1을 증가시킨다. 이는 전혀 문제가 없어보일 수도 있겠지만, 사실은 잠재적으로 큰 문제로 이어질 수 있는 코드에 해당된다.

왜 그럴까?

상태는 리액트의 `상태 업데이트 스케줄링`에 의해 즉각적으로 업데이트되지 않는다.

**상태 업데이트 스케줄링**이란?
==React가 상태(state)를 업데이트할 때, 즉시 처리하지 않고 일정을 조정해서 필요한 타이밍에 한꺼번에 처리하는 동작을 말한다.==

`상태(count)`는 렌더링이 일어난 이후 정해진 값으로 고정되어 있다. 리렌더링이 일어나는 순간 그 값이 변화하게 된다. 따라서 setCount를 통해 count의 값을 변화시켰다 하더라도 리렌더링이 일어나는 전까지는 해당 변화가 반영되지 않는다.

아래와 같은 코드가 있다고 가정해보자.

```jsx
setNumber(number + 1);
setNumber(number + 1);
setNumber(number + 1);
```

그냥 보기엔 "number는 3이 되겠지?"라고 생각할 수도 있다.
하지만 이는 `console.log`를 통해 값을 조회해보면 number는 1로 표시된다.
아까 말했던 것처럼 number는 렌더링이 된 이후 처음 설정된 값으로 고정되어 있기 때문이다.

만약 렌더링이 된 시점에 number가 0이였다면 세 줄 모두 `0 + 1`을 하는 셈이 되버린다.
React는 state를 업데이트를 하기 전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다린다.
따라서 setNumber는 호출이 완료된 이후에만 일어난다.

만약 이해가 안된다면 식당으로 비유를 해보자.  
![[Pasted image 20250826171631.png]]
음식점에서 주문을 받는 웨이터는 첫 번째 요리를 만들어달라고 손님이 요청한다고 해서 주방으로 달려가 음식을 요청하지 않는다.
대신 주문이 끝날 때까지 기다렸다가 주문을 변경하거나, 다른 테이블로 이동해 다른 주문을 받는다.

그렇다면 어떻게 해결할 수 있을까?

아래 코드를 확인해보면,

```jsx
setNumber((n) => n + 1);
setNumber((n) => n + 1);
setNumber((n) => n + 1);
```

setNumber안에 또다른 함수를 넣어서, `setNumber(number + 1)`처럼 다음 state 값을 전달하는 대신, 위 코드와 같이 이전 큐의 state를 기반으로 다음 state를 계산하는 함수를 전달하도록 만들었다. 여기서 `n => n + 1`는 리액트에서 `업데이터 함수(Updater function)`이라고 부른다.

그렇다면 만약, setNumber(number + 1)과 setNumber(n => n + 1)를 같이 사용하게 된다면 무슨 일이 벌어질까?

아래는 두 개를 같이 사용한 코드이다.

```jsx
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 5);
          setNumber((n) => n + 1);
        }}
      >
        Increase the number
      </button>
    </>
  );
}
```

아까 보았던 것처럼 setNumber은 먼저 (0 + 5)를 진행하게 된다. 이후 업데이터 함수를 통해 기존의 값인 5에 +1를 추가하여 렌더링을 통해 결과적으로 6을 만들어낸다.

## 정리 📦

- useState는 상태를 관리하고 UI를 자동 업데이트하게 해주는 핵심 Hook이다.
- 상태는 즉시 변하지 않고 스케줄링되므로, 이전 상태 기반 변경 시 updater 함수를 사용해야 한다.
- 여러 개의 관련 상태는 객체로 묶어 관리하는 것이 더 효율적이다.

React 상태를 정확히 이해하면 예측 가능한 UI를 만들 수 있다.
