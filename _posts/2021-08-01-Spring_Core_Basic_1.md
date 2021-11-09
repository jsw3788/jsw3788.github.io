## Spring이란?

- [기술](spring.io/projects)

  - 필수 : Spring Boot, Spring Framework
  - 선택 : Spring Data, Spring Cloud, Spring Security, Spring Session 등등

  

- 스프링 프레임워크

  - 핵심 기술 : 스프링 DI 컨테이너, AOP, 이벤트, 기타
  - 웹 기술 : 스프링 MVC, 스프링 WebFlux
  - 데이터 접근 기술 : 트랜잭션, JDBC, ORM 지원, XML 지원
  - 기술 통합 : 캐시, 이메일, 원격접근, 스케쥴링
  - 테스트 : 스프링 기반 테스트 지원
  - 언어 : 코틀린, 그루비
  - 최근에는 스프링 부트를 통해서 스프링 프레임워크의 기술들을 편리하게 사용

  

- 스프링 부트

  - **스프링을 편리하게 사용할 수 있도록 지원, 최근에는 기본으로 사용**
  - 단독으로 실행할 수 있는 스프링 애플리케이션을 쉽게 생성
  - Tomcat 같은 웹 서버를 내장해서 별도의 웹 서버를 설치하지 않아도 됨
  - 손쉬운 빌드 구성을 위한 starter 종속성 제공
  - 스프링과 3rd parth(외부) 라이브러리 자동 구성
  - 메트릭, 상태 확인, 외부 구성 같은 프로덕션 준비 기능 제공
  - 관례에 의한 간결한 설정

  

- 스프링의 핵심

  - 스프링은 자바(객체지향언어) 기반의 프레임워크
  - 스프링은 **좋은 객체 지향**  애플리케이션을 개발할 수 있게 도와줌

  

- 스프링과 객체 지향

  - 다형성이 가장 중요함
  - 스프링은 다형성을 극대화해서 이용할 수 있게 도와줌
  - 제어의 역전(IoC), 의존관계 주입(DI)은 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원함

  

## SOLID

- ***SRP 단일 책임 원칙***	(Single responsibility principle)

  - 한 클래스는 하나의 책임만 가져야 한다
  - 하나의 책임이라는 것은 모호하다
    - 클 수 있고, 작을 수 있다
    - 문맥과 상황에 따라 다르다
  - **중요한 기준은 변경** 이다. 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것 
    - 예) UI 변경, 객체의 생성과 사용을 분리

  

- ***OCP 개방-폐쇄 원칙***	(Open/closed principle)

  - 소프트웨어 요소는 **확장에는 열려** 있으나 **변경에는 닫혀** 있어야 한다

  

- ***LSP 리스코프 치환 원칙***	(Liskov substitution principle)

  - 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다

  - 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다는 것

  - 인터페이스를 구현한 구현체를 믿고 사용하려면 이 원칙이 필요하다

    - 예) 자동차 인터페이스의 엑셀은 앞으로 가라는 기능, 뒤로 가게 구현하면 LSP 위반, 느리더라도 앞으로 가야함

    

- ***ISP 인터페이스 분리 원칙***	(Single responsibility principle)

  - 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다
  - 인터페이스가 명확해지고 대체 가능성이 높아진다

  

- ***DIP 의존관계 역전 원칙***	(Single responsibility principle)

  - 구현 클래스에 의존하지 말고, 인터페이스에 의존하라
  - 역할(Role)에 의존하게 해야 한다는 것
  - 클라이언트가 인터페이스에 의존해야 유연하게 구현체를 변경가능함
  - 클라이언트가 구현체에 의존하게 되면 변경이 매우 어려워짐
    - 예) 운전자는 자동차 역할(자동차를 어떻게 운전하는지)에 대해 알아야하고 아반떼, 테슬라에 대해서는 몰라도 됨



## 객체 지향 설계와 스프링

- 모든 설계에 **역할** 과 **구현** 을 분리하자
- 인터페이스를 도입하면 추상화라는 비용이 발생한다. 인터페이스 코드를 보고 구현체 코드도 봐야한다는 의미
- 기능을 확장할 가능성이 없다면, 구체 클래스를 직접 사용하고 향후 필요할 때 리팩터링해서 인터페이스를 도입하는 것도 방법이다.



## 자바 예제 : 스프링 핵심 원리 이해 1

- [객체 다이어그램 vs 클래스 다이어그램](https://booolean.tistory.com/586)

  - 객체 다이어그램

    - 실행 중 특정 시점에서 객체들이 동작하는 상황을 표현
    - ***클래스 다이어그램이 표현하는 정적인 구조가 아닌 동적인 상황을 표현***
    - 4+1뷰에서 논리적 뷰에 해당

  - 클래스 다이어그램

    - 클래스 또는 인터페이스의 명세로 클래스의 이름, 속성과 메소드를 정의
    - 하나의 클래스 외에 클래스 간의 관계를 표현해야 함
    
    

- Assertions는 junit이 아닌 assertj를 import 한다.

- enum 값을 비교할 때는 `==`를 쓴다.
- Interface의 구현체가 하나면 관례적으로 인터페이스 이름 뒤에 Impl을 붙인다.



## 스프링 예제 : 스프링 핵심 원리 이해 2

- 생성자 주입
  - 생성자를 통해서 객체가 인스턴스에 들어감
  - 생성자 주입을 통해 객체의 책임과 역할을 분리할 수 있음
  - 주입만 관리하는 파일(AppConfig)을 만들어서 클라이언트가 인터페이스에만 의존할 수 있도록 함
  - AppConfig는 애플리케이션의 실제 동작에 필요한 **구현객체를 생성**함
  - final이 되어있는 변수는 생성자 할당이나 필드 할당이 되어있어야 함



- 의존관계 주입

  - DI(Dependency Injection), 의존성 주입이라고도 함

  - 클라이언트 입장에서는 의존관계를 외부에서 주입해주는 것처럼 보임

    ~~~java
    public class AppConfig {
    
        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }
        
    	// 나중에 DBMemberRepository로 바꿀 때 이 부분만 수정
        private MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
    
        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }
        
        // 나중에 RateDiscountPolicy로 바꿀 때 이 부분만 수정
        public DiscountPolicy discountPolicy(){
            return new FixDiscountPolicy();
        }
    }
    ~~~

  - 위의 코드처럼 짜게 되었을 때의 이점

    - 중복이 제거됨 -> 이후 수정이 필요할 때 한 부분만 수정하면 됨
    - 역할과 구현 클래스가 한눈에 들어옴 -> 애플리케이션 전체 구성을 쉽게 파악 가능
    - DIP, OCP, SRP를 지키게 됨

    

  - 의존 관계는 **정적인 클래스 의존 관계**와 **실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계**로 분리해서 생각해야함

    - 정적인 클래스 의존 관계

      - import만 보고 의존관계를 파악 가능
      - 애플리케이션을 실행하지 않아도 분석 가능
      - 실제 어떤 객체가 주입될지 알 수 없음
      - 클래스 다이어그램으로 나타남

      

    - 동적인 객체 인스턴스 의존 관계

      - 애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계
      - 객체 다이어그램으로 나타남

      

    - 애플리케이션 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 의존관계 주입이라고 한다.

    

    - 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.

  

- 제어의 역전 IoC(Inversion of Control)
  - AppConfig를 쓰기 전에는 클라이언트 구현 객체가 필요한 서버 구현 객체를 생성, 연결, 실행 했다. 즉, 클라이언트가 서버의 코드를 포함하고 있으므로 어떤 구현체가 실행될지 알 수 있었다. 이 때, 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다.
  - AppConfig가 등장한 이후, 클라이언트는 여전히 필요한 인터페이스를 호출하지만 어떤 구현 객체들이 실행되는지 모른다. 이제 AppConfig가 프로그램의 제어 흐름을 대한 권한을 가지게 된다.
  - 이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전이라 한다.
  - ***프레임워크 vs 라이브러리***
    - 프레임워크는 내가 작성한 코드를 제어하고, 대신 실행한다.(JUnit)
    - 내가 작성한 코드가 직접 제어의 흐름을 담당하면 그것은 라이브러리다.



- AppConfig처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 **DI 컨테이너**(IoC 컨테이너, 어셈블러, 오브젝트 팩토리라고도 함)라고 한다.



- **스프링 컨테이너**

  - `ApplicationContext`를 스프링 컨테이너라 한다.
  - 기존에는 개발자가 AppConfig를 사용해서 직접 객체를 생성하고 DI를 했지만  이제는 스프링 컨테이너를 사용한다.
  - 스프링 컨테이너는 `@Configuration`이 붙은 AppConfig를 설정(구성) 정보로 사용한다. 여기서 `@Bean`이 붙은 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록하는데 이를 스프링 빈이라고 한다.
  - 스프링 빈은 `applicationContext.getBean("메서드명", 인터페이스명.class)`를 사용해서 찾을 수 있다. ( `@Bean(name="호출시 원하는 이름")`을 사용하면 호출명을 바꿀 수 있으나 관례상 안씀)

  

## Java

- enum(열거 타입)
  - 한정된 값만을 갖는 데이터 타입
  - 몇 개의 열거 상수(열거 타입의 값) 중에서 하나의 상수를 저장하는 데이터 타입
    - 예) 요일(월/화/수/목/금/토), 계절(봄/여름/가을/겨울)
  - enum class의 인스턴스 메소드를 사용할 수 있다는 이점이 있다.



## References

- [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/스프링-핵심-원리-기본편/dashboard)
- 이것이 자바다 : 신용권의 Java 프로그래밍 정복