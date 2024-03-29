# 브라우저 내부 동작

## 브라우저 아키텍처

![브라우저 아키텍처](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_1-08.png)

브라우저는 여러 개의 스레드를 가진 하나의 프로세스 또는 스레드를 조금씩만 사용하는 여러 개의 프로세스로 구현할 수 있다. 크롬은 다중 프로세서 아키텍처를 선택한다. (그러나 하드웨어 성능에 따라 메모리 절약을 위해 하나의 프로세스에서 동작할 수도 있음) 또한 탭마다 렌더러 프로세스를 별도로 할당한다. (정확히는 사이트마다 할당한다고 하지만 이부분은 정확히 이해하지 못함)

</br>

### 각 프로세스의 역할

![브라우저 프로세스](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_1-09.png)

#### 브라우저 프로세스

- 주소 표시줄, 북마크, 뒤/앞으로 가기 버튼 등 웹 사이트가 표시되는 부분을 제외한 나머지 부분 제어
- 네트워크 요청이나 파일 접근과 같은 권한이 필요한 부분 처리

#### 렌더러 프로세스

- 탭 안에서 웹 사이트가 표시되는 부분의 모든 것 제어

이외에 플러그인 프로세스, GPU 프로세스가 존재

</br>

### 다중 프로세스 아키텍처의 장점과 단점

#### 장점

- 탭마다 독립적인 렌더러 프로세스를 사용하기 때문에 한 탭이 응답하지 않아도 다른 탭 정상적으로 동작

     → 의문점 : 스레드를 사용해도 하나의 스레드가 동작하지 않아도 다른 스레드에 영향은 없지 않나 ?🤔

- 보안과 격리 - 프로세스마다 권한을 설정할 수 있어 다른 프로세스에서 특정 기능에 접근하지 못하도록 제한할  수 있다. (하나의 탭에서 다른 사이트를 표시하는 다른 탭에 접근할 수 있으면 안되겠지 ..)

#### **단점**

- 프로세스는 각자 별도의 주소 공간을 갖기 때문에 공통부분(예: JavaScript 엔진 V8)을 복사해서 가지고 있음→ 메모리 사용량 증가

</br>

## 네비게이션 과정

### 브라우저 프로세스에서 시작

![](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-01.png)

브라우저 프로세스는 브라우저의 버튼과 입력란을 제어하는 UI 스레드, 네트워크 요청을 처리하는 네트워크 스레드, 파일에 대한 접근을 제어하는 스토리지 스레드를 갖고 있다. 주소 표시줄에 URL를 입력하면 UI 스레드가 입력을 처리한다.

### 1. 입력 처리

- UI 스레드는 입력되는 내용이 검색어인지 URL인지 확인한다. 입력 내용에 따라 검색 엔진으로 이동할지 요청된 사이트로 이동할지 결정한다.

### 2. 내비게이션 시작

- 사용자가 Enter 키를 누르면 사이트의 콘텐츠를 가져오기 위해 UI 스레드가 네트워크 호출을 시작한다.
- 네트워크 스레드는 요청에 대한 DNS Lookup 및 TLS 연결 설정과 같은 적절한 프로토콜을 거쳐 요청을 처리한다. (TLS(Transport Layer Security): 인터넷에서의 데이터를 암호화하여 송수신하는 프로토콜)

### 3. 응답 읽기

- 응답이 HTML 파일이라면 데이터를 렌더러 프로세스에 전달하는 단계로 넘어간다. 하지만 응답이 ZIP 형식 파일이나 다른 형식의 파일이라면 다운로드 요청이므로 다운로드 매니저에 데이터를 전달하는 단계로 넘어가야 한다.
- 이 단계는 또한 Safe Browsing의 검사가 실행되는 단계이다. 도메인과 응답 데이터가 악성 사이트로 알려진 사이트와 일치하는 것 같다면 네트워크 스레드는 경고 페이지를 표시하라고 알린다.

### 4. 렌더러 프로세스 찾기

- 모든 검사가 끝나고 브라우저가 요청된 사이트로 이동해야 한다고 네트워크 스레드가 확신하게 되면 네트워크 스레드는 UI 스레드에 데이터가 준비되었음을 알린다. 그러면 UI 스레드는 웹 페이지의 렌더링을 수행할 렌더러 프로세스를 찾는다.

### 5. 네비게이션 실행

![](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-07.png)

- 이제 데이터와 렌더러 프로세스가 준비되었으므로 내비게이션을 실행하도록 브라우저 프로세스에서 렌더러 프로세스로 IPC 메시지를 전송한다. 또한 렌더러 프로세스가 HTML 데이터를 계속 수신할 수 있도록 브라우저 프로세스는 데이터 스트림을 전달한다. 렌더러 프로세스에서 내비게이션이 실행되었다는 것을 브라우저 프로세스가 확인하고 나면 내비게이션이 완료되고 문서 로딩 단계가 시작된다.


## 렌더링 과정

### 렌더러 프로세스

![](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-01.png)

렌더러 프로세스는 브라우저로 전송된 대부분의 코드를 처리하는 메인 스레드를 갖고 있다. 웹 워커나 서비스 워커를 사용하는 경우에는 워커 스레드가 일부 JavaScript 코드를 처리한다.

- [web worker](https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API) : 메인 스레드와 별도의 백그라운드 스레드에서 스크립트 연산 수행.

### HTML 파싱 → DOM 구축

렌더러 프로세스가 HTML 데이터를 수신하기 시작하면  메인 스레드는 문자열(HTML)을 파싱해서 DOM(document object model)으로 변환하기 시작한다. DOM은 HTML 문서를 객체로 표현한 것이며 JavaScript가 HTML 태그에 접근할 수 있도록 하는 API이다. 여러 태그와의 관계를 나타내기 위해 계층적인 구조인 트리 구조를 사용한다.

![](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/dom-tree.png?hl=ko)

**JavaScript가 파싱을 막을 수 있다** 

`<script>` 태그를 만나면 HTML 문서의 파싱을 중지하고 JavaScript 코드를 로딩하고 파싱해 실행한다. 이는 JavaScript가 DOM 구조를 바꿀 수 있기 때문이다. 만약 파싱을 막고 싶지 않다면 script 태그에 defer/async 속성을 이용할 수 있다.
- defer : 스크립트는 백그라운드에서 병렬적으로 로딩된다 → HTML 파싱이 완료된 후에 스크립트를 실행한다.
- async: 마찬가지로 스크립트는 백그라운드에서 병렬적으로 로딩된다 → HTML 파싱이 완료되기 전에 로드가 끝난 경우 스크립트가 실행되므로 파싱이 멈출 수 있다.

### CSS 파싱 → CSSOM

HTML 파싱과 같이 CSS를 파싱하여 CSSOM(CSS Object Model)으로 변환한다. CSSOM은 다음과 같은 트리 구조를 갖는다. DOM 노드의 최종 스타일을 계산할 때 해당 노드의 가장 일반적인 규칙(예: body 요소의 하위인 경우 모든 body 스타일 적용)으로 시작한 후 더욱 구체적인 규칙을 적용하는 방식으로, 즉 '하향식'으로 규칙을 적용하는 방식으로 스타일을 계산한다.

![](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/cssom-tree.png?hl=ko)

### 렌더 트리 생성

CSSOM 및 DOM 트리는 결합하여 렌더링 트리를 형성한다. 렌더링 트리는 표시되는 각 요소의 레이아웃을 계산하는데 사용되며 픽셀을 화면에 렌더링하는 페인트 프로세스의 입력이 된다.

- DOM 및 CSSOM 트리는 결합되어 렌더링 트리를 형성한다.
- 렌더링 트리에는 페이지를 렌더링하는 데 필요한 노드만 포함된다. (예: `{ display: none }` 인 요소, `<head>` 제외)

![](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/render-tree-construction.png?hl=ko)

### Layout (Reflow)

이제 렌더러 프로세스가 문서의 구조와 각 노드의 스타일을 알지만 페이지를 렌더링하기에는 충분하지 않다.

 viewport 내에서 노드의 정확한 위치와 크기를 계산하는 '레이아웃' 단계가 필요하다. 페이지에서 각 객체의 정확한 크기와 위치를 파악하기 위해 브라우저는 렌더링 트리의 루트에서 시작하여 렌더링 트리를 순회한다.

- 레이아웃은 각 객체의 정확한 위치(x, y 좌표) 및 박스 영역의 크기를 계산한다.

### 전역 배치와 점증 배치

**전역 배치 -동기적**

- 글꼴 크기 변경과 같이 모든 렌더러에 영향을 주는 전역 스타일 변경
- 화면 크기(viewport) 변경에 의한 결과

**점증 배치 -비동기적**

- 다시 배치할 필요가 있는 변경 요소 또는 추가된 요소가 있을 때 - 해당 요소와 자식 요소들만 새로 배치 작업

### Paint (Repaint)

이제 표시되는 노드와 해당 노드의 계산된 스타일 및 기하학적 형태까지 파악했으므로, 마지막으로 렌더링 트리의 각 노드를 화면의 실제 픽셀로 변환하는 그리기 작업이 수행된다. 이런 그리기 작업은 레이어 별로 수행되는데, 레이어는 z-index나 will-change와 같은 속성에 따라서 분리된다. 레이어로 분리할 때의 장점은 부분적인 그리기 작업이 필요할 때 전체 화면을 다시 그리는 것이 아니라 해당 레이어만 그리면 되기 때문에 성능적으로 이점이 있다. 그렇다고 레이어를 지나치게 세분화하는 것은 메모리에 부담을 주고 오히려 성능이 저하될 수 있다.

### Composite

레이어를 순서대로 화면에 그린다.
### 렌더링 파이프라인을 갱신하는 데는 많은 비용이 든다

각 단계에서 이전 단계의 결과물을 사용하기 때문에 레이아웃 트리에서 변경이 생겨 문서의 일부가 영향을 받으면 페인팅 순서도 새로 생성해야 한다. 최적의 렌더링 성능을 얻기 위해서는 이러한 단계 각각을 최적화하는 것이 중요하다.
- 레이아웃 너비, 높이, 왼쪽 또는 상단 위치 등 요소의 기하학적 형태에 영향을 주는 'layout' 속성을 변경하면? Layout → Paint → Composite
- 페이지의 레이아웃에 영향을 주지 않는 배경 이미지, 텍스트 색상 또는 그림자 등의 'paint only' 속성을 변경하면? Paint → Composite
- 레이아웃과 페인트가 필요 없는 속성을 변경하면(ex. transform)? Only Composite (Best👍)
- 각 단계를 트리거하는 속성 확인하려면 [CSS Trigger](https://csstriggers.com/) 참고하기

![https://developers.google.com/web/fundamentals/performance/rendering/images/intro/frame-full.jpg?hl=ko](https://developers.google.com/web/fundamentals/performance/rendering/images/intro/frame-full.jpg?hl=ko)

![](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-10.gif)

### 동적 변경

브라우저는 변경에 대해 가능한 한 최소한의 동작으로 반응하려고 노력한다. 그렇기 때문에 요소의 색깔이 바뀌면 해당 요소의 리페인팅만 발생한다. 요소의 위치가 바뀌면 요소와 자식 그리고 형제의 리페인팅과 재배치가 발생한다. DOM 노드를 추가하면 노드의 리페인팅과 재 배치가 발생한다. "html" 요소의 글꼴 크기를 변경하는 것과 같은 큰 변경은 캐시를 무효화하고 트리 전체의 배치와 리페인팅이 발생한다.

## 🔗 참고
- [최신 브라우저의 내부 살펴보기 시리즈](https://d2.naver.com/helloworld/2922312)
- https://d2.naver.com/helloworld/59361
- https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction?hl=ko
- https://developers.google.com/web/fundamentals/performance/rendering?hl=ko
