# 2021-spring-web-beginner-april-week-2

## 🙋 Who am I

- [r4k0nb4k0n](https://github.com/r4k0nb4k0n)

## 🧑‍💻  What I did

- [r4k0nb4k0n/terrarium](https://github.com/r4k0nb4k0n/terrarium) deployed to [r4k0nbk40n.github.io/terrarium](https://r4k0nb4k0n.github.io/terrarium)
  - A project to learn about HTML, CSS, and DOM manipulation using JavaScript.
  - DOM manipulation, `e.preventDefault()`, Closure.
  - Implement dragging elements to wherever I want using event handlers such as
`onpointerdown`, `onpointerup`, `onpointerdown`.
  - Implement bringing specific element to the front using event handler `ondblclick`.
- [r4k0nb4k0n/typing-game](https://github.com/r4k0nb4k0n/typing-game)
deployed to [r4k0nb4k0n.github.io/typing-game](https://r4k0nb4k0n.github.io/typing-game)
  - A project to sharpen the most underrated skills of the developer, typing.
  - Event-driven programming.
  - Implement playing 3 videos on background using `<video>` element and DOM manipulation.

## 🧗 What I struggled with

- [r4k0nb4k0n/typing-game](https://github.com/r4k0nb4k0n/typing-game)
deployed to [r4k0nb4k0n.github.io/typing-game](https://r4k0nb4k0n.github.io/typing-game)
  - Flickering
    - Cause
      - I changed the `src` attribute of `<video>` element manually by
manipulating DOM. This is expensive.
    - How I solved
      - I created 3 `<video>` elements corresponding to each video.
      - I hooked the event handler `onended` to 3 `<video>` elements.
      - The event handler `onended` works like this:
        - sets `style.visibility` of current `<video>` element to `hidden`
        - sets `style.visibility` of next `<video>` element to `visible`
        - execute `play()` method of next `<video>` element DOM.

## 🧐  What I am going to do

- Implement JavaScript code of typing game.
