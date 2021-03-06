# 1. 네트워킹
+ 두 대 이상의 컴퓨터를 케이블로 연결하여 네트워크를 구성하는것을 말한다.
+ 네트워킹을 구성하여 컴퓨터들은 연결된 상태로 데이터를 손쉽게 주고받거나, 자원프린터같은 주변기기를 함께 공유하고자 하는 노력에서 시작되었다.
+ 전세계의 컴퓨터들은 현재 인터넷이라는 거대한 네트워크를 구성하고 있다.
+ java.net패키지를 통해 네트워크App의 데이터 통신부분에 관한 여러 메서드들을 얻을 수 있다.

## 1.1 클라이언트/서버
+ Server는 서비스를 제공하는 컴퓨터이고, Clinet는 서비스를 사용하는 컴퓨터가 된다.
+ 서비스는 서버가 클라이언트로부터 요청받은 작업을 처리하여 그 결과를 제공하는것을 뜻한다.
+ 네트워크를 구성할때 전용서버를 두는 것을 서버기반모델(server-based model)이라 하고 별도의 전용서버없이 각 클라이언트가 서버역할을 동시에 수행하는 것을 P2P(peer-to-peer)라고 한다.

## 1.2 IP 주소
+ IP주소는 컴퓨터(호스트)를 구별하는데 사용되는 고유한 값으로 인터넷에 연결된 모든 컴퓨터는 IP주소를 갖는다.
+ IP주소는 다시 네트워크주소와 호스트주소로 나눌수 있는데, 이 둘이 각각 몇 bit를 차지하는지는 네트워크를 어떻게 구성하느냐에 따라 달라진다.
+ 그리고 서로 두 호스트의 IP주소의 네트워크 주소가 같다는 것은 두 호스트가 같은 네트워크에 포함 되어있다는 것을 의미한다.
+ IP주소와 서브넷마스크(cmd에서 ipconfig 실행시 제공)를 '&' 연산하면 네트워크 주소를 얻어낼 수 있다.

## 1.3 InetAddress
+ 자바에서는 IP주소를 다루기 위한 클래스로 InetAddress를 제공한다.

## 1.4 URL(Uniform Resource Locator)
+ URL은 인터넷에 존재하는 여러 서버들이 제공하는 자원에 **접근할 수 있는 주소를 표현**하기 위한 것으로 아래의 구조로 이루어져 있다.
```
http://www.codechobo.com:80/sample/hello.html?referer=javachobo#index1

프로토콜    자원에 접근하기위해 서버와 통신하는데 사용되는 통신규약(http)
호스트명    자원을 제공하는 서버의 이름(www.codechobo.com)
포트번호    통신에 사용되는 서버의포트번호(80)
경로명      접근하려는 자원이 저장된 서버상의 위치(/sample/)
파일명      접근하려는 자원의 이름(hello.html)
쿼리(query) URL에서 '?'이후의 부분으로 쿼리를 나타냄(referer=javachobo)
참조(anchor)URL에서 '#'이후의 부분(index1)
```
+ 자바에서는 URL을 다루기 위해 URL클래스를 제공한다.
+ 메서드는 책 참고

## 1.5 URLConnection
+ URLConnection은 APP과 URL간의 **통신연결을 나타내는 클래스**의 최상위 클래스로 추상클래스이다.
+ URLConnection을 사용해서 연결하고자하는 자원에 접근하여 읽고 쓰기를 할수 있다.

# 2. 소켓 프로그래밍
+ 소켓을 이용한 통신 프로그래밍을 뜻하며, 소켓이란 프로세스간의 통신에 사용되는 양쪽 끝단을 의미한다.
+ 자바에서는 java.net패키지를 통해 소켓프로그래밍을 지원하며, 소켓통신에 사용되는 프로토콜에 따라 다른 종류의 소켓을 구현하여 제공한다.

## 2.1 TCP와 UDP
+ TCP/IP 프로토콜은 이기종(시스템 아키텍쳐) 시스템간의 통신을 위한 표준 프로토콜로 프로토콜의 집합이다.
+ 어플리케이션의 특징에 따라 적절한 프로토콜을 선택하여 사용해야한다.(우리는 TCP와 UDP를 알아볼 예정)
+ TCP를 이용한 통신은 전화에, UDP를 이용한 통신은 소포에 비유된다. TCP는 데이터를 전송하기 전에 먼저 상대편과 연결을 한 후에 데이터를 전송한다. 그러므로 파일을 주고받는데 용이하다.
+ UDP는 상대편과 연결하지 않고 데이터를 전송하며, 제대로 전송되었는지 확인할 길은 없지만 빠른 전송이 가능하다.

## 2.2 TCP 소켓 프로그래밍
> 통신과정의 단계(그림참고 p964)
```
1. 서버 프로그램에서는 서버 컴퓨터의 특정포트에서  서버소켓을 사용하여 클라이언트의 연결요청을 처리할 준비를 한다.
2. 클라이언트 프로그램은 접속할 서버의 IP주소와 포트 정보를 가지고 소켓을 생성해서 서버에 연결을 요청한다.
3. 서버소켓은 클라이언트의 연결 요청을 받으면 서버에 새로운 소켓을 생성해서 클라이언트의 소켓과 연결되도록 한다.
4. 이제 클라이언트의 소켓과 새로 생성된 서버의 소켓은 서버 소켓과 상관없이 1대1 통신을 한다.
```
```
Socket        : 프로세스간의 통신을 담당하며, InputStream과 OutputStream을 가지고 있다. 이 두 스트림을 통해 프로세스간의 통신(입출력)이 이루어진다.
ServerSocket  : 포트와 연결(bind)되어 외부의 연결요청을 기다리다 연결요청이 들어오면, Socket을 생성해서 소켓과 소켓간의 통신이 이루어 지도록 한다.
              : 한 포트에 하나의 ServerSocket만 연결할 수 있다.(프로토콜이 다르면 같은 포트를 공유할 수는 있음)
```

## 2.3 UDP 소켓 프로그래밍
+ DatagramSocket과 DatagramPacket을 사용한다.
+ DatagramPacket은 헤더와 데이터로 구성되어 있으며, 헤더에는 DatagramPacket을 수신할 호스트의 정보(호스트의 주소와 포트)가 저장되어있다.(마치 소포의 주소와 같다)
