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

아까 위에서 언급했듯 리엑트는 지정한 props와 state에 따라 DOM과 동기화한다. 따라서 렌더링시 Mount, Update의 구분이 없다.

useEffect는 기본적으로 첫번째 렌더링과 그 이후의 모든 업데이트에서 수행된다. 리엑트는 effect가 수행되는 시점에 이미 DOM이 업데이트 되어있음을 보장한다.

**리액트는 컴포넌트를 렌더링할 때 이용한 useEffect를 모두 기억했다가 DOM을 업데이트한 이후에 실행한다.**

컴포넌트가 첫번째로 렌더링 할 때와 그 후에 다르게 동작하는 이펙츠를 작성하고자 한다면 그 흐름을 거스른다.

반복문, 조건문, 중첩된 함수 내부가 아닌, 최상위 레벨에서만 리엑트훅을 호출해야 한다는 hook의 규칙에서 그 이유를 알 수 있다.

하나의 컴포넌트에서 state나 Effect훅을 여러개 쓸 수 있다. React는 Hook이 호출되는 순서에 의존한다.

따라서 특정 state가 어떤 useState 호출에 해당하는지 알 수 있다.

이펙츠 중 하나를 조건문 안에 사용한다고 가정해보자.

첫 렌더링과 다르게 조건문이 거짓이 되어 해당 훅을 이후 렌더링시 건너 뛴다 하면, Hook호출 순서가 달라진다.

리액트는 예상과는 다른 결과에 그 시점 부터 버그를 발생시키게 된다.

=> 다음 게시물에서 계속...
