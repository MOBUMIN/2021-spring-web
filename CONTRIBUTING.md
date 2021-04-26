# CONTRIBUTING

아래와 같은 과정을 통해 해당 Git Repo 활동에 참여하실 수 있습니다.

## How to contribute

1. [DSC-University-of-Seoul/2021-spring-web](https://github.com/DSC-University-of-Seoul/2021-spring-web)
repository를 여러분 개인 계정으로 fork 합니다.

![image-1-fork](https://user-images.githubusercontent.com/5409746/116043673-ef0a1a80-a6aa-11eb-9bc3-cbe59e3398b6.png)

1. Fork가 된 repository를 여러분의 local에 clone으로 받습니다.

```shell
# In your local...
$ git clone https://github.com/YourAccount/2021-spring-web
$ cd 2021-spring-web
```

1. `.md` 파일을 작성합니다. 아래와 같은 규칙을 지켜야 합니다.

![image-2-terminal](https://user-images.githubusercontent.com/5409746/116047241-d69bff00-a6ae-11eb-9fd8-cfb3061e311c.png)

- 파일 이름 형식
  - `Git닉네임_이름(카테고리).md`
  - ex) `edit8080_leetaehee(React).md`
- markdownlint
  - [nodejs 런타임](https://nodejs.org/ko/)과 [npm](https://www.npmjs.com/get-npm) 또는
[Yarn](https://classic.yarnpkg.com/en/docs/install#mac-stable)이 설치되어 있어야 합니다.
  - `npm install -g markdownlint-cli` 또는 `yarn global add markdownlint-cli`로
`markdownlint-cli` 모듈을 설치합니다.
  - 터미널에서 `markdownlint [검사할 마크다운 파일 이름]`을 실행하여 검사할 수 있습니다.

1. stage, commit, push 합니다.

```shell
# In your local...
$ git add .
$ git commit -m "Add [your file] to April-Week-3"
$ git push
```

1. 작성중...
