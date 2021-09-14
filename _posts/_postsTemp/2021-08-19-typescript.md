## 타입 정의하기

동적 언어인 자바스크립트에서 몇몇 디자인 패턴은 자동으로 타입을 제공해주는 것이 어려울 수 있다 이럴 때 타입이 어떻게 되어야하는지 명시해 줄 수 있다.

```ts
const user = {
  name: "Hayes",
  id: 0,
};
// 이 객체의 형태를 명시적으로 나타내기 위해서는 interace로 선언한다.

interface User {
  name: string;
  id: number;
}

// 변수 선언 뒤에 :TypeName의 구문을 이용해 이 객체가 새로운 interface 의 형태를 따르고 있음을 선언할 수 있다.

const user: User = {
  name: "Hayes",
  id: 0,
};
```

인터페이스는 함수에서 매개변수와 리턴 값을 명시하는데 사용되기도 한다.

```ts
function getAdminUser(): User {
  // ...
}

function deleteUser(user: User) {
  // ...
}
```

## 타입구성, 조합 (Composing Types)

여러가지 타입을 이용해 새로운 타입을 구성하기 위한 방법

많이 사용되는 방법은 유니언, 제너릭이 있다.

### 유니언

타입이 여러 타입 중 하나일 수 있음을 선언하는 방법

```typescript
type MyBool = true | false;
```

MyBool 은 자동으로 Boolean으로 분류되고 이는 구조적 시스템의 프로퍼티다.

```ts
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

위는 유니언 타입이 가장많이 사용되는 사례 중 하나고 허용되는 string 또는 number의 리터럴 집합을 설명하는 것이다.

제너릭 -제너릭은 타입에 변수를 제공하는 방법
