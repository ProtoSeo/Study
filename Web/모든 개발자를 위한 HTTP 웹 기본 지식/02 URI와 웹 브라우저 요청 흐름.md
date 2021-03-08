# 2. URI와 웹 브라우저 요청 흐름
## 2.1 URI(Uniform Resource Identifier)
### URI? URL? URN?    
URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.(URI는 URL과 URN을 포함하고 있다.)

### URI 단어 뜻   
**Uniform** : 리소스 식별하는 통일된 방식   
**Resource** : 자원, URI로 식별할 수 있는 모든 것(제한 없음)   
**Identifier** : 다른 항목과 구분하는 데 필요한 정보   

#### URL : Uniform Resource Locator   
- Locator : 리소스가 있는 위치를 지정
#### URN : Uniform Resource Name   
- Name : 리소스에 이름을 부여

- 위치는 변할 수 있지만, 이름은 변하지 않는다.
- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음
- 앞으로 URI를 URL과 같은 의미로 이야기한다.

> ### URL분석    
> https://www.google.com/search?q=hello&hl=ko   

### URL전체 문법   
scheme://[userinfo@]host[:port][/path][?query][#fragment]

- 주로 프로토콜 사용
- 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
- http는 80포트, https는 443 포트를 주로 사용, 포트는 생략 가능
- https는 http에 강력한 보안이 추가되었다.
<hr>

- userinfo : URL에 사용자 정보를 포함해서 인증 
- host : 호스트명, 도메인명 또는 IP 주소를 직접 사용가능
- port : 포트, 접속포트, 일반적으로 생략
- path : 리소스경로, 계층적 구조
- query : key = value형태, ?로 시작, &로 추가가능
- fragment : html 내부 북마크 등에 사용, 서버에 전송하는 정보가 아님

## 2.2 웹 브라우저 요청 흐름
https://www.google.com/search?q=hello&hl=ko   
1. 웹브라우저에서 DNS 조회해서 IP를 알아낸 다음, Https포트로 전송한다.
2. HTTP 요청 메시지를 생성한다.
```
GET /search?q=hello&hl=ko HTTP/1.1
Host:www.google.com
```
3. SOCKET라이브러리를 통해 전달
   - TCP/IP 연결
   - 데이터 전달
4. TCP/IP 패킷 생성, HTTP 메시지 포함
5. 요청 패킷이 서버에 전달하면 패킷에 있는것을 벗기고, HTTP 메시지를 분석한다.
6. HTTP 응답 메시지를 작성한다.
```
HTTP/1.1 200 OK
Content-type: text/html;charset=UTF-8
Content-Length: ...
```
7. 이를 클라이언트한테 보낸다.
8. 내부의 HTTP 응답 메시지를 해석해서 웹 브라우저에서 HTML을 렌더링 한다.

