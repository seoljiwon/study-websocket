# Noom

Zoom Clone using NodeJS, WebRTC and Websockets.

<br/>

## 1. HTTP vs Websocket
HTTP는 client가 요청을 보내면 server가 응답하는 단방향 통신이다.
  - HTTP Method(GET, POST, PUT, DELETE)를 통해 통신한다.

Websocket은 client와 server 모두 메세지를 전송할 수 있는 양방향 통신이다.
  - websocket에서는 event를 send 하고 listen 하는 방식으로 통신한다.
  
<br/>

## 2. WebRTC란?
Web Real-Time Communications으로, 브라우저 간 Peer to Peer 통신으로 음성, 영상, 텍스트 등 데이터를 주고 받을 수 있는 기술이다.

<br/>

### Signaling이란? 
RTCPeerConnection이 데이터를 주고 받을 수 있도록 하는 협의(negotiation)이다.

<img width="550" alt="스크린샷 2022-12-07 오전 9 02 37" src="https://user-images.githubusercontent.com/76513385/206818235-43733eaa-d8c4-488f-8918-ee74722ef509.png">

Signaling 과정
1) SDP Offer, Answer 교환
  - SDP(Session Description Protocol): 스트리밍 미디어의 초기화 인수를 기술하기 위한 포맷
2) ICE Candidate 교환
  - ICE(Interactive Connectivity Establishment): peer를 통한 연결이 가능하도록 하게 해주는 프레임워크
  
<br/>

### STUN server란?
STUN은 Session Traversal Uilities for NAT로, client는 자신의 public address를 확인하기 위해 STUN server에 요청을 보낸다.
P2P 통신을 하려면 서로의 위치를 알아야 하기 때문에 이러한 server가 필요하게 된다.

여기서 NAT는 private IP를 public IP로 변경하는 데 필요한 주소 변환 서비스로 다수의 private IP를 하나의 public IP로 변환한다. (public IP는 같으나 port 번호를 달리하여 host를 구분한다.) NAT는 public address 절약 혹은 보안 목적으로 사용한다.

STUN server로 찾지 못하는 경우에는 TURN server를 이용한다.

<br/>

### WebRTC의 한계
Peer가 늘어날수록 Mesh 구조를 띄기 때문에 매우 복잡해지고, upload와 download에 있어 모두 비효율이 발생한다.
이럴 때는 SFU(Selective Forwarding Unit) 방식을 사용할 수 있는데 중앙 서버를 두고 upload와 download를 중계하도록하는 것이다.

하지만, DataChannel을 이용하는 것은 괜찮다.

위의 한계점은 audio 혹은 video 같이 무거운 data를 공유할 때 고민하게 되는데, text 정도 크기의 data는 Mesh 구조여도 이용하는 데 무리가 없다.

-> Server를 가져도 괜찮다면 Socket IO를, P2P 통신을 하고 싶다면 DataChannel을 이용해서 chat 기능을 구현할 수 있다.
