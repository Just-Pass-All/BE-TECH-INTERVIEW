# 의존성 주입(DI)과 IoC 컨테이너의 동작 방식에 대해 설명해주세요.
## 면접 답변
의존성 주입은 객체 간의 의존 관계를 개발자가 직접 생성하지 않고, 외부에서 주입받는 설계 방식입니다.
스프링에서는 IoC 컨테이너가 이 역할을 수행하며, 애플리케이션 실행 시 필요한 객체를 생성하고 의존 관계를 자동으로 연결해줍니다.
이를 통해 결합도를 낮추고, 테스트와 유지보수가 용이한 구조를 만들 수 있습니다.

<br>

## 개념 정리
### IoC (Inversion of Control)
- 객체 생성 및 의존성 관리를 개발자가 아닌 프레임워크가 담당하는 것을 말한다.
- 스프링에서는 IoC를 구현하기 위해 **IoC 컨테이너**를 제공한다.

<br>

### IoC 컨테이너란?
- 애플리케이션 실행 시 객체(Bean)를 생성 및 관리하고, 의존성을 주입하는 역할을 하는 컴포넌트
- 스프링의 주요 IoC 컨테이너
  - ApplicationContext (가장 일반적인 컨테이너)
  - BeanFactory (상위 개념, 지연 초기화 등 제한된 기능만 제공하는 컨테이너)

<br>

### DI (Dependency Injection)
- IoC의 구현 방식 중 하나라고 할 수 있다.
- 객체가 필요한 의존 객체를 직접 생성하는 것이 아니라 외부로부터 주입받는 방식을 말한다.
- 스프링에서는 IoC 컨테이너(ApplicationContext)가 객체 생성과 DI를 함께 담당한다.

#### DI 방식의 종류
| 방식             | 설명                                                            |
|------------------|-----------------------------------------------------------------|
| 생성자 주입      | 생성자를 통해 의존 객체를 주입받음. 불변성과 테스트 용이성이 높음 |
| 필드 주입        | 필드에 직접 주입. 편리하지만 테스트 및 DI 명시성이 떨어짐       |
| 세터 주입        | Setter 메서드를 통해 주입. 선택적 의존성에 적합                   |

<br>

### 예시 코드
#### 1. 생성자 주입 방식
> 가장 권장되는 방식
```java
@Component
public class OrderService {

    private final PaymentService paymentService;

    @Autowired
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```
- OrderService가 PaymentService에 의존하고 있으므로 생성자를 통해 의존성을 주입받는다.
- `@Autowired`를 생략해도 스프링이 자동으로 인식한다. (생성자가 하나일 경우)
- 불변 객체로 만들 수 있어 테스트 용이성과 안정성이 뛰어나다.
- 필수 의존성 주입에 적합하며, 스프링 공식 문서에서도 권장한다.

<br>

#### 2. 필드 주입 방식
```java
@Component
public class OrderService {

    @Autowired
    private PaymentService paymentService;
}
```
- `@Autowired`를 필드에 직접 붙여 의존성을 주입한다.
- 코드가 간결하다는 장점이 있지만, DI가 프레임워크에 완전히 의존하게 되어 테스트가 어렵고 유지보수하기 불편하다.
- 필드가 final일 수 없으므로 객체 불변성을 해친다.
- 테스트 코드에서 mock 객체를 주입하거나 생성자 호출이 불가능하므로 단위 테스트 시 좋지 않다.

<br>

#### 3. setter 주입 방식
```java
@Component
public class OrderService {

    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```
- `@Autowired`가 붙은 Setter 메서드를 통해 의존성을 주입한다.
- 필수 의존성이라면 생성자 주입 방식이 더 적합하지만, 선택적으로 필요한 의존성일 경우 세터 주입을 활용할 수 있다.
- 단점은 외부에서 Setter를 호출할 수 있게 열어두는 구조가 되므로 캡슐화가 약해질 수 있다.

<br>

### IoC 컨테이너의 동작 흐름
1. 설정 클래스 혹은 컴포넌트 스캔을 통해 Bean 정의 확인
2. Bean 생성 및 초기화
3. 의존성 주입 수행
4. ApplicationContext에 Bean 등록
5. 필요 시 주입된 Bean 사용

<br>

## 꼬리 질문
1. 결합도를 낮춘다는 것이 어떤 것인가요?  
   결합도를 낮춘다는 것은 하나의 클래스가 다른 클래스에 의존하는 정도를 최소화하는 것을 말합니다.
   결합도를 낮추면 코드 변경 시 영향을 받는 범위가 줄고, 유지보수와 테스트가 쉬워집니다.   
2. 의존성 주입 없이 직접 객체를 생성하면 어떤 문제가 발생할 수 있나요?  
   직접 객체를 생성하면 코드가 특정 구현체에 강하게 결합되고, 테스트할 때 Mock 객체를 주입하기 어려우며, 객체 생성 방식 변경 시 코드 수정 범위가 넓어지는 문제가 생깁니다.   
3. @Component와 @Bean의 차이점은 무엇인가요?  
   @Component는 클래스에 직접 붙여 자동 감지되는 방식이고, @Bean은 @Configuration 클래스 안에서 메서드에 붙여 직접 등록하는 방식입니다.
   둘 다 스프링 빈을 등록하지만, @Component는 컴포넌트 스캔에 의존하고 @Bean은 수동으로 제어가 가능하다는 차이가 있습니다.

