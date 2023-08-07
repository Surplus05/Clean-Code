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
