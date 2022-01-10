# DNS(Domain Name System)

google.com와 같은 웹사이트를 식별하는 도메인 네임을 IP 주소로 매핑해주는 룩업 서비스
- 인터넷에서는 IP 주소로 동작하지만 인간이 더 이해하기 쉬운 도메인 네임으로 접근할 수 있도록 한다.

## DNS 서버 계층 구조
DNS 서버는 다음과 같은 계층 구조로 되어 있으며 URL의 각 파트가 서버 계층에 대응된다.

![https://www.cloudflare.com/ko-kr/learning/dns/glossary/dns-root-server/](https://www.cloudflare.com/img/learning/dns/glossary/dns-root-server/dns-root-server.png)

### Root Servers
(클라이언트 쪽에 캐시가 안되어있는 경우) DNS 쿼리의 첫 진입점이 되며, 적절한 TLD(Top-level Domain) 서버의 IP 주소를 반환한다.
- 예) `dnsimple.com`에 대해서 `com` TLD 서버의 주소 반환

### Top-level Servers
특정 도메인(ex. com, io, net, org ...)에 대해 매핑해주는 DNS 서버이다. 각 도메인은 `mozilla.org`와 같은 특정 조직에 의해 사용되며, Top-level 도메인의 서버는 질의된 조직의 Authoritative 서버 주소를 반환한다.

### Authoritative Servers
웹 사이트나 이메일 서버를 가진 조직은 호스트 네임을 IP 주소로 매핑하는 DNS 레코드를 가진다. Authoritative 서버에서 이런 레코드를 가지고 있으며 최종적으로 질의된 IP 주소를 반환하는 역할을 한다. (서브 도메인인 경우 제외)

### Local DNS Cache
- 웹사이트 접속 시 빈번한 DNS 룩업을 막고 빠르게 접속할 수 있도록 클라이언트에서 DNS 매핑이 캐시된다. (브라우저, OS local resolver library)

### Local DNS Servers(Resolver)
클라이언트의 요청을 네임 서버로 전달하고 네임 서버로부터 IP 주소를 받아 클라이언트에게 제공하는 기능을 수행한다. 리졸버는 보통 ISP(Internet Service Provider)이다.
![https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png)

## 🔗 참고
https://www.cloudflare.com/learning/dns/what-is-dns/
