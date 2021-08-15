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
- https://wit.nts-corp.com/2017/07/25/4675
