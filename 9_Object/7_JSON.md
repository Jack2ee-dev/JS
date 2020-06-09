## JSON

JSON(JavaScript Object Notation)은 자바스크립트 객체를 문자열로 표현하는 데이터 포맷입니다. JSON을 사용하면 객체를 **직렬화**할 수 있습니다. 직렬화란 컴퓨터의 메모리 속에 있는 객체를 똑같은 객체로 환원할 수 있는 문자열로 변환하는 과정을 말합니다.

## JSON의 변환과 환원

1. 자바스크립트 객체를 JSON 문자열로 변환하기: JSON.stringify
   JSON.stringify 메서드는 인수로 받은 객체를 JSON 문자열로 바꾸어 반환합니다.

```
JSON.stringify(value[, replacer[, space]])
```

첫 번째 인수인 value에는 JSON으로 변환할 객체를 지정합니다. 두 번째 인수인 replacer에는 함수 또는 배열을 지정합니다. 두 번째 인수로 함수를 지정하면 문자열로 만드는 프로퍼티의 키와 값을 함수의 인수로 받아서 프로퍼티 값을 표현하는 문자열을 반환합니다. 배열을 지정하면 배열의 요소로 객체의 프로퍼티 이름을 필터링합니다. 세 번째 인수인 space에는 출력하는 문자열을 구분할 때 사용할 공백 문자를 지정합니다.

- 주의할 점

> NaN, Infinity, -Infinity는 null로 직렬화된다.
> Date 객체는 ISO 포맷의 날짜 문자열로 직렬화된다. 단, JSON.parse은 이 문자열을 그래도 출력한다.
> Function, RegExp, Error 객체, undefined, 심벌은 직렬화할 수 없다.
> 객체 자신이 가지고 있는 **열거 가능한 프로퍼티**만 직렬화된다.
> 직렬화할 수 없는 프로퍼티는 문자열로 출력되지 않는다.
> 프로퍼티 중에서 키카 심벌이 프로퍼티는 직렬화되지 않는다.

2. JSON 문자열을 자바스크립트 객체로 환원하기: JSON.parse
   JSON.parse 메서드는 인수로 받은 문자열을 자바스크립트 객체로 환원해서 반환합니다.

```
JSON.parse(text[, receiver])
```

## JSON을 활용한 객체의 깊은 복사

객체를 단순히 변수에 대입하면 얕은 복사를 합니다. JSON.stringify와 JSON.parse 메서드를 사용하면 깊은 복사를 할 수 있습니다.

```
var copy = JSON.parse(JSON.stringify(obj));
```

이 방법을 사용하면 중첨된 객체도 올바르게 복사할 수 있습니다. 단, 가장 아래에 중첩된 객체의 프로퍼티(말단 프로퍼티)는 값의 타입이 원시 타입이어야 합니다.
하지만 이 방법은 말단 프로퍼티 값이 Date, function, RegExp 객체이거나 프로퍼티 이름이 심벌일 때는 올바르게 동작하지 않습니다. 안전하게 깊은 복사를 하기 위해서는 Object.assign 메서드를 사용해야 합니다.

```
var obj1 = {x:1, y:2};
var obj2 = Object.assign({}, obj1);
console.log(obj1 === obj2) // -> false
```
