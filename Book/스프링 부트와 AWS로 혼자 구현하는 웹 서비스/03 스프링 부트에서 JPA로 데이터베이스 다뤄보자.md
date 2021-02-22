# 03 스프링 부트에서 JPA로 데이터베이스 다뤄보자

## 3.1 JPA소개
Oracle, My SQL, MSSQL 등을 쓰지 않는 웹 애플리케이션은 거의 없다. 그러다 보니 **객체를 관계형 데이터베이스에서 관리**하는 것이 무엇보다 중요하다.   

### 문제점
#### 1. 관계형 데이터베이스가 계속해서 웹 서비스의 중심이 되면서 모든 코드는 SQL 중심이 되었다.   

즉, 현업 프로젝트 대부분이 애플리케이션 코드보다 SQL로 가득하게 된 것이다.
또한, 이 반복적인 SQL을 만들고 유지하기 위해서 단순 반복작업을 수백 번 해야한다.

#### 2. 패러다임 불일치 문제발생

관계형 데이터베이스는 **어떻게 데이터를 저장할지에 초점**이 맞춰진 기술이다.   
반대로 객체지향 프로그래밍 언어는 **메시지를 기반으로 기능과 속성을 한 곳에서 관리**하는 기술이다.   
즉, 관계형 데이터베이스와 객체지향 프로그래밍 언어의 패러다임이 서로 다른데, 객체를 데이터베이스에 저장하려고 하니 여러 문제가 발생하게 된 것이다.

> 이러한 문제점들로 인해서 웹 애플리케이션 개발은 점점 **데이터 베이스 모델링**에만 집중하게 된다.

JPA는 이런 문제점을 해결하기 위해 등장하게 되었다.
서로 지향하는 바가 다른 2개영역을 중간에서 패러다임 일치를 시켜주기 위한 기술이다.
- 개발자는 **객체지향적으로 프로그래밍**을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성해서 실행한다.
- 개발자는 항상 객체지향적으로 코드를 표현할 수 있으니 더는 **SQL에 종속적인 개발을 하지 않아도 되었다.**

### Spring Data JPA
JPA는 인터페이스로서 자바 표준명세서이다. 인터페이스인 JPA를 사용하기 위해서는 구현체가 필요하다. 대표적으로 Hibernate, Eclipse Link가 있다.

Spring에서는 구현체들을 좀 더 쉽게 사용하고자 추상화시킨 **Spring Data JPA**라는 모듈을 이용하여 JPA 기술을 다룬다.

> `JPA <- Hibernate <- Spring Data JPA`

#### 이렇게 한 단계 더 감싸놓은 Spring Data JPA가 등장한 이유는 무엇일까?
1. 구현체 교체의 용이성
   - Hibernate 외에 다른 구현체로 쉽게 교체하기 위함이다.
2. 저장소 교체의 용이성
   - 관계형 데이터베이스 외에 다른 저장소로 쉽게 교체하기 위함이다.
   - 만약 MongoDB로 교체가 필요하다면 개발자는 Spring Data JPA에서 **Spring Data MongoDB로 의존성만 교체**하면 된다. 

### 실무에서 JPA
JPA를 잘 쓰려면 **객체지향 프로그래밍과 관계형 데이터베이스**를 둘 다 이해해야한다.

#### 장점
1. CRUD쿼리를 직접 작성할 필요가 없다.
2. 부모-자식 관계표현, 1:N 관계 표현, 상태와 행위를 한 곳에서 관리하는 등 객체지향 프로그래밍을 쉽게 할 수 있다.

### 요구사항 분석
1. 게시판 기능
    - 게시글 조회
    - 게시글 등록
    - 게시글 수정
    - 게시글 삭제
2. 회원 기능
    - 구글 / 네이버 로그인
    - 로그인한 사용자 글 작성 권한
    - 본인 작성 글에 대한 권한 관리

## 3.2 프로젝트에 Spring Data Jpa 적용하기
1. `build.gradle`에 코드 추가하기
```gradle
dependencies {
    ...

    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    runtimeOnly 'com.h2database:h2'
}
```
- `spring-boot-starter-data-jpa`
    + 스프링 부트용 Spring Data Jpa 추상화 라이브러리이다.
    + 스프링 부트 버전에 맞춰 자동으로 JPA관련 라이브러리들의 버전을 관리해 준다.
- `h2`
    + 인메모리 관계형 데이터베이스이다.
    + 별도의 설치가 필요 없이 프로젝트 의존성만으로 관리할 수 있다.
    + 메모리에서 실행되기 때문에 애플리케이션을 재시작할 때마다 초기화된다는 점을 이용하여 테스트 용도로 많이 사용한다.
    + 이 책에서는 JPA의 테스트, 로컬 환경에서의 구동에서 사용할 예정이다.

2.  `org.example`디렉터리에 `domain`패키지를 추가한다.
- `domain`패키지는 도메인을 담을 패키지이다.
> 도메인이란?   
> 게시글, 댓글, 회원, 정산, 결제 등 소프트웨어에 대한 요구사항 혹은 문제 영역이라고 생각하면 된다.

3. `domain`패키지 안에 `posts`패키지를 만들고, `Posts`클래스를 만든다.
4. `Posts`에 코드를 작성한다.
```java
package org.example.web.domain.posts;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Getter
@NoArgsConstructor
@Entity
public class Posts {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT",nullable = false)
    private String content;

    private String author;

    @Builder
    public Posts(String title,String content,String author){
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```
> 필자는 어노테이션 순서를 **주요 어노테이션을 클래스에 가깝게**둔다.
> 위 코드에서는 롬복의 어노테이션은 코드를 단순화 시켜주지만 **필수 어노테이션은 아니다.**   
> 그러다보니 주요 어노테이션인 `@Entity`를 클래스에 가깝게 두고, 롬복 어노테이션을 위로 두게되었다.

- `@Entity`
    + 테이블과 링크될 클래스임을 나타낸다.
    + 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍으로 테이블 이름을 매칭한다.
    + > ex) SaleManager.java -> sales_manager table
- `@Id`
    + 해당 테이블의 PK 필드를 나타낸다.
- `@GeneratedValue`
    + PK의 생성 규칙을 나타낸다.
    + 스프링 부트 2.0 에서는 GenerationType.IDENTITY 옵션을 추가해야만 auto_increment가 된다.
- `@Column`
    + 테이블의 칼럼을 나타내며 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 칼럼이 된다.
    + 사용하는 이유는, 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용한다.
    + 문자열의 경우 VARCHAR(255)가 기본값인데, 사이즈를 500으로 늘리고 싶거나, 타입을 TEXT로 변경하고 싶거나 등의 경우에 사용된다.

> 왠만하면 Entity의 PK는 Long 타입의 Auto_increment를 추천함   
> 주민등로번호와 같이 비즈니스상 유니크 키나, 여러 키를 조합한 복합키로 PK를 잡을 경우 난감한 상황이 종종 발생한다.
> 1. FK를 맺을 때 다른 테이블에서 복합키 전부를 갖고 있거나, 중간 테이블을 하나 더 둬야 하는 상황이 발생한다.
> 2. 인덱스에 좋은 영향을 끼치지 못한다.
> 3. 유니크한 조건이 변경될 경우 PK 전체를 수정해야 하는 일이 발생한다.

*(서비스 초기 구축 단계에선 테이블 설계(Entity 설계)가 빈번하게 변경되는데, 이때 롬복의 어노테이션들은 코드 변경량을 최소화시켜 주기 떄문에 적극적으로 사용하자.)*
- `@NoArgsConstructor`
    + 기본 생성자 자동 추가
    + `public Posts(){}`와 같은 효과
- `@Getter`
    + 클래스 내 모든 필드의 Getter 메서드 자동생성
- `@Builder`
    + 해당 클래스의 빌더 패턴 클래스를 생성
    + 생성자 상단에 선언 시 생성자에 포함된 필드만 빌드에 포함

#### Posts 클래스의 특이점
바로 Setter 메서드가 없다는 것.   
자바빈 규약을 생각하면서 getter/setter를 무작정 생성한느 경우가 있다. 이렇게 되면 해당 클래스의 인스턴스 값들이 언제 어디서 변해야 하는지 코드상으로 명확하게 구분할 수가 없어, 차후 기능 변경시 정말 복잡해진다.

그래서 **Entity 클래스에서는 절대 Setter 메서드를 만들지 않는다.** 대신, 해당 필드의 값 변경이 필요하면 명확히 그 목적과 의도를 나타낼 수 있는 메서드를 추가해야만 한다.

- 잘못된 사용 예
```java
public class Order{
    public void setStatus(boolean status){
        this.status = status;
    }
}

public void 주문서비스의_취소이벤트(){
    order.setStatus(false);
}
```
- 올바른 사용 예
```java
public class Order{
    public void cancleOrder(){
        this.status = false;
    }
}

public void 주문서비스의_취소이벤트(){
    order.cancleOrder();
}
```  
**Setter가 없는 이 상황에서 어떻게 값을 채워 DB에 삽입**해야할까?   
- 기본적인 구조는 **생성자를 통해**최종값을 채운 후 DB에 삽입하는 것이다.
- 값 변경이 필요한 경우 **해당 이벤트에 맞는 public메서드를 호출**하여 변경한다.

*(이 책에서는 생성자 대신에 **`@Builder`를 통해 제공되는 빌더 클래스**를 사용한다.)*
- 빌더를 사용하게 되면 **어느 필드에 어떤 값을 채워야 할지** 명확하게 인지할 수 있다.

5. `Posts` 클래스로 Database를 접근하게 해줄 `JpaRepository`를 생성한다.
6. `PostsRepository`에 코드를 작성한다.
```java
package org.example.web.domain.posts;

import org.springframework.data.jpa.repository.JpaRepository;

public interface PostsRepository extends JpaRepository<Posts,Long> {

}
```
- 보통 ibatis나 MyBatis에서 **Dao**로 불리는 DBLayer이다.
- JPA에선 Repository라고 부르며 **인터페이스**로 생성한다.
- 단순히 인터페이스를 생성 후, `JpaRepository<Entity 클래스,PK 타입>`를 상속하면 기본적인 CRUD 메서드가 자동으로 생성된다.
- `@Repository`를 추가할 필요가 없다.
- 여기서 주의할 점은 **Entity 클래스와 기본 Entity Repository는 함께 위치**해야 한다.
- 둘은 매우 밀접한 관계이고, Entity 클래스는 **기본 Repository 없이는 제대로 역할을 할 수가 없다.**

## 3.3 Spring Data JPA 테스트 코드 작성하기
1. `test` 디렉토리에 `domain.posts` 패키지를 생성하고, 테스트 클래스는 `PostsRepositoryTest` 이름으로 생성한다.
2. `PostsRepositoryTest`에 코드 작성
```java
package org.example.web.dto;

import org.assertj.core.api.Assertions;
import org.example.web.domain.posts.Posts;
import org.example.web.domain.posts.PostsRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import java.util.List;

@ExtendWith(SpringExtension.class)
@SpringBootTest
public class PostsRepositoryTest {
    @Autowired
    PostsRepository postsRepository;

    @AfterEach
    public void cleanUp(){
        postsRepository.deleteAll();
    }

    @Test
    public void getPosts(){
        //given
        String title = "테스트 게시글";
        String content = "테스트 본문";

        postsRepository.save(Posts.builder().title(title).content(content).author("proto_seo").build());

        //when
        List<Posts> postsList = postsRepository.findAll();

        //then
        Posts posts = postsList.get(0);
        Assertions.assertThat(posts.getTitle()).isEqualTo(title);
        Assertions.assertThat(posts.getContent()).isEqualTo(content);

    }
}
```
- 별다른 설정 없이 `@SpringBootTest`를 사용할 경우 **H2 데이터베이스**를 자동으로 실행해 준다.
- `@AfterEach`
    + Juni에서 단위 테스트가 끝날 때마다 수행되는 메소드를 지정
    + 보통은 배포 전 전체 테스트를 수행할 때 테스트간 데이터 침범을 막기 위해 사용한다.
    + 여러 테스트가 동시에 수행되면 테스트용 데이터베이스인 H2에 데이터가 그대로 남아 있어 다음 테스트 실행 시 테스트가 실패할 수 있다.
- `postRepository.save`
    + 테이블 posts에 insert/update 쿼리를 싱행한다.
    + id 값이 있다면 update, 없다면 insert 쿼리가 실행된다.
- `postRepository.findAll`
    + 테이블 posts에 있는 모든 데이터를 조회해오는 메서드이다.

> **실제로 실행된 쿼리의 형태를 보고싶을 때**   
> `src/main/resources`디렉토리의 `application.properties`파일을 생성한다.   
> `spring.jpa.show-sql=true`옵션을 추가한다.

> **출력되는 쿼리 로그를 MySQL 버전으로 변경하고 싶을 때**
```properties
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.properties.hibernate.dialect.storage_engine=innodb
spring.datasource.hikari.jdbc-url=jdbc:h2:mem://localhost/~/testdb;MODE=MYSQL
```
추가

## 3.4 등록/수정/조회 API 만들기

API를 만들기 위해 총 3개의 클래스가 필요하다.
1. Request 데이터를 받을 Dto
2. API 요청을 받을 Controller
3. 트랙잭션, 도메인 기능 간의 순서를 보장하는 Service   
(**Service에서 비지니스 로직을 처리하지 않아도 된다. 트랙잭션, 도메인 간 순서 보장의 역할만 한다.**)

|**Spring의 웹 계층**|
|:--|
|**Web Layer**|
|**Service Layer**| 
|**Repository Layer**|
- **Web Layer**
    + 흔히 사용하는 컨트롤러(`@Controller`)와 JSP/Freemarker 등의 뷰 템플릿 영역이다.
    + 이외에도 필터(`@Filter`), 인터셉터, 컨트롤러 어드바이스(`@ControllerAdvice`)등 **외부 요청과 응답**에 대한 전반적인 영역을 이야기한다.
- **Service Layer**
    + `@Service`에 사용되는 서비스 영역이다.
    + 일반적으로 Controller와 Dao의 중간 영역에서 사용된다.
    + `@Transactinal`이 사용되어야 하는 영역이기도 하다.
- **Repository Layer**
    + **Database**와 같이 데이터 저장소에 접근하는 영역이다.
    + Dao(Data Access Object)영역으로 이해하면 쉽다.
- **Dtos**
    + Dto(Data Transfer Object)는 **계층 간에 데이터 교환을 위한 객체**를 이야기하며 Dtos는 이들의 영역을 이야기한다.
    + 예를 들어 뷰 템플릿 엔진에서 사용될 객체나 Repository Layer에서 결과로 넘겨준 객체 등이 이들을 이야기한다.
- **Domain Model**
    + 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화 시킨 것을 도메인 모델이라고 한다.
    + 이를테면 택시 앱이라고 하면 배차, 탑승, 요금 등이 모두 도메인이 될 수 있다.
    + `@Entity`를 사용해본 사람들이라면 `@Entity`가 사용된 영역 역시 도메인 모델이라고 이해해도 된다.
    + 다만, 무조건 데이터베이스의 테이블과 관계가 있어야만 하는것은 아니다.
        * VO처럼 값 객체들도 이 영역에 해당하기 때문이다.

이 다섯가지 레이어에서 비지니스 처리는 Domain에서 담당해야한다. 

> ### 기존에 서비스로 비지니스 처리하는 경우(트랜잭션 스크립트)
1. 슈도코드
```Java
@Transactional
public Order cancelOrder(int orderId){
    1) 데이터베이스로부터 주문정보(Orders), 결제정보(Billing),배송정보(Delivery) 조회
    2) 배송 취소를 해야하는지 확인
    3) if(배송 중이라면){
        배송 취소로 변경
    }
    4) 각 테이블에 취소 상태 Update
}
```

2. 실제 코드
```java
@Transactional
public Order cancelOrder(int orderId){
    //1)
    OrderDto order = orderDao.selectOrder(orderId);
    BillingDto billing = billingDao.selectBilling(orderId);
    DeliveryDto delivery = deliveryDao.selectDelivery(orderId);

    //2)
    String deliveryStatus = delivery.getStatus()

    //3)
    if("IN_PROGRESS".equals(deliveryStatus)){
        delivery.setStatus("CANCLE");
        deliveryDao.update(delivery);
    }

    //4)
    order.setStatus("CANCLE");
    orderDao.update(order);
    
    billing.setStatus("CANCLE");
    billingDao.update(billing);

    return order;
}
```

모든 로직이 **서비스 클래스 내부에서 처리된다.** 그러다보니 **서비스 계층이 무의미하며, 객체란 단순히 데이터 덩어리**역할만 하게 된다.

> ### 도메인 모델에서 비지니스 처리하는 경우
```java
@Transactional
public Order cancelOrder(int orderId){
    // 1)
    Orders order = ordersRepository.findById(orderId);
    Billing billing = billingRepository.findByOrderId(orderId);
    Delivery delivery = deliveryRepository.findByOrderId(orderId);

    // 2-3)
    delivery.canel();

    //4)
    order.cancel();
    billing.cancel();

    return order;
}
```
order, billing, delivery가 각자 본인의 취소 이벤트 처리를 하며, 서비스 메소드는 **트랜잭션과 도메인 간의 순서만 보장**해준다.
> 이 책에서는 계속 이렇게 **도메인 모델**을 다루고 코드를 작성한다.

---
1. `PostsApiController`를 `web`패키지에, `PostsSaveRequestDto`를 `web.dto`패키지에, `PostsService`를 `service.posts`패키지에 생성한다.
2. 코드작성   
#### `PostsApiController`
```java
package org.example.web;

import lombok.RequiredArgsConstructor;
import org.example.service.posts.PostsService;
import org.example.web.dto.PostsSaveRequestDto;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RequiredArgsConstructor
@RestController
public class PostApiController {
    private final PostsService postsService;

    @PostMapping("/api/v1/posts")
    public Long save(@RequestBody PostsSaveRequestDto requestDto){
        return postsService.save(requestDto);
    }
}
```
#### `PostsService`
```java
package org.example.service.posts;

import lombok.RequiredArgsConstructor;
import org.example.web.domain.posts.PostsRepository;
import org.example.web.dto.PostsSaveRequestDto;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@RequiredArgsConstructor
@Service
public class PostsService {
    private final PostsRepository postsRepository;

    @Transactional
    public Long save(PostsSaveRequestDto requestDto){
        return postsRepository.save(requestDto.toEntity()).getId();
    }
}
```
- Controller와 Service에서 `@Autowired`가 없는 것이 어색하게 느껴질 수도 있다.
- 스프링에선 Bean을 주입받는 방식들이 다음과 같다.
    + `@Autowired`
    + setter
    + 생성자
- 이 중에서 가장 권장하는 방식이 **생성자로 주입**받는 방식이다.   
*(`@Autowired`는 권장하지 않는다.)*
- 하지만 코드에서 생성자가 보이지 않는다.
- 바로 `@RequiredArgsConstructor`에서 해결해주기 때문이다.
    + **final이 선언된 모든 필드**를 인자값으로 하는 생성자를 롬복의 `@RequiredArgsConstructor`가 대신 생성해준 것이다.

#### `PostsSaveRequestDto`
```java
package org.example.web.dto;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import org.example.web.domain.posts.Posts;

@Getter
@NoArgsConstructor
public class PostsSaveRequestDto {
    private String title;
    private String content;
    private String author;

    @Builder
    public PostsSaveRequestDto(String title, String content,String author){
        this.title = title;
        this.content = content;
        this.author = author;
    }

    public Posts toEntity(){
        return Posts.builder()
                .title(title)
                .content(content)
                .author(author)
                .build();
    }
}
```
- 여기서 Entity 클래스와 거의 유사한 형태임에도 Dto클래스를 추가로 생성했다.
- 절대로 **Entity 클래스를 Request/Response 클래스로 사용해서는 안된다.**
- Entity 클래스는 **데이터베이스와 맞닿은 핵심 클래스**이다. 
- Entity 클래스를 기준으로 테이블이 생성되고, 스키마가 변경된다.
    + 화면 변경은 사소한 기능 변경인데, 이를 위해 테이블과 연결된 Entity 클래스를 변경하는 것은 너무 큰 변경이다.
    + 수많은 서비스 클래스나 비즈니스 로직들이 Entity 클래스를 기준으로 동작하는데, 이것이 변경되면 많은 영향을 끼치게 된다.
- Request/Response용 Dto는 View를 위한 클래스라 정말 자주 변경이 필요하다.

> View Layer와 DB Layer의 역할 분리를 철저하게 하는 게 좋다.   
> 실제로 Controller에서 **결과값으로 여러 테이블을 조인해서 줘야 할 경우**가 빈번하므로 Entity 클래스만으로 표현이 힘든 경우도 있다.   
> 꼭, Entity 클래스와 Controller에서 쓸 Dto는 분리해서 사용해야한다.

3. 테스트 패키지 중 `web`패키지에 `PostsApiControllerTest`를 생성하고, 코드를 작성한다.
```java
package org.example.web;

import org.assertj.core.api.Assertions;
import org.example.web.domain.posts.Posts;
import org.example.web.domain.posts.PostsRepository;
import org.example.web.dto.PostsSaveRequestDto;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import java.util.List;

@ExtendWith(SpringExtension.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {
    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private PostsRepository postsRepository;

    @AfterEach
    public void tearDown() throws Exception{
        postsRepository.deleteAll();
    }

    @Test
    public void setPosts() throws Exception{
        //given
        String title = "title";
        String content = "content";
        PostsSaveRequestDto requestDto = PostsSaveRequestDto.builder()
                .title(title)
                .content(content)
                .author("author")
                .build();

        String url = "http://localhost:"+port+"/api/v1/posts";

        //when
        ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url,requestDto,Long.class);

        //then
        Assertions.assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        Assertions.assertThat(responseEntity.getBody()).isGreaterThan(0L);
        List<Posts> all = postsRepository.findAll();
        Assertions.assertThat(all.get(0).getTitle()).isEqualTo(title);
        Assertions.assertThat(all.get(0).getContent()).isEqualTo(content);
    }
}
```
- Api Controller를 테스트 하는데 `@WebMvcTest`를 사용하지 않았다.
- `@WebMvcTest`의 경우 JPA기능이 작동하지 않기 떄문인데, Controller와 ControllerAdvice 등 외부 연동과 관련된 부분만 활성화 되니 지금 같이 JPA기능 까지 한번에 테스트할 때는 `@SpringBootTest`와 `TestRestTemplate`를 사용하면 된다.

4. 수정/조회기능을 추가한다.
#### `PostsApiController`
```java
@RequiredArgsConstructor
@RestController
public class PostApiController {
    ...

    @PutMapping("/api/v1/posts/{id}")
    public Long Update(@PathVariable Long id, @RequestBody PostsUpdateRequestDto requestDto){
        return postsService.update(id,requestDto);
    }

    @GetMapping("/api/v1/posts/{id}")
    public PostsResponseDto findById(@PathVariable Long id){
        return postsService.findById(id);
    }
}
```

#### `PostsResponseDto`
```java
package org.example.web.dto;

import lombok.Getter;
import org.example.web.domain.posts.Posts;

@Getter
public class PostsResponseDto {
    private Long id;
    private String title;
    private String content;
    private String author;

    public PostsResponseDto(Posts entity) {
        this.id = entity.getId();
        this.title =entity.getTitle();
        this.content = entity.getContent();
        this.author = entity.getAuthor();
    }
}
```
- `PostsResponseDto`는 **Entity의 필드 중 일부**만 사용하므로 생성자로 Entity를 받아 필드에 값을 넣는다.
- 굳이 모든 필드를 가진 생성자가 필요하진 않으므로 Dto는 Entity를 받아 처리한다.
#### `Posts`
```java
@Getter
@NoArgsConstructor
@Entity
public class Posts {
    ... 
    public void update(String title,String content){
        this.title = title;
        this.content = content;
    }
}
```

#### `PostsService`
```java
@RequiredArgsConstructor
@Service
public class PostsService {
    ...
    @Transactional
    public Long update(Long id, PostsUpdateRequestDto requestDto){
        Posts posts = postsRepository.findById(id)
                .orElseThrow(()-> new IllegalArgumentException("해당 게시글이 없습니다. id="+id));
        posts.update(requestDto.getTitle(), requestDto.getContent());

        return id;
    }

    public PostsResponseDto findById(Long id){
        Posts entity = postsRepository.findById(id)
                .orElseThrow(()->new IllegalArgumentException("해당 게시글이 없습니다. id="+id));
        return new PostsResponseDto(entity);
    }
}
```
- update 기능에서 데이트베이스에 **쿼리를 날리는 부분이 없다.**
- 이게 가능한 이유는 JPA의 **영속성 컨텍스트** 때문이다.
    + 영속성 컨테스트 : **엔티티를 영구 저장하는 환경**이다. 일종의 논리적 개념이라고 생각하면 되고, JPA의 핵심 내용은 **엔티티가 영속성 컨텍스트에 포함되어 있냐 아니냐**로 갈린다.
- JPA의 엔티티 매니저가 활성화된 상태로 **트랜잭션 안에서 데이터베이스에서 데이터를 가져오면** 이 데이터는 영속성 컨텍스트가 유지된 상태이다.
- 이 상태에서 해당 데이터 값을 변경하면 **트랙잭션이 끝나는 시점에 해당 테이블에 변경분을 반영**한다.
- 즉, Entity 객체의 값만 변경하면 별도로 **Update 쿼리를 날릴 필요가 없다**는 것이다.
    + 이 개념을 **더티 체킹**이라고 한다.

5. `PostApiControllerTest`에 테스트 코드 추가
```java
@ExtendWith(SpringExtension.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {
    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private PostsRepository postsRepository;

    @AfterEach
    public void tearDown() throws Exception{
        postsRepository.deleteAll();
    }

    ... 
    @Test
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

        String url = "http://localhost:"+port+"/api/v1/posts/"+updateId;

        HttpEntity<PostsUpdateRequestDto> requestEntity = new HttpEntity<>(requestDto);

        //when
        ResponseEntity<Long> responseEntity = restTemplate.exchange(url, HttpMethod.PUT,
                requestEntity,Long.class);

        //then
        Assertions.assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        Assertions.assertThat(responseEntity.getBody()).isGreaterThan(0L);
        List<Posts> all = postsRepository.findAll();
        Assertions.assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
        Assertions.assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
    }
}
```
> 로컬환경에서는 데이터베이스로 H2를 사용한다.   
> 메모리에서 실행하기 때문에 직접 접근하려면 웹 콘솔을 사용해야만 한다.   
> `application.properties`에 `spinrg.h2.console.enabled=true`을 추가해주면 접근 가능하다.   
> 이후에 application을 동작 시킨다음, [H2 Console](http://localhost:8080/h2-console)에서 `JDBC URL`에 `jdbc:h2:mem://localhost/~/testdb` 

## 3.5 Jpa Auditing으로 생성시감/수정시간 자동화하기
보통 엔티티에는 해당 데이터의 생성시간과 수정시간을 포함한다.
- 언제 만들어졌는지, 언제 수정되었는지 등은 차후 유지보수에 있어 굉장히 중요한 정보이기 때문이다.
- 따라서 매번 DB에 삽입하기 전, 갱신하기 전에 날짜 데이터를 등록/수정하는 코드가 여기저기 들어가게된다.

```java
// 생성일 추가 코드 예제
public void savePosts(){
    ...
    posts.setCreateDate(new LocalDate());
    postsRepository.save(posts);
    ...
}
```
### LocalDate 사용
1. `domain`패키지에 `BaseTimeEntity`클래스를 생성한다.
2. `BaseTimeEntity`클래스에 코드를 작성한다.
```java
package org.example.web.domain;

import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import javax.persistence.EntityListeners;
import javax.persistence.MappedSuperclass;
import java.time.LocalDateTime;

@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseTimeEntity {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```
- `@MappedSuperClass`
    + JPA Entity 클래스들이 BaseTimeEntity을 상속할 경우 필드들(createdDate, modifiedDate)도 칼럼으로 인식하도록 한다.
- `@EntityListeners(AuditingEntityListener.class)`
    + BaseTimeEntity클래스에 Auditing 기능을 포함시킵니다.
- `@CreatedDate`
    + Entity가 생성되어 저장될 때 시간이 자동 저장된다.
- `@LastModifiedDate`
    + 조회한 Entity의 값을 변경할 때 시간이 자동 저장된다.
3. Posts 클래스가 BaseTimeEntity를 상속받도록 변경한다.
```java
public class Posts extends BaseTimeEntity {

}
```
4. `Application`클래스에 JPA Auditing 어노테이션들을 모두 활성화 할 수있도록, 활성화 어노테이션 하나를 추가한다.
```java
package org.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@EnableJpaAuditing
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(org.example.Application.class,args);
    }
}
```
### JPA Auditing 테스트 코드 작성하기
1. `PostsRepositoryTest`클래스에 테스트 메서드를 하나 추가한다.
```java
@ExtendWith(SpringExtension.class)
@SpringBootTest
public class PostsRepositoryTest {
    @Autowired
    PostsRepository postsRepository;

    @AfterEach
    public void cleanUp(){
        postsRepository.deleteAll();
    }
    ...
    @Test
    public void setBaseTimeEntity(){
        // given
        LocalDateTime now = LocalDateTime.of(2019,6,4,0,0,0);
        postsRepository.save(Posts.builder()
                .title("title")
                .content("content")
                .author("author")
                .build());

        //when
        List<Posts> postsList = postsRepository.findAll();

        //then
        Posts posts = postsList.get(0);

        System.out.println(">>>>>>>> createDate="+posts.getCreatedDate()+"modifiedDate="+posts.getModifiedDate());
        Assertions.assertThat(posts.getCreatedDate()).isAfter(now);
        Assertions.assertThat(posts.getModifiedDate()).isAfter(now);
    }
}
```