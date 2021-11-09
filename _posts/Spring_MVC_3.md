## DispatcherServlet

- 스프링 MVC도 프론트 컨트롤러 패턴으로 구현되어 있고 그 프론트 컨트롤러가 DispatcherServlet이다.

- HttpServlet을 상속 받으며 스프링 부트가 자동으로 등록한다.

- `urlPatterns="/"`로 모든 경로에 대해서 매핑되는데 더 자세한 경로가 등록될 수록 우선순위가 높다.

  따라서, DispatcherServlet은 우선순위가 낮다.

  

## Handler

- HandlerMapping

  - 스프링 빈의 이름으로 핸들러를 찾을 수 있는 핸들러 매핑이 필요하다.

  - 0순위 - `RequestMappingHandlerMapping` : 애노테이션 기반의 컨트롤러인 `@RequestMapping`에서 사용

  - 1순위 - `BeanNameUrlHandlerMapping` : 스프링 빈의 이름으로 핸들러 조회, `@Component`로 등록된 이름 조회

    

- HandlerAdapter

  - 핸들러 매핑을 통해서 찾은 핸들러를 실행할 수 있는 핸들러 어댑터가 필요하다.
  - 0순위 - `RequestMappingHandlerAdapter` : `@RequestMaping`에서 사용
  - 1순위 - `HttpRequestHandlerAdpater` : HttpRequestHandler 처리
  - 2순위 - `SimpleControllerHandlerAdapter` : Controller 인터페이스 처리

- 실제 개발에서는 대부분 `@RequestMapping` 을 사용한다.



## ViewResolver

- 스프링 부트는 `InternalResourceViewResolver`라는 ViewResolver를 자동으로 등록하는데 이 때, `resource/application.properties`에 등록한 `spring.mvc.view.prefix`, `spring.mvc.view.suffix` 설정 정보를 사용해서 등록한다. => (prefix + viewName + suffix)

  - 아래 코드가 내부적으로 등록됨

  ```java
  @SpringBootApplication
  public class ServletApplication{
      @Bean
      ViewResolver internalResourceViewResolever() {
          return new InternalResourceViewResolver(prefix, suffix);
      }
  }
  ```

  - 스프링 부트가 자동으로 등록하는 ViewResolver
    - 0순위 - `BeanNameViewResolver` : 빈 이름으로 뷰를 찾아서 반환한다.
    - 1순위 - `InternalResourceViewResolver` : 내부에서 찾을 수 있는 자원(JSP)를 처리할 수 있는 뷰를 반환한다.
  - Tymeleaf 뷰 템플릿을 사용하면 ThymeleafViewResolver를 스프링부트가 자동으로 등록해준다.

  

  

  ## 애노테이션

  - `@Controller` : 스프링이 자동으로 스프링 빈으로 등록하며 컨트롤러(핸들러)로 인식한다.

  - `@RequestMapping` : 요청 정보를 매핑하며 해당 URL이 호출되면 메서드가 호출된다.

    - `@RequestMapping(value, method)`  : value에는 url이 들어가고 method에는 허용할 HTTP Method가 들어간다. ex) `@RequestMapping(value="/save", method = RequestMethod.POST)` 
    - `@GetMapping`, `@PostMapping`을 사용하면 더 편리하다.
    - `@RequestMapping`을 클래스 레벨에 붙인 후 그 클래스의 메서드에 `@RequestMapping`을 사용하면 클래스 레벨에서 지정한 url 뒤에 메서드에서 정한 url이 붙는다. 따라서, url 중복을 줄일 수 있고 메서드에서 더이상의 url 이 필요없는경우 `@RequestMapping`만 쓰면 된다.

    

  - 위의 둘 중 하나가 클래스 레벨에 붙어있으면 `RequestMappingHandlerMapping`이 매핑 정보로 인식한다.

  - `@RequestParam` : HTTP 요청 파라미터를 받을 수 있다. `@RequestParam("username")` 은 `request.getParameter("username")`에 기능이 몇 가지 더 있는 것이다.

  

