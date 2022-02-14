## HTTP1.1 vs HTTP 2.0

**HTTP는 1.0버전부터 상용화 되어 사용되기 시작했다.**

### HTTP 1.0 단점
- HTTP 1.0에서는 open/close를 위한 flow의 제한으로 대역폭이 적게 할당되어 연결되는데, 이로 인해 혼잡 데이터가 자주 발생하고 disconnect가 반복적으로 나타나게 된다. 반복되는 disconnect 현상으로 인해 한 서버에 계속해서 접속을 시도하게 되면 과부하가 걸리고 성능이 떨어지게 되는 문제가 발생한다.

- TCP Connection당 하나의 URL만 fetch하며, 매번 request/response가 끝나면 연결이 끊기므로 필요할 때마다 다시 연결해야 해서 속도가 떨어진다.

- URL의 크기가 작고 한번에 가져올 수 있는 데이터의 양이 제한되어 있다.
 

### HTTP 1.1 특징

#### 1. 지속적 연결 : connectionless
HTTP 요청은 TCP 프로토콜을 이용하기 때문에 요청을 전송하기 위해선 3-way-handshake 과정을 거쳐 연결을 설정 해야한다. 하지만 매 요청마다 연결/해지 하는 과정에서 네트워크 지연을 일으킨다. 
HTTP 1.1 이전에는 keep-alive 헤더를 이용하여 일정 시간동안 연결을 끊지않고 여러 요청에 재사용 하였다. HTTP 1.1 부터는 기본적으로 keep-alive를 사용한다.

#### 2. Pipelining 
HTTP1.1는 기본적으로 연결당 하나의 요청과 응답을 처리하기 때문에 동시 전송 문제와 다수의 리소스를 처리하는데 있어 성능 이슈가 있었다.
하지만 HTTP 1.1 부턴 Pipelining 구조로 이런 문제를 해결하고자 했다. 하나의 커넥션에서 여러개의 요청과 응답을 받을 수 있으며 이전 요청의 완료를 기다리지 않고 다음 요청을 전송할 수 있다. 하지만 모든 요청에 Pipelining이 적용되는 것은 아니다. GET, HEAD, DELETE, PUT과 같은 멱등성이 보장되는 요청에만 적용된다.

Pipelining의 문제점도 있다. 하지만, 응답은 서버의 요청 순서에 맞게 반환되야 하기 때문에 앞선 요청에 대한 응답이 지연되는 상황이라면 그 이후 응답도 영향을 받는 Head Of Line Blocking 이 발생한다.

#### 3. 무거운 Header 구조 
http/1.1의 헤더에는 많은 메타 정보들이 저장되어 있다. 클라이언트와 서버 간에 수 많은 http 요청이 발생하게 되는데 매 요청마다 중복된 헤더를 계속 보낼 뿐 아니라 도메인에 설정된 cookie 정보도 헤더에 포함되어 전송된다. 전송하려는 값보다 헤더 값이 더 큰 경우도 자주 발생


#### HTPP 1.1 단점 극복 방법
- Image Spriting
웹페이지를 구성하는 다양한 아이콘 이미지 파일의 요청 횟수를 줄이기 위해 아이콘을 하나의 큰 이미지로 만든다음 CSS에서 해당 이미지의 좌표 값을 지정해 표시.

- CSS/JavaScript 압축
http를 통해서 전송되는 데이터의 용량을 줄이기 위해 CSS, Javascript 코드를 축소하여 적용.

- Data URI Scheme
HTML 문서 내에 이미지 리소스를 Base64로 인코딩된 이미지 데이터로 직접 기술하는 방법으로 이를 이용하여 서버로의 요청을 줄이는 방식이다.

- Domain Sharding
요즘 브라우저들은 http 1.1 단점을 극복하기 위해 도메인 명을 다르게 하고 리소스를 다르게 저장한 후, 다수의 Connection을 생성해서 병렬로 요청을 보내기도 한다. 하지만 브라우저 별로 Domain당 Connection개수의 제한이 존재하기 때문에 근본적인 해결책은 아니다.


### HTTP 2.0 특징
웹페이지가 예전의 텍스트 위주와는 다르게 점점 미디어들이 추가되고 데이터 요청/응답량이 많아지니 대역폭을 늘리는데 한계가 생겼고 성능 개선이 반드시 필요하게 되었다. 기존 HTTP 1.1의 성능 쪽 개선에 초점을 맞춘 프로토콜이다.

#### 1. Multiplexed Streams
HTTP 1.1의 HTTP Pipelining 의 개선안으로 하나의 Connection으로 동시에 여러 개의 메세지를 주고 받을 수 있습니다.

HTTP 메시지를 바이너리 형태의 프레임으로 나누고 이를 전송 후 받은 쪽에서 다시 조립한다. 요청과 응답이 동시에 이루어지니 하나의 연결에 여러 요청과 응답이 뒤섞여 있다. 프레이밍 작업은 서버와 클라이언트(브라우저)에서 해주기 때문에 큰 변경사항을 고려하지 않아도 된다. 

파싱과 전송 속도가 상승하고 오류 발생 가능성도 줄어든다. 또한 응답은 요청 순서에 상관없이 Stream으로 받기 때문에 HOL Blocking 도 발생하지 않는다.

#### 2. Stream Prioritization
응답에 대한 우선순위를 정해 우선순위가 높을수록 응답을 빨리 한다.
예를들어 클라이언트가 요청한 HTML 문서 안에 CSS 파일 1개와 Image 파일 2개가 존재하는데, 만약 Image 파일보다 CSS파일의 수신이 늦어지는 경우 브라우저 렌더링이 늦어질 수 있다. 이때 리소스간의 우선순위를 정함으로써 해결이 가능하다.

#### 3. Server Push
서버는 클라이언트가 요청하기 전에 필요한 리소스를 Push 해주는 방법으로 클라이언트의 요청을 최소화 해서 성능 향상을 이끌어 낸다.
예를 들어, HTTP/1.1에서 클라이언트(브라우저)가 요청한  HTML문서를 요청했고 수신한 후 HTML문서를 해석하면서 포함되어 있는 여러개의 리소스(CSS, Image 등)를 재요청한다. HTTP/2.0에선 Server Push 기법을 통해 클라이언트가 요청을 하지 않아도 필요한 리소스를 Push해준다.


#### 4. Header Compression
HTTP 1.1의 경우 이전 요청과 중복되는 Header도 똑같이 전송하느라 네트워크 자원을 불필요하게 낭비하였다. HTTP 2.0에서는, Header Table과 Huffman Encoding을 사용하는 HPACK 압축방식으로 이를 개선하였다. (Overhead 최소화)

클라이언트와 서버는 각각 Header Table을 관리하고 이전 요청과 동일한 필드는 table의 index만 보내고, 중복되지 않는 값은 Huffman Encoding 후 보냄으로서 Header의 크기를 경령화 하였다. (기존에 HTTP 1.1 은 Header가 Plain Text(평문))

+) Stream : 양방향 통신을 통해 전달되는 한개 이상의 메시지 즉, 프레임이 여러개가 모여 메시지가 되고 메시지가 여러개가 모여 스트림이 된다.

+) Huffman Coding 방식: 데이터 문자의 빈도에 따라서 다른 길이의 부호를 사용하는 알고리즘

### HTTP 2.0 의 한계
- 서버에서 HTTP/2를 지원한다고 해도 클라이언트에서 HTTP/2를 지원하지 않는 다면 사용 할 수가 없기에 아직까지 PC용 웹페이지에서 HTTP/2 사용은 미지원 클라이언트(브라우저)가 많아  한계가 있다. 하지만 모바일 Device에서 구동되는 대 부분 브라우저는 HTTP/2를 지원하고 있다.

- tcp 프로토콜 자체의 Head of line Bloking 문제는 여전히 남아 있다.