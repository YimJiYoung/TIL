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
특정 도메인(ex. com, io, net ...)에 대해 매핑해주는 DNS 서버이며 도메인에 맞는 Authoritative 서버 주소를 반환한다.

### Authoritative Servers

### Local DNS Servers(Resolver)
