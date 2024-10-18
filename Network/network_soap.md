

## SOAP(Simple Object Access Protocol)란?

- SOAP는 REST API가 대중화되기 전 널리 사용되었던 프로토콜
- SOAP로 설계된 API는 XML 메시지 형식을 사용하여, 일반적으로 HTTP, HTTPS, SMTP를 통해 요청을 수신

## SOAP 통신 방식
![alt text](/Network/img/network_soap.png)


### 구성 요소

- 서비스 요청자(Client)
- 서비스 브로커(UDDI)
    - 서비스 제공자와 요청자 사이에 존재하며, 서비스 등록 및 검색, 저장, 관리하는 주체
    - 클라이언트가 웹 서비스를 찾고 사용할 수 있도록 도와주는 **중개자** 역할
- 서비스 제공자(Server)

### WSDL(Web Services Description Language)란?

- 웹서비스가 기술된 정의 파일의 총칭으로 XML을 사용해 기술됨
- **웹서비스의 구체적인 내용이 기술되어 있는 문서**이고, 서비스 제공장소, 서비스 메세지 포맷, 프로토콜들이 기술되어 있음
- 해당 문서를 보고 Client는 서비스 위치, 필요한 파라미터 등의 정보를 알아낼 수 있음
- 구성요소
    
![alt text](/Network//img/network_wsdl.png)
    
    - `Types` : 교환될 메세지 설명, 사용될 데이터 형식 정의
    - `Interface` : operation(함수)정의 (input과 output)
    - `Binding` : Interface에 정의된 작업에 대해 메세지 형식과 프로토콜 정의 (클래스화)
    - `Service` : WebService URL endpoint정의
    - 예시 코드: https://raw.githubusercontent.com/GRuuuuu/temprepo/main/Files/hololy/220524-soap-rest/wsdl-2-sample.xml

### UDDI(Universal Description, Discovery and Integration)란?

- 위의 WSDL을 담아두는 public registry
- 전역 저장소이기 때문에 공개적으로 접근하고 WSDL을 검색할 수 있음
- IBM, MS에서 공용, 사설 UDDI 저장소 운영 중이며, 많은 회사가 자신의 웹서비스를 등록하고 있음

### 동작 프로세스

> **사용자는 UDDI를 통해 자신이 원하는 웹 서비스를 검색하고, 서비스에 대한 파라미터나 리턴 타입 등의 자세한 내용을 알아낸 다음, SOAP 메시지의 형태로 HTTP 프로토콜을 사용하여 통신**
> 
1. 서비스 제공자는 UDDI에 사용가능한 WSDL 등록
2. 서비스 사용자는 원하는 서비스를 위해 UDDI를 검색
3. 원하는 서비스에 대한 WSDL 다운로드
4. WSDL 문서를 처리하여 적절한 인터페이스에 맞게 SOAP메세지 작성
5. HTTP를 통해 서비스 요청
6. 서비스 제공자는 요청 값을 디코딩 -> 적절한 서비스 로직 수행
7. 결과값을 SOAP메세지로 작성 후 HTTP를 통해 요청자에게 반환

## SOAP Message 구조

![alt text](/Network//img/network_soap_message.png)

- 메시지를 포함하는 Envelope와 첨부 파일인 Attachment로 구성되며, Envelope는 Header와 Body로 구성
- **`SOAP Envelope`**
    - **`Header`**: 선택적 요소로 메시지 처리와 관련된 속성 정보나, 메타데이터를 포함
    - **`Body`**: 필수 요소로 실제 메시지 데이터가 포함된다. 요청이나, 응답이 해당 내용이 된다.
- **`Attachment`**
    - 첨부 파일을 의미하고, 메시지에 담을 수 없는 큰 파일들이 해당 위치에 존재
- 예시

```jsx
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Header>
        <!-- 헤더는 선택적으로 사용되며, 보안, 트랜잭션 등의 정보를 포함할 수 있습니다. -->
    </soap:Header>
    <soap:Body>
        <!-- 'GetPrice' 메소드를 호출하고, 'productId' 파라미터를 전달합니다. -->
        <m:GetPrice xmlns:m="http://www.example.org/stock">
            <m:ProductID>12345</m:ProductID>
        </m:GetPrice>
    </soap:Body>
    <!-- 만약 오류가 발생하면, Fault 요소가 여기 포함됩니다. -->
</soap:Envelope>
```

### SOAP과 REST비교

- **SOAP :**
    - 그 자체로 프로토콜이며, 보안이나 메시지 전송 등에 있어서 REST보다 더 많은 표준들이 정해져있기 때문에 조금 더 복잡
    - 이러한 표준들로 인해 오버헤드가 많지만, 보안, 트랜잭션, ACID(원자성, 일관성, 고립성, 지속성)을 준수해야 하는 종합적인 기능이 필요한 조직에게는 적합
    - SSL, WS-Security를 지원하여 보안수준이 엄격
    - ACID를 준수하기 때문에 데이터의 변형을 줄여주고, 데이터베이스와의 상호작용에 대해 사전에 정확하게 정하기 때문에 데이터읭 무결성을 지켜줌
    
    → 은행용 모바일 앱처럼 보안 수준이 높아야 하거나, 신뢰할 수 있는 메시징 앱, 또는 ACID를 준수해야 하는 경우 SOAP 방식이 더욱 선호됨 
    
- REST
    - 인터넷 식별자(URI)와 HTTP 프로토콜을 기반으로 하는 아키텍처 스타일로 **HTTP 메서드를 활용하여 구현이 간단하고 가벼움**
    - 단점으로는 SOAP에 비해 **보안 기능이 덜 내장**되어 있음
    - 또한, SOAP처럼 정해진 표준이 없어, 구현 방식에 따라 일관성이 없을 수 있음

|  | REST | SOAP |
| --- | --- | --- |
| 유형 | 아키텍처 스타일  | 프로토콜 |
| 작성방식 | 데이터 위주: 데이터를 위해서 리소스에 접근  | 기능 위주: 구조화된 정보 전송  |
| 전송데이터 | 유연한 데이터 형식  (html, json, text) | XML 데이터(고정) |
| 데이터 캐시 | 캐시를 사용할 수 있음 | 캐시를 사용할 수 없음(모든 요청이 POST로 감. POST 요청은 캐싱 불가능) |
| 보안 | WS-Security 보안 프로토콜을 지원하여 **메시지의 무결성, 기밀성을 인증,  SSL을 지원** | SSL 지원, HTTPS 통한 **전송 계층 보안** 제공, 메시지 자체에 보안을 적용하기 보다는 전송 과정에 의존  |
| 복잡성  | 고급 기능 및 복잡성 지원(트랜젝션을 안전하게 처리하는 프로토콜 지원) | 가볍고, 간단함  |
| 상태성 | 무상태성(Stateless) | 상태를 유지할 수 있음  |
| ACID 준수 | 자체적인 ACID 기준이 있어서 데이터 손상을 줄여줌  | ACID 준수와 관련된 내용 없음  |

> 정리: 복잡한 보안 및 트랜잭션 관리가 필요하다면 SOAP이 적합, 빠르고 가벼운 통신이 필요하다면 REST를 사용하는 것이 일반적임!

### 예상 면접 질문
1. SOAP와 REST의 차이점에 대해 설명하고, 각각의 장단점을 말해주세요.
2. WSDL과 UDDI의 역할에 대해 설명하고, SOAP 웹 서비스를 호출할 때 이들을 어떻게 활용하는지 말해주세요.
3. 보안이 중요한 시스템에서 SOAP과 REST 중 어느 것을 선택할지 설명하고, 그 이유를 말해주세요.
   
### 참고자료
https://yozm.wishket.com/magazine/detail/119/

https://velog.io/@scroll0908/RESTful-API-SOAP

https://velog.io/@imkkuk/Protocol-SOAP

[https://velog.io/@lenyleny/API가-뭔지-아는데-설명을-못한다면](https://velog.io/@lenyleny/API%EA%B0%80-%EB%AD%94%EC%A7%80-%EC%95%84%EB%8A%94%EB%8D%B0-%EC%84%A4%EB%AA%85%EC%9D%84-%EB%AA%BB%ED%95%9C%EB%8B%A4%EB%A9%B4)

https://www.youtube.com/watch?v=5o1IiHuUxPk

[https://gogetem.tistory.com/entry/REST-API-vs-SOAP-API-정의ㅣ차이-완벽정리](https://gogetem.tistory.com/entry/REST-API-vs-SOAP-API-%EC%A0%95%EC%9D%98%E3%85%A3%EC%B0%A8%EC%9D%B4-%EC%99%84%EB%B2%BD%EC%A0%95%EB%A6%AC)