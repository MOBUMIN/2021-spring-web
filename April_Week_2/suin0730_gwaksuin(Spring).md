- [1. Hello Backend!](#1-hello-backend-)
  * [1.1. 왜 프레임워크를 쓰나요?](#11--------------)
  * [1.2. 프레임워크는 어떻게 구성되나요?](#12------------------)
    + [1.2.1. 디자인 패턴](#121-------)
    + [1.2.2. 라이브러리](#122------)
  * [1.3. Spring과 IntelliJ](#13-spring--intellij)
    + [1.3.1. Spring의 장점](#131-spring----)
    + [1.3.2. IntelliJ Idea](#132-intellij-idea)
  * [1.4. Gradle](#14-gradle)
    + [1.4.1. Gradle Build Lifecycle](#141-gradle-build-lifecycle)
    + [1.4.2. Gradle 사용법](#142-gradle----)
      - [__Plugin__](#--plugin--)
      - [__Repository__](#--repository--)
      - [__Dependency__](#--dependency--)
      - [__Test__](#--test--)
- [2. Testcode](#2-testcode)
  * [2.1. 테스트코드를 왜 작성해야 하나요?](#21-------------------)
  * [2.2. Spring boot 테스트코드는 어떻게 작성해야 하나요?](#22-spring-boot---------------------)
    + [2.2.1. Given-when-then](#221-given-when-then)
    + [2.2.2. Given-when-then 연습](#222-given-when-then---)
  * [2.3. Lombok으로 자바를 더 편하게 써봅시다!](#23-lombok------------------)
- [3. Java Persistence Api](#3-java-persistence-api)
  * [3.1. JPA가 무엇인가요?](#31-jpa--------)
  * [3.2. JAP를 적용해봅시다!](#32-jap---------)
  * [3.3. 게시글 등록 API를 만들어봅시다!](#33--------api---------)
  * [3.4. 테스트 코드로 JPA가 잘 돌아가는지 확인해볼까요?](#34---------jpa-----------------)
- [4. Template Engine](#4-template-engine)
  * [4.1. Server Side vs Client Side](#41-server-side-vs-client-side)
    + [4.1.1. Server Side Template Engine](#411-server-side-template-engine)
    + [4.1.2. Client Side Template Engine](#412-client-side-template-engine)
  * [4.2. Mustache](#42-mustache)
  * [4.3. Mustache를 사용해보아요!](#43-mustache---------)
- [5. References](#references)



# 1. Hello Backend!
## 1.1. 왜 프레임워크를 쓰나요?
소프트웨어적 관점에서 **프레임워크**는 애플리케이션을 만들기 위한 큰 틀과도 같습니다. 앱의 골격을 프레임워크로부터 빌려쓴다면 개발자는 구현해야 하는 핵심 로직에만 집중할 수 있습니다. 
> 프레임워크와 함께라면, 레스토랑의 셰프에게는 재료 수급이나 매장 관리에 신경쓸 필요 없이 요리만 할 수 있는 환경이 주어집니다.

> 프레임워크와 함께라면, 개발자에게는 XML 설정과 WAS 설치 등에 신경쓸 필요 없이 로직을 짜는 것에만 집중할 수 있는 환경이 주어집니다.

## 1.2. 프레임워크는 어떻게 구성되나요?
프레임워크는 **디자인 패턴과 라이브러리를 모아 프로그램 형태로 만든 것**입니다. 
> 프레임워크 = 디자인패턴 + 라이브러리
### 1.2.1. 디자인 패턴
**디자인 패턴은 객체 지향 설계를 위한 정형화된 설계 방법입니다.** 객체지향 언어로 넘어오며 개발자가 맞닥뜨릴 수 있는 문제를 피하기 위해 무엇을 객체로 정할지, 객체 간 관계를 어떻게 정할지가 중요해졌습니다. 디자인 패턴은 코드를 처음 보는 사람도 설계의 목적, 용도, 구현 방법을 쉽게 이해하고 개발할 수 있도록 돕습니다.
### 1.2.2. 라이브러리
**라이브러리는 프로그램의 구성요소로 공통으로 사용될 수 있는 특정 기능들을 모듈화한 것입니다.** 중복된 코드를 반복해서 작성하지 않도록 하기 위해 재사용할 수 있도록 만들어둘 수 있습니다.

## 1.3. Spring과 IntelliJ
### 1.3.1. Spring의 장점
* 복잡하지 않습니다.
    * 애플리케이션 기본 설정이 간편합니다.
* 프로젝트 전체 구조 설계에 유용합니다.
    * 스프링은 웹, 데이터베이스 등 어느 한 분야에만 집중하지 않고 전체를 설계하는 용도로 사용할 수 있습니다.
* 다른 프레임워크를 포용합니다.
    * 다른 프레임워크와의 상생을 추구하므로 최소한의 수정으로 여러 종류의 프레임워크를 혼용해서 쓸 수 있습니다.
* 개발 생산성이 좋습니다.
    * XML 설정을 이용해 유지보수가 용이하고 여러 플러그인을 지원해 새로운 개발 도구에 대한 별도의 적응 없이도 개발이 가능합니다.

그 중 **Spring Boot는 스프링의 프로젝트 중 하나로, 설정이 최소화되어 간단히 실행할 수 있다는 장점이 있습니다.** 버전관리가 쉽고 JUnit 등 테스트 관련 라이브러리들이 포함되어 있어 테스트 케이스를 쉽게 작성할 수 있습니다. JAR 파일로 패키징해서 사용할 수 있어 빠르게 어플리케이션을 개발하는 데에도 효과적입니다.

~~저같은 백린이는 스프링부트 허들이 제일 낮아서 사용합니다...~~

### 1.3.2. IntelliJ Idea
IntelliJ는 대표적인 자바 웹 개발도구입니다. 

Eclipse vs IntelliJ
* 추천 기능이 강력합니다.
* 리팩토링과 디버깅 기능이 다양합니다.
* 깃 자유도가 높습니다.
* 파일을 비롯한 자원 검색이 빠릅니다.
* 자바와 스프링부트 버전업에 맞추어 업데이트가 빠릅니다.
* 대학생이시면 젯브레인 홈페이지에 가입하고 웹메일 인증 후 ultimate 버전을 무료로 사용하실 수 있습니다.

## 1.4. Gradle
**Gradle은 Groovy 기반의 오픈소스 빌드 도구입니다.** Ant의 유연성과 Maven의 편리성을 조합해 XML에 대한 이슈도 Groovy를 사용해 해결할 수 있습니다. gradle은 Maven에서 종종 발생했던 라이브러리 버전 문제, 충돌 문제 등을 해결합니다.

### 1.4.1. Gradle Build Lifecycle
1. Initialization: 빌드 대상 프로젝트를 결정하고 각각에 대한 Project 객체 생성 후 setting.gradle 파일에서 프로젝트를 구성
2. Configuration: 빌드 대상이 되는 모든 프로젝트의 빌드 스크립트를 실행
3. Execution: 구성 단계에서 생성/설정된 프로젝트의 task 중 실행 대상을 결정

### 1.4.2. Gradle 사용법
build.gradle 파일에 빌드정보를 정의해서 프로젝트에서 사용하는 환경설정, 빌드방법, 라이브러리 정보 등을 기술함으로써 빌드 및 프로젝트의 관리환경을 구성합니다.
#### __Plugin__
플러그인을 적용함으로써 프로젝트를 확장할 수 있습니다. 플러그인은 재사용을 촉진하고 더 높은 수준의 모듈화를 허용합니다. 기존에는 apply plugin을 사용했지만 [공식 홈페이지](https://docs.gradle.org/current/userguide/plugins.html#sec:using_plugins)에서는 Gradle이 더 안전하게 실행할 수 있도록 plugins를 사용하는 것을 지향합니다.
1. 기존 방법
```
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'
```
2. 개선 방법
```
plugins {
    id 'java'
    id 'eclipse'
    id 'org.springframework.boot' version '2.1.7.RELEASE'
    id 'io.spring.dependency-management' version '1.0.9.RELEASE'
}
```
#### __Repository__
![의존성 관리 모식도](https://docs.gradle.org/current/userguide/img/dependency-management-resolution.png)

**repository는 소프트웨어를 등록하서 관리하는 장소를 가리킵니다.** 로컬 환경이나 네트워크에 라이브러리를 공개하고 그 주소를 저장소로 등록하면 저장소에 있는 라이브러리를 gradle이 취득해 이용할 수 있습니다. 만약 중앙저장소에 공개되지 않거나 사내에서만 사용하는 라이브러리가 있을 경우 로컬 저장소를 이용하면 됩니다. 예를 들어 아래는 mavenCentral()과 jcenter()를 사용해 두 저장소를 이용하는 코드입니다.
```
repositories {
    mavenCentral()
    jcenter()
}
```

#### __Dependency__
**dependency는 의존성에 관한 설정을 관리하는 프로퍼티입니다.** 필요한 라이브러리를 기술하면 해당 라이브러리를 참조할 수 있습니다. 기본적으로 group, name, version 순으로 의존성을 기술하지만 group:name:version으로 축약해 쓸 수 있습니다. 만약 ext 블록으로 버전을 적어두면 dependency에 version 정보를 생략해도 자동으로 적용됩니다.
```
dependencies {
    // 기본형
    compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
    // 축약형
    compile 'org.hibernate:hibernate-core:3.6.7.Final'
}

```
#### __Test__
gradle을 통해서 테스트 또한 간편하게 할 수 있습니다. gradle은 test시에 특정 테스트만 진행할 수 있습니다. 아래는 jUnit을 사용하기 위한 코드입니다. ~~절대 글쓰는게 지쳐서가 아니라~~ 테스트 코드에 대해서는 다음 장에서 더 공부해보려 합니다.
```
test {
    useJUnitPlatform()
}
```
# 2. Testcode

## 2.1. 테스트코드를 왜 작성해야 하나요?
요즘의 개발에서 **테스트는 필수**적입니다. 이번주에 저는 Junit4를 사용해 테스트하는 방법을 익혔습니다.
* 단위 테스트는 개발단계 초기에 문제를 발견하게 도와줍니다.
* 단위 테스트를 하면 개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인할 수 있습니다.
* 단위 테스트는 기능에 대한 불확실성을 감소시킬 수 있습니다.
## 2.2. Spring boot 테스트코드는 어떻게 작성해야 하나요?
### 2.2.1. Given-when-then
테스트 코드는 일반적으로 given-when-then 형식으로 작성합니다. given 단계에서 테스트를 위해 준비하고 when 단계에서 실제로 액션하는 테스트를 실행한 후 then 단계에서 테스트를 검증합니다.
### 2.2.2. Given-when-then 연습
이번 주에 실습했던 게시글이 제대로 저장되었는지 확인하고 불러오는 코드입니다.
```
// given
String title = "테스트 게시글";
String content = "테스트 본문";

postsRepository.save(Posts.builder()
    .title(title)
    .content(content)
    .author("ei654028@gmail.com")
    .build());


// when
List<Posts> postsList = postsRepository.findAll();


//then
Posts posts = postsList.get(0);
assertThat(posts.getTitle()).isEqualTo(title);
assertThat(posts.getContent()).isEqualTo(content);
```

## 2.3. Lombok으로 자바를 더 편하게 써봅시다!
**Lombok은 자바에서 DTO, Domain 등을 만들 때 반복적으로 만들어야 하는 멤버 필드 생성자 코드를 줄여 주는 라이브러리 입니다.** Getter, Setter, ToString 등 다양한 코드를 자동완성 해 줍니다. lombok을 적용하면 코드의 가독성이 높아져 일명 코드 다이어트로 불리기도 합니다.

# 3. Java Persistence Api
## 3.1. JPA가 무엇인가요?
**Java Persistence Api는 자바 ORM 기술에 대한 API 입니다.** 본격적으로 객체지향 개발을 하게 되면서 애플리케이션은 객체지향 언어와 관계형 DB로 구성되었습니다. 

그러나 이 조합은 자바 객체를 SQL로, SQL을 자바 객체로 바꾸는 과정이 반복되어 지루한 코드가 반복된다는 문제가 있었습니다. 더불어 자바와 SQL간에는 **패러다임 불일치 문제**가 존재합니다. 자바는 추상화, 캡슐화, 정보 은닉을 통해 객체의 기능과 속성을 한 곳에서 관리하려는 패러다임을 가집니다. 반면 관계형 데이터베이스는 어떻게 데이터를 저장할지에 집중한 기술이므로 개발자들이 점점 웹 애플리케이션 개발은 데이터베이스 모델링에 치중하게 되었습니다.

JPA는 개발자가 객체지향적으로 프로그래밍한 것을 관계형 데이터베이스에 걸맞게 대신 SQL문을 생성해서 실행합니다. 따라서 JPA를 사용한 프로그램의 생산성이 높아지고 유지보수가 쉬워졌습니다. 
## 3.2. JAP를 적용해봅시다!
JPA는 인터페이스이므로 이를 사용하기 위해 구현체가 필요합니다(e.g., Hibernate, Eclipse Link). 또한 구현체를 더욱 쉽게 사용하기 위해 Spring Data JPA라는 모듈을 사용합니다.
> JPA <- Hibernate <- Spring Data JPA
또한 모듈을 사용하면 Spring Data JPA는 Hibernate 외에 다른 구현체로 교체하는 작업과 관계형 데이터베이스 외에 다른 저장소(e.g., MongoDB)로 교체하는 작업의 오버헤드를 줄여줍니다.
## 3.3. 게시글 등록 API를 만들어봅시다!
게시글 등록 기능을 만들기 위해 아래 세 파일을 만들어야 했습니다. 
* web 패키지 내에 PostApiController.java
* web.dto 패키지 내에 PostsSaveRequestDto.java
* service.posts 패키지 내에 PostsService.java

~~아쉽게도 디렉토리 구조가 왜 이렇게 되어야 하는지, 각 클래스가 어떤 역할을 하는지 온전히 파악하지 못해서 모두 정리하진 못할 것 같습니다. 추후에 내용 추가하겠습니다.~~
## 3.4. 테스트 코드로 JPA가 잘 돌아가는지 확인해볼까요?
저는 스프링 부트 프로젝트의 test 폴더 내에 앞서 연습했던 given-when-then 방식으로 테스트 코드를 작성했습니다.
```
 @Test
    public void Posts_등록된다() throws Exception{
        //given
        String title = "title";
        String content = "content";

        PostsSaveRequestDto requestDto =
                PostsSaveRequestDto.builder()
                    .title(title)
                    .content(content)
                    .author("author")
                    .build();

        String url = "http://localhost:"+port+"/api/v1/posts";

        //when
        ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url, requestDto, Long.class);

        //then
        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(responseEntity.getBody()).isGreaterThan(0L);
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(title);
        assertThat(all.get(0).getContent()).isEqualTo(content);
    }
```

# 4. Template Engine
**템플릿 엔진이란 템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성해 결과 문서를 출력하는 소프트웨어를 말합니다.** 그 중 웹 템플릿 엔진이란 웹 템플릿과 컨텐츠 정보를 처리하기 위해 설계된 소프트웨어입니다. 대부분의 템플릿 엔진은 html보다 간단한 문법을 사용하고 재사용성이 높습니다. 또한 하나의 템플릿으로 여러 페이지를 렌더링할 수 있습니다. 
## 4.1. Server Side vs Client Side
### 4.1.1. Server Side Template Engine
**서버 사이드 템플릿 엔진은 DB 혹은 API에서 가져온 데이터를 미리 정의된 템플릿에 넣어 서버에서 html을 그립니다.** html 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 부분만 템플릿에 소스 코드를 끼워넣는 방식으로 동작합니다. 
### 4.1.2. Client Side Template Engine
html 형태로 코드를 작성해 동적으로 DOM을 그리게 해주는 역할을 합니다. 클라이언트에서는 공통적인 프레임을 미리 템플릿으로 만들고 서버에서 필요한 데이터를 받아 적절한 위치에 replace합니다.
## 4.2. Mustache
저는 Spring Boot 프로젝트에 Mustache라는 마크업 언어를 사용할 것입니다. "스프링 부트와 AWS로 혼자 구현하는 웹서비스"에서는 mustache를 사용하는 이유를 다음과 같이 소개합니다.
* mustache는 수많은 언어를 지원하는 가장 심플한 템플릿 엔진입니다.
* 문법이 다른 템플릿 엔진보다 심플합니다.
* 로직 코드를 사용할 수 없어 View와 서버의 역할이 명확하게 분리됩니다. 만약 템플릿 엔진에서 너무 많은 기능을 제공하면 api와 템플릿 엔진, js가 서로 로직을 나눠 갖게 되어 유지보수가 어려워진다는 단점이 있습니다.
* mustach.js와 mustache.java를 둘 다 지원해, 하나의 문법으로 클라이언트/서버 템플릿으로 모두 사용 가능합니다.
## 4.3. Mustache를 사용해보아요!
반복적으로 사용되는 코드는 별도의 파일로 분리하여 필요한 곳에서 가져다쓰도록 레이아웃 방식으로 추가합니다. 저는 책을 따라 header와 footer 파일을 만들었습니다.

* css는 화면을 그리는 역할이므로 먼저 로딩시키기 위해 header에서 부릅니다.
* js는 용량이 클수록 로딩이 느려 body 부분의 실행이 늦어지기 때문에 footer에 둡니다.
* bootstrap.js은 jquery.js에 의존하기 때문에 footer 내에서도 아래에 위치합니다.

```
//header.mustache
<!DOCTYPE HTML>
<html>
<head>
    <title>스프링부트 웹서비스</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
```
```
//footer.mustache
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>

<!--index.js 추가-->
<script src="/js/app/index.js"></script>
</body>
</html>
```
이제 mustache 파일을 이용해 html을 구성해봅시다. 코드의 재사용성이 높아졌습니다.
```
{{>layout/header}}

<h1> 안녕하세요 여러분! </h1>

{{>layout/footer}}
```



# 5. References
Section 1
* https://elevatingcodingclub.tistory.com/25
* https://futurecreator.github.io/2016/06/18/spring-boot-get-started/
* https://mckdh.tistory.com/entry/%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%EC%9D%98-%ED%83%84%EC%83%9D-%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4%EA%B3%BC-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC
* https://freestrokes.tistory.com/79
* https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.6:dep:build_tool:gradle
* https://madplay.github.io/post/what-is-gradle
* https://willbesoon.tistory.com/93
* https://docs.gradle.org/current/userguide/plugins.html
* https://doughman.tistory.com/19
* https://webfirewood.tistory.com/129
* https://docs.gradle.org/current/userguide/core_dependency_management.html
* https://limdevbasic.tistory.com/12
* https://docs.gradle.org/current/userguide/java_testing.html

Section 2
* https://brunch.co.kr/@springboot/418
* https://martinfowler.com/bliki/GivenWhenThen.html
* https://woowabros.github.io/study/2018/03/01/spock-test.html
* https://brunch.co.kr/@springboot/292
* https://goddaehee.tistory.com/95

Section 3
* https://velog.io/@adam2/JPA%EB%8A%94-%EB%8F%84%EB%8D%B0%EC%B2%B4-%EB%AD%98%EA%B9%8C-orm-%EC%98%81%EC%86%8D%EC%84%B1-hibernate-spring-data-jpa
* https://gmlwjd9405.github.io/2019/08/03/reason-why-use-jpa.html

Section 4
* https://gmlwjd9405.github.io/2018/12/21/template-engine.html
* https://insight-bgh.tistory.com/252
