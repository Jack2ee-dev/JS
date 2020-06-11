## 문자열 검색하기: search 메서드

search 메서드는 인수로 받은 정규 표현식 객체와 일치한 최초 문자열의 첫 번째 문자 위치를 반환합니다. 일치하는 문자열을 찾지 못했을 때는 -1을 반환합니다.

```
var s = '1 little,2 little indian';
console.log(s.search(/little/)); // -> 2
console.log(s.search(/\d/)); // -> 0
console.log(s.search(/\bindianl/)); // -> 18
console.log(s.search(/3\s/)); // -> -1
```

인수에 \d 등의 이스케이프된 문자를 지정하려면 \ 자체를 이스케이프해서 \\d라고 표기해야 합니다. 또한 search 메서드는 전역 검색을 지원하지 않습니다.

## 문자열 치환하기: replace 메서드

1. replace 메서드의 기본적인 사용법
   replace 메서드는 첫 번째 인수로 받은 정규 표현식과 일치하는 문자열을 검색하고, 두 번째 인수로 받은 문자열로 치환한 새로운 문자열을 반환합니다. replace 메서드는 원본 문자열을 고치치 않습니다. 정규 표현식에 g 플래그를 설정하면 일치한 문자열을 모두 치환합니다.

```
var s = '1 little,2 little indian';
console.log(s.replace(/little/, 'big')); // -> '1 big,2 little indian'
console.log(s.replace(/little/g, 'big')); // -> '1 big,2 big indian'
```

2. 치환 패턴: $n, $&

- $n: $n에는 정규 표현식 안에 **소괄호**를 사용하여 그룹화한 n번째 부분 정규 표현식과 일치한 문자열이 들어가며, n에는 0~9999 사이의 값을 넣을 수 있습니다.

```
// 전화번호의 첫 번째 '0'을 '+82-'로 치환하는 예
var person = 'Tom, tom@example.com, 010-1234-5678';
var result = person.replace(/0(\d{1,4}-\d{1,4}-\d{4})/g, '+82-');
console.log(result); // -> 'Tom, tom@example.com, +82-10-1234-5678'

// 날짜 서식을 치환하는 예
var date = '오늘은 2016년9월10일 입니다.';
var result = date.replace(/(\d+)년(\d+)월(\d+)일/, '$1/$2/$3');
console.log(result); // -> '오늘은 2016/9/10/ 입니다.'

// 성과 이름을 맞바꾸는 예
var name = 'Tom Sawyer';
var result = name.replace(/(\w+)\s(\w+)/, '$2 $1');
console.log(result); // -> 'Sawyer Tom'
```

- $&: $&에는 일치한 부분 문자열이 들어옵니다.

```
var address = '121-842 서울특별시 마포구 월드컵로10길 56';
var result = address.replace(/\d{3}-\d{3}/, '우)$&');
console.log(result); // -> '우)121-842 서울특별시 마포구 월드컵로10길 56'
```

3. 문자열 치환을 처리하는 함수
   두 번째 인수에 함수를 넘길 수도 있습니다. 그 함수는 다음과 같은 인수를 받습니다.

- match: 일치한 부분 문자열(\$&와 같은 값)
- group1: 첫 번째 부분 정규 표현식과 일치한 부분 문자열(\$1과 같은 값)
- group2: 두 번째 부분 정규 표현식과 일치한 부분 문자열(\$2과 같은 값)
  :
  :
  offset: 일치한 부분 문자열의 첫 번째 문자 위치
  inputStr: 원본 문자열 전체

## 문자열 추출하기: match 메서드

match 메서드는 첫 번째 인수로 받은 정규 표현식과 일치하는 문자열을 순서대로 저장해서 배열로 반환합니다. 이는 RegExp.prototype의 exec 메서드를 사용해도 같은 결과가 나옵니다.

```
'1 little,2 little indian'.match(/\d+/g); // -> ['1', '2']
```

정규 표현식에 g 플래그를 설정하지 않으면 가장 처음 일치한 문자열만 반환합니다.

## 문자열 나누기: split 메서드

1. split 메서드의 기본 사용법
   split 메서드는 첫 번째 인수로 문자열을 분할한 다음에 배열에 담아서 반환합니다. 첫 번째 인수로는 문자열 또는 정규 표현식 객체를 넘깁니다. 첫 번째 인수를 생략하면 원본 문자열 전체를 배열에 담아서 반환합니다.

```
console.log('172.20.51.65').split('.); // -> ['172', '20', '51', '65']
```

다음 코드는 첫 번째 인수로 정규 표현식을 넘기는 예입니다. ; 문자로 나눈 이름 목록에서 앞뒤의 공백을 제거한 후에 배열로 만들어서 반환합니다.

```
var names = ' Tom Sawyer ; Huckleberry Finn ;Becky Thatcher ';
var withoutSpaceOfFB = names.replace(/(^\s*|\s*$)/g, "") // -> 앞뒤 공백 제거
var list = withoutSpaceOfFB.split(/\s*;\s/); // -> 중간 ' ; '를 기준으로 나누기
console.log(list); // -> ['Tom Sawyer', 'Huckleberry Finn', 'Becky Thatcher']
```

2. 반환할 문자열의 개수 제한하기
   split 메서드의 두 번째 인수는 선택 사항입니다. 두 번째 인수로는 반환할 문자열의 개수를 제한할 수 있습니다.

```
console.log('1, 2, 3, 4, 5'.split(/\s*,\s*/, 3)); // -> ['1', '2', '3']
```
