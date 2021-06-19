# HTML(Hypertext Markup Language)

- 태그(`<...>`)를 이용하여 데이터나 문서의 구조를 표현하는 마크업 언어의 일종.
   - Hypertext는 a 요소의 동작과 같이 링크를 통한 문서 이동을 의미한다.
- HTML 표준은 마크업 언어로서의 HTML뿐만 아니라 Web Storage, Socket와 같은 관련 API를 포함한다.
- HTML의 최신 버전은 [HTML Living Standard](https://html.spec.whatwg.org/)

## DOCTYPE
- 문서의 타입과 버전을 명시한다. (생략 시 하위 호환성 모드인 Quirks mode에서 동작)
```html
<!-- HTML Living Standard 버전 -->
<!DOCTYPE html>
```
## HTML Content Model
특정한 요소가 어떤 콘텐츠를 나타내는 지 보여주기 위한 모델

### Metadata content
문서의 정보를 표현하거나 다른 문서와의 관계를 나타낸다.
- link
- meta
- noscript, script
- style
- template
- title

###  Flow content
문서의 body 내부에서 사용되는 대부분의 요소 

### Sectioning content
heading과 footer의 범위를 정하기 위한 콘텐츠 모델
- article
- aside
- nav
- section

### Heading content
section의 제목을 나타낼 때 사용하는 콘텐츠 모델
- h1, h2, h3, h4, h5, h6, hgroup

### Phrasing content
문서 내에서 text를 사용하는 모든 콘텐츠 모델

### Embedded content
문서 내에서 다른 리소스를 불러오는 콘텐츠
- audio
- canvas
- embed
- iframe
- img
- picture
- svg
- video

### Interactive content
유저와 상호작용이 발생하는 콘텐츠 모델 (일반적으로 interactive content안에 다른 interactive content를 넣을 수 없다)
- a (if the href attribute is present)
- audio (if the controls attribute is present)
- video (if the controls attribute is present)
- input (if the type attribute is not in the Hidden state), label, select, textarea
- button


## HTML Element - Document Metadata

### head
문서에 대한 메타데이터를 담는 요소.

### meta
해당 문서, 애플리케이션의 메타데이터를 담는 요소
- charaset : 문자 인코딩 방식 결정
- viewport : 모바일 디바이스의 화면에서 어떻게 보일지 결정
- description : SEO 최적화를 위해서 검색 결과에 보이는 설명을 작성
- Open Graph Protocol : Facebook 등 소셜 서비스에서 사용하는 공유를 위한 프로토콜
- Twitter Card : 트위터에서 카드 형태로 공유할 때 사용하는 프로토콜

```html
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">

   <meta property="og:title" content="The Rock" />
   <meta property="og:type" content="video.movie" />
   <meta property="og:url" content="https://www.imdb.com/title/tt0117500/" />
   <meta property="og:image" content="https://ia.media-imdb.com/images/rock.jpg" />

   <meta name="twitter:card" content="summary_large_image">
   <meta name="twitter:site" content="@nytimes">
   <meta name="twitter:creator" content="@SarahMaslinNir">
   <meta name="twitter:title" content="Parade of Fans for Houston’s Funeral">
   <meta name="twitter:description" content="NEWARK - The guest list and parade of limousines with celebrities emerging from them seemed more suited to a red carpet event in Hollywood or New York than than a gritty stretch of Sussex Avenue near the former site of the James M. Baxter Terrace public housing project here.">
   <meta name="twitter:image" content="http://graphics8.nytimes.com/images/2012/02/19/us/19whitney-span/19whitney-span-articleLarge.jpg">
```

### title
문서의 제목을 담는 요소 (SEO를 위해서 필수)

### link
외부 문서, 콘텐츠와 연결하는 요소. 스타일 시트와 연결하는데 주로 사용. (HTTP 요청이 추가로 발생하기 때문에 성능 저하의 가능성이 있다)
```
<link rel="stylesheet" href="style.css">
```

### style
문서 내에서 사용하는 스타일 정의. 별도의 HTTP 요청이 발생하지 않기 때문에 렌더링에 필수적인 스타일을 style 요소 내에 두면 성능을 개선할 수 있다.


## HTML Element - Sections

### body

### heading(h1, h2, h3, h4, h5, h6)
- 제목을 명시할 때 사용하는 요소. (숫자는 중요도 또는 계급을 나타냄)
- heading 요소를 적절하게 사용하여 문서의 아웃라인을 나타낼 수 있다
  - 참고: [HTML 구획과 개요 사용하기](https://developer.mozilla.org/ko/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines)

### article & section
- heading만으로 문서의 구조와 아웃라인을 표현하는데 한계가 있기 때문에 article, section과 같은 요소를 사용해서 구역을 명확하게 나눌 수 있다.
- 독립적인 콘텐츠를 담고있는 경우 article을, 다른 콘텐츠와 연관이 있는 경우 section을 사용한다.

### header
- 소개 및 탐색에 도움을 주는 콘텐츠를 나타낸다. 
- 콘텐츠의 시작 부분 명시

### hgroup
- heading 요소를 그룹화하기 위해 사용한다.
- 제목-부제목과 같은 "다단계 제목"을 만들 수 있다.
```html
<hgroup>
  <h1>주요 제목</h1>
  <h2>부제목</h2>
</hgroup>
```

### footer
- 콘텐츠의 마무리를 나타내는 요소.
  - 작성자 정보나 copyright, 출처 등의 정보를 담는다.

### address
- contact 정보를 담는 요소.
```html
<address>
  <a href="mailto:jim@rock.com">jim@rock.com</a><br>
  <a href="tel:+13115552368">(311) 555-2368</a>
</address>
```

### nav
- 페이지 네비게이션 콘텐츠를 나타내는 요소.
  - 내부 혹은 외부 문서에 연결되는 네비게이션 링크를 포함한다. (주로 a 태그와 함께 사용)
 ```
 <nav class="menu">
  <ul>
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
</nav>
 ```
 
### aside
- 문서의 메인 콘텐츠와 직접적인 연관이 없는 콘텐츠를 나타내는 요소. (ex. 추천, 광고)



## HTML Element - Grouping content

### p
- 하나의 문단(paragraph)을 나타내는 요소

### blockquote
- 인용 블록을 나타내는 요소
- 인용문의 출처 URL은 cite 특성으로, 출처 텍스트는 <cite> 요소로 제공할 수 있다.
   
```html
<figure>
    <blockquote cite="https://www.huxley.net/bnw/four.html">
        <p>Words can be like X-rays, if you use them properly—they’ll go through anything. You read and you’re pierced.</p>
    </blockquote>
    <figcaption>—Aldous Huxley, <cite>Brave New World</cite></figcaption>
</figure>
```
   
   
### ol, ul, li
- ol(ordered list): 순서 있는 목록을 나타내는 요소 (ex. 레시피, 커리큘럼 등)
- ul(unordered list): 순서 없는 목록을 나타내는 요소 
- li(list item): 목록 내의 아이템을 나타내는 요소. 부모 요소로 li, ul, menu를 가져야 한다.
   
### dl, dt, dd
- dl(Description List): 정의 목록을 나타내는 요소
- dt(Description Term): 정의하고자 하는 대상을 나타내는 요소
- dd(Description Details): dt 요소에 이어서 대상에 대한 설명을 나타내는 요소
   - dt와 dd는 쌍을 이루어야 한다. (하나의 dt에 여러 dd 가능)
 ```html
 <dl>
    <dt>Denim (semigloss finish)</dt>
    <dd>Ceiling</dd>
 
    <dt>Denim (eggshell finish)</dt>
    <dt>Evening Sky (eggshell finish)</dt>
    <dd>Layered on the walls</dd>
</dl>
 ```
 
### figure, figcation
  - figure: 삽화나 다이어그램, 사진 등과 같이 문서의 주요 흐름과는 독립적이지만 부연 설명을 위한 콘텐츠를 담는 요소.
     - figure 요소 내에 figcation 요소를 이용해서 부연 설명을 추가한다.
 ```
 <figure>
    <img src="/media/cc0-images/elephant-660-480.jpg"
         alt="Elephant at sunset">
    <figcaption>An elephant at sunset</figcaption>
</figure>
 ```
   
### main
- 문서의 메인 컨텐츠를 나타내는 요소.
   - 문서는 hidden 속성을 가지지 않은 main 요소를 하나 이상 가질 수 없다.


###  div 
- 의미 없이 css 등의 목적으로 사용하는 컨테이너의 역할을 한다.
- <div> 요소는 의미를 가진 다른 요소(<article>, <nav> 등)가 적절하지 않을 때만 사용해야 한다. (🚧 마지막 수단으로 사용하자 !)
   

## HTML Element - Text-level semantics

### a 
- href 속성을 통해 하이퍼 링크를 생성하는 요소
 - href 속성이 필수는 아니다(없을 시 비활성화 상태)
 - download 속성을 이용해 링크로 이동하는 대신 리소스를 다운로드 받게 할 수 있다.
 - target 속성을 통해 링크된 url을 표시할 위치를 결정한다.
    - `_self`: 현재 브라우징 맥락(탭, 윈도우 또는 iframe)에서 이동 (기본값)
    - `_blank`: 새로운 탭으로 이동 

#### hash link
같은 페이지 내에 해당 아이디를 가진 요소로 이동
```html
<!-- <a> element links to the section below -->
<p><a href="#Section_further_down">
  Jump to the heading below
</a></p>

<!-- Heading to link to -->
<h2 id="Section_further_down">Section further down</h2>
```