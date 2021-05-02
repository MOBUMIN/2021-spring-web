# Study with Me :)

* [6. 게시판 만들기](#6-게시판-만들기)
  * [6.1. IndexController](#61-indexcontroller)
  * [6.2. Contents Delivery Network](#62-contents-delivery-network)
  * [6.3. 게시글 등록](#63-게시글-등록)
  * [6.4. 전체 조회](#64-전체-조회)
  * [6.5. 게시글 수정](#65-게시글-수정)
  * [6.6. 게시글 삭제](#66-게시글-삭제)
* [7. 로그인 기능 구현하기](#7-로그인-기능-구현하기)
  * [7.1. Spring Security](#71-spring-security)
  * [7.2. Google Login](#72-google-login)
  * [7.3. Naver Login](#73-naver-login)
  * [7.4. Improving based on Annotations](#74-improving-based-on-annotations)
* [8. References](#8-references)

## 6. 게시판 만들기

### 6.1. IndexController

저는 4절에서 언급했던 mustache를 사용해 화면을 구성하는 연습을 했습니다.
mustache 파일들의 기본 위치는 src/main/resources/templates로 설정해
 IndexController.java의 url mapping을 도울 것입니다.
아래 코드에서는 "index"라는 문자열을 반환하므로 src/main/resources/template/index.mustache로 전환됩니다.

```java
    @RequiredArgsConstructor
    @Controller
    public class IndexController {

        @GetMapping("/")
        public String index() {
            return "index";
        }
    }
```

메인페이지를 로딩하는 테스트 코드는 사전조건이 필요없으므로 given에 해당하는 부분도 없습니다.
또한 전체 코드를 확인할 필요가 없으니, 메인 페이지에 포함된 문자열 일부가 있는지 검사하는 코드를 다음과 같이 짰습니다.

```java
    public void 메인페이지_로딩(){
        //when
        String body = this.restTemplate.getForObject("/", String.class);
        //then
        assertThat(body).contains("Spring Boot Webservice");
    }
```

### 6.2. Contents Delivery Network

게시글을 등록하는 화면을 예쁘게 구성하기 위해 프론트엔드 라이브러리를 사용했습니다.
라이브러리를 사용하는 방법에는 크게 두가지가 있습니다.

1. 직접 라이브러리 다운받기
2. CDN 사용하기

CDN은 Contents Delivery Network의 약자로 정적 컨텐츠(이미지, css, js파일 등)를 캐시해 전 세계에 복사합니다.
사용자가 사이트를 요청하면 사용자와 물리적으로 가장 가까운 서버가 데이터를 전송하도록 해 대기 시간을 줄입니다.
우리는 CDN을 사용해 프론트엔드 라이브러리를 간편하게 사용할 수 있습니다.
(다만 실제 서비스에서는 CDN 서버에 문제가 생기면 서비스에도 영향을 미치기 때문에 이 방법을 사용하지 않는다고 합니다.)
CDN을 사용하는 코드는 다음과 같습니다.

```java
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
```

### 6.3. 게시글 등록

게시글 등록 화면을 만들기 위해 다음 과정을 수행했습니다.

1. index.mustache에 글 등록 버튼을 추가합니다.

    ```mustache
        <a href="/posts/save" role="button" class="btn btn-primary">글 등록하기</a>
    ```

2. 페이지가 맵핑되도록 IndexController.java를 수정해줍니다.

    ```java
        @GetMapping("/posts/save")
        public String postsSave(){
            return "posts-save";
        }
    ```

3. 게시글을 저장하는 역할을 하는 post-save.mustache를 생성합니다.

    ```html
        {{>layout/header}}

        <h1>게시글 등록</h1>

        <div class="col-md-12">
            <div class="col-md-4">
                <form>
                    <div class="form-group">
                        <label for="title">제목</label>
                        <input type="text" class="form-control"
                        id="title" placeholder="제목을 입력하세요">
                    </div>
                    <div class="form-group">
                        <label for="author"> 작성자 </label>
                        <input type="text" class="form-control"
                        id="author" placeholder="작성자 입력">
                    </div>
                    <div class="form-group">
                        <label for="content"> 내용 </label>
                        <textarea class="form-control"
                        id="content" placeholder="내용 입력"></textarea>
                    </div>
                </form>
                <a href="/" role="button" class="btn btn-secondary">취소</a>
                <button type="button" class="btn btn-primary" id="btn-save">등록</button>
            </div>
        </div>

        {{>layout/footer}}
    ```

4. 게시글 등록 버튼이 동작할 수 있도록 index.js를 생성합니다.
경로는 src/main/resources/static/js/app 입니다.
var main 속성 안에 function을 추가함으로써 브라우저의 scope가 겹치는 문제,
 여러 사람이 작성한 코드에서 함수 이름이 중복되는 문제를 미연에 방지할 수 있습니다.

    ```javascript
        var main = {
        init: function(){
            var _this = this;
            $('#btn-save').on('click', function(){
                _this.save();
            });
        },
        save: function(){
            var data = {
                title: $('#title').val(),
                author: $('#author').val(),
                content: $('#content').val()
            };

            $.ajax({
                type: 'POST',
                url: '/api/v1/posts',
                dataType: 'json',
                contentType: 'application/json; charset=utf-8',
                data: JSON.stringify(data)
            }).done(function(){
                alert('글이 등록되었습니다.');
                window.location.href = '/';
            }).fail(function(error){
                alert(JSON.stringify(error));
            });
        }
    }

    main.init();
    ```

5. footer.js에 index.js를 추가합니다.
~~버전을 지정하지 않으면 캐시 문제로 application을 구동해도 수정된 코드가 반영되지 않는 불상사가 발생할 수 있어요...~~

    ```javascript
        <script src="/js/app/index.js?ver=2"></script>
    ```

### 6.4. 전체 조회

게시글 전체 조회 화면을 만들기 위해 다음 과정을 수행했습니다.

1. 전체 목록을 나타낼 수 있도록 index.mustache의 UI를 변경합니다.

    ```mustache
        <!-- 목록 출력 영역 -->
        <table class="table table-horizontal table-bordered">
            <thead class="thead-strong">
            <tr>
                <th>게시글번호</th>
                <th>제목</th>
                <th>작성자</th>
                <th>최종수정일</th>
            </tr>
            </thead>
            <tbody id="tbody">
            {{#posts}}
                <tr>
                    <td>{{id}}</td>
                    <td><a href="/posts/update/{{id}}">{{title}}</a></td>
                    <td>{{author}}</td>
                    <td>{{modifiedDate}}</td>
                </tr>
            {{/posts}}
            </tbody>
        </table>
    ```

2. 가독성을 높이기 위해 @Query 어노테이션을 사용해 PostsRepository.java를 작성합니다.

    ```java
        public interface PostsRepository extends JpaRepository<Posts, Long>{
            @Query("SELECT p FROM Posts p ORDER BY p.id DESC")
            List<Posts> findAllDesc();
        }
    ```

3. PostRepository 결과로 넘어온 Stream을 Dto로 변환해 List로 반환하기 위해 PostsService.java를 수정합니다.

    ```java
        @Transactional(readOnly = true)
        public List<PostsListResponseDto> findAllDesc(){
            return postsRepository.findAllDesc().stream()
                    .map(PostsListResponseDto::new)
                    .collect(Collectors.toList());
        }
    ```

4. PostListResponseDto.java를 작성합니다.

    ```java
        @Getter
        public class PostsListResponseDto {
            private Long id;
            private String title;
            private String author;
            private LocalDateTime modifiedDate;

            public PostsListResponseDto(Posts entity){
                this.id = entity.getId();
                this.title = entity.getTitle();
                this.author = entity.getAuthor();
                this.modifiedDate = entity.getModifiedDate();
            }
        }
    ```

5. PostController.java가 게시글 목록을 보여줄 수 있도록 수정합니다.

    ```java
    @GetMapping("/")
        public String index(Model model) {
            model.addAttribute("posts", postsService.findAllDesc());
            return "index";
        }
    ```

### 6.5. 게시글 수정

게시글을 수정하는 기능을 구현했습니다.
이 부분은 코드를 최소한으로 작성하고 어떤 과정을 거쳤는지 기록하겠습니다!

1. 게시글을 수정하기 위해 template 폴더 내에 posts-update.mustache를 생성했습니다.
2. update 메소드를 index.js var main 속성 내에 추가했습니다.
3. index.mustache에 게시글 수정 시각이 표시되도록 modifiedDate 속성을 추가했습니다.
4. IndexController에 update id에 따라 url을 맵핑해주는 기능을 추가했습니다.

    ```java
        @GetMapping("/posts/update/{id}")
        public String postsUpdate(@PathVariable Long id, Model model) {
            PostsResponseDto dto = postsService.findById(id);
            model.addAttribute("post", dto);

            return "posts-update";
        }
    ```

### 6.6. 게시글 삭제

게시글을 삭제하는 기능을 구현하기 위해 다음과 같은 과정을 거쳤습니다.

1. 삭제 기능은 수정 페이지에서 구현되어야 하므로 posts-update.mustache 내에 삭제 버튼을 추가합니다.
2. index.js var main 속성 내에 delete 함수를 정의했습니다.
3. PostsService.java에 삭제 기능을 처리하는 API를 추가했습니다.

    ```java
        @Transactional
        public void delete(Long id){
            Posts posts = postsRepository.findById(id).orElseThrow(()
            ->new IllegalArgumentException("해당 게시글이 없습니다. id="+id));
            postsRepository.delete(posts);
        }
    ```

4. PostsService.java에서 만든 메소드를 사용할 수 있도록 PostApiController.java에 다음 코드를 추가합니다.

    ```java
        @DeleteMapping("/api/v1/posts/{id}")
        public Long delete(@PathVariable Long id){
            postsService.delete(id);
            return id;
        }
    ```

## 7. 로그인 기능 구현하기

구글/카카오/네이버 계정 사용해서 로그인하기 기능을 사용해보신 적이 있나요?
소셜 로그인은 우리가 처음 접하는 사이트에서 id와 password를 만들지 않고도 서비스를 사용할 수 있도록 돕습니다.
또한 개발자들을 api 구현의 고통에서 벗어나게 해주죠!
OAuth 로그인을 사용하면 개발자들은 아래와 같은 기능을 신경쓰지 않고 서비스 개발에만 집중할 수 있습니다.

* 로그인 시 보안
* 회원가입 시 이메일 혹은 전화번호 인증
* 비밀번호 찾기
* 비밀번호 변경
* 회원정보 변경

### 7.1. Spring Security

Spiring Sequrity는 스프링 기반 어플리케이션의 보안을 담당하는 프레임워크입니다.
보안과 관련된 다양한 옵션을 지원하고 java bean 설정만으로도 간단하게 사용할 수 있습니다.
본 섹션에서는 Spring Sequrity가 무엇이고 어떻게 동작하는지 보다도, OAuth에 사용된다는 점만 알고 넘어가려 합니다.

### 7.2. Google Login

구글 로그인을 제 SpringBoot 프로젝트와 연동시키기 위해 구글 클라우드 플랫폼에서 신규 서비스 정보를 만들었습니다.
application-oauth.properties에 여기서 생성한 client id와 보안 코드를 붙여넣습니다.

```properties
spring.security.oauth2.client.registration.google.client-id="제 클라이언트 id"
spring.security.oauth2.client.registration.google.client-secret="제 클라이언트 secret"
spring.security.oauth2.client.registration.google.scope = profile, email
```

잊지말고 .gitignore에도 등록해주어야 합니다!

구글 로그인을 연동하기 위해 다음 과정을 거쳤습니다.

1. domain/user에 User 패키지를 생성합니다.

    ```java
        @Getter
        @NoArgsConstructor
        @Entity
        public class User extends BaseTimeEntity {

            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;

            @Column(nullable = false)
            private String name;

            @Column(nullable = false)
            private String email;

            @Column
            private String picture;

            @Enumerated(EnumType.STRING)
            @Column(nullable = false)
            private Role role;

            @Builder
            public User(String name, String email, String picture, Role role) {
                this.name = name;
                this.email = email;
                this.picture = picture;
                this.role = role;
            }

            public User update(String name, String picture) {
                this.name = name;
                this.picture = picture;

                return this;
            }

            public String getRoleKey() {
                return this.role.getKey();
            }
        }
    ```

2. 각 사용자의 권한을 관리할 Enum 클래스 Role.java을 생성합니다.

    ```java
        @Getter
        @RequiredArgsConstructor
        public enum Role {
            GUEST("ROLE_GUEST", "손님"),
            USER("ROLE_USER", "일반 사용자");
            private final String key;
            private final String title;
        }
    ```

3. USER의 CRUD를 다루기 위해 UserRepository.java를 생성합니다.

    ```java
        public interface UserRepository extends JpaRepository<User, Long> {
        Optional<User> findByEmail(String email);
        }
    ```

스프링 시큐리티를 설정하기 위해 다음 과정을 거쳤습니다.

1. build.gradle에 스프링 시큐리티 의존성을 추가합니다.

    ```gradle
        compile('org.springframework.boot:spring-boot-starter-oauth2-client')
    ```

2. config/auth 패키지를 생성해 관련 파일을 작성합니다.
    * config/auth/SecurityConfig.java
    * config/auth/CustomOAuth2UserService.java
    * config/auth/OAuthAttributes.java
    * config/auth/dto/SessionUser.java

~~왜 이 파일들이 필요한지에 대해서는 모두 이해하지 더 공부한 후 다음 PR때 추가하도록 하겠습니다..!~~

### 7.3. Naver Login

~~추가예정입니다~~

### 7.4. Improving based on Annotations

~~추가예정입니다.~~

## 8. References

Section 6

* <https://brownbears.tistory.com/408>
* <https://goddaehee.tistory.com/173>

Section 7

* <https://mangkyu.tistory.com/76>
* <https://sjh836.tistory.com/165>
