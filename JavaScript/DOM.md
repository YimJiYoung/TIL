## DOM

## textContent vs innerHTML
### textContent
- <script>와 <style>을 포함한 모든 자식 요소의 text 가져와서 반환
- textContent로 설정 시 해당 요소의 모든 자식 노드를 삭제하고 하나의 텍스트 노드로 대치한다.
```html
<p id="source">
  <style>#source { color: red; }</style>
아래에서<br>이 글을<br>어떻게 인식하는지 살펴보세요.
  <span style="display:none">숨겨진 글</span>
</p>
```
- textContent 결과
```
  #source { color: red; }
아래에서이 글을어떻게 인식하는지 살펴보세요.
  숨겨진 글
```
- innerText 결과 
   - `<br>` 인식, 숨겨진 요소 무시
   - textContent를 지원하지 않는 IE 8 버전 이하를 지원하기 위해서 IE에 특화되어 개발된 API로 되도록 사용하지 않는 것 권장
```
아래에서
이 글을
어떻게 인식하는지 살펴보세요.
```

### innerHTML
- 모든 자식 요소 문자열 형태로 반환
- innerHTML 의 값을 설정(대입)하면 요소의 모든 자식 노드가 제거되고, 문자열 htmlString에 지정된 HTML을 파싱하고, 생성된 노드로 대체한다.
- cross site scripting 보안 문제. 사용자 입력 문자열로 innerHTML을 사용해서는 안된다 !! 
    - HTML5에서는 innerHTML로 삽입된 <script> 실행하지 않도록 되어 있음 (그러나 다른 방법으로 스크립트가 실행되도록 할 수 있음 - onclick에 설정 등)
