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

둘다 값이 없음을 의미하지만, null은 명시적으로 넣어준 없음 값.  
Number로 type casting시 undefined는 NaN, null은 0.  
typeof로 확인해 보면 undefined는 undefined 타입, null은 object타입이기 때문에 두개를 유의해서 사용해야 함.

## eqeq

eqeqeq가 eqeq보다 더 엄격한 검사.

eqeq시 type casting이 묵시적으로 이루어지게 되는데, 이때 의도하지 않은 결과가 생길 수 있다.

그래서 type casting은 항상 명시적으로 수행해 주는것이 좋다.

human error를 줄이기 위해 eqeqeq를 사용하자.
