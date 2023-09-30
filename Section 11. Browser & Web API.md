# Browser & WEB API

## Semantic

div를 남발하기보다는 Semantic한 Tag를 사용하자.  
웹 표준을 준수해야 하며, SEO 에도 유리하다.

## NodeList

Node는 문서 내 모든 객체(DOM),  
Element는 Tag로 둘러싸인 요소.

QuerySelectorAll로 찾으면 NodeList를 반환.  
이 NodeList는 배열과는 다르니 사용에 주의.

HTMLCollection vs NodeList, Node vs Element 는 다른 문서에서 따로 다루도록 하자.

## innerHTML

```js
const string = "<h1>hello world</h1>";

document.querySelector("main").innerHTML = string;
```

이런식의 코드를 자주 볼 수 있다.  
innerHTML 은 오래됐고, 좋지 않은 API이다.

XSS에 주의.

insertAdjacentHTML 을 대신 사용하도록 하자.

innerHTML은 완전히 대체, insertAdjacentHTML 은 부분적 삽입.  
그리고, 일반적인 Text는 innerText, insertAdjacentText 등을 사용해 더 안전하게 사용하자.

보안적인 측면에서 innerHTML 이 더 취약하다.  
insertAdjacentHTML또한 상대적으로 덜 취약 할 뿐이지 사용자 입력을 그대로 삽입하는것은 여전히 위험하다.

## Data Attributes

input의 type, class, id 등이 attribute가 해당한다.

존재하지 않는 속성을 설정할 수 있다.

```html
<div custom="test">test</div>
```

비 표준 속성을 조금 더 의미있게 사용할 수 있도록 규칙을 정했다.

data라는 prefix를 통해 설정한다.

```html
<div data-message="test" class="test">test</div>
```

접근도 더 쉽게 가능하다.

```js
const test = document.getElementByClass("test");
console.log(test.dataset.message); // test
```

js에서는 -를 변수명이나 key로 사용할 수 없기에 data attribute 를 사용하면 자동으로 변환해 주므로 data attribute를 사용하는 것이 좋다.  
css에서도 접근이 가능하므로 굉장히 간편하고, 강력하다.

data-color-mode, data-visible 등등 굉장히 많이 쓰임.  
그리고, 개발자 도구를 통해 노출되므로 이에 유의해 사용하자.

결론 : custom attribute도 사용이 가능하지만 쉽고 강력한 data attribute를 만들어 놨으니 표준에 따르자.

## Black Box Event Listner

프로그래밍 관점에서의 블랙박스는 자동차의 그것과 다르다.

박스 내부 동작이 보이지 않음을 의미, 추상화가 잘못된(너무 과한 추상화거나 명시적 코드가 아님) 것을 의미한다.  
내부 구현이 어떻게 동작될 지 예측 할 수 없는 경우도 포함된다.

```js
const button = document.querySelector('button');

buton.addEventListner('이벤트 타입', 리스너 함수);
```

주로 리스너함수 명으로는 handleClick, onClick 을 주로 사용함.

```js
buton.addEventListner("click", handleClick);
```

코드만 보고 무슨 일이 일어나는지 확인할 수 있는가 ?

불가능하다.

```js
buton.addEventListner("click", setLog);
```

무슨 일을 하는지 이해됨.

또는 익명 함수가 리스너로 올 수 있다.

익명 함수의 길이가 커지면 파악하기 힘들다.

서치바 실수 사례를 한번 살펴 보자.

```js
const handleClick = () => {
  1. input을 받음
  2. 유효성 검사
  3. form 전송
}
```

무슨 일이 일어나는지 파악이 힘들다.

그리고 엔터키 입력도 검색할 수 있도록 해야 한다고 생각 해 보자.

```js
buton.addEventListner("click", handleSearch);
buton.addEventListner("keyup", handleSearch);
form.addEventLister("onsubmit", handleSearch);
```

handleClick으로 작성하면, 무슨 일이 일어나는지 모르게 되는, 블랙박스로 감춰지는 로직이 된다.
