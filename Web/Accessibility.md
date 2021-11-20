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


## 자료
- https://dev.to/programmingduck/how-to-learn-web-accessibility-1d9k
