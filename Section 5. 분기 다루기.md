# 분기 다루기

개발자는 문법을 잘 지키는것이 중요.  
그러나 쉽지 않다.

JSX를 사용하는 과정에서 초보 개발자는 많은 실수를 한다.

그렇기에 JS를 근본적으로 이해하는 것이 중요.

## 삼항 연산자 다루기

삼항 연산자는 굉장히 좋게 사용될 수 있지만, 무리하게 사용될 경우 가독성을 저하시킬 수 있다.

```javascript
function example() {
  return condition1 ? value1
  : condition2 ? value2
  : condition3 ? value3
  : value 4;
}

function example() {
  if(condition1) {
    return value1;
  } else if (condition2) {
    return value2;
  } else if (condition3) {
    return value3;
  } else {
    return value4;
  }
}

```

너무 길어지면 switch문을 사용하자.

```javascript
const example = condition1 ? (a === 0 ? "zero" : "positive") : negative;
```

이해를 돕기위해 괄호를 사용해 주자.

Bad Case

```javascript
function alertMessage(isAdult) {
  isAdult ? alert("입장 가능합니다") : alert("입장 불가능합니다");
}
```

Good Case ?

삼항 연산자의 결과로 값을 반환하는 경우가 어울린다고 생각.

```javascript
function alertMessage(isAdult) {
  const message = isAdult ? "입장 가능합니다" : "입장 불가능합니다";

  alert(message);
}
```

- 그래서....  
  삼항 연산자는 삼'항'으로, 3개의 피 연산자를 사용하는 것이 Good Case.

  모든 값이 필요하지 않는데도 삼항연산자를 사용하는 경우가 존재.

  조건 ? 참 : 사용X;  
  조건 ? 사용X : 거짓;

# Truthy & Falsy

JS에서 조건을 검사할때, 형 변환이 이루어지게 되는데, 문자열, 숫자 등이 boolean으로 변환된다.

```javascript
if ("string".length) {
  console.log("true");
}

// 'true'
```

단순히 문자열 뿐 만 아니라,  
빈 객체 {},  
빈 배열 [],  
숫자 10,  
문자열 '0',  
등이 모두 조건문 내에서 true 로 변환되므로 엄밀히 따지면 true는 아니지만 true 취급을 받는데, 이를 truthy 값 이라 부른다.

false 취급을 받는 값들로는 null, undefined, 0, "", false, NaN 등이 있다.

이 truthy 와 falsy 를 잘 조합하면 null check시 매우 유용하다.

```javascript
function printName(name) {
  if (!name) {
    return "No Name";
  }
  return "안녕하세요" + name + "님";
}
```

# Short-Circuit Evaluation

```javascript
true && true && "도달 O";
true && false && "도달 X";

false || false || "도달 O";
true || true || "도달 X";
```

기본적인 순서는 왼쪽에서 오른쪽 순서.

- and 의 경우

  false 를 만나면 즉시 false 반환.

  true 를 만나면 다음 항으로, 마지막 항이면 마지막 항의 값 반환.

* or 의 경우  
  true 를 만나면 즉시 그 항의 값 반환.

  false를 만나면 다음 항으로, 마지막 항 까지 false라면 false 반환.

or 연산자의 경우 Default Value와 관련된 경우 유용함.

```javascript
console.log(state.data || "fetching...");
```

```javascript
function getActiveUserName(user, isLogin) {
  if (isLogin) {
    if (user) {
      if (user.name) {
        return user.name;
      } else {
        return "이름 없음";
      }
    }
  }
}

// 위 코드를 단축평가로 줄일 수 있다.

function getActiveUserName(user, isLogin) {
  if (user && isLogin) {
    return user.name || "이름 없음";
  }
}
```

# else if 피하기

```javascript
const x = 1;

if (x >= 0) {
  console.log("case 1");
} else if (x > 0) {
  console.log("case 2");
}

// case 1 출력.

if (x >= 0) {
  console.log("case 1");
} else {
  if (x > 0) {
    console.log("case 2");
  }
}
```

위 코드와 아래 코드는 논리적으로 동일.

맨 처음 만족하는 조건을 만나면 종료.

else if 를 여러번 사용하는 경우엔 switch문을 쓰거나

차라리 if 문을 여러번 사용하자.

```javascript
const x = 1;

if (x >= 0) {
  console.log("case 1");
}
if (x > 0) {
  console.log("case 2");
}
```

파이프라인 방식으로 동작하지 않음에 유의.

else 도 피하는것을 추천.

```javascript
function getActiveUserName(user) {
  if (user.name) {
    return user.name;
  } else {
    return "이름 없음";
  }
}

function getActiveUserName(user) {
  if (user.name) {
    return user.name;
  }

  return "이름 없음";
}
```

age가 20 미만시 특수함수 실행하는 기능을 추가한다고 생각 해 보자.

```javascript
function getHelloCustomer(user) {
  return "안녕하세요";
}

function getHelloCustomer(user) {
  if (user.age < 20) {
    report(user);
  } else {
    return "안녕하세요";
  }
}

function getHelloCustomer(user) {
  if (user.age < 20) {
    report(user);
  }

  return "안녕하세요";
}
```

위 중간 코드는 else 를 잘못 사용하고 있다.

# Early Return

```javascript
function loginService(isLogin, user) {
  if (!isLogin) {
    if (checkToken()) {
      if (!user.nickname) {
        return registerUser(user);
      } else {
        refreshToken();
        return "로그인 성공";
      }
    } else {
      throw new Error("No Token");
    }
  }
}
```

위 코드에서 분기는 다음과 같이 나뉜다.

1. 로그인 여부 검사.
2. 토큰이 있는지 검사.
3. 닉네임 검사.

복잡한 로직을 리팩토링을 통해 개선 해 나가자.

```javascript
function loginService(isLogin, user) {
  // early return 을 활용하자.
  /**
   * 함수를 미리 종료
   * 가독성 개선에 좋다.
   */

  if (isLogin) {
    return;
  }

  if (!checkToken()) {
    throw new Error("No Token");
  }

  if (!user.nickname) {
    return registerUser(user);
  }

  refreshToken();
  return "로그인 성공";
}
```

로직을 분리할 수도 있음.

```javascript
function login() {
  refreshToken();
  return "로그인 성공";
}

function loginService(isLogin, user) {
  // early return 을 활용하자.
  /**
   * 함수를 미리 종료
   * 가독성 개선에 좋다.
   */

  if (isLogin) {
    return;
  }

  if (!checkToken()) {
    throw new Error("No Token");
  }

  if (!user.nickname) {
    return registerUser(user);
  }

  login();
}
```

다른 예제를 한번 더 연습해 보자.

```javascript
function Today(condition, weather, isJob) {
  if (condition === "GOOD") {
    Study();
    Game();
    WatchTV();

    if (weather === "GOOD") {
      Wash();
      Exercise();
    }

    if (isJob === "GOOD") {
      NightShift();
      SleepEarly();
    }
  }
}
```

```javascript
function Today(condition, weather, isJob) {
  if (condition !== "GOOD") {
    return;
  }

  Study();
  Game();
  WatchTV();

  if (weather === "GOOD") {
    Wash();
    Exercise();
  }

  if (isJob === "GOOD") {
    NightShift();
    SleepEarly();
  }
}
```

공통된 의존성이 로직을 묶어둔 경우, 빼내서 early return 하는 것을 명심하자.

# 부정조건문

```javascript
const isLogin = true;

if (!isLogin) {
  // 로그인 안됨
}
```

이런식으로 부정조건문을 사용하면 헷갈릴 가능성이 존재.

isNaN을 생각 해 보자.

```javascript
if (!isNaN(3)) {
  console.log("숫자입니다");
}
```

숫자가 아님 isNaN()

숫자가 아님이 아님 !isNaN()

생각을 여러번 해야함 -> Human Error 를 높일 수 있음.

```javascript
if (isCondition) {
  // 참 로직
} else {
  // 거짓 로직
}
```

보통 이렇게 사용한다.

Early Return, Form Validation 등 검사 로직에서 사용하는 경우를 포함해 참 로직이 사용되지 않는 경우에만 한정적으로 사용하자.

```javascript
if (!isCondition) {
  // 거짓 로직
}
```

# Default Case

```javascript
function sum(x, y) {
  return x + y;
}

sum(); // ?
```

```javascript
function sum(x, y) {
  x = x || 1; // 기본값 1
  y = y || 1;

  return x + y;
}

sum(); // 2
```

다른 예제를 한번 보자.

```javascript
function createElement(type, height, width) {
  const element = document.createElement(type);

  element.style.height = height || 100 + "px";
  element.style.width = width || 100 + "px";

  return element;
}

createElement();
```

Default Case를 고려해야 안전하고 확장성 높은 코드 작성이 가능.

```javascript
function registerDay(userInputDay) {
  switch (userInputDay) {
    case "월":
    case "화":
    case "수":
    case "목":
    case "금":
    case "토":
    case "일":
  }
}
```

Default Case 가 필요할까?

만약 입력에 오타가 들어왔다면? ex) 워ㅓㄹ

```javascript
function registerDay(userInputDay) {
  switch (userInputDay) {
    case "월":
    case "화":
    case "수":
    case "목":
    case "금":
    case "토":
    case "일":
    default:
      throw new Error("wrong input");
  }
}
```

언제나 worst case 를 고려해야 함.

```jsx
const Root = () => (
  <Router history={browserHistory}>
    <Switch>
      <Route exact path="/" component={App} />
      <Route exact path="/welcome" component={Welcome} />
      <Route component={NotFound} />
    </Switch>
  </Router>
);

// 이렇게 React의 Router에서도 Default Case를 고려해 주는것이 좋다.
```

# 명시적인 연산자 사용 지향

연산자 우선순위를 외워야 하는가 ?  
현실적으로 불가능.

차라리 안전하게, 괄호를 사용하자.

( 2 + ( 2 x 2 ) )

우선순위를 명시적으로 적용시켜 주자.
가독성도 증가시켜주고, 디버그 시에도 오류를 찾기 쉽게 된다.

전위 후위 증가연산자도, 명시적으로

number = number + 1 처럼 명시해주는것이 좋다.

단순히 숫자 연산뿐만 아니라 조건문에도 적용된다.

if( ((isLogin && isDone) || isAuthenticated) )

즉, 예측 가능하고 디버깅하기 쉬운 코드를 작성하기 위해서는 연산자 우선순위에 괄호를 적용해서 코드를 작성하자!!

# 널 병합 연산자

Default Case에서 살펴본 예제를 다시 살펴보자.

```javascript
function createElement(type, height, width) {
  const element = document.createElement(type);

  element.style.height = height || 100 + "px";
  element.style.width = width || 100 + "px";

  return element;
}

createElement("div", 0, 0);
```

0은 Falsy 값이므로, height 와 width는 100으로 설정되게 된다.

기본값 or 단축평가시 null과 undefined만 분리하고 싶은 경우?

널 병합 연산자를 사용하자.

```javascript
function createElement(type, height, width) {
  const element = document.createElement(type);

  element.style.height = height ?? 100 + "px";
  element.style.width = width ?? 100 + "px";

  return element;
}

createElement("div", 0, 0);
```

falsy => || 사용.

falsy 를 엄격하게 적용해 null, undefined만 적용하고 싶다면 ? => ??

최신 문법이기 때문에, Babel 이 필요할 수 있음.

하기쉬운 실수들을 살펴보자.

```javascript
function HelloWorld(message) {
  if (!message) {
    return "Hello World";
  }

  return "Hello" + message;
}

console.log(HelloWorld(0));
// Hello World
```

다른 예제를 한번 더 보자.

```javascript
function getUserName() {
  return isLogin && user ?? user.name;
}
```

논리연산자와 혼합해서 사용이 불가능.

실수를 너무 많이해서 문법적 오류로 만든 것.

그리고 Optional Chaining이랑 상당히 궁합이 좋음.

# 드모르간 법칙

```javascript
if (isValidToken && isValidUser) {
  console.log("로그인 성공");
}
```

간단한 로직.

로그인 실패도 만들어 볼까?

flag를 재활용을 해 보자.

```javascript
if (!(isValidToken && isValidUser)) {
  console.log("로그인 실패");
}
```

여기서 또 무언가가 늘어난다면? 더 어려워 진다.

드모르간의 법칙을 적용시킬 수 있다.

!(A && B) 로직은 !A || !B 와 같은 의미.

!(A || B) 로직은 !A && !B 와 같은 의미.

```javascript
if (!isValidToken || !isValidUser) {
  console.log("로그인 실패");
}
```

괄호를 벗겨낼 수 있다.
