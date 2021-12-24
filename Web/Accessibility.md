# Accessibility

## [ARIA(Accessible Rich Internet Application)](https://www.w3.org/TR/html-aria/)
HTML만으로는 부족한 정보를 보완하기 위해 역할(roles), 상태(states), 속성(properties) 정보를 부여하여 보조기기의 웹 문서 접근을 지원한다.
- ARIA 사용에 앞서 HTML을 충분히 시맨틱하게 작성했는지 검토해보자

### roles
```html
<element role="tablist"> ⭐
<element role="tab"> ⭐
<element role="tabpanel"> ⭐
<element role="tooltip"> ⭐
<element role="status"> ⭐
<element role="alert"> ⭐
```
- `tablist | tab | tabpanel`
- `tooltip`: 마우스 호버되거나 owning element가 포커스를 받았을 때 설명을 표시하는 문맥 팝업 
  - tooltip role을 가진 요소는 `aria-describedby`를 사용하여 레퍼런스(연관)되어야 한다.
```html
<button aria-describedby="TIP-DEL">게시물 삭제</button>
<p id="TIP-DEL" role="tooltip" hidden>게시물 삭제 후 복원할 수 없음.</p
```
- `status`: `alert`만큼 중요하진 않지만 실시간 결과와 같은 정보를 전달
   - aria-live="polite", aria-atomic="true"으로 설정됨
- `alert`: 시간에 민감하고 오류와 같은 중요한 실시간 정보를 전달
- 참고 : [가능한 role 목록](https://www.w3.org/TR/wai-aria-1.1/#role_definitions)

### states
```html
<element aria-current="page|step|location|date|time|true|false(default)"> ⭐
<element aria-selected="false|true|undefined(default)"> ⭐
<element aria-haspopup="true|menu|dialog|listbox|tree|grid|false(default)"> ⭐
<element aria-expanded="true|false|undefined(default)"> ⭐
<element aria-hidden="true|false|undefined(default)"> ⭐
<element aria-invalid="true|false(default)|grammar|spelling"> ⭐
```
- `aria-hidden`: 요소의 접근성 API 차단 여부 결정
 - 모달 대화상자를 화면에 표시할 때 모달 대화상자 뒤 본문 콘텐츠에 aria-hidden="true" 속성을 선언하면 보조기기가 무시하며 접근할 수 없게 된다.

### properties
```html
<element aria-controls="ID reference list"> ⭐
<element aria-live="polite|assertive|off(default)"> ⭐
<element aria-labelledby="ID reference list"> ⭐
<element aria-label="string">
<element aria-describedby="ID reference list">
<element aria-errormessage="ID reference"> ⭐
<element aria-modal="true|false(default)"> ⭐
```
- `aria-controls`: 현재 요소에 의해 컨트롤되는 대상 명시
  - ex) tab 요소는 다른 tabpanel 요소를 컨트롤 하므로 tabpanel의 id를 aria-controls로 설정한다.
- `aria-live`
- `aria-labelledby`: 현재 요소를 나타내는(label) 대상 명시
  - aria-label보다 우선순위가 높음
- `aria-label`: 현재 요소를 나타내는 값 지정
  - 현재 요소를 나타내는 텍스트가 존재할 경우 aria-labelledby 사용을 권장한다.
- `aria-describedby`: 현재 요소를 설명하는 대상 명시
  - 주로 툴팁, 링크(a), 폼 콘트롤(input, textarea, select, button), 알럿(role="alert"), 알럿 대화상자(role="alertdialog") 요소에 사용
- `aria-modal`: 요소가 모달인지 나타낸다 (모달 요소는 페이지의 다른 컨텐츠를 사용할 수 없게 막는다)


## 자료
- https://dev.to/programmingduck/how-to-learn-web-accessibility-1d9k
