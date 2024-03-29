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
## HTML Contents 종류

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
문서 내에서 외부 리소스를 불러오는 콘텐츠
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

### Palpable content
비어 있지 않은(텍스트나 볼 수 있거나 들을 수 있는 또는 인터렉션을 할 수 있는 자손을 갖고 있는) 요소를 나타내며 hidden 속성을 갖지 않는다.



## Content model
각 요소의 콘텐츠(자식 요소)에 대한 기술. HTML의 요소들은 각 요소의 content model에 정의된 콘텐츠를 담아야 한다.

### Transparent Content model
transparent하게 부모의 content model을 따른다.


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
    - 섹셔닝 요소는 개요(제목)에 대한 범위를 지정해주기 때문에 자식 요소로 헤딩 요소를 갖고 있지 않다면 어색한 섹션이 된다.
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
 ```html
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
 ```html
 <figure>
    <img src="/media/cc0-images/elephant-660-480.jpg"
         alt="Elephant at sunset">
    <figcaption>An elephant at sunset</figcaption>
</figure>
 ```
   
### main
- 문서의 메인 컨텐츠를 나타내는 요소.
   - 문서는 hidden 속성을 가지지 않은 main 요소를 하나 이상 가질 수 없다.
- 페이지마다 반복되는 내용이 아닌 고유한 내용을 담아야 하므로 GNB나 푸터를 포함하는 것은 부적절하다.


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
       - 열린 새창에서 `window.opener`를 통해 부모 창의 제어할 수 있으므로 rel 속성으로 `noopener` 또는 `onreferrer` 지정해야 함

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
   
### span
- 여러 개의 텍스트 요소를 감싸기 위해 사용하는 요소. (div와 마찬가지로 의미를 가지지 않으므로 마지막 수단으로 사용하자 !)
   
### br, wbr
- br: 텍스트 내에서 줄바꿈을 일으키는 요소.
- wbr: 줄바꿈이 일어나는 위치를 나타내는 요소
   
   
   
## HTML Element - Embedded Content
   
### img, [picture](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture)
- 이미지를 불러올 때 사용하는 요소
- picture 요소를 사용하여 브라우저 지원 범위에 따라 다양한 이미지 포맷을 사용할 수 있다.
   - 용량: jpg/png > webp > avif([지원범위](https://caniuse.com/?search=avif))
   - type: 이미지의 타입(포멧) 지정. 브라우저가 해당 타입을 지원하지 않으면 스킵된다.
   - medai: media 조건에 따라서 스킵된다.
   - srcset: 크기(너비, 픽셀 밀도)에 기반하여 가능한 이미지들을 명시한다. 
   - loading: `lazy`로 설정 시 유저가 이미지 가까이 스크롤한(뷰포트 높이의 1~2배 지점까지 근접한) 시점에 지연 로딩된다. (iframe에도 적용 가능)
   - decoding: `async`로 설정 시 다른 컨텐츠의 표시가 지연되는 것을 막고 비동기적으로 이미지를 디코딩한다. ([참고](https://www.smashingmagazine.com/2021/04/humble-img-element-core-web-vitals/#image-decoding))
```html
   <picture>
     <source type="im ge/svg+xml" srcset="pyramid.svg">
     <source type="image/webp" srcset="pyramid.webp">
     <img src="pyramid.png" alt="정삼각형 4개로 만든 일반적인 피라미드">
   </picture>
   
   
   <picture>
     <source srcset="logo-768.png 768w, logo-768-1.5x.png 1.5x">
     <source srcset="logo-480.png, logo-480-2x.png 2x">
     <img src="logo-320.png" alt="logo">
   </picture>
```

### video
- 비디오를 불러오기 위해 사용하는 요소
- src 속성으로 리소스의 경로를 지정하고 source 요소를 사용하여 여러 개의 foramt을 지원할 수 있다. 
   - video 내의 컨텐츠는 브라우저가 해당 요소를 지원하지 않을 때 fallback으로 보여진다.
- autoplay: 비디오 자동 재생 설정. 접근성 이슈로 muted 속성으로 음소거를 해둔 상태로 재생해야한다.
- controls: 볼륨 조절, 재생/멈춤 등의 브라우저 기본 컨트롤을 제공한다.
```html
   <video controls width="250">
     <source src="/media/cc0-videos/flower.webm"
            type="video/webm">
     <source src="/media/cc0-videos/flower.mp4"
            type="video/mp4">
     Sorry, your browser doesn't support embedded videos.
   </video>
```
   
### iframe
- 외부의 HTML 문서를 불러오기 위해 사용하는 요소
- src
- allow: 어떤 기능을 허용해줄 것인지 

   
## HTML Element - tabular data
   
### table 
행과 열로 이루어진 표를 나타내는 요소
```html
   <table>
    <caption>Council budget (in £) 2018</caption>
    <thead>
        <tr>
            <th>Items</th>
            <th scope="col">Expenditure</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">Donuts</th>
            <td>3,000</td>
        </tr>
        <tr>
            <th scope="row">Stationery</th>
            <td>18,000</td>
        </tr>
    </tbody>
  </table>
```
   
### caption
표의 제목 또는 설명을 나타내는 요소
   
### thead, tbody, tfoot
각각 테이블의 헤드, 본문, 결론(요약) 부분을 나타내는 요소
   
### colgroup, col
각 셀에 공통적인 스타일을 적용하고자 할 때 사용된다.
   - col의 갯수는 tr 요소 내의 셀 갯수와 동일해야 한다.
   - span 속성으로 여러 셀에 같은 스타일을 부여할 수 있다.
```html
   <table>
    <caption>Superheros and sidekicks</caption>
    <colgroup>
        <col>
        <col span="2" class="batman">
        <col span="2" class="flash">
    </colgroup>
    <tr>
        <td> </td>
        <th scope="col">Batman</th>
        <th scope="col">Robin</th>
        <th scope="col">The Flash</th>
        <th scope="col">Kid Flash</th>
    </tr>
    <tr>
        <th scope="row">Skill</th>
        <td>Smarts</td>
        <td>Dex, acrobat</td>
        <td>Super speed</td>
        <td>Super speed</td>
    </tr>
</table>
```
   
### tr, th, td
- tr: 테이블의 행(row)를 의미한다. 
   - td와 th 요소를 자식 요소로 사용 
- td: 테이블 내의 각 데이터를 의미한다.
- th: 데이터의 제목(header)를 의미한다.
   - scope 속성을 통해 해당 요소를 기준으로 다음 요소를 어느 순서로 읽어야하는지 지정할 수 있다. (row, col, rowgroup, colgroup)
   

## HTML Element - Forms

### form
유저가 입력하는 데이터를 입력받아 서버로 전송하기 위한 요소
- `<input type="submit">`이나 `<input type="image">` 클릭하거나 인풋 필드에서 Enter 키 누르면 submit 이벤트가 발생한다.

### input
유저가 데이터를 입력하는 입력창을 나타내는 요소
- type: button, checkbox, color, date, email, password, radio, submit 등 .. 

### lable
input에 대한 설명을 나타내는 요소
- input과 연결된 label 클릭 시 해당 input 요소가 포커스된다.
- input과 연결시키기 위해서 for 속성에 input의 id를 지정하거나 label 요소 내에 input 요소를 둔다.

### fieldset
form 내에서 그루핑하기 위한 요소
- legend 요소로 fieldset의 제목(설명)을 나타낸다.
```html
<form>
  <fieldset>
    <legend>Choose your favorite monster</legend>

    <input type="radio" id="kraken" name="monster">
    <label for="kraken">Kraken</label><br/>

    <input type="radio" id="sasquatch" name="monster">
    <label for="sasquatch">Sasquatch</label><br/>

    <input type="radio" id="mothman" name="monster">
    <label for="mothman">Mothman</label>
  </fieldset>
</form>
```

### button
유저가 클릭할 수 있는 버튼을 나타내는 요소
- type: 기본적으로 submit 타입으로 form을 전송하기 위한 용도로 사용된다.
  - button: 기본 동작 없이 주로 자바스크립트로 인터렉션을 다룬다.
  - reset: input 데이터 초기화
- disabled: 유저와의 인터렉션을 막는다.

#### a vs button
같은 외형이라도 a, button을 의미에 따라 구별해서 사용해야 함
   - 실행 결과를 가리킬 url이 존재한다면 a 태그 아니면 button 태그를 사용한다.
