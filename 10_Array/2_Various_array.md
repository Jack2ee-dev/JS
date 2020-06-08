## Array 타입의 객체

자바스크립트에서 배열이란 곧 Array 타입의 객체를 말합니다. Array 타입의 객체는 다음과 같은 성질이 있습니다.

- 0 이상의 정수 값을 프로퍼티 이름으로 갖는다.
- length 프로퍼티가 있으며, 요소를 추가하거나 삭제하면 length 프로퍼티 값이 바뀐다. 또한 length 프로퍼티 값을 줄이면 그에 따라 배열 크기가 줄어든다.
- 프로토타입이 Array.prototype이므로 Array.prototype의 메서드를 상속받아서 사용할 수 있다. 또한 instanceof 연산자로 평가하면 Array 생성자 함수로 생성된 객체로 표시된다.

## 유사 배열 객체

Array 타입의 객체의 성질 중 프로퍼티 이름이 0 이상의 정수이며 length 프로퍼티가 있는 객체는 대부분 배열로 다룰 수 있습니다. 이러한 객체를 '유사 배열 객체'라고 합니다. 결국 유사 배열 객체 대부분은 프로토타입이 Array 생성자가 아니라는 점에서 일반 배열과 차이를 보입니다. 따라서 기존의 유사 배열 겍체에서는 Array.prototype의 메서드를 사용할 수 없습니다.

- 예) 함수의 인수를 저장한 Arguments 객체, DOM의 document.getElementByTagName 메서드, document.getElementsByName 메서드 등이 반환하는 NodeList 객체

length가 10인 유사 배열 객체는 다음과 같은 방법으로 직접 생성할 수 있습니다.

```
var a = {};
for (var a = 0; i < 10; i++) {
  a[i] = i;
}
a.length = 10;
```

## Array.prototype의 메서드를 유사 배열 객체에서 사용하기

유사 배열 객체는 Array.prototype의 메서드를 직접 사용할 수 없습니다. 그러나 Function.prototype.call 메서드로 간접 호출하면 사용할 수 있습니다.

- 예)

```
var a = {0: 'A', 1: 'B', 2: 'C', length: 3};
Array.prototype.join.call(a, ','); // -> 'A,B,C'
Array.prototype.push.call(a, 'D'); // -> Object {0: 'A', 1: 'B', 2: 'C', 3: 'D', length: 4}
```

이처럼 Array.prototype의 메서드를 유사 배열 객체에 적용할 수는 있지만 concat 메서드를 제외한 나머지 배열 메서드는 배열처럼 동작하지 않습니다.
