# 오류

date : 2021-05-16

author : MOBUMIN(김수빈)

이번 주 역시 프로젝트들을 진행했다. 전공, DSC 외 프로젝트에서 map을 쓰는 데 이상한 오류가 떴는데, 아주 간단한 거였지만 잔디 채울겸 PR을 보내야겠다😁

## 첫 번째 오류

react-router-dom을 사용하다가 뜬 에러다.

`must use destructuring props assignment` 이런 오류가 떴다.

`props.match.params.Id` 이렇게 이용하고 싶었는데, props 오류가 떠서 그냥 props 대신에 `{match}`로 바꿔주고 `match.params.Id` 이렇게 접근하니 해결되었다.

왜 이런 오류가 떴을까? 아는 분이 있다면.... 알려주세요... 흑흑

## 두 번째 오류

map 함수를 쓰다가 오류가 났다.

`expected an assignment or function call and instead saw an expression` 이런 오류가 떴다.

컴포넌트가 하나면 `map((value)=>(<Component />))` 이렇게.

혹은 `map((value)=>{return <Component />})` 이렇게 해결 할 수도 있다.

나는 한 줄이어서 `map((value)=> setComponent("Hi"))` 이렇게 해결했다.
