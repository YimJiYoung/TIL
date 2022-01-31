# HTTP

### URL(Universal Resource Locator)
네트워크 상에서 자원의 위치를 나타내는 주소
- protocol/domain name(+port)/path(리소스 위치)/query/fragment로 구성된다.
```
        foo://example.com:8042/over/there?name=ferret#nose
        \_/   \______________/\_________/ \_________/ \__/
        |           |            |            |        |
    scheme     domain name     path        query   fragment
```

### Stateless Protocol
- HTTP는 클라이언트-서버 프로토콜로 클라이언트는 html, 이미지와 같은 리소스를 요청(Request)하고 서버는 리소스를 제공하며 응답(Response)한다.
- HTTP는 상태를 유지하지 않는 Stateless 프로토콜이다.
   - 요청과 응답을 주고 받으며 상태를 관리하지 않기 때문에 기본적으로 새로운 요청이 보내질 때마다 새로운 응답이 생성되며 과거의 요청, 응답에 대한 정보를 저장하지 않는다.
   - 상태를 유지하지 않으므로 클라이언트에 대한 응답 서버를 쉽게 바꿀 수 있으므로 수평 확장에 유리하다.

## HTTP Connections
어플리케이션 계층 하위에 위치한 전송 계층에는 TCP와 UDP가 있으며, 그 중 TCP는 데이터가 전송되는 것을 보장한다. HTTP(1, 2버전)는 TCP를 사용한다.

> TCP는 연결지향 프로토콜로, 데이터가 보내진 순서대로 전송되는 것을 보장한다.

### Persistent / Non-persistent HTTP
- Non-persistent: 요청마다 하나의 TCP 연결을 사용한다. 클라이언트가 html 파일을 요청한다고 했을 때 html 파일 내에서 다른 리소스(이미지, javascript 등)을 참조하고 있으면 이후 요청 시마다 TCP 연결이 새로 생성되고 종료된다 → 연결 유지에 필요한 자원 ⬇ 
- Persistent: 어느 한 쪽이 명시적으로 연결을 종료하지 않는 이상 TCP 연결을 유지한다  → TCP 커넥션의 연결과 종료의 오버헤드 ⬇

## Request Message
### Request Line
```
    GET /path/to/file/index.html HTTP/1.1x
```
#### Method
- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`
- `HEAD`

### Version
HTTP 버전

### Header Lines
- `HOST`
- `Connection`: 커넥션 타입(persistent or not)
- `User-agent`: 
- `Accept-language`
- `Accept`: 
