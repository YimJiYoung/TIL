# CSS

## CSS 연산자 우선 순위 규칙
`id` > `class, [attr], :class(pseudo-classes)` > `element, ::element(pseudo-elements)`

<br/> 

## Inheritance
`color`와 같이 상속 가능한 속성은 값을 지정해주지 않았을 떄 부모 요소의 해당 속성 값을 사용한다.

- 참고: [상속 가능한 CSS 속성 리스트](https://www.sitepoint.com/css-inheritance-introduction/#list-css-properties-inherit)

<br/> 

## Box Model

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
- margin을 음수로 설정하여 원래의 위치보다 sibling 요소에 가까워진다거나 부모 요소를 벗어나게 위치할 수 있다.
   - absoulte position와 달리 음수 margin은 sibling 요소들의 위치에도 영향을 미친다.

#### margin collapsing
- 인접한 block 형제 요소 또는 부모/자식 요소(padding, border 없는 경우) 간의 수직 마진이 병합되는 현상. 
- 양수/음수끼리 만난 경우 절대값이 큰 값 적용이 된다.
- 양수와 음수가 만난 경우 두 값의 합이 적용된다.

<br />

## FLow Layout

### display
> `position: absolute | fixed`이거나 `float: left | right`일 때 `display`는 `block`이 된다.

#### inline
- 흐름 방향 : 수평
- width, height, 수직 마진 적용 X / 수평 마진, 패딩 적용 O (수직 패딩도 적용되긴 하지만 중첩됨)
- img 감싼 block에 이상한 spacing이 생긴다 ? → img는 inline 요소이기 때문에 브라우저가 텍스트로 인식하여 line-height 속성에 맞게 여백이 추가한다.

#### block
- 흐름 방향: 수직
- width, height, 수직, 수평 패딩, 마진 적용 O (수직 마진에서 margin collapsing 발생)

#### inline-block
- 흐름 방향 : 수평
- width, height, 수직, 수평 패딩, 마진 적용 O

#### none
- 레이아웃에서 제외되며 어떤 보조 장치에서도 접근할 수 없다. (접근성 주의!)

### width
- min-conent: 컨텐츠의 최소 크기
- max-content: 컨텐츠의 최대 크기 (no line break)
- fit-content: max-content와 같이 동작하지만 넘치게 될 경우 auto와 같이 동작하며 필요한 경우 line break를 삽입하여 넘치지 않도록 한다.
```caa
/* width: fit-content 적용한 것과 동일 */
    width: auto;
    min-width: min-content;
    max-width: max-content;
```

<br />

## Positioned Layout

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

<br />

## Flex Layout
컨테이너 내 아이템의 배치를 크기, 정렬, 간격 등을 유동적으로 조절하여 레이아웃을 구성할 수 있다.
- main axis: `flex-direction`에 의해 결정되며 flex 컨네이너 내의 아이템이 main axis에 따라 흐르며 배치된다.
![image](https://css-tricks.com/wp-content/uploads/2018/11/00-basic-terminology.svg)

### Flex Container 속성
```css
.flex-container {
  display: flex;					/* 컨테이너 선언 */
  flex-flow: row nowrap;       /* flex-direction + flex-wrap */
  flex-direction: row;			/* 흐름 방향 */
  flex-wrap: nowrap;			/* 줄 바꿈 */
  justify-content: flex-start;		/* main axis 정렬 */
  align-items: stretch;			/* cross axis 정렬 */
  align-content: stretch;			/* 여러 줄의 cross axis 정렬 */
  gap: 10px 20px; /* row-gap column gap */
  row-gap: 10px;
  column-gap: 20px;
}
```
#### gap
- 플렉스, 그리드, 다중 컬럼 아이템 사이의 간격을 지정한다.

### Flex Item 속성
```css
.flex-item {
  flex: 0 1 auto;          /*  flex-grow + flex-shrink + flex-basis */
  flex-grow: 0;			/* 팽창지수 */
  flex-shrink: 1;		/* 수축지수 */
  flex-basis: auto;		/* 기준 크기 */
  align-self: auto;		/* 독립적 cross axis 정렬 */
  order: 0;				/* 배치 순서 */
}

```

> free space는 플렉스 아이템이 점유하는 영역(`flex-basis`, `width`, `height`, `padding`, `border`, `margin`)을 제외하고 남은 공간을 말한다.

### flex grow
- 양의 free space 발생 시 플렉스 아이템이 얼마나 공간을 할당받을 것인지(팽창할 것인지) 지정
- 초기값 : 0, flex 속성에서 생략 시 1

### flex shrink
- 음의 free space 발생 시 플렉스 아이템이 얼마나 수축할 것인지 지정
- 초기값 : 1, flex 속성에서 생략 시 1

### flex basis
- 아이템의 진행 방향의 기본 크기 지정
- 초기값 : auto, flex 속성에서 생략 시 0

### flex
- `none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]`
- `none`: 0 0 auto

<br/> 

## 그외 CSS 속성

#### flow-root


### float
![image](https://user-images.githubusercontent.com/37496919/136690720-6a132ce0-fdca-410a-9cc4-0c71451d0d78.png)
- 인라인 요소(텍스트 포함)이 플로팅 요소 주변으로 흐르게 한다. (블록 요소는 겹침)
- 플로팅 요소의 height는 컨테이너 요소에 포함되지 않기 때문에 floaging 해제가 필요하다.
   - `display: flow-root`

### word-break
텍스트의 길이가 컨텐츠보다 커졌을 때 줄바꿈이 일어날 것인지 지정한다. 
기본적으로 non-CJK(Chinese, Japanes, Korean) 언어의 경우는 공백 문자에서, CJK의 경우는 음절 사이에서 줄바꿈이 일어날 수 있다.
이런 특성 때문에 word-break: normal(기본값)일 때, 한국어의 경우 단어 사이에서 의도치 않게 줄바꿈이 일어나는데 keep-all로 설정하면 단어 사이에서 줄바꿈이 일어나지 않도록 설정할 수 있다.
> 하지만 일본어나 중국어의 경우는 띄어쓰기를 사용하지 않기 떄문에 keep-all을 설정했을 때 overflow가 발생할 수 있다 .. 
```css
:lang(ko) {
  word-break: keep-all;
}
```
- 참고: [word-break 속성과 word-wrap 속성 알아보기](https://wit.nts-corp.com/2017/07/25/4675)





