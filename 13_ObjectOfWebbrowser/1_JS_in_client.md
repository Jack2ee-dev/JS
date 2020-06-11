## 웹 브라우저에서 자바스크립트가 하는 일

1. 웹 페이지의 Document 객체 제어(HTML 요소와 CSS 스타일 작업) ~ DOM
2. 웹 페이지의 Window 객체 제어 및 브라우저 제어 ex) Location, Navigator
3. 웹 페이지에서 발생하는 이벤트 처리
4. HTTP를 이용한 통신 제어 ~ XMLHttpRequest

## 웹 브라우저에서의 자바스크립트 실행 순서

1. 웹 브라우저로 웹 페이지를 열면 가장 먼저 Window 객체가 생성됩니다. Window 객체는 웹 페이지의 전역 객체로 웹 페이지와 탭마다 생성됩니다.

2. Document 객체가 Window 객체의 프로퍼티로 생성되며 웹 페이지를 해석해서 DOM 트리의 구축을 시도합니다. Document 객체는 readyState 프로퍼티를 가지고 있으며, 이 프로퍼티에는 HTML 문서의 해석 상태를 뜻하는 문자열이 저장됩니다. readyState 프로퍼티의 초깃값은 'loading'이라는 문자열입니다.

3. HTML 문서는 HTML 구문을 작성 순서에 따라 분석하며 Document 객체 요소와 텍스트 노드를 추가해 나갑니다.

4. HTML 문서 안에 script 요소가 있으면 script 요소 안의 코드 또는 외부 파일에 저장된 코드의 구문을 분석합니다. 그 결과 오류가 발행하지 않으면 그 시점에 코드를 실행합니다. 이 때 script 요소는 동기적으로 실행됩니다. 즉, script 요소의 구문을 분석해서 실행할 때는 HTML 문서의 구문 분석이 일시적으로 막히고, 자바스크립트 코드의 실행을 완료한 후에는 일시적으로 막혀 있었던 HTML 문서의 구문 분석을 재개합니다.

5. HTML 문서의 모든 내용을 읽은 후에 DOM 트리 구축을 완료하면 document.readyState 프로퍼티 값이 'interactive'로 바뀝니다.

6. 웹 브라우저는 Document 객체에 DOM 트리 구축 완료를 알리기 위해 DOMContentLoaded 이벤트를 발생시킵니다.

7. img 등의 요소가 이미지 파일 등의 외부 리소스를 읽어 들여야 한다면 이 시점에 읽어 들입니다.

8. 모든 리소스를 읽어 들인 후에는 document.readyState 프로퍼티 값이 'complete'로 바뀝니다. 마지막으로 웹 브라우저는 Window 객체를 상대로 load 이벤트를 발생시킵니다.

9. 이 시점부터 다양한 이벤트(사용자 정의 이벤트, 네트워크 이벤트)를 수신하며, 이벤트가 발생하면 이벤트 처리기가 비동기로 호출됩니다.

## async와 defer 속성

1. async
   script 요소에 async 속성을 설정하면 script 요소의 코드가 비동기적으로 실행됩니다. 즉, HTML 문서의 구문 분석 처리를 막지 않으며 script 요소의 코드를 최대한 빨리 실행합니다. 여러 개의 script 요소에 async 속성을 설정하면 다 읽어 들인 코드부터 비동기적으로 실행하므로 실행 순서가 보장되지 않습니다. 읽어 들이는 순서에 의존하는 script 요소에는 async 속성을 설정하지 말아야 합니다.

2. defer
   defer 속성을 설정한 script 요소는 DOM 트리 구축이 끝난 후에 실행됩니다. DOM 구축이 끝난 시점에 실행되기 때문에 자바스크립트 코드로 요소 객체에 이벤트 처리기를 등록하는 등의 초기화 작업을 할 수 있습니다. 따라서 defer 속성은 DOMContentLoaded 이벤트의 대안으로 활용할 수 있습니다.

- async 또는 defer 속성이 설정된 script 요소에 document.write 메서드가 있으면 async와 defer 속성이 무시되어 동기적으로 실행됩니다.

## Window 객체
