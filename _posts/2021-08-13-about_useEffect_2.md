---
title: "useEffect에 대하여(2)"
categories:
  - TIL
tags:
  - TIL
  - React Hook
  - useEffect
---

# useEffect 제대로 이해하기(2)

이 글은 [React공식문서](https://ko.reactjs.org/docs/hooks-effect.html)와 Dan Abramov의 [useEffect 완벽가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)를 읽고 정리한 글입니다. 질문 또는 수정이 필요한 내용이 있으면 댓글로 남겨주세요!
{: .notice--info}

## 의존성 제대로 적어주기

deps를 지정할 때 꼭 유념해야 하는 게 있다. **deps에는 컴포넌트 범위 내에서 바뀌는 값들과 effect에 의해 사용되는 값들을 모두 포함해야한다**는 것이다. 리액트 공식문서에 따르면 그렇게 하지 않는다면 현재 값이 아닌 이전의 렌더링 값을 참고하게 되어있다.

### 그렇다면 의존성에 [ ] 빈 배열을 사용하는 경우는 언제인가?

effect를 실행하고 clean up 하는 과정을 딱 한 번씩만 실행하고 싶을 때에는 두 번째 인자에 빈 배열을 넘겨주면 된다. 이는 effect가 어느 값에도 의존하지 않는다는 것을 react에게 알려주는 것이다.
{: .notice}

### 의존성을 제대로 적어주지 않으면 생기는 일

```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => {
      clearInterval(id);
    };
  }, []);

  return <h1>{count}</h1>;
}
```

Counter라는 함수 컴포넌트 안 useEffect에서 count가 1초 간격으로 1씩 증가하게 만들고 싶어서 이런 함수를 적었다. 처음 마운트 될때만 실행되었으면 해서 두 번째 인자에 빈 배열을 넘겨주었다. **그러나 이 예제는 숫자가 한 번밖에 증가하지 않는다. 의존성 배열에 count를 명시해 주지 않았기 때문이다. 지금의 코드로는 첫번째 렌더링에서만 useEffect가 호출되고 나머지에선 전부 무시된다.**

이에 대한 첫번째 해결책은 의존성을 전부 빠짐없이 적어주는 것이다.

```js
useEffect(() => {
  const id = setInterval(() => {
    setCount(count + 1);
  }, 1000);

  return () => {
    clearInterval(id);
  };
}, [count]);
```

하지만 이렇게되면 매 렌더링시 마다 인터벌이 해제되었다가 다시 설정될 것이다. 두 번째 인자를 빈배열로 놔두면서 즉, 의존성을 제거하지 않고도 문제를 해결할 수 있는 방법이 있다.

```js
useEffect(() => {
  const id = setInterval(() => {
    setCount((c) => c + 1);
  }, 1000);

  return () => {
    clearInterval(id);
  };
}, []);
```

count를 오직 setCount 안에서 count를 증가 시키기위해 사용하기 있음을 인지하면 useEffect의 스코프 안에서 count를 사용하지 않아도 된다는 것을 알 수 있다. **이전 상태를 기준으로 상태 값을 업데이트하기 위해서는 함수형태의 업데이터를 사용하면 되는 것이다.**
