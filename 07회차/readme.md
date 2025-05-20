# 21장 빌트인 객체

## 21.1 자바스크립트 객체의 분류

**표준 빌트인 객체**

- ECMAScript 사양에 정의된 객체로 애플리케이션 전역의 공통 기능을 제공
- 자바스크립트 실행환경 (브라우저 또는 Node.js 환경) 과 관계없이 사용 가능
- 전역 객체의 프로퍼티로서 제공되어 별도의 선언 없이 전역 변수처럼 참조 가능

**호스트 객체**

- ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체
- 브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker 와 같은 클라이언트 사이드 Web API 를 호스트 객체로 제공
- Node.js 환경에서는 Node.js 고유 API 를 호스트 객체로 제공

**사용자 정의 객체**

- 사용자가 직접 정의한 객체

## 21.2 표준 빌트인 객체

자바스크립트는 40여 개의 표준 빌트인 객체를 제공한다. 생성자 함수 객체는 프로토타입 메서드, 정적 메서드를 제공하고, 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

**Math, Reflect, JSON 을 제외한 표준 빌트인 객체는 모두 생성자 함수 객체**로 ****표준 빌트인 객체인 String, Number, Boolean, Function, Array, Date 등 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj);       // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj);     // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true); // Boolean {true}
console.log(typeof boolObj);      // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // ƒ anonymous(x )
console.log(typeof func);                       // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr);        // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp);         // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();  // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object
```

**생성자 함수인 표준 빌트인 객체로 생성한 인스턴스의 프로토타입**은 **표준 빌트인 객체의 prototype 프로퍼티**에 바인딩된다.

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

**Number 빌트인 객체**는 인스턴스 없이 정적으로 호출할 수 있는 정적 메서드도 제공한다.

```jsx
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

## 21.3 원시값과 래퍼 객체

문자열, 숫자, 불리언 등 원시값이 있는데도 문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등 표준 빌트인 생성자 함수가 존재하는 이유는 뭘까?

```jsx
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

→ 여기서 **생성되는 임시 객체**를 **wrapper object** 라고 한다.

```jsx
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

**과정**

1. 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고 문자열은 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된다.
2. 이 때 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype 메서드를 상속받아 사용할 수 있다.
3. 사용된 후 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]`내부 슬롯에 할당된 원시값으로 원래의 상태로(원시 값)되돌린다.
4. 래퍼 객체는 가비지 컬렉션의 대상이 되어 사라진다.

<aside>
💡 문자열, 숫자, boolean, Symbol 은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용할 수 있지만 **null, undefined 는 래퍼 객체를 생성하지 않는다.** 
null, undefined 값을 객체처럼 사용하면 에러가 발생한다.

</aside>

## 21.4 전역 객체

전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 **어떤 객체보다도 먼저 생성되는 특수한 객체**

어떤 객체에도 속하지 않은 최상위 객체

→ 브라우저 환경에서는 window, Node.js 환경에서는 global 이다.

**전역 객체의 프로퍼티**

- 표준 빌트인 객체(`Object`, `String`, `Number`, `Function`, `Array`등)
- 환경에 따른 호스트 객체(클라이언트 Web API, Node.js의 호스트 API)
- `var`키워드로 선언한 전역 변수
- 전역 함수

**전역 객체의 특징**

- 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- 전역 객체의 프로퍼티를 참조할 때 `window`(또는 `global`)를 생략할 수 있다.

```jsx
window.parseInt === parseInt; // -> true
```

- 전역 객체는 `Object`, `String`, `Number`, `Boolean`등 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행환경(브라우저 또는 Node.js)에 따라 추가적인 프로퍼티와 메서드를 갖는다.(브라우저: DOM, BOM, Canvas, XMLHttpRequest등 / Node.js: Node.js 고유의 API)
- `var`키워드로 선언한 전역 변수, 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.

```jsx
// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
bar = 2; // window.bar = 2
console.log(window.bar); // 2

// 전역 함수
function baz() { return 3; }
console.log(window.baz()); // 3
```

- `let`이나 `const`키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

```jsx
let foo = 123;
console.log(window.foo); // undefined
```

- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 `window`를 공유한다.

### 21.4.1 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.

**Infinity**

`Infinity`프로퍼티는 무한대를 나타내는 숫자값 `Infinity`를 갖는다.

```jsx
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0);  // Infinity
// 음의 무한대
console.log(-3/0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

**NaN**

`NaN`프로퍼티는 숫자가 아님(`Not a Number`)을 나타내는 숫자값 `NaN`을 갖는다.

`NaN`프로퍼티는 `Number.NaN`프로퍼티와 같다.

```jsx
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number
```

**undefined**

undefined 프로퍼티는 원시 타입 `undefined`를 값으로 갖는다.

```jsx
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

### 21.**4.2 빌트인 전역 함수**

빌트인 전역 함수는 애플리케이션 전역에서 호추할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

**eval**

`eval`함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받아, 전달받은 문자열 코드가 표현식 이라면 `eval`함수는 문자열 코드를 런타임에 평가하여 값을 생성하고,전달받은 인수가 표현식이 아닌 문이라면 `eval`함수는 문자열 코드를 런타임에 실행한다.

```jsx
/**
 * 주어진 문자열 코드를 런타임에 평가 또는 실행한다.
 * @param {string} code - 코드를 나타내는 문자열
 * @return {*} 문자열 코드를 평가/실행한 결과값
 */
eval(code)

// 표현식인 문
eval('1 + 2;'); // -> 3

// 표현식이 아닌 문
eval('var x = 5;'); // -> undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언되었다.
console.log(x); // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('({ a: 1 })');
console.log(o); // {a: 1}

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval('(function() { return 1; })');
console.log(f()); // 1
```

여러개의 문으로 이루어져 있다면 모든 문을 실행한 다음, 마지막 결과값을 반환한다.

```jsx
console.log(eval('1 + 2; 3 + 4;')); // 7
```

`eval`함수는 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정한다.

```jsx
const x = 1;

function foo() {
  // eval 함수는 런타임에 foo 함수의 스코프를 동적으로 수정한다.
  eval('var x = 2;');
  console.log(x); // 2
}

foo();
console.log(x); // 1
```

**strict mode** 에서는 `eval`함수는 기존의 스코프를 수정하지 않고 `eval`함수 자신의 자체적인 스코프를 생성한다.

```jsx
const x = 1;

function foo() {
  'use strict';

  // strict mode에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프를 생성한다.
  eval('var x = 2; console.log(x);'); // 2
  console.log(x); // 1
}

foo();
console.log(x); // 1
```

인수로 전달받은 문자열 코드가 `let`, `const` 키워드를 사요한 변수 선언문이라면 암묵적으로 `strict mode`가 적용된다.

```jsx
const x = 1;

function foo() {
  eval('var x = 2; console.log(x);'); // 2
  // let, const 키워드를 사용한 변수 선언문은 strict mode가 적용된다.
  eval('const x = 3; console.log(x);'); // 3
  console.log(x); // 2
}

foo();
console.log(x); // 1
```

`eval`함수는 보안에 매우 취약하다.또한, 최적화가 수행되지 않아 일반적인 코드 실행에 비해 느리다.따라서 `eval`**함수의 사용은 금지해야 한다.**

**isFinite**

전달받은 인수가 정상적인 유한수 인지 검사하여 유한수이면 `true`, 무한수이면 `false`를 반환한다.

```jsx
// 인수가 유한수이면 true를 반환한다.
isFinite(0);    // -> true
isFinite(2e64); // -> true
isFinite('10'); // -> true: '10' → 10(문자열->숫자)
isFinite(null); // -> true: null → 0(null->0)

// 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(Infinity);  // -> false
isFinite(-Infinity); // -> false

// 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(NaN);     // -> false
isFinite('Hello'); // -> false
isFinite('2005/12/12'); // -> false
```

**isNaN**

전달받은 인수가 `NaN`인지 검사하여 그 결과를 불리언 타입으로 반환한다. 전달받은 인수의 타입이 숫자가 아닌경우 숫자로 타입을 변환한 후 검사를 수행한다.

```jsx
// 숫자
isNaN(NaN); // -> true
isNaN(10);  // -> false

// 문자열
isNaN('blabla'); // -> true: 'blabla' => NaN
isNaN('10');     // -> false: '10' => 10
isNaN('10.12');  // -> false: '10.12' => 10.12
isNaN('');       // -> false: '' => 0
isNaN(' ');      // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date());            // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```

**parseFloat**

전달받은 문자열 인수를 부동 소수점 숫자(`floating point number`)즉, 실수로 해석하여 반환한다.

```jsx
// 문자열을 실수로 해석하여 반환한다.
parseFloat('3.14');  // -> 3.14
parseFloat('10.00'); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat('34 45 66'); // -> 34
parseFloat('40 years'); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat('He was 40'); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(' 60 '); // -> 60
```

**parseInt**

전달받은 문자열 인수를 정수(`integer`)로 해석하여 반환한다.

두 번째 인수로 진법을 나타내는 기수(2~36)을 전달할 수 있고 결과는 10진수다.

```jsx
// 문자열을 정수로 해석하여 반환한다.
parseInt('10');     // -> 10
parseInt('10.123'); // -> 10

parseInt(10);     // -> 10
parseInt(10.123); // -> 10

// 10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10'); // -> 10
// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 2); // -> 2
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 8); // -> 8
// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt('10', 16); // -> 16
```

기수를 지정하여 10진수 숫자를 문자열로 변환하여 반환하려면 `Number.prototype.toString`메서드를 사용한다.

```jsx
const x = 15;

// 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(2); // -> '1111'
// 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(2), 2); // -> 15

// 10진수 15를 8진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(8); // -> '17'
// 문자열 '17'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(8), 8); // -> 15

// 10진수 15를 16진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(16); // -> 'f'
// 문자열 'f'를 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(8), 8); // -> 15

// 숫자값을 문자열로 변환한다.
x.toString(); // -> '15'
// 문자열 '15'를 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString()); // -> 15
```

문자열이 16진수 리터럴(`0x`또는 `0X`로 시작)이면 16진수로 해석하여 10진수 정수로 반환한다.

```jsx
// 16진수 리터럴 '0xf'를 16진수로 해석하고 10진수 정수로 그 결과를 반환한다.
parseInt('0xf'); // -> 15
// 위 코드와 같다.
parseInt('f', 16); // -> 15
```

2진수, 8진수 리터럴은 안된다. 0으로 시작하는 숫자로 인식해 버리기 때문에 두 번째 인수로 명확하게 알려줘야 한다.

```jsx
// 2진수 리터럴(0b로 시작)은 제대로 해석하지 못한다. 0 이후가 무시된다.
parseInt('0b10'); // -> 0
// 8진수 리터럴(ES6에서 도입. 0o로 시작)은 제대로 해석하지 못한다. 0 이후가 무시된다.
parseInt('0o10'); // -> 0

// 문자열 '10'을 2진수로 해석한다.
parseInt('10', 2); // -> 2
// 문자열 '10'을 8진수로 해석한다.
parseInt('10', 8); // -> 8
```

첫 번째 문자가 해당 지수의 숫자로 변환될 수 없다면 NaN 을 반환한다.

```jsx
// 'A'는 10진수로 해석할 수 없다.
parseInt('A0'); // -> NaN
// '2'는 2진수로 해석할 수 없다.
parseInt('20', 2); // -> NaN
```

해석할 수 없는 문자열을 마주치면 이후부터는 무시되며 해석된 정수값만 반환한다.

앞뒤 공백은 무시되고, 첫 번째 문자열을 숫자로 해석할 수 없는 경우 NaN 을 반환한다.

```jsx
// 10진수로 해석할 수 없는 'A' 이후의 문자는 모두 무시된다.
parseInt('1A0'); // -> 1
// 2진수로 해석할 수 없는 '2' 이후의 문자는 모두 무시된다.
parseInt('102', 2); // -> 2
// 8진수로 해석할 수 없는 '8' 이후의 문자는 모두 무시된다.
parseInt('58', 8); // -> 5
// 16진수로 해석할 수 없는 'G' 이후의 문자는 모두 무시된다.
parseInt('FG', 16); // -> 15

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseInt('34 45 66'); // -> 34
parseInt('40 years'); // -> 40
// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseInt('He was 40'); // -> NaN
// 앞뒤 공백은 무시된다.
parseInt(' 60 '); // -> 60
```

**encodeURI / decodeURI**

**encodeURI 함수**는 완전한 `URI Uniform Resource Identifier`를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.`URI`는 인터넷에 있는 자원을 나타내는 유일한 주소를 말한다.

```jsx
// 완전한 URI
const uri = 'http://example.com?name=이웅모&job=programmer&teacher';

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
```

**decodeURI 함수**는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

```jsx
const uri = 'http://example.com?name=이웅모&job=programmer&teacher';

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

// decodeURI 함수는 인코딩된 완전한 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
const dec = decodeURI(enc);
console.log(dec);
// http://example.com?name=이웅모&job=programmer&teacher
```

**🔍 인코딩?**

URI 의 문자들을 이스케이프 처리하는 것을 의미한다. 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.

`알파벳, 0~9의 숫자, - _ . ! ~ * ‘ ( )` 문자는 이스케이프 처리에서 제외된다.

**encodeURIComponent / decodeURIComponent**

**encodeURIComponent** : URI 구성 요소를 인수로 전달받아 인코딩한다.

**decodeURIComponent** : 매개변수로 전달된 URI 구성 요소를 디코딩한다. ****

| encodeURIComponent | encodeURI |
| --- | --- |
| 함수는 인수로 전달된 문자열을 URI의 구성요소인 쿼리스트링의 일부로 간주 | 매개변수로 전달된 문자열을 완전한 URI 전체로 간주 |
| 쿼리 스트링 구분자 =, ?, & 까지 인코딩 | 쿼리 스트링 구분자로 사용되는 =, ?, & 은 인코딩하지 않음 |

```jsx
// URI의 쿼리 스트링
const uriComp = 'name=이웅모&job=programmer&teacher';

// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.
let enc = encodeURIComponent(uriComp);
console.log(enc);
// name%3D%EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec);
// 이웅모&job=programmer&teacher

// encodeURI 함수는 인수로 전달받은 문자열을 완전한 URI로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &를 인코딩하지 않는다.
enc = encodeURI(uriComp);
console.log(enc);
// name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

dec = decodeURI(enc);
console.log(dec);
// name=이웅모&job=programmer&teacher
```

### 21.4.3 암묵적 전역

선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티로 등록된다. 이러한 현상을 **암묵적 전역 (implicit global)** 이라 한다.

```jsx
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

`foo`함수 내부에 `y = 20;`이 실행되면 참조 에러가 발생하지 않고, 전역 변수처럼 사용된다.전역 객체의 프로퍼티로 등록되어 참조되기 때문이다.

하지만 `y`는 변수 선언 없이 **단지 전역 객체의 프로퍼티로 추가되었을 뿐**이다.`y`**는 변수가 아니다.** 따라서 **변수 호이스팅도 발생하지 않는다.**

```jsx
// 전역 변수 x는 호이스팅이 발생한다.
console.log(x); // undefined
// 전역 변수가 아니라 단지 전역 객체의 프로퍼티인 y는 호이스팅이 발생하지 않는다.
console.log(y); // ReferenceError: y is not defined

var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

전역 변수는 프로퍼티이지만 delete 연산자로 삭제할 수 없고, 프로퍼티인 y 는 delete 연산자로 삭제할 수 있다.

```jsx
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
  console.log(x + y);
}

foo(); // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않는다.
delete y; // 프로퍼티는 삭제된다.

console.log(window.x); // 10
console.log(window.y); // undefined
```

# 22장 this

## 22.1 this 키워드

```jsx
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.

따라서 **자신이 속한 객체, 인스턴스를 가리키는 식별자인** **this** 를 제공한다.

**✨ this**

**자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수 (self-referencing variable)** 다.

this 를 통해 자신이 속한 객체, 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

```jsx
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

```jsx
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

**⚠️ this 바인딩이 동적으로 결정되고, strict mode 일 때 this 바인딩은 또 달라질 수 있다.**

this 는 코드 어디에서든 참조가 가능하지만, strict mode 일 때는?

→ 일반 함수 내부에서는 this 를 사용할 필요가 없기 때문에 일반 함수 내부의 this 에서는 `undefined` 가 바인딩된다.

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');

(function () {
	'use strict';
	
	function foo() {
		console.log(this); // undefined
	}
});
```

## 22.2 함수 호출 방식과 this 바인딩

🔍 **렉시컬 스코프와 this 바인딩 결정 시기**

- 렉시컬 스코프 (함수 상위 스코프를 결정하는 방식) 는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 결정
- **this 바인딩은 함수 호출 시점에 결정**

**함수 호출 방식**

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 22.2.1 일반 함수 호출

**기본적으로 this 에는 전역 객체 (global object) 가 바인딩된다.**

전역 함수, 중첩 함수를 **일반 함수로 호출하면** 함수 내부의 this 는 전역 객체가 바인딩된다.

```jsx
function foo() {
  console.log("foo's this: ", this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

다만 일반 함수에서 this 는 의미가 없다.

아래 예제처럼 strict mode 에서는 일반 함수 내부의 this 에는 undefined 가 바인딩된다.

```jsx
function foo() {
  'use strict';

  console.log("foo's this: ", this);  // undefined
  function bar() {
    console.log("bar's this: ", this); // undefined
  }
  bar();
}
foo();
```

**콜백 함수가 일반 함수로 호출된다면** 콜백 함수 내부의 this 에도 **전역 객체가 바인딩**된다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  }
};

obj.foo();
```

위 예제에서 콜백함수와 외부에서 호출한 `[obj.foo](http://obj.foo)` 메서드의 this 가 일치하지 않으면 중첩 함수, 콜백 함수를 헬퍼 함수로 사용하기 어렵다.

위 케이스에서 this 바인딩을 일치하는 방법은 여러가지가 있다.

**foo 메서드 내부에서 this 를 변수에 바인딩하는 방법**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  }
};

obj.foo();
```

**화살표 함수를 사용하는 방법**

화살표 함수 내부의 this 는 상위 스코프의 this 를 가리킨다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    setTimeout(() => console.log(this.value), 100); // 100
  }
};

obj.foo();
```

### 22.2.2 메서드 호출

메서드 내부의 this 는 메서드를 호출한 객체가 바인딩된다.

메서드 내부의 this 는 메서드를 소유한 객체가 아닌 **메서드를 호출한 객체에 바인딩된다는 것**이다.

```jsx
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

### 22.2.3 생성자 함수 호출

생성자 함수 내부의 this 에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

Function.prototype.apply/call/bind 메서드에 **첫 번째 인수로 전달한 객체가 this 에 바인딩**된다.

```jsx
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}
```

apply, call 메서드와 달리 **Function.prototype.bind 메서드는 함수를 호출하지 않는다.**

첫 번째 인수로 전달한 값으로 **this 바인딩이 교체된 함수를 새롭게 생성해 반환**한다.

```jsx
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

bind 메서드는 메서드의 **this 와 메서드 내부 중첩 함수/콜백 함수의 this 가 불일치하는 문제를 해결할 때 유용**하다.

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    // ①
    setTimeout(callback, 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
});
```

bind 메서드를 사용하여 아래와 같이 this 를 일치시킬 수 있다.

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```
