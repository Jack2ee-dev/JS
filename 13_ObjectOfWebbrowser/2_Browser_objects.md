## Location 객체

Location 객체는 창에 표시되는 문서의 URL을 관리합니다. Location 객체는 window.location 또는 location으로 참조할 수 있습니다. document.location 또한 Location 객체를 참조합니다.

## History 객체

History 객체는 창의 웹 페이지 열람 이력을 관리합니다. History 객체는 window.history 또는 history로 참조할 수 있습니다.

1. History 객체의 프로퍼티

- length: 현재 세션의 이력 개수(읽기 전용)
- scrollRestoration: 웹 페이지를 이동한 후에 동작하는 웹 브라우저의 자동 스크롤 기능을 켜거나 끄는 값. 'auto' 또는 'manual'이 들어갈 수 있다.
- state: pushState와 replaceState 메서드로 설정한 state(읽기 전용)

2. History 객체의 메서드

- back(): 창의 웹 페이지 열람 이력을 하나 되돌린다.
- forward(): 창의 웹 페이지 열람 이력을 하나 진행한다.
- go(number): 창의 웹 페이지 열람 이력을 number만큼 진행한다. number 값이 음수면 그만큼 되돌린다.
- pushState(state, title, url): 창에 웹 페이지 열람 이력을 추가한다. 페이지는 이동하지 않는다.
- replaceState(state, title, url): 현재 창의 열람 이력을 수정한다.

## Navigator 객체

Navigator 객체는 스크립트가 실행 중인 웹 브라우저 등의 애플리케이션 정보를 관리합니다. Navigator 객체는 window.navigator 또는 navigator로 참조할 수 있습니다.

## Screen 객체

Screen 객체는 화면(모니터) 크기와 색상 등의 정보를 관리합니다. Screen 객체는 window.screen 또는 screen으로 참조할 수 있습니다.
