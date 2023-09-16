# 객체 다루기

## Shorthand Properties

css에서의 Shorthand Properties 예제를 한번 보자.

css에서는 어트리뷰트, js에서는 프로퍼티라 부른다.

```css
.before {
  margin-top: 10px;
  margin-right: 5px;
  margin-bottom: 10px;
  margin-left: 5px;
}

.after {
  margin: 10px 5px 10px 5px;
}

.after {
  margin: 10px 5px;
}
```

js에서는 어떻게 사용할까?

```javascript
const counterApp = combineReducers({
  counter,
  extra,
});
```

redux에서 사용하는 이 방식.  
바로 Shorthand Properties.

```javascript
const firstName = "poco";
const lastName = "jang";

const person = {
  firstName: "poco",
  lastName: "jang",
  getFullName: function () {
    return this.firstName + " " + this.lastName;
  },
};
```

코드를 축약할 수 있다.

```javascript
const person = {
  firstName,
  lastName,
  getFullName,
};
```

Shorthand Properties,
Concise method를 통해 축약이 가능.

문법을 모르면 이해하기 힘드므로, 제대로 알고 있어야 한다.

## Computed Property Name

React, Vue, JS 각각 어디에서 지원하는 문법인지 헷갈리는 경우가 있다.

그 예가 바로 Computed Property Name.

```js
const handleChange = (e) => {
  setState({ [e.target.name]: e.target.value });
};
```

```js
{ [e.target.name]: e.target.value }
```

이 프로퍼티를 동적으로 다룰 수 있는데 이 부분이 Computed Property Name.

대괄호를 사용하면 프로퍼티를 동적으로 넣을 수 있다.

```js
const func0 = "func0";
const func1 = "func1";
const func2 = "func2";

const obj = {
  [func0]() {
    return "func0";
  },
  [func1]() {
    return "func1";
  },
  [func2]() {
    return "func2";
  },
};

console.log(obj[func0]()); // func0
console.log(obj[func1]()); // func1
console.log(obj[func2]()); // func2
```

## Lookup Table

KEY-VALUE 형태의 테이블.

다음 코드를 한번 보자.

```js
function getUserType(type) {
  if (type === "admin") {
    return "관리자";
  } else if (type === "student") {
    return "학생";
  } else {
    return "외부인";
  }
}
```

```js
function getUserType(type) {
  const USER_TYPE = {
    admin: "관리자",
    student: "학생",
    unk: "해당 없음",
  };

  return USER_TYPE[type] ?? USER_TYPE.unk;
}
```

새로 추가해야 하는 경우나 길어지는 경우 아래쪽이 훨씬 유리하다.

또, 내부의 지역변수를 외부로 빼면 더 간단하게 만들수도 있다.

```js
function getUserType(type) {
  return USER_TYPE[type] ?? USER_TYPE.unk;
}
```

잘 활용하면 정말 깔끔하게 작성할 수 있다.

## Object Destructuring

```js
function Person(name, age, location) {
  this.name = name;
  this.age = age;
  this.location = location;
}

const poco = new Person("poco", 30, "korea");
```

문제점  
매개변수의 순서가 강제되고, 넣지 않는 값이 있을경우 undefined를 넣어주어야 한다.

```js
function Person({name, age, location})
```

객체로 받자.

```js
const poco = new Person({ location: "korea", name: "poco", age: 30 });
```

만약 필수적인 값이 필요하다면 ?

물론 타입스크립트 타입 지정을 통해 해결도 가능하지만 js라면 ?

필수는 따로 받고, 선택 사항은 options로 받자.

```js
function Person(name, {age,location})

const pocoOptions = {
  age:30,
  location:'korea',
} // 별도 변수말고 바로 넣어주어도 됨.

const poco = new Person('poco',pocoOptions)
```

라이브러리 별로 다 다르니 확인하고 넣어주자.

배열도 가능하다.

```js
const orders = ["First", "Second", "Third"];

const [first, , third] = orders;
```

객체로도 가능하다.

```js
const { 0: st, 2: rd } = orders;
```

```js
const [st, , , , , , rd] = orders;
```

배열 사이사이 비어있는것이 많다면 배열을 객체로도 받을 수 있다.

구조분해할당은 리액트의 props에서도 자주 활용되므로 잘 알아두도록 하자.

## Object freeze

말 그대로 freeze.

```js
const STATUS = Object.freeze({
  PENDING: "PENDING",
  SUCCESS: "SUCCESS",
  FAIL: "FAIL",
});

STATUS.FAIL = "test";
STATUS.TEST = "TEST";
// 조작 해도 변하지 않음.
```

Object 내부에 Object가 있다면 ?

```js
const STATUS = Object.freeze({
  PENDING: "PENDING",
  SUCCESS: "SUCCESS",
  FAIL: "FAIL",
  OPTIONS:{
    COLOR:'RED';
  }
});

Object.isFrozen(STATUS.OPTIONS) // false

// STATUS.OPTIONS는 변할 수 있게 됨.
```

shallow copy, deep copy 와 동일한 개념으로, deep frozen 은 불가능.

TypeScript 에서는 readonly를 통해 가능.

## Prototype 조작 지양하기

Class 문법이 없던 시절에는 생성자 함수를 통해야 했다.

메서드는 프로토타입으로 지정해 줬었다.

이미 JS는 충분히 발전해 구식 방법을 굳이 사용할 필요 없다.

내장 객체를 조작하지 말자.

특히 자바스크립트는 몽키패칭 언어이므로 런타임에 동작하는 것들을 멋대로 바꿀 수 있다.

옛날에는 가끔 프로토타입 건들기도 했음.

프로토타입을 건들면 위험하다.

어디서나 접근이 가능하고, 다른곳에서 사용할 수 있으므로.

그러나 충분히 발전했다.

건들지 말자.

클래스도 내부적으로는 프로토타입을 사용한다.

프로토타입에 대한 이해는 좋지만, 건들지는 말자.

## hasOwnProperty

hasOwnProperty 는 예약어로서 보호받지 못함.

그래서, Object Prototype 객체에서 가져오자.

그래서 객체에서 바로 사용하는 경우 ESLint 에서 catch 가능.

너무 길면 스니펫으로 따로 빼자.

## 직접 접근 지양하기

상태 관리에서, 객체를 읽고 쓰고 할 일이 많다.

아래 사례를 한번 살펴보자.

```js
const model = {
  isLogin: false,
  isValidToken: false,
};

function login() {
  model.isLogin = true;
  model.isValidToken = true;
}

function logout() {
  model.isLogin = false;
  model.isValidToken = false;
}

someElement.addEventListner("click", login);
```

model 에 직접 접근하고 있다.  
model은 중요한데, 접근에 열려있다.

숨겨주어야 한다.

```js
function setLogin(bool) {
  model.isLogin = bool;
}

function setValidToken(bool) {
  model.isValidToken = bool;
}
```

함수에 위임하고 model로의 접근은 숨기자.

더 safe 하고, 추적이 쉬워진다.

login시 로그 남겨야 한다면 setLogin에 로직을 추가하면 된다.

redux (flex 패턴)

> '예측 가능한 상태 컨테이너'

예측 가능한 코드를 작성해서 동작이 예측 가능하게 하자.

그래서, 직접 접근을 지양하자.  
더욱 안전하게 만들 수 있다.

## Optional Chaining

```js
const obj = {
  name: "value",
};
```

```
obj.name
```

으로 name속성의 값 접근 가능.

```
obj?.name
```

실무에서 일어날 법한 예를 한번 보자.

```js
const response = {
  data: {
    userList: [
      {
        name: "jang",
        info: {
          tel: "010",
          email: "jang@gmail.com",
        },
      },
    ],
  },
};

// 접근
response.data.userList[0].info.email;
```

이렇게 접근하면 런타임에서 터질 수 있다.

```js
function getUserEmailByIndex(userIdx) {
  return response.data.userList[userIdx].info.email;
}
```

서버 response 이기 때문에 userList가 없다면?

TypeError: Cannot read properties of undefined

잡히지 않는 오류가 생기면 뻗어버린다.

특히 FE 에서 서버 응답을 다루기 때문에, 런타임에서 에러가 생기지 않도록 안전한 코드를 만들어야 한다.

```js
function getUserEmailByIndex(userIdx) {
  if (response.data.userList) return response.data.userList[userIdx].info.email;

  return "알 수 없는 에러가 발생했습니다.";
}
```

data 도 안들어 온다면 ?

if문 또 추가? -> 아름답지 않다.

&& 연산자 사용하면 if문 사용을 줄일 수 있다.

```js
function getUserEmailByIndex(userIdx) {
  if (
    response.data &&
    response.data.userList &&
    response.data.userList[userIdx]
  )
    return response.data.userList[userIdx].info.email;

  return "알 수 없는 에러가 발생했습니다.";
}
```

그래도 너무 길어진다.

이때 Optional Chaining을 사용하자.

```js
function getUserEmailByIndex(userIdx) {
  if (response?.data?.userList[userIdx]?.info?.email)
    return response.data.userList[userIdx].info.email;

  return "알 수 없는 에러가 발생했습니다.";
}
```

모든 depth 에서 대응이 된다.
