# 생활코딩과 함께하는 WEB
  
## WEB02-210503

### 6. CSS 속성을 스스로 알아내기

#### Property를 스스로 알아내자

검색을 통해서 찾고싶은 내용 스스로 알아내기

- *CSS text size property* 검색
- *CSS text center property* 검색

```html
<!doctype html>
<html>
<head>
  <title>Your best Tech device</title>
  <meta charset="utf-8">
  <style>
  a {
    color:black;
    text-decoration: none
  }
  h1 {
    font-size: 60px;
    text-align: center;
    text-decoration: overline;
    text-decoration: underline;
  }
</style>
</head>
<body>
  <h1><a href="index.html">Tech Device</a></h1>
  <ul>
    <li><a href="1.html">Laptop</a></li>
    <li><a href="2.html"
    style="color:red;text-decoration:underline">Tablet pc</a></li>
    <li><a href="3.html">Smart phone</a></li>
  </ul>
  <h2>What is your best Tech device?</h2>
</body>
</html>
```

### 6. CSS 선택자를 스스로 알아내기

- class 선택자: 특정 태그에 대해서 html의 속성을 주는 것인데 구분은 띄어쓰기로 함 (선택자 앞에 `.`이 붙인 경우 class를 가리킴)  
- id 선택자: 고유의 태그에 대해서 html의 속성을 주는 것으로 중복이 되지 않음 (선택자 앞에 `#`이 붙은 경우 id를 가리킴)

선택자 우선순위  
id(구체적) > Class > Tag(포괄적)  
태그에 우열이 존재하지 않는다면 최근의 것을 가장 우선함  

```html
<!doctype html>
<html>
<head>
  <title>Your best Tech device</title>
  <meta charset="utf-8">
  <style>
  a {
    color:black;
    text-decoration: none
  }
  #active {
    color:red;
  }
  .saw {
    color:gray;
  }
  .nonstoa {
      text-decoration: underline;
  }
  h1 {
    font-size: 60px;
    text-align: center;
  }
</style>
</head>
<body>
  <h1><a href="index.html">Tech Device</a></h1>
  <ul>
    <li><a href="1.html" class="saw nonstop">Laptop</a></li>
    <li><a href="2.html" class="saw" id="active">Tablet pc</a></li>
    <li><a href="3.html">Smart phone</a></li>
  </ul>
  <h2>What is your best Tech device?</h2>
</body>
</html>
```

> 속성과 선택자 개념을 알게 되었고 이를 활용할 수 있도록 검색하는 방법을 배웠다. 특히 선택자의 경우 빠르게 웹페이지의 이미지를 바꿀 수 있다는 점에서 효율적으로 보였습니다. 아직 익숙치 않아 계속 쓰면서 익숙해져야겠습니다.  

## WEB02(CSS)-210504

### 8. 박스모델

#### 각 태그의 크기

- block level element: 화면 전체를 쓰는 태그  
- inline element: 자신 크기만큼 쓰는 태그

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
      /*
      block level element
      */
      h1{
        border-width:5px;
        border-color:red;
        border-style: solid;
        /*
        block element -> inline element
        */
        display: inline;
        /*
        tag 안보이게 설정
        */
        display: none;
      }
      /*
      inline element
      */
      a{
        border-width:5px;
        border-color:red;
        border-style: solid;
        /*
        inline element -> block element
        */
        display: block;
      }
    </style>
  </head>
  <body>
    <h1>COOKIE RUN</h1>
    새롭게 경험하는 쿠키들의 세계! 
    마녀의 오븐을 탈출한 쿠키들이 드디어 새로운 <a href="https://www.cookierun-kingdom.com/ko/">쿠키들의 왕국</a>을 건설합니다!
    개성 있는 다양한 쿠키들과 함께 나만의 쿠키 왕국을 건설하고, 즐거운 순간을 함께 경험해보세요.
    미지의 쿠키 세계를 탐험하고 적들과 전투를 벌이며 왕국의 영향력을 넓혀보세요. 
    왕국에 축제가 열리면 친구들이 찾아와요! 친구들과 함께 공동의 미션을 수행해보세요. 
    사랑스러운 쿠키들의 다양한 매력이 가득한 새로운 쿠키 세계가 여러분을 찾아갑니다.
  </body>
</html>
```

#### 크기 확인 방법에서 중복 제거

- 선택자에서 `,`를 이용해서 중복 제거
- 속성에서 공백을 이용해서 중복 제거하는데 이 때 순서는 중요하지 않다.

```html
<style>
  h1, a{
    border: 5px red solid;
  }
```

#### 실제 박스모델

- 컨텐츠와 테두리 여백 `padding`
- 테두리와 테두리 간객 `margin`
- block level element 특징 바꾸기 `width`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
    h1{
      border:5px solid red;
      padding: 20px;
      margin: 0;
      width:100px
    }
    </style>
  </head>
  <body>
    <h1>COOKIE RUN</h1>
  </body>
</html>
```

#### 박스 모델 확인

- css box model 이미지 검색하면 쉽게 정보를 찾을 수 있음  
- 검사(inspect) 이용
  - styles 항목: 특정 태그가 어떠한 css style 영향 받는 지 알 수 있음
  - 웹 개발에 도움 주는 도구  

## WEB02(CSS)-210505

### 9. 그리드

태그를 적용하는 것을 목적으로 사용하는 태그  

- `<div></div>`: 의미가 존재하지 않는 디자인 용도 태그이며 block level element  
- `<span></span>`: 의미가 존재하지 않는 디자인 용도 태그이며 inline element

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
    /* 
    부모태그로 아이디가 grid 인것을 선택
    */
      #grid
      {
        border: 5px solid pink;
        display:grid;
        /* 
        첫 번째 칼럼은 특정 크기 사용하는 px 단위 선택
        두 번째 칼럼은 나머지 화면 전체를 사용하는 fr 단위 선택 (화면크기에 따라 조절)
        (예시 2fr 1fr 일 때 전체를 3이라고 하면 첫 번째 칼럼은 2/3, 두 번재 칼럼은 1/3을 사용한다는 것을 뜻함
        */
        grid-template-columns: 150px 1fr;
      }
      div{
        border: 5px solid gray;
      }
    </style>
  </head>
  <body>
    <div id = "grid">
      <div>Navigation</div>
      <div>Article</div>
    </div>
  </body>
</html>
```

그리드를 사용하면 자동적으로 한 칼럼의 크기가 커지면 연결된 칼럼 역시 크기가 커지는 장점이 있다!  
[현재 웹브라우저가 채택하고 있는 기술 통계](https://caiuse.com)를 통해 기술을 활용하는 지 통계를 통해 알 수 있고 그것을 기반으로 공부할 수 있다

css에서 `ol`, `ul` 이런 태그를 사용하기 보다 명확하게 표시하기 위해서 `#grid ul` 이렇게 grid id에 있는 ul 태그라고 명시하는 것이 좋다

```html
<!doctype html>
<html>
<head>
<title>Your best Tech device</title>
<meta charset="utf-8">
  <style>
  h1{
    font-size: 45px;
    font-family:fantasy;
    border-bottom: 1px solid black;
    margin: 0;
    padding:20px;
  }
  ul{
    border-right: 1px solid black;
    width: 100px;
    margin: 0;
    padding: 20px
  }
  #grid{
    display: grid;
    grid-template-columns: 150px 1fr;
  }
  #grid ul{
    padding-left: 30px
  }
  #grid #article{
    padding-left: 50px;
  }
  </style>
</head>
<body>
  <h1><a href="index.html">Tech Device</a></h1>
  <div id ="grid">
    <ul>
      <li><a href="1.html">Laptop</a></li>
      <li><a href="2.html">Tablet pc</a></li>
      <li><a href="3.html">Smart phone</a></li>
    </ul>
      <div id = "article">
        <h2>iPad pro</h2>
        <p>
          The ultimate iPad experience. Now with breakthrough M1 performance,
          a breathtaking XDR display, and blazing‑fast 5G wireless.
          With M1, iPad Pro is the fastest device of its kind.
          It’s designed to take full advantage of next‑level performance and custom technologies like the advanced image signal processor and unified memory architecture of M1.
          And with the incredible power efficiency of M1, iPad Pro is still thin and light with all‑day battery life,
          making it as portable as it is powerful. The 8‑core CPU of M1 delivers up to 50 percent faster performance.
          And M1 has an 8‑core GPU in a class of its own, providing up to 40 percent faster graphics performance to iPad Pro.
          So you can build intricate AR models, play games with console‑quality graphics at high frame rates, and more.
          The Liquid Retina XDR display delivers true-to-life detail with a 1,000,000:1 contrast ratio,
          great for viewing and editing HDR photos and videos or enjoying your favorite movies and TV shows.
          It also features a breathtaking 1000 nits of full‑screen brightness and 1600 nits of peak brightness.
          And advanced display technologies like P3 wide color, True Tone, and ProMotion.
          Over 10,000 mini‑LEDs are grouped into more than 2500 local dimming zones.
          Depending on the content, the brightness in each zone can be precisely adjusted to achieve an astonishing 1,000,000:1 contrast ratio.
          Even the most detailed HDR content with the finest specular highlights — like galaxies and action movie explosions — are more true to life than ever.
          The Liquid Retina display on the 11‑inch iPad Pro is not only gorgeous and portable, it also features incredibly advanced technologies.
          Like ProMotion, True Tone, P3 wide color, and ultralow reflectivity, which make everything feel responsive and look stunning.
          iPad has always been uniquely portable with superfast Wi‑Fi and cellular options.
          Now with 5G capabilities, you can connect to the fastest wireless networks when you need to download files, stream movies, collaborate with colleagues, and upload content on the go.
          And iPad Pro has the most 5G bands of any device of its kind — so it can get 5G in more places.
        </p>
      </div>
  </div>
</body>
</html>
```

> 박스모델과 그리드를 이용해서 웹페이지 레이아웃을 적용하는 방법을 학습했습니다. 처음에 그리드를 접하기 전에 칼럼 나누는 방법을 잠깐 찾아봤습니다. css에 적용하는 모델과 html에 적용할 수 있는 모델 두 가지를 보았는데 (관련사이트: [웹 칼럼 레이아웃](https://www.w3schools.com/howto/howto_css_two_columns.asp)) 둘 다 중복이 많은 것을 보았습니다. 그런데 그리드를 이용하면 쉽고 간편하게 레이아웃을 적용할 수 있어서 좋았습니다. 또한 #grid ul{padding-left: 30px} 이 부분에서 위에 ul 태그에 있는 것을 모두 집어 넣었는데 padding-left가 작동하지 않았습니다. 한 태그에 유사한 태그가 들어가도 위에 padding 먼저하고 다음 padding-left를 적용할 줄 알았는데 그게 아니었고 한 태그에는 유사한 태그가 하나만 들어가야 한다는 것을 깨달았습니다.

## WEB02(CSS)-210506

### 8. 반응형 디자인

웹은 수많은 형태에 적용할 수 있도록 만들어야 함  
위의 이유로 **반응형 디자인**이 탄생하였는데 반응형 디자인이란 화면의 크기에 따라 웹페이지의 요소들이 반응해서 작동된다 -> 미디어 쿼리로 구현  
화면 크기는 구글 개발자 툴(검사)을 이용하면 알 수 있음  

#### 미디어쿼리

- `@media(){}`를 이용해서 만듦
- `min-width: x`: x 크기보다 크면 적용
- `max-width: x`: x 크기보다 작으면 적용  

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
      div{
        border:10px solid green;
        font-size:60px;
      }
      @media(min-width:800px) {
        div{
          display:none;
        }
      }
    </style>
  </head>
  <body>
    <div>
      Responsive
    </div>
  </body>
</html>
```

#### 실제 미디어쿼리 적용

```html
<!doctype html>
<html>
<head>
<title>Your best Tech device</title>
<meta charset="utf-8">
  <style>
  h1{
    font-size: 45px;
    text-align: center;
    font-family:fantasy;
    border-bottom: 1px solid black;
    margin: 0;
    padding:20px;
  }
  ul{
    border-right: 1px solid black;
    width: 100px;
    margin: 0;
    padding: 20px
  }
  #grid{
    display: grid;
    grid-template-columns: 150px 1fr;
  }
  #grid ul{
    padding-left: 30px
  }
  #grid #article{
    padding-left: 50px;
  }
  @media(max-width:800px){
    #grid{
      display: block;
    }
    ul{
      border-right:none;
    }
    h1 {
      border-bottom:none;
    }
  }
  </style>
</head>
<body>
  <h1><a href="index.html">Tech Device</a></h1>
  <div id ="grid">
    <ul>
      <li><a href="1.html">Laptop</a></li>
      <li><a href="2.html">Tablet pc</a></li>
      <li><a href="3.html">Smart phone</a></li>
    </ul>
      <div id = "article">
        <h2>iPad pro</h2>
        <p>
          The ultimate iPad experience. Now with breakthrough M1 performance,
          a breathtaking XDR display, and blazing‑fast 5G wireless.
          With M1, iPad Pro is the fastest device of its kind.
          It’s designed to take full advantage of next‑level performance and custom technologies like the advanced image signal processor and unified memory architecture of M1.
          And with the incredible power efficiency of M1, iPad Pro is still thin and light with all‑day battery life,
          making it as portable as it is powerful. The 8‑core CPU of M1 delivers up to 50 percent faster performance.
          And M1 has an 8‑core GPU in a class of its own, providing up to 40 percent faster graphics performance to iPad Pro.
          So you can build intricate AR models, play games with console‑quality graphics at high frame rates, and more.
          The Liquid Retina XDR display delivers true-to-life detail with a 1,000,000:1 contrast ratio,
          great for viewing and editing HDR photos and videos or enjoying your favorite movies and TV shows.
          It also features a breathtaking 1000 nits of full‑screen brightness and 1600 nits of peak brightness.
          And advanced display technologies like P3 wide color, True Tone, and ProMotion.
          Over 10,000 mini‑LEDs are grouped into more than 2500 local dimming zones.
          Depending on the content, the brightness in each zone can be precisely adjusted to achieve an astonishing 1,000,000:1 contrast ratio.
          Even the most detailed HDR content with the finest specular highlights — like galaxies and action movie explosions — are more true to life than ever.
          The Liquid Retina display on the 11‑inch iPad Pro is not only gorgeous and portable, it also features incredibly advanced technologies.
          Like ProMotion, True Tone, P3 wide color, and ultralow reflectivity, which make everything feel responsive and look stunning.
          iPad has always been uniquely portable with superfast Wi‑Fi and cellular options.
          Now with 5G capabilities, you can connect to the fastest wireless networks when you need to download files, stream movies, collaborate with colleagues, and upload content on the go.
          And iPad Pro has the most 5G bands of any device of its kind — so it can get 5G in more places.
        </p>
      </div>
  </div>
</body>
</html>
```

> 실제 적용했을 때 작동이 안되는 부분이 좀 있었지만 웹 페이지를 껐다가 키니까 됐다. 잠깐의 오류였던 것으로 보인다. 그리고 추천해주신 AWS S3 정적호스팅을 해보려고 추천 사이트에 들어갔는데 처음 CRP라는 용어부터 낯설어서 보류해두기로 했다. 일단 유튜브에 있는 AWS 웹 호스팅 영상을 시청한 후에 한번 더 봐야겠다. 그 전까지는 깃으로 웹 호스팅을 해야지..

## WEB02(CSS)-210507

### css 코드의 재사용

style을 바꿀 때 웹페이지가 몇 개 없다면 쉽게 바꿀 수 있지만 1억개가 넘는 웹페이지가 있다면 우리는 절망에 빠진다.  
이러한 문제가 일어나는 이유는 css가 모든 웹페이지에서 중복해서 나타나기 때문이다.  

**중복의 제거**는 프로그램 및 코딩이 얼마나 효율적인지 깨달을 수 있으며 중복의 제거의 중요성을 확인할 수 있다!

1. style 태그에 있는 css 코드만 보가하여 별도의 파일로 만들기
2. `<link></link>` 태그를 통해서 어떤 css와 연결되어 있는지 웹브라우저에게 알려주기

겉보기에는 똑같지만 내부적으로는 구조가 완전히 달라지고 효율적으로 바뀐 것!  

- 중복의 제거 효과
- 사용성이 증가
- css 바꿀 때 효과적
- 더 적은 트래픽 사용 -> cashing

[적용 Web page](https://maybeluna.github.io/web2/index.html)

> 예전에 폰에서 용량을 차지하는 캐시를 자주 삭제했는데, 무슨 파일인지도 모르고 일단 용량을 줄여준다고 해서 삭제했다. 그리고 왜 필요도 없는 cash가 쌓이는 지 불만이 많았는데 이번 공부를 통해 cash의 쓰임새를 알게 되어서 좋았다!

느낀점😊  
-> css를 다 마무리했는데 과제 때문에 많이 하지 못해 아쉽다. 비록 웹페이지 하나밖에 만들지 않았지만 계속 html과 css를 활용해서 다른 페이지를 만들어보고 싶다!  
