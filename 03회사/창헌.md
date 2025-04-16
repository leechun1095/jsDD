# 10. 객체 리터럴

## 10.1 객체란?

객체란 **0개 이상의 프로퍼티로 구성된 집합, property 는 key 와 value 로 구성된다.**

원시 값을 제외한 나머지 값 (함수, 배열, 정규 표현식 등) 은 모두 객체로, 원시 값은 변경 불가능한 값 `immutable value` , 객체는 변경 가능한 값 `mutable value`

```jsx
var person = {
	name: 'Lee',
	age: 20
}
```

함수는 일급 객체이므로 값으로 취급할 수 있으므로 함수도 property 값으로 사용할 수 있다.

property 값이 함수일 경우, 일반 함수와 구분하기 위해 method 라고 한다.

```jsx
var counter = {
	num: 0, // property
	increase: function () { // method
		this.num++;
	}
}
```

- property: 객체의 상태를 나타내는 값
- method: property 를 참조하고 조작할 수 있는 동작

### 객체 리터럴에 의한 객체 생성

**클래스 기반 객체지향 언어의 객체 생성 방법**

클래스를 사전에 정의하고, 필요한 시점에 `new 연산자` 와 함께 생성자를 호출하여 인스턴스를 생성한다.

```java
class User {}

...

User user = new User();
```

**자바스크립트 객체 생성 방법**

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create method
- 클래스 (ES6)

**객체 리터럴**

- 중괄호 `({…})` 내에 n개 이상의 프로퍼티를 정의
- 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성

```jsx
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: ƒ}

var empty = {}; // 빈 객체
console.log(typeof empty); // object
```

객체 리터럴은 코드 블록이 아니라서 세미콜론을 붙이지 않을 수 있지만, **값으로 평가되는 표현식**으로 세미콜론을 붙인다.

### 프로퍼티

객체는 프로퍼티의 집합, 프로퍼티는 key 와 value 로 구성된다.

- key: **빈 문자열을 포함하는** 모든 문자열 또는 Symbol 값
    - 빈 문자열을 사용해도 에러가 발생하지 않지만, 의미를 갖지 못하므로 권장하지 않음
- value: 자바스크립트에서 사용할 수 있는 모든 값

```jsx
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: 'Lee',
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20
};
```

식별자 네이밍 규칙을 따르지 않는 이름에는 **반드시 따옴표를 사용해야 한다.**

```jsx
var person = {
  firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Lee'   // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}
```

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 key 를 동적으로 생성할 수 있다.

```jsx
var obj = {};
var key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';

// ES6: 계산된 프로퍼티 이름
// var obj = { [key]: 'world' };

console.log(obj); // { hello: "world" }
```

프로퍼티 키에 **문자열이나 Symbol 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열**이 된다.

```jsx
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

var, function 같은 예약어를 사용해도 에러가 발생하지 않지만 권장하지 않는다.

```jsx
var foo = {
  var: '',
  function: ''
};

console.log(foo); // {var: "", function: ""}
```

존재하는 프로퍼티 key 를 중복 선언하면 나중에 선언한 프로퍼티가 덮어쓰고 에러는 발생하지 않는다.

```jsx
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```

### 메서드

자바스크립트의 함수는 객체 (일급 객체) 로 값으로 취급할 수 있어서 프로퍼티 값으로 사용할 수 있다.

```jsx
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () { // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

### 프로퍼티 접근

- 마침표 표기법 (dot notation)
- 대괄호 표기법 (bracket notation)

```jsx
var person = {
  name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee
```

**대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 key 는 반드시 따옴표로 감싼 문자열**이어야 한다.

```jsx
var person = {
  name: 'Lee'
};

console.log(person[name]); // ReferenceError: name is not defined
```

**객체에 존재하지 않는 프로퍼티에 접근하면 undefined 를 반환하기 때문에** ReferenceError 는 발생하지 않는다.

```jsx
var person = {
  name: 'Lee'
};

console.log(person.age); // undefined
```

프로퍼티 key 가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.

```jsx
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

👀 **Node 환경 / 브라우저 환경 person.last-name 의 실행결과 차이**

Node 환경

`ReferenceError: name is defined`

브라우저 환경

`NaN`

브라우저 환경에서는 name 이 전역 변수로 암묵적으로 존재한다.

### 프로퍼티 변경 방법

**갱신**

이미 존재하는 프로퍼티에 값을 할당하면 갱신된다.

```jsx
var person = {
  name: 'Lee'
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person);  // {name: "Kim"}
```

**동적 생성**

존재하지 않는 프로퍼티에 값을 할당하면 동적으로 생성되고 값이 할당된다.

```jsx
// person 객체에는 age 프로퍼티가 존재하지 않는다.
// 따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

**삭제**

delete 연산자는 객체의 프로퍼티를 삭제한다.

```jsx
// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;

console.log(person); // { name: "Lee" }
```

### ES6 에서 추가된 객체 리터럴 확장

**프로퍼티 축약 표현**

프로퍼티 값으로 변수를 사용하는 경우,

**변수 이름과 프로퍼티 키가 동일한 이름일 때** key 를 생략할 수 있다.

```jsx
// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

**계산된 프로퍼티 이름**

ES5 에서는 객체 리터럴 외부에서 대괄호 `([…])` 표기법을 사용해야 한다.

ES6는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 동적 생성할 수 있다.

```jsx
// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

**메서드 축약 표현**

ES6 는 메서드를 정의할 때 `function` keyword 를 생략한 축약 표현을 사용할 수 있다.

```jsx
// ES6
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```

# 11. 원시 값과 객체의 비교

## **원시 타입과 객체 타입의 차이점**

| 원시 타입 (primitive type) | 객체 타입 (object/reference type) |
| --- | --- |
| 변경 불가능한 값 | 변경 가능한 값 |
| 변수에 할당 시, 실제 값 저장 | 변수에 할당 시, 참조 값 저장 |
| 원시 값이 복사되어 전달 (pass by value) | 참조 값이 복사되어 전달 (pass by reference) |

## 원시 값

**변경 불가능한 값**

변경 불가능 하다는 것은 변수가 아니라 값에 대한 진술이다.

변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있다.

변수 값을 변경하기 위해 원시 값을 재할당하면 새로운 메모리 공간을 확보하고 재할당한 값을 저장한 후, 변수가 참조하던 메모리 공간의 주소를 변경한다. 값의 이러한 특성을 **불변성**`immutability`라고 한다.

<img width="713" alt="스크린샷 2022-10-09 오전 11 02 53" src="https://user-images.githubusercontent.com/29244798/194745107-575f70a9-2553-4134-adbd-60d62c5e9f4c.png">

**문자열과 불변성**

> ECMAScript 사양에 따르면 원시 타입의 크기를 아래와 같이 규정하고 있다.문자열 타입: 2바이트숫자 타입: 8바이트이외의 원시 타입은 규정되어 있지 않아 브라우저 제조사의 구현에 따라 다를 수 있다.
> 

```jsx
// 문자열은 0개 이상의 문자들로 이뤄진 집합이다.
var str1 = '';      // 0개의 문자로 이뤄진 문자열(빈 문자열)
var str2 = 'Hello'; // 5개의 문자로 이뤄진 문자열
```

```jsx
var str = 'Hello';
str = 'world';
```

👀 유사 배열 객체

배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, `length` 프로퍼티를 갖는 객체

문자열은 마치 배열처럼 인덱스를 통해 각 문자에 접근할 수 있으며, `length` 프로퍼티를 갖기 때문에 유사 배열 객체이고 `for`문으로 순회할 수 도 있다.

```jsx
var str = 'string';

// 문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근할 수 있다.
console.log(str[0]); // s

// 원시 값인 문자열이 객체처럼 동작한다.
console.log(str.length); // 6
console.log(str.toUpperCase()); // STRING
```

원시값인 문자열이 객체처럼 동작한다는 것은 래퍼 객체(`wrapper object`)로 자동 변환되기 때문인데 아래를 참고하자.

**값에 의한 전달**

```jsx
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;

console.log(score, copy); // 80  80
console.log(score === copy); // true
```

score 변수와 copy 변수는 숫자 값 80을 갖는다는 점에서 일치 비교를 하면 참이라는 결과가 나오지만, **score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값**이다.

<img width="649" alt="스크린샷 2022-10-09 오전 11 12 02" src="https://user-images.githubusercontent.com/29244798/194745123-ca80d2ec-fb5f-458f-b88f-3a7882f6934f.png">

```jsx
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;

console.log(score, copy);    // 80  80
console.log(score === copy); // true

// score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다.
// 따라서 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.
score = 100;

console.log(score, copy);    // 100  80
console.log(score === copy); // false
```

score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.

**값에 의한 전달**이라는 용어에 대해 🤔

ECAMScript 사양에는 등장하지 않는 용어로 공유에 의한 전달 (pass by sharing) 이라고 표현하는 경우도 있다.

용어로 인해 오해가 있을 수 있지만, 엄격하게 표현하면 **변수에는 값이 전달되는 것이 아니라 메모리 주소가 전달된다.**

식별자로 값을 구별해서 식별한다는 것은 식별자가 기억하고 있는 메모리 주소를 통해 메모리 공간에 저장된 값에 접근할 수 있기 때문이다.

### 객체

객체 타입의 값, 즉 객체는 변경 가능한 값 (mutable value) 이다.

객체를 할당한 변수에 접근하면 참조 값 (reference value) 에 접근할 수 있다. 참조 값은 **생성된 객체가 저장된 메모리 공간의 주소**이다.

```jsx
var person = {
  name: 'Lee'
};
```

<img width="461" alt="스크린샷 2022-10-09 오전 11 21 04" src="https://user-images.githubusercontent.com/29244798/194745129-101c4187-67fc-4c4f-810b-00f6d83847ef.png">


객체를 할당한 변수 `person` 을 참조하면 메모리에 저장되어 있는 참조 값 (`0x00…1`) 을 통해 실제 객체에 접근한다.

객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있다. 재할당 없이 프로퍼티 또한 동적으로 변경할 수 있다.

```jsx
var person = {
  name: 'Lee'
};

// 프로퍼티 값 갱신
person.name = 'Kim';

// 프로퍼티 동적 생성
person.address = 'Seoul';

console.log(person); // {name: "Kim", address: "Seoul"}
```

<img width="422" alt="스크린샷 2022-10-09 오전 11 24 54" src="https://user-images.githubusercontent.com/29244798/194745137-952d01ec-bb15-4b16-82d6-b564f3a67405.png">


**객체를 복사해 생성하는 비용을 절약하여 성능을 향상시키기 위해 객체는 변경 가능한 값으로 설계되어 있다.**

하지만 이러한 구조적 단점에 따른 부작용이 있다. 그것은 원시 값과는 다르게 **여러개의 식별자가 하나의 객체를 공유할 수 있다**는 것이다.

*얕은 복사`shallow copy`와 깊은 복사`deep copy`*얕은 복사는 객체의 한 단계까지만 복사하는 것을 말하고,깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사하는 것을 말한다.

```jsx
const o = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...o }; // 35장 "스프레드 문법" 참고
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// lodash의 cloneDeep을 사용한 깊은 복사
// "npm install lodash"로 lodash를 설치한 후, Node.js 환경에서 실행
const _ = require('lodash');
// 깊은 복사
const c2 = _.cloneDeep(o);
console.log(c2 === o); // false
console.log(c2.x === o.x); // false
```

**얕은 복사**

객체에 중첩되어 있는 객체의 경우 참조 값을 복사

중첩된 객체인 경우 Object.assign, spread operator 를 활용할 수 있다.

**깊은 복사**

객체에 중첩되어 있는 객체까지 모두 복사해서 원시 값처럼 완전한 복사본을 만듦

중첩된 객체인 경우 lodash cloneDeep, 브라우저 환경에서는 `JSON` 객체를 활용할 수 있다.

원시값을 다른 변수에 할당하는 것을 **깊은 복사**

객체를 할당한 변수를 다른 변수에 할당하는 것을 **얕은 복사** 라고 부르는 경우도 있다.

```jsx
const v = 1;

// "깊은 복사"라고 부르기도 한다.
const c1 = v;
console.log(c1 === v); // true

const o = { x: 1 };

// "얕은 복사"라고 부르기도 한다.
const c2 = o;
console.log(c2 === o); // true
```

**참조에 의한 전달**

```jsx
var person = {
  name: 'Lee'
};

// 참조값을 복사(얕은 복사)
var copy = person;
```

**두 개의 식별자가 하나의 객체를 공유**

원본 `person` 과 사본 `copy` 는 저장된 메모리 주소는 다르지만 동일한 참조 값을 갖기 때문에 동일한 객체를 가리킨다.

그래서 둘 중 하나를 변경(변수에 새로운 객체를 재 할당하는 것이 아니라, 프로퍼티 값을 변경하거나, 추가, 삭제)하면 **서로 영향을 주고 받는다.**

```jsx
var person = {
  name: 'Lee'
};

// 참조값을 복사(얕은 복사). copy와 person은 동일한 참조값을 갖는다.
var copy = person;

// copy와 person은 동일한 객체를 참조한다.
console.log(copy === person); // true

// copy를 통해 객체를 변경한다.
copy.name = 'Kim';

// person을 통해 객체를 변경한다.
person.address = 'Seoul';

// copy와 person은 동일한 객체를 가리킨다.
// 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다.
console.log(person); // {name: "Kim", address: "Seoul"}
console.log(copy);   // {name: "Kim", address: "Seoul"}
```

`값에 의한 전달`과 `참조에 의한 전달`은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다는 면에서 동일하다.

다만 **변수에 저장되어 있는 값**이 **원시 값**이냐, **참조 값**이냐의 차이만 있을 뿐이다.

따라서 자바스크립트에는 `참조에 의한 전달`은 존재하지 않고 `값에 의한 전달`만이 존재한다고 할 수 있다.

# 12. 함수

## 12.1 함수란?

함수는 **일련의 과정을 statement 로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것**

함수 내부로 입력을 전달받는 변수를 `매개변수 (parameter)`, 입력을 `인수 (argument)`, 출력을 `반환 값 (return value)` 이라 한다.

```jsx
// add: 함수 이름, x, y 는 매개변수
function add(x, y) {
	return x + y; // 반환값
}

// 2, 5는 인수
add(2, 5);
```

함수는 함수 정의 (function definition) 를 통해 생성하고, **함수를 호출**하면 코드 블록에 담긴 문들이 실행되고 반환 값을 반환한다.

```jsx
// 함수 정의
function add(x, y) {
	return x + y;
}

add(2, 5);
```

## 12.2 함수를 사용하는 이유

- 함수는 몇 번이든 호출할 수 있으므로 **코드의 재사용**이라는 측면에서 매우 유용하다.
- 코드의 중복을 억제하고 재사용성을 높이는 함수는 **유지보수의 편의성을 높이고**, **코드의 신뢰성**을 높인다.
- 함수는 객체 타입의 값으로 이름 (식별자) 을 붙일 수 있다. 함수 이름을 적절하게 붙이면 **코드의 가독성** 을 향상시킨다.

## 12.3 함수 리터럴

함수도 `함수 리터럴`로 생성할 수 있다. 함수 리터럴은 function 키워드, 함수 이름, 매개 변수 목록, 함수 몸체로 구성된다.

```jsx
var f = function add(x, y) {
	return x + y;
}
```

**함수 리터럴의 구성 요소**

- 함수 이름 (식별자)
    - 함수 몸체 내에서만 참조할 수 있는 식별자
    - 생략할 수 있다. 이름이 있는 함수는 기명 함수 (named function), 이름이 없는 함수는 무명/익명 함수(anonymous function) 이라 한다.
- 매개변수 목록
    - 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분
    - 매개변수 목록은 순서가 있다.
    - 함수 몸체 내에서 변수와 동일하게 취급
- 함수 몸체
    - 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록
    - 함수 몸체는 함수 호출에 의해 실행

함수 리터럴도 평가되어 값을 생성하며, 이 값은 객체다. 즉, **함수는 객체다.**

**일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.**

## 12.4 함수 정의

함수 선언문이 평가되면 식별자가 암묵적으로 생성되고 함수 객체가 할당되기 때문에 변수는 `선언` 이라고 하지만 함수는 `정의` 라고 표현한다.

- 함수 선언문
    
    ```jsx
    function add(x, y) {
    	return x + y;
    }
    ```
    
- 함수 표현식
    
    ```jsx
    var add = function (x, y) {
    	return x + y;
    }
    ```
    
- Function 생성자 함수
    
    ```jsx
    var add = new Function('x', 'y', 'return x + y');
    ```
    
- 화살표 함수 (ES6)
    
    ```jsx
    var add = (x, y) => x + y;
    ```
    

### 12.4.1 함수 선언문

```jsx
function add(x, y) {
	return x + y;
}

// 함수 선언문은 함수 이름을 생략할 수 없다.
function (x, y) {
	return x + y;
}
```

- 함수 선언문은 함수 이름을 생략할 수 없다.
- **함수 선언문은 표현식이 아닌 문**이다.
- **자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당**한다.

### 12.4.2 함수 표현식

**자바스크립트의 함수는 일급 객체다.**

→ 값처럼 변수에 할당할 수도 있고 프로퍼티 값이 될 수도 있으며 배열의 요소가 될 수 있는 것처럼 값의 성질을 갖는 객체를 일급 객체라고 한다.

함수 리터럴로 생성한 함수 객체를 변수에 할당하는 형태로 정의하는 것을 `함수 표현식` 이라고 한다.

함수 이름을 생략하는 익명 함수로 만들 수 있다.

```jsx
var add = function (x, y) {
	return x + y;
}
```

함수 선언문은 **표현식이 아닌 문** 이고 함수 표현식은 **표현식인 문** 이다.

### 12.4.3 함수 생성 시점과 호이스팅

함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있다.

그러나, 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출할 수 없다.

→ **함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르다.**

```jsx
console.dir(add); // f add(x,y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // sub is not a function
```

`함수 선언문`은 코드의 선두로 끌어 올려진 것처럼 **함수 호이스팅** 이 동작한다.

→ 런타임 이전에 함수 객체가 먼저 생성되고, 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 생성된 함수 객체를 할당하기 때문이다.

`함수 표현식`은 변수에 할당되는 값이 함수 리터럴인 문

함수 표현식으로 함수를 정의하면 **함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생**한다.

### 12.4.4 Function 생성자 함수

```jsx
var add = new Function('x', 'y', 'return x +y');

console.log(add(2, 5));
```

Function 생성자 함수로 생성한 함수는 **closure 를 생성하지 않는 등, 함수 선언문이나 함수 표현식으로 생성한 함수와 다르게 동작**한다.

### 12.4.5 화살표 함수

ES6 에서 도입된 화살표 함수(arrow function) 는 function 키워드 대신 ⇒ 를 사용해 함수를 선언할 수 있다.

화살표 함수는 **항상 익명 함수로 정의**된다.

```jsx
const add = (x, y) => x + y;
console.log(add(2,5)); // 7
```

생성자 함수로 사용할 수 없고, 기존 함수와 this 바인딩 방식이 다르고, prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않는다.

## 12.5 함수 호출

### 12.5.1 매개변수와 인수

`매개변수`는 함수를 정의할 때 선언하며, 함수 몸체 내부에서 변수와 동일하게 취급된다.

함수 몸체 내부에서만 참조할 수 있고 외부에서는 참조할 수 없다.

```jsx
function add(x,y) {
	return x + y;
}

add(2, 5);
console.log(x, y); // x is not defined
```

- 인수가 부족해서 인수가 할당되지 않은 매개변수의 값은 undefined 다.
- 매개변수보다 인수가 더 많은 경우 초과된 인수는 무시된다.
    - 그냥 버려지는 것은 아니고 암묵적으로 arguments 객체의 프로퍼티로 보관된다.

### 12.5.2 인수 확인

1. 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않음
2. 동적 타입 언어로 매개변수의 타입을 사전에 지정할 수 없음

→ 함수 내부에서 적절한 인수가 전달되었는지 확인하거나 arguments 객체를 통해 인수 개수를 확인할 수도 있다.

```jsx
function add(a = 0, b = 0, c, = 0) {
	return a + b + c;
}

add(1); // 1
```

ES6 에서 도입된 매개변수 기본값을 사용할 수 있다. 매개변수 기본값은 인수를 전달하지 않았을 경우와 undefined 를 전달한 경우에만 유효하다.

### 12.5.3 매개변수의 최대 개수

이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 한다.

이상적인 **매개변수 개수는 0개이며 최대 3개 이상을 넘지 않는 것을 권장**한다.

→ 그 이상의 매개변수가 필요하다면 하나의 매개변수를 선언하고 객체를 인수로 전달하는 것이 유리하다. 매개변수의 순서를 신경쓰지 않아도 된다.

### 12.5.4 반환문

`return` 키워드와 표현식으로 이뤄진 반환문을 사용하여 실행 결과를 함수 외부로 반환할 수 있다.

```jsx
function multiply(x, y) {
	return x * y; // 반환문
}
```

1. 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나간다.
2. return 키워드 뒤에 오는 표현식을 평가해 반환한다. 반환값으로 사용할 표현식을 명시적으로 지정하지 않으면 undefined 가 반환된다.
3. 전역에서 반환문을 사용하면 문법 에러가 발생한다.

## 12.6 참조에 의한 전달과 외부 상태의 변경

## 12.7 다양한 함수의 형태

### 12.7.1 즉시 실행 함수

즉시 실행 함수란 **함수 정의와 동시에 즉시 호출되는 함수**이다. 단 한번만 호출되며 다시 호출할 수 없다.

익명 함수를 사용하는 것이 일반적이지만 기명 즉시 실행 함수도 사용할 수 있다.

```jsx
(function () {
	var a = 3;
	var b = 5;
	return a * b;
}());
```

### 12.7.2 재귀 함수

함수가 자기 자신을 호출하는 것을 **재귀 호출** 이라고 한다. 재귀 호출을 수행하는 함수를 `재귀 함수`라고 한다.

```jsx
function countdown(n) {
	if (n < 0) return;
	countdown(n - 1); // 재귀 호출
}

countdown(10);
```

재귀 함수 내에는 재귀 호출을 멈출 수 있는 **탈출 조건** 을 반드시 만들어야 한다.

탈출 조건이 없으면 함수가 무한 호출 되어 스택 오버플로 에러가 발생한다.

### 12.7.3 중첩 함수

함수 내부에 정의된 함수를 `중첩 함수` 또는 내부 함수라고 한다. 중첩 함수를 포함하는 함수는 외부 함수라 부른다.

```jsx
function outer() {
	var x = 1;

	function inner() {
		var y = 2;
		// 외부 함수의 변수를 참조 할 수 있다.
		console.log(x + y); // 3
	}

	inner();
}

outer();
```

### 12.7.4 콜백 함수

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 **콜백 함수** 라고 하며, 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수 (HOF, Higher-Order Function) 라고 한다. 

고차 함수는 매개변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출한다.

```jsx

// logOdds 함수는 단 한번만 생성된다.
var logOdds = function (i) {
	if (i % 2) console.log(i);
}

// 고차 함수에 함수 참조를 전달한다.
repeat(5, logOdds); // 1 3
```

### 12.7.5 순수 함수와 비순수 함수

- 순수 함수
    - 부수 효과가 없는 함수
    - 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수
    - 인수의 불변성을 유지
- 비순수 함수
    - 외부 상태에 의존하거나 외부 상태를 변경하는, 즉 부수 효과가 있는 함수
    - 함수 내부 상태에만 의존한다 해도 내부 상태가 호출될 때마다 변화하는 값 (예: 현재 시간) 이라면 순수함수가 아니다.

**함수형 프로그래밍** 은 순수 함수와 보조 함수의 조합을 통해 외부 상태를 변경하는 부수효과를 최소화해서 불변성을 지향하는 프로그래밍 패러다임이다.

결국 순수 함수를 통해 부수 효과를 최대한 억제해 오류를 피하고 프로그램의 안정성을 높이려는 노력의 일환이라 할 수 있다.
