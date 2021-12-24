# Performance

## Eliminate render blocking resources

### render blocking resources
- async나 defer 속성이 없는 `<head>` 내의 `<script>` 
- media 값이 없거나 all인 `<link rel="stylesheet">`
위의 요소들은 브라우저가 페이지에 컨텐츠를 렌더링을 막아 First Paint를 지연시킨다.

### How to eliminate render-blocking stylesheets
1. First Paint에 사용되는 필수 스타일은 `<head>` 내의 `<style>`에 인라인으로 정의
2. 나머지 스타일은 `<link rel="preload">` 병렬적으로 로딩 후 지연 적용
```html
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```
3. 해상도 구간별로 CSS 파일을 분리하고 media 속성으로 분기
- 미디어 조건을 만족할 때만 해당 리소스를 불러온다.
```html
<link href="*.css" rel="stylesheet" media="(max-width:639px)">
<link href="*.css" rel="stylesheet" media="(min-width:640px) and (max-width:960px)">
<link href="*.css" rel="stylesheet" media="(min-width:961px)">
```
