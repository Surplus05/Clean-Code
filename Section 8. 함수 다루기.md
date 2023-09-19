# 함수 다루기

함수는 사실 1급 객체.

변수나 데이터에 담길 수 있다.  
매개변수로 전달 가능하고(콜백), 함수가 함수를 반환(고차함수)할 수 있다.

## this

함수에서의 this 는 때에 따라 다른데 다음과 같다.

일반 함수 -> global  
생성자 함수 -> 생성될 인스턴스  
메서드 -> 객체  
arrow -> 외부에서 상속  
등등..

conciseMethod -> 함수명으로 객체의 key를 바로 설정할 수 있게 해줌.

## arguments, parameter

parameter는 함수 정의에서 이름지어진다.  
argument는 함수로 전달되는 실제 값을 의미.

## 복잡한 인자 관리하기

```js
function toggleDisplay(isToggle) {}

function sum(sum1, sum2) {}

function genRandomNumber(min, max) {}

function genSquare(top, right, bottom, left) {}
```

무조건 많은게 나쁜것이 아님.  
흐림과 맥락에 맞다면 좋은 것.

```js
function createCar(name, brand, color, type) {}

// 뭔가 흐름이.. 별로다. 코드를 개선 해 보자.

// 객체를 사용 해 볼까?

function createCar(options) {
  var name = options.name;
  var brand = options.brand;
  // .... 옛날에는 이렇게 할 수 밖에 없었다.
  // 그래도 헷갈리지는 않았다.
}

// 모던 JS에서 좋은 문법이 나왔기에, 구조분해할당을 해 보자.

function createCar({ name, brand, color, type }) {
  // 1. 순서가 없어지고(객체를 통해), 2. 추가적인 변수선언 필요도 없어 짐(구조분해).
}

// 더 개선해 보자면,

function createCar(name, { brand, color, type }) {
  // 필수적인 값과 선택적인 값을 구분하면 더 편하다.
}
```

다음 상황을 보자.

```js
function createCar({ name, brand, color, type }) {
  if (!name) {
  }
  if (!brand) {
  }
  if (!color) {
  }
  if (!type) {
  }
  // ... 널 체크를 하나하나 하기 너무 힘들다.
}
```

## Default Parameter(value)

공식 문서를 보면 Default 를 볼수 있다.

다음 코드를 한번 보자.

```js
function func(options) {
  options = options || {};
  var name = options.name;
}

func();
```

> Cannot read properties of undefined 에러 발생함!

변수에 값을 저장한다면 || 나 ?? 를 통해 프로퍼티 하나하나 지정해 줄 수도 있다.

```js
function func({ name = "철수" } = {}) {}

func();
```

= {} 이 부분은 options || {} 역할을 함.  
name = "철수"는 기본값을 지정함.

빼먹었을 때 에러 출력하고 싶다면 ?

```js
const required = (argName) => {
  throw new Error(agrName + " is required.");
};

function func({ name = required("items") } = {}) {}
```

값이 들어오지 않으면 default parameter 문법이 실행, required 함수가 실행되며 오류 뿜어냄.

## Rest Parameters

```js
function sumTotal() {
  return Array.from(arguments).reduce((acc, curr) => acc + curr);
}

sumTotal(1, 2, 3, 4, 5);
```

이 상태에서 sumTotal에서 인자를 받고 싶다면 ?

```js
function sumTotal(init, middle, ...args) {
  console.log(init); // 1
  console.log(middle); // 2
  return args.reduce((acc, curr) => acc + curr);
}

sumTotal(1, 2, 3, 4, 5);
```

spread operator 와 생김새가 똑같음에 유의.  
rest parameter는 가변적인 parameter에 좋다.  
인자 중에서 가장 마지막에 들어가야 함을 꼭 기억하자.
