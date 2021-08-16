## 의존관계 자동 주입

- 다양한 의존관계 주입 방법(`@Autowired`를 어디에 붙일건지)

  - 생성자 주입

    - 생성자 호출 시점에 딱 1번만 호출되는 것이 보장된다.
    - 주로 **불변, 필수** 의존관계에 사용 (생성시에만 조작될 때 불변, 객체 선언시 final을 사용할 때 필수)
    - 스프링 빈의 경우 **생성자가 1개**만 있으면 `@Autowired`는 생략해도 자동으로 주입한다.
    - **불변** : 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없다.
    - **누락** : 생성자 주입을 사용하면 주입 데이터를 누락 했을 때 **컴파일 오류**가 발생하고 어떤 값을 필수로 주입해야하는지 알 수 있다.
    - **필수** : 생성자 주입을 사용하면 필드에 `final` 키워드를 사용할 수 있다. 생성자에서 값이 설정되지 않는 오류를 컴파일 시점에 막아준다. 생성자 주입만 `final` 사용가능하다.
    - 컴파일 오류 : 유지보수하다가 문제가 생기면 오류를 찾고 수정하기 힘드므로 컴파일 시점에 알려주는 것이 중요하다. 컴파일 오류가 가장 빠르고 좋은 오류다

    

  - 수정자 주입(setter 주입)

    - **선택, 변경 가능성이 있는** 의존관계에 사용한다.

    - `@Autowired(required=false)`를 통해 선택적으로 주입 (주입할 대상이 없어도 동작하게 할 때도 사용함)

    - 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법

      - 자바빈 프로퍼티  규약 : 자바에서 필드의 값을 직접 변경하지 않고, setXxx, getXxx라는 메서드를 통해서 값을 읽거나 수정하는 규칙

      

  - 필드 주입

    - 외부에서 변경이 불가능해서 테스트 하기 힘들다는 단점이 있다.
    - DI 프레임워크가 없으면 아무것도 할 수 없다.
    - 안티패턴이지만 애플리케이션의 실제 코드와 관계 없는 테스트 코드 스프링 설정을 목적으로 하는 `@Configuration` 같은 곳에서만 특별한 용도로 사용한다.

    

  - 일반 메서드 주입

    - 한번에 여러 필드를 주입 받을 수 있다.
    - 잘 사용하지 않는다.

    ```java
    @Autowired
    public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy){
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
    ```

- 옵션 처리

  >- `@Autowired(required = false)` : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
  >
  >- `org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 null이 입력된다.
  >- `Optional<>` : 자동 주입할 대상이 없으면 `Optional.empty`가 입력된다.



- 롬복

  - Getter, Setter, 생성자등을 자동으로 만들어주는 라이브러리

    ```java
    package hello.core;
    
    import lombok.Getter;
    import lombok.Setter;
    import lombok.ToString;
    
    @Getter
    @Setter
    @ToString
    public class HelloLombok {
    
        private String name;
        private int age;
    
        public static void main(String[] args) {
            HelloLombok helloLombok = new HelloLombok();
            helloLombok.setName("abc");
    
            String name = helloLombok.getName();
            System.out.println("name = " + name);
            System.out.println("helloLombok = " + helloLombok);
        }
    }
    // name = abc
    // helloLombok = HelloLombok(name=abc, age=0)
    ```

  - `@RequiredArgsConstructor`를 사용하면 final이 붙은 필드를 모아서 생성자를 자동으로 만들어줌 (인텔리제이에서 작성한 코드에서 Ctrl + F12로 생성자 확인 가능)

  

- 타입으로 조회하여 선택된 빈이 2개 이상일 때

  - 예를 들어 `DiscountPolicy`의 하위 타입인 `FixDiscoutPolicy`, `RateDiscoutPolicy` 를 동시에 스프링 빈으로 선언하면 `NoUniqueBeanDefinitionException` 오류가 발생한다.

  - 해결 방법

    - `@Autowired 필드 명 매칭` :  타입 매칭을 시도하고 타입 매칭의 결과가 2개 이상일 때(타입을 상속한 자식들까지 모두 긁어옴) 필드 이름이나 파라미터 이름으로 빈 이름을 매칭함

      ```java
      // @Autowired
      // private DiscountPolicy discountPolicy
      
      @Autowired
      private DiscountPolicy rateDiscountPolicy
      
      ```

    

    - `@Qualifier` : 추가 구분자를 붙여주는 방법, 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것이 아님

      ```java
      @Autowired
          public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
              this.memberRepository = memberRepository;
              this.discountPolicy = discountPolicy;
          }
      ```

    - `@Primary` : 우선순위를 정해준다. 우선권을 주고 싶은 빈에 붙여준다. 가장 편한 방법이다.

    

    - `@Qualifier`는 `@Primary`보다 우선권이 높다.

    

    - 메인 빈과 서브 빈이 있다면 메인 빈에는 `@Primary`를 붙여줘서 기본값으로 지정하고 서브빈에는 `@Qualifier`를 붙여줘서 사용할 곳을 명시하는 것이 좋다.

    

- 애노테이션 직접 만들기

  - 애노테이션의 파라미터(ex.`@Qualifier`)는 문자이므로 오타 등으로 인해서 오류가 발생해도 컴파일 타임에 캐치할 수 없다는 문제가 있다. 이를 해결하기 위해 애노테이션을 직접 만든다.
  - `@MainDiscountPolicy`라는 애노테이션을 만들고 그 안에 원래 사용하고자했던 `@Qualifier("mainDiscountPolicy")`를 넣어준다.
  - 애노테이션은 상속이 안되므로 `@Qualifer`가 품은 애노테이션을 `@MainDiscountPolicy`에 넣어서 사용하며 필요에 따라 다른 애노테이션들도 조합해서 사용 가능하다.

  

- 조회한 빈이 모두 필요할 때 List, Map
  - 디자인패턴 중 전략 패턴으로 해결하는 방법
  - DiscountService는 Map으로 `DiscountPolicy`를 통해 구현체들을 주입받는다.
  - `discount()` 메서드는 인자로 넘어온 구현체 클래스를 map에서 동일한 이름의 스프링 빈을 찾아서 실행한다.
  - `Map<String, DiscountPolicy>` : map의 키에 스프링 빈의 이름을 넣어주고 그 값으로 `DiscountPolicy` 타입으로 조회한 **모든** 스프링 빈을 담아준다.
  - `List<DiscountPolicy>` : `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담아준다.

  
  
- 자동 빈 등록 vs 수동 빈 등록

  - 기본으로는 자동 기능을 사용하는 것이 편리하고 스프링 부트의 스프링 빈들도 자동으로 등록하도록 설계되어있다.

  - 스프링 빈의 갯수가 많아질수록 일일이 수동 등록하는 것보다 `@Component`를 사용하여 자동등록 하는 것이 편하고 효율적이기때문이다.

  - 자동 빈 등록을 사용해도 OCP, DIP를 지킬 수 있다.

  - **업무 로직 빈**

    -  웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 지원하는 리포지토리 등이 있다.

    - 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.

    - 숫자가 매우 많고 어느정도 유사한 패턴이 있으므로 **자동** 기능을 사용하는 것이 좋다. 보통 문제가 발생해도 문제가 어디서 발생했는지 파악하기 쉽다.

    - **수동** 빈 등록이 필요한 경우 : 다형성을 적극 활용할 때 사용한다. List, Map을 사용하여 조회한 빈을 모두 사용하는 경우 어떤 구현체(빈)들을 가져오는지 한눈에 파악하기 힘들다. 이런 경우 수동 빈으로 등록하거나 자동으로 하게된다면 **특정 패키지에 함께 묶어두는 것**이 좋다.

      

  - **기술 지원 빈**

    - 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. DB 연결이나 공통 로그 처리처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술이다.
    - 상대적으로 수가 적고 보통 애플리케이션 전반에 걸쳐서 광범위하게 영향을 미친다. 적용여부조차도 파악하기 어렵기때문에 **수동** 빈 등록을 통해 명확하게 들어내는 것이 좋다.
    - 설정 정보에 나타나게 한다.
    - **스프링과 스프링 부트가 자동으로 등록하는 빈**들은 기술지원 로직을 포함하고 있더라도(DataSource는 DB 연결에 사용하는 기술을 포함) 그대로 사용하는 것이 좋다.

    

## 빈 생명주기 콜백

- 초기화 작업 : 데이터베이스 커넥션 풀 연결, 네트워크 소켓 연결 등이 있다. 객체 생성하는 작업을 말하는 것이 아니다. 정상적으로 종료되도록 설계하는 것이 좋다.

- 스프링 빈은 객체를 생성하고 의존관계 주입이 완료된 후에 필요한 데이터를 사용할 수 있다. 따라서 **초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출**해야 한다. 스프링은 의존관계 주입이 완료되면 스프링 빈에게 **콜백 메서드를 통해서 초기화 시점을 알려주는** 다양한 기능을 제공한다. 스프링은 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 준다. 따라서 안전하게 종료 작업을 진행할 수 있다.

- (싱글톤) 스프링 빈의 이벤트 라이프사이클

  > 스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸 직전 콜백 -> 스프링 종료

- 객체의 생성과 초기화를 분리
  - 생성자는 파라미터를 받고 메모리를 할당해서 객체를 생성하도록 하고 초기화는 이렇게 생성된 값들을 활용해서 외부 커넥션을 연결하는 등 무거운 동작을 수행할 수 있도록 한다. 초기화 작업이 복잡한 경우에는 생성자와 초기화 작업을 명확하게 나누는 것이 유지보수 관점에서 바람직하다.

- 방법

  - 초기화, 소멸 인터페이스

    - `InitializingBean`(초기화 인터페이스)는 `afterPropertiesSet()`메서드로 초기화를 지원한다.
    - `DisposableBean`(소멸 인터페이스)는 `destroy()`메서드로 소멸을 지원한다.
    - 단점
      - 스프링 전용 인터페이스이므로 해당 코드는 스프링 전용 인터페이스에 의존한다.
      - 초기화, 소멸 메서드의 이름을 변경할 수 없다.
      - 코드를 고칠 수 없는 외부 라이브러리에 적용할 수 없다.
    - 아래 방법들에 비해서 잘 사용하지 않는다.

    

  - 빈 등록 초기화, 소멸 메서드

    - 설정 정보(XXXConfig)에 `@Bean(initMethod = "{init 메서드명}", destroyMethod ="{close 메서드명}")`로 초기화, 소멸 메서드 지정
    - 메서드 이름을 자유롭게 줄 수 있다.
    - 스프링 빈이 스프링 코드에 의존하지 않는다.
    - 설정 정보를 사용하기 때문에 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용 가능하다.
    - **추론기능** : 종료메서드(`destroyMethod`)의 기본값이 `(inferred)`로 등록되어 있다. 라이브러리는 대부분 `close()`, `shutdown()`라는 이름의 종료 메서드를 사용하는데 기본값은 이를 추론해서 자동으로 호출해준다. 추론 기능이 필요 없다면 `destoyMethod=""`를 지정하면 된다.

    

  - 애노테이션

    - 초기화 메서드에 `@PostConstruct`, 소멸 메서드에 `@PreDestroy`를 달아준다.
    - 스프링에서 가장 권장하는 방법이고 매우 편리하다.
    - javax에서 지원하므로 스프링이 아닌 컨테이너에도 적용 가능하다.
    - 외부 라이브러리에는 적용하지 못한다. 이 때는 `@Bean`을 활용한 위의 기능을 사용하면 된다.



## 빈 스코프

- 스코프의 종류
  - 스코프 : 빈이 존재할 수 있는 범위

  - 싱글톤 스코프 (`@Scope("singleton")`): 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프

  - 프로토타입 스코프: 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관여하지 않는 매우 짧은 범위의 스코프

  - 웹 스코프

    

- 프로토타입 스코프

  - 프로토타입 스코프를 스프링 컨테이너에서 조회하면 스프링 컨테이너는 항상 새로운 인스턴스를 생성해서 반환한다.
  - 프로토타입 빈 요청 과정
    1. 클라이언트가 프로토타입 스코프의 빈을 스프링 컨테이너에 요청한다.
    2. 요청을 받으면 스프링 컨테이너는 프로토타입 빈을 생성하고 필요한 의존관계를 주입한다.
    3. 스프링 컨테이너는 생성한 프로토타입 빈을 클라이언트에 반환하고 더 이상 관리하지 않는다.
    4. 이후에 스프링 컨테이너에 같은 요청이 오면 항상 새로운 프로토타입 빈을 생성해서 반환한다.

  - 스프링 컨테이너는 프로토타입 빈의 생성, 의존관계 주입, 초기화를 처리하고 관리하지는 않는다. 프로토타입 빈을 관리하는 것은 클라이언트이며 따라서 `@PreDestroy` 같은 스프링 컨테이너의 종료 메서드는 호출되지 않는다.(필요하다면 `prototypeBean1.destroy()`처럼 수동으로 호출한다.)
  - 싱글톤 빈은 스프링 컨테이너 생성 시점에 초기화 메서드가 실행된다면 프로토타입 스코프 빈은 스프링 컨테이너에서 빈을 조회할 때 생성되고 초기화 메서드가 실행된다.

  - 싱글톤 빈과 프로토타입 빈을 함께 사용시 발생하는 문제점

    - 싱글톤 빈에서 프로토타입 빈을 주입받는 형태로 사용하게 되면 싱글톤 빈이 주입받을 때만 프로토타입 빈이 생성됨

    - 이는 요청을 받을때마다 새로운 인스턴스를 생성하려는 프로토타입 빈을 사용하려는 의도에서 벗어남

    - 해결방법

      1. Dependency Lookup(DL, 의존관계 조회(탐색)) 

         > - 의존관계를 외부에서 주입(DI)받지 않고 직접 필요한 의존관계를 찾는 것
         > - 요청을 받는 메서드나 로직에 프로토타입 빈을 조회하는 코드 `ac.getBean(PrototypeBean.class)`를 넣어서 항상 새로운 프로토타입 빈이 생성되도록 함
         > - 이렇게 스프링 애플리케이션 컨텍스트 전체를 주입받게 되면 스프링 컨테이너에 종속적인 코드가 되고 단위 테스트도 어려워진다.
         > - DL 이외에도 애플리케이션 컨텍스트의 다른 기능들까지 모두 끌어오게 된다.

      2. ObjectFactory, ObjectProvider

         

         ```java
         @Autowired
         private ObjectProvider<PrototypeBean> prototypeBeanProvider;
         
         public int logic() {
             PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
             prototypeBean.addCount();
             int count = prototypeBean.getCount();
             return count;
         }
         ```

         >- `ObjectFactory` : 기능이 단순, 스프링에 의존, DL 기능 제공, 별도의 라이브러리 필요 없음
         >- `ObjectProvider`: `ObjectFactory` 상속 , 옵션이나 스트림 처리등 편의 기능이 많음 스프링에 의존, DL기능 제공, 별도의 라이브러리 필요 없음
         >- `prototypeBeanProvider.getObject()`을 통해서 항상 새로운 프로토타입 빈을 생성
         >- `ObjectProvider`의 `getObject()`를 호출하면 내부에서 스프링 컨테이너를 통해 해당 빈을 찾아서 반환
         >- 프로토타입 빈에서만 사용하는 것은 아니고 스프링 컨테이너에 대리적으로 조회하고 싶을 때 사용하는 기능이다.
         >
         >- 스프링이 제공하는 기능을 사용하지만 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기 쉬워진다.

      3. JSR-330 Provider

         

         ```java
         @Autowired
         private Provider<PrototypeBean> prototypeBeanProvider;
         
         public int logic() {
             PrototypeBean prototypeBean = prototypeBeanProvider.get();
             prototypeBean.addCount();
             int count = prototypeBean.getCount();
             return count;
         }
         ```

         > - 장점 : `javax.inject.Provider`라는 JSR-330 자바 표준을 사용하는 방법으로 스프링에 의존적이지 않다.
         > - 단점 : `javax.inject:javax.inject:1` 라이브러리를 gradle에 추가해야한다.
         > - `provider.get()`을 통해 항상 새로운 프로토타입 빈을 생성
         > - 기능이 단순하므로 단위테스트를 만들거나 mock 코드를 만들기 쉬워진다.
         > - DL 기능을 제공하지만 별도의 라이브러리가 필요하다.

    - 실무에서 위의 방법을 사용하는 일은 매우 드물지만 외부 프레임워크나 라이브러리를 분석할 때 개념이 필요함

    

- 웹 스코프

  - 웹 스코프는 웹 환경에서만 동작한다.

  - 웹 스코프는 스프링이 해당 스코프의 종료시점까지 관리하므로 종료 메서드가 호출된다.

  - 이런 특별한 스코프들은 꼭 필요한 곳에 최소화해서 사용 해야하며 무분별하게 사용시 유지보수가 어려워진다. 

  - 종류
  
    - request : HTTP 요청 하나가 들어오고 나갈 때까지 유지되는 스코프, 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고 관리된다.
    - session : HTTP Session과 동일한 생명주기를 가지는 스코프
    - application : 서블릿 컨텍스트(`ServeletContext`)와 동일한 생명주기를 가지는 스코프
    - websocket : 웹 소켓과 동일한 생명주기를 가지는 스코프

    

  - 웹환경 추가

    - build.gradle의 dependencies에 `implementation 'org.springframework.boot:spring-boot-starter-web'` 추가

    - 스프링 부트는 웹 라이브러리가 없으면 `AnnotationConfigApplicationContext`를 기반으로 애플리케이션을 구동하지만 웹 라이브러리가 추가되면 웹과 관련된 추가 설정과 환경들이 필요하므로 `AnnotationConfigServletWebServerApplicationContext`를 기반으로 애플리케이션을 구동한다.

    - 포트 변경하기

      - 기본포트 8080 포트를 다른 곳에서 사용중이라면 `main/resources/application.properties`에서 `server.port=9090`을 추가하여 9090 포트로 변경할 수 있다.

      

  - request 스코프 개발
  
    - 기대하는 공통 포맷 : `[UUID][requestURL] {message}`
      - UUID : HTTP 요청을 구분
      - requestURL : 어떤 URL을 요청해서 남은 로그인지 확인
    - `@Scope(value = "request")`를 달아준다.

  

  - 스코프와 Provider
  
    - Provider를 사용하지 않으면 스프링 컨테이너가 뜨는 시점에 빈의 의존관계 주입을 요구하는데 이 때는 HTTP요청이 없으므로 request 스코프가 아니며 따라서 생성자의 파라미터로 필요한 requestURL도 없다. 그러므로 의존관계 주입에 실패하여 에러가 반환된다.
    - Provider를 사용하면 `ObjectProvider.getObject()`를 호출하는 시점까지 request scope 빈의 생성을 지연할 수 있다.
    - `ObjectProvider.getObject()`를 호출하는 시점에는 HTTP 요청이 진행 중이므로 request scope 빈의 생성이 정상 처리된다.
    - 같은 HTTP요청끼리는 request scope를 공유하므로 Controller와 Service에서 따로 호출해도 같은 스프링 빈이 반환된다. UUID를 통해 로그를 찍어내면 같은 요청을 쉽게 구분 가능하다.

    

  - 스코프와 Proxy
  
    - 프록시 방식을 이용하면 Provider를 사용하지 않아도 진짜 객체 조회를 필요한 시점까지 지연처리 할 수 있으며 훨씬 편리하다.
    
    - `@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)`를 달아준다.
    
      - 적용 대상이 클래스이면 위와 같이 `TARGET_CLASS`를 붙여주고 인터페이스면 `INTERFACES`를 붙인다.
    
    - CGLIB라는 바이트코드를 조작하는 라이브러리로 내 클래스를 상속 받은 가짜 프록시 객체를 만들어서 주입한다.
    
    - 클래스를 조회해보면 순수 클래스가 아니라 `$$EnhancerBySpringCGLIB`이 붙은 클래스로 만들어진 객체가 대신 등록된 것을 확인할 수 있다.
    
    - `ac.getBean("myLogger", MyLogger.class)`로 조회해도 프록시 객체가 조회되는 것을 확인할 수 있다.
    
    - 가짜 프록시 객체는 요청이 오면 그때 내부에서 진짜 빈을 요청하는 위임 로직이 들어있고 원본 클래스를 상속 받아서 만들어지기때문에 클라이언트 입장에서는 구분이 불가능하다.(다형성)
    
    - 가짜 프록시 객체는 request scope와는 관계가 없다.
    
    - 웹 스코프가 아니어도 프록시는 사용할 수 있다.
    
    - 싱글톤을 사용하는 것 같지만 다르게 동작하므로 주의해서 사용해야한다.
    
      

## CS

- 데이터 커넥션 풀

- 바이트코드

  

## References

- [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/스프링-핵심-원리-기본편/dashboard)
- 이것이 자바다 : 신용권의 Java 프로그래밍 정복
