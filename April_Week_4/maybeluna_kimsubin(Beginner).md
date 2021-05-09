# 생활코딩과 함께하는 WEB
  
## WEB01-210426
  
### 16. 원시웹
  
#### 우리의 목표

- web page 만들어 코딩 무엇인가 파악  
- 내가 만든 web page 인터넷을 통해 누구나 가져갈 수 있도록 만들어 인터넷이 무엇인가 파악  
  
#### web의 역사

인터넷과 웹은 다를까? -> 인터넷이라는 전체 안에 웹이라는 부분이 있는 셈이며 웹 외에도 수많은 서비스가 존재

- 1960년 인터넷 등장: 통신시스템이 중앙집중적이었는데, 분산된 통신 시스템이 필요하여 인터넷이 탄생
- 1990년 웹 등장: 유럽입자물리연구소에 인터넷이 들어오면서 [원시웹](infor.cern.ch) 탄생  
  
### 17. 인터넷을 여는 열쇠: 서버와 클라이언트

#### 인터넷 동작 기본원리

- 인터넷 동작을 위해서 정보를 주고 받는 웹 브라우저&웹서버를 사용하여 최소 2대의 컴퓨터가 필요
- 웹 브라우저(request) 설치 컴퓨터 -> client
- 웹 서버(response) 설치 컴퓨터 -> server
  
#### 웹 서버 사용

내 컴퓨터 있는 문서를 인터넷이 연결되어 있는 웹브라우저를 깔면 가져다가 볼 수 있음  

- web server 직접 깔기
- web hosting으로 대행해주는 업체에게 맡기기
  
### 18. 웹호스팅
  
무료면서 굉장히 유명한 서비스 `GitHub`  

1. new repository
2. reposityro name, public, initialize this repsository with a README 설정
3. create repsitory
4. files upload
5. settings
6. GitHub Pages에서 Source 부분에서 master branch(or main)로 변경 후 주소 탄생  

[나의 Web Hosting](https://maybeluna.github.io/web1/)  
  
html 파일만 웹 호스팅하는 것 Static web hosting  
github 외의 웹 호스팅 서비스 제공하는 곳을 찾기 위해서 *free static web hosting* 검색  
  
### 19. 웹서버 운영하기

웹 서버에는 여러 제품군이 존재하는데 우리는 무료인 `Apache`를 선택  
*how to install apaches http server os* 검색  
  
#### 윈도우에 웹서버 설치  

1. *how to easy install apache on window* serch  
2. Downloading Apache for windows 내부에 있는 여러 프로그램 중 하나 설치 ex. bitnami wamp stack
  
#### 웹서버와 http

1. 기존 주소에서 `127.0.0.1/index.html`로 바꾸기
2. Bitnami의 apache2의 `htdocs` 디렉토리 안에 index.html 존재
    - `127.0.0.1/index.html` 중에서 127.0.0.1은 웹브라우저가 설치되어 있는 컴퓨터를 가리키는 주소
    - `127.0.0.1/index.html` 중에서 index.html은 웹브라우저가 로컬 컴퓨터 안 index.html 파일
3. 주소를 통해 웹페이지를 여는 것과 파일 열기를 통해 웹페이지를 여는 것은 매우 다름
    - http://(주소): 웹 브라우저가 웹서버에 요청 후 웹 서버가 파일을 열어서 전송
    - file://(파일): 웹 브라우저가 직접 파일을 읽어서 여는 것
    - http:// (Hyper Text Transfer Protocol) -> 통신 규약을 이용해서 데이터를 가져 오는 것
  
#### 웹브라우저와 웹서버의 통신

웹브라우저가 웹서버에게 요청하기 위해서 **IP 주소**가 필요  

1. 네트워크 및 인터넷 설정 열기
2. 하드웨어 및 연결 속성 보기
3. IPv4 Address 확인
    - IPv4 Address: 자기 자신 IP
    - 127.0.0.1: 모든 컴퓨터 자기
같은 와이파이에 접속했을 때 IPv4 주소에 접속하면 아까 작성했던 html 파일이 뜬다  

> 웹의 역사와 웹서버와 웹브라우저의 개념을 알고 웹호스팅 혹은 웹서버 적용해보았다

## WEB01-210427

### 20~22. 수업을 마치며  
  
기술은 본질과 혁신으로 구분  
학습은 교양과 직업이며 본질과 교양, 혁신과 직업이 서로의 짝이라고 생각  
  
진도가 나아갈수록 중요도는 낮아지지만 난이도는 높아지는데 이것의 교점 왼편이 본질과 교양이며 오른쪽은 혁신과 직업이라고 생각한다.  
-> 우리가 교양에 있을 때는 낮은 능력에 자괴감을 느끼고 직업에 상태에 있을 때는 빠르게 성장하지 않는 자신의 모습에 자괴감을 느낀다.  
그러나 부정적인 감정보다는 긍정적인 감정이 실력향상에 도움이 되고 오래 지속할 수 있기 때문에 그렇게 되도록 노력하자!  

공부를 더하기 위해서 공부를 멈쳐보고 공부한 것을 사용해보자  
개념을 많이 알수록 더 복잡한 것을 조작하기 때문에 이는 우리로 하여금 힘들게 한다.  
다만 중요한 것을 본질으로 본질 개념을 더 많이 활용할수록 우리의 실력은 기하급수적으로 상승한다.
  
- web site를 아름답게 만들기 -> css  
- 사용자와 상호작용하기 위해 만드는 web page 만들기 -> javascript  
- 하나의 파일을 바꾸면 1억개의 web page가 동시에 바뀜 -> back end  
  
웹 프로젝트에서 가장 중요한 것은 WEB 01을 기초로 만드는 것!  

### 부록 - 한줄의 코드가 얼마나 강력한 지?  

#### 동영상 삽입 `iframe`

동영상 서비스를 제공하기 위해서 수억원의 자본이 필요하지만  
`iframe`이라는 코드로 우리의 웹페이지에 합성할 수 있게 되었다.

#### 댓글 기능 추가

방문자와의 교류수단인 댓글 기능은 HTML로만으로는 구현이 불가능  

- back end 기술이 필요
- 스팸 차단 기능 필요
- 사용자들이 이미지 업로드 필요
- 여러가지 서비스와 연동 필요 etc..  
-> 따라서 남들이 만든 댓글 서비스 이용해보기  
ex. [DISQUS](https://disqus.com/), [LiveRe](https://livere.com/)  

*단 주소가 file로 되면 안되고 웹 서버를 통해서 열어야 한다.  

#### 채팅 기능 추가  

남들이 만든 채팅 서비스 이용해보기 ex. [tawk](https://www.tawk.to/)  
단 주소가 file로 되면 안되고 웹 서버를 통해서 열어야 한다.  

#### 웹사이트 방문자 분석기

남들이 만든 분석 서비스 이용해보기 ex. [Google_analytics](https://analytics.google.com)  

1. 관리
2. 계정 및 속성 선택
3. 데이터 스트림 추가하여 웹 선택
4. 웹사이트 URL, 스트림 이름 설정
5. 웹 스트림 세부화면에서 태그하기에 대한 안내 > 새로운 온페이지 태그 추가 > 전체 사이트 태그 클릭 시 Tracking code 확인 가능
6. 코드 복사 후 head 내부에 넣기

> 처음에 안되서 계속 원인을 찾아보았다.  
댓글에서 사람들이 웹서버인 apache2폴더에 htdocs 폴더에 넣으면 된다고 하길래 시도해보았더니 가뿐하게 뜬다.  
아마 기능이 인터넷에 연결되어야 해서 그런 것 같다.  

## WEB02(CSS)-210429

### 1. 수업 소개  

CSS 이전 HTML  
컴퓨터로 정보 표현하고 이 정보를 인터넷을 통해 누구나 볼 수 있게 되어 혁명적인 변화가 나타남  
그러나 HTML 단독으로는 해결할 수 없는 문제, 불만들이 튀어나왔고 그 중 웹페이지의 가시성과 심미성에 대한 불만을 해결하기 나온 것이 **CSS**

### 2. CSS가 등장하기 전 상황

쉽지만 한계가 있는 HTML 디자인 관련 Tag: `<font>` - 폰트의 정보를 바꾸는 것  
다만 `<font>`는 디자인이지 정보가 아니므로 HTML의 접근성이 줄어들었으며, 태그가 무수히 많아져서 변경이나 수정이 힘듦  
  
### 3. CSS의 등장

CSS 문법 사용 표시 `<style></style>`  

- 새로운 언어로 더욱 어려움  
- 폭발적으로 효과가 있기 때문에 사용 (중복의 제거)  
- 효율적인 디자인 가능
- 유지 보수를 편리하게 함
- 가독성이 높아짐
- 웹페이지 해석할 때 정보만을 갖고 있는 코드만 분석 가능
  
```html
<!doctype html>
<html>
<head>
  <title>Your best Tech device</title>
  <meta charset="utf-8">
  <style>
  a {
    color:black;
  }
</style>
</head>
<body>
  <h1><a href="index.html">Tech Device</a></h1>
  <ul>
    <li><a href="1.html">Laptop</a></li>
    <li><a href="2.html">Tablet pc</a></li>
    <li><a href="3.html">Smart phone</a></li>
  </ul>  
<!--
<ol>
<li><a href="1.html"><font color="red">Laptop</font></a></li>
<li><a href="2.html"><font color="red">Tablet pc</font></a></li>
<li><a href="3.html"><font color="red">Smart phone</font></a></li>
</ol>
-->
  <h2>What is your best Tech device?</h2>
</body>
</html>
```

*참고사향 HTML 문법 무시 `<!---->`  

### 4. CSS의 기본 문법  

#### 방법

`<style>a{}</style>`  

- `a`는 누구에게 효과를 주는 지 표시하는 것으로 Selector라고 불림  
- `{}` 안에 있는 효과는 선택자에게 지정될 효과로 Declaration라고 불림  
- 스타일 속성 이용  `style=""` 이것은 직접 효과를 주기 때문에 선택자(Selector)라는 것이 필요 없음  
- 스크립트 구분을 위해 `;` 사용  

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

### 5. 혁명적 변화

```html
<style>
a{
    color:red;
}
</style>
```

a -> Selector  
{} -> Declaration  
color -> Property  
red -> Value  
  
> 이제 기본 문법을 알게되어, CSS 상에서 우리는 무엇을 모르는 지 알 수 있게 되었다. 이를 활용한다면 공부하기가 수월해질 것이다.  
  
느낀점😊  
-> 빠르게 css로 넘어갔는데 css로 앞으로 웹페이지를 꾸밀 생각을 하니 너무 기대된다! :)
