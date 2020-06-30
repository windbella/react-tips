# react-tips (작성 중입니다...)
## 상태관리
### state
- state 끌어올리기 (https://reactjs-kr.firebaseapp.com/docs/lifting-state-up.html)
  1. state가 없는 component 만들기  
  2. 데이터가 위에서 아래로 흐름
- React스럽게 생각하기 (https://reactjs-kr.firebaseapp.com/docs/thinking-in-react.html)
  1. 정적 버전 만들기 (state가 일절 없음)
  2. UI state 찾기
     1. 부모로부터 props를 통해 전달됩니까? 그러면 확실히 state가 아닙니다.
     2. 시간이 지나도 변하지 않나요? 그러면 확실히 state가 아닙니다
     3. 컴포넌트 안의 다른 state나 props를 가지고 계산 가능한가요? 그렇다면 state가 아닙니다.
  3. state의 위치 찾기 -> container?
  4. 역방향 데이터 흐름 추가하기
### redux
- redux 사용하기 (https://redux.js.org/basics/usage-with-react)
   1. ![table1](https://raw.githubusercontent.com/windbella/react-tips/master/table1.png)
   2. 컴포넌트가 redux를 알고 있는가?
   3. 목적에 충실
   4. 역할의 분리
      1. (presentational) component -> 렌더링 -> 렌더와 이벤트 발생에만 집중 (스와이프? 애니메이션 효과 등등)
      2. container (component) -> 연결, 관리 (동작, 데이터 제공)
      3. redux -> 대부분의 비지니스 로직 -> 앱의 핵심을 다룬 코어(리듀서 + 액션)를 만들면 여러가지 형태의 앱을 적은 노력으로 프로토타입핑 해볼수 있지 않을까?
- 컴포넌트 합성 (https://ko.reactjs.org/docs/composition-vs-inheritance.html)
   1. 컴포넌트에서 다른 컴포넌트를 담기
   2. 컨테이너 중첩 문제? -> 컨테이너 in 컨테이너?
### async
- redux-thunk (https://github.com/reduxjs/redux-thunk)
  1. async/await
  2. 추가적인 미들웨어나, 자체 관리
- rematch (https://rematch.github.io/rematch/#/)
  1. redux-thunk 스타일의 편의 버전
- redux-saga (https://redux-saga.js.org/)
  1. yeild(generator) + util
  2. 제공되는 유용한 기능들
- 기타
  1. redux-promise-middleware, redux-pender 미들웨어 아이디어
## 팁
- 비교 알고리즘 (https://ko.reactjs.org/docs/reconciliation.html)
## 참조
- 고차 컴포넌트 (https://reactjs-kr.firebaseapp.com/docs/higher-order-components.html) -> 합성
- Portals (https://ko.reactjs.org/docs/portals.html) -> Popup 같은 케이스의 해결책 중 하나
- 리액트 상의 에러 페이지 (https://ko.reactjs.org/docs/error-boundaries.html)
