이번 주는 이전에 학습한 Redux Middleware의 또다른 종류인 `redux-saga`에 대해 학습하고
`redux-thunk`와 `redux-saga`의 사용법에 대해 구체적으로 알아보는 시간을 가졌습니다.

# 1. redux-saga

`redux-saga`는 액션을 모니터링하다 특정 액션이 발생하면 해당 액션과 매칭되는 동작을 수행하는 미들웨어입니다.
`redux-thunk`는 비동기 작업을 처리한 후 dispatch로 리듀서에 결과를 전달하는 과정으로 동작하고 `redux-saga`는
액션이 호출될 때 액션 리스너 역할을 하는 saga 함수에서 비동기 작업을 처리한 뒤 리듀서에 해당 결과를 
전달해주는 과정으로 이루어집니다. `redux-saga`에서는 액션을 함수 형태로 호출하지 않고 **객체 형태**로만 
모든 작업을 수행할 수 있어 훨씬 깔끔하고 독립적인 기능을 가진 코드를 작성할 수 있습니다.

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
`takeLeading` 같은 헬퍼함수를 사용합니다. `takeEvery` 함수는 특정 액션 타입에 대해 dispatch되는
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

# 2. 비동기 처리

## 2-1. Promise(ES6) & async/await(ES8)

`redux-thunk`와 `redux-saga`의 비동기 처리 방법을 이해하기 위해 JavaScript의 Promise와
async/await에 대해 간단히 정리한 후 두 미들웨어의 비동기 처리 수행 방법을 살펴보고자 합니다.

### Promise

Promise는 JavaScript의 비동기 처리를 위해 ES6 부터 도입된 기능입니다.
Prommise를 사용하면 비동기 함수에서 동기 함수처럼 값을 반환하여 사용할 수 있습니다.
Promise는 비동기를 처리하면서 다음의 상태를 가집니다.

- 대기(pending) : 초기 상태
- 이행(fulfilled) : 연산 성공
- 거부(rejected) : 연산 실패

비동기가 성공적(fulfilled)으로 이루어지면 `resolve()` 함수를 통해 결과값을 return하고 오류가
발생(rejected)하면 `reject()` 함수를 호출합니다. 이후 이행(fulfilled)된 결과에 대해선 `.then()` 함수를
통해 실행된 비동기 작업 이후의 작업을 실행할 수 있고 거부(rejected)된 결과에 대해선 `.then()`이나
`.catch()` 함수를 통해 예외 처리를 수행할 수 있습니다.

```JavaScript

function delay2seconds(){
    return new Promise(resolve => {
        setTimeout(() => {
            resolve("Success!!");
        }, 2000);
    });
}

console.log("Calling");
delay2seconds().then((message) => {
    console.log(message); // 결과 : 2초 뒤 Success!!
}).catch((e) => {
    console.err(e); // 에러 발생 시 catch
});

```

### async/await

async/await은 Promise와 callback 이외에 비동기를 처리하기 위해 ES8부터 도입된 기능입니다.
async/await을 사용하면 비동기 처리 종료 후 다음 작업을 수행하기 위해 Promise의 `.then()` 함수와 같이
response에 대한 함수를 추가로 만들 필요가 없어 Promise Chain을 간결하게 표현할 수 있고 에러 처리를 위해
`try/catch` 문을 사용하기 때문에 비동기 기능에 구애받지 않고 에러 처리를 할 수 있습니다.

async/await을 사용하기 위해선 비동기 처리가 이루어지는 함수에 `async` 키워드를 선언한 후 비동기 처리 부분에
`await`를 선언해줍니다.

```JavaScript

function delay2seconds(){
    return new Promise(resolve => {
        setTimeout(() => {
            resolve("Success!!");
        }, 2000);
    });
}
async function asyncFunction(){
    try{
        console.log("Calling");
        const result = await delay2seconds();
        console.log(result); // 결과 : 2초 뒤 Success!!
    }catch(e){
        console.err(e); // 에러 발생 시 catch
    }
}
asyncFunction();

```

## 2-1. redux-thunk와 redux-saga의 비동기 처리 비교

### redux-thunk

redux-thunk에서 비동기 처리를 수행하고 이후의 동작을 수행하기 위해서는 액션 함수를 사용하기 위한 dispatch,
비동기 동작을 처리하기 위해 await/async를 사용합니다.

```JavaScript

// 액션 함수
export const fetchData = (url) => async dispatch => {
  dispatch({ type: "FETCH_REQUESTED" }); // 1) FETCH_REQUESTED 리듀서 동작 수행 (state)

  try {
    const data = await Api.fetchUser(url); // 2) Api의 fetchUser 함수를 url 인자를 통해 호출 => 유저 정보 fetch
    dispatch({ type: "FETCH_SUCCEEDED", payload:data }); // 3) FETCH_SUCCEEDED 리듀서 동작 수행 (state)
  } catch (e) {
    dispatch({ type: "FETCH_FAILED", payload:error }); // 4) 에러 발생 시 FETCH_FAILED 리듀서 동작 수행 (state)
  }
}

// 사용 시
const dispatch = useDispatch();

useEffect(()=>{
  dispatch(fetchData(url)); // 액션 함수 호출
},[dispatch])

```

> 데이터를 fetch하기 위해서 fetchData **액션 함수** 호출이 필요하다.

### redux-saga

redux-saga에서 비동기 처리를 수행하고 이후의 동작을 수행하기 위해서는 액션 리스닝을 위한 `takeEvery()` 헬퍼 함수,
액션을 dispatch하는 `put()` 함수, 비동기 함수 실행을 위해 `call()` 함수와 제너레이터의 yield를 사용합니다.

```JavaScript

import { takeEvery, call, put } from 'redux-saga/effects'

// 액션 객체
export const fetchData = url => ({ type:"FETCH_REQUESTED", payload:url });

function* fetchDataSaga(action) {
    const url = action.payload.url;

    try {
        const data = yield call(Api.fetchUser, url) // 2) Api의 fetchUser 함수를 url 인자를 통해 호출 => 유저 정보 fetch
        yield put({ type: "FETCH_SUCCEEDED", payload:data }) // 3) FETCH_SUCCEEDED 리듀서 동작 수행 (state)
    } catch (error) {
        yield put({ type: "FETCH_FAILED", payload:error }) // 4) 에러 발생 시 FETCH_FAILED 리듀서 동작 수행 (state)
    }
}

export default function* watchFetchDataSaga() {
  yield takeEvery("FETCH_REQUESTED", fetchData); // 1) FETCH_REQUESTED 리듀서 동작 수행 이후 fetchData 함수 실행 (state)
}

// 사용 시 
const dispatch = useDispatch();

useEffect(()=>{
  dispatch(fetchData(url)); // 액션 객체 호출
},[dispatch])

```

> 데이터를 fetch하기 위해서 "FETCH_REQUESTED" **액션 객체**만 호출하면 된다. => 훨씬 직관적이고 가독성 높은 코드

- put : 액션 dispatch
- call : Promise 함수의 완료를 대기
- takeEvery : 특정 액션 타입에 대해 dispatch되는 모든 액션을 처리(헬퍼 함수)

&lt;참고자료&gt;
- [redux-thunk와 redux-saga에 대하여](https://react.vlpt.us/redux-middleware/10-redux-saga.html)
- [Redux-saga에 대하여](https://medium.com/@han7096/redux-saga%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-5e39b72380af)
- [ES6의 제너레이터를 사용한 비동기 프로그래밍](https://meetup.toast.com/posts/73)
- [Promise MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [async function MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)
- [JS의 Async/Await가 Promise를 사라지게 만들 수 있는 이유](https://medium.com/@constell99/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-async-await-%EA%B0%80-promises%EB%A5%BC-%EC%82%AC%EB%9D%BC%EC%A7%80%EA%B2%8C-%EB%A7%8C%EB%93%A4-%EC%88%98-%EC%9E%88%EB%8A%94-6%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0-c5fe0add656c)
- [redux-thunk로 프로미스 다루기](https://react.vlpt.us/redux-middleware/05-redux-thunk-with-promise.html)
- [redux-saga로 프로미스 다루기](https://react.vlpt.us/redux-middleware/11-redux-saga-with-promise.html)
