# react-tips
## 상태관리
### state
- state 끌어올리기 (https://reactjs-kr.firebaseapp.com/docs/lifting-state-up.html)
  1. state가 없는 component 만들기  
    -> 이러한 컴포넌트는 재활용성이 높아지는 형태이며, state뿐 아니라 기능적인 부분도 고려해야 함
  2. 데이터가 위에서 아래로 흐름  
    -> state를 동기화 하기보다는 source of truth를 기준으로 하향식 데이터 흐름을 만드는 것을 추천
- React스럽게 생각하기 (https://reactjs-kr.firebaseapp.com/docs/thinking-in-react.html)
  1. 데이터 모델 준비
  2. 컴포넌트 계층 분리 -> 단일 책임 원칙, 모델의 한 조각
  3. 정적 버전 만들기 (state가 일절 없음)
  4. UI state 찾기
     1. 부모로부터 props를 통해 전달됩니까? 그러면 확실히 state가 아닙니다.
     2. 시간이 지나도 변하지 않나요? 그러면 확실히 state가 아닙니다
     3. 컴포넌트 안의 다른 state나 props를 가지고 계산 가능한가요? 그렇다면 state가 아닙니다.
  5. state의 위치 찾기 -> container?
  6. 역방향 데이터 흐름 추가하기
### redux
- redux 사용하기 (https://redux.js.org/basics/usage-with-react)
   1. ![table1](https://raw.githubusercontent.com/windbella/react-tips/master/table1.png)  
   -> 컨테이너 패턴은 HOOK의 등장으로 필요성이 감소하긴 했지만, 여전히 프로젝트가 규모가 커지면 커지면 의미가 있는 패턴이라는 의견이 많음
   (https://blog.bitsrc.io/implementing-the-container-pattern-using-react-hooks-f490a8492d05)
   2. 컴포넌트가 redux를 알고 있는가?
   3. 목적에 충실
   4. 역할의 분리
      1. (presentational) component -> 렌더링 -> 렌더와 이벤트 발생에만 집중 (스와이프? 애니메이션 효과 등등)
      2. container (component) -> 연결, 관리 (동작, 데이터 제공)
      3. redux -> 대부분의 비지니스 로직 -> 앱의 핵심을 다룬 코어를 만들면 여러가지 형태의 앱을 적은 노력으로 프로토타입핑 해볼수 있지 않을까?
- 컴포넌트 합성 (https://ko.reactjs.org/docs/composition-vs-inheritance.html)
   1. 컴포넌트에서 다른 컴포넌트를 담기
   2. 컨테이너 중첩 문제? -> 컨테이너 in 컨테이너?
### async
- redux-thunk (https://github.com/reduxjs/redux-thunk)
  1. promise (async/await)
  2. 추가적인 미들웨어나, 자체 관리
``` javascript
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
  return {
    type: INCREMENT_COUNTER,
  };
}

function incrementAsync() {
  return (dispatch) => {
    setTimeout(() => {
      // Yay! Can invoke sync or async actions with `dispatch`
      dispatch(increment());
    }, 1000);
  };
}
```
- rematch (https://rematch.github.io/rematch/#/)
  1. redux-thunk 스타일의 편의 버전
``` javascript
export const count = {
	state: 0, // initial state
	reducers: {
		// handle state changes with pure functions
		increment(state, payload) {
			return state + payload
		},
	},
	effects: dispatch => ({
		// handle state changes with impure functions.
		// use async/await for async actions
		async incrementAsync(payload, rootState) {
			await new Promise(resolve => setTimeout(resolve, 1000))
			dispatch.count.increment(payload)
		},
	}),
}
```
- redux-saga (https://redux-saga.js.org/)
  1. yield(generator) + util
  2. 제공되는 유용한 기능들
``` javascript
import { call, put } from 'redux-saga/effects'

export function* fetchData(action) {
   try {
      const data = yield call(Api.fetchUser, action.payload.url)
      yield put({type: "FETCH_SUCCEEDED", data})
   } catch (error) {
      yield put({type: "FETCH_FAILED", error})
   }
}
```
``` javascript
import { takeEvery } from 'redux-saga/effects'

function* watchFetchData() {
  yield takeEvery('FETCH_REQUESTED', fetchData)
}
```
- 기타
  1. redux-promise-middleware, redux-pender 미들웨어 아이디어
## 팁
- 비교 알고리즘 (https://ko.reactjs.org/docs/reconciliation.html)
## 참조
- 고차 컴포넌트 (https://reactjs-kr.firebaseapp.com/docs/higher-order-components.html) -> 합성, 꽤 강력한 기능이지만 중첩이 많이 되면 효율적이지 않을 수 있어 아래 HOOK 등에서 다른 방법 들을 제시하고 있음
- HOOK (https://ko.reactjs.org/docs/hooks-intro.html)  
-> Class 스타일의 컴포넌트를 문제점들을 해결하기 위한 시도 (함수형 컴포넌트에서 사용 가능)
최근에 함수형 컴포넌트를 기본으로 필요에 따라서 훅을 추가하거나 React.Component를 고려하는 것을 밀고있는 듯함  
-> 소개 자료 (https://dev-momo.tistory.com/entry/React-Hooks)
- Portals (https://ko.reactjs.org/docs/portals.html) -> Popup 같은 케이스의 해결책 중 하나, 구조적으로 해결하는 것이 좋지만 임시방편으로 고려할 수 있음
- 리액트 상의 에러 페이지 (https://ko.reactjs.org/docs/error-boundaries.html) -> HOOK API로는 없기 때문에 상단 컴포넌트에서(클래스 스타일) Fallback 느낌으로 쓰는걸 고려해볼만함
