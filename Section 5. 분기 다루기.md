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
