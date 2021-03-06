## DOM
![DOM Node](https://media.vlpt.us/images/wiostz98kr/post/cf345b7f-5f26-4a57-a061-ffd3e1a3f3ea/image.png)

### EventTarget
이벤트가 발생하고 이벤트 핸들러를 등록할 수 있는 객체에 대한 DOM 인터페이스
- addEventListener(type, listener, options or useCapture): 이벤트 타입에 대한 핸들러 등록
  - options default false
  - capture: 이벤트 캡처링 단계에서 전달받은 이벤트에 대해 핸들러 실행 여부(하위 요소에 이벤트 발생 시 미리 실행)
  - once: 등록된 이벤트 핸들러 한 번만 실행
  - passive: true로 설정 시, 핸들러에서 preventDefault()를 실행하지 않을 것을 암시
- removeEventListener(type, listener, options or useCapture): 이벤트에 등록된 핸들러 삭제
  - options에는 capture만 가지고 있음
  - `capture/useCapture`가 일치해야 성공적으로 삭제 가능
- dispatchEvent(event): `Event` 객체를 받아 해당 이벤트 실행.
  - 참고: https://ko.javascript.info/dispatch-events

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
    - 따라서 추가되는 전체 htmlString에 대해 HTML 파싱 -> DOM tree 구성 & 삽입 등의 추가 작업 발생 ! 
    - 전체 변경이 아닌 부분적인 변경이 필요한 경우나 삽입이 필요한 경우는 element나 `insertAdjacentHTML()`를 사용하는 것이 좋다.
- cross site scripting 보안 문제. 사용자 입력 문자열을 innerHTML에 추가해서는 안된다 !! 
    - HTML5에서는 innerHTML로 삽입된 <script> 실행하지 않도록 되어 있음
       - 그러나 다른 방법으로 스크립트가 실행되도록 할 수 있음 - `<img src='x' onerror='alert(1)'>` 등)
    - 문자열 삽입의 경우 textContent를 쓰는 것이 더 권장된다.

### insertAdjacentHTML()
- element의 지정된 위치에 HTML string을 파싱하여 노드 삽입. innerHTML과 달리 elemet 내에 있는 기존의 요소를 건드리지 않기 때문에 더 빠르게 동작한다.
```jsx
element.insertAdjacentHTML(position, text);

<!-- beforebegin -->
<p>
  <!-- afterbegin -->
  foo
  <!-- beforeend -->
</p>
<!-- afterend -->
```
