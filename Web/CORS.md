## CORS(Cross Origin Resource Sharing)

### Same Origin Policy
보안상의 이유로 브라우저는 (도메인, 프로토콜, 포트가) 다른 Origin으로 fetch 요청을 보내는 것을 제한한다.
이를 우회하기 위해 `<script src="http://another.com/…">`와 같이 스크립트를 통해 [JSONP(JSON with padding)](https://ko.wikipedia.org/wiki/JSONP) 프로토콜로 요청을 하고 데이터를 받아오는 방법을 사용하기도 했다. 

### CORS
크로스 오리진 요청이 불가능했다. 하지만 크로스 오리진 요청을 허용하기로 결정하고 
대신 크로스 오리진 요청은 서버에서 명시적으로 크로스 오리진 요청을 ‘허가’ 하는지 안 하는지를 알려주는 특별한 헤더를 전송받았을 때만 가능하도록 제약을 걸게 되었다.


### Simple 요청
simple request(simple 요청)은 다음과 같은 두 가지 조건 모두를 충족하는 요청이며 CORS preflight를 트리거하지 않는다.
- simple method(simple 메서드) – GET이나 POST, HEAD 메서드를 사용한 요청
- simple header(simple 헤더) – 다음 목록에 속하는 헤더
    - `Accept`
    - `Accept-Language`
    - `Content-Language`
    - 값이 `application/x-www-form-urlencoded`이나 `multipart/form-data`, `text/plain`인 `Content-Type`
    

### Simple 요청과 CORS
크로스 오리진 요청을 보낼 경우 브라우저는 항상 `Origin`이라는 헤더를 요청에 추가한다.
서버는 크로스 오리진 요청을 허가할 origin을 명시한 헤더 `Access-Control-Allow-Origin`를 응답에 추가한다.
해당 orign이나 *(모든 origin 허용)이 있는 `Access-Control-Allow-Origin` 헤더를 받으면 fetch는 성공하고, 그렇지 않으면 실패한다.
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5bbf551e-3652-432b-8d58-e9bcf4c99872/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210116T030850Z&X-Amz-Expires=86400&X-Amz-Signature=f93a531f490b0a4a9f8cfac067f5401c7b37fe89a635b72a9ba729e6c5557832&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### Non-Simple 요청과 CORS
위에 정의된 simple 요청이 아닌 경우(`PUT`, `DELETE` 메서드를 사용하는 경우 등)는 다 non simple 요청이다.
과거엔 웹페이지에서 GET, POST 이외의 HTTP 메서드를 사용해 요청을 보낼 수 있게 될 것이라는 상상조차 할 수 없었다고 한다. 
‘non-simple’ 요청이 이뤄지는 경우, 브라우저는 서버에 바로 요청을 보내지 않고, ‘preflight’ 요청이라 불리는 사전 요청을 서버에 먼저 보내 권한이 있는지를 확인한다.

#### 요청
preflight 요청은 OPTIONS 메서드를 사용하고 헤더는 다음과 같다.
- `Access-Control-Request-Method` 헤더 – non-simple 요청에서 사용하는 메서드 정보
- `Access-Control-Request-Headers` 헤더 – non-simple 요청에서 사용하는 헤더 정보

#### 응답
non-simple 요청을 받아들이기로 동의한 경우, 서버는 상태 코드가 200인 응답을 다음과 같은 헤더와 함께 브라우저로 보낸다.
- `Access-Control-Allow-Methods` – 허용된 메서드 정보
- `Access-Control-Allow-Headers` – 허용된 헤더 목록
- `Access-Control-Max-Age` – 퍼미션 체크 여부를 몇 초간 캐싱해 놓을지를 명시 (이렇게 퍼미션 정보를 캐싱해 놓으면 브라우저는 일정 기간 동안 preflight 요청을 생략하고 non-simple 요청을 보낼 수 있다)

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/52b22e3e-a537-40e7-a6ab-a11e62356310/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210116T031643Z&X-Amz-Expires=86400&X-Amz-Signature=a6b173c50dc8645eb6c507a6e9ae5a9ba6a23ee7eb0294fce16db5b642a1c495&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### 자격 증명
자바스크립트를 사용해 보내는 크로스 오리진 요청의 경우 기본적으로 쿠키나 HTTP 인증 같은 자격 증명(credential)이 함께 전송되지 않는다.
fetch 메서드에 자격 증명 정보를 함께 전송하려면 다음과 같이 credentials: "include" 옵션을 추가해줘야 한다. 
또한 서버는 응답에 `Access-Control-Allow-Origin` 헤더와 함께 `Access-Control-Allow-Credentials: true` 헤더를 추가해서 보내야 한다.
이때의 `Access-Control-Allow-Origin` 값은 *이 될 수 없고 허용할 origin을 명시해줘야 한다.


## 참고 
- https://ko.javascript.info/fetch-crossorigin
- https://developer.mozilla.org/ko/docs/Web/HTTP/CORS
