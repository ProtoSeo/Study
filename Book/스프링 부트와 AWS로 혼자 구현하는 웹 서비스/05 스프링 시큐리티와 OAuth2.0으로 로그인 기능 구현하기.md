# 05 스프링 시큐리티와 OAuth2.0으로 로그인 기능 구현하기
> 스프링 시큐리티는 막강한 인증과 인가기능을 가진 프레임워크이다.    
> 사실상 스프링 기반의 애플리케이션에서는 보안을 위한 표준이다.

## 5.1 스프링 시큐리티와 스프링 시큐리티 Oauth2 클라이언트
많은 서비스에서 로그인 기능을 id/password 방식보다는 구글, 페이스북, 네이버 로그인과 같은 소셜 로그인 기능을 사용한다.   
- 직접 로그인을 구현하면 아래의 기능을 직접 구현해야한다.
    + 로그인 시 보안
    + 비밀번호 찾기
    + 비밀번호 변경
    + 회원정보 변경
    + 회원가입 시 이메일 혹은 전화번호 인증

- 스프링 부트 1.5 vs 스프링 부트 2.0
    + 스프링 부트 1.5에서의 OAuth 연동 방법이 2.0에서는 크게 변경되었다. 하지만, 인터넷 자료를 보면, **설정 방법에 크게 차이가 없는 경우가 있다.**
    + 그 이유는 `spring-security-oauth2-autoconfigure`라이브러리 때문이다.
- 이 책에서는 스프링 부트 2 방식인 `Spring Security Oauth2 Client` 라이브러리를 사용해서 진행한다.
    + 스프링 팀에서 기존 1.5에서 사용되던 `spring-security-oauth`프로젝트는 유지 상태로 결정했으며 더는 신규 기능은 추가되지 않고 버그 수정 정도의 기능만 추가될 예정, 신규 기능은 새 oauth2 라이브러리에서만 지원하겠다고 선언
    + 스프링 부트용 라이브러리(starter)출시
    + 기존에 사용되던 방식은 확장 포인트가 적절하게 오픈되어 있지 않아 직접 상속하거나 오버라이딩 해야하고 신규 라이브러리의 경우 확장 포인트를 고려해서 설계된 상태

## 5.2 구글 서비스 등록
먼저 구글 서비스에 신규 서비스를 생성한다.   
여기서 발급된 인증 정보(clientId와 clientSecret)을 통해서 로그인 기능과 소셜 서비스 기능을 사용할 수 있으니 무조건 발급 받아야한다.

1. [구글 클라우드 플랫폼 주소](https://console.cloud.google.com/)로 이동한다.
2. **프로젝트 선택** 탭을 클릭한 다음, **새 프로젝트** 버튼을 클릭한다.
3. 등록될 서비스의 이름을 입력한다. 원하는 이름으로 지으면 된다.
4. 생성이 완료된 프로젝트를 선택하고 왼쪽 메뉴 탭을 클릭해서 `API 및 서비스`카테고리로 이동한다.
5. 사이드바 중간에 있는 **사용자 인증 정보**를 클릭하고 **사용자 인증 정보 만들기** 버튼을 클릭한다.
6. 사용자 인증 정보에는 여러 메뉴가 있는데 이 중 이번에 구현할 소셜 로그인은 OAuth 클라이언트 ID로 구현한다.**OAuth 클라이언트 ID** 항목을 클릭한다.
7. 클라이언트 ID가 생성되기 전에 동의 화면 구성이 필요하므로 안내에 따라 **동의 화면 구성**버튼을 클릭한다.
8. 동의 화면에는 3개의 탭이 있는데, 이 중 **OAuth 동의 화면**탭에서 각 항목을 작성한다.
    - 애플리케이션 이름 : 구글 로그인 시 사용자에게 노출될 애플리케이션 이름을 이야기 한다.
    - 지원 이메일 : 사용자 동의 화면에서 노출될 이메일 주소이다. 보통은 서비스의 help 이메일 주소를 사용하지만, 여기서는 독자의 이메일 주소를 사용하면 된다.
    - Google API의 범위 : 이번에 등록할 구글 서비스에서 사용할 범위 목록이다. 기본값은 email/profile/openid이며, 여기서는 딱 기본 범위만 사용한다. 이외 다른 정보들도 사용하고 싶다면 범위 추가 버튼으로 추가하면 된다.
9. 동의 화면 구성이 끝났으면 OAuth 클라이언트 ID 만들기 화면으로 이동한다.
10. **애플리케이션 유형**을 웹 애플리케이션으로 선택하고, **프로젝트 이름**을 등록한다.
11. 밑으로 내려가서 **승인된 리디렉션 URI**항목에 `http://localhost:8080/login/oauth2/code/google`을 등록한다.
> 승인된 리디렉션 URI
> - 서비스에서 파라미터로 인증 정보를 주었을 때 인증이 성공하면 구글에서 리다이렉트할 URL이다.
> - 스프링 부트 2 버전의 시큐리티에서는 기본적으로 `{도메인}/login/oauth2/code/{소셜서비스코드}`로 리다이렉트 URL을 지원하고 있다.
> - 사용자가 별도로 리다이렉트 URL을 지원하는 Controller를 만들 필요가 없다. 시큐리티에서 이미 구현해 놓은 상태이다.
12. 생성 버튼을 클릭하면 다음과 같이 생성된 클라이언트 정보를 볼 수 있고 생성된 애플리케이션을 클릭하면 그림과 같이 인증 정보를 볼 수 있다.
13. 클라이언트 ID와 클라이언트 보안 비밀 코드를 프로젝트에서 설정한다.
14. `src/main/resources/` 디렉토리에 `application-oauth.properties` 파일을 생성한 후, 코드를 작성한다.
```properties
spring.security.oauth2.client.registration.google.client-id=클라이언트 ID
spring.security.oauth2.client.registration.google.client-secret=클라이언트 보안 비밀
spring.security.oauth2.client.registration.google.scope=profile,email
```
- 많은 예제에서는 이 scope를 별도로 등록하지 않는다.
- 기본 값이 `openid, profile, email`이기 때문이다.
- 강제로 `profile, email`을 등록한 이유는 openid라는 scope 가 있으면 Open id Provider로 인식하기 때문이다.
- 이렇게 되면 OpenIdProvider인 서비스(구글)과 같지 않은 서비스(네이버/카카오 등)로 나눠서 각각 OAuth2Service를 만들어야한다.
- 하나의 OAuth2Service를 사용하기 위해 일부러 openid scope를 빼고 등록한다.

스프링 부트에서는 properties의 이름을 application-xxx.properties로 만들면 xxx라는 이름의 **profile**이 생성되어 이를 통해 관리할 수 있다.   
즉, `profile=xxx`라는 식으로 호출하면 **해당 properties의 설정들을 가져올** 수 있다.   
호출하는 방식은 여러 방식이 있지만 이 책에서는 스프링 부트의 기본 설정 파일인 `application.properties`에서 `application-oauth.properties`를 포함하도록 구성한다.   
15. `application.properties`에 다음과 같이 코드를 추가한다.
```properties
```
- 이제 이 설정값을 사용할 수 있게 되었다.

16. `.gitignore`에 `application-oauth.properties`를 등록한다.
- 구글 로그인을 위한 클라이언트 ID와 클라이언트 보안 비밀을 보안이 중요한 정보들이다.
- 이들이 외부에 노출될 경우 언제든 개인정보를 가져갈 수 있는 취약점이 될 수 있다.
- 따라서 보안을 위해 깃허브에 `application-outh.properties`파일이 올라가는 것을 방지한다.

## 5.3 구글 로그인 연동하기
1. 사용자 정보를 담당할 도메인인 `User` 클래스를 `domain/user`패키지에 생성한다.
2. `User`클래스에 코드 작성
```java
package org.example.domain.user;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import org.example.domain.BaseTimeEntity;

import javax.persistence.*;

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
    public User(String name, String email, String picture,Role role){
        this.name = name;
        this.email = email;
        this.picture = picture;
        this.role = role;
    }

    public User update(String name, String picture){
        this.name = name;
        this.picture = picture;

        return this;
    }

    public String getRoleKey(){
        return this.role.getKey();
    }
}
```
- `@Enumerated(EnumType.STRING)`
    + JPA로 데이터베이스로 저장할 때 Enum 값을 어떤 형태로 저장할지를 결정한다.
    + 기본적으로는 int로 된 숫자가 저장된다.
    + 숫자로 저장되면 데이터베이스로 확인할 때 그 값이 무슨 코드를 의미하는지 알 수가 없다.
    + 그래서 문자열(EnumType.STRING)로 저장될 수 있도록 선언한다.   

3. 각 사용자의 권한을 관리할 Enum 클래스 `Role`을 생성한다.
```java
package org.example.domain.user;

import lombok.Getter;
import lombok.RequiredArgsConstructor;

@Getter
@RequiredArgsConstructor
public enum Role {
    GUEST("ROLE_GUEST","손님"),
    USER("ROLE_USER","일반 사용자");

    private final String key;
    private final String title;
}
```
- 스프링 시큐리티에서는 권한 코드에 항상 **`ROLE_`이 앞에 있어야만**한다.
- 그래서 코드별 키 값을 `ROLE_GUEST`, `ROLE_USER` 등으로 지정한다.

4. User의 CRUD를 책임질`UserRepository`도 생성한다.
```java
package org.example.domain.user;

import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface UserRepository extends JpaRepository<User,Long> {
    Optional<User> findByEmail(String email); 
}
```
- `findByEmail`
    + 소셜 로그인으로 반환되는 값 중 `email`을 통해 이미 생성된 사용자인지 처음 가입하는 사용자인지 판단하기 위한 메서드이다.

### 스프링 시큐리티 설정
1. `build.gradle`에 스프링 시큐리티 관련 의존성을 추가한다.
```gradle
dependencies {
    ...
    implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
}
```
- `spring-boot-starter-oauth2-client`
    + 소셜 로그인 등 클라이언트 입장에서 소셜 기능 구현 시 필요한 의존성이다.
    + `spring-security-oauth2-client`와 `spring-security-oauth2-jose`를 기본으로 관리해 준다.
2. `config.auth` 패키지를 생성한다.
- 앞으로 **시큐리티 관련 클래스는 모두 이곳에** 담는다.
3. `SecurityConfig` 클래스를 생성하고 코드를 작성한다.
```java
package org.example.config.auth;

import lombok.RequiredArgsConstructor;
import org.example.domain.user.Role;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    private final CustomOAuth2UserService customOAuth2UserService;

    @Override
    protected void configure(HttpSecurity http) throws Exception{
        http.csrf().disable().headers().frameOptions().disable()
                .and()
                    .authorizeRequests()
                    .antMatchers("/","/css/**","/images/**","/js/**","/h2-console/**")
                    .permitAll().antMatchers("/api/v1/**").hasRole(Role.USER.name())
                    .anyRequest().authenticated()
                .and()
                    .logout().logoutSuccessUrl("/")
                .and()
                    .oauth2Login().userInfoEndpoint().userService(customOAuth2UserService);
    }
}
```
- `@EnableWebSecurity`
    + Spring Security 설정들을 활성화 시켜준다.
- `.csrf().disable().headers().frameOptions()`
    + h2-console 화면을 사용하기 위해 해당 옵션들을 disable한다.
- `authorizeRequests`
    + URL별 권한 관리를 설정하는 옵션의 시작점이다.
    + `authorizeRequests`가 선언되어야만 `andMatchers`옵션을 사용할 수 있다.
- `andMatchers`
    + 권한 관리 대상을 지정하는 옵션이다.
    + URL, HTTP 메서드별로 관리가 가능하다
    + "/"등 지정된 URL 들은 `permitAll()`옵션을 통해 전체 열람 권한을 주었다.
    + "api/v1/**" 주소를 가진 API는 USER 권한을 가진 사람만 가능하도록 했다.
- `andRequest`
    + 설정된 값들 이외 나머지 URL들을 나타낸다.
    + 여기서는 `authenticated()`을 추가하여 나머지 URL들을 모두 인증된 사용자들에게만 허용하게 한다.
    + 인증된 사용자 즉, 로그인한 사용자들을 이야기한다.
- `logout().logoutSuccessUrl("/")`
    + 로그아웃 기능에 대한 여러 설정의 진입점이다.
    + 로그아웃 성공시 "/"주소로 이동한다.
- `oauth2Login`
    + OAuth2 로그인 기능에 대한 여러 설정의 진입점이다.
- `userInfoEndPoint`
    + OAuth2 로그인 성공 이후 사용자 정보를 가져올 때의 설정들을 담당한다.
- `userService`
    + 소셜 로그인 성공 시 후속 조치를 진행할 `UserService` 인터페이스의 구현체를 등록한다.
    + 리소스 서버(즉, 소셜 서비스들)에서 사용자 정보를 가져온 상태에서 추가로 진행하고자 하는 기능을 명시할 수 있다.
4. `CustomOAuth2UserService` 클래스를 생성한 뒤 코드를 작성한다.
```java
package org.example.config.auth;

import lombok.RequiredArgsConstructor;
import org.example.config.auth.dto.OAuthAttributes;
import org.example.config.auth.dto.SessionUser;
import org.example.domain.user.User;
import org.example.domain.user.UserRepository;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.oauth2.client.userinfo.DefaultOAuth2UserService;
import org.springframework.security.oauth2.client.userinfo.OAuth2UserRequest;
import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
import org.springframework.security.oauth2.core.OAuth2AuthenticationException;
import org.springframework.security.oauth2.core.user.DefaultOAuth2User;
import org.springframework.security.oauth2.core.user.OAuth2User;
import org.springframework.stereotype.Service;

import javax.servlet.http.HttpSession;
import java.util.Collections;

@RequiredArgsConstructor
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {
    private final UserRepository userRepository;
    private final HttpSession httpSession;

    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
        OAuth2UserService<OAuth2UserRequest,OAuth2User> delegate = new DefaultOAuth2UserService();
        OAuth2User oAuth2User = delegate.loadUser(userRequest);

        String registrationId = userRequest.getClientRegistration().getRegistrationId();
        String userNameAttributeName = userRequest.getClientRegistration().getProviderDetails()
                .getUserInfoEndpoint().getUserNameAttributeName();
        OAuthAttributes attributes = OAuthAttributes.of(registrationId, userNameAttributeName, oAuth2User.getAttributes());

        User user = saveOrUpdate(attributes);

        httpSession.setAttribute("user",new SessionUser(user));

        return new DefaultOAuth2User(Collections.singleton(new SimpleGrantedAuthority(user.getRoleKey())),
                attributes.getAttributes(),
                attributes.getNameAttributeKey());
    }

    private User saveOrUpdate(OAuthAttributes attributes){
        User user = userRepository.findByEmail(attributes.getEmail())
                .map(entity -> entity.update(attributes.getName(),attributes.getPicture())).orElse(attributes.toEntity());
        return userRepository.save(user);
    }
}
```
- `@registrationId`
  + 현재 로그인 진행 중인 서비스를 구분하는 코드이다.
  + 지금은 구글만 사용하는 불필요한 값이지만, 이후 네이버 로그인 연동 시에 네이버 로그인인지, 구글 로그인인지 구분하기 위해 사용한다.
- `userNameAttributeName`
    + OAuth2 로그인 진행 시 키가 되는 필드값을 이야기 한다. Primary Key와 같은 의미이다.
    + 구글의 경우 기본적으로 코드를 지원하지만, 네이버 카카오 등은 기본 지원하지 않는다. 구글의 기본 코드는 "sub"이다.
    + 이후 네이버 로그인과 구글 로그인을 동시 지원할 때 사용된다.
- `OAuthAttributes`
    + `OAuthAttributes`를 통해 가져온 `OAuth2User`의 attribute를 담을 클래스이다.
    + 이후 네이버 등 다른 소셜 로그인도 이 클래스를 사용한다.
    + 바로 아래에서 이 클래스의 코드가 나오니 차례로 생성하면 된다.
- `SessionUser`
    + 세션에 사용자 정보를 저장하기 위한 Dto 클래스 이다.
- 구글 사용자 정보가 업데이트 되었을 때를 대비하여 **update**기능도 같이 구현하였다.
5. `OAuthAttributes` 클래스를 `config.auth.dto`패키지에 생성한다.
```java
package org.example.config.auth.dto;

import lombok.Builder;
import lombok.Getter;
import org.example.domain.user.Role;
import org.example.domain.user.User;

import java.util.Map;

@Getter
public class OAuthAttributes {
    private Map<String, Object> attributes;
    private String nameAttributeKey;
    private String name;
    private String email;
    private String picture;

    @Builder
    public OAuthAttributes(Map<String,Object> attributes, String nameAttributeKey, String name,String email,String picture){
        this.attributes = attributes;
        this.nameAttributeKey = nameAttributeKey;
        this.name = name;
        this.email = email;
        this.picture = picture;
    }

    public static OAuthAttributes of(String registrationId, String userNameAttributeName, Map<String, Object> attributes){
        return ofGoogle(userNameAttributeName,attributes);
    }

    public static OAuthAttributes ofGoogle(String userNameAttributeName, Map<String, Object> attributes){
        return OAuthAttributes.builder()
                .name((String) attributes.get("name"))
                .email((String) attributes.get("email"))
                .picture((String) attributes.get("picture"))
                .attributes(attributes)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    public User toEntity(){
        return User.builder()
                .name(name)
                .email(email)
                .picture(picture)
                .role(Role.GUEST)
                .build();
    }
}
```
- `of()`
    + `OAuth2User`에서 반환하는 사용자 정보는 Map이기 때문에 값 하나하나를 변환해야만 한다.
- `toEntity()`
    + User 엔티티를 생성한다.
    + `OAuthAttributes`에서 엔티티를 생성하는 시점은 처음 가입할 때이다.
    + 가입할 때의 기본 권한을 GUEST로 주기 위해서 role 빌더값에는 Role.GUEST를 사용한다.

6. 같은 패키지에 `SessionUser` 클래스를 생성한다.
```java
package org.example.config.auth.dto;

import lombok.Getter;
import org.example.domain.user.User;

import java.io.Serializable;

@Getter
public class SessionUser implements Serializable {
    private String name;
    private String email;
    private String picture;

    public SessionUser(User user) {
        this.name = user.getName();
        this.email = user.getEmail();
        this.picture = user.getPicture();
    }
}

```
- `SessionUser`에는 **인증된 사용자 정보**만 필요하다.
- 그 외에 필요한 정보들은 없으니, name, email, picture만 필드로 선언한다.
> **왜 User 클래스를 사용하면 안되는가?**  
> User 클래스에 **직렬화가 구현되지 않았기 때문**이다.  
> 
> **왜 User 클래스에 직력화 코드를 넣으면 안될까?**  
> **User 클래스가 엔티티**이기 때문이다.   
> 엔티티 클래스는 언제 다른 엔티티와 관계가 형성될지 모른다.   
> 예를 들어 `@OneToMany`, `@ManyToMany`등 자식 엔티티를 갖고 있다면 직렬화 대상에 자식들까지 포함되니 **성능 이슈, 부수효과**가 발생할 확률이 높다.   
> 그래서 **직렬화 기능을 가진 세션 Dto**를 하나 추가로 만드는 것이 이후 운영 및 유지보수 때 많은 도움이 된다.

### 로그인 테스트
1. `index.mustache`에 로그인 버튼과 로그인 성공 시 사용자 이름을 보여주는 코드를 추가한다.
```html
{{>layout/header}}
<h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
<div class="col-md-12">
    <!-- 로그인 기능 영   -->
    <div class="row">
        <div class="col-md-6">
            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
            {{#userName}}
                Logged in as: <span id="user">{{userName}}</span>
                <a href="/logout" class="btn btn-info active" role="button">Logout</a>
            {{/userName}}
            {{^userName}}
                <a href="/oauth2/authorization/google" class="btn btn-success active" role="button">Google Login</a>
            {{/userName}}
        </div>
    </div>
    <br>
    <!-- 목록 출력 영역 -->
    ...
</div>
{{>layout/footer}}
```
- `{{#userName}}`
    + 머스테치는 다른 언어와 같은 if문을 제공하지 않는다.
    + true/false 여부만 판단할 뿐이다.
    + 그래서 머스테치에서는 항상 최종값을 넘겨줘야 한다.
    + 여기서도 역시 userName이 있다면 userName을 노출시키도록 구성하였다.
- `a href ="/logout"`
    + 스프링 시큐리티에서 기본적으로 제공하는 로그아웃 URL이다.
    + 즉, 개발자가 별도로 저 URL에 해당하는 컨트롤러를 만들 필요가 없다.
    + `SecurityConfig` 클래스에서 URL을 변경할 순 있지만 기본 URL을 사용해도 충분하니 여기서는 그대로 사용한다.
- `{{^userName}}`
    + 머스테치에서 해당 값이 존재하지 않는 경우에는 ^를 사용한다.
    + 여기서는 userName이 없다면 로그인 버튼을 노출시키도록 구성했다.
- `a href="/oauth2/authorization/google"`
    + 스프링 시큐리티에서 기본적으로 제공하는 로그인 URL이다.
    + 로그아웃 URL과 마찬가지로 개발자가 별도의 컨트롤러를 생성할 필요가 없다.
2. `index.mustache`에서 userName을 사용할 수 있게 `indexController`에서 userName을 model에 저장하는 코드를 추가한다.
```java
@RequiredArgsConstructor
@Controller
public class IndexController {
    private final PostsService postsService;
    private final HttpSession httpSession;
    @GetMapping("/")
    public String index(Model model){
        model.addAttribute("posts",postsService.findAllDesc());
        SessionUser user = (SessionUser) httpSession.getAttribute("user");
        if(user != null){
            model.addAttribute("userName", user.getName());
        }
        return "index";
    }
    ...
}
```
- ` (SessionUser) httpSession.getAttribute("user")`
    + 앞서 작성된 CustomOAuth2UserService에서 로그인 성공 시 세션에 `SesionUser`를 저장하도록 구성했다.
    + 즉, 로그인 성공 시 `httpSession.getAttribute("user")`에서 값을 가져올 수 있다.
- `if(user != null)`
    + 세션에 저장된 값이 있을 때만 model에 userName으로 등록한다..
    + 세션에 저장된 값이 없으면 model엔 아무런 값이 없는 상태이니 로그인 버튼이 보이게 된다.

## 5.4 어노테이션 기반으로 개선하기
1. `config.auth`패키지에 `@LoginUser`어노테이션을 생성하고 코드를 작성한다.
```java
package org.example.config.auth;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser {
}
```
- `@Target(ElementType.PARAMETER)`
    + 이 어노테이션이 생성될 수 있는 위치를 지정한다.
    + PARAMETER로 지정했으니 메서드의 파라미터로 선언된 객체에서만 사용할 수 있다.
    + 이 외에도 클래스 선언문에 쓸 수 있는 TYPE 등이 있다.
- `@interface`
    + 이 파일을 어노테이션 클래스로 지정한다.
    + LoginUser라는 이름을 가진 어노테이션이 생성되었고 보면 된다.
2. 같은 위치에 `LoginUserArgumentResolver`를 생성한다. `LoginUserArgumentResolver`라는 `HandleMethodArgumentResolver` 인터페이스를 구현한 클래스이다.
```java
package org.example.config.auth;

import lombok.RequiredArgsConstructor;
import org.example.config.auth.dto.SessionUser;
import org.springframework.core.MethodParameter;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;

import javax.servlet.http.HttpSession;

@RequiredArgsConstructor
@Component
public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {
    private final HttpSession httpSession;

    @Override
    public boolean supportsParameter(MethodParameter parameter){
        boolean isLoginUserAnnotation = parameter.getParameterAnnotation(LoginUser.class) != null;
        boolean isUserClass = SessionUser.class.equals(parameter.getParameterType());
        return isLoginUserAnnotation && isUserClass;
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                                  NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception{
        return httpSession.getAttribute("user");
    }
}
```
- `supportsParameter()`
    + 컨트롤러 메서드의 특정 파라미터를 지원하는지 판단한다.
    + 여기서는 파라미터에 `@LoginUser` 어노테이션이 붙어 있고, 파라미터 클래스 타입이 `SessionUser.class`인 경우 true를 반환한다.
- `@ResolveArgument()`
    + 파라미터로 전달할 객체를 생성한다.
    + 여기서는 세션에서 객체를 가져온다.

3. 이렇게 생성된 `LoginUserArgumentResolver`가 **스프링에서 인식될 수 있도록** `WebMvcConfigurer`에 추가한다.
4. `config` 패키지에 `WebConfig`클래스를 생성하여 다음과 같이 설정을 추가한다.
```java
package org.example.config;

import lombok.RequiredArgsConstructor;
import org.example.config.auth.LoginUserArgumentResolver;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.List;

@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {
    private final LoginUserArgumentResolver loginUserArgumentResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers){
        argumentResolvers.add(loginUserArgumentResolver);
    }
}
```
- `HandlerMethodArgumentResolver`는 항상 `WebMvcConfigurer`의 `addArgumentResolvers()`를 통해 추가해야 한다.
5. `IndexController` 코드에서 반복되는 부분을 `@LoginUser` 어노테이션을 통해 개선
```java
@RequiredArgsConstructor
@Controller
public class IndexController {
    private final PostsService postsService;
    private final HttpSession httpSession;
    @GetMapping("/")
    public String index(Model model,@LoginUser SessionUser user){
        model.addAttribute("posts",postsService.findAllDesc());
        if(user != null){
            model.addAttribute("userName", user.getName());
        }
        return "index";
    }
    ...
}
```
- `@LoginUser SessionUser user`
    + 기존에 `(User) httpSession.getAttribute("user")`로 가져오던 세션 정보 값이 개선되었다.
    + 이제는 어느 컨트롤러든지 `@LoginUser`만 사용하면 세션 정보를 가져올 수 있게 되었다.
## 5.5 세선 저장소로 데이터베이스 사용하기
> **애플리케이션을 재실행**하면 로그인이 풀리는 이유?   
> 세션이 **내장 톰캣의 메모리에 저장**되기 때문이다.   
> 기본적으로 세션은 실행되는 WAS의 메모리에서 저장되고 호출된다.   
> 메모리에 저장되다 보니 **내장 톰캣처럼 애플리케이션 실행 시 실행되는 구조에선 항상 초기화**가 된다.   
> 즉, **배포할 때 마다 톰캣이 재시작** 되는 것이다.
> 또한, 2대 이상의 서버에서 서비스 하고 있다면 **톰캣마다 세션 동기화** 설정을 해야만 한다.

#### 해결방법
1. 톰캣 세션을 사용한다.
    + 일반적으로 별다른 설정을 하지 않을 때 기본적으로 선택되는 방식이다.
    + 이렇게 될 경우 톰캣(WAS)에 세션이 저장되기 때문에 2대 이상의 WAS가 구동되는 환경에서는 톰캣들 간의 세션 공유를 위한 추가 설정이 필요하다.
2. MySQL과 같은 데이터베이스를 세션 저장소로 사용한다.
    + 여러 WAS 간의 공용 세션을 사용할 수 있는 가장 쉬운 방법이다.
    + 많은 설정이 필요 없지만, 결국 로그인 요청마다 DB IO가 발생하여 성능상 이슈가 발생할 수 있다.
    + 보통 로그인 요청이 많이 없는 백오피스, 사내 시스템 용도에서 사용한다.
3. Redis, Memcached와 같은 메모리 DB를 세션 저장소로 사용한다.
    + B2C 서비스에서 가장 많이 사용하는 방식이다.
    + 실제 서비스로 사용하기 위해서는 Embedded Redis와 같은 방식이 아닌 외부 메모리 서버가 필요하다.

이 책에서는 두 번째 방식인 **데이터베이스를 세션 저장소**로 사용하는 방식을 선택하여 진행한다.
- **설정이 간단**하고 사용자가 많은 서비스가 아니며 비용 절감을 위해서이다.

1. `build.gradle`에 `spring-session-jdbc` 등록
```gradle
dependencies {
    ...
    implementation 'org.springframework.session:spring-session-jdbc'
}
```
2. `application.properties`에 세션저장소를 jdbc로 선택하도록 코드를 추가한다.
```properties
...
spring.session.store-type=jdbc
```

## 5.6 네이버 로그인
1. [네이버 오픈 API](https://developers.naver.com/apps/#/wizard/register)이동하고 각 항목을 채운다.
- 애플리케이션 이름
- 사용 API
- 로그인 오픈 API 서비스 환경
- 사용환경 = PC 웹
- 서비스 URL = http://localhost:8080
- 네이버아이디로 로그인 Callback URL = http://localhost:8080/login/oauth2/code/naver

2. 발급받은 Client ID와 Client Secret을 `application-oauth.properties`에 등록한다. (네이버에서는 스프링 시큐리티를 공식 지원하지 않기 때문에 CommonOAuth2Provider에서 해주던 값들도 전부 수동으로 입력해야 한다.)
```properties
...
# registration
spring.security.oauth2.client.registration.naver.client-id= ...
spring.security.oauth2.client.registration.naver.client-secret= ...
spring.security.oauth2.client.registration.naver.redirect-uri={baseUrl}/{action}/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.naver.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.naver.scope=name,email,profile_image
spring.security.oauth2.client.registration.naver.client-name=Naver

# provider
spring.security.oauth2.client.provider.naver.authorization_uri=https://nid.naver.com/oauth2.0/authorize
spring.security.oauth2.client.provider.naver.token_uri=https://nid.naver.com/oauth2.0/token
spring.security.oauth2.client.provider.naver.user-info-uri=https://openapi.naver.com/v1/nid/me
spring.security.oauth2.client.provider.naver.user_name_attribute=response
```
- `user_name_attribute=response`
    + 기준이 되는 user_name의 이름을 네이버에서는 response로 해야한다.
    + 이유는 네이버의 회원 조회 시 반환되는 JSON형태 때문이다.

### 스프링 시큐리티 설정 등록
1. `OAuthAttributes`에 다음과 같이 **네이버인지 판단하는 코드와 네이버 생성자**를 추가한다.
```java
@Getter
public class OAuthAttributes {
    ...
    public static OAuthAttributes of(String registrationId, String userNameAttributeName, Map<String, Object> attributes){
        if("naver".equals(registrationId)){
            return ofNaver("id",attributes);
        }
        return ofGoogle(userNameAttributeName,attributes);
    }

    public static OAuthAttributes ofNaver(String userNameAttributeName,Map<String,Object> attributes){
        Map<String,Object> response = (Map<String,Object>)attributes.get("response");
        return OAuthAttributes.builder()
                .name((String) response.get("name"))
                .email((String) response.get("email"))
                .picture((String) response.get("picture"))
                .attributes(response)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }
    ...
}
```
3. `index.mustache`에 네이버 로그인 버튼을 추가한다.
```html
{{>layout/header}}
<h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
<div class="col-md-12">
    <!-- 로그인 기능 영   -->
    <div class="row">
        <div class="col-md-6">
            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
            {{#userName}}
                Logged in as: <span id="user">{{userName}}</span>
                <a href="/logout" class="btn btn-info active" role="button">Logout</a>
            {{/userName}}
            {{^userName}}
                <a href="/oauth2/authorization/google" class="btn btn-success active" role="button">Google Login</a>
                <a href="/oauth2/authorization/naver" class="btn btn-secondary active" role="button">Naver Login</a>
            {{/userName}}
        </div>
    </div>
    <br>
    <!-- 목록 출력 영역 -->
    ...
```
- `/oauth2/authorization/naver`
    + 네이버 로그인 URL은 `application-oauth.properties`에 등록한 `redirect-uri`값에 맞춰 자동으로 등록된다.
    + `/oauth2/authorization/`까지는 고정이고 마지막 Path만 각 소셜 로그인 코드를 사용하면 된다.

## 5.7 기존 테스트에 시큐리티 적용하기
기존 테스트에 시큐리티 적용으로 문제가 되는 부분들을 해결해보자.   
기존에는 바로 API를 호출할 수 있어 테스트 코드 역시 바로 API를 호출하도록 구성하였다. 하지만, 시큐리티 옵션이 활성화되면 인증된 사용자만 API를 호출할 수 있으므로 기존의 API 테스트 코드들이 전부 인증에 대한 권한을 받지 못하였으므로, 테스트 코드마다 인증한 사용자가 호출한 것처럼 작동하도록 수정하자.

### 1. `CustomOAuth2UserService`를 찾을 수 없다.
`CustomOAuth2UserService`를 생성하는데 필요한 **소셜 로그인 관련 설정값들이 없기 때문에** 발생한다.   
> 분명 `application-oauth.properties`에 설정값들을 추가 했는데 왜 설정이 없다고 할까?   
> 이는 `src/main`과 `src/test`환경의 차이 때문이다.    
> `application.properties`가 적용되는 이유는?   
> test에 `application.properties`가 없으면 main의 설정을 그대로 가져오기 때문이다.

따라서 이 문제를 해결하기 위해서는, `application.properites`를 생성해서 해결할 수 있다.   
1. `src/test/resources`에 `application.properties`를 추가하고 코드를 작성한다.
```properties
spring.jpa.show-sql=true

spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.properties.hibernate.dialect.storage_engine=innodb
spring.datasource.hikari.jdbc-url=jdbc:h2:mem://localhost/~/testdb;MODE=MYSQL

spring.h2.console.enabled=true

spring.session.store-type=jdbc

# Test OAuth
spring.security.oauth2.client.registration.google.client-id=test
spring.security.oauth2.client.registration.google.client-secret=test
spring.security.oauth2.client.registration.google.scope=profile,email
```
### 2. 302 Status Code
응답의 결과로 200 Status Code를 원했는데 결과는 302(리다이렉션 응답) Status Code가 와서 실패하였다.   
이는 스프링 시큐리티 설정 때문에 **인증되지 않은 사용자의 요청은 이동**시키기 때문이다.   
그래서 이런 API 요청은 **임의로 인증된 사용자를 추가**하여 API만 테스트해 볼 수 있게 한다.
1. `spring-security-test`를 `build.gradle`에 추가한다.
```gradle
dependencies {
    ...
    testImplementation 'org.springframework.security:spring-security-test'
}
```
2. `PostApiControllerTest`의 2개 테스트 메서드에 다음과 같이 임의 사용자 인증을 추가한다.
```java
    @Test
    @WithMockUser(roles="USER")
    public void setPosts() throws Exception{
        ...
    }

    @Test
    @WithMockUser(roles="USER")
    public void UpdatePostsTest() throws Exception{
        ...
    }
```
- `@WithMockUser(roles="USER")`
    + 인증된 모의(가짜) 사용자를 만들어서 사용한다.
    + roles에 권한을 추가할 수 있다.
    + 즉, 이 어노테이션으로 인해 ROLE_USER 권한을 가진 사용자가 API를 요청하는 것과 동일한 효과를 가지게 된다.
> 이정도만 하면 테스트가 될 것 같지만, 실제로 작동하진 않는다.   
> **`@WithMockUser`가 MockMvc에서만 작동하기 때문**이다.   

3. 따라서 `@SpringBootTest`에서 MockMvc를 사용할 수 있도록 코드를 변경해야 한다.
```java
package org.example.web;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.example.domain.posts.Posts;
import org.example.domain.posts.PostsRepository;
import org.example.web.dto.PostsSaveRequestDto;
import org.example.web.dto.PostsUpdateRequestDto;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.*;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import static org.assertj.core.api.Assertions.assertThat;
import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.springSecurity;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import java.util.List;

@ExtendWith(SpringExtension.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {
    @LocalServerPort
    private int port;

    @Autowired
    private WebApplicationContext context;

    private MockMvc mvc;
    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private PostsRepository postsRepository;

    @AfterEach
    public void tearDown() throws Exception{
        postsRepository.deleteAll();
    }
    @BeforeEach
    public void setup(){
        mvc = MockMvcBuilders.webAppContextSetup(context).apply(springSecurity()).build();
    }
    @Test
    @WithMockUser(roles="USER")
    public void setPosts() throws Exception{
        //given
        String title = "title";
        String content = "content";
        PostsSaveRequestDto requestDto = PostsSaveRequestDto.builder()
                .title(title)
                .content(content)
                .author("author")
                .build();

        String url = "http://localhost:" + port + "/api/v1/posts";

        //when
        mvc.perform(post(url)
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .content(new ObjectMapper().writeValueAsString(requestDto)))
                .andExpect(status().isOk());

        //then
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(title);
        assertThat(all.get(0).getContent()).isEqualTo(content);
    }

    @Test
    @WithMockUser(roles="USER")
    public void UpdatePostsTest() throws Exception{
        //given
        Posts savedPosts = postsRepository.save(Posts.builder()
                .title("title")
                .content("content")
                .author("author")
                .build());

        Long updateId = savedPosts.getId();
        String expectedTitle = "title2";
        String expectedContent = "content2";

        PostsUpdateRequestDto requestDto = PostsUpdateRequestDto.builder()
                .title(expectedTitle)
                .content(expectedContent)
                .build();

        String url = "http://localhost:" + port + "/api/v1/posts/" + updateId;

        //when
        mvc.perform(put(url)
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .content(new ObjectMapper().writeValueAsString(requestDto)))
                .andExpect(status().isOk());

        //then
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
        assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
    }
}
```
- `@BeforeEach`
    + 매번 테스트가 시작되기 전에 MockMvc 인스턴스를 생성한다.
- `mvc.perform`
    + 생성된 MockMvc를 통해 API를 테스트한다.
    + 본문(Body) 문자열로 표현하기 위해 ObjectMapper를 통해 문자열 JSON으로 변환한다.

### 3. `@WebMvcTest`에서 `CustomOAuth2UserService`을 찾을 수 없음
HelloControllerTest는 `@WebMvcTest`를 사용한다.   
`@WebMvcTest`는 `CustomOAuth2UserService`를 스캔하지 않는다.   
`@WebMvcTest`는 WebSecurityConfigurerAdapter, WebMvcConfigurer를 비롯한 `@ControllerAdvice`, `@Controller`를 읽는다.   
**즉, `@Repository`, `@Service`, `@Component`는 스캔대상이 아니다.**   
그러니, SecurityConfig는 읽었지만, SecurityConfig를 생성하기 위해 필요한 CustomOAuth2UserService는 읽을수가 없어 앞에서와 같이 에러가 발생한다.  

1. 문제를 해결하기 위해 `HelloController`에 스캔대상에서 `SecurityConfig`를 제거한다.
```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = HelloController.class,
        excludeFilters = {
        @ComponentScan.Filter(type= FilterType.ASSIGNABLE_TYPE,classes = SecurityConfig.class)
        }
)
public class HelloControllerTest {
    ...
}
```
2. `@WithMockUser`를 사용해서 가짜로 인증된 사용자를 생성한다.
```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = HelloController.class,
        excludeFilters = {
        @ComponentScan.Filter(type= FilterType.ASSIGNABLE_TYPE,classes = SecurityConfig.class)
        }
)
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc;
    @WithMockUser(roles="USER")
    @Test
    public void helloTest() throws Exception{
        ...
    }
    
    @WithMockUser(roles="USER")
    @Test
    public void helloDtoTest() throws Exception{
        ...
    }
}
```
이렇게 한 뒤 다시 테스트를 돌려보면 추가적인 에러가 발생한다.   
해당 에러는 `@EnableJpaAuditing`로 인해 발생한다.   
`@EnableJpaAuditing`를 사용하기 위해선 **최소 하나의 `@Entity`클래스가 필요**하다.   
하지만 `@WebMvcTest`이다 보니 존재하지 않는다.   
`@EnableJpaAuditing`와 `@SpringBootApplication`이 함께 있다보니 `@WebMvcTest`에서도 스캔하게 되었다.   
이 둘을 분리하자.
3. `Application.java`에서 `@EnableJpaAuditing`을 제거한다.
```java
//@EnableJpaAuditing
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(org.example.Application.class,args);
    }
}
```
4. `config`패키지에 `JpaConfig`를 생성해서 `@EnableJpaAuditing`를 추가한다.
```java
package org.example.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@Configuration
@EnableJpaAuditing
public class JpaConfig {
}
```