### 인터넷 네트워크

- 인터넷 통신
- IP(Internal Protocol)
	- IP(인터넷 프로토콜)
		- 지정한 IP 주소(IP Address)에 데이터 전달
		- 패킷 이라는 통신 단위로 데이터를 전달
			- 출발지IP, 도착지IP, 메시지 등으로 구성
	- IP 프로토콜의 한계
		- 비연결성: 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
			- 프로토콜은 IP 대상 서버가 패킷을 받을 상태인지 알 수 없다.
		- 비신뢰성: 중간에 패킷이 사라지거나, 패킷이 순서대로 오지 안는 경우
		- 프로그램 구분: 같은 IP를 사용하는 서버에서 통신하는 어플리케이션이 둘 이상이면?
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
![[Pasted image 20250106194347.png]] 

![[Pasted image 20250106195302.png]]
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
### URI와 웹 브라우저 요청 흐름
#### URI(Uniform Resource Identifier)

**URI**(Uniform Resource Identifier)는 하나의 리소스를 가리키는 문자열이다.
가장 흔한 URI는 URL로, 웹 상에서의 위치로 리소스를 식별한다. 반면, URN은 주어진 이름공간 안의 이름으로 리소스를 식별한다.

#### URL(Uniform Resource Locator)

URL은 웹에서 주어진 고유 리소스 주소이다. 이론적으로 각각의 유효한 URL은 고유한 리소스를 가리킨다. 이러한 리소스는 HTML 페이지, CSS 문서, 이미지 등이 될 수 있다. URL로 표시되는 리소스와 URL 자체는 웹 서버에서 처리 된다.

![[Pasted image 20250107210646.png]]



### 브라우저에서 렌더링 과정

브라우저의 핵심 기능은 필요한 리소스(HTML,CSS,자바스크립트,이미지,폰트)등의 정적 파일 또는 동적으로 생성한 데이터를 서버에 요청하고 응답 받아 브라우저에 시각적으로 렌더링하는 것이다.

- 브라우저의 주소창에 URL 입력 -> 호스트 이름이 DNS를 통해 IP주소로 변환 -> 이 주소를 갖는 서버에게 요청을 전송

1. **사용자가 웹 브라우저를 통해 google.com 을 입력하면 URL 주소 중 도메인 네임 부분을 [DNS](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-dns) 서버에서 검색.**
2. **DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 [URL](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-url) 정보와 함께 전달.**
3. **브라우저는 [HTTP](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-http) [프로토콜](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-프로토콜)을 사용하여 요청 메시지를 생성하고 HTTP 요청 메시지는 [TCP](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-tcp)/[IP](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-ip) 프로토콜을 사용하여 서버로 전송.**
4. **서버는 [response](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-response) 메시지를 생성하여 다시 브라우저에게 데이터를 전송합니다.**
5. **브라우저는 response를 받아 [파싱](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-파싱)하여 화면에 [렌더링](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/what-happens-when-type-google.md#gear-렌더링)한다.**

- DNS(**Domain Name System**): 도메인 이름 시스템은 사용자가 입력한 도메인을 머신이 읽을 수 있는 IP주소로 변환하는 시스템이다.
- URL(Uniform Resource Locator): URL은 통합 자원 지시자로 인터넷의 리소스를 가르키는 표준명칭으로 서버의 자원을 요청할때 사용된다.URL을 통해 인터넷 상의 모든 리소스를 요청할 수 있으며, HTTP, FTP 등의 자원 요청도 가능하다.
- HTTP(HyperText Transfer Protocol): HTTP는 TCP기반의 클라이언트와 서버 사이에 이루어지는 요청/응답 프로토콜이다.HTTP는 Text Protocol로 사람이 쉽게 읽고 쓸 수 있습니다. 프로토콜 설계상 클라이언트가 요청을 보내면 반드시 응답을 받아야 합니다. 응답을 받아야 다음 request를 보낼 수 있다.
- TCP (**T**ransmission **C**ontrol **P**rotocol)은 두 개의 호스트를 연결하고 데이트 스트림을 교환하게 해주는 중요한 네트워크 프로토콜이다. TCP는 데이터 전송을 제어하고 데이터를 어떻게 보낼지, 어떻게 맞출지를 정한다.또한 데이터와 패킷이 보내진 순선대로 전달 하는 것을 보장해준다.신뢰성과 연결성을 책임지기 위한 프로토콜이며, 호스트와 호스트간의 데이터 전송은 IP(인터넷 계층 프로토콜)에 의지하면서 동시에 신뢰성 있는 전송에 대해서는 TCP가 책임지는 구조이다.



