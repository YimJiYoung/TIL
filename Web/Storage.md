## Web Storage 
브라우저의 내부 스토리지를 이용할 수 있도록 localStorage, sessionStorage 기능이 HTML5에 도입되었다.
localStorage는 데이터를 수동으로 지울 때까지 남아있지만 sessionStorage의 데이터는 윈도우나 탭을 닫았을 때 사라진다.(세션동안만 유지) 
기존의 쿠키도 저장소 역할을 하는데 이런 스토리지 기능이 도입된 것은 쿠키의 제한점 때문이다. 
쿠키는 4kb의 데이터 제한이 있고 서버 요청을 할 때마다 포함되므로 웹이 느려지는 원인이 될 수 있다.
이에 대한 문제점을 보완하기 위하여 용량을 확장하고, 서버로는 전달이 되지 않고 브라우져 로컬에만 저장될 수 있도록 새로운 표준을 정의한 것이 바로 웹 스토리지이다.



### localStorage
- key/value 형태로 저장. 값은 null, undefined, Object 등 저장할 수 있지만 다 문자열로 변환되어 저장된다.
따라서 객체를 저장할때는 JSON.stringfy()를 사용하고 조회할 때도 마찬가지로 JSON.parse() 해줘야 함.
- 용량 5mb ~ 10mb (데스크탑)
```javascript
localStorage.setItem('myCat', 'Tom'); 
const cat = localStorage.getItem('myCat'); 
localStorage.removeItem('myCat');
localStorage.clear();
```


## 참고
- https://unikys.tistory.com/352 
- https://www.zerocho.com/category/HTML&DOM/post/5918515b1ed39f00182d3048
