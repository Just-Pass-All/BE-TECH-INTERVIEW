# CORS란 무엇이며, 왜 필요하고 어떻게 동작하나요?
## 면접용 답변
CORS는 Cross-Origin Resource Sharing의 약자로, 브라우저에서 다른 출처의 리소스를 요청할 수 있도록 허용하는 보안 정책입니다. 
기본적으로 브라우저는 보안을 위해 동일 출처의 요청만 허용하는데, 다른 출처의 API 서버를 두거나, 다른 출처의 외부 리소스를 가져다 쓰는 경우를 위해 CORS 설정이 필요합니다.
서버는 Access-Control-Allow-Origin과 같은 헤더를 통해 허용할 출처를 명시하고, 브라우저는 이를 보고 요청을 허용하거나 차단합니다.

<br>

## 개념 설명
### CORS란?
> CORS (Cross-Origin Resource Sharing)

- 다른 출처(origin)의 리소스를 웹 페이지에서 요청할 수 있도록 허용하는 HTTP 기반 보안 정책
- 브라우저의 동일 출처 정책(Same-Origin Policy)에 의해 기본적으로 차단되는 cross-origin 요청을 예외적으로 허용한다.

<br>

### 왜 필요한가?
현대 웹 개발 과정에서 프론트엔드와 백엔드가 서로 다른 서버나 포트에서 실행되는 구조가 일반적이다. <br>
Ex. React(3000) ↔ Spring Boot(8080)

→ 이 경우, 브라우저가 cross-origin 요청을 차단하므로 명시적으로 이를 허용해줘야 한다.

<br>

### 어떻게 동작하는가?
#### 기본 흐름
- 브라우저가 cross-origin 요청을 보낸다.
- 서버가 응답 시에 CORS 관련 헤더(Access-Control-Allow-Origin)를 포함하면, 브라우저가 이를 확인하고 조건을 만족할 경우 요청을 허용한다.

#### 주요 CORS 응답 헤더
| 헤더                        | 설명                                                                 |
|-----------------------------|----------------------------------------------------------------------|
| `Access-Control-Allow-Origin`      | 허용할 Origin (예: `https://frontend.com`, 또는 모든 출처 허용 시 `*`)      |
| `Access-Control-Allow-Methods`     | 허용할 HTTP 메서드 목록 (예: `GET`, `POST`, `PUT`, `DELETE` 등)           |
| `Access-Control-Allow-Headers`     | 클라이언트에서 요청 시 사용할 수 있는 커스텀 헤더를 지정                          |
| `Access-Control-Allow-Credentials` | 쿠키와 같은 자격 정보를 포함한 요청 허용 여부 (`true` 설정 필요)                 |
| `Access-Control-Max-Age`           | Preflight 요청 결과를 브라우저에 캐싱할 시간(초 단위)                         |

<br>

#### Preflight 요청
> 브라우저가 실제 요청을 보내기 전에, 서버가 해당 요청을 허용하는지 먼저 확인하기 위해 보내는 OPTIONS 메서드 요청
- 브라우저는 다음 조건 중 하나라도 만족하면 preflight 요청을 먼저 보낸다.
1. 요청 메서드가 GET, POST, HEAD 이외일 때 (예: PUT, DELETE 등)
2. Content-Type이 다음 중 하나가 아닐 때
```
application/x-www-form-urlencoded
multipart/form-data
text/plain → Ex. application/json이면 preflight 발생
```
3. 요청에 커스텀 헤더가 포함될 때 (ex. Authorization, X-Custom-Header 등)

<br>

## 꼬리 질문

<br>

### 참조 
https://velog.io/@effirin/CORS%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
