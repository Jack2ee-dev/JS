## 확장 가능 속성

객체의 확장 가능(extensible) 속성은 객체에 새로운 프로퍼티를 추가할 수 있는지를 결정합니다. 확장 가능 속성 값이 true로 설정된 객체에는 새로운 프로퍼티를 추가할 수 있지만 false로 설정된 객체에는 추가할 수 없습니다.

## 확장 방지: Object.preventExtensions 메서드

Object.preventExtensions 메서드는 인수로 받은 객체를 확장할 수 없게 만듭니다. 이 메서드로 확장할 수 없게 만든 객체는 두 번 다시 프로퍼티를 추가할 수 없게 됩니다.
Object.isExtensible 메서드를 사용하면 인수로 지정한 객체가 확장 가능한지 확인할 수 있습니다.

## 밀봉: Object.seal 메서드

Object.seal 메서드는 인수로 받은 객체를 밀봉합니다. 밀봉이란 객체에 프로퍼티를 추가하는 것을 금지하고 기존의 모든 프로퍼티를 재정의할 수 없게 만드는 것을 말합니다. 다시 말해 객체를 밀봉하면 프로퍼티의 추가, 삭제, 수정을 할 수 없고 값의 읽기와 쓰기만 가능해집니다.

## 동결: Object.freeze 메서드

Object.freeze 메서드는 인수로 받은 객체를 동결합니다.
동결이란 객체에 프로퍼티를 추가하는 것을 금지하고 기존의 모든 프로퍼티를 재정의할 수 없게 만들며 **데이터 프로퍼티**를 쓸 수 없게 만드는 것입니다. 다시 말해 객체를 통결하면 객체의 프로퍼티가 읽기만 가능한 상태가 됩니다. 단, 객체에 접근자 프로퍼티가 정의되어 있다면 게터 함수와 세터 함수 모두를 호출할 수 있습니다.
