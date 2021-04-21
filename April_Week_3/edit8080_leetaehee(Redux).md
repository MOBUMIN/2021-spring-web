이번 주는 이전에 학습한 Redux Middleware의 또다른 종류인 `redux-saga`에 대해 학습하고
`redux-thunk`와 `redux-saga`의 사용법에 대해 구체적으로 알아보는 시간을 가졌습니다.

# 1. redux-saga

`redux-saga`는 액션을 모니터링하다 특정 액션이 발생하면 해당 액션과 매칭되는 동작을 수행하는 미들웨어입니다.
`redux-thunk`는 액션에 비동기 작업을 바로 연결한 후 dispatch하여 사용하는 과정과 달리 `redux-saga`는
액션을 단순히 리듀서와 연결해놓고 해당 액션이 호출될 때 액션 리스터 역할을 하는 saga 함수에서 관련 비동기 작업을
처리한 뒤 리듀서에 해당 결과를 전달해주는 과정으로 이루어집니다. 따라서 `redux-saga`에서는 액션을 함수
형태로 표현하지 않고 객체 형태로만 모든 작업을 수행할 수 있어 훨씬 깔끔하고 독립적인 기능을 가진 코드를
작성할 수 있습니다.

## 1-1. 제너레이터 문법(ES6)

`redux-saga`는 액션이 발생할 때 해당 액션과 관련된 작업을 수행해주기 위해 제너레이터 문법을 사용합니다.
따라서 `redux-saga`의 원활한 이해를 위해 제너레이터 문법을 간단히 정리하였습니다.

제너레이터 문법은 비동기 작업을 처리하기 위해 ES6부터 도입된 기능입니다.
제너레이터 함수를 사용하면 여러 개의 값을 필요에 따라 하나씩 반환(yield)할 수 있습니다.
제너레이터 함수는 `function*` 키워드를 사용하여 만들 수 있고 `.next()` 메소드로 순차적으로 동작을
iterate할 수 있습니다. 또한 `.next()`를 통해 제너레이터 함수 내부의 변수에 값을 전달할 수도 있습니다.

```JavaScript

function* getUserInfo(id) {
    yield getName(id);
    yield getEmail(id);
    yield getPhoneNumber(id);
    return id;
}
const iterator = getUserInfo(1);
iterator.next(); // 결과 {value: "Taehee Lee", done: false}
iterator.next(); // 결과 {value: "leetaehee0205@gmail.com", done: false}
iterator.next(); // 결과 {value: "010-xxxx-xxxx", done: false}
iterator.next(); // 결과 {value: 1, done: false}

```

## 1-2. redux-saga 사용

saga에서는 액션에 대한 반응을 위해 다양한 함수를 사용합니다. saga함수를 액션 함수와 연결하기 위해서 `put` 함수를
사용합니다. 또한 액션을 모니터링하고 각 동작에 반응하는 리스너 동작을 위해 `takeEvery`, `takeLatest`,
`takeLeading` 같은 유틸함수를 사용합니다. `takeEvery` 함수는 특정 액션 타입에 대해 dispatch되는
모든 액션을 처리하고 `takeLatest` 함수는 dispatch될 때 가장 마지막 액션만을, `takeLeading` 함수는 가장
처음의 액션을 처리하는 함수입니다.

### saga 함수 선언

```JavaScript

import { delay, put, takeEvery, takeLatest } from 'redux-saga/effects';

const INCREASE = 'INCREASE';

export const increase = () => ({ type: INCREASE }); // 3) 리듀서 동작 호출

function* increaseSaga() {
  yield delay(1000);
  yield put(increase()); // 2) 1초 delay 후 increase() 액션 함수 호출
}
export function* counterSaga() {
  yield takeEvery(INCREASE, increaseSaga); // 1) 외부에서 INCREASE 동작을 호출하면 increaseSaga 호출 (리스너 saga)
}

const initialState = 0;

export default function counter(state = initialState, action) {
  switch (action.type) {
    case INCREASE:  // 4) 리듀서 동작을 통해 state 변경
      return state + 1;
    default:
      return state;
  }
}

```

### 여러 개의 리듀서와 리스너 saga 결합

다른 곳에서 선언한 여러 개의 리듀서를 결합하기 위해서는 redux의 `combineReducers`를 사용하고
모든 리스너 saga 함수를 실행하기 위해 `all` 함수를 사용합니다.

```JavaScript

import { combineReducers } from 'redux';
import { counterSaga } from './counter';
import { todoSaga } from './todo';
import { all } from 'redux-saga/effects';

const rootReducer = combineReducers({ counter, posts });

export function* rootSaga(){
    yield all[counterSaga(), todoSaga()];
}
export default rootReducer;

```

이후 `index.js`에서 `createSagaMiddleware` 함수를 통해 saga 미들웨어를 구성하고 위에서 합쳐진
리듀서와 리스너 saga 함수를 스토어에 전달합니다. 스토어가 구성된 이후에 `run` 함수를 통해 리스너 saga 함수를 실행시켜줍니다.

```JavaScript

import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import rootReducer, { rootSaga } from './modules';
import createSagaMiddleware from 'redux-saga';

const sagaMiddleware = createSagaMiddleware();
const store = createStore(rootReducer, applyMiddleware(sagaMiddleware))

sagaMiddleware.run(rootSaga);

ReactDOM.render(
    <Provider store={store}>
      <App />
    </Provider>,
  document.getElementById('root')
);

```

&lt;참고자료&gt;
- [redux-thunk와 redux-saga에 대하여](https://react.vlpt.us/redux-middleware/10-redux-saga.html)
- [Redux-saga에 대하여](https://medium.com/@han7096/redux-saga%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-5e39b72380af)
- [ES6의 제너레이터를 사용한 비동기 프로그래밍](https://meetup.toast.com/posts/73)
