HTTP(HyperText Transfer Protocol)는 **웹에서 데이터를 주고받기 위한 프로토콜**이다. 웹 브라우저와 서버가 서로 통신하는 방식을 정의하며, 클라이언트(예: 브라우저)가 서버에 요청(request)을 보내고, 서버가 응답(response)을 반환하는 방식으로 동작한다.

### # 인터넷 네트워크

- 인터넷 통신
- IP(Internat Protocol)
	- IP(인터넷 프로토콜)
		- 지정한 IP 주소(IP Address)에 데이터 전달
		- 패킷 이라는 통신 단위로 데이터를 전달
			- 출발지IP, 도착지IP, 메시지 등으로 구성
	- IP 프로토콜의 한계
		- 비연결성: 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
			- 프로토콜은 IP 대상 서버가 패킷을 받을 상태인지 알 수 없다.
		- 비신뢰성: 중간에 패킷이 사라지거나, 패킷이 순서대로 오지 않는 경우
		- 프로그램 구분: 같은 IP를 사용하는 서버에서 통신하는 어플리케이션이 둘 이상이면?
		- 전송 방식, 포트 번호, 패킷의 순서, 검증 정보등을 담을 수 없다.
- TCP, UDP
	- 인터넷 프로토콜 스택의 4계층
		- 애플리케이션 계층 - HTTP, FTP
		- 전송 계층 - TCP, UDP
		- 인터넷 계층 - IP
		- 네트워크 인터페이스 계층
	- 계층의 메시지 구조
		- 프로그램이 메시지 생성
		- SOCKET 라이브러리를 통해 전달
		- TCP 정보 생성, 메시지 데이터 포함
			- 출발지 PORT
			- 도착지 PORT
			- 전송 제어
			- 순서
			- 검증 정보 등
		- IP 패킷 생성, TCP 데이터 포함
			- 출발지 IP, 목적지 IP
	- 전송 제어 프로토콜(Transmission Control Protocol) 특징
		- 연결 지향 - TCP 3 way handshake(가상 연결)
			- 연결 후 메시지를 전달
			- TCP 3 way handshake의 연결 과정
				- SYN - 싱크로나이저라로 씬이라는 메시지를 보냄 :클라이언트가 서버에게 접속 요청
				- SYN + ACK - 서버가 클라이언트에게 접속 요청 + 서버가 클라이언트 요청 수락 
				- ACK - 클라이언트가 서버의 요청 수락
					- 최근에는 클라이언트의 ACK 응답 단계에서 데이터도 함께 전송
				- 데이터 전송
			- 논리적으로 연결이 되었다는 개념이다.
				- 실제 물리적 중간 노드들은 두 서버가 연결되었는지 확증할 수 없다.
		- 데이터 전달 보증
			- 클라이언트에서 데이터 전달 후 서버는 이에 대해 응답한다.
		- 순서 보장
			- 패킷의 순서를 보장한다.
		- 신뢰할 수 있는 프로토콜
		- 현재는 대부분 TCP 사용
	- 사용자 데이터그램 프로토콜(User Datagram Protocol)의 특징
		- 하얀 도화지에 비유(기능이 거의 없음)
		- 연결 지향 x
		- 데이터 전달 보증 x
		- 순서 보장 x
		- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠르다.
		- IP와 거의 같다. 하지만 PORT, 체크섬 정도가 추가되어 있다.
		- 애플리케이션 레벨에서 추가 작업이 필요하다.
![Pasted image 20250106194347.png](../img/Pasted%20image%2020250106194347.png) 

![Pasted image 20250106195302.png](../img/Pasted%20image%2020250106195302.png)
- PORT
	- 서버 안에서 돌아가는 애플리케이션을 구분
	- 같은 IP 내에서 프로세스를 구분
	- 0 ~ 65535 할당 가능
	- 잘 알려진 포트
		- FTP - 20,21
		- TELNET - 23
		- HTTP - 80
		- HTTPS - 443
- DNS
	- IP는 기억하기 어렵고, 변경될 수 있다.
	- DNS(Domain Name System)
		- 도메인 명을 IP 주소로 변환
	- DNS 사용
		1. 클라이언트가 도메인 명 입력
		2. DNS 서버에 요청
		3. DNS 서버가 클라이언트에 실제 IP를 응답
		4. 클라이언트가 실제 서버 IP에 요청

---
### # URI와 웹 브라우저 요청 흐름

![Pasted image 20250107210910.png](../img/Pasted%20image%2020250107210910.png)

#### URI(Uniform Resource Identifier)

**URI**(Uniform Resource Identifier)는 하나의 리소스를 가리키는 문자열이다.
가장 흔한 URI는 URL로, 웹 상에서의 위치로 리소스를 식별한다. 반면, URN은 주어진 이름공간 안의 이름으로 리소스를 식별한다.
- URL: 리소스의 위치/주소
- URN: 리소스의 이름
- 위치는 변할 수 있지만, 이름은 변한지 않는다.
##### URI 단어의 뜻
- Uniform: 리소스를 식별하는 통일된 방식 
- Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier: 다른 항목과 구별하는데 필요한 정보
#### URL(Uniform Resource Locator)

URL은 웹에서 주어진 고유 리소스 주소(위치)이다. 이론적으로 각각의 유효한 URL은 고유한 리소스를 가리킨다. 이러한 리소스는 HTML 페이지, CSS 문서, 이미지 등이 될 수 있다. URL로 표시되는 리소스와 URL 자체는 웹 서버에서 처리 된다.

![Pasted image 20250107210646.png](../img/Pasted%20image%2020250107210646.png)

#### URL 구성 요소

##### 스키마(scheme)

- 주로 프로토콜 사용
- 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속/규칙
	- 프로토콜은 네트워크에서 데이터를 교환하거나 전송하기 위한 설정 방법
	- http, https, ftp 등
- http는 보통 80 포토, https 는 보통 443 포트를 사용한다.
	- 포트는 생략 가능하다.
- https는 http에 보안을 추가한것이다 (HTTP secure)

##### 도메인 네임(Domain Name)

- 호스트명
- 도메인명 또는 IP 주소를 직접 사용가능하다.

##### PORT

- 포트는 엡 서버의 리소스에 접근하는데 사용되는 기술적인 게이트를 나타낸다.
- 접속 포트이다
- 일반적으로 생략가능하다.

##### PATH

- 리소스 경로이다.
- 계층적 구조

##### QUERY

- key=value 형태
- ?로 시작, &로  추가 가능
- query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자 형태이다.

##### FRAGMENT

- html 내부 북마크 등에 사용
- 서버에 전송하는 정보는 아님

#### 브라우저에서 요청 과정

![Pasted image 20250107222057.png](../img/Pasted%20image%2020250107222057.png)

1. 웹 브라우저(클라이언트)가 HTTP 메시지 생성
	- 대략적인 메시지 형태는 아래와 같다
	- GET / search?q=hello HTTP/1.1 HOST:www.google.com
 2. 소켓 라이브러리를 통해 OS 계층 TCP/IP 계층에 메시지 전달
	 1. TCP/IP 연결 (IP, PORT)
	 2. 데이터 전달
 3. TCP/IP 패킷 생성
	 - 이때 데이터를 패킷으로 감싼다.
	 - 포트 번호, 검증 정보, 패킷 순서 등의 정보를 작성한다.
 4. 네트워크 인터페이스 계층 물리적 구조 LAN 장비를 통해 인터넷망으로 서버에 요청한다.
 5. 서버에서 HTTP 응답 메시지를 생성하여 응답한다.

브라우저의 핵심 기능은 필요한 리소스(HTML,CSS,자바스크립트,이미지,폰트)등의 정적 파일 또는 동적으로 생성한 데이터를 서버에 요청하고 응답 받아 브라우저에 시각적으로 렌더링하는 것이다.

- 브라우저의 주소창에 URL 입력 -> 호스트 이름이 DNS를 통해 IP주소로 변환 -> 이 주소를 갖는 서버에게 요청을 전송

1. **사용자가 웹 브라우저를 통해 google.com 을 입력하면 URL 주소 중 도메인 네임 부분을 [DNS](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-dns) 서버에서 검색.**
2. **DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 [URL](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-url) 정보와 함께 전달.**
3. **브라우저는 [HTTP](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-http) [프로토콜](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-프로토콜)을 사용하여 요청 메시지를 생성하고 HTTP 요청 메시지는 [TCP](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-tcp)/[IP](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-ip) 프로토콜을 사용하여 서버로 전송.**
4. **서버는 [response](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-response) 메시지를 생성하여 다시 브라우저에게 데이터를 전송합니다.**
5. **브라우저는 response를 받아 [파싱](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-파싱)하여 화면에 [렌더링](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-렌더링)한다.**

- DNS(**Domain Name System**): 도메인 이름 시스템은 사용자가 입력한 도메인을 머신이 읽을 수 있는 IP주소로 변환하는 시스템이다.
- URL(Uniform Resource Locator): URL은 통합 자원 지시자로 인터넷의 리소스를 가르키는 표준명칭으로 서버의 자원을 요청할때 사용된다.URL을 통해 인터넷 상의 모든 리소스를 요청할 수 있으며, HTTP, FTP 등의 자원 요청도 가능하다.
- HTTP(Hyper Text Transfer Protocol): HTTP는 TCP기반의 클라이언트와 서버 사이에 이루어지는 요청/응답 프로토콜이다.HTTP는 Text Protocol로 사람이 쉽게 읽고 쓸 수 있습니다. 프로토콜 설계상 클라이언트가 요청을 보내면 반드시 응답을 받아야 합니다. 응답을 받아야 다음 request를 보낼 수 있다.
- TCP (**T**ransmission **C**ontrol **P**rotocol)은 두 개의 호스트를 연결하고 데이트 스트림을 교환하게 해주는 중요한 네트워크 프로토콜이다. TCP는 데이터 전송을 제어하고 데이터를 어떻게 보낼지, 어떻게 맞출지를 정한다. 또한 데이터와 패킷이 보내진 순선대로 전달 하는 것을 보장해준다.신뢰성과 연결성을 책임지기 위한 프로토콜이며, 호스트와 호스트간의 데이터 전송은 IP(인터넷 계층 프로토콜)에 의지하면서 동시에 신뢰성 있는 전송에 대해서는 TCP가 책임지는 구조이다.

> 참조 - https://developer.mozilla.org/ko/docs/Learn_web_development/Howto/Web_mechanics/What_are_hyperlinks
> 
---

### # HTTP(Hyper Text Transfer Protocol) 

- HTTP 메시지에 모든 것을 전송
- HTML, TEXT, 이미지, 음성, 영상, 파일 등
- JSON,XML (API)
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터 전송 가능

#### HTTP 역사
- 1.0: 메서드, 헤더 추가
- 1.1: 가장 많이 사용
- 2.0: 성능 개선

#### 기반 프로토콜
- TCP: HTTP/1.1, HTTP/2.0
- UDP: HTTP/3

#### HTTP 특징

HTTP API는 HTTP 프로토콜을 사용하여 애플리케이션 간에 데이터를 전송하거나 상호작용하는 API이다.
- HTTP 프로토콜: 인터넷에서 주로 사용되는 통신 규약으로, 클라이언트와 서버 간에 요청(Request)과 응답(Response)을 주고받는 구조이다.
- HTTP API는 주로 HTTP 메서드(GET, POST, PUT, DELETE 등)를 통해 동작하며, 데이터 형식으로는 JSON, XML, HTML 등이 사용된다.
- HTTP API는 REST, GraphQL, gRPC 같은 여러 스타일로 설계될 수 있다.

##### 특징

- 클라이언트 서버 구조
	- Request Response 구조
	- 클라이언트는 서버에 요청을 보내고, 응답을 대기
	- 서버가 요청에 대한 결과를 만들어서 응답
- 무상태 프로토콜(stateless)
	- stateless
	- 서버가 클라이언트의 상태를 보존하지 않는다.
		- 서버는 클라이언의 이전 상태를 알지 못한다.
			- 단지 요청에 응답을 한다.
		- 장점 - 서버 확장성이 높다 -> 스케일 아웃(수평 확장 유리)
		- 단점 - 클라이언트에서 추가적인 데이터 전송이 필요하다.
	- 무상태는 응답 서버를 쉽게 바꿀 수 있다 -> 무한한 서버 증설 가능
	- 일반적으로 상태 유지를 위해 브라우저의 쿠키/세션 등을 사용해서 상태를 유지한다.
- 비연결성
	- 클라이언트 와 서버가 자원을 현재 요청을 주고 받을때만 연결한다.
		- 이는 서버가 유지하는 자원을 최소한으로 줄 일 수 있다.
	- HTTP는 기본이 연결을 유지하지 않는 모델이다
	- 일반적으로 초 단위 이하의 빠른 속도로 응답하기 때문이다
		- 이는 서버 자원을 매우 효율적으로 사용할 수 있다.
	- 단점
		- TCP/IP 연결을 매 요청마다 새로 맺어야 한다.
			- 3  way handshake 시간이 추가 된다.
		- 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 이미지 등 수 많은 자원이 함께 다운로드
		- 지금은 기본적으로 HTTP 지속 연결로 문제를 해결
			- Persistent Connections
				- 한 번 설정된 TCP 연결을 유지하여 여러 HTTP 요청 과 응답을 동일한 연결을 통해 처리하는 방식이다.
				- HTTP/1.1 부터 기본 동작으로 설정되었다.
				- HTTP/1.1에서는 기본적으로 Connection 헤더의 Persistent Connections을 사용한다.
				- 한계
					- 리소스 사용 증가:
					    - 연결을 오래 유지하면 서버와 클라이언트 모두 메모리와 리소스를 더 많이 사용
					- 비효율적인 연결 유지:
					    - 연결을 장시간 유지하면서 아무 요청도 없는 경우 리소스 낭비가 발생
					- 최대 연결 제한:
					    - 서버 또는 네트워크 장치에서 허용하는 동시 연결 수가 제한
		- HTTP/2, HTTP3에서 더 많은 최적화가 이루워짐
			- HTTP/2: Persistent Connections의 개념을 더욱 발전시켜, 하나의 연결에서 멀티플렉싱을 통해 동시 다중 요청을 처리.
			- HTTP/3: TCP 대신 UDP 기반의 QUIC 프로토콜을 사용하여 연결 설정 시간을 더 줄이고 성능을 최적화.
- HTTP 메시지
	- ![Pasted image 20250113021251.png](../img/Pasted%20image%2020250113021251.png)
	- HTTP 메세지 구조
		- start-line 시작 라인
		- header 헤더
		- empty line 공백 라인(CRLF)
		- message Body
	- 요청 메시지
		- 시작라인
			- start-line = request-line/status-line
			- start-line = method SP(공백) request-target SP(공백) HTTP-version CRLF(엔터)
			- HTTP 메서드
				- GET, POST, PUT, DELETE
				- 서버가 수행해야 할 동작을 지정한다.
			- 요청 대상
				- absolute-path`[?query]` = 절대경로 ? 쿼리
				- 절대경로는 "/"로 시작하는 경로이다.
			- HTTP Version 으로 구성
		- 헤더
			- field-name: field-value OWS(띄어쓰기 허용)
			- field-name은 대소문자 구문이 없다.
			- HTTP 전송에 필요한 모든 부가정보
				- 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증 ,요청 클라이언트 정보, 캐시 관리정보 등등
			- 표준 헤더가 많음
			- 필요시 임의의 헤더 추가 가능
		- 메시지 바디
			- 실제 전송할 데이터
			- HTML 문서, 이미지, 영상 ,JSON 등등 byte로 표현할 수 있는 모든 데이터는 전송 가능하다.
	- 응답 메시지
		- 시작 라인
			- start-line = HTTP-version CRLF SP(공백) status-code SP(공백) reason-pharse CRLF
			- HTTP 버전
			- HTTP 상태 코드: 요청 성공/실패를 나타냄
				- 200: 성공
				- 400: 클라이언트 요청 오류
				- 500: 서버 내부 오류
			- 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글 
- 단순함, 확장 가능
	- HTTP는 단순하다.
	- HTTP 메시지도 매우 단순하다.

#### REST API 란?
#RESTAPI

REST API는 HTTP API의 설계 스타일 중 하나로, REST (Representational State Transfer)라는 아키텍처 원칙을 따른다.
- REST API는 REST 설계 원칙에 따르는 HTTP API 이다.
- 모든 REST API는 HTTP API이지만, 모든 HTTP API REST API는 아니다.
- URL는 리소스를 표현하는데 집중해야 한다.
- 행위에 대한 정의는 HTTP 요청 메소드를 통해 해야 한다.
##### 특징

- Client-Server 구조: 클라이언트와 서버는 서로 독립적으로 작동하며, 클라이언트는 사용자 인터페이스를 담당하고 서버는 데이터 처리를 담당한다.
- Stateless (무상태성): 서버는 요청 간에 클라이언트의 상태를 저장하지 않는다. 각 요청은 필요한 모든 정보를 포함해야 합니다.
- 리소스 기반: REST는 리소스를 중심으로 설계 된다. 각 리소스는 고유한 URL를 가지며, HTTP 메서드를 사용하여 작업을 수행한다.
- 표현의 통일성: 서버는 리소스의 표현을 JSON, XML 등 표준 형식으로 전달해야한다.
- 캐싱 가능: 서버 응답은 캐시될 수 있어야 한다.
- 계층화된 시스템: 클라이언트트 중간 서버(로드 밸런서, 프록시)를 통해 서버와 상호작용 할 수 있다.
---
### HTTP 메서드

URL 구성시 가중 중요한것은 리소스 식별이다.
- 리소스의 의미
	- 미네라를 캐라 -> 미네랄이 리소스이다.
- 어떻게 식별할까?
	- 예를 들어 회원을 등록하고, 수정하고, 조회하는 행위는 모두 배제 한다.
	- 회원이라는 리소스만 식별하면 된다. -> URL에 매핑
- URL 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용을 권장한다.
	- /members/{id} 형태

#### 리소스와 행위의 분리

가장 중요한것은 리소스를 식별하는 것이다.
- URL는 리소스만 식별해야 한다.
- 리소스와 해당 리소스를 대상으로 하는 행위를 분리
	- 리소스: 회원
	- 행위: 조회, 등록, 삭제 ,변경
- 리소스는 명사, 행위는 동사이다.
- 행위(메서드)는 어떻게 구분?
#### HTTP 메서드

- GET: 리소스 조회
	- 서버에 전달하고 싶은 데이터는 query를 통해서 전달한다.
	- 메시디 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않는다.
- POST: 요청 데이터 조회, 주로 등록에 사용
	- 스펙: POST 메서드는 대상 리소스가 리소스의 고유한 의미 체계에 따라 요청에 포함 된 표현(데이터)을 처리하도록 요청
	- 요청 데이터 조회
	- 메시지 바디를 통해 서버로 요청 데이터를 전달하고 처리를 요청
	- ==서버는 요청 데이터를 처리==
		- 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
		- 서버에서 신규 리소스 처리, 프로세스 처리에 사용
		- 리소스 URL은 서버에서 생성한다.
	- 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용 한다.
	- 리소스 URL에 대한 응답 메시지의 상태 코드로 201 Created를 반환한다.
- PUT: 리소스를 대체, 해당 리소스가 없으면 생성
	- 리소스가 있으면 대체 하고, 리소스가 없으면 생성한다.
	- ==클라이언트가 리소스를 식별(알고 있어야 한다.)==
		- 클라이언트가 리소스 위치를 알고 있어 URL을 지정
		- ex) PUT / users/100
		- 위 부분이 POST와의 차이점이다.
- PATCH: 리소스 부분 변경
- DELETE: 리소스 삭제
- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS 에서 사용)
#### HTTP 메서드 속성

![Pasted image 20250116233013.png](../img/Pasted%20image%2020250116233013.png)

- 안전
	- 호출해도 리소스를 변경하지 않는다.
- 멱등성 
	- #멱등성
	- 동일한 요청을 한 번 보내는 것과 여러 번 연속으로 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일하게 남을 때, 해당 HTTP 메서드가 멱등성을 가졌다고 한다.
		- 요청에 어떠한 부수효과(side effect)도 존재해서는 안된다.
	- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 같다.
	- 멱등 메서드
		- GET: 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
		- PUT: 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
		- DELETE: 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 같다.
		- POST: 멱등이 아니다. 두 번 호출하면 같은 결제가 중복해서 발생한다.
	- 활용
		- 자동 복구 메커니즘
		- 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가의 판단 근거이다.
		- 네트워크 지연, 중복 요청 등의 문제를 해결하는데 도움을 준다.
	- 멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않는다.
- 캐시 가능
	- 응답 결과 리소스를 캐시해서 사용해도 되는가?
	- GET, HEAD, POST, PATCH는 캐시가 가능하다.
	- 실제로는 GET, HEAD 정도만 캐시로 사용
		- POST,  PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않다.

---
### # HTTP 메서드 활용

#### 클라이언트에서 서버로 데이터 전송

- 쿼리 파리미터를 통한 데이터 전송
	- GET
	- 주로 정렬 필터(검색어)
- 메시지 바디를 통한 데이터 전송
	- POST,PUT,PATCH
- 정적 데이터 조회
	- 이미지, 정적 텍스트 문서
	- 조회는 GET 메서드 사용
	- 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능하다.
- 동적 데이터 조회
	- 검색, 게시판 목록에서 정렬 필터
	- 조회 조건을 줄여주는 필터, 조회 결과를 졍렬하는 정렬 조건에 주로 사용 한다.
	- 조회는 GET 사용
	- 쿼리 마라미터를 사용해서 데이터를 전달한다.
- HTML Form을 통해 데이터 전송
	- 회원 가입, 상품 주문, 데이터 변경
	- x-www-form-urlencoded
		- form의 내용을 메시지 바디를 통해 전송 (key=value) 쿼리 마라미터로 인코딩처리
		- 주로 POST를 사용하지만 GET도 가능
	- multipart/form-data
		- 파일 업로드 같은 바이너리 데이터 전송시 사용
		- 다른 종류의 여러 파일과 폼의 내용을 함께 전송 가능하다.
	- HTML Form 전송은 GET/POST 메서드만 지원한다.
- HTTP API를 통한 데이터 전송
	- 회원 가입, 상품 주문, 데이터 변경
	- 서버 to 서버
		- 백엔드 시스템 통신
	- 웹 클라이언트
		- 아이폰, 안드로이드
	- Ajax
		- HTML에서 Form 전송대신 자바스크립트 AJAX 모델 사용

#### HTTP API 설계 예시

- HTTP API - 컬렉션
	- POST 기반 등록
	- 회원 관리 API 제공
	- 서버가 리소스 URL을 결정한다.
- HTTP API - 스토어
	- PUT 기반 등록
	- 클라이언트가 리소스 URL를 알고 있어야 한다.
		- 파일 등록 /files/{filename} -> PUT
		- PUT /files/star.jpg
	- 클라이언트가 직접 리소스의 URI을 관리하고 결정한다. 
	- 정적 컨텐츠 관리, 원격 파일 관리
- HTTP FORM 사용
	- 순수 HTML FORM 사용
	- 웹 페이지 회원 관리
	- GET, POST만 지원
---
###  # 상태 코드
#HTTP상태코드

클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

- 1xx(Informational): 요청이 수신되어 처리중
- 2xx(Successful): 요청 정상 처리
- 3xx(Redirection): 요청을 완료하려면 추가 행동이 필요
- 4xx(Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx(Server Error): 서버오류, 서버가 정상 요청을 처리하지 못함

만약 모르는 상태 코드가 나타나면?
- 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하며?
- 클라이언트는 상위 상태코드로 해석해서 처리한다.
- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨.
- ex)
	- 299 ??? -> 2xx(Successful)로 해석
	- 451 ??? -> 4xx(Client Error)로 해석
	- 599 ??? -> 5xx(Server Error)로 해석

#### 1xx(Informational)

- 요청이 수신되어 처리중
- 거의 사용하지 않음
#### 2xx(Successful)

- 클라이언트의 요청을 성공적으로 처리
- 200 OK
	- 요청 성공
- 201 Created
	- 요청 성공해서 새로운 리소스가 생성
	- 생성된 리소스는 응답의 Location 헤더 필드로 식별
- 202  Accepted
	- 요청이 접수되었으나 처리가 완료되지 않았음
	- 배치 처리 같은 곳에서 사용
- 204 No Content
	- 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
	- 결과 내용이 없어도 204메시지 만으로 성공을 인식할 수 있다.

#### 3xx (Redirection)

![Pasted image 20250218014445.png](../img/Pasted%20image%2020250218014445.png)

![Pasted image 20250218015812.png](../img/Pasted%20image%2020250218015812.png)

- 요청을 완료하기 위해 유저 에이전트의 추가 조치가 필요
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
- 리다이렉션 종류
	- 영구 리다이렉션 - 특정 리소스의 URL가 영구적으로 이동
		- 예 /users -> /members
		- 검색 엔진 등에서 URL을 변경
		- 301 Moved Permanently
		- 308 Permanent Redirect
	- 일시 리다이렉션 - 일시적인 변경
		- 리소스의 URL가 일시적으로 변경
			- 따라서 검색 엔진 등에서 URL을 변경하면 안된다.
		- 주문 완료 후 주문 내역 화면으로 이동
		- PRG: Post/Redirect/Get
			- POST로 주문후에 새로 고침으로 인한 중복 주문 방지
			- POST로 주문후에 주문 결과 화면을 GET 메서드로 리다이렉트
			- 새로고침해도 결과 화면을 GET으로 조회
			- 중복 주문 대신에 결과 화면만 GET으로 다시 요청
		- 302 Found, 
		- 307 Temporary Redirect 
		- 303 See Other
	- 특수 리다이렉션
		- 결과 대신 캐시를 사용
		- 304 Not Modified
- 300 Multiple Choices
	- 여러 선택지, 리소스에 여러 선택지가 있음
- 301 Moved Permanently
	- 리다이렉트시 요청 메서드가 GET으로 변하고, 요청 메시지의 바디가 제거될 수 있다.
	- 리소스가 완전히 새로운 URL로 변경
		- 즉 영구적으로 이동
	- 검색엔진 등에서도 새로운 URL로 변경
		- 원래의 URL를 사용하지 않는다. 이는 검색 엔진 등에서도 변경을 인지한다.
- 308 Permanent Redirect
	- 301과 기능은 같다.
	- 리다이렉트시 요청 메서드와 본문 유지
		- 처음 POST 요청이면 리다이렉트도 POST
	- 실무에서 거의 사용되지 않는다.
- 302 Found
	- 리다이렉트시 요청 메서드가 GET으로 변하고, 요청 메시지의 바디가 제거될 수 있다.
		- 스펙상 유지 그러나 거의 바뀐다.
	- 리소스가 일시적으로 새로운 URL로 변경
	- 즉 일시적으로 이동
	- 검색 엔진 등에서 URL을 변경하지 않는다.
- 307 Temporary Redirect
	- 302와 기능은 같다.
	- 리다이렉트시 요청 메서드와 본문 유지
		- 처음 POST 요청이면 리다이렉트도 POST
- 303 See Other
	- 302와 기능은 같다.
	- 리다이렉트시 요청 메서드가 GET으로 변경 (무조건)
	- 리다이렉트시 요청 메시지의 바디가 제거
- 304 Not Modified
	- 캐시를 목적으로 사용한다.
	- 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용한다. (캐시로 리다이렉트 한다.)
	- 304 응답은 응답에 메시지 바디를 포함하면 안된다.
	- 조건부 GET, HEAD 요청시 사용

#### 4xx(Client Error)

- 클라이언트 오류 
- 클라이언트의 요청에 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 오류의 원인이 클라이언트에 있음
- 클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에 똑같은 재시도가 실패함.
	- 요청을 수정해서 보내야 함.
- 400 Bad Request
	- 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
	- 요청 구문, 메시지 등등 오류
	- 클라이언트는 요청 내용을 다시 검토하고, 보내야함
	- 예) 요청 파라미터가 잘못되거나, API 스펙이 맞지 않을때
- 401 Unauthorized
	- 인증 되지 않음
	- 401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명
	- 인증(Authentication)과 인가(Authorization)
		- 인증(Authentication): 본인이 누구인지 확인 
			- 로그인
		- 인가(Authorization): 권한(권한 레벨) 부여 (Admin 권한처럼 특정 리소스에 접근할 수 있는 권한, 인증이 있어야 인간가 있음)
		- 오류 메시지가 Unauthorized 이지만, 인증 실패가 아니라 권한이 없는 경우에도 사용
- 403 Forbidden
	- 서버가 요청을 이해했지만 승인을 거부함
	- 주로 인증 자격 증명은 있지만, 접근 권한이 불충반한 경우이다.
	- 예) 어드민 등급이 아닌 사용자가 로그인은 했지만, 어드민 등급의 리소스에 접근하는 경우
- 404 Not Found
	- 요청 리소스를 찾을 수 없음
	- 요청 리소스가 서버에 없음
	- 클라이언트가 권한이 부족한 리소스에 접근할때 해당 리소스를 숨기고 싶을 때 사용

#### 5xx(Server Error)

- 서버 문제로 오류가 발생
- 서버에 문제가 있기 때문에 재시도 하면 성공할 수 있음 (복구가 되거나 등등)
- 500 Internal Server Error
	- 서버 내부 문제로 오류가 발생
	- 서버 문제로 정상적인 요청을 처리하지 못함
- 503 Service Unavailable
	- 서비스 이용 불가
	- 서버가 일실적인 과부화 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
	- Retry-After 헤더 필드로 언제 복구되는지 보낼 수 있음

---
###  # HTTP 헤더

![Pasted image 20250306152248.png](../img/Pasted%20image%2020250306152248.png)

- header-field = field-name ":" OWS field-value OWS
- field-name은 대소문자 구분이 없다.

#### 용도

- HTTP 전송에 필용한 모든 부가정보
	- 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트 정보, 서버 정보, 캐시 관리 정보 등등
- 표준 헤더가 많음
	- https://developer.mozilla.org/ko/docs/Web/HTTP/Headers
- 필요시 임의의 헤더 추가가 가능하다.
	- 커스텀 헤더 추가 가능
	- X-로 시작하는 커스텀 헤더

#### HTTP 헤더 분류

- General Header
	- 메시지 전체에 적용되는 정보
	- Cache-Control, Connection, Date, Pragma, Trailer, Transfer-Encoding, Upgrade, Via, Warning
- Request Header
	- 요청 정보
	- Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match, If-Modified-Since, If-None-Match, If-Range, If-Unmodified-Since, Max-Forwards, Proxy-Authorization, Range, Referer, TE, User-Agent
- Response Header
	- 응답 정보
	- Accept-Ranges, Age, ETag, Location, Proxy-Authenticate, Retry-After, Server, Vary, WWW-Authenticate
- Entity Header -> 표현헤더로 변경
	- 엔티티 바디 정보
	- Allow, Content-Encoding, Content-Language, Content-Length, Content-Location, Content-MD5, Content-Range, Content-Type, Expires, Last-Modified

#### HTTP Body
message body - RFC7230(최신)

- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공
	- 데이터 타입, 길이, 압축, 가공 등등
##### 참고
- RFC723x 변화
	- 엔티티 -> 표현
	- Representation -> Representation Metadata + Representation Data
	- 표현 = 표현 메타데이터 + 표현 데이터

#### 표현

- Content-Type: 
	- 표현 데이터의 현식
	-  text/html; charset=utf-8, application/json, image/png
- Content-Encoding
	- 표현 데이터의 압축 방식
	- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
	- 데이터를 읽는 쪽에서 메시지 바디를 인코딩 헤더의 정보로 압축 해제
	- gzip, deflate, identity(압축 하지 않음)
- Content-Language: 
	- 표현 데이터의 자연 언어
	- ko, en, en-US
- Content-Length: 
	- 표현 데이터의 길이
	- Transfer-Encoding을 사용하면 Content-Length를 사용하면 안된다.
		- Transfer-Encoding 내부에 정보들이 들어가기 때문에 Content-Length를 사용하면 안된다.
		- Transfer-Encoding: chunked
	- 바이트 단위

#### 협상(Content Negotiation)
클라이언트가 선호하는 표현 요청

- 협상 헤더는 요칭시에만 사용한다.
- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

#### 협상과 우선순위

![Pasted image 20250306161956.png](../img/Pasted%20image%2020250306161956.png)

- Quality Value(q)
	- 0~1, 클수록 높은 우선순위
	- 생략시 1
	- Accept-Language: ko-KR, en;q=0.9
		- 한국어(1) q 생략, 영어(0.9)
- 구체적인 것이 우선이다.
	- Accept: text/*, text/html, text/html;level=1, text/html;level=2;q=0.4
		- text/html;level=1
		- text/html
		- text/*

#### 전송 방식

- 단순 전송
	- 한번에 요청하여 한번에 응답
- 압축 전송
	- 서버에서 응답 데이터를 압축해서 전송
	- Content-Encoding을 명시하여 전송
- 분할 전송
	- 여러개의 응답 메시지를 전송
	- Transfer-Encoding: chunked
	- Content-Length를 사용하면 안된다.
		- chunked 마다 길이를 명시
- 범위 전송
	- 범위를 지정해서 요청 후 응답
	- Range: bytes=5001-10000/10000

#### 일반 정보

- From: 사용자 에이전트의 이메일 정보
	- 검색 엔진 같은 곳에서 주로 사용
	- 요청에서 사용
- Referer: 이전 웹 페이지 주소
	- 현재 요청된 페이지의 이전 웹 페이지 주소
	- A -> B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
	- Referer를 사용해서 유입 경로 분석 가능
	- 요청에서 사용
- User-Agent: 사용자 에이전트 애플리케이션 정보
	- 클라이언트의 애플리케이션 정보 (브라우저 정보)
	- 통계 정보
	- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
	- 요청에서 사용
- Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보 
	- 실제 요청을 처리하는 서버
		- 중간의 프록시 서버의 정보가 아님
	- Server: Apache/2.2.22 (Debian) / nginx
	- 응답에서 사용
- Date: 메시지가 발생한 날짜와 시간
	- 응답에서 사용

#### 특별한 정보

![Pasted image 20250307160644.png](../img/Pasted%20image%2020250307160644.png)
- Host: 요청한 호스트 정보 (도메인)
	- 요청에서 사용
		- 필수 값이다.
	- 하나의 서버가 여러 도메인을 처리해야 할 때
	- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
- Location: 페이지 리다이렉션
	- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
	- 201 Created: Location값은 요청에 의해 생성된 리소스 URL
	- 3xx Redirection: Location 값은 요청을 자동으로 리다이렉션 할 리소스 URL
- Allow: 허용 가능한 HTTP 메서드
	- 405(Method Not Allowed)에서 응답에 포함해야함
	- Allow: GET, HEAD, PUT
- Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
	- 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
	- Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
	- Retry-After: 120 (초단위 표기)

#### 인증

- Authorization: 클라이언트 인증 정보를 서버에 전달
	- Authorization: Basic xxxxxxxxxxx
- WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의
	- 401 Unauthorized 응답과 함께 사용
	- WWW-Authenticate: Basic realm="User Visible Realm"

#### 쿠키

HTTP는 무상태(Stateless) 프로토콜이다. 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다. 따라서 서버는 이전 클라이언트의 상태를 알지 못한다. 이러한 무상태 프로토콜의 단점을 극복하기 위해 쿠키(Cookie)를 사용한다.

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
	- Set-Cookie: key=value, key2=value2
- Cookie: 클라이언트가 서버에게 쿠키 전달(요청)
- 쿠키는 모든 요청에 쿠키 정보를 자동 포함한다.
	- 사용처 
		- 사용자 로그인 세션 관리
		- 광고 정보 트래킹
	- 쿠키 정보는 항상 서버로 전송된다.
		- 네트워크 트래픽 추가 유발
		- 최소한의 정보만 사용 (세션 ID, 인증 토큰)
		- 보안에 민감한 데이터는 저장하면 안됨
		- 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지를 사용한다.
			- localStorage: 영구 보관
				- 만료 기간이 없다.
				- 용량이 크다.
			- sessionStorage: 세션 보관
				- 세션 종료시 데이터 삭제
				- 용량이 작다.
- 생명 주기
	- Set-Cookie: Expires=Sat, 26-Mar-2022 07:28:00 GMT
		- 만료일을 지정할 수 있다.
		- 만료일이 되면 쿠키 삭제
	- Set-Cookie: Max-Age=3600 (3600초 후 만료)
		- 0 이나 음수를 지정하면 쿠키 삭제
	- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
	- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지
- 도메인 (Domain)
	- 예) domain=example.org
	- 명시: 명시한 기준 도메인 + 서브 도메인 포함
		- domain=example.org를 지정해서 쿠키를 생성하면
		- example.org, www.example.org, dev.example.org등 서브 도메인에서도 사용 가능하다.
	- 생략: 현재 문서 기준 도메인만 적용
		- 현재 문서의 도메인이 example.org 에서 쿠키를 생성하고 domain 지정을 생략
		- example.org 에서만 쿠키 접근
		- dev.example.org는 쿠키 미접근
- 경로 (Path)
	- 예) path=/home
	- 이 경로를 포함한 하위 경로 페이지에서만 쿠키 접근 가능
	- 일반적으로 path =/로 루트로 지정
- 보안
	- Secure
		- 쿠키는 HTTP, HTTPS 를 구분하지 않고 전송
	    - Secure를 적용하면 HTTPS인 경우에만 전송
    - HttpOnly
	    - XSS(크로스 사이트 스크립팅) 공격을 방지하기 위해 사용
        - 자바스크립트에서 쿠키에 접근 불가
        - HTTP 전송에만 사용
	- SameSite
		- XSRF(사이트간 요청 위조) 공격 방지
		- 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키 전송

### # 캐시

- 캐시가 없다면?
	- 웹 브라우저는 이미 다운로드한 리소스를 계속해서 다운로드
		- 즉 데이터가 변경되지 않아도 게속 네트워크롤 통해서 데이터를 다운로드 받는다.
	- 인터넷 네트워크 비용이 소모
		- 상대적으로 PC의 메모리, 하드디스크에 비해서 네트워크는 매우 느리고 비싸다.
	- 느린 사용자 경험
		- 브라우저 로딩 속도가 느리다.

![Pasted image 20250316235917.png](../img/Pasted%20image%2020250316235917.png)

- 캐시 적용
	- 웹 브라우저에서는 캐시 내부에 캐시를 저장하는 저장소가 있다.
		- 캐시 가능 시간이 유효하면 서버를 거치지 않고 캐시에서 바로 로딩
		- 캐시 가능 시간이 만료되면 서버를 통해 데이터를 다시 조회하고 캐시를 갱신한다.
		- `max-age`: 캐시 유효 시간 max-age=60  
			- 60초 동안 캐시를 사용하고, 60초가 지나면 서버를 통해 다시 조회한다.
	- 캐시 덕분에 캐시 가능 동안 네트워크를 사용하지 않아도 된다.
		- 비싼 네트워크 사용량을 줄일 수 있다 -> 브라우저 로딩 속도가 빠르다.(사용자 경험 향상)

#### 검증 헤더와 조건부 요청

1. 서버에서 기존 데이터를 변경
2. 서버에서 기존 데이터를 변경하지 않음

##### 캐시 시간 초과 (Last-Modified)

- 캐시 만료후에도 서버에서 데이터를 변경하지 않음
- 데이터를 전송하는 대신에 저장해 두었던 캐시를 재사용 할 수 있다.
- 단 ==클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법이 필요==하다.
- 서버에서 `Last-Modified` 혹은 `Etag`를 통해 검증 헤더를 제공한다.
	- `Last-Modified`: 마지막 수정 시간
	- `max-age` 만료시간이 지나면, 클라이언트는 서버에 다시 요청한다. 
	- 이때 `If-Modified-Since`헤더를 통해 서버에 데이터가 변경되었는지 확인한다.
		- `If-Modified-Since`는 이전에 서버에 요청했을 때 응답으로 받은 `Last-Modified` 값을 넣어서 요청 한다. 
		- 해당 시점 이후 데이터 변경 여부 확인
	- 서버는 데이터가 변경되지 않았다면 304 Not Modified를 응답한다.
		- 이때 응답 바디는 포함되지 않는다.
	- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신한다.
		- 캐시저장소에 저장된 데이터를 재사용한다.
	- 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드한다.

![Pasted image 20250317001715.png](../img/Pasted%20image%2020250317001715.png)

![Pasted image 20250317002640.png](../img/Pasted%20image%2020250317002640.png)

##### Last-Modified, If-Modified-Since 단점

- 1초 미만 단위로 캐시 조정이 불가능하다.
	- 초 단위로만 데이터 변경을 확인할 수 있다.
- 날짜 기반의 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 같은 경우
	- `Last-Modified` 날짜는 변경되어 있다.
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
	- 데이터가 바뀌지 않았는데, 계속해서 네트워크 다운로드가 발생한다.
	- 예) 스페이스나, 주석처럼 크케 영향이 없는 변경에서 캐시를 유지하고 싶은 경우

##### ETag, If-None-Match

- Etag(Entity Tag)
- 캐시용 데이터에 임의의 고유한 버전 이름을 달아둔다.
	- Etag: "v1.0", Etag: "a2j3a"
- 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
	- Etag가 같으면 데이터가 같다.
-  `max-age` 만료시간이 지나면, 클라이언트는 서버에 다시 요청한다
	- 이때 `If-None-Match` 헤더를 통해 서버에 데이터가 변경되었는지 확인한다.
    - `If-None-Match`는 이전에 서버에 요청했을 때 응답으로 받은 `Etag` 값을 넣어서 요청 한다.
    - 해당 데이터가 변경되었는지 확인
- ETag만 서버에 보내서 같으면 유지, 다르면 다시 응답 받는다.
- 캐시 제어 로직을 서버에서 완전히 관리
- 클라이언트는 단순히 이 값을 서버에 제공하고, 서버의 응답을 기다린다.
	- 예) 서버는 배타 오프 기간인 3일 동안 파일이 변경되어도 같은 Etag를 유지하도록 설정 가능
	- 애플리케이션 배포 주기에 맞추어 ETag 갱신

##### 캐시 제어 헤더

- Cache-Control: 캐시 제어
- Pragma: 캐시 제어(하위 호환)
- Expires: 캐시 만료일 지정(하위 호환)
	- 캐시 만료일을 정확한 날짜로 지정
	- HTTP 1.0부터 사용
	- 지금은 더 유연한 Cache-Control 사용을 권장
	- Cache-Control 우선 순위가 더 높다.
- Cache-Control: max-age=600
	- 600초(10분) 동안 캐시를 사용
	- 초 단위 사용
- Cache-Control: no-cache
	- 데이터는 캐시해도 되지만, 항상 원 서버(origin) 에 검증하고 사용
- Cache-Control: no-store
	- 데이터에 민감한 정보가 있으므로 저장하면 안된다.
	- 메모리에서 사용하고 최대한 빨리 삭제
- Cache-Control: public
	- 리소스를 캐시해도 되고, 공유해도 된다.
- Cache-Control: private
	- 개인 캐시에만 저장해야 한다.
- Cache-Control: no-transform
	- 프록시 등은 응답을 변환해서는 안된다.
- Cache-Control: must-revalidate
	- 캐시 만료 후 최초 검증을 서버에 요청해야 한다.
- Cache-Control: proxy-revalidate
	- 프록시 캐시 만료 후 최초 검증을 서버에 요청해야 한다.
- Cache-Control: max-age=0, no-cache, no-store, must-revalidate
	- 캐시를 사용하면 안된다.`









