# 19장 프로토타입

자바스크립트는 **멀티 패러다임 프로그래밍 언어**다.

명령형, 함수형, 프로토타입 기반, 객체지향 프로그래밍을 지원한다.

자바스크립트는 **객체 기반의 프로그래밍 언어**이며 **원시 타입의 값을 제외한 나머지 값들은 모두 객체**다.

🔍 **클래스(class)**

ES6에서 클래스가 도입되었다.

클래스도 함수이며, 기존 프로토타입 기반 패턴의 문법적 설탕(syntatic sugar)이라고 볼 수 있다.

클래스는 생성자 함수보다 엄격하며 생성자 함수에서 제공하지 않는 기능도 제공한다.

따라서, 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕으로 보기보다는 새로운 객체 생성 메커니즘으로 보는것이 좀 더 합당하다.

## 19.1 객체지향 프로그래밍

**객체지향 프로그래밍**

프로그램을 명렁어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나, **여러개의 독립적 단위인 객체(`object`)의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임**을 말한다.

사람에게는 다양한 속성이 있으나 우리가 구현하려는 프로그램에서는 사람의 “이름”과 “주소”라는 속성에만 관심이 있다고 가정하자. 이처럼 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **추상화**`abstraction`라 한다.

```jsx
// 이름과 주소 속성을 갖는 객체
const person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(person); // {name: "Lee", address: "Seoul"}
```

객체지향 프로그래밍은 객체의 **상태를 나타내는 데이터 (**`property`**)**, **상태 데이터를 조작할 수 있는 동작 (**`method`**)** 을 하나의 논리적인 단위로 묶어 생각한다.

객체는 **상태 데이터, 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조**라고 할 수 있다.

```jsx
const circle = {
  // property
  radius: 5, // 반지름

  // method
  // 원의 지름: 2r
  getDiameter() {
    return 2 * this.radius;
  },

  // method
  // 원의 둘레: 2πr
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // method
  // 원의 넓이: πrr
  getArea() {
    return Math.PI * this.radius ** 2;
  }
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter());  // 10
console.log(circle.getPerimeter()); // 31.41592653589793
console.log(circle.getArea());      // 78.53981633974483
```

## 19.2 상속과 프로토타입

**상속**은 **어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용**할 수 있는 것을 말한다. 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비한다.

**프로토타입을 기반으로 상속을 구현**하여 불필요한 중복을 제거해 보자.

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

Circle 생성자 함수가 생성한 모든 인스턴스의 부모 객체 역할을 하는 Circle.prototype 의 모든 프로퍼티와 메서드를 상속받는다.

`getArea` 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype 의 메서드로 할당되어 있어서 모든 인스턴스는 `getArea` 메서드를 상속받아 사용할 수 있다.

## 19.3 프로토타입 객체

모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가지고, 이 내부 슬롯의 값은 프로토타입의 참조다.

`[[Prototype]]` 에 저장되는 프로토타입은 **객체 생성 방식에 의해 결정되고 저장**된다.

모든 객체는 하나의 프로토타입을 갖고, 모든 프로토타입은 생성자 함수와 연결되어 있다.

`[[Prototype]]`  내부 슬롯에 직접 접근할 수 없고, `__proto__` 접근자 프로퍼티로 접근할 수 있다.

#### P.272 그림 19-8

프로토타입은 자신의 `constructor` 프로퍼티를 통해 생성자 함수에 접근할 수 있다.

생성자 함수는 자신의 `prototype` 프로퍼티를 통해 프로토타입에 접근할 수 있다.

### 19.3.1 __proto__ 접근자 프로퍼티

모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.


**__proto__ 는 접근자 프로퍼티다.**

직접 [[Prototype]] 에 접근할 수 없고 이에 접근할 수 있는 역할을 해준다.

**__proto__  접근자 프로퍼티는 상속을 통해 사용된다.**

`__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype 의 프로퍼티이다. 상속을 통해 Object.prototype.__proto__ 접근자 프로퍼티를 사용할 수 있다.

```jsx
const person = { name: 'Lee' };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty('__proto__')); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

**__proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 **상호 참조에 의해 프로토 타입 체인이 생성되는 것을 방지하기 위해서다.**

```jsx
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

**프로토타입은 단방향 링크드 리스트로 구현**되어야 하는데 위 코드가 정상적으로 처리되면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어진다.

⚠️ 프로토타입 체인 종점이 존재하지 않으면 프로토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠진다.

**__proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**

`__proto__` 는 ES5 까지는 비표준이었지만 ES6 에 표준으로 채택되었다.

그러나 직접 상속을 통해 `__proto__` 접근자 프로퍼티를 사용할 수 없는 경우가 있어서 코드 내에서 직접 사용하는 것은 권장하지 않는다.

```jsx
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

 `__proto__` 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우 Object.getPrototypeOf 메서드를

프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.

```jsx
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 19.3.2 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 `prototype`프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

```jsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype'); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // -> false
```

prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

생성자 함수로 호출할 수 없는 함수, **non-constructor 인 화살표 함수, ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않는다.**

```jsx
// 화살표 함수는 non-constructor다.
const Person = name => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {}
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
| __proto__접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

⚠️ `__proto__`접근자 프로퍼티 === 함수 객체의 `prototype`프로퍼티

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__);  // true
```

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 `constructor` 프로퍼티를 갖는다.

constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person);  // true
```

me 객체에는 constructor 프로퍼티가 없지만 me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결된다.

me 객체의 프로토타입인 `me.__proto__` → `Person.prototype` 에는 constructor 프로퍼티가 있어 이를 상속받아 사용할 수 있다.

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수의 프로토타입

new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않고 **리터럴 표기법에 의한 객체 생성 방식** 이 있다.

**리터럴로 생성하는 방식**

```jsx
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) { return a + b; };

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexp = /is/ig;
```

리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재한다.

하지만 **constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 함수라고 단정할 수 없다.**

```jsx
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

![스크린샷 2022-10-23 오후 8.23.27.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1c3606e-2db0-4b82-ab52-b954d447c07f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-23_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.23.27.png)

2번에서 Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null 을 인수로 전달하면서 호출하면 내부적으로 OrdinaryObjectCreate 를 호출하여 `Object.prototype` 을 프로토타입으로 갖는 빈 객체를 생성한다.

```jsx
// 1. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 2. Object 생성자 함수에 의한 객체 생성
// Object 생성자 함수는 new 연산자와 함께 호출하지 않아도 new 연산자와 함께 호출한 것과 동일하게 동작한다.
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Object();
console.log(obj); // {}

// 3. 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}

// String  객체 생성
obj = new Object('123');
console.log(obj); // String {"123"}
```

Object 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 `OrdinaryObjectCreate` 를 호출하여 빈 객체를 생성하는 점에서 동일하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다. 따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.

🔍 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다. 프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다. 다시 말해, `프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍pair으로 존재한다.`

## 19.5 프로토타입의 생성 시점

프로토타입은 **생성자 함수가 생성되는 시점에 더불어 생성**된다.

**프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재**한다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, **즉 constructor 는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.**

```jsx
// 호이스팅으로 선언문에 도달하기 전에 함수 객체가 생성된다.
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. 따라서 함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다. 이 때 프로토타입도 더불어 생성된다.

→ 다시 말해, 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입의 프로토타입은 언제나 **Object.prototype** 이다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

`Object`, `Striong`, `Number`, `Function`, `Array`, `RegExp`, `Date`, `Promise`등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

**모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성**된다.

**🔍 전역 객체 (global object)**

전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 생성되는 특수한 객체

표준 빌트인 객체인 Object 도 전역 객체의 프로퍼티이며, 전역 객체가 생성되는 시점에 생성된다.

```jsx
window.Object === Object; // true
```

## 19.6 객체 생성 방식과 프로토타입의 결정

**객체 생성 방식**

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 **추상 연산 OrdinaryObjectCreate 에 의해 생성**된다.

🔍 **추상연산`OrdinaryObjectCreate`에 의한 객체 생성 순서**

1. 생성할 객체의 프로토타입을 인수로 전달 받는다.
2. 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달한다.
3. 빈 객체를 생성한다.
4. 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 객체에 프로퍼티를 추가한다.
5. 인수로 전달받은 프로토타입을 생성한 객체의 `[[Prototype]]`내부 슬롯에 할당한다.
6. 생성한 객체를 반환한다.

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

```jsx
const obj = { x: 1 };
```

1. 객체 리터럴을 평가한다.
2. OrdinaryObjectCreate 를 호출한다.
3. 이 때 전달되는 프로토타입은 `Object.prototype`

 

Object.prototype 을 프로토타입으로 갖는 obj 객체는 constructor, hasOwnProperty 메서드 등을 소유하지 않지만 사용할 수 있다.

```jsx
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성된다.

추상 연산에 전달되는 프로토타입은 Object.prototype 이다.

```jsx
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

`new` 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상연산 OrdinaryObjectCreate 가 호출된다. 이때 추상 연산에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');
```

## 19.7 프로토타입 체인

자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티/메서드를 검색한다.

객체간의 상속관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다.

**프로토타입 체인** 은 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 자바스크립트 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

```jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty('name')); // true
```

⚠️ **Object.prototype 은 언제나 프로토타입의 종점이다.**

**프로토타입 체인 vs 스코프 체인**

프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘,

스코프 체인은 식별자 검색을 위한 메커니즘으로 **서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용**된다.

## 19.8 오버라이딩과 프로퍼티 섀도잉

**오버라이딩**은 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

**프로퍼티 섀도잉**은 상속 관계에 의해 프로퍼티가 가려지는 현상이다.



🔍 **오버로딩**

함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고, 매개변수에 의해 메서드를 구별하여 호출하는 방식으로 자바스크립트는 지원하지는 않지만 arguments 객체를 사용하여 구현할 수는 있다.

👀 **프로퍼티 삭제**

하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 액세스는 허용되지 않는다.

따라서 프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근해야 한다.

```jsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // ① 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');
```

### 19.9 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경할 수 있다. 다시 말해, 부모 객체인 프로토타입을 동적으로 변경할 수 있다. **생성자 함수** 또는 **인스턴스**에 의해 교체할 수 있다.

### 19.9.1 생성자 함수에 의한 프로토타입 교체

```jsx
const Person = (function(){
  function Person(name){
    this.name = name;
  }
  
  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };
  
  return Person;
}());

const me = new Person('Sujin');
```

위 예제에서 프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다. 따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

이처럼 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다. 프로토타입으로 교체한 **객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살릴 수 있다.**

```jsx
const Person = (function(){
  function Person(name){
    this.name = name;
  }
  
  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
   // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor : Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };
  
  return Person;
}());

const me = new Person('Sujin');
```

### 19.9.2 인스턴스에 의한 프로토타입 교체

프로토타입은 `__proto__` 접근자 프로퍼티를 통해 접근할 수 있고, 이를 통해 프로토타입을 교체할 수 있다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};

// ① me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

<aside>
💡 프로토타입은 직접 교체하지 않는것이 좋다.
인위적으로 설정하려면 직접 상속이 더 편리하고 안전하고, ES6 에서 도입된 클래스를 사용하는 것이 좋다.

</aside>

## 19.10 instanceof 연산자

`객체 instance of 생성자 함수` 

우변의 생성자 함수의 prototype 에 바인딩된 객체가 **좌변의 객체의 프로토타입 체인 상에 존재**하면 true

그렇지 않은 경우 false

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라

**생성자 함수의 prototype 에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인**한다.

## 19.11 직접 상속

### 19.11.1 Object.create 에 의한 직접 상속

`Object.create` 메서드는 명시적으로 **프로토타입을 지정**하여 새로운 객체를 생성한다. 마찬가지로 추상연산`OrdinaryObjectCreate`를 호출하여 객체를 생성한다.

```jsx
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

`Object.create`메서드로 객체를 생성할 때 장점

- `new` 연산자 없이도 객체 생성
- 프로토타입을 지정하면서 객체 생성
- 객체 리터럴에 의해 생성된 객체도 상속 가능

### 19.11.2 객체 리터럴 내부에서 __proto__ 에 의한 직접 상속

ES6 에서는 객체 리터럴 내부에서 `__proto__` 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```jsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto
};

/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 19.12 정적 프로퍼티/메서드

정적 프로퍼티 메서드는 **생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드** 이다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// ① 정적 프로퍼티
Person.staticProp = 'static prop';

// ② 정적 메서드
Person.staticMethod = function () {
  console.log('staticMethod');
};

const me = new Person('Lee');

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// ③ 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

③ 에서 보면 Person 생성자 함수는 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.

Object.create 메서드는 Object 생성자 함수의 정적 메서드

Object.prototype.hasOwnProperty 메서드는 Object.prototype 의 메서드

👀 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만, 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.

```jsx
function Foo() {}

// 프로토타입 메서드
// this를 참조하지 않는 프로토타입 메소드는 정적 메서드로 변경해도 동일한 효과를 얻을 수 있다.
Foo.prototype.x = function () {
  console.log('x');
};

const foo = new Foo();
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 한다.
foo.x(); // x

// 정적 메서드
Foo.x = function () {
  console.log('x');
};

// 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.
Foo.x(); // x
```

## 19.13 프로퍼티 존재 확인

### 19.13.1 in 연산자

`in` 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

```jsx
const person = {
  name: 'Lee',
  address: 'Seoul'
};

// person 객체에 name 프로퍼티가 존재한다.
console.log('name' in person);    // true
// person 객체에 address 프로퍼티가 존재한다.
console.log('address' in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log('age' in person);     // false
```

확인 대상 객체의 프로퍼티 뿐만 아니라 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의해야 한다.

toString 은 Object.prototype 의 메서드다.

```jsx
console.log('toString' in person); // true 
```

`in` 대신 ES6 에서 도입된 Reflect.has 메서드를 사용할 수도 있다.

```jsx
const person = { name: 'Lee' };

console.log(Reflect.has(person, 'name'));     // true
console.log(Reflect.has(person, 'toString')); // true
```

### 19.13.2 Object.prototype.hasOwnProperty 메서드

Object.prototype.hasOwnProperty 메서드를 사용하면 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.

```jsx
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('age'));  // false
```

in 과는 다르게 **상속받은 프로토타입 프로퍼티를 확인하는 경우 false 를 반환**한다.

```jsx
console.log(person.hasOwnProperty('toString')); // false
```

## 19.14 프로퍼티 열거

### 19.14.1 for … in 문

객체의 모든 프로퍼티를 순회하며 열거하려면 for … in 문을 사용한다.

```jsx
for (변수 선언문 in 객체) {...}
```

```jsx
const person = {
  name: 'Lee',
  address: 'Seoul'
};

// for...in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당된다.
for (const key in person) {
  console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

**특징**

- 프로퍼티 키를 순회한다.
- 프로토타입 체인을 모두 순회한다.
- `for...in`문은 객체의 프로토타입 체인 상에 존재하는 모든 프로퍼티 중에서 프로퍼티 어트리뷰트`[[Enumerable]]`값이 `true`인 프로퍼티만 열거한다.
    
    ```jsx
    Object.getOwnPropertyDescriptor(Object.prototype, "toString")
    // {writable: true, enumerable: false, configurable: true, value: ƒ}
    ```
    
- Symbol 을 열거하지 않는다.
    
    ```jsx
    const sym = Symbol();
    const obj = {
      a: 1,
      [sym]: 10
    };
    
    for (const key in obj) {
      console.log(key + ': ' + obj[key]);
    }
    // a: 1
    ```
    
- 순서를 보장하지 않는다. (하지만 대부분의 모던 브라우저는 순서를 보장하고 숫자인 프로퍼티 키에 대해서는 정렬을 실시한다.)

⚠️ 배열에는 for … in 보다는 일반적인 `for 문` 이나 `for ... of` 문이나 `Array.prototype.forEach` 메서드를 사용하기를 권장한다.

배열도 객체이므로 프로퍼티와 상속받은 프로퍼티가 포함될 수 있기 때문이다.

```jsx
const arr = [1, 2, 3];
arr.x = 10; // 배열도 객체이므로 프로퍼티를 가질 수 있다.

for (const i in arr) {
  // 프로퍼티 x도 출력된다.
  console.log(arr[i]); // 1 2 3 10
};

// arr.length는 3이다.
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1 2 3
}

// forEach 메서드는 요소가 아닌 프로퍼티는 제외한다.
arr.forEach(v => console.log(v)); // 1 2 3

// for...of는 변수 선언문에서 선언한 변수에 키가 아닌 값을 할당한다.
for (const value of arr) {
  console.log(value); // 1 2 3
};
```

### 19.14.2 Object.keys/values/entries 메서드

객체 자신의 고유 프로퍼티만 열거하기 위해서는 **Object.keys, Object.values, Object.entries** 메서드 사용을 권장한다.

**Object.keys**

객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.

```jsx
const person = {
  name: 'Lee',
  address: 'Seoul',
  __proto__: { age: 20 }
};

console.log(Object.keys(person)); // ["name", "address"]
```

**Object.values**

객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다. ES8 에서 도입되었다.

```jsx
console.log(Object.values(person)); // ["Lee", "Seoul"]
```

**Object.entries**

객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아서 반환한다. ES8에서 도입되었다.

```jsx
console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seoul"]]

Object.entries(person).forEach(([key, value]) => console.log(key, value));
/*
name Lee
address Seoul
*/
```

# 20장 strict mode

## 20.1 strict mode 란?

**🤔 아래 예제의 실행 결과는 무엇일까?**

```jsx
function foo() {
  x = 10;
}

foo();

console.log(x); // ?
```

- **실행 결과**
    
    전역 스코프에도 변수 선언이 존재하지 않아서 ReferenceError 가 발생할 것 같지만, 자바스크립트 엔진이 **암묵적으로 전역** **객체에 x 프로퍼티를 동적 생성**한다. → **암묵적 전역 (implicit global)**
    

**strict mode** 는 자바스크립트 언어의 문법을 **엄격히 적용**하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다. ES5 부터 추가 되었다.

ESLint 같이 정적 분석 기능을 통해 코드를 실행하기 전에 스캔하여 문법적, 잠재적 오류까지 리포팅해주는 도구를 사용해도 유사한 효과를 얻을 수 있다.

위와 같은 내용과 함께 코딩 컨벤션을 정의하고 강제할 수 있기 때문에 strict mode 보다 **린트 도구 사용을 선호한다.**

## 20.2 strict mode 의 적용

```jsx
'use strict';

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

코드의 선두에 **use strict;** 를 위치시키지 않으면 strict mode 가 제대로 동작하지 않는다.

```jsx
function foo() {
  x = 10; // 에러를 발생시키지 않는다.
  'use strict';
}
foo();
```

## 20.3, 4 strict mode 적용을 피해야 하는 케이스

### 전역에 strict mode 적용 피하기

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict';
  </script>
  <script>
    x = 1; // 에러가 발생하지 않는다.
    console.log(x); // 1
  </script>
  <script>
    'use strict';

    y = 1; // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```

위와 같이 스크립트 단위로 strict mode 를 적용하면 다른 스크립트에 영향을 주지 않고 해당 스크립트에만 적용된다. 다만 **strict mode, non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다**.

외부 라이브러리가 non-strict mode 인 경우도 있으므로 전역에 strict mode 를 적용하는 것은 지양하는 것이 좋다. 이럴 때는 아래와 같이 **즉시 실행 함수로 스코프를 구분하여 strict mode 를 적용**한다.

```jsx
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
```

### 함수 단위로 strict mode 적용 피하기

함수 단위로 strict mode 를 적용하면 어떤 함수는 strict, 다른 함수는 non-strict 로 적용될 수 있는데 해당 케이스는 바람직하지 않다.

또한 아래와 같이 strict mode 가 적용된 함수가 참조할 함수 외부 컨텍스트에 non-strict mode 를 적용하지 않으면 문제가 발생할 수 있으므로 **즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직**하다.

```jsx
(function () {
  // non-strict mode
  var lеt = 10; // 에러가 발생하지 않는다.

  function foo() {
    'use strict';

    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
}());
```

## 20.5 strict mode 가 발생시키는 에러

### 20.5.1 암묵적 전역

선언하지 않은 변수 참조 시 `ReferenceError` 발생

```jsx
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
}());
```

### 20.5.2 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수 삭제 시 `SyntaxError` 발생

```jsx
(function () {
  'use strict';

  var x = 1;
  delete x;
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 20.5.3 매개변수 이름의 중복

중복된 매개변수 이름 사용 시 `SyntaxError` 발생

```jsx
(function () {
  'use strict';

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```

### 20.5.4 with 문의 사용

with 문은 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있어서 사용하지 않는 것이 좋다.

그래서 with 문을 사용하면 `SyntaxError` 가 발생한다.

```jsx
(function () {
  'use strict';

  // SyntaxError: Strict mode code may not include a with statement
  with({ x: 1 }) {
    console.log(x);
  }
}());
```

**🔍 with 문?**

동일한 객체 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있다.

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

생성자 함수가 아닌 일반 함수 내부에서는 this 를 사용할 필요가 없기 때문에 **일반 함수로서 호출하면 this 에 undefined 가 바인딩** 된다.

```jsx
(function () {
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
}());
```

### 20.6.2 arguments 객체

매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```jsx
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
}(1));
```
