## 상속

상속(inheritance)이란 일반적으로 특정 객체가 다른 객체로부터 기능을 이어받는 것을 말합니다. 자바스크립트는 클래스를 이용하는 객체 지향 언어(C++, Java 등)와는 달린 객체를 상속합니다. 자바스크립트에서는 생성자(new)가 클래스의 역할을 하지만 생성자를 상속하기 위한 구문을 언어 차원에서 제공하지는 않습니다.

## 상속을 하는 이유

상속을 사용하면 이미 정의된 프로퍼티와 메서드의 코드를 재사용할 수 있고 새로운 기능을 추가해서 확장된 객체를 만들 수 있습니다.

## 프로토타입 체인

1. 내부 프로퍼티 [[Prototype]]
   모든 객체는 내부 프로퍼티 [[Prototype]]을 가지고 있습니다. 이것은 함수 객체의 prototype 프로퍼티와는 다른 객체입니다. 현재의 웹 브라우저에서 \_\_proto\_\_ 프로퍼티로 [[Prototype]]에 접근 가능합니다.

2. 프로토타입 체인
   객체의 \_\_proto\_\_ 프로퍼티는 그 객체에게 상속을 해 준 부모 객체를 가리킵니다. 따라서 객체는 \_\_proto\_\_ 프로퍼티가 가리키는 부모 객체의 프로퍼티를 사용할 수 있습니다.

- 예)

```
var objA = {
  name: 'Tom',
  sayHello: function() { console.log('Hello! ' + this.name); }
};
var objB = {
  name: 'Huck'
};
objB.__proto__ = objA;
var objC = {};
objC.__protp__ = objB;
objC.sayHello(); // -> 'Hello Huck'
```

여기서 자신이 갖고 있지 않은 프로퍼티를 \_\_proto\_\_ 프로퍼티가 가리키는 객체를 차례대로 거슬러 올라가며 검색합니다. 이와 같은 객체의 연결 고리를 **프로토타입 체인**이라고 합니다.
여기에서 객체의 \_\_proto\_\_ 프로퍼티가 가리키는 객체가 바로 상속을 해 준 객체이며, 이 객체를 그 객체의 **프로토타입**이라고 합니다. 객체는 자신이 가지고 있지 않은 특성(프로퍼티와 메서드)을 프로토타입 객체에 위임(delegate)한다고 할 수 있습니다. **프로토타입 샹속**을 하는 객체 지향 언어를 가리켜 **프로토타입 기반 객체 지향 언어**라고 합니다. 그러나 \_\_proto\_\_ 프로퍼티 값을 직접 입력해서 상속하지는 않고 다음과 같은 방법으로 상속합니다.

- 생성자로 객체를 생성할 때 생성자의 prototype 프로퍼티에 추가하는 방법
- Object.create 메서드로 상속받을 프로토타입을 지정하여 생성하는 방법

3. 프로토타입 가져오기
   객체의 프로토타입은 Object.getPrototypeOf 메서드로 가져올 수 있습니다. ES6부터는 객체의 프로토타입을 설정하는 메서드인 Object.setPrototypeOf도 추가되었습니다.

4. new 연산자의 역할

```
function Circle(center, radius) {
  this.center = center;
  this.radius = radius;
};
Circle.prototype.area = function() {
  return Math.PI*this.radius*this.radius;
};
```

Circle 생성자로 인스턴스를 생성할 때는 다음과 같이 new 연산자를 사용했습니다.

```
var c = new Circle({x: 0, y: 0}, 2.0);
```

new 연산자로 Circle 생성자를 사용하면 내부적으로는 다음과 같은 작업을 수행합니다.

1) 빈 객체를 생성합니다.

```
var newObj = {};
```

2) Circle.prototype을 생성된 객체의 프로토타입으로 설정합니다.

```
newObj.__proto__ = Circle.prototype;
```

이 때 Circle.prototype이 가리키는 값이 객체가 아니라면(초기값 undefined) Object.prototype을 프로토타입으로 설정합니다.

3) Circle 생성자를 실행하고 newObj를 초기화합니다. 이때 this는 1)로 생성한 객체로 설정합니다. 인수는 new 연산자와 함께 사용한 인수를 그대로 사용합니다.

```
Circle.apply(newObj, arguments);
```

4. 완성된 객체를 결괏값으로 반환합니다.

```
return newObj;
```

이처럼 생성자를 new 연산자로 호출하면 객체의 생성, 프로토타입의 설정, 객체의 초기화를 수행합니다.

## 프로토타입 객체의 프로퍼티

함수를 정의하면 함수 객체는 기본적으로 prototype 프로퍼티를 갖게 됩니다. 그리고 prototype 프로퍼티는 프로토타입 객체를 가리키며, 이 프로토타입 객체는 기본적으로 constructor 프로퍼티와 내부 프로퍼티 [[Prototype]](__proto__)을 가지고 있습니다.

1. constructor
   생성자와 생성자의 프로토타입 객체는 서로를 참조합니다. 정확히는 '생성자의 prototype 프로퍼티가 프로토타입 객체를 가리키며, 이 프로토타입 객체의 constructor 프로퍼티가 생성자를 가리키는 연결 고리'로 묶여 있습니다. 반면 생성자로 생성한 인스턴스는 생성될 때의 프로토타입 객체의 참조만 가지고 있을 뿐 생성자와는 직접적인 연결 고리가 없습니다.

2. 내부 프로퍼티 [[Prototype]]
   함수 객체가 가진 프로토타입 객체의 내부 프로퍼티 [[Prototype]]는 기본적으로 Object.prototype을 가리킵니다.
   이 덕분에 생성자로 생성한 인스턴스가 Object.prototype의 프로퍼티를 사용할 수 있습니다. 또한 Object.prototype의 프로토타입은 null을 가리킵니다.

3. 프로토타입 객체의 교체 및 constructor 프로퍼티
   생성자가 가진 prototype 프로퍼티 값을 새로운 객체로 교체할 때는 주의해야 합니다. 프로퍼티만 정의되어 있는 새로운 객체를 prototype 프로퍼티 값으로 대입하면 인스턴스와 생성자 사이의 연결 고리가 끊겨 버립니다. 그 객체에는 constructor 프로퍼티가 없기 때문입니다(인스턴스가 생성될 때 constructor가 생성자를 참조한다). 인스턴스와 생성자 사이의 연결 고리를 유지하려면 prototype으로 사용할 객체의 constructor 프로퍼티를 정의하고, 그 프로퍼티에 생성자의 참조를 대입해야 합니다.

- 예)

```
function Circle(center, radius) {
  this.center = center;
  this.radius = radius;
};
Circle.prototype = {
  constructor = Circle,
  area: function () {
    return Math.PI*this.radius*this.radius;
  };
};
var c = new Circle({x: 0, y:0}, 2.0);
console.log(c.constructor); // -> Function Circle
console.log(c instanceof Circle); // -> true
```

4. 인스턴스 생성 후에 생성자의 프로토타입을 수정하거나 교체한 경우
   인스턴스의 프로토타입은 생성자가 인스턴스를 생성할 때 가지고 있던 프로토타입 객체입니다. 인스턴스를 생성한 후에 생성자의 prototype 프로퍼티 값을 다른 객체로 교체해도 인스턴스의 프로토타입은 바뀌지 않습니다. 다시 말해 인스턴스의 프로퍼티는 생성되는 시점의 프로토타입에서 참조값을 상속받습니다. 생성된 후에는 생성자의 프로토타입을 바꾸어도 교체한 객체로부터는 프로퍼티를 상속받지 않습니다.

```
function Circle(center, radius) {
  this.center = center;
  this.radius = radius;
};
var c = new Circle({x:0, y:0}, 2.0);
Circle.prototype = {
  constructor = Circle,
  area: function () {
    return Math.PI*this.radius*this.radius;
  };
};
c.area(); // -> Uncaught TypeError: c.area is not a function
```

하지만 생성자가 기존에 가지고 있던 프로토타입 객체에 프로퍼티를 추가한 경우에는 생성자와 인스턴스 사이의 연결 고리가 끊기지 않습니다. 따라서 생성자에서 정의한 프로퍼티를 인스턴스에서 사용할 수 있습니다.

```
function Circle(center, radius) {
  this.center = center;
  this.radius = radius;
};
var c = new Circle({x:0, y:0}, 2.0);
Circle.prototype.area = {
  return Math.PI*this.radius*this.radius;
};
c.area(); // -> 12.56...
```

## 프로토타입의 확인

특정 프로토타입 객체가 그 객체의 **프로토타입 체인**에 포함되어 있는지 확인하는 방법

1. instanceof 연산자

```
객체 instanceof 생성자
```

2. isPrototypeOf

```
프로토타입객체.isPrototypeOf(객체)
// 예시
function F() {};
var obj = new F();
console.log(Object.prototype.isPrototypeOf(obj)); // true
console.log(F.prototype.isPrototypeOf(obj)); // true
console.log(Date.prototype.isPrototypeOf(obj)); // false
```

## Object.prototype

Object.prototype의 메서드는 모든 내장 객체로 전파되며 모든 인스턴스에서 사용할 수 있습니다.

1. Object 생성자
   Object 생성자는 내장 생성자로 일반적인 객체를 생성합니다. Object 생성자를 인수 없이 실행하면 Object 생성자는 빈 객체를 생성합니다.
   Object 생성자는 객체를 생성하는 것보다는 일반적인 객체를 조작하기 위한 메서드와 프로퍼티를 제공하고, Object.prototype으로 모든 내장 생성자 인스턴스에서 사용할 수 있는 메서드를 제공하는 것에 의의가 있습니다.

2. Object 생성자의 프로퍼티와 메서드 & Object.prototype의 메서드
   Link: [MDN][https://developer.mozilla.org/ko/docs/web/javascript/reference/global_objects/object]
   내장 생성자의 모든 인스턴스는 Object.prototype의 프로퍼티와 메서드를 상속하며, Object.prototype의 프로토타입은 null을 가리킵니다. 즉, Object.prototype은 인스턴스에서 프로토타입 체인을 따라 거슬러 올라갈 수 있는 마지막 단계의 객체입니다.
   인스턴스가 직접 상속한 프로토타입(가령, Date.prototype)과 Object.prototype에는 이름이 같은 메서드(가령, toString과 valueOf)가 있지만 프로토타입 체인에서는 호출한 인스턴스와 가까운 Date.prototype의 메서드를 사용합니다.

3. Object.create로 객체 생성하기
   Object.create 메서드를 사용하면 명시적으로 프로토타입을 지정해서 객체를 생성할 수 있습니다. 이 메서드를 사용하면 가장 간단하게 상속을 표현할 수 있습니다.
   Object.create 메서드의 첫 번째 인수는 생성할 객체의 프로토타입입니다. 두 번째 인수를 지정하면 생성할 객체의 프로퍼티도 지정할 수 있습니다(선택 사항).

```
var person1 = {
  name: 'Tom',
  sayHello: function() { console.log('Hello! ' + this.name); }
}
var person2 = Object.create(person1);
person2.name = 'Huck';
person2.sayHello(); // -> 'Hello!  Huck'
```
