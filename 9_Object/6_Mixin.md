## Mixin 함수

믹스인이란 특정 객체에 다른 객체가 가지고 있는 프로퍼티를 붙여 넣어 '뒤섞는' 기법을 말합니다. 믹스인은 상속을 사용하지 않는 대신에 특정 객체의 프로퍼티를 동적으로 다른 객체에 추가합니다.

- Mixin 함수

```
function mixin(target, source) {
  for (var property in source) {
    if (source.hasOwnProperty(property)) {
      target[property] = source[property];
    }
  }
  return target;
}

var obj1 = { a:1, b:2 };
var obj2 = { b:3, c:4 };
var obj = mixin(obj1, obj2);
console.log(obj1) // -> {a:1, b:3, c:4}
```

결과에서 볼 수 있듯이 mixin 함수는 source 객체가 소유하고 있으며 열거할 수 있는 모든 프로퍼티를 target 객체에 복사합니다. 이때 이미 target 객체에 있는 프로퍼티 값은 덮어쓰고 target 객체에 없는 프로퍼티는 추가합니다. 이때 덮어쓰기나 추가할 때 사용하는 방식은 얕은 복사입니다.

## 좀 더 완전한 Mixin 함수

위의 코드의 mixin 함수에는 원본 객체가 접근자 프로퍼티를 가지고 있을 때 접근자 프로퍼티도 데이터 프로퍼티로 바꾸어 복사하는 문제가 있습니다.

- 예)

```
  var person1 = {
  _name: 'Tom',
  get name() {
  return this._name;
  }
  };
  var person2 = {};
  mixin(person2, person1);
  person2.name = 'Huck';
  console.log(person2.name); // -> Huck
  console.log(person2) // -> {_name: 'Tom', name: 'Huck'}
```

mixin 함수가 person1 객체의 프로퍼티를 person2 객체에 복사할 때 단순히 새로운 프로퍼티를 대상 객체에 추가한 다음 원본 객체의 프로퍼티가 복사하는 시점에 가지고 있던 값을 할당해 버리기 때문입니다.
객채의 접근자 프로퍼티를 다른 객체에 믹스인하려면 mixin 함수에서 프로퍼티를 생성할 때 아래와 같이 Object.defineProperty 메서드를 사용해야 합니다.

- 예)

```
function mixin(target, source) {
  var keys = Object.keys(source);
  for (var i = 0; i < keys.length; i++) {
    var descriptor = Object.getOwnPropertyDescriptor(source, keys[i]);
    Object.defineProperty(target, keys[i], descriptor);
  }
  return target;
}
```
