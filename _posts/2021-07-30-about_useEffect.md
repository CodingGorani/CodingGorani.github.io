---
title: "useEffect에 대하여"
categories:
  - TIL
tags:
  - TIL
  - React Hook
  - useEffect
---

# useEffect 제대로 이해하기

이 글은 [React공식문서](https://ko.reactjs.org/docs/hooks-effect.html)와 Dan Abramov의 [useEffect 완벽가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)를 읽고 정리한 글입니다. 질문 또는 수정이 필요한 내용이 있으면 댓글로 남겨주세요!
{: .notice--info}

## useEffect 이해하기 위한 기본 지식

useState를 통해 설정된 값이 렌더링 시마다 독립적인 값이며, 렌더링 내부의 독립적인 값이다.

**즉 특정 렌더링 시 그 내부에서 props와 state값은 영원히 같은 상태로 유지된다.** 리액트 컴포넌트는 프롭스나 상태값이 변경되면 다시 렌더링이 이루어진다. 그래서 그 때마다 함수가 새로 호출된다. <span style="color:navy">(생각해보면 사실 너무나도 당연한 자바스크립트의 동작방식이다!)</span>

```javascript
// 처음 랜더링 시
function Counter() {
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + 0);
    }, 3000);
  }
  // ...
  <button onClick={handleAlertClick} />; // 0이 안에 들어있음
  // ...
}

// 클릭하면 함수가 다시 호출된다
function Counter() {
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + 1);
    }, 3000);
  }
  // ...
  <button onClick={handleAlertClick} />; // 1이 안에 들어있음
  // ...
}

// 또 한번 클릭하면, 다시 함수가 호출된다
function Counter() {
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + 2);
    }, 3000);
  }
  // ...
  <button onClick={handleAlertClick} />; // 2가 안에 들어있음
  // ...
}
```

## useEffect가 클래스 컴포넌트의 마운트/업데이트/언마운트와 다른 점

useEffect로 마운트/업데이트/언마운트처럼 동작하게 만들 수 있지만 이 둘은 다르다. 아예 다르게 접근하는게 좋다.

- **리엑트는 지정한 props와 state에 따라 DOM과 동기화한다.**

  아까 위에서 언급했듯 props와 state가 달라지면 컴포넌트의 렌더링이 새로 이뤄진다. 따라서 렌더링시 Mount, Update의 구분이 없다.

- **useEffect는 기본적으로 첫번째 렌더링과 그 이후의 모든 업데이트에서 수행된다.**

- **리액트는 컴포넌트를 렌더링할 때 이용한 useEffect를 모두 기억했다가 DOM을 렌더,업데이트한 이후(브라우저를 모두 그린 이후)에 실행한다.**

  따라서 리엑트는 effect가 수행되는 시점에 이미 DOM이 업데이트 되어있음을 보장한다. 대부분의 이펙츠가 UI렌더링을 가로막지 않기 때문에 앱을 빠르게 만들어 준다.

  컴포넌트가 첫번째로 렌더링 할 때와 그 후에 다르게 동작하는 effects를 작성하고자 한다면 흐름을 거스르게 된다.

  반복문, 조건문, 중첩된 함수 내부가 아닌, 최상위 레벨에서만 리엑트훅을 호출해야 한다는 hook의 규칙에서 그 이유를 알 수 있다.

  하나의 컴포넌트에서 state나 Effect훅을 여러 개 쓸 수 있다. 이 때 React는 Hook이 호출되는 순서에 의존한다.

  따라서 특정 state가 어떤 useState 호출에 해당하는지 알 수 있다.

  이펙츠 중 하나를 조건문 안에 사용한다고 가정해보자.

  첫 렌더링과 다르게 조건문이 거짓이 되어 해당 훅을 이후 렌더링 시 건너뛴다 가정하면, Hook호출 순서가 달라진다.

  리액트는 예상과는 다른 결과에 그 시점 부터 버그를 발생시키게 된다.

## clean-up

effect를 정리하지 않으면 메모리 누수나 버그가 발생하는 경우가 있다. 예를 들어 외부데이터를 구독해 친구의 온라인 상태를 불러오는 경우가 그렇다. 클래스 컴포넌트를 사용할 때에는 componentDidMount에서 구독하고 componentWillUnmount에서 이를 정리하는 로직을 사용했다. 비슷한 역할을 하는 effect를 마운트/언마운트에 따로 적어야 했다. 불편하고 복잡하다.
{: .text-justify}

하지만 훅을 이용하면 같은 분류의 역할을 하나에 묶을 수 있다.

```javascript
import React, {useStatus, useEffect} form 'react';

function FriendStatus(props) {

  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    //구독하는 이펙트
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    //반환하는 함수는 클린 업 메커니즘
    return () => {
      ChantAPI.unsubsubscribeToFriendStatus(props.friend.id, handleStatusChange);
    };
  });
// props.friend.id를 의존성 배열에 넣지 않는 이유는 그것의 변화가 없더라도 온라인 오프라인 상태가 변경될 때를 포착(?)해야 하기 때문이다.

  if(isOnline === null) {
    return 'Loading...'
  }
  return isOnline ? 'Online' : 'Offline';
}
```

위 코드처럼 구독, 정리하는 로직을 useEffect안에 가까이 묶어둘 수 있다. 관심사를 분리하기 위해 useEffect를 여러 번 사용할 수도 있다.

### 실제 클린업이 동작하는 방식

클린업이 동작하는 방식을 이해하려면 위에서 언급한 useEffect가 동작하는 방식을 다시 볼 필요가 있다.

- **리엑트는 지정한 props와 state에 따라 DOM과 동기화한다.**
- **useEffect는 기본적으로 첫번째 렌더링과 그 이후의 모든 업데이트에서 수행된다.**
- **리액트는 컴포넌트를 렌더링할 때 이용한 useEffect를 모두 기억했다가 DOM을 업데이트한 이후(브라우저를 모두 그린 이후)에 실행한다.**

  이펙츠는 UI렌더링 이후로 미뤄지는데 클린업도 똑같이 미뤄진다. 이전 이펙트는 새 props와 함께 리렌더링되고 난 뒤에 클린업된다. 즉 다음과 같은 순서로 클린업과 이펙트 실행이 이뤄진다.

- props가 {id: 10} 에서 {id: 20}으로 업데이트 된다.
- props({id: 20})를 가지고 UI를 렌더링한다.
- 브라우저가 실제 그리기를 하고 {id: 20} 이 반영된 UI를 볼 수 있다.
- 리엑트는 {id: 10}에 대한 이펙트를 클린 업한다.
- 리엑트는 {id : 20}에 대한 이펙트를 실행한다.

## useEffect 두번째 인자의 의미\_ Effect 건너 뛰기

useEffect의 두 번째 오는 인자를 의존성 배열 "deps"라고 한다.

**이 값이 리렌더시 변함이 없다면 리엑트로 하여금 effect를 건너뛰게 만든다.**

훅의 기본 동작방식을 이해한다면 모든 렌더링시에 effect가 적용되거나 정리된다는 것을 알 수 있다.

이것은 때때로 성능 저하를 일으킬 수도 있다. 만약 effects안에서 사용되는 props나 state의 값이 변화가 없다면 굳이 다시 effect를 호출할 필요가 없다.

하지만 원래 리액트는 effect 함수 안을 볼 수 없기 때문에(스코프) 그 함수를 호출하지 않는 이상, effect안에서 일어나는 변화를 알 수 없다.

```js
const oldEffect = () => {
  return "a";
};
const newEffect = () => {
  return "a";
};
//여기서 oldEffect() === newEffect() 지만 호출하지 않으면 같은지 알 수 없다
```

의존성 배열에 명시해주는 값들은 리액트에게 '이 외의 값들은 사용하지 않겠다'는 약속과도 같다.

[-> 이어서 계속](https://codinggorani.github.io/til/about_useEffect_2)
