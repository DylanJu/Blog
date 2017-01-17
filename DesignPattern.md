# Design Pattern Pattern.md

디자인패턴의 정의


내가 생각하는 디자인 패턴
디자인 패턴은 철학이다. There is no answer but various choice to choose what is the best to you and computer.

MVC Model
1. Model: 데이터를 직접 조작 하는 곳
2. Contoroller: Model에 지시를 내리는 곳
3. View: 현재의 데이터를 어떻게 보여줄지 정하는 곳

Flux
1. Store
2. Dispatcher
3. Action
4. View
5. Redux

Pub/Sub
Pub-sub: 발행-구독. 발행자는 결과를 따로 전송할 필요 없다. 구독자가 자동으로 받아가는 시스템. 즉 store에만 return 하면 subscriber가 알아서 changed data를 반영한다.
(pub/sub 하위) broadcast: 자기 이외에 모두에게 payloads(=data??) 전송




https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modularjavascript
번역

패턴은 소프트웨어 엔지니어링에서 일반적으로 발생하는 문제에 적용할 수 있는, 재사용할 수 있는 솔루션이다. 이번 케이스에서는 자바스크립트로 프로그래밍하는 부분에 대해 다루겠다.
디자인 패턴을 바라보는 또 다른 방법은 우리가 문제(각기 다른 상황의)를 해결하는 일종의 템플릿으로 보는 것이다.

이제 왜 디자인패턴을 이해하고 친숙하게 만드는 것이 중요한지 알았나? 디자인 패턴은 세 가지 이점을 가지고 있다.
1. 패턴들은 이미 입증된 해결책이다.
디자인 패턴은 확고한 해결책을 제시한다.

////////////////

MVC 모델
- Model: Domain-specific data를 나타내고, 유저 인터페이스(Views, Contorollers)를 무시한다. 모델이 바꾸면 이것을 옵저버에게 알려준다.
- View: 모델의 현재 상태를 나타낸다. 모델이 업데이트 되거나 변경되면 옵저버패턴이 View가 바로 알 수 있게 한다.
- 화면Presntation은 View에 의해 처리되지만 단지 a Single View나 Contoroller가 처리하는 것은 아니다. 각각의 섹션이나 엘리먼트가 스크린에 나타나야 할때는 View-Controller pair가 요구된다.
- Contoroller: 이 View-Controller pair에서 컨트롤러의 역할은 View에 대한 의사결정을 하면서 유저 인터랙션을 핸들링 하는 것이다.

많은 개발자들은 MVC 패턴을 배울 때 옵저버패턴이 이미 수십년 전에 MVC 개념의 일부분이 된 것을 알게되고 놀란다. 80년대 MVC인 스몰토크에서 뷰는 모델을 observe한다. 그래서 위에 불릿포인트에서 설명했듯 모델이 변할때마다 뷰가 변한다.
