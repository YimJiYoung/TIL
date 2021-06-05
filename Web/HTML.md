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
