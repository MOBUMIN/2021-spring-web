# MVVM 구조와 CONTEXT API

date : 2021-04-18

author : MOBUMIN(김수빈)

이번 주에는 프로젝트 이해도를 높이기 위해 MVVM 구조에 대해 공부했다. 그리고 MVVM 구조를 만들기 위해 React의 Context를 공부했다.

## MVVM 구조

디자인 패턴은 다양하다. MVC, MVVM, MVP...

나는 프론트엔드단에서 쓰이는 MVVM 디자인 패턴에 대해 공부했다.

**먼저, 디자인 패턴은 왜 쓸까?**

나보다 대단한 개발자들이 오랜기간 이용했다.

패턴대로 코드가 진행이 되기 때문에 유지보수 비용이 감소한다.

언어에 제약이 없다.

이런 저런 이유로 사용할 수 있을 것이다. 실제로, 프로젝트에서 사용해본 결과 코드가 패턴대로 진행이 되어서 코드 작성이 용이했다.

**그럼 MVVM 패턴이란?**

MVVM은 Model, View, ViewModel의 첫 글자이다.

1. 뷰 (View)
2. 뷰 모델 (View Model)
3. 모델 (Model)

뷰 <- 뷰 모델 <-> 모델

뷰는 뷰 모델을 알고, 뷰 모델은 모델을 안다.

반대로 모델은 뷰 모델을 모르고, 뷰 모델은 뷰를 모른다.

이 방식으로 코드 의존도를 낮추고 UI로부터 비즈니스 로직과 프레젠테이션 로직을 분리할 수 있다.

### 뷰

뷰는 UI에 관한 것을 다룬다. 이용자가 스크린을 통해서 보는 것들이라고 할 수 있다.

애니메이션과 같은 UI 로직은 포함하지만, 비즈니스 로직(데이터의 처리가 이루어지는 부분)은 포함하지 않는다.

### 뷰 모델

뷰가 사용할 메서드, 필드를 구현하는 곳이며, 뷰에게 상태 변화를 알려준다.

내가 이해하여 사용한 바로는 모델의 정보를 가공해서 넘겨주는 느낌이다.

뷰의 UI가 제공할 기능을 정의한다. (어떻게 보여줄 지는 뷰가 결정한다.)

뷰 모델과 모델은 일 대 다의 관계다.

내가 작성한 간단한 예제는 아래와 같다. (이 코드는 내가 작성하기는 했지만, MVVM을 적용한 프로젝트를 하며 동기의 도움을 많이 받았다. 감사합니다.)

``` js
import React, { useEffect, useContext, createContext } from 'react';
import axios from 'axios';
import { usePostState, usePostDispatch } from './PostModel';

const GetPostContext = createContext(()=>{});

function PostViewModel({children}) {
 const postDispatch = usePostDispatch();

 useEffect(() => {
  axios.get('/api/post')
  .then(res => {
   postDispatch(res.data.post);
  })
 }, [])

 const getPost = () => {
  axios.get('/api/post')
  .then(res => {
   postDispatch(res.data.post);
  })
 }

 return (
  <GetPostContext.Provider value={getPost}>
   {children}
  </GetPostContext.Provider>
 )
}

export default PostViewModel;
export function useGetPost() {
 const context = useContext(GetPostContext);
 return context;
}
```

### 모델

**데이터**에 관련한 행위와 데이터를 다루는 곳이다.

Redux에서는 reducer와 비슷한 느낌이라고 하는데 redux를 안 써봐서 이 부분은 잘 모르겠다.

내가 작성한 간단한 예시는 아래와 같다. (이 코드는 내가 작성하기는 했지만, MVVM을 적용한 프로젝트를 하며 동기의 도움을 많이 받았다. 감사합니다.)

```js
import React, { useState, useContext } from 'react'

const PostContext = React.createContext();
const PostContextDispatch = React.createContext(() => {});

function PostModel({children}) {
 const [post, setPost] = useState();
 return (
  <PostContext.Provider value={post}>
   <PostContextDispatch.Provider value={setPost}>
    {children}
   </PostContextDispatch.Provider>
  </PostContext.Provider>
 )
}

export default PostModel

export function usePostState(){
 const context = useContext(PostContext);
 return context;
}

export function usePostDispatch(){
 const context = useContext(PostContextDispatch);
 return context;
}
```

이 MVVM 구조의 사용 예시는 다음과 같다.

1. 뷰에서 저장하기 버튼을 누른다.
2. 뷰 모델에서 요청된 명령을 실행한다. (참고. 뷰는 뷰 모델을 안다.)
3. 뷰 모델이 모델의 데이터를 수정한다. (참고. 뷰 모델은 모델을 안다.)

여기서 뷰 모델에서 갱신한 데이터에 맞게 뷰도 갱신이 되어야하는데, 뷰 모델은 뷰를 모르기 때문에 뷰를 갱신시킬 수 없다.

이 때 뷰 모델에서 바인더를 불러서 바인더가 뷰를 갱신시키는 방식을 이용하면 뷰를 갱신시킬 수 있다.

바인딩에는 두 가지 방식이 있다.

* Template Scan ( Vue, Angular에서 이용한다고 한다. )
* Template State ( React에서 이용한다고 한다. )

Template State는 바인더가 뷰를 안고 있는 방식이다.

Binder와 HTMLELEMENT와의 연결을 끊기 위해 Scanner도 사용한다고 하는데, 아직 이 부분은 이해가 부족해 무슨 말인지 모르겠다.

내가 구현한 코드는 뷰 모델이 뷰를 감싸고 있어서 바인더 없이 구현했다. 아래 예제 코드를 참고하자.

```js
import React from 'react'
import PostModel from './PostModel';
import Routers from '../routes';
import PostViewModel from './PostViewModel';

function Provider() {
 return (
  <PostModel>
   <PostViewModel>
    <Routers />
   </PostViewModel>
  </PostModel>
 )
}

export default Provider;
```

이번 프로젝트가 아니었다면 영원히 state 관리에 대해 손 놓고 있을 뻔 했다.

state 관리는 참 어렵다. 다음 번에는 redux나 mobx중 하나에 대해 공부하고 싶다.

공부한 사이트들

> <https://junilhwang.github.io/TIL/CodeSpitz/Object-Oriented-Javascript/02-MVVM/>
> <https://velog.io/@k7120792/Model-View-ViewModel-Pattern>
> <https://black-jin0427.tistory.com/133>
> <https://xho95.github.io/swiftui/architecture/mvvm/logic/2020/08/05/SwiftUI-MVVM.html>
> <https://hini7.tistory.com/145>
