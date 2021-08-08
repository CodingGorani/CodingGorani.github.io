---
title: "useEffect에 대하여"
categories:
  - TIL
tags:
  - TIL
  - React Hook
  - useEffect
---

# useEffect

이 글은 Dan Abramov의 [useEffect 완벽가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)를 읽고 정리한 글이다.

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

  따라서 리엑트는 effect가 수행되는 시점에 이미 DOM이 업데이트 되어있음을 보장한다.

- **리액트는 컴포넌트를 렌더링할 때 이용한 useEffect를 모두 기억했다가 DOM을 업데이트한 이후에 실행한다.**

  컴포넌트가 첫번째로 렌더링 할 때와 그 후에 다르게 동작하는 effects를 작성하고자 한다면 흐름을 거스르게 된다.

  반복문, 조건문, 중첩된 함수 내부가 아닌, 최상위 레벨에서만 리엑트훅을 호출해야 한다는 hook의 규칙에서 그 이유를 알 수 있다.

  하나의 컴포넌트에서 state나 Effect훅을 여러 개 쓸 수 있다. 이 때 React는 Hook이 호출되는 순서에 의존한다.

  따라서 특정 state가 어떤 useState 호출에 해당하는지 알 수 있다.

  이펙츠 중 하나를 조건문 안에 사용한다고 가정해보자.

  첫 렌더링과 다르게 조건문이 거짓이 되어 해당 훅을 이후 렌더링 시 건너뛴다 가정하면, Hook호출 순서가 달라진다.

  리액트는 예상과는 다른 결과에 그 시점 부터 버그를 발생시키게 된다.

## useEffect 두번째 인자의 의미

**useEffect는 두번째 인자의 배열안에 있는 값들이 리렌더시 변함이 없다면 리엑트로 하여금 effect를 건너뛰게 만든다.**

훅의 기본 동작방식을 이해한다면 모든 렌더링시에 effect가 적용되거나 정리된다는 것을 알 수 있다.

그러나 만약 effects안에서 사용되는 props나 state의 값이 변화가 없다면 굳이 다시 effect를 호출할 필요가 없다.

이것은 때때로 성능 저하를 일으킬 수도 있다. 이러한 문제는 useEffect의 두번째 인자의 **의존성 배열**을 명시함으로써 해결할 수 있다.
