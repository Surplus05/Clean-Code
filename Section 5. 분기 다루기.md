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
