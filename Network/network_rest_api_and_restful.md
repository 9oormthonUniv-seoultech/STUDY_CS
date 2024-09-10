# REST API, RESTful

### 목차
* REST
* REST 구성요소
* REST 특징
* RESTFUL
* RESTful API 작동 과정
* RESTful API 클라이언트 요청 구성 요소
* RESTful API 서버 응답 구성 요소
* RESTful API 설계 규칙
* RESTful API 인증 방법
* 예상 면접 질문
* 참고 자료
---

### REST

> Representational State Transfer

자원을 이름(표현)으로 구분해 해당 자원의 상태(정보)를 주고받는 모든 것을 말한다.



```jsx
1. HTTP URI를 통해 자원을 명시하고
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원에 대한 CRUD Operation을 적용하는 것
```
<br>

### REST 구성 요소

1. 자원(Resource) : HTTP URI
    - 모든 자원에 고유한 ID가 존재하며, 이 자원은 서버에 있다.
2. 자원에 대한 행위(Verb) : HTTP Method
    - HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공
3. 자원에 대한 표현(Representations) : HTTP Message Payload

<br>

### REST의 특징

1. Server-Client(서버-클라이언트 구조)
    - 자원을 제공하는 쪽은 Server, 요청하는 쪽은 Client.
    -  REST 서버는 API를 제공하고, 클라이언트는 인증이나 컨텍스트(세션, 로그인 정보 등)를 관리하는 구조로 **각자의 역할이 명확히 구분**되며, 의존성이 줄어 개발 내용이 명확해진다.

2. Stateless(무상태)
    - HTTP 프로토콜은 무상태 프로토콜이므로 REST도 무상태성을 가진다.
    - **클라이언트의 컨텍스트를 서버에 저장하지 않으며**, 세션과 쿠키와 같은 컨텍스트 정보를 신경 쓸 필요가 없다. 이로 인해 구현이 단순해진다.
    - 서버는 **각각의 요청을 완전히 별개의 것으로 인식하고 처리**한다. API 서버는 클라이언트의 요청만을 처리하며, 이전 요청이 다음 요청 처리에 영향을 미치지 않는다. 다만 DB 수정에 의한 변화는 허용된다.
    - 처리 일관성이 생기고, 서버 부담이 줄어 자유도가 높아진다.

3. Cacheable(캐시 처리 가능)
    - HTTP 프로토콜을 사용하므로 기존 웹 인프라의 캐싱 기능을 그대로 활용할 수 있다.
    - Last-Modified 태그나 E-Tag를 이용해 캐시를 구현할 수 있다.
    - 캐시 사용으로  **응답 시간이 빨라지고**, REST 서버 트랜잭션이 발생하지 않기 때문에  **전체 성능과 서버 자원 이용률이 향상**된다.

4. Layered System(계층화)
    - 클라이언트는 REST API 서버만 호출한다.
    - REST 서버는 **다중 계층**으로 구성될 수 있으며, 보안, 로드 밸런싱, 암호화 등을 추가해 유연성을 높인다. 프록시, 게이트웨이 같은 중간 매체도 사용할 수 있다.
5. Uniform Interface(인터페이스 일관성)
    - HTTP 표준을 따르는 한, **어떤 플랫폼에서도 사용 가능**하다.

<br>

### RESTful

> REST 아키텍처를 구현한 웹 서비스를 RESTful 웹 서비스라고 한다. 그러나 REST를 사용했다고 해서 모두 RESTful한 것은 아니며, **REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다**고 할 수 있다.

<br>

### RESTful API 작동 과정
REST의 아키텍처 스타일을 따르는 API

> API(Application Programming Interface) : 다른 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙
1. 클라이언트가 서버에 요청을 전송한다. 
2. 서버는 클라이언트를 인증하고, 요청을 수행할 권한이 있는지 확인한다. 
3. 서버가 요청을 수신하고 내부적으로 처리한다.
4. 서버는 요청 성공 여부와 요청한 정보를 클라이언트에 응답한다. 

<br>

### RESTful API 클라이언트 요청 구성 요소


1. URL (Uniform Resource Locator)

    서버는 고유한 리소스 식별자로 각 리소스를 식별하며, REST 서비스의 경우 URL을 사용해 이를 수행한다. URL은 클라이언트가 접근하고자 하는 특정 리소스에 대한 경로를 지정하며, 요청 엔드포인트라고도 한다.
<br>
2. 메서드

   클라이언트가 서버에 어떤 종류의 작업을 요청하는지 정의한다.
    
    - GET: 리소스를 조회
    - POST: 새로운 리소스를 생성
    - PUT: 기존 리소스를 업데이트
    - PATCH: 리소스의 부분 업데이트를 수행
    - DELETE: 리소스를 삭제
    
<br>

3. 헤더 (Headers)

   헤더는 클라이언트와 서버 간 교환되는 메타데이터이다. 

    - Content-Type: 요청의 본문에 포함된 데이터 형식 (예: `application/json`, `application/xml`)
    - Authorization: 인증 정보를 포함해, API 호출이 인증되었는지 확인 (예: 토큰)
    - Accept: 서버로부터 어떤 형식의 응답을 원하는지 지정 (예: `application/json`)

<br>

4. 쿼리 파라미터 (Query Parameters)

    URL에 추가적인 데이터를 전달하기 위해 사용된다.  `?query=value`형식으로 표현한다. 
    
    ```jsx
    [파라미터 유형]
    1. URL 경로의 일부로 사용하는 **경로 파라미터** (예: `/users/{id}`)
    2. 추가 정보를 요청할 때 사용하는 **쿼리 파라미터** (예: `/users?id=123`)
    3. 클라이언트 인증 정보가 쿠키에 저장될 때 사용되는 **쿠키** (쿠키는 헤더에서 전송됨)
     ```
<br>

5. 본문 Body

   POST, PUT, PATCH 요청에서는 리소스를 생성하거나 업데이트하기 위해 본문에 데이터를 포함한다. 주로 JSON, XML, Form-data 등의 형식으로 전송된다.

<br>

### RESTful API 서버 응답 구성 요소

1. 상태 코드 (Status Code) 

   HTTP 상태 코드는 서버가 클라이언트의 요청에 어떻게 응답했는지를 나타내며, 3자리 숫자로 표현된다. 
    
    - **200 OK**: 요청이 성공적으로 처리됨
    - **201 Created**: 새로운 리소스가 성공적으로 생성됨
    - **400 Bad Request**: 잘못된 요청 (클라이언트 측 오류)
    - **401 Unauthorized**: 인증되지 않음 (유효한 인증이 필요)
    - **404 Not Found**: 요청한 리소스가 없음
    - **500 Internal Server Error**: 서버 측에서 발생한 내부 오류
    
<br>

2. 헤더 Headers

    서버의 응답에 대한 메타데이터를 포함
    - **Content-Type** : 응답 데이터의 형식을 지정 (예: application/json, text/html)
    - **Cache-Control** :  클라이언트가 응답을 캐시할 수 있는 방법에 대한 규칙을 지정
    - **Set-Cookie** :  클라이언트에 쿠키를 설정할 때 사용
    
<br>

3. 메시지 본문 (Body)

    서버의 응답 데이터가 포함된 부분으로, 리소스 표현이 포함된다. 클라이언트는 데이터 작성 방식을 XML 또는 JSON 방식으로 정보를 요청할 수 있습니다. 
    ```jsx
    {
      "id": 123,
      "name": "John Doe",
      "email": "john@example.com"
    }
    ```

<br>

### RESTful API 설계 규칙

1. URI는 명사, 소문자를 사용해야한다.
    ```
    ❌ GET /GetUser/123
    ⭕ GET /users/123
    ```
    
2. CRUD 행위를 포함하지 않는다.
    ```
    ❌ POST /createUser
    ⭕ POST /users 
    ```
    
3. 경로 부분 중 변하는 부분은 유일한 값으로 대체한다. 
    ```
    ❌ GET /users/soojin
    ⭕ GET /users/123
    ```
    
4. 마지막에 슬래시(/) 포함하지 않는다.
    ```
    ❌ GET /users/
    ⭕ GET /users
    ```
    
5. 언더바 대신 하이픈을 사용한다.
    ```
    ❌ GET /user_groups
    ⭕ GET /user-groups
    ```
    
6. 파일 확장자는 URI에 포함하지 않는다. 헤더에 값을 넣는다
    ```
    ❌ GET /users/123.json
    ⭕ GET /users/123 (Accept 헤더에 application/json을 설정)
    ```
    
<br>

### RESTful API 인증 방법

클라이언트가 특정 리소스에 접근할 권한이 있는지를 확인하는 과정

1. **HTTP  기본 인증**
    
    - 사용자의 아이디와 비밀번호를 Base64로 인코딩해 요청에 포함한다.
    - 간단하고 빠르게 구현 가능하지만, Base64는 암호화가 아니기 때문에 쉽게 탈취될 수 있다.

<br>

2. **API Key 인증**

    - 서버가 제공한 고유한 API Key를 클라이언트가 요청에 포함하여 인증한다.
    - 구현이 간단하고 키 관리가 용이하나, 보안이 기본적이고 역할 기반의 세부 권한 관리는 어렵다

<br>

3. **OAuth (Open Authorization)**
    - 제3자 애플리케이션이 리소스 소유자의 자원을 안전하게 접근할 수 있도록 권한을 부여하는 표준 프로토콜
    - 주로 OAuth 2.0을 사용하며, 암호와 토큰을 결합해 안전한 접근을 보장한다. 토큰은 범위와 수명에 따라 관리가 가능하다.
    - 복잡하지만 매우 안전하고 권한 관리가 용이하다.

<br>


4. **JWT (JSON Web Token)**
    
    - 클라이언트가 인증 후 서버에서 서명된 토큰을 받아, 이후 요청 시 이를 제출하여 인증하는 방식
    - 상태를 유지하지 않는 방식으로, 효율적이고 보안성이 높다.

<br>

5. **Session 기반 인증**
    
    - 서버가 클라이언트 로그인 시 세션 ID를 생성하고, 이를 쿠키 형태로 전송한다. 이후 API 요청 시 쿠키에 세션 ID를 포함해 인증한다.
    - 서버 측에서 세션 정보를 저장하며 관리한다.
   
<br>

6. **Mutual SSL**
    - 클라이언트와 서버가 서로의 인증서를 교환하여 인증한다.
    - 보안성이 매우 높아 금융 서비스 등 고보안 환경에서 주로 사용된다.

<br>

### 예상면접 질문
- REST와 RESTful의 차이점에 대해서 설명해주세요
- RESTful API 설계에서 URI를 설계할 때 지켜야 할 주요 규칙에 대해 설명해주세요
-  REST에서 지원하는 HTTP 메서드에 대해서 설명해주세요


<br>

### 참고 자료 

[[Network] REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

[REST](https://www.incodom.kr/REST)
