# network
## 1. IP(인터넷 프로토콜)
> Http 프로토콜을 이해하기 전 기본 배경지식으로 알고 있어야하는 항목
 * 클라이언트와 서버에 각각의 IP주소가 할당되어 있는 상황에서 클라와 서버 사이에는 인터넷 프로토콜이 있다.
 * 인터넷 프로토콜의 역할
   * 지정한 IP주소에 데이터 전달
   * 패킷이라는 통신 단위로 데이터 전달
   * 이 패킷에는 출발지IP, 목적지 IP가 있고 전달하려는 데이터가 담겨있다.
  ### IP 프로토콜의 한계
   * 비연결성 : 패킷이 받을 대상이 없거나, 서비스 불능 상태여도 패킷이 전송된다.
   * 비신뢰성 
     * 중간에 패킷이 사라진다면..?
     * 패킷이 순서대로 안오면..? => 여러 패킷을 보내야 하는 상황이면 하나씩 일정시간 끊어서 전송하게 된다. 그런데 패킷이 인터넷 프로토콜의 여러 노드를 타고 이동한다. 그렇게 되면 도착을 해도 순서가 달라질수 있는 변수가 있다. 
    * 프로그램 구분 : 같은IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이라면?
    => 한계가 명확하다
 ## TCP,UDP
  ### 인터넷 프로토콜 스택의 4계층
   * 어플리케이션 계층 - HTTP,FTP
   * 전송계층 - TCP,UDP
   * 인터넷 계층 - IP
   * 네트워크 인터페이스 계층
   * 만약 내가 채팅프로그램을 미국의 친구에게 메세지를 보내는경우
     1) 어플리케이션 계층의 채팅 프로그램이 Hello 메세지 작성
     2) 어플리케이션 계층에서 socket 라이브러리를 통해 OS계층에 메세지 전달
     3) OS의 TCP에서 메세지 정보를 포함해 TCP데이터를 생성
     4) IP 패킷을 생성해 패킷안에 TCP데이터를 포함시킨다.
     5) 네트워크 인터페이스에서 전송한다. 
  ### TCP 특징
   * TCP데이터 : 출발지 port , 목적지 port, 전송데이터,전송제어 ,순서,검증정보등이 포함되어있다.
   * 연결지향 - TCP 3 way handshake(가상연결)
     1) 먼저 클라이언트에서 서버가 연결할수 있는 상태인지 확인하는 SYN(접속요청)을 보내고
     2) 서버가 가능한 상태면 ACK(요청 수락)와 SYN을 함께 클라이언트에 전달한다.
     3) 클라이언트 역시 SYN(접속요청)을 받았으므로 ACK를 서버에 전송후 데이터를 전송한다
   * 데이터전달 보증
     * TCP 프로토콜에선 클라에서 데이터 전송시 서버로 부터 잘 받았다는 응답을 받게 되어 데이터가 유실 파악이 가능하다.
   * 순서 보장
     * 클라에서 패킷 1,2,3 순서로 보냈는데 서버에 도착이 1,3,2순서로 도착했다면 클라에 패킷 2번부터 다시 보내라는 응답을 전송해준다.
    * 위같은 기능이 가능한 것은 TCP 데이터 내에 전송순서, 전송제어,순서 등의 정보가 포함되어있기 때문이다.
   * 신뢰할수 있는 프로토콜, 현재는 대부분 TCP사용
  ### UDP 특징
   * 기능이 거의 없다.
   * 연결지향 x
   * 데이터 전달 보증 x
   * 순서보장 x 
   * 데이터 전달 및 순서가 보장되지 않지만 단순하고 빠르다.주로 전화에서 많이 쓰이는 방식이라고 한다.
## PORT
 > 만약 클라이언트 = IP하나에서 게임도 하고 있고 화상통화도 하고 있고 웹 브라우저 요청도 한다. 즉 3개의 서버와 통신을 하고 있는 상황이다. 그렇다면 응답 패킷이 3개씩 올텐데 이것을 어떻게 구분해야 하나?
 * TCP/IP패킷을 보면 출발지 포트, 목작디 포트 정보가 포함되어있다.
 => 결국 같은 IP내에서 프로세스를 구분할수 있는 기준이 PORT이고 이 port정보를 토대로 패킷의 종류를 구분해준다.
## DNS
 > ip는 다 기억하기 어렵고 변경될수도 있다.. 그래서 IP를 저장하는 전화번호부로 DNS(도메인 네임 시스템)을 통해 도메인 명을 IP주소로 변환한다. => ex) 200.2.4.1(임의) -> googel.com

## 웹 브라우저 요청 흐름
 1) https://www.google.com/search?q=hello&hl=ko 라는 url로 요청이 입력되면 웹브라우저는 먼저 www.google.com = DNS를 먼저 조회해 IP를 받고 Http 요청 메세지를 생성한다.
 '''
 GET/search?q=hello&hl=ko HTTP/1.1
 HOST:www.google.com   
 '''
 HTTP 요청메세지는 간단하게 이러한 형태로 생성된다.
 
 2) 위에서 공부했던 인터넷 프로토콜의 4계층의 내용을 토대로 보게되면 어플리케이션 단의 웹브라우저가 http메세지를 생성해 OS계층(=전송+인터넷계층 = TCP/IP 계층)에 socket라이브러리를 통해 전달한다
 3) OS계층에서 TCP/IP 패킷을 생성하는데 패킷의 전송데이터를 위의 http메세지로 전달하는 방식이다.
## HTTP
> http 메세지에 모든 것을 담아서 전송한다
* HTML,TEXT,이미지,음성,영상,파일
* JSON,XML
* 거의 모든 형태의 데이터를 전송 가능하다.
* 서버간 데이터 주고 받을 시에도 대부분 HTTP를 사용한다
* TCP : HTTP/1.1, HTTP/2
* UDP : HTTP/3
* 현재 HTTP/1.1 가 상용화 되어있다.
  ### HTTP 특징
   * 클라이언트 서버 구조
   * 무상태 프로토콜(스테이스리스), 비연결성
   * HTTP 메세지
   * 단순함, 확장가능
## 클라이언트 서버 구조
> HTTP는 클라이언트와 서버구조로 되어있다. 
 * Request, Response 구조
 * 클라이언트는 서버에 요청을 보내고 응답을 받을때 까지 대기한다.
 * 서버는 요청에 대한 결과를 만들어서 응답해준다.
 * 클라이언트와 서버를 분리한 이러한 형태.
 * 클라이언트는 UI와 사용성에 집중하고 비즈니스로직과 데이터는 모두 서버가 담당한다. 이렇게되면 클라와 서버가 각각의 기능에 집중하고 독립적으로 발전할 수 있다는점이 큰 장점이자 강점이다.
## 무상태 프로토콜 (Stateless)
 * 서버가 클라의 상태를 보존 하지 않는다.
 * 장점 : 서버 확장성 높음
 * 단점 클라이언트가 추가 데이터 전송
  ### Statful,Stateless의 차이
  * Statful : 중간에 서버가 바뀌면 안된다. 즉 항상 같은 서버가 유지되어야 한다. 해당 서버에 문제가 생기면 상태가 날아가 버린다.
  * Statless : 중간에 다른 서버로 바뀌어도 된다. 클라이언트의 요청을 처리하던 서버에 문제가 생기면 다른 서버에서 대신 응답이 가능하기 때문에 유연하다.
  * 갑자기 클라이언트의 요청이 증가해도 서버를 대거 투입가능
  * => 무상태는 응답 서버를 쉽게 바꿀수 있다. -> 서버 증설 용이
  
## 비연결성
> 클라이언트1이 요청을 보내면 서버는 응답을 해주고 연결을 유지한다고 가정할때 클라이언트 2,3도 요청을 보내면 똑같이 응답을 하고 연결을 유지하고 있을 것이다. 이렇게 되면 요청이 따로 안오더라도 연결이 지속되기 떄문에 서버의 자원이 낭비되버린다.
> 그럼 연결을 응답후 끊어버린다면 최소한의 자원으로 서버를 운영할 수 있게 된다
 * http는 기본이 연결을 유지하지 않는 모델
 * 일반적으로 초단위 이하의 빠른 속도로 응답
 * 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우작다.
 * 서버 자원을 매우 효울적으로 사용할 수 있다.
 ### 비연결성의 단점
  * TCP/IP연결을 새로 맺어야함 - 3 way handshake를 새로 해줘야 한다.
  * 웹 브라우저로 사이트를 요청하면 html뿐만 아니라 js,css, 추가 이미지등 수많은 자원이 함께 다운로드 된다.
  * 지금은 HTTP 지속연결로 문제 해결.
## HTTP 메서드   

* API URI 설계
> 현재 URI설계에 가장 중요한것은 **리소스 식별** 이다. 여기서 리소스란 회원을 등록하고 수정하고 조회하는것이 리소스가 아니다. **회원이라는 개념자체가 '리소스'** 이다. 결국 회원을 등록하고 수정하고 조회하는 것을 URI에서 배제하고 오직 리소스만 식별하는 것이 좋은 URI설계이다
  * 회원 목록 조회 : /members
  * 회원 조회 : /member/{id}
  * 회원 등록 : /member
  * 회원 수정 : /member/{id}
  * 회원 삭제 : /member/{id}
  * 근데 위의 URI는 다 같아서 URI만으로는 구별이 불가능하다. 여기서 행위를 구분하는것을 HTTP 메서드를 활용해 구별해준다.
  ### GET
  * 리소스 조회
  * 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터)를 통해서 전달
  * 한번에 쿼리파라미터이상의 데이터는 보내지 못함
  * 메시지 바디를 사용해서 데이터를 전달할 수 있지만 지원하지 않는곳이 많아 권장x
  ### POST 
  * 요청 데이터 처리
  * 메시지 바디를 통해 서버로 요청데이터 전달
  * 서버는 요청데이터를 처리
    * 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
  * 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
  * POST메서드의 쓰임은 사실 단순히 신규 리소스를 만들때만 사용하는 것이 아니다. POST요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해햐한다.
  * 결론적으로
     1) 새 리소스 생성(등록)
       * 서버가 아직 식별하지 않은 새 리소스 생성
     2) 요청 데이터 처리
       * 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는경우
       * ex) 주문에서 결제완료 -> 배달시작 -> 배달완료 처럼 값변경을 넘어 프로세스의 상태가 변경되는 경우
       * POST의 결과로 새로운 리소스가 생성되지 않을 수도 있다.
     3) 다른 메서드로 처리하기 애매한경우
       ex) JSON으로 조회 데이터를 넘겨야 하는데 GET메서드를 사용하기 어려운경우(메세지 바디에 넣고 싶은 경우나 쿼리스트링으로 넘기기엔 데이터가 많은 경우에도 POST사용)
  ### PUT
  * 리소스 대체
    * 리소스가 있으면 대체
    * 리소스가 없으면 생성
    * 쉽게말해 기존데이터에 새 데이터를 덮어버린다
    > 만약 서버의 100-member의 정보가 username:"young", age:20 인 상황에서 put으로 /members/100 {"age":50}의 데이터가 요청으로 오게되면 기존 username은 날아가고 age가 50으로 적용되버린다.
  * **클라이언트가 리소스를 식별**
    * POST와 비슷하다고 생각 할 수 있지만 POST는 리소스 위치를 모른다 ex) post/ members , put/ members/100
    * 위처럼 PUT은 리소스의 자세한 위치를 알고 URI를 지정해주는 차이가 있다
  ### PATCH 
  * 리소스 부분 변경
  > 만약 위의 put과 마찬가지로 서버의 100-member의 정보가 username:"young", age:20 인 상황에서 patch로 /members/100 {"age":50}의 데이터가 요청으로 오게되면 기존 username은 유지한채 age가 50으로 적용된다.
  ### DELETE
  * 리소스 제거
## HTTP메서드 속성
* 안전
  * 호출해도 리소스를 변경하지 않는 동작
  * 만약 계속호출해서 로그같은 부분떄문에 장애가 발생하면?
  * => 안전은 해당 '리소스' 자체만 고려한다. 그런부분은 범위 밖
  * 결국 계속호출해도 리소스에 변화가 없는경우를 말한다. GET이 대표적이고 대표적인 위의 메서드 5개중 GET만 유일하게 '안전'하다.
* 멱등
  * f(f(x)) = f(x)
  * 한번 호출하든 두번호출하든 100번 호출하든 '결과'가 똑같은 것을 의미한다.
  * 멱등 메서드
   * GET : 한번 조회하든 두번 조회하든 같은 결과가 조회된다
   * PUT : 결과를 대체한다 따라서 같은 요청을 여러번해도 최종 '결과'는 같다
   * DELETE : 결과를 삭제한다. 같은 요청을 여러번해도 최종 '결과'는 같다
   * POST : 멱등이 아니다. 두번 호출하면 같은 결제가 중복해서 발생할 수 있다.ex) 같은 결제, 로그인, 회원가입등
  * 활용
   *  자동 복구 메커니즘
   *  서버가 TIMEOUT등으로 정상응답을 못주었을떄 클라가 같은 요청을 다시해도 되는가?의 판단 근거.
* 캐시가능
  * 응답결과 리소스를 캐시해서 사용해도 되는가?
  * GET,HEAD,POST,PATCH 캐시가능
  * 실제는 GET,HEAD 정도만 캐시가능
   * POST, PATCH는 본문 내용까지 캐시키로 고려해야 하는데 구현이 쉽지않다.
## HTTP 상태코드
* 1xx (informational): 요청이 수신되어 처리중. 거의 사용하지 않으므로 생략
* 2xx (Successful) : 요청 정상 처리
* 3xx (Redirection) : 요청을 완료하려면 추가 행동이 필요
* 4xx (Client Error) : 클라이언트 오류. 잘못된 문법, 파라미터 등으로 서버가 요청을 수행할수 없음
* 5xx (Server Error) : 서버오류, 서버가 정상요청을 처리하지 못함.
* 만약 모르는 상태 코드가 나타난다면?
  * 클라가 인식할수 없는 상태코드를 서버가 반환한다면?
  * 클라는 상위 상태코드로 해석해서 처리한다
  * 미래에 새로운 상태코드가 추가 되어도 클라이언트를 변경하지 않아도 된다.
   ex) 299 ?? -> 2xx (Successful) , 451 ?? -> 4xx (Client Error), 599 ?? -> 5xx(Server Error)
 ### 2xx (Successful)
  * 200 ok : 요청 성공
  * 201 Created : 요청 성공해서 새로운 리소스가 생성됨.
  * 202 Accepted : 요청이 접수되었으나 처리가 완료되지 않았음
    * 배치 처리같은 곳에서 사용 ex) 요청접수후 1시간 뒤에 배치 프로세스가 요청을 처리 -> 많이는 사용x
  * 204 No Accepted : 서버가 요청을 성공적으로 수행했지만 응답 페이로드 본문에 보낼 데이터가 없음
 ### 3xx (Redirection)
   * 300 Multiple Choices
   * 301 Moved Permanently
   * 302 Found
   * 303 See Other
   * 304 Not Modified
   * 307 Temporary Redirect
   * 308 Permanent Redirect
  #### 리다이렉션의 이해
   > 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있다면 Location 위치로 자동 이동한다(리다이렉트)
   * ex) 예를들어 기존에 /event라는 URL로 이벤트 페이지를 운영하고 있었고 사용자는 해당 URL을 북마크 해두었는데 URL을 /new-event로 변경 했을 경우 사용자의 북마크는 잘못된 주소를 가르키고 있다. 이러한 상황에 /event로 요청이 올경우 서버는 응답헤더에 301과 location : /new-event를 전달해주면 자동으로 리다이렉션이되 정상페이지로 접근이 가능하다.
   * 종류
     * 영구 리다이렉션 : 특정 리소스의 URL가 영구정으로 이동
      * /members -> /users
      * /event -> /new-event
     * 일시 리다이렉션 - 일시적인 변경
      * 주문 완료후 주문 내역 화면으로 이동
      * PRG : Post/Redirect/Get
     * 특수 리다이렉션
      * 결과대신 캐시를 사용
   #### 영구 리다이렉션
   * 301,308
    * 리소스의 URI가 영구적으로 이동
    * 원래의 URL을 사용x, 검색엔진 등에서도 변경 인지
    * 301 Moved Permanently
     * 리다이렉트시 요청 메서드가 GET으로 변하고 본문이 제거될수 있다.
    * 308 Permanent Redirect
     * 301과 기능은 같다
     * 리다이렉트시 요청 메서드오 본문 유지(처음 POST를 보내면 리다이렉트도 POST)
   #### 일시적 리다이렉션
   * 302,307,303
   * 리소스의 URI가 일시적으로 변경
   * 따라서 검색 엔진등에서 URL을 변경하면 안됨.
   * 302 Found 
    * 리다이렉트시 요청 메서드가 GET으로 변하고 본문이 제거될수 있다.
   * 307 Temporary Redirect
    * 302와 기능은 같다
    * 리다이렉트시 요청메서드와 본문유지.
   * 303 see other
    * 302와 기능동일 
    * 리다이렉트시 요청메서드가 무조건 GET으로 변경
   #### PRG : Post/Redirect/Get
   * Post로 주문후 웹브라우저를 새로고침하면?
   * 새로고침은 다시 요청
   * 중복 주문이 될 수 있다.
   * 그래서 처음 POST로 주문(19번째)이 들어가면 응답으로 302와 주문 결과 URI(/order-result/19)를 Location으로 응답해 리다이렉트 시켜 GET으로 재요청(GET /order-result/19)하면 Get으로 리다이렉트 되었기 때문에 새로고침에 대한 이슈가 x
 ### 4xx (Client Error)
 * 클라이언트의 요청에 잘못된 문법,파라미터 등으로 서버가 요청을 수행할 수 없다.
 * 오류의 원인이 클라이언트에 있다.
 * 클라이언트가 이미 잘못된 요청,데이터를 보내고 있기 떄문에 계속 시도해도 똑같은 재시도가 실패한다.
 #### 400 Bad Request
 > 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
 * 요청구문,메세지 등의 오류
 * 클라이언트는 요청내용을 다시 검토하고 요청해야한다.
 * 파라미터, api 스펙이 맞지 않는 경우
 #### 401 Unauthorized
 > 클라이언트가 해당 리소스에 대한 인증이 필요함.
 * 인증이 되지 않음
 * 401오류 발생시 응답에 www-Authenticate 헤더와 함께 인증 방법을 설명
  * 인증(Authentication) : 본인이 누구인지 확인(로그인)
  * 인가(Authorization) : 권한부여 (ADMIN 권한 처럼 특정 리소스에 접근할수 있는 권한을 이야기한다. 인증이 있어야 인가가 있다)
 #### 403 Forbidden
 > 서버가 요청을 이해했지만 승인을 거부함.
 * 주로 인증 자격 증명은 있지만 접근 권한이 불충분한 경우
 * ex) 어드민 등급이 아닌 사용자가 로그인은 했지만 어드민 등급의 리소스에 접근하는 경우
 #### 404 Not Found
 > 요청 리소스를 찾을 수 없음
 * 요청 리소스가 서버에 없음
 * 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을떄 
### 5xx (Server Error)
* 서버 문제로 오류발생(대표적으로 널포인터...)
* 서버에 문제가 있기에 재시도하면 성공할 수도 있음
 #### 500 (Internal Server Error)
 * 서버 내부 문제로 오류 발생
 * 작업 시 표현하기 애매한 부분들을 500으로 예외처리 해주는게 편하다
 #### 503 (Servive Unavailable)
 > 서비스 이용불가
 * 서버가 일시적 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
 * Retry-After헤더 필드로 얼마뒤에 복구되는지 보낼수도 있다

## HTTP 헤더 - 일반헤더
 ### HTTP헤더
 * header-field = field-name ":" OWS fiek-value OWS (OWS:띄어쓰기 허용)
 * field-name은 대소문자 구문 없음
 ```
 HTTP/1.1 200 OK
 Content-Type:text/html;charset=UTF-8
 Content-Length: 3423
 Content-Type,Content-Length : header-field
 text/html;charset=UTF-8,3423 : header-value
 
 바디....
 ```
 * HTTP 전송에 필요한 모든 부가정보
 * 예) 메시지 바디의 내용, 메시지 바디의 크기,압축,인증,요청클라이언트,서버 정보,캐시관리정보
 * 표준 헤더가 너무 많음
 * 필요시 임의의 헤더추가 가능
 ### HTTP BODY
 * 메시지 본문을 통해 표현 데이터 전달
 * 메시지 본문 = 페이로드
 * 표현은 요청이나 응답에서 전달할 실제 데이터
 * 표현헤더는 표현 데이터를 해석할 수 있는 정보 제공
   * 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
 * 참고 : 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야하지만 일단 생략
 ### 표현
 > 어떤 리소스를 html이라는 '표현'으로 전달할건지, json이라는 데이터 형태의 표현으로 전달할것인지? 느낌 이해했다. 
 * Content-Type : 표현 데이터의 형식
   * 미디어 타입, 문자인코딩
   * http 바디의 내용의 타입에 대한 정보와 문자 인코딩 정보를 말한다.
   * 예) text/html; charset=UTF-8,  application/json, image/png
 * Content-Encoding : 표현 데이터의 압축방식
   * 표현데이터를 압축하기 위해 주로 사용
   * 데이터를 전달하는 곳에서 압축후 인코딩헤더 추가
   * 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
   * 예) gzip, deflate, identity 등
 * Content-Language : 표현 데이터의 자연 언어
   * 표현 데이터의 자연언어를 표현. 즉 바디에 어떤 언어의 내용이 들어가 있는지 설명해준다
   * 예) Content-Language : ko,en ,en-US 
 * Content-Length : 표현 데이터의 길이
   * 바이드 단위
   * Transfer-Encoding을 사용하면 Content-Length를 사용하면 안된다.
  ### 협상(콘텐츠 네고시에이션)
  > 클라이언트가 선호하는 표현요청
  * Accept : 클라이언트가 선호하는 미디어 타입 전달. 만약 클라이언트가 json보다 xml을 주로 다룬다면 xml에 대한 우선순위가 높다고 서버에게 보내어 응답받을 데이터 타입을 협상하는것.
  * Accept-Charset : 클라이언트가 선호하는 문자 인코딩
  * Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
  * Accept-Language : 클라이언트가 선호하는 자연언어. 역시 요청헤더에 'Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7' 이런식으로 언어에 대한 우선순위를 보낸다. 물론 서버에서 ko를 제공하지 않으면 어쩔수 없지만 최대한 원하는 것을 전달하는 방식이다(q 값은 0~1사이 높을수록 우선된다.)
  * 협상헤더는 요청시에만 사용한다.
  ### 전송방식
  * 단순 전송 : 단순 전송시 사용
  * 압축 전송 : 압축된 데이터를 주고받을때 사용 헤더에 Content-Encoding이 있어야 한다.
  * 분할 전송 : 헤더에 Transfer-Encoding : chunked를 담아줘야 한다. 응답데이터가 용량이 클경우 응답시간이 지연되기에 서버가 정보를 나눠서 응답한다고 이해하면 될것 같다.
  * 범위 전송 : 응답데이터를 받다가 중간에 끊키고 다시 요청하는 경우 기존에 받았던 데이터용량이 아깝기에 끊킨 상태를 기억했다 남은 부분을 응답해달라고 요청하는 방식이라고 한다.
  ### 일반정보
  * From : 유저 에이전트의 이메일 정보. 일반적으로 잘 안쓰이고 검색엔진 같은 파트의 요청으로 주로 사용
  * Refer : 현재 요청된 페이지의 이전 웹 페이지 주소 정보. B -> A 이동시 B 요청에 Refer:A를 포함해 요청. Referer을 사용해 유입경로 분석가능
  * User-Agent : 클라이언트의 애플리케이션 정보(웹브라우저 정보등등), 통계정보, 어떤 종류의 브라우저에서 장애가 발생하는지 파악가능
  * Server : 요청을 처리하는 오리진 서버의 소프트웨어 정보
  * Date : 메시지가 생성된 날짜
  ### 특별한 정보
  * Host :  요청한 호스트 정보(도메인)
   * 요청에서 필수로 사용
   * 하나의 서버가 여러 도메인을 처리해야 할때
   * 하나의 ip주소에 여러 도메인이 적용되어 있을때  
   * 하나의 서버에 여러 호스트가 있을경우 요청헤더에 host가 없다면 서버내에 어떤 호스트에 접근해야 할지 모르기 때문에 host가 필수라고한다. 조금더 공부해야할듯
  * Location : 페이지 리다이렉션
   * 웹브라우저는 3xx응답 결과에 Location헤더가 있으면 Location 위치로 자동 이동
   * 응답코드 3xx에서 설명  
   * 201 (Created)에서도 Location헤더를 사용할수 있는데 201에서의 Location값은 요청에 의해 새로 생성된 리소스 URI를 의미할때 사용한다
   * 3xx : Location 값은 요청을 자도응로 리다이렉션하기 위한 대상 리소스를 가리킨다.
  * Allow : 허용가능한 HTTP 메서드
   * 405에서 응답에 포함해야함
   * 많이 쓰이진 않아서 있다는것 정도 알아둘것
  * Retry-After : 유저 에이저느가 다음 요청을 하기까지 기다려야 하는 시간
   * 503 : 서비스가 언제까지 불능인 알려줄수 있다
   * 실제로 사용하기 쉽지않다고 한다 
  ### 인증
  * Authorization : 클라이언트 인증 정보를 서버에 전달
   * JWT토큰 사용시 헤더에 토큰을 담아서 사용 했었다.
  * WWW-Authenticate : 리소스 접근시 필요한 인증 방법 정의
   * 접근시 인증에 문제가 있는경우
   * 401 Unauthorized 응답과 함께 사용
   * 401 오류가 날때 응답 헤더에 WWW-Authenticate 를 넣어줘야한다. 
  ### 쿠키
  * Set-Cookie : 서버에서 클라로 쿠키 전달(응답)
  * Cookie : 클라가 서버에서 받은 쿠키를 저장하고 HTTP 요청시 서버로 전달.
  * 사용처
   * 사용자 로그인 세션 관리
   * 요청시마다 요청에 포함되 전송됨
   * 광고 정보 트래킹
  * 쿠키 정보는 항상 요청에 포함되 서버에 전송됨
   * 네트워크 트래픽 추가 유발
   * 최소한의 정보만 사용(세션id, 인증 토큰)
   * 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶다면 웹 스토리지 차목
  * 보안에 민감한 데이터는 저장하면 x 
   #### 쿠키에 포함되는 정보들
   1) 쿠키-생명주기
    * set-Cookie : expires = Sat, 26-Dec-2020 
     * 만료일이 되면 쿠키 삭제
    * Set-Cookie : max-age=3600 (3600초)
     * 0이나 음수를 지정하면 쿠키 삭제
    * 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
    * 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지
   2) 쿠키-도메인 : 아무사이트나 쿠키가 요청에 포함되면 안됨.
    * 도메인을 쿠키에 명시하면 지정됨. domain=example.org를 명시해서 쿠키 생성시 명시한 문서기준 도메인 +서브도메인을 포함한 곳에서만 요청에 포함됨
     * example.org는 물론 dev.example.org도 가능
   3) 쿠키 - 경로 
    * ex) path=/home
    * 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
    * 일반적으로 path=/ 루트로 지정
    * 예) path=/home 지정했을경우
     * /home -> 가능
     * /home/level1 -> 가능
     * /home/level2 -> 가능 
     * /hello -> 불가능
   4) 쿠키-보안
    * secure
     * 쿠키는 원래 http, https를 구분하지 않고 전송
     * Secure를 적용하면 https인경우에만 전송
    * htpOnly 
     * XSS 공격 방식용도
     * htpOnly를 적용하면 js에서 쿠키 접근불가능해짐
     * HTTP 전송에만 사용 
    * sameSite
     * XSRF 공격 방지
     * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송  
 ## HTTP 헤더 - 캐시와 조건부 요청
 > 브라우저에서 star.jpg라는 사진파일을 다운받는다고 할때 헤더에 0.1m , 바디에 1.0m으로 총 1.1m이 전송된다고 가정한다.
  ### 캐시 기본 동작
  * 캐시가 없을때 
   * 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다
   * 인터넷 네트워크는 매우 느리고 비싸다
   * 브라우저 로딩 속도가 느리다.
   * 느린 사용자 경험
  * 캐시 적용
   > 첫번째 요청시 star.jpg를 받을때 서버에서 응답 헤더에 cache-control : max-age=60 이렇게 적용시 브라우저가 해당 내용을 캐시 저장소에 60초동안 저장해서 60초사이에 같은 요청이 오면 저장소를 먼저 뒤져 네트워크를 타지 않게하는 방식으로 동작한다
   * 캐시 덕분에 캐시가능 시간동안 네트워크를 사용하지 않아도 된다.
   * 비싼 네트워크 사용량을 줄일수 있다
   * 브라우저 로딩 속도가 매우 빠르다
   * 빠른 사용자 경험 
  * 캐시 시간 초과 
   > 만약 60초가 지나면 다시 서버에서 응답을 내려줘서 다시 저장소에 캐시를 저장한다.
   * 캐시 유효시간이 초과하면 서버를통해 데이터를 다시 조회하고, 캐시를 갱신단다
   * 이때 다시 네트워크 다운로드가 발생한다..
   * 근데 이때 받는 star.jpg와 전에 받아 캐시로 저장되었던 star.jpg는 바뀌지 않았다. 그럼 같은 내용을 다운 받는 상황이 비효율적이지 않나..?
   * 이렇게 되면 두가지 상황을 고려해야함
    1) 서버에서 기존 데이터를 변경
    2) 서버에서 기존 데이터를 변경하지 않음
  ### 검증헤더와 조건부 요청
  > 위의이야기 처럼 같은 내용을 다시 받기 위해 네트워크를 타서 데이터를 받는게 비효율적이라 서버의 내용이 캐시의 내용과 같다면 네트워크를 타고 싶지 않은 경우는 서버내용과 캐시내용이 같은걸 검증해야 한다. 그래서 검증헤더라는 것을 적용한다
  * 검증 헤더 추가
   1) 첫번째 요청: start.jpg 요청후 1.1m의 데이터를 전송받아 캐시저장소에 저장함.그런데 이때 헤더에는 Last-Modified 라는 최종 변경날짜 값을 가진 헤더를 포함해 요청하고 캐시가 저장될떄 같이 저장됨
   2) 두번쨰 요청: 요청시 캐시저장소를 뒤져 요청과 같은 값이 있는데 캐시 기간이 만료되었다면 요청헤더의 if-modified-since에 최종수정일을 담아서 서버에 보내준다. 요청받은 수정일과 서버의 수정일이 같다면 304 Not Modified 응답을 하고 똑같이 cache-control, last-modified를 담아서 보내주는데 이떄에 응답 바디가 없이 헤더만 보내기 때문에 데이터 전송을 큰폭으로 줄일수 있다.
   * 여기서 응답의 last-modified가 '검증헤더', 요청의 if-modified-since가 '조건부 요청' 이다
  * 정리
   * 캐시 유효시간이 초고해도 서버의 데이터가 갱신되지 않으면 304 Not Modified + 헤더 메타정보만 응답하고 이떄 응답 바디는 없어야 한다.
   * 클라는 서버가 보낸 응답헤더 정보로 캐시의 메타 정보를 갱신
   * 클라는 캐시에 저장되어있는 데이터 재활용
   * 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드
   * 매우 실용적인 해결책이다
 ### 검증헤더와 조건부 요청2
  * 검증헤더
   * 캐시 데이터와 서버데이터가 같은지 검증하는 데이터
   * Last-Modifyed, ETag
  * 조건부 요청 헤더
   * 검증헤더로 조건에 다른 분기
   * if-Modified-Since : Last-Modified 사용
   * If-None-Match: ETag 사용
   * 조건이 만족하면 200 OK
   * 조건이 만족하지 않으면 304 Not Modifie
   * If-Modified-Since: 이후에 데이터가 수정되었으면?
    * 데이터 미변경 예시
     * 캐시: 2020년 11월 10일 10:00:00 vs 서버: 2020년 11월 10일 10:00:00
     * 304 Not Modified, 헤더 데이터만 전송(BODY 미포함)
     * 전송 용량 0.1M (헤더 0.1M, 바디 1.0M)
    * 데이터 변경 예시
     * 캐시: 2020년 11월 10일 10:00:00 vs 서버: 2020년 11월 10일 11:00:00
     * 200 OK, 모든 데이터 전송(BODY 포함)
     * 전송 용량 1.1M (헤더 0.1M, 바디 1.0M
   * Last-Modified, If-Modified-Since 단점
    * 1초 미만(0.x초) 단위로 캐시 조정이 불가능
    * 날짜 기반의 로직 사용
    * 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우. 똑같은 내용 복붙이나 간단한 주석이 추가된건 변경되었다고 볼수 없지만 최종변경 날짜는 변경됨.
    * 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
     * 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우 
   * ETag, If-None-Match
    * ETag(Entity Tag)
     * 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
      * 예) ETag: "v1.0", ETag: "a2jiodwjekjl3"
     * 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
      * 예) ETag: "aaaaa" -> ETag: "bbbbb"
     * 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기

   ### 캐시와 조건부요청 관련 헤더
   #### 캐시제어헤더
   * cache-control : 캐시 제어
    * max-age : 캐시 유효 시간, 초단위
    * no-cache : 데이터는 캐시해도 되지만, 항상 서버에 검증하고 사용.즉 캐시에 두긴하지만 사용시 항상 서버에 접근해 검증후 사용해야한다는것.
    * no-store : 데이텅 민감함 정보가 있으므로 저장하면 안된다는 의미(메모리에서 사용하고 최대한 빨리 삭제)
   * Pragma : 캐시 제어 (하위 호환)
    * 거의 사용안한다고함 
   * Expires : 캐시 유효기간 (하위 호환)
    * 캐시 지시어, 캐시만료일 지정
    * 다만 날짜로만 지정하고 초단위로는 할수 없어 max-age가 훨씬 유연함.
    * cache-control : max-age가 함께 사용되면 Expires는 무시됨
  
  ### 프록시 캐시 (CDN 서비스)
  > 원(origin) 서버. 만약 미국에 원 서버가 있는경우 원서버에 데이터를 요청하면 0.5초가 걸리게 된다. 이렇게 되면 클라들은 모두 0.5초씩 기다려야하는데 중간(한국 어딘가..)에 프록시 캐시서버를 두고 DNS서버 요청이오면 프록시 서버를 거쳐서 오게한다. 이렇게 하면 속도가 훨씬 향상된다.
  > 예를들어 유튜브를 봐도 한국에서 많이보는 컨탠츠 들은 로딩속도가 빠른데 생영어로된 외국강의 영상을 보게되면 로딩속도가 느리다 
* 프록시 캐시 서버를 'public 캐시', 프록시 캐시 서버에서 받은 캐시를 'private 캐시'라고 한다
* 캐시 지시어(directives) - 기타
 * Cache-Control: public 
  * 응답이 public 캐시에 저장되어도 됨
 * Cache-Control: private 
  * 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)
 * Cache-Control: s-maxage 
  * 프록시 캐시에만 적용되는 max-age
 * Age: 60 (HTTP 헤더)
  * 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)
  ### 캐시 무효화
  > 캐시를 서버에서 주는것이 아니면 적용이 안되는것이 아닌 브라우저 자체에서도 사용자 편의를 위해 캐시를 만든다고 한다. 그래서 해당페이지에 절대 캐시가 적용되면 안되는 경우 밑의 조건들을 적용시켜야 한다고 한다.
  * Cache-Control : no-cache, no-store, must-revalidate
   *  Cache-Control: no-cache   
    * 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용(이름에 주의!)
   * Cache-Control: no-store 
    * 데이터에 민감한 정보가 있으므로 저장하면 안됨 (메모리에서 사용하고 최대한 빨리 삭제)
   * Cache-Control: must-revalidate 
    * 캐시 만료후 최초 조회시 원 서버에 검증해야함
    * 원 서버 접근 실패시 반드시 오류가 발생해야함 - 504(Gateway Timeout)
    * must-revalidate는 캐시 유효 시간이라면 캐시를 사용함
  * Pragma : no-cache
   * HTTP 1.0하위 호환 
