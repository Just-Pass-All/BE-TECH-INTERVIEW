# RESTful API란?

## 면접용 답변
REST 아키텍쳐를 따르는 API를 RESTful API라고 합니다. <br>
Rest 설계 규칙은 URI는 정보의 자원만을 표현하고, 자원의 상태와 행위는 HTTP Method에 명시하는 것을 의미합니다.


## 개념 설명

### REST(Representational State Transfer)
웹의 설계 원칙 중 하나로, 자원을 URI로 표현하고 이를 HTTP 프로토콜을 통해 조작하는 아키텍처 스타일

<br>

#### REST 설계 원칙
1. 균일한 인터페이스
   <br>동일한 리소스에 대한 모든 API 요청은 요청의 출처에 관계없이 동일하게 표시되어야 함
   <br>REST API는 사용자의 이름 또는 이메일 주소와 같은 동일한 데이터가 하나의 통합 리소스 식별자(URI)에만 속하도록 해야 합니다.
   <br>리소스가 너무 커서는 안 되며 클라이언트가 필요로 할 수 있는 모든 정보를 포함해야 합니다.  


2. 클라이언트-서버 분리
   <br> 클라이언트 및 서버 애플리케이션은 서로 완전히 독립적이어야 함
   <br> 클라이언트와 서버는 서로 HTTP 요청을 통해서만 통신할 수 있음 


3. 무상태
   <br>  REST API는 무상태성이기 때문에 각 요청에는 처리에 필요한 모든 정보가 포함되어야 함
   <br> 즉, REST API에는 서버 측 세션이 필요하지 않음 ==  서버는  클라이언트 요청과 관련된 데이터를 저장할 수 없음


4. 캐시 가능성
   <br> 가능한 경우 클라이언트나 서버 측에서 리소스를 캐시할 수 있어야 함
   <br> 서버 응답에는 전달된 리소스에 대해 캐싱이 허용되는지 여부에 대한 정보도 포함되어야 함
   <br> 목표는 클라이언트 측의 성능을 향상시키는 동시에 서버 측의 확장성을 높이는 것!

  
5. 계층화된 시스템 아키텍처
   <br> REST API에서는 호출과 응답이 서로 다른 계층을 거침, 일반적으로 클라이언트와 서버 애플리케이션이 서로 직접 연결된다고 가정하지 않음
   <br> 즉, REST API는 클라이언트나 서버가 최종 애플리케이션과 통신하는지, 중개자와 통신하는지 알 수 없도록 설계

  
6. 코드 온디맨드(선택 사항)
   <br> REST API는 일반적으로 정적 리소스를 전송하지만 경우에 따라 응답에 실행 코드(예: Java 애플릿)가 포함될 수도 있음
   <br> 이러한 경우 코드는 온디맨드 방식으로만 실행


#### 장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없음
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능
- Hypermedia API의 기본을 충실히 지키며 범용성 보장 
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있음 
- 여러 가지 서비스 디자인에서 생길 수 있는 문제 최소화 
- 서버와 클라이언트의 역할을 명확하게 분리

#### 단점
- 표준이 없음 
- HTTP Method 형태 제한적  
- 구형 브라우저에서 호환 되지 않음(익스폴로어)
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구됨

<br>

### REST API

#### REST API 설계 규칙

1. 슬래시 구분자(/)는 계층 관계를 나타냄
   <br> ex) http://restapi.example.com/houses/apartments
   <br> URI 마지막 문자로 슬래시(/ )를 포함하지 않음

2. URI에 포함되는 모든 글자는 리소스의 유일한 식별자
   <br> URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 함


3. 하이픈(-)은 URI 가독성을 높이는데 사용
   <br> 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높임


4. 밑줄(_ )은 URI에 사용하지 않음


5. URI 경로에는 소문자 사용
   <br> RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문

6. 파일확장자는 URI에 포함하지 않음
   <br> REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않음

7. Accept header를 사용한다.
   <br>ex) http://restapi.example.com/members/soccer/345/photo.jpg (X)
   <br>ex) GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)

8. 리소스 간에는 연관 관계가 있는 경우
   <br> /리소스명/리소스ID/관계가 있는 다른 리소스명
   <br>ex) GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)


### RESTful API
RESTFUL이란 REST 아키텍쳐를 지키는 시스템!
<br> 하지만 REST를 사용했다고 모두가 RESTful하지 않음 
<br> REST API의 설계 규칙을 "올바르게" 지킨 시스템을 RESTful하다고 함

<br> 모든 CRUD 기능을 POST로 처리 하는 API나 URI 규칙을 올바르게 지키지 않은 API
<br> == REST API⭕️ RESTful API❌ 


<br>


## 새끼 질문
- REST란 무엇인가요?
- REST API와 RESTful API의 차이점은 무엇인가요?
- RESTful API의 장단점에 대해 설명해주세요.


---
## Reference
https://khj93.tistory.com/entry/네트워크-REST-API란-REST-RESTful이란
https://www.ibm.com/kr-ko/topics/rest-apis