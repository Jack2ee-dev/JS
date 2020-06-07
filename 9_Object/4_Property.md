## 프로퍼티의 속성

- 프로퍼티의 속성

1. 쓰기 가능(writable)
2. 열거 가능(enumerable)
3. 재정의 가능(configurable)

객체에 새로운 프로퍼티를 추가하면 기본적으로 그 프로퍼티의 기본속성은 '쓰기 가능/열거 가능/재정의 가능'으로 설정됩니다. 내장 생성자가 가지고 있는 프로토타입 객체의 프로퍼티 대부분의 내장 속성은 '쓰기 가능/열거 불가능/재정의 가능'입니다.

## 프로퍼티 디스크립터와 프로퍼티를 읽고 쓰는 메서드

1. 프로퍼티 디스크립터(프로퍼티 기술자)는 프로퍼티의 속성 값을 뜻하는 객체입니다.

```
// 데이터 프로퍼티
{
  value: 프로퍼티의 값(원시 값, 함수의 참조),
  writable: 논리값,
  enumerable: 논리값,
  configurable: 논리값
}
```

```
// 접근자 프로퍼티
{
  get: getter함수값,
  set: setter함수값,
  enumerable: 논리값,
  configurable: 논리값
}
```

2. 프로퍼티 디스크립터 가져오기: Object.getOwnPropertyDescriptor
   Object.getOwnPropertyDescriptor 메서드는 객체 프로퍼티의 프로퍼티 디스크립터를 가져옵니다. 첫 번째 인수는 객체의 참조고 두 번째 인수는 프로퍼티 이름을 뜻하는 문자열입니다.

- 예)

```
var tom = { name: 'Tom' };
Object.getOwnPropertyDescriptor(tom, 'name'); // -> {value: 'Tom', writable: true, enumerable: true, configurable: true}
```

3. 객체의 프로퍼티 설정하기: Object.defineProperty
   Object.defineProperty 메서드는 객체의 프로퍼티에 프로퍼티 디스크립터를 설정합니다. 첫 번째 인수는 객체의 참조, 두 번째 인수는 프로퍼티의 이름을 뜻하는 문자열, 세 번째 인수는 프로퍼티 디스크립터의 참조입니다.

```
var obj = {};
Object.defineProperty(obj, 'name', {
  value: 'Tom',
  writable: true,
  enumerable: false,
  configurable: true
});
Object.getOwnePropertyDescriptor(obj, 'name'); // { value: 'Tom', writable: true, enumerable: false, configurable: true }
```

이 메서드를 사용할 때 프로퍼티 디스크립터의 각 프로퍼티는 생략할 수 있습니다. 디스크립터의 일부 프로퍼티를 생략한 후에 특정 객체에 새로운 프로퍼티를 추가하면 새로운 프로퍼티의 속성 값 중에서 프로퍼티 디스크립터에서 생략한 프로퍼티에 대응하는 속성 값은 true로 설정됩니다.

4. 객체의 프로퍼티 속성 여러 개를 한꺼번에 설정하기: Object.defineProperties
   Object.defineProperties 메서드는 객체가 가진 프로퍼티 여러 개에 각각의 프로퍼티 디스크립터를 설정하며 첫 번째 인수는 객체의 참조입니다. 두 번째 인수는 새롭게 설정 또는 변경하고자 하는 프로퍼티의 이름이 키로 지정된 프로퍼티 여러 개가 모인 객체이며, 각 키 값은 프로퍼티 디스크립터의 참조입니다.

- 예)

```
var person = Object.defineProperties({}, {
  _name: {
    value: 'Tom',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  name: {
    get: function() {return _name},
    set: function(value) {
      var str = value.charAt(0) + value.substring(1);
      _name = str;
    },
    enumerable: true,
    configurable: true
  }
})
Object.getOwnPropertyDescriptor(person, 'name'); // -> { get: [Function: get], set: [Function: set], enumerable: true, configurable: true }
```

## Object.create의 두 번째 인수

- 예)

```
var group = {
  groupName: 'Tennis circle',
  sayGroupName: function() { console.log('belong to ' + this.groupName); };
};
var persone = Object.create(group, {
  name: {
    value: 'Tom',
    writable: true,
    enumerable: true,
    configurable: true
  },
  age: {
    value: 18,
    writable: true,
    enumerable: true,
    configurable: true
  },
  sayName: {
    value: function() { console.log("I'm " + this.name); },
    writable: true,
    enumerable: false,
    configurable: true
  }
})
```

## in 연산자

in 연산자는 객체 안에 지명한 프로퍼티가 있는지 검색하며, 그 검색 대상은 그 객체가 소유한 프로퍼티와 상속받은 프로퍼티 모두입니다.

- 예)

```
var person = { name: 'Tom' };
console.log('name' in person); // true
console.log('age' in person); // false
console.log('toString' in person); // true
```

## hasOwnProperty 메서드

hasOwnProperty 메서드는 지명한 프로퍼티가 그 객체가 소유한 프로퍼티면 true를 반환하고 상속받은 프로퍼티면 false를 반환합니다.

## propertyIsEnumerable 메서드

propertyIsEnumerble 메서드는 지정한 프로퍼티가 그 객체가 소유한 프로퍼티이며 열거할 수 있을 때 true를 반환합니다. 다른 객체로부터 상속받았거나 열거할 수 없을 때에는 false를 반환합니다.

## for/in 문

for/in 문은 객체와 객체의 프로토타입 체인에서 열거할 수 있는 프로퍼티를 찾아내어 꺼내는 반복문입니다. Object.prototype의 프로퍼티는 열거할 수 없으므로 for/in문으로 찾아낼 수 없습니다.

## Object.keys 메서드

Object.keys 메서드는 지정한 객체가 소유한 프로퍼티 중에서 열거할 수 있는 프로퍼티 이름만 배열로 만들어서 반환합니다.

## Object.getOwnPropertyNames 메서드

Object.getOwnPropertyNames 메서드는 인수로 지정한 객체가 소유한 프로퍼티 이름을 배열로 만들어서 반환합니다. 그때 열거할 수 있는 프로퍼티와 열거할 수 없는 프로퍼티의 이름을 모두 배열로 만드는 것이 특징힙니다.
