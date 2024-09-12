## DNS

DNS (Domain Name System) : 인터넷 전화번호부. 도메인 이름과 IP주소가 서로 매핑되어 저장되어있다.

→ 도메인 주소를 IP주소로 변환해서 브라우저가 인터넷 리소스를 로드할 수 있도록 도와준다.

### DNS 동작 방식

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/be0198d7-dcc4-468c-b22d-abd7eefc4914/613a94ae-9fa6-4f86-b3c5-c9969f2444ba/image.png)

### DNS 서버의 종류

1. 순환 DNS 서버 (DNS Recursive Server/DNS resolver) : 요청받은 도메인에 매치되는 IP주소를 찾기위해 계층적으로 DNS 쿼리를 수행
2. 루트 DNS 서버 (Root Name Server) : 순환 DNS 서버에서 처음으로 DNS 쿼리 요청을 보내는 서버. 해당 도메인의 확장자 (.com, .net, .org 등 )에 따른 TLD 네임서버의 주소를 DNS Resolver에 응답해준다.
3. 최상위 도메인 DNS 서버 (TLD Name Server) : 최상위 도메인 (.com, .net, .org 등)에 대한 DNS 정보를 관리한다.
4. 권한 네임 서버 (Authoritative Name Server) : 도메인의 IP주소를 응답해주는 최종단계 DNS 서버이다. 여기서 얻은 IP주소가 순환 DNS 서버를 거쳐 브라우저까지 전달된다.

## 웹 통신 흐름

### 작동 방식

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/be0198d7-dcc4-468c-b22d-abd7eefc4914/f7236105-dcf0-415d-86a5-1558bcac5167/image.png)

1. 사용자가 브라우저에 도메인 이름을 입력한다.
2. DNS서버에서 사용자가 입력한 Domain name을 검색하고, 매핑되는 IP주소를 찾는다. 사용자가 입력한 URL 정보와 함께 리턴한다.
3. IP주소는 HTTP 프로토콜을 이용해서 HTTP 요청 메시지를 생성한다.
4. 생성된 HTTP 요청 메시지는 TCP 프로토콜을 사용해서 인터넷을 거쳐 해당 IP 주소의 컴퓨터(서버)로 전송된다. - 네트워크 패킷을 캡슐화 과정을 거친다.
5. 서버는 클라이언트의 요청을 승인하고, 응답 메시지를 전송한다. - 네트워크 패킷을 역캡슐화하는 과정을 거친다
6. 도착한 HTTP 응답 메시지는 HTTP 프로토콜을 사용하여 웹페이지 데이터로 변환되고, 웹 브라우저의 출력에 의해 사용자가 볼 수 있다.

### DNS 서버의 주소 찾기

DHCP (Dynamic Host Configuration Protocol)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/be0198d7-dcc4-468c-b22d-abd7eefc4914/25586c33-8223-487a-addb-f837d658795e/image.png)

- 호스트의 IP주소와 TCP/IP 설정을 클라이언트에 의해 자동으로 제공하는 응용 계층 프로토콜
  → 사용자는 DHCP 서버에서 자신의 IP 주소, 가장 가까운 라우터의 IP 주소, 가장 가까운 DNS 서버의 주소를 받는다.

ARP (Address Resolution Protocol)

- 네트워크상에서 IP 주소를 물리적 네트워크 주소로 바인딩시키기 위해 사용되는 프로토콜
- DHCP로 얻은 라우터의 IP 주소를 MAC 주소로 변환

## 예상 면접 질문

1. DNS를 왜 사용하는지?
2. 대략적인 웹 통신의 흐름에 대해 설명해보시오
3. DHCP를 사용하는 이유

### 참고자료

1. [https://velog.io/@woo0_hooo/네트워크-웹-통신의-흐름](https://velog.io/@woo0_hooo/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%9B%B9-%ED%86%B5%EC%8B%A0%EC%9D%98-%ED%9D%90%EB%A6%84)
