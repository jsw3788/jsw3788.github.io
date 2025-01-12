## 프론트 컨트롤러 패턴

- 특징

  - 공통 로직을 포함하는 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받은 후 각 요청에 맞는 컨트롤러를 찾아서 호출한다.

  - 나머지 컨트롤러는 서블릿을 사용하지 않아도 된다.

  - 스프링 MVC의 DispatcherServlet이 FrontController 패턴으로 구현되어 있다.

     

## 모델을 추가한 프론트 컨트롤러의 동작 흐름(v3)

1. 프론트 컨트롤러는 서블릿을 상속받는다.

2. WebServlet의 urlpatterns에 의해 프론트 컨트롤러가 생성되며 이 때 controllerMap에 각 url과 컨트롤러 정보가 key, value 형태로 들어간다.

3. request url과 일치하는 컨트롤러가 프론트 컨트롤러의 컨트롤러 인스턴스에 담긴다.

4. createParamMap 함수는 http request의 정보를 paramMap에 key, value 형태로 담고 반환한다.

   ex) username = kim , age = 20

5. 컨트롤러에서 paramMap의 정보를 이용하여 로직을 처리하고 호출하고자 하는 뷰의 논리적 이름(viewName)을 담은 ModelView로 반환한다.

6. viewResolver 함수에서 논리적 이름을 물리적 이름(jsp)으로 바꿔준다.

7. 물리적 이름에 렌더링한다. (jsp에 model, request, response 정보를 넘겨준다.)



## 어댑터 패턴

- 발단 : V4까지의 프론트 컨트롤러는 하나의 컨트롤러 버젼에 종속되어있다. import나 controllerMap의 key, value의 이름등에 V4가 담겨 있고 하나의 컨트롤러 패턴만 사용해야한다. 개발자의 요구사항에 따라 다양한 방식의 컨트롤러를 사용하기 위해서 어댑터 패턴을 도입한다.

- 핸들러 어댑터 : 다양한 종류의 컨트롤러를 호출하는 역할을 함

- 핸들러 : 컨트롤러 뿐만 아니라 다른 것들도 해당하는 종류의 어댑터만 있으면 처리할 수 있다는 의미에서 컨트롤러의 의미를 확장하여 핸들러로 칭함

- 강의에서는 핸들러와 컨트롤러는 동일한 의미로 사용해도 무방함

- 강의에서 프론트 컨트롤러와 컨트롤러의 불일치를 조정하는 역할을 함

  