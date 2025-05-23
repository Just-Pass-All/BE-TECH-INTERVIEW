# HTTP와 HTTPS의 차이는 무엇인가요?

## 면접용 답변

HTTP와 HTTPS는 웹에서 데이터를 주고받는 프로토콜로 둘의 가장 큰 차이는 보안입니다. <br>
HTTP는 데이터를 암호화하지 않고 전송하기 때문에 제3자가 데이터를 가로채고 읽을 수 있습니다. <br>
반면 HTTPS는 모든 요청 및 응답을 SSL및 TLS 프로토콜에 따라 암호화합니다. 
따라서 사용자의 개인정보나 로그인 정보처럼 민감한 데이터를 다룰 때 HTTPS를 사용하면 중간에서 도청이나 변조를 막을 수 있습니다.


<br>


## 개념 설명


### 1. HTTP (HyperText Transfer Protocol)

웹에서 클라이언트와 서버 간의 요청/응답 통신에 사용되는 프로토콜

특징
- 비연결성(Connectionless): 요청 후 연결이 끊김
- 무상태성(Stateless): 요청 간 상태를 유지하지 않음 
- 단방향성 : 클라이언트 -> 서버
- 보안 없음: 평문으로 데이터를 전송 ➡️ 도청, 위조, 변조 위험

<br>

### 2. HTTPS (Hypertext Transfer Protocol Secure)
HTTP에 SSL/TLS 보안 계층을 추가한 버전

- 암호화 방식
  - 대칭키 : 암호화와 복호화에 같은 키를 사용하는 방식
      - 장점) 속도 빠름
      - 단점) 키 전달이 보안상 취약 → 그래서 비대칭키로 보호함
  - 공개키 :  암호화와 복호화에 다른 키를 사용하는 방식 (공개키&개인키)
      - 장점) 키 공유 과정에서 안전
      - 단점) 속도 느림 → 본문 전체 암호화에 비효율적
  - 대칭키 + 공개키 : 공개키(서버의 인증서)로 대칭키를 암호화해서 전달한 후, 대칭키로 본문 통신


- HTTPS 통신 흐름
  1. 사용자가 HTTPS 웹 사이트 방문
  2. 웹 사이트 : 서버의 SSL 인증서 요청  
  3. 서버 : 공개 키가 포함된 SSL 인증서를 회신 
  4. 웹 사이트 : 서버 인증서 검증 브라우저가 퍼블릭 키로 비밀 세션 키가 포함된 메시지를 암호화하고 전송 
  5. 웹 사이트 : 세션 키를 공개키로 암호화해 전송
  6. 이후 대칭키 기반으로 본문 통신 시작


- SSL과 TLS
  - 두 개 모두 암호화된 데이터 전송을 위한 프로토콜
  - SSL은 현재 안쓰임 / TLS가 표준
  - 그러나 두개 모두 같은 의미로 통용됨

<br>

### 3. SSL 인증서

SSL 인증서: 서버의 공개키, 도메인 정보, 인증서 발급자, 유효기간 등이 담긴 디지털 문서

CA : 디지털 인증서를 발급해주는 기관. SSL/TLS 기반 통신을 하기 위해서는 CA로부터 인증서를 발급받아야 한다.

- SSL/TLS 인증서 발급 과정
  - 웹 사이트에서 공개키 - 개인키 쌍을 생성
  - 웹 사이트의 정보와 공개키를 바탕으로 CSR을 작성하여 CA에게 전송 
  - CA는 웹 사이트의 공개키를 해시 후 CA의 개인키로 암호화해서 디지털 서명 생성 
  - 웹 사이트 정보와 디지털 서명 정보를 포함하는 인증서 발급

  
- SSL/TLS 통신 과정
  - TCP 3 way handshaking 과정을 통해 연결을 확인 
  - 클라이언트 - 인증서 요청/ 서버 - 인증서 전송 
  - 인증서를 발급한 기관을 브라우저의 CA 목록에서 찾아 CA의 공개키를 가져옴 
  - CA 공개키로 서명을 복호화 
  - 서버의 공개키를 해시하여 서명을 복호화 한 값과 일치한지 확인 
  - 클라이언트는 대칭키를 서버와 교환






<br>

## 새끼 질문
- HTTPS 통신에서 사용되는 암호화 방식은 어떻게 구성되어 있나요? 
- SSL 인증서는 어떤 역할을 하며, 신뢰할 수 있는지 어떻게 판단하나요? 



---
## Reference
https://docs.tosspayments.com/resources/glossary/http-protocol
https://kdeon.tistory.com/132#대칭키%20방식(Symmetric%20Key%20Cryptosystem)-1