# 배열도 객체다.

다른 언어들은 흔히 배열과 객체가 구분되어 있으나 JS에서는 배열도 객체에 해당한다.

그러므로 다음과 같은 코드가 문제없이 동작한다.

```javascript
const arr = [1, 2, 3];

arr[3] = "test";
arr["obj"] = {};
arr[{}] = [1, 2, 3];
arr["func"] = () => {
  return "hello";
};
```

```javascript
const obj = {
  3: "test",
  obj : {},
  {}.toString(): [1,2,3],
  func: () => {
    return 'hello';
  }
};
```

배열의 key로 숫자가 아닌 값을 주더라도 사용이 가능.

```javascript
const arr = [1, 2, 3];

const obj = {
  0: 1,
  1: 2,
  2: 3,
};

// 그래서, 배열을 내부에서는 동작한다고 생각해볼 수 있겠다.
```

어떻게 배열을 확인할까?

```javascript
const arr1 = [1, 2, 3];
const arr2 = "[1, 2, 3]";

// 문자열과 구분 불가능
if (arr2.length) {
  console.log("배열일까?");
}

// 객체와 구분 불가능
if (typeof arr2 === "object") {
  console.log("배열일까?");
}
if (arr1 instanceof Array) {
  console.log("배열일까?");
}

if (Array.isArray(arr1)) {
  console.log("배열일까?");
}
```

instanceof 나 isArray 를 사용하는게 안전하다.

# arr.length

굉장히 자주 사용된다.

```jsx
if (arr.length > x) {
}

arr.legnth > 10 && arr.map((el) => <Element></Element>);
```

다음 코드를 한번 보자.

```javascript
const arr = [1, 2, 3];

console.log(arr.length); //3

arr.length = 5;

console.log(arr);
// arr [1, 2, 3, , ]

const arr2 = [1, 2, 3];

arr2[4] = 5;

console.log(arr2);
// [1, 2, 3, , 5]

console.log(arr2.length); // 5
```

배열에 구멍이 생길 수 있다.

legnth가 요소의 개수를 항상 보장하는것은 아님. (배열의 마지막 요소 인덱스에 더 가깝다.)

그래서, length를 0으로 만들면 초기화도 가능하다.

# 요소로의 접근

배열의 단위를 요소라고 부름.

요소가 3개인 배열  
const arr = [1, 2, 3];

함수에서 배열의 index로 접근하는 경우가 흔하다.

이러한 경우 혼돈이 생길 수 있다.

```javascript
function func(input) {
  if (input[0] === "admin") {
  }
  if (input[1] === "seoul") {
  }
}
```

input[0], input[1] 이 무엇을 의미하는지 애매해짐.

그래서, 받을때도 배열로 받으면 좋다.  
구조분해할당을 이용하자.

```javascript
function func([role, location]) {
  if (role === "admin") {
  }
  if (location === "seoul") {
  }
}
```

```javascript
// from
function buttonHandler() {
  const confirmBtn = document.getElementByTagName("button")[0];
  const resetBtn = document.getElementByTagName("button")[1];
}

// to
function buttonHandler() {
  const [confirmBtn, resetBtn] = document.getElementByTagName("button");
}
```

```javascript
function formatDate(targetDate) {
  const date = targetDate.toIOString().split("T")[0];

  const [year, month, day] = date.split("-");

  return `${year}년 ${month}월 ${day}일`;
}
```

구조분해할당 잘 해주고 있지만?

```javascript
const date = targetDate.toIOString().split("T")[0];
```

이 부분을 개선해 보자.

```javascript
const [date] = targetDate.toIOString().split("T");
```

또는 유틸 함수를 사용해 볼 수 있다.

```javascript
function head(arr) {
  return arr[0];
  // return arr[0] ?? '';
}
```

# 유사 배열 객체

배열도 객체라고 했다.

```javascript
const obj = {
  0: "hello",
  1: "world",
  length: 2,
};

const arr = Array.from(obj);
```

arr은 문제없이 배열이 된다.

함수의 arguments도 유사배열객체에 해당.

그러나 언제까지 '유사'배열객체이므로 배열은 아니다.

그래서, 프로토타입 객체도 배열이 아니므로 map, filter, reduce 등 고차함수 사용이 불가능.

# 고차함수로 리팩터링하기

```javascript
const price = [1000, 2000, 3000];

function convertPrice(priceList) {
  let temp = [];
  for (let i = 0; i < price.length; i++) {
    temp.push(price[i] + "원");
  }

  return temp;
}

function convertPrice(priceList) {
  return priceList.map((price) => price + "원");
}
```

추가 요구사항  
1000원보다 큰(초과) 값만

```javascript
const price = [1000, 2000, 3000];

function convertPrice(priceList) {
  let temp = [];
  for (let i = 0; i < price.length; i++) {
    if (price > 1000) temp.push(price[i] + "원");
  }

  return temp;
}
```

최종 리팩터링

```javascript
const isOverThousand = (price) => price > 1000;
const addSuffixWon = (price) => price + "원";

function convertPrice(priceList) {
  const filteredPriceList = priceList.filter(isOverThousand);
  const finalPriceList = filteredPriceList.map(addSuffixWon);

  return finalPriceList;
}
```

그래서, 고차함수를 쓰면 간단하게 만들 수 있다.

더 간단하게 만들 수 있는데, 메서드 체이닝 방식으로 표현하자.

```javascript
function convertPrice(priceList) {
  return priceList.filter(isOverThousand).map(addSuffixWon);
}
```

# map vs forEach

```javascript
const price = [1000, 2000, 3000];

const newPricesForEach = price.forEach((price) => price + "원");

const newPricesMap = price.map((price) => price + "원");
```

가장 큰 차이점은 return.

map -> 맵핑되는 값으로 새로운 배열 생성.
forEach -> 배열 순회만 이루어짐.

# break 와 continue

for문에서는 break, continue가 동작하지만 순회함수 (foreach, map 등) 에서는 동작하지 않는다.

대안은?

for문을 쓰거나, try-catch 사용하자..
