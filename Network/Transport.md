# The Transport Layer

네트워크 계층은 한 엔드 시스템에서 다른 엔드 시스템으로 메시지를 전송하는 역할을 하고, 전송 계층은 엔드 시스템에서 실행되는 애플리케이션에서 애플리케이션으로의 전송하는 역할을 한다.

### TCP
- 메시지(세그먼트)를 신뢰성있게, 순서대로 전송한다.
- 패킷이 전송되는 동안 생긴 변경을 감지하고 수정한다.
- 한 번에 적절한 양의 데이터를 보내 트래픽의 양을 조절한다.
- TCP를 사용하는 프로토콜 : HTTP, E-mail, File Transfers

### UDP
- 메시지(데이터그램)이 순서대로 전송되는 것을 보장하지 않는다. (데이터가 보내지는 것 또한 보장하지 않음. 신뢰성 X)
- 패킷이 전송되는 동안 생긴 변경을 감지하지만 수정하지는 않는다.
- TCP보다 가볍고 빠르다.
- UDP를 사용하는 프로토콜: Domain Name System (DNS), live video streaming, and Voice over IP (VoIP).

## 네트워크 계층의 한계
1. 페킷은 전송 오류로 인해 corrupt될 수 있다 → checksum
2. 패킷은 중간에 유실될 수 있다 → retrasmission timer
3. 패킷은 순서가 바뀌거나 중복해서 올 수 있다 → sequence number

### Checksum
패킷은 전송 오류로 손상될 수 있다. 이런 오류를 감지하기 할 수 있도록 전송자는 패킷의 모든 바이트의 합으로 계산된 체크섬을 세그먼트에 포함해서 보낸다. 수신자는 수신한 메세지로 체크섬을 계산하고 세그먼트에 포함된 체크섬과 비교해서 세그먼트가 유효한지 확인한다.

### Retrasmission timer
전송자는 세그먼트를 보낼 때 재전송 타이머를 시작하고 타이머가 끝날 때까지 ack 세그먼트가 오지 않으면 데이터가 유실됐다고 판단하고 해당 세그먼트를 재전송한다.
- 재전송 타이머의 값은 round-trip-time(세그먼트 전송 후 ack 세그먼트가 오기까지의 지연 시간)보다 커야한다.

### Sequence Numbers
전송 계층에서 각 세그먼트는 sequence number로 식별될 수 있다. sequence number는 세그먼트에 포함되어 전송된다.

## [The User Datagram Protocol](https://datatracker.ietf.org/doc/pdf/rfc768.pdf)
- IP 기반으로 동작하는 비연결성 프로토콜
    - TCP와 같이 핸드쉐이킹을 통한 연결을 하지 않음
- IP에 최소한의 기능(포트, 체크섬)만 더함

### Header & Data
- 출발지 port number : `2 Bytes`
- 도착지 port number : `2 Bytes`
- Length, 데이터그램의 길이(헤더 + 데이터) : `2 Bytes`
- 체크섬 : `2 Bytes`
- 데이터 : `65,528 bytes`
   - 데이터그램의 가능한 길이가 Length 헤더에 의해 2^16인 65,536 bytes로 제한되므로 데이터의 크기는 헤더의 크기를 제외한 65,528 bytes
  
![image](https://user-images.githubusercontent.com/37496919/151689694-65168c98-7dea-467c-ad5c-bd76fdbeace9.png)

## The Transmission Control Protocol
- IP 기반으로 동작하는 연결지향 프로토콜
- 네트워크 정체를 일으키지 않도록 데이터를 적절한 속도로 전송한다. (Congestion Control)
- 애플리케이션 계층에서 전달받은 데이터를 적절한 크기의 세그먼트로 쪼갠다.
- 수신자가 과부하되지 않도록 적절한 양의 데이터를 전송한다. (Flow Control)
- 전달되지 않은 메세지를 식별하고 재전송한다.
- 순서에 맞지 않은 메세지를 식별하고 재조합한다.

### Header
헤더의 크기는 `20~60Bytes`이다 
- 출발지 port number : `2 Bytes`
- 도착지 port number : `2 Bytes`
- sequence number : `4 Bytes`. 데이터의 byte마다 sequence number를 갖는데 헤더의 sequence number는 첫 번째 byte를 의미한다.
- acknowledgment number : `4 Bytes`. (송신자가 보낼, 수신자가 받을 것으로 기대되는) 다음 세그먼트의 seqnece number를 의미한다.
    - acknowledgment number를 통해 세그먼트가 순서에서 벗어나거나 손실(missing)됐음을 알 수 있다.
- flags: `8Bits`
    - CWR, ECN, URG, ACK, PSH, RST, SYN, FIN
    - CWR: Congestion Window reduced. 정체에 대응해서 window size를 줄인다.
    - ECN: Explicit Congestion Notification. 정체가 발생했음을 알린다.
    - URG: Urgent.
    - ACK: 이전에 세그먼트를 수신했음을 인정(acknowledge)할 떄 ACK 플래그와 함께 적절한 acknowledgment number를 설정하여 보낸다. 
    - PSH: Push. 버퍼의 내용 즉시 푸시하여 어플리케이션으로 전달.
    - RST: 연결을 즉시 종료한다.
    - SYN: 새로운 호스트와 연결을 시작한다.
    - FIN: 호스트와의 연결을 종료한다.
<img width="412" alt="image" src="https://user-images.githubusercontent.com/37496919/156108275-c7edd997-7154-4cc0-b10e-b3b3f256f82f.png">

