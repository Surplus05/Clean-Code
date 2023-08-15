# 경계 다루기

```javascript
function getRandomNumber(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// Constants

const MIN_NUMBER = 1;
const MAX_NUMBER = 10;

let number = getRandomNumber(MIN_NUMBER, MAX_NUMBER);
```

코드만 보고도 동작 방식이 쉽게 이해가 가능하다.  
그러나 MIN, MAX 값이 포함되는지는 되게 애매하다.

```javascript
const MAX_AGE = 20;

function isAdult(age) {
  return age >= MAX_AGE; // > 인지 >= 인지..?
}
```

경계를 다룰 때에는  
이상, 초과 vs 이하, 미만 (즉, 포함 여부)  
에 따라 되게 애매해질 수 있다.

네이밍에 명시해서 표현할 수 있지만, 널리 알려진 Rule이 존재한다.

## Boundary Naming Convention

- first - last  
  Boundary 를 전부 포함하는 경우 (이상, 이하)

* begin - end  
  Boundary가 begin 은 포함되고 end는 제외하는 경우 (이상, 미만)

배열의 내장 고차함수에서도 똑같은 규칙이 적용되니, 알아두도록 하자.

## prefix, suffix

여러 prefix들이 존재.

private를 지칭하는 \_ 등도 prefix로 사용하기도 했다. (최근 문법에서는 #으로 가능)

훅에서는 use prefix 가 사용되기도 한다.

suffix는 리덕스에서 살펴볼 수 있는데, 비동기 요청에서 요청 성공 실패에 따라 suffix로 사용하기도 한다.

USER_REQUEST, USER_SUCCESS, USER_FAILURE 등

이러한 prefix와 suffix는 일관성을 유지할 수 있게 해 줌.

## 매개변수

매개변수의 순서도 경계가 될 수 있다.

매개변수는 다음과 같은 권장사항을 따르는게 좋다.

매개변수는 2개를 넘지 않도록 하자.  
arguments, rest parameter를 사용하자.  
랩핑하는 함수를 만들자.  
매개변수를 객채에 담자.

```typescript
function someFunc(someArg1, someArg2, someArg3, someArg4) {}

// 랩핑 함수
function getFunc(someArg1, someArg3) {
  someFunc(someArg1, _, someArg3);
}
```

```typescript
function someFunc({ someArg1, someArg2, someArg3, someArg4 }) {}

someFunc(someObject);
```
