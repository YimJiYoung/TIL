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
