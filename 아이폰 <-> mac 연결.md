아이폰 <-> Mac 화면공유 테스트 환경 구축

1. ForticClientVPN 설치

- 아이폰 VPN 설정 체크
- VPN -> add Configuration 클릭
- 이름,host,port 다음과 같이 입력 후 생성
  - Name: NEW-Mrblue
  - Host: 220.118.169.70
  - Port: 10443
- VPN 으로 돌아와 연결
- 연결 시 민호책임님한테 발급받은 아이디/비밀번호 입력
- ForiToken으로 발급받은 OTP 입력
- VPN 연결
- [VNC 내용 참조](https://ko.aiseesoft.com/how-to/control-computer-from-iphone.html)

2. realVNC Viewer앱 설치

- +버튼 클릭
- address에 본인 PC IP입력
  - Name은 상관없음
- 연결
- 연결 시 유저네임/비밀번호는 본인 컴퓨터 접근권한을 설정한 사용자 계정으로 입력
  - 원격접속 mac 세팅 [민호 책임님 댓글 참조](https://gw.mrblue.com/app/board/10/post/5431)
  - [화면 공유 설정](https://support.apple.com/ko-kr/guide/mac-help/mh11848/mac)
  - <img src="/Users/khg/Library/Application Support/typora-user-images/image-20240514153011044.png" alt="image-20240514153011044" style="zoom: 50%;" />