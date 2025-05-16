# POJO란 무엇인가요? Spring Framework에서 POJO는 무엇이 될 수 있을까요?

## 면접용 답변
POJO는 "Plain Old Java Object"의 약자로, 특정 프레임워크나 기술에 종속되지 않은 순수한 자바 객체를 의미합니다.
특정 인터페이스를 구현하거나 어노테이션을 붙일 필요 없이, 단순히 필드와 메서드만으로 구성됩니다.

Spring Framework에서는 이러한 POJO 객체들을 IoC 컨테이너에서 관리하면서,
필요한 기능(DI, AOP 등)을 주입해줍니다.
예를 들어, @Service, @Repository 등이 붙은 클래스들도 내부적으로는 POJO입니다.

결국 Spring에서는 비즈니스 로직, DAO, 도메인 객체, 컨트롤러 클래스 등
대부분의 컴포넌트가 POJO로 작성될 수 있으며, 이로 인해 테스트 용이성과 재사용성이 높아집니다.

## 개념 설명

### POJO란?  

POJO는 "Plain Old Java Object"의 줄임말로,
EJB(Java EE의 무거운 컴포넌트 모델)에 대한 대안으로 등장한 가볍고 단순한 자바 객체를 말합니다.

- 특징:

  특정 라이브러리나 프레임워크에 의존하지 않음
  복잡한 상속, 어노테이션 없이 작성 가능
  일반적인 getter/setter, 생성자, toString 등으로 구성

    ```java
    public class Person {
        private String name;
        private int age;

        // 기본 생성자
        public Person() {}

        // getter/setter
        public String getName() { return name; }
        public void setName(String name) { this.name = name; }

        public int getAge() { return age; }
        public void setAge(int age) { this.age = age; }
    }
    ```

- POJO는 특정 기술 스택에 종속되지 않기 때문에,
  다양한 환경에서 재사용이 가능하고, 테스트가 용이합니다.
  또한, 코드의 가독성을 높이고 유지보수를 쉽게 만들어줍니다.

### Spring Framework에서 POJO

Spring Framework의 가장 큰 특징 중 하나는 POJO 기반의 컴포넌트 구성입니다.
즉, 비즈니스 로직을 담당하는 클래스가 Spring 전용 API에 전혀 의존하지 않아도,
Spring이 이 객체를 빈(Bean) 으로 등록하고 DI, AOP 같은 기능을 제공할 수 있다는 뜻입니다.

아래와 같은 클래스들은 모두 POJO로, 특별한 상속이나 인터페이스 구현 없이 일반 자바 클래스입니다.

| 역할      | 예시 어노테이션                         | 설명                |
| ------- | -------------------------------- | ----------------- |
| 일반 컴포넌트 | `@Component`                     | 범용 빈              |
| 서비스 계층  | `@Service`                       | 비즈니스 로직 담당        |
| DAO 계층  | `@Repository`                    | 데이터 접근 담당         |
| 컨트롤러 계층 | `@Controller`, `@RestController` | 웹 요청 처리           |
| 데이터 전송  | DTO / VO / Entity                | 상태 정보 전달 또는 DB 매핑 |


- 예시:

1. 순수 POJO 클래스

    ```java
    public class GreetingService {
        public String greet(String name) {
            return "Hello, " + name;
        }
    }

    ```

2. Spring에서 빈으로 등록하기 (@Component 사용)

    ```java
    import org.springframework.stereotype.Component;

    @Component
    public class GreetingService {
        public String greet(String name) {
            return "Hello, " + name;
        }
    }

    ```
- `@Component`를 붙이면 Spring이 이 클래스를 빈으로 등록함
- 내부 로직은 변하지 않음 → **POJO의 순수성 유지**

## 정리

| 구분       | 설명                                            |
| -------- | --------------------------------------------- |
| POJO     | 순수 자바 클래스, 프레임워크에 종속되지 않음                     |
| Spring 빈 | POJO에 `@Component`류 어노테이션을 붙이면 등록됨            |
| 비침투적 설계  | POJO 자체를 수정하지 않고도 DI, AOP, 생명주기 등을 Spring이 처리 |
| 장점       | 유지보수성, 테스트 용이성, 재사용성 우수                       |


## 꼬리 질문

- POJO를 도입하지 않았을 때 발생할 수 있는 코드의 문제점은 무엇인가요?

- Spring은 어떻게 POJO에 DI와 AOP 기능을 주입하나요? (프록시 기반 메커니즘 설명)