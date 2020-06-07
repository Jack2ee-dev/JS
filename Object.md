객체의 생성
--------
0. 개념
* 객체: 이름과 값을 한 쌍으로 묶은 집합
* 프로퍼티: 이름(key)과 값(value)이 한 쌍을 이룬 것
* 키(key): 프로퍼티의 이름
* 값(value): 모든 타입의 데이터(원시 값, 객체) 저장 가능
* 메서드: 함수의 참조를 값으로 가진 프로퍼티

1. 객체 생성법
* 객체 리터럴로 생성
```
var card = { suit: '하트', rank: 'A'};
```

* 생성자로 생성
```
function Card(suit,rank) {
  this.suit = suit;
  this.rank = rank;
}
var card = new Card('하트', 'A');
```

* Object.create로 생성
```
var card = Object.create(Object.prototype, {
    suit: {
      value: '하트',
      writable: true,
      enumerable: true,
      configurable: true,
    },
    rank: {
      value: 'A',
      writable: true,
      enumerable: true,
      configurable: true,
    }
  }
)
```

프로토타입
-------
1. 생성자 안에서 메서드를 정의하는 방식의 문제점: 생성자 안에서 this 뒤에 메서드를 정의하면 그 생성자로 생성한 모든 인스턴스에 똑같은 메서드가 추가됩니다. 따라서 메서드를 포함한 생성자로 인스턴스를 여러 개 생성하면 같은 작업을 하는 메서드를 인스턴스 개수만큼 생성하게 되며 결과적으로 메모리를 추가 소비하게 됩니다.
2. 프로토타입 
