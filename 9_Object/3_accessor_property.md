## 프로퍼티의 종류

1. 데이터 프로퍼티: 값을 저장하기 위한 프로퍼티
2. 접근자 프로퍼티: 값이 없음. 프로퍼티를 읽거나 쓸 때 호출하는 함수를 값 대신에 지정할 수 있는 프로퍼티

## 접근자 프로퍼티

**접근자**란 객체 지향 프로그래밍에서 객체가 가진 프로퍼티 값을 객체 바깥에서 읽거나 쓸 수 있도록 제공하는 메서드를 말합니다. 접근자 프로퍼티를 사용하면 데이터를 부적절하게 변경하는 것을 막고 특정 데이터를 외부로부터 숨길 수 있으며 외부에서 테이터를 읽으려고 시도할 때 적절한 값으로 가공해서 넘길 수 있습니다.

1. 게터 함수(getter): 객체의 프로퍼티를 읽을 때의 처리를 담당합니다.
2. 세터 함수(setter): 객체의 프로퍼티를 쓸 때의 처리를 담당합니다.

- 예)

```
var person = {
  _name = 'Tom'
  get name() {
    return this._name;
  };

  set name(value) {
    var str = value.charAt(0).toUpperCase() + value.substring(1);
    _name = str;
  };
};
console.log(person.name); // -> Tom
person.name = 'huck'; // 접근자 프로퍼티 set에 값을 대입한다.
console.log(person.name); // -> Huck
```

getter가 없는 접근자 프로퍼티를 읽으려고 시도하면 undefined가 반환됩니다. setter가 없는 접근자 프로퍼티를 쓰려고 시도하면 아무것도 실행되지 않으며 제어권이 곧장 호출자에게 되돌아옵니다.

## 데이터의 캡슐화

즉시 실행 함수로 클로저를 생성하면 데이터를 객체 외부에서 읽고 쓸 수 없도록 숨기고 접근자 프로퍼티로만 읽고 쓰도록 만들 수 있습니다.

- 예)

```
var person = (function() {
  var _name = 'Tom';
  return {
    get name() {
      return _name;
    };
    set name(value) {
      var str = value.charAt(0) + value.substring(1);
      _name = str;
    };
  };
})(); // 외부 함수가 즉시 시행되면서 getter와 setter만 반환된다. -> _name은 private 변수로 getter와 setter로만 접근 가능하다(클로저 생성).
console.log(person.name); // Tom
person.name = 'huck';
console.log(perons.name); // Huck
```

위 코드의 변수 \_name은 즉시 실행 함수의 지역 변수이므로 함수 바깥에서 읽거나 쓸 수 없습니다.
