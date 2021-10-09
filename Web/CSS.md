# CSS

## CSS 연산자 우선 순위 규칙
`id` > `class, [attr], :class(pseudo-classes)` > `element, ::element(pseudo-elements)`

### display
> `position: absolute | fixed`이거나 `float: left | right`일 때 `display`는 `block | flex | grid | table`이 된다. ([참고](https://developer.mozilla.org/en-US/docs/Web/CSS/float))

#### inline
- 흐름 방향 : 수평
- width, height, 수직 마진 적용 X / 수평 마진, 패딩 적용 O (수직 패딩도 적용되긴 하지만 중첩됨)

#### block
- 흐름 방향: 수직
- width, height, 수직, 수평 패딩, 마진 적용 O (수직 마진에서 margin collapsing 발생)

#### inline-block
- 흐름 방향 : 수평
- width, height, 수직, 수평 패딩, 마진 적용 O

#### none
- 레이아웃에서 제외되며 어떤 보조 장치에서도 접근할 수 없다. (접근성 주의!)

#### flow-root

#### flex

#### grid

### position

#### static
- 문서의 일반적인 흐름대로 배치되며 left, right, top, bottom, z-index 속성 효과 X

#### relative
- 문서의 일반적인 흐름대로 배치된 위치를 기준으로 베치된다.
- 배치의 변경은 다른 요소의 위치에 영향을 미치지 않는다.
- `inset` 속성으로 offset 관련 속성(left, right, top, bottom) 한번에 정의 가능

#### absolute
- 문서의 일반적인 흐름에서 완전히 이탈한다. (이 요소를 위한 공간이 따로 할당되지 않음)
- `position: relative | absolute |fixed`인 부모 요소 또는 [컨테이닝 블록](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block) 기준으로 배치된다.

#### fixed
- 문서의 일반적인 흐름에서 완전히 이탈한다. (이 요소를 위한 공간이 따로 할당되지 않음)
-  `transform`, `perspective`, `filter` 속성을 가진 부모 요소 또는 뷰포트를 기준으로 배치된다.
 
#### sticky
-  스크롤 포트(`overflow: hidden | scroll | auto | overlay`)를 기준으로 배치된다.
-  컨테이닝 블록이 스크롤 포트에 보이는 동안 고정되었다가 스크롤 밖으로 이탈하거나 [sticky 요소가 컨테이닝 블록의 끝에 도달했을 때](https://stackoverflow.com/questions/49848196/position-sticky-not-working-when-height-is-defined) 고정을 멈춘다.

### box-sizing
![image](https://user-images.githubusercontent.com/37496919/136652041-0b427187-8ac3-4a9f-bfe8-c0d1aefc71a2.png)
- content-box: 기본값. content 영역을 기준으로 width와 hegiht이 지정되기 때문에 padding을 추가하면 지정한 커진다.
- border-box: padding과 border 영역까지 포함하여 width와 height 지정.

### padding, margin
- 퍼센트로 값을 지정하면 컨테이너의 너비가 기준이 된다.
  - 이를 이용해서 종횡 비율를 유지하는 요소를 만들 수 있다.
```css
.parent {
  position: relative;
  padding-top: 56.25%; /* 315 / 560 * 100 */
}
.child {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
}
```

### word-break
텍스트의 길이가 컨텐츠보다 커졌을 때 줄바꿈이 일어날 것인지 지정한다. 
기본적으로 non-CJK(Chinese, Japanes, Korean) 언어의 경우는 공백 문자에서, CJK의 경우는 음절 사이에서 줄바꿈이 일어날 수 있다.
이런 특성 때문에 word-break: normal(기본값)일 때, 한국어의 경우 단어 사이에서 의도치 않게 줄바꿈이 일어나는데 keep-all로 설정하면 단어 사이에서 줄바꿈이 일어나지 않도록 설정할 수 있다.
> 하지만 일본어나 중국어의 경우는 띄어쓰기를 사용하지 않기 떄문에 keep-all을 설정했을 때 overflow가 발생할 수 있다 .. 
```
:lang(ko) {
  word-break: keep-all;
}
```




## 참고
- [word-break 속성과 word-wrap 속성 알아보기](https://wit.nts-corp.com/2017/07/25/4675)
