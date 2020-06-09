## throw 문

throw 문은 예외를 던집니다.

```
throw 표현식;
```

표현식으로는 어떤 타입의 값도 지정할 수 있습니다. 사용자에게 표시할 오류 메시지가 포함된 문자열이나 오류 코드를 의미하는 숫자도 허용됩니다. 그러나 일반적으로는 Error 객체나 Error 객체를 상속받은 객체를 지정합니다.
예외를 던지면 자바스크립트 인터프리터는 프로그램의 실행을 중단하고 바깥 블록에서 예외를 처리하는 예외 처리기를 찾습니다. 예외 처리기는 바로 try/catch/finally 절 부분의 catch 절입니다. 예외 처리기가 없으면 프로그램을 **종료**합니다.

- 타입의 판정
  예외를 처리할 때 함수가 받은 인수의 타입이 적절한지를 확인하고 적절하지 않을 때 오류를 던지려면 먼저 인수의 타입을 판정해야 합니다. 인수가 원시 값인지 또는 함수 타입인지 판정할 때에는 typeof 연산자를 사용할 수 있습니다.
  예)

```
if (typeof callback != 'function') throw new Error(callback + ' is not a function');
```

그러나 인수가 함수 타입 외의 객체 타입이면 typeof 연산자는 자료형에 관계없이 'object'를 반환합니다. 이때는 instanceof 연산자를 사용합니다. instanceof 연산자는 특정 객체의 프로토타입 체인에 특정 생성자의 프로토타입 객체가 포합되어 있는지를 판정합니다.

```
if (!(map instanceof Map)) throw new Error(map + ' is not a Map object');
```

## Error 객체

- 예외를 표현하는 내장 객체의 생성자
  **Error**: 범용적인 예외 객체
  **EvalError**: eval 함수와 관련해서 발생한 예외 객체
  **RangeError**: 숫자 값이 허용 범위를 벗어났을 때 발생하는 예외 객체
  **ReferenceError**: 잘못된 참조를 만났을 때 발생하는 에외 객체
  **SyntaxError**: 자바스크립트 문법에 어긋나는 구문을 만났을 때 발생하는 예외 객체
  **TypeError**: 변수 및 인수 타입이 유효하지 않을 때 발생하는 예외 객체
  **URIError**: encodeURI와 decodeURI 메서드에 잘못된 인수가 전달되었을 때 발생하는 예외 객체

예외를 표현하는 모든 내장 객체는 Error.prototype의 프로퍼티와 메서드를 상속받습니다. Error.prototype의 프로퍼티는 다음과 같습니다.
**message**: 오류 메시지를 뜻하는 문자열
**name**: 오류 이름을 뜻하는 문자열

Error.protype의 메서드는 다음과 같습니다.
**toString**: 지정된 객체를 표현하는 문자열을 반환

## try/catch/finally 문

```
try {
  // 이곳에 실행할 코드를 적는다(예외가 발생할 수 있는 코드)
} catch(exception) {
  // 이 블록은 try 블록에서 예외가 발생했을 때 실행된다.
  // exception에는 던져진 예외 값이 들어옴. 이 값을 바탕으로 예외를 처리한다.
} finally {
  // 이 블록 안의 코드는
  // try 블록 코드와 catch 블록 코드가 실행된 이후에 반드시 실행된다.
}
```

## 예외의 전파

예외는 호출 스택을 거슬러 올라가며 전파됩니다.

```
try {
  f();
} catch(e) {
  console.log('예외를 캐치함 -> ' + e);
}
function f() {g()};
function g() {h()};
function h() {
  throw new Error('오류가 발생했습니다.');
}
```

위 코드를 실행하면 콘솔에 다음과 같은 내용이 표시됩니다.

```
예외를 캐치함 -> Error: 오류가 발생했습니다.
```

이 예제에서 발생한 예외는 'h -> g -> f -> 전역 코드' 순서대로 호출 스택을 거슬러 올라가 전파되며, 마지막에는 전역 코드에서 전역 코드에서 try/catch 문에 걸려서 처리됩니다. 호출 스택에서 예외 처리기를 찾지 못하면 프로그램이 강제로 종료되면 예외는 사용자 오류로 보고됩니다.
이처럼 예외는 호출 스택의 위 단계로 차례차례 전파되기에 이러한 단계에서 한꺼번에 처리할 수 있습니다. 이 성질을 활용하면 라이브러리에서 발생한 오류의 처리를 라이브러리 사용자에게 위임할 수 있습니다.

## 비동기 처리의 콜백 함수가 던진 예외의 처리

비동기 처리의 콜백 함수가 던진 예외는 콜백 함수를 넘긴 함수로 전파되지 않습니다.

```
try {
  setTimeout(function throwError() {
    throw new Error('오류가 발생했습니다.');
  }, 1000);
} catch(e) {
  console.log('예외를 캐치함 -> ' + e);
}
```

이 코드를 실행하면 콘솔에는 다음과 같은 내용이 표시됩니다. 그리고 예외가 캐치되지 않은 상태(catch 문이 실행되지 않은 상태로)로 프로그램이 종료됩니다.

```
Uncaught Error: 오류가 발생했습니다.
```

예외를 던지는 콜백 함수 throwError는 함수 정의가 try 블록 안에 있을 뿐 try 블록 안에서 호출된 것은 아닙니다. 이 콜백 함수를 호출한 주체는 타이머 이벤트(setTimeout)이며 try 블록 안에서 발생한 예외가 아닙니다. 그래서 예외를 잡을 수 없는 것입니다.
이처럼 비동기 처리의 콜백 함수에서 발생한 예외는 바깥의 try/catch 문으로 잡을 수 없습니다.
ES6부터 추가된 제너레이터를 활용하면 비동기 처리 중에 발생한 예외를 잡을 수 있습니다. 다음 예제에 등장하는 함수 run은 인수로 받은 함수 callback을 실행합니다. 이때 인수로 받은 함수가 비동기 처리를 하는 중에 발생시킨 예외도 잡아서 처리합니다(generator의 특성). 그리고 두 번째 이후 인수로 callback 함수가 원래 받아야 하는 모든 인수를 지정할 수 있습니다(...argsList). callback 함수는 첫 번째 인수로 제너레이터의 참조인 g를 받아야 합니다. 그리고 callback 한수 안에서 예외를 던질 때 g.throw 메서드를 실행합니다.

```
function sleepAndError(g, n) {// g: 제너레이터 함수 참조값
  setTimeout(function() {
    for (var i = 0; i < n; i++) console.log(i);
    g.throw(new Error('오류가 발생했습니다.'));
  }, 1000);
}

// callback 함수를 실행하는 함수: callback 함수가 비동기 처리 중에 발생시킨 예외도 잡아서 처리함
function run(callback, ...argsList) {
  var g = (function* (cb, args) {
    try {
      yield cb(g, ...args);
    } catch(e) {
      console.log('예외를 잡음 -> ' + e);
    }
  })(callback, argsList);
  g.next();
}

// 실행하기
run(sleepAndError, 10);
```

실행 결과는 다음과 같습니다.
0
1
2
_중략_
예외를 잡음 -> Error: 오류가 발생했습니다.

이 예제는 sleepAndError 함수의 실행을 run 함수에 위임합니다. sleepAndError 함수는 setTimeout 함수를 사용해서 약 1초 후에 예외를 던지는 비동기 처리를 하는 함수입니다. sleepAndError 함수가 예외를 던질 때 그 예외를 throw 문이 아니라 첫 번째 인수로 받은 제너레이터 객체 g의 throw 메서드에 던지는 부분이 포인트입니다. 제너레이터 객체 g의 throw 메서드는 호출된 그 자리에서 예외를 던지는 것이 아니라 제너레이터를 한 번 동작시킨 다음에 yield 문이 위치한 자리에서 예외를 던집니다. 그러면 이 예외는 제너레이터가 던지 예외가 되므로 catch 블록 안에서 잡아낼 수 있습니다.

제너레이터의 메서드와 프로퍼티는 다음과 같이 확인할 수 있습니다.

```
var g = function* () {} // 제너레이터 생성
console.log(Object.getOwnPropertyNames(g.__proto__.prototype)) // -> [ 'constructor', 'next', 'return', 'throw' ]
```
