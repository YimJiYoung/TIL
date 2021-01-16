## Redux

### MVC
기존의 MVC 패턴에서는 프로젝트의 규모가 커지고 모델과 뷰 사이의 의존성이 
다음과 같이 복잡한 형태로 되어있을 때 데이터의 흐름이 양방향으로 되어 있어 예측하기(디버깅하기) 어렵다라는 단점을 갖고 있다.
- MVC에서도 데이터를 단방향으로 흐르게 할 수 있지 않을까? 왜 컨트롤러는 하나밖에 없는가 ..
- 관련 논의 참고: https://blog.coderifleman.com/2015/06/19/mvc-does-not-scale-use-flux-instead/
![MVC](https://blog.kakaocdn.net/dn/ALrHe/btqBTMSuHfN/ZlW9i9ET34e90APgCRChk1/img.png)

### Flux
Flux는 데이터를 단방향으로 흐르게 하여 예측가능하도록 만든다.
- action : 애플리케이션의 상태를 변경하거나 뷰를 업데이트하고 싶다면 액션을 생성해야 한다. 액션 생성자는 타입(type)과 페이로드(payload)를 포함한 액션을 생성한다. 
- dispatcher : 디스패쳐는 액션을 보낼 필요가 있는 모든 스토어(store)를 가지고 있고, 액션 생성자로부터 액션이 넘어오면 여러 스토어에 액션을 보낸다.
- store : 스토어는 애플리케이션 내의 모든 상태와 그와 관련된 로직을 가지고 있다. 스토어에 상태 변경을 완료하고 나면, view에게 알려 새로 렌더링하도록 한다.
- view: 유저와 인터렉션하면서 상태를 변경할 필요가 있으면 action을 생성해 dispatcher에게 전달한다.

![FLUX](https://media.vlpt.us/images/shaqok/post/1d43a57f-6773-4544-917d-679def1526a5/image.png)


## 참고
- http://bestalign.github.io/2015/10/06/cartoon-guide-to-flux/
- https://blog.coderifleman.com/2015/06/19/mvc-does-not-scale-use-flux-instead/
