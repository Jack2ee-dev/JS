## Array.prototype의 메서드 목록

1. 수정 메서드: 원본 배열을 바로 수정합니다.

- copyWithin(_target_, _begin_, _end_): begin~end-1 사이의 요소를 target의 위치의 복사합니다.

```
var a = ['a','b','c','d','e'];
a.copyWithin(0,3,4); // -> ['d','b','c','d','e']
a.copyWithin(1,3); // -> ['d', 'd', 'e', 'd', 'e']
```

- fill(_value_, _begin_, _end_): begin~end-1 사이의 요소를 value로 대체합니다.

```
var a = [1,2,3,4];
a.fill(0,2,4); // -> [1,2,0,0]
a.fill(5,1); // -> [1,5,5,5]
```

- pop(): 배열의 마지막 요소를 잘라내고 잘라낸 값을 반환합니다.

- push(_data_): 배열 끝에 data 값을 추가하고 그 배열의 길이를 반환합니다.

```
var a = ['A', 'B', 'C'];
a.push('D'); // -> ['A', 'B', 'C', 'D'], 반환값: 4
```

- reverse(): 배열의 요소를 역순으로 정렬합니다.

- shift(): 배열의 첫 번째 요소를 잘라내고 잘라낸 값을 반환합니다.

```
var a = ['A', 'B', 'C'];
a.shift(); // -> ['B', 'C'], 반환값: 'A'
```

- unshift(_data [, data2, ...]_): 인수로 지정한 데이터를 배열의 시작 부분에 추가하고 그 배열의 길이를 반환합니다.

```
var a = ['A', 'B', 'C'];
a.unshift('D'); // -> ['D', 'A', 'B', 'C'], 반환값: 4
```

- sort(_[callback]_); 배열의 요소를 callback(f(a, b))이 구현한 방법에 따라 정렬합니다.

* f(a, b) < 0: a를 b보다 작은 인덱스로 정렬한다. -> 오름차순
* f(a, b) > 0: b를 a보다 작은 인덱스로 정렬한다. -> 내림차순
* f(a, b) == 0: a와 b의 순서를 바꾸지 않는다.

여기서 비교 함수를 지정하지 않으면 배열의 요소를 문자열로 변환한 다음에 사전순으로 정렬합니다. 값이 undefined인 요소가 있다면 그 요소는 배열의 마지막에 위치시킵니다.

- splice(_index, howmany [, data ...]_): index부터 howmany개의 배열 요소를 data로 대체하고 삭제된 값을 반환합니다.
  **첫 번째 인수(index)**: 배열을 수정하기 시작할 위치를 가리키는 인텍스. 이 값이 배열 길이보다 크면 배열 마지막을 시작 위치로 간주합니다. 이 값이 음수면 이 값에 배열 길이를 더한 값을 시작 위치로 간주합니다.
  **두 번째 인수(howmany)**: 배열에서 삭제할 요소의 개수, 0을 넘기면 어떠한 요소도 삭제하지 않습니다. 이 때는 인수로 새로운 요소를 적어도 한 개는 넘겨야 합니다. 아무런 값을 넘기지 않으며 index 이후의 모든 배열 요소를 삭제합니다.
  **세 번째 이후의 인수**: 배열에 삽입할 요소의 값을 쉼표로 구분해서 넘깁니다. 값이 없으면 단순히 배열에서 요소를 삭제합니다.

```
var a = ['A', 'B', 'C', 'D'];
a.spliec(1, 2, 'X', 'Y', 'Z'); // ['A', 'X', 'Y', 'Z', 'D'], 반환값: ['B', 'C']
```

2. 접근자 메서드: 배열을 다른 형태로 가공한 새로운 배열을 반환하며 원본 배열을 수정하지 않습니다.

- concat(_array_): 지정한 배열을 대상 배열에 연결합니다.

- indexOf(_data, index_): data와 같은 첫 번째 배열 요소의 키를 검색합니다(index는 검색을 시작하는 위치).

- join(_string_): 배열의 요소를 string으로 연결한 문자열을 반환합니다.

```
var a = ['h', 'a', 'p', 'p', 'y'];
a.join(''); // -> happy
a.join('/'); // -> h/a/p/p/y
```

- lastIndexOf(_data, index_): data와 같은 마지막 배열 요소의 키를 검색합니다(index는 검색을 시작하는 위치).

- slice(_begin [, end]_): begin~end-1 사이의 요소를 제거한 새로운 배열을 반환합니다.

```
var a = ['h', 'a', 'p', 'p', 'y'];
a.slice(0, 3); // -> ['h', 'a']
a.slice(1); // -> ['a', 'p', 'p', 'y']
```

- toLocalString(): 배열의 요소를 해당 지역에 맞는 언어로 번역한(지역화한) 문자열로 변환한 뒤 쉼표로 연결해서 반환합니다.

- toString(): 배열의 요소를 문자열로 변환한 뒤 쉼표로 연결해서 반환합니다.

```
var date = new Date();
['A', 'B', 'C', date].toLocalString(); // -> 'A,B,C,2020. 6. 8. 오후 7:48:53'
['A', 'B', 'C', date].toString(); // -> 'A,B,C,Mon Jun 08 2020 19:48:53 GMT+0900 (대한민국 표준시)'
```

3. 반복 메서드: 원본 배열의 모든 요소를 순회하며 특정한 작업을 수행합니다. 반복 메서드 대부분은 첫 번째 인수로 함수의 참조를 받는 고차 합수입니다. 반복 메서드의 사용법은 전형적인 함수형 프로그래밍의 형태를 띱니다.

- entries(): 배열 요소의 인덱스와 값이 요소로 들어 있는 배열을 값으로 반환하는 이터레이터를 반환합니다.

```
var a = ['a', 'b', 'c'];
var iterator = a.entries();
iterator.next().value; // -> Array [0, 'a']
iterator.next().value; // -> Array [1, 'b']
```

- every(_callback_): 배열의 모든 요소가 callback 조건에 부합하는지 판정합니다.

```
var isBelowThreshold = (currentValue) => currentValue < 40;
var a = [1, 30, 39, 29, 10, 13];
a.every(isBelowThreshold); // -> true
```

- some(_callback_): 배열 요소 단 하나라도 callback 조건에 부합하는지 판정합니다.

```
var isAboveThreshold = (currentValue) => currentValue > 40;
var a = [1, 30, 49, 29, 10, 13];
a.every(isAboveThreshold); // -> true
```

- filter(_callback_): callback 조건에 부합하는 배열 요소만 담은 새로운 배열을 생성합니다.

- find(_callback_): callback 조건에 부합하는 첫 번째 배열 요소를 반환합니다.

- findIndex(_callback_): callback 조건에 부합하는 첫 번째 배열의 인덱스를 반환합니다.

- forEach(_callback_): 배열의 요소를 callback을 사용하여 차례대로 처리합니다(**수정 메서드**).

- map(_callback_): 배열의 요소를 callback으로 처리한 결과물을 배열로 반환합니다(**접근자 메서드**).

- keys(): 배열 요소의 인덱스를 값으로 가지는 이터레이터를 반환합니다.

- reduce(_callback [, initial]_): 이웃한 배열 요소를 배열의 왼쪽부터 차례대로 callback으로 처리하여 하나의 값으로 만들어 반환합니다(initial은 초깃값). 이를 합성 곱 처리라고 하는데 합성 곱 처리란 배열 요소 하나를 함수로 처리한 후에 그 반환값을 다음 번 요소를 처리할 때 함수의 입력 값으로 사용하는 처리 방법을 말합니다.
  callback 함수는 다음과 같은 인수를 받습니다.
- prev: 이전 요소를 처리한 함수의 반환값 또는 initial 또는 첫 번째 요소의 값
- value: 현재 처리하는 배열 요소의 값
- index: 현재 처리하는 배열 요소의 인덱스
- array: 메서드를 적용 중인 배열의 참조

```
var a = [1,2,3,4,5];
a.reduce(function(prev, value) { return prev + value }, 0) // -> 15
a.reduce(function(prev, value) { return prev + value}) // -> 15
```

위의 코드에서 initial을 지정하지 않은 경우는 prev는 첫 번째 요소의 값, value는 배열의 두 번째 요소의 값, index는 1입니다. initial을 지정한 경우는 prev는 initial의 값, value는 배열의 첫 번째 요소, index는 0입니다.

```
var a = ['Tom', 'Huck', 'Becky'];
a.reduce(function(p, v) { p[v]=v.length; return p}, {}) // -> {'Tom':3, 'Huck':4, 'Becky':5}
```

- 적용 예) 모든 순열 목록 구하기

```
function permutation(a) {
  return a.reduce(function(list, element) {
    var newList = [];
    list.forEach(function(seq) {
      for (var i = seq.length; i >= 0; i--) {
        var newseq = [].concat(seq);
        newseq.splice(i, 0, element);
        newList.push(newseq);
      }
    });
    return newList;
  }, [[]])
}
var a = [1,2,3];
permutation(a) // -> [1,2,3], [1,3,2], [3,1,2], [2,1,3], [2,3,1], [3,2,1]
```
