# typeof 연산자

```javascript
typeof 1234;

// 'number'
```

문자열을 반환한다.

Primitive Type은 일반적으로 문제 없이 사용할 수 있다.

Refrence Type의 경우 문제가 생길 여지가 있다.

```javascript
const str = new String("test");

typeof str;
// 'object'

funtion f() {}

typeof f;
// 'function'

class MyClass {}

typeof MyClass;
// 'function'

typeof null;
// 'object'

```

그래서, Refrence Type과 null을 type check시 유의해야 한다.

# instanceof 연산자

프로토타입 체인에 포함되어 있는지를 확인한다.

```javascript
const arr = [];

arr instanceof Array;
// true

// 최상위에는 Object가 있게 됨.
arr instanceof Object;
// true
```

외울 필요는 없다.  
단, typeof instanceof 사용시 유의하자.  

# undefined & null  
값이 없거다 정의되지 않음을 의미.  
null은 값이 없다고 명시적으로 넣어준 것.  

숫자와 같이 연산시 undefined 는 NaN, null은 0을 의미한다.  
typeof 를 통해 타입을 보면 undefined 는 undefied를, null 은 object를 출력하게 된다.  

이 점에 유의해서 코드를 작성하자.  

# eqeq와 eqeqeq
eqeq는 값이 같으면 true, eqeqeq는 값과 타입이 같아야 true를 반환  

eqeqeq는 equality의 strict version  

eqeq는 형 변환이 일어나게 됨.  

``` javascript
'1' == 1 // true
1 == true // true
```

예측 불가한 상황이 생길 수 있다.  
eqeq사용을 자제하자.  

형 변환은 묵시적이 아니라 명시적으로 이루어지는게 좋다.  

```javascript
userInput.value == 0 // userInput.value 가 string 타입이라면 묵시적 형변환이 이루어지게 됨.

Number(userInput.value) === 0 // 이처럼 명시적 형변환을 해 주는게 좋다.

// 아래와 같은 묵시적 형변환은 자제하자.
!'string' // true
1234 + 'string' // '1234string'

//이런식으로 해야 Human Error를 최소화할 수 있다.
Boolean('string')
String(1234) + 'string'
```
eqeq와 eqeqeq의 Equality Table 참고
https://dorey.github.io/JavaScript-Equality-Table/  

결론 - Human Error를 줄이기 위해, eqeqeq와 명시적 형변환을 사용해야 한다.  

# isNaN
숫자인지 아닌지 검사.  
검사시 === 가 아니라 !== 처럼 동작한다.  

``` javascript
isNaN(123) // false 숫자가 아닌게 아니다(숫자다)

Number.isNaN(123 + '테스트') // false
isNaN(123 + '테스트') // true

// Number.isNaN을 사용하는것이 권장된다.
```

isNaN 은 느슨한 검사.  
Number.isNaN은 엄격한 검사.  

엄격하지 않은게 더 편할 수 있지만, Human Error를 야기할 가능성도 높인다.  
웬만하면 엄격하게 코드를 작성하자.
