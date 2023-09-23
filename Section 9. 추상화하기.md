# 추상화하기

추상화 이해하는데 있어 제일 쉬운 접근은 Magic Number.

## Magic Number

```js
setTimeout(() => {
  console.log("timeout");
}, 3 * 60 * 1000);
```

이렇게 숫자를 넘겨주는 방식에서 Magic Number가 많이 나타남.

```js
const COMMON_DELAY_MS = 3 * 60 * 1000;

setTimeout(() => {
  console.log("timeout");
}, COMMON_DELAY_MS);
```

정책이나 규칙이 있다고 하면, 하나하나 수정해야 하므로 별도로 빼는것이 좋다.  
특정 파일에 모아서 export한다면 관리나 사용에서 매우 유용해질 것.

```js
const PRICE = {
  MAX: 1000000,
  MIN: 100000000,
};
```

0이 많아서 파악이 힘들다.  
Numeric Operator를 사용해 보자.

```js
const PRICE = {
  MAX: 1_000_000,
  MIN: 100_000_000,
};
```

```js
// min, max 를 넘겨줌을 유추할 수 있긴 하다.
// 그러나 어디까지나 유추하는것이지 명확하게 파악은 실제 코드나 명세를 봐야 알 수 있다.
getRandomPrice(0, 10);

// 한층 이해가 편해진다.
getRandomPrice(PRICE.MIN, PRICE.MAX);
```

```js
function isValidName(name) {
  return carName.length >= 1 && carName.length <= 5;
}
```

Policy 가 하드코딩되어져 있다.  
수정되어야 하는 경우 매우 번거로울 수 있다.

```js
const CAR_NAME_LEN = {
  MAX: 5,
  MIN: 1,
};

function isValidName(name) {
  return carName.length >= CAR_NAME_LEN.MIN && carName.length <= MAX;
}
```

일반적으로 SNAKE CASE 면 건들지 않겠으나 정 불안하면 readonly 설정해주면 된다.

```js
// 1 depth
const CAR_NAME_LEN = Object.freeze({
  MAX: 5,
  MIN: 1,
});

// typescript readonly
const CAR_NAME_LEN = {
  MAX: 5,
  MIN: 1,
} as const;
```

참고로 as const는  
type 추론을 number가 아니라 5로 하게 만들며 readonly 프로퍼티가 된다.

정책을 수정한다면, CAR_NAME_LEN만 수정하면 되게 된다.

## DOM API 접근 추상화

```js
const loader = () => {
  const el1 = document.createElement("div");
  el1.setAttribute("class", "클래스명 지정");
  const el2 = document.createElement("div");
  el2.setAttribute("class", "클래스명 지정");
  const el3 = document.createElement("div");
  el3.setAttribute("class", "클래스명 지정");
};
```

이 코드를 리팩터링 해 보자.

element 생성을 따로 빼줄 수 있다.

```js
const createLoader = () => {
  const el1 = document.createElement("div");
  const el2 = document.createElement("div");
  const el3 = document.createElement("div");

  return { el, el2, el3 };
};

const loader = () => {
  const { el1, el2, el3 } = createLoader();
  el1.setAttribute("class", "클래스명 지정");
  el2.setAttribute("class", "클래스명 지정");
  el3.setAttribute("class", "클래스명 지정");
};
```

style 지정도 따로 빼줄 수 있다.

```js
const createLoader = () => {
  const el1 = document.createElement("div");
  const el2 = document.createElement("div");
  const el3 = document.createElement("div");

  return { el, el2, el3 };
};

const createLoaderStyle = ({ el1, el2, el3 }) => {
  el1.setAttribute("class", "클래스명 지정");
  el2.setAttribute("class", "클래스명 지정");
  el3.setAttribute("class", "클래스명 지정");

  return {
    newEl1: el1,
    newEl2: el2,
    newEl3: el3,
  };
};

const loader = () => {
  const { el1, el2, el3 } = createLoader();
  const { newEl1, newEl2, newEl3 } = createLoader({ el1, el2, el3 });

  // extra logic ...
};
```

create 부분과 style 지정 부분을 추상화 하였음.
