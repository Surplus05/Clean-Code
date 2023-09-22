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

## void, return  

void 는 아무 값도 반환하지 않는 것.  
return 은 함수의 반환값.  

```js
function handleClick() {
  return setState(false);
}

function showAlert(msg) {
  return alert(msg);
}
```

setState, alert 는 void 함수.  
즉 리턴이 존재하지 않음.  

기본적으로 리턴값이 없으면 undefined를 리턴한다.  
리턴값이 없는 함수를 호출해 리턴하면 undefined를 리턴한다.  

굳이 return 을 붙여줄 필요가 없다.  
명세를 잘 확인해서, 무슨 값을 반환하는지 잘 알고 불필요한 코드를 줄여주자.  

## 화살표 함수  

```
const user = {
  name: 'Poco',
  getName: () => {
    return this.name;
  }
}
```

실제로 메소드 호출시 undefined를 출력한다.  
this는 외부에서 this를 상속받는데, 객체의 { } 리터럴은 스코프가 아니다.  
그래서 객체가 선언된 스코프에서 name을 찾고, 없으니 undefined를 반환하는 것 이다.  

뿐만 아니라 arguments를 사용할 수 없으며, 화살표 함수는 생성자로 사용할 수 없다.  

클래스 문법에서의 일반 함수와 화살표 함수의 차이를 살펴 보자.  
자식 클래스에서 부모의 arrow function 을 호출시 에러를 발생시킨다.  
생성자 함수에서 arrow function method는 초기화가 되어버리기 때문.  

그리고, 자식 클래스에서 부모의 arrow function을 오버라이딩 하는 경우 정상 동작하지 않는다.  
오히려 부모의 Method가 호출되어 버린다.  

다음 예제를 한번 보자.  

```js
function* gen() {
  yield () => {}
}
```

불가능. 일반 함수를 사용해야 한다.
맹목적으로 사용하지는 말자.  

## Callback Function  
함수의 인자로 함수를 넘겨주어 호출하는 방식.  
무조건 비동기가 아니다.  

## 순수 함수  

* Side Effect
콘솔로그, 비동기 타이머, 파일 저장, api 호출, 변화 가능한 인자나 함수를 받거나, 랜덤성의 유틸리티를 사용하거나 등등은 Side Effect를 일으킬 수 있다.

순수함수는 Side Effect를 일으키지 않으며, 위의 조건을 준수해야 한다.  
순수함수는 동일한 인자를 받으면 동일한 결과를 반환해야 한다.  

아래 코드를 한번 보자.  

```js

let num1 = 10;
let num2 = 20;

function impureSum1() {
  return num1 + num2;
}

function impureSum2(newNum) {
  return num1 + newNum
}

```

외부에서 num1, num2 를 조작할 수 있기 때문에 동일한 결과가 아닐 수 있다.  
그러므로 순수함수가 아니게 된다.

```js
function pureSum(num1, num2) {
  return num1 + num2;
}
```

동일한 input에 대해 항상 동일한 결과를 반환.  
Primitive Type이 아니라 Reference Type을 다루는 경우 항상 새로운 Reference를 만들어 반환해야 한다.

## Closure  

함수가 반환될때 렉시컬 환경까지 같이 반환되는 것.  

```js
function add(num1) {
  return function sum2(num2) {
    return num1 + num2;
  }
}

const addOne = add(1);
```
add(1)을 호출하면 num1 = 1 인 새로운 렉시컬 환경이 만들어지고, 이 만들어진 환경을 참조하는 sum2 함수가 리턴된다.  
addOne은 num1에 1이 들어간 렉시컬 환경을 기억하고 있는 함수라 할 수 있다.  
