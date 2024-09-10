# OSI 계층

## 목차

* OSI 7 계층
* OSI를 나눈 이유
* OSI 7 계층 흐름
* OSI 층 
  * 1 계층 - 물리계층
  * 2 계층 - 데이터링크 계층
  * 3 계층 - 네트워크 계층
  * 4 계층 - 전송 계층 
  * 5 계층 - 세션 계층
  * 6 계층 - 표현 계층
  * 7 계층 - 응용 계층 
* 예상 면접 질문
* 참고 자료
---

### OSI란 
표준 프로토콜을 사용하여 다양한 통신 시스템이 통신할 수 있도록 국제표준화기구에서 만든 개념 모델이다.
> 실질적으로는 TCP/IP 모델이 사용된다. OSI는 개념적인 모델로 사용되고 있다. 

![](/Network/img/network_osi_layer_img.png)
### OSI를 나눈 이유 
흐름을 한눈에 알아보기 쉽고, 7단계 중 이상이 생기면 다른 단계를 확인할 필요 없이 해당 단계만 고칠 수 있기 때문이다. 


### OSI 7 계층 흐름 
![](/Network/img/network_flow.png)

> 각 층마다 자신의 로직을 처리하고 필요 정보를 헤더에 추가하여 **캡슐화**한다. 그리고 아래 단계에 넘겨준다. 
> 가장 끝 단계인 물리계층에 오면 데이터를 비트로 변환하여 케이블로 다음 노드에 전달한다. 노드가 라우터인 경우, 
> 네트워크 계층까지 올라가서 IP주소를 확인하고 다음 경로를 찾는다.
> 가장 효율적인 방식으로 목적지에 도착하면 다시 물리계층부터 시작하여 각 계층마다 자신의 헤더 정보들을 이용하여 데이터를
> **역캡슐화** 한다. 각 단계를 거쳐 마지막 응용 계층에서는 사용자가 이해할 수 있는 형태로 데이터를 제공한다. 

### OSI 7계층 

1. 물리계층(Physical Layer)<br>
    > 하드웨어적으로 데이터를 통신하는 단계이다.

    통신 단위는 비트이며 1,0으로 나타내어진다. 

    예) 두 컴퓨터간의 통신을 할 때는 케이블로 전기적인 신호를 보내고 받으면 된다. 여러 컴퓨터간의 통신을 해야 할 경우도 필요하므로, 이 경우 허브가 필요하다.


2. 데이터 링크 계층(Data Link Layer) 
    > 같은 네트워크에 있는 여러 장치들이 신뢰성 있게 데이터를 주고받을 수 있도록 도와주는 계층이다. 이 계층은 물리적 링크에서 데이터 전송을 제어하며, 데이터를 프레임 단위로 관리하고, MAC 주소를 사용해 장치를 식별한다. 또한, 오류 검출과 흐름 제어를 통해 데이터 전송의 신뢰성을 보장한다. 직접 연결된 노드 간의 통신을 담당한다. 
    
    **프레임이란?**

    데이터 링크 계층에서 데이터를 캡슐화하는 기본 단위이다. 
    
    헤더, 데이터, 트레일러로 구성되어있다.

    <br>

    **두가지 하위 계층**

    1. LLC 계층
    > 상위 Layer와 통신하는 Software 기능을 한다. 
    2. MAC 계층 
    > 하위 Layer와 통신하는 Hardware 기능을 한다.

    ![](/Network/img/network_data_link_img.png)

    **MAC 주소**

    MAC주소는 네트워크 인터페이스 카드(NIC)에 고유하게 할당된 물리적인 주소이다. 이 계층에서는 MAC주소 기반으로 통신을 한다. 

     
3. 네트워크 계층(Network Layer)<br>

    > 라우팅을 통해 다른 네트워크 간에 데이터를 전달하는 역할을 한다. 최적의 경로를 계산하여 여러 네트워크를 거쳐 목적지까지 전달될 수 있도록 한다. 

    라우팅 프로토콜은 여러 네트워크 간의 경로를 효율적으로 선택하는 알고리즘을 사용한다. 

    통신은 IP주소 기반으로 한다. 

    ![](/Network/img/network_network_layer.png)

    <br> 


4. 전송 계층(Transport Layer)<br>

    ![](/Network/img/network_transport_layer.png)

    > 계층 구조의 네트워크 구성요소와 프로토콜 내에서 송신자와 수신자를 연결하는 통신 서비스를 제공한다. 서로 다른 호스트에서 동작하는 애플리케이션 프로세스들 간의 논리적 통신을 제공한다.
    
    논리적 통신이란, 호스트들이 직접 연결된 것처럼 보인다는 의미이다. 

    주요 프로토콜(TCP/UDP)

    **TCP**

    신뢰성 있는 전송을 제공하는 프로토콜이다. 데이터를 순서대로 전달하며, 전송 중에 오류가 발생할 경우 이를 복하기 위한 메커니즘을 제공한다. 

    **UDP**

    신뢰성 없는 전송을 제공하는 프로토콜이다. TCP와 달리 연결을 설정하지 않고 데이터를 전송한다. 오류 제어나 흐름제어를 수행하지 않아서 속도가 매우 빠르지만, 신뢰성이 떨어진다. 
    예) 실시간 통신(동영상, 음악), 온라인 게임, 화상 회의

    **데이터 단위**

    TCP의 경우 **세그먼트**라고 한다. 순서 번호, 확인 응답 번호(ACK), 플래그(제어 플래그) 등이 포함되어 있다. 이를 통해 데이터 전송 상태를 추적하고, 손실된 세그먼트를 재전송하는 등의 기능을 수행한다. 

    UDP의 경우, **데이터그램**이라고 한다. 순서 번호, 오류검출, 복구 기능이 없다. 

    
    ![](/Network/img/network_transport_layer_2.png)
    <br>

5. 세션 계층(Session Layer)

    > 송신자와 수신자 간의 대화를 설정, 관리, 종료하는 역할을 한다. 네트워크에서 두 장치 간의 통신이 이루어질 때, 
    그 통신이 지속적으로 유지되고 필요한 경우 동기화되도록 관리한다. 

    <br>

6. 표현 계층(Presentation Layer)
    > 데이터의 형식 변환을 담당하는 계층이다. 서로 다른 형식의 데이터가 네트워크 상에서 일관성 있게 처리될 수 있도록,
    데이터의 인코딩, 디코딩, 변환 작업을 수행한다. 
    예) 암호화 복호화, 압축과 풀기, 데이터 형식 변환
    
7. 응용 계층(Application Layer)
   
    > 네트워크 서비스와 사용자 또는 애플리케이션 간의 인터페이스 역할을 한다. 사용자가 네트워크와 상호작용할 수 있도록 애플리케이션에 네트워크 기능을 제공하는 계층이다. 

    *네트워크 서비스: 웹, 이메일, 파일 전송 등을 뜻한다.*

    다양한 네트워크 프로토콜을 관리한다. 

    예) HTTP, FTP, SMTP, DNS 와 같은 다양한 주요 프로토콜이 있다. 


### 예상 면접 질문

* OSI 7 계층에 대해 설명해주세요.
* TCP와 UDP에 대해서 설명해주세요. 
* IP주소와 MAC주소에 대해 알고 계시나요?


### 참고 자료 
[[10분 테코톡] 🔮 히히의 OSI 7 Layer](https://www.youtube.com/watch?v=1pfTxp25MA8)

[[입문용] 프로토콜과 OSI 7 layer 설명! 네트워크의 기능들이 어떻게 구조화 돼서 동작하는지를 설명합니다! 👍](https://www.youtube.com/watch?v=6l7xP7AnB64)