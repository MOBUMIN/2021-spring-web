# 2021-spring-web-beginner-april-week-3

## 🙋 Who am I

- [r4k0nb4k0n](https://github.com/r4k0nb4k0n)

## 🧑‍💻  What I did

- [r4k0nb4k0n/typing-game](https://github.com/r4k0nb4k0n/typing-game)
deployed to [r4k0nb4k0n.github.io/typing-game](https://r4k0nb4k0n.github.io/typing-game)
  - 가짜 caret과 깜박이는 애니메이션 추가
  - 기본 게임 로직 구현

## 🧗 What I struggled with

- [r4k0nb4k0n/typing-game](https://github.com/r4k0nb4k0n/typing-game)
deployed to [r4k0nb4k0n.github.io/typing-game](https://r4k0nb4k0n.github.io/typing-game)
  - 가짜 caret 들통나지 않기
    - 원인
      - 현재 실제 caret의 색깔만 바꿀 수 있고 모양을 바꿀 수 있는 spec이 없다.
      - 이에 가짜 caret을 만들고 실제 caret의 색깔을 transparent로 하여 가짜를 꾸민다.
      - 근데 `Home`, `End`, `Arrow Left`, `Arrow Right` 등 caret의 위치를 바꿀 수 있는
key를 입력하면, 실제 caret만 옮겨지고 가짜 caret은 계속 끝에 있어서 가짜 caret이 들통난다.
    - 해결방법
      - caret의 위치를 바꿀 수 있는 key가 입력되면 `e.preventDefault();`를 실행하여 기존 동작을
무시한다.

## 🧐  What I am going to do

- [r4k0nb4k0n.github.io/typing-game](https://r4k0nb4k0n.github.io/typing-game) 관련하여
제보된 버그들을 해결한다.
- [5-browser-extension](https://github.com/r4k0nb4k0n/Web-Dev-Fundamentals/tree/main/5-browser-extension)
브라우저 확장 프로그램을 직접 실습해보면서 학습합니다.
