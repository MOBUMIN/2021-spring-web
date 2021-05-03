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

> 속성과 선택자 개념을 알게 되었고 이를 활용할 수 있도록 검색하는 방법을 배웠다. 특히 선택자의 경우 빠르게 웹페이지의 이미지를 바꿀 수 있다는 점에서 효율적으로 보였다. 아직 익숙치 않아 계속 쓰면서 익숙해져야겠다.

