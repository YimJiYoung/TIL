# 네트워크 인터페이스층
같은 네트워크 내의 인터페이스 간 데이터 전송을 담당한다. 다른 네트워크에 접속된 서버까지의 데이터 전송은 같은 네트워크 내의 전송을 반복하면서 이루어진다.
- PC -> 같은 네트워크 상의 다음 라우터 -> ... -> 같은 네트워크 내의 목적지 서버 

## 프로토콜 
네트워크 인터페이스층의 프로토콜에는 대표적으로 이더넷과 무선 LAN이 있다.

## 이더넷 
이더넷은 같은 네트워크 내의 어떤 이더넷 인터페이스에서 다른 이더넷 인터페이스까지 데이터를 전송한다.

### MAC 주소
이더넷 인터페이스를 특정하기 위한 48비트 주소이다. 앞의 24비트는 OUI(이더넷 인터페이스 제조 벤더 식별 코드), 뒤의 24비트는 각 벤더가 할당하는 시리얼 넘버로 구성된다. MAC 주소는 이더넷 인터페이스에 미리 할당되어 있어, 기본적으로 변경할 수 없는 주소이므로 '하드웨어 주소'라고도 불린다.

### 데이터 형식 
- 이더넷 헤더에는 목적지 MAC 주소, 출발지 MAC 주소, 타입 코드가 있다.
   - 타입 코드는 운반할 대상의 데이터(패킷)의 타입을 의미한다. (IPv4의 경우 0x0800, ARP의 경우 0x0806)
   - 데이터의 최대 크기는 1500바이트이다.

<img width="439" alt="image" src="https://github.com/YimJiYoung/TIL/assets/37496919/e4f63313-4a19-4d3d-82d5-79a5a09dc595">

### 레이어2 스위치
레이어2 스위치는 이더넷을 이용한 네트워크 하나를 구성하는 네트워크 기기이다. 레이어2 스위치를 통해 MAC 주소에 기반한 동일 네트워크 내의 데이터 전송이 가능해진다.

### 레이어2 스위치의 동작
1. 수신한 이더넷 프레임의 출발지 MAC 주소를 MAC 주소 테이블에 등록한다. (주소의 제한 시간 기본 5분)
2. 목적지 MAC 주소와 MAC 주소 테이블에서 전송할 포트를 결정해, 이더넷 프레임을 전송한다.
   - MAC 주소 테이블에 존재하지 않는 MAC 주소의 경우는 수신 포트를 제외한 모든 포트로 이더넷 프레임을 전송한다. (플러딩) 이때 전송 범위는 같은 네트워크 안이다.