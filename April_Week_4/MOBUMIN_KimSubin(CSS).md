# CSS Flexible Box

date : 2021-05-03

author : MOBUMIN(김수빈)

이번 주에는 자주 검색하는 내용인 CSS Flex에 대해 간단하게 정리를 했습니다.

## 내가 자주 쓰고 잘 아는 부분

**Container**

flex를 줘야한다.

`display: flex / inline-flex`

container 자체가 inline이 되느냐, 아니냐의 차이.

그 다음으로 내부 item의 방향을 정하자.

`flex-direction: row / column / row-reverse / column-reverse`

방향을 정했더니 item이 삐져나갔다(no-wrap은 item이 삐져나가는 듯). 줄 바꿈 해보자.

`flex-wrap: no-wrap / wrap / wrap-reverse`

위 두개를 같이 하자!

`flex-flow: row wrap`

그 다음으로 내부 item의 정렬 방식을 정하자.

- 주 축 방향

`justify-content: flex-start / flex-end / center / space-between(사이의 간격) / space-around(둘레의 간격) / space-evenly(사이, 둘레 간격)(IE, Edge X)`

- 수직 축 방향

`align-items: stretch / flex-start / flex-end / center / baseline`

- 수직 축 방향(wrap 설정 상태에서 여러 줄 나왔을 때)

`align-content: flex-start / flex-end / center / stretch / space-between / space-around / space-evenly`

## 내가 매번 검색하는 부분

**item**

컨테이너에 작성하는 게 아니라 item 속성에 작성해야한다.

한 item의 수직 축 정렬 상태를 바꾸고 싶다.

`align-self: auto / stretch / flex-start / flex-end / baseline / center`

아이템의 증가 비율 설정하자.

`flex-grow: 0 (flex-basis보다 커지지 않음) / 1 (이 비율에 맞게 나눠 가질것)`

아이템의 감소 비율을 설정하자.

`flex-shrink: 1 / 0 (flex-basis보다 작아지지않음)`

flex-item의 주축 방향 기본 크기를 정하자.

`flex-basis: auto / 0 / % / em / cm / px ...`

flex-basis보다 이미 큰 경우는 건들지 않는다.

위 세 개를 다 같이 적용하자.

`flex: grow / shrink / basis`

item 순서와 z-index는 `order`, `z-index`로 설정하면 된다.

다음 번에는 css grid도 공부해봐야겠다.

참고 사이트는 제 티스토리에 올라가 있습니당 :)

> <https://kiju23.tistory.com/18>
