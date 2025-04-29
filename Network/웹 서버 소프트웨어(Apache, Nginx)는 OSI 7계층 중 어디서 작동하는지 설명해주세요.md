# 웹 서버 소프트웨어(Apache, Nginx)는 OSI 7계층 중 어디서 작동하는지 설명해주세요.

## 면접용 답변
Apache와 NGINX는 HTTP 웹 서버로, 이들이 동작하는 HTTP 프로토콜은 OSI 7 Layer 중 7계층인 Application Layer 에 해당하는 프로토콜입니다. 
HTTP 프로토콜은 TCP/IP 프로토콜을 통해 동작합니다. TCP/IP 프로토콜은 OSI 7 Layer 중 4계층인 Transport Layer에서 동작합니다. 
따라서 웹 서버 소프트웨어는 4계층의 TCP/IP 프로토콜과 7계층의 HTTP 프로토콜을 활용하여 동작합니다.

## 개념 설명

### OSI 7계층 (Open Systems Interconnection Model)


| 계층 | 이름 | 역할                                        | 주요 프로토콜 | 주요 장비 |
|------|------|-------------------------------------------|--------------|----------|
| 7 | **응용 계층 (Application Layer)** | 사용자가 네트워크 서비스를 이용할 수 있도록 제공               | HTTP, FTP, SMTP, DNS | - |
| 6 | **표현 계층 (Presentation Layer)** | 데이터 형식 변환, 인코딩/디코딩<br>암호화/복호화             | JPEG, GIF, ASCII, SSL/TLS | - |
| 5 | **세션 계층 (Session Layer)** | 세션 설정, 유지<br>세션 종료 관리                     | NetBIOS, RPC | - |
| 4 | **전송 계층 (Transport Layer)** | 송·수신 간 신뢰성 있는 데이터 전송<br>패킷의 흐름 제어 및 오류 복구 | TCP, UDP | - |
| 3 | **네트워크 계층 (Network Layer)** | IP 주소를 이용한 패킷 라우팅<br>최적의 경로를 선택하여 전달      | IP(IPv4, IPv6), ICMP, ARP | 라우터 |
| 2 | **데이터 링크 계층 (Data Link Layer)** | MAC 주소를 이용한 데이터 전송<br>오류 검출 및 수정          | 이더넷, PPP | 스위치 |
| 1 | **물리 계층 (Physical Layer)** | 통신 케이블로 데이터 전송(전기 신호)<br>물리적 연결 및 매체 제어   | - | 리피터, 허브, 케이블 |

** 출처 : OSI 7 layer에 대해 설명하세요..md

<br>

### 웹서버는 어디서 동작하는가?

웹서버는 HTTP 요청과 응답을 처리함
- HTTP는 Application Layer(7계층)에 속하는 프로토콜
- 그리고 HTTP 통신은 TCP 기반으로 동작, TCP는 Transport Layer(4계층)
즉,웹 서버는 직접적으로는 7계층에서 동작하지만, 하위 계층인 4계층(TCP)과 협력하여 네트워크 통신을 수행

<br>

### HTTP와 TCP/IP의 관계
| 프로토콜 | 계층        | 설명 |
|----------|-------------|------|
| HTTP     | 7계층 (Application Layer) | 웹 데이터(요청/응답)를 정의하는 애플리케이션 프로토콜 |
| TCP      | 4계층 (Transport Layer)   | 패킷의 순서 보장, 재전송 등 신뢰성 있는 데이터 전송 담당 |
| IP       | 3계층 (Network Layer)     | 목적지까지 데이터 전송을 위한 주소 지정과 경로 설정 |

<br>


### 웹서버와 OSI 계층 연결
| 계층           | 해당 역할 |
|--------------|-----------|
| 7 . 응용 계층    | **Nginx, Apache, 웹 브라우저, HTTP, HTTPS** |
| 4 . 전송 계층    | TCP (포트 80, 443 사용) |
| 3 . 네트워크 계층  | IP 주소 기반 통신 |
| 2 . 데이터 링크 계층 | MAC 주소, 이더넷 등 |
| 1 . 물리 계층    | 실제 물리적 전송: 케이블, 전파 등 |


<br><br>

## 새끼 질문
- HTTP는 OSI 7계층 중 어디에 속하며, 왜 그렇게 분류되나요?
- OSI 7계층 중 전송 계층과 응용 계층은 각각 어떤 프로토콜을 다루고, 어떤 역할을 하나요?