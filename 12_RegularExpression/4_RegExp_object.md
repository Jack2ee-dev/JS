## RegExp 객체의 프로퍼티

RegExp 객체는 RegExp.prototype에서 프로퍼티와 메서드를 상속받습니다. 다음이 RegExp.prototype의 프로퍼티 목록입니다.

1. source: 정규 표현식 패턴 문자열을 저장한다.
2. global: g 플래그가 사용되고 있는지를 뜻하는 논리값, 읽기 전용
3. ignoreCase: i 플래그가 사용되고 있는지를 뜻하는 논리값, 읽기 전용
4. multiLine: m 플래그가 사용되고 있는지를 뜻하는 논리값, 읽기 전용

## RegExp의 메서드

1. exec 메서드
   exec 메서드는 정규 표현식과 일치하는 문자열을 검색해서 **일치한 문자열**과 **부분 정규 표현식에 일치한 문자열**을 배열에 담아 반환합니다. 일치하는 문자열을 찾지 못했을 때는 null을 반환합니다. exec 메서드가 반환하는 결과는 String 객체의 match 메서드에서 g 플래그를 설정하지 않았을 때 반환하는 결과와 같습니다.

```
var tel1 = /(\d{2,5})-(\d{1,4})-(\d{1,4})/;
var tel2 = /(\d{2,5})-(\d{1,4})-(\d{1,4})/g;
var text = 'Tom: 010-1234-5678';
var result1 = tel1.exec(text);
var result2 = tel2.exec(text);
console.log(result1); // -> ['010-1234-5678', '010', '1234', '5678']
console.log(result2); // -> ['010-1234-5678']
```

2. lastIndex 메서드
   g 플래그를 설정한 정규 표현식으로 exec나 test 메서드를 실행하면 일치한 문자열 바로 다음 번 문자의 위치가 정규 표현식 객체의 lastIndex 프로퍼티에 저장됩니다. g 플래그를 설정하지 않으면 최초 1번의 검색 결과만 일치하지 않을 때는 0이 저장됩니다.

```
var tel = /(\d{2,5})-(\d{1,4})-(\d{1,4})/g;
var text = 'Tom: 010-1234-5678\nHuck: 010-2345-6789\nBecky: 030-4321-9876';
console.log(tel.lastIndex); // -> 0
console.log(tel.exec(text)); // -> ['010-1234-5678', '010', '1234', '5678']
console.log(tel.lastIndex); // -> 18
```

같은 정규 표현식 객체로 다시 한번 exec나 test 메서드를 호출하면 lastIndex 프로퍼티가 가리키는 문자의 위치에서 검색을 재개합니다.

```
console.log(tel.exec(text)); // -> ['010-2345-6789', '010', '2345', '6789']
```
