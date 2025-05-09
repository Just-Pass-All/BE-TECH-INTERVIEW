# JAVA의 프록시 객체는 어떻게 동작하나요?

## 면접용 답변
Java에서 프록시 객체는 대상 객체의 메서드를 호출하기 전후로 추가 동작을 삽입하기 위해 사용됩니다. <br>
대표적으로 JDK 동적 프록시와 CGLIB 프록시 두 가지 방식이 있습니다. <br>
JDK 동적 프록시는 인터페이스를 구현한 객체에 대해 프록시를 생성하고, CGLIB 프록시는 클래스 자체를 상속하여 프록시를 만듭니다.<br>
프록시 객체는 실제 객체 대신 호출을 받아, 로깅, 트랜잭션 처리, 보안 검사 등 공통 기능을 부가적으로 실행합니다.

## 개념 설명

### 프록시(Proxy)
- Proxy == 대리인
- 다른 객체를 대신하여 그 객체에 대한 접근을 제어하거나 기능을 확장하는 객체
- 프록시는 실제 객체를 대신하여 그 객체의 메서드 호출을 가로채고, 필요한 경우 실제 객체에 대한 작업을 수행
- 프록시는 일반적으로 원래 객체와 같은 인터페이스를 구현하여, 클라이언트가 원래 객체를 사용하는 것처럼 보이게 한다!

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbA9teJ%2FbtsJD7UZLl5%2FTQnYkP29FBCXCxhIn8BeSK%2Fimg.png">

→  프록시 객체는 실제 Entity를 상속 받아서 만들어지기 때문에 실제 클래스와 겉 모양이 같음

→  프록시 객체는 실제 엔티티에 대한 접근을 지연시킴 & 실제 엔티티가 필요할 때까지 데이터베이스 쿼리를 실행하지 않음

→  프록시 객체는 실제 엔티티의 메서드를 호출할 때, 이 호출을 실제 엔티티 객체로 위임(delegate)함

<br>

### 프록시의 종류와 동작 방식

####  1️⃣ JDK 동적 프록시
- `java.lang.reflect.Proxy`
- 조건: 대상 객체가 인터페이스를 구현하고 있음 
- 원리: InvocationHandler를 구현해 메서드 호출 전/후 동작 정의
```java
public interface HelloService {
    void sayHello();
}

public class HelloServiceImpl implements HelloService { // 대상 객체
    public void sayHello() {
        System.out.println("Hello!");
    }
}
```

```java
HelloService target = new HelloServiceImpl();

HelloService proxy = (HelloService) Proxy.newProxyInstance(
    HelloService.class.getClassLoader(),
    new Class[]{HelloService.class},
    (proxyObj, method, args) -> {
        System.out.println("Before");
        Object result = method.invoke(target, args);
        System.out.println("After");
        return result;
    }
);

```

출력 결과 
```
Before
Hello!
After
```

<br>

#### 2️⃣ CGLIB 프록시
- Code Generation Library)
- 조건: 클래스를 상속할 수 있어야 하며, final 클래스/메서드는 안 됨
- 원리: 바이트코드 조작을 통해 클래스의 서브클래스를 동적으로 생성
```java
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(HelloServiceImpl.class);
enhancer.setCallback(new MethodInterceptor() {
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        System.out.println("Before");
        Object result = proxy.invokeSuper(obj, args);
        System.out.println("After");
        return result;
    }
});
HelloService proxy = (HelloService) enhancer.create();
```
 <br>

### 주요 용도
| 용도                                | 예시                |
| --------------------------------- | ----------------- |
| AOP (Aspect-Oriented Programming) | 로깅, 트랜잭션, 보안 처리 등 |
| DI 컨테이너에서 Lazy 로딩                 | Spring의 `@Lazy`   |
| 원격 호출 (RMI 등)                     | 네트워크 프록시          |
| Mock 객체 생성                        | 테스트 시 사용          |

<br>

### 주의 사항
- this 호출은 프록시를 우회하므로 AOP 적용 안됨
- 프록시 내부에서 내부 메서드를 직접 호출하면 AOP 적용 안됨

 <br>

### 프록시 객체와 일반 객체의 주요 차이점

- 데이터 로딩 방식: 프록시 객체는 데이터를 지연 로딩/ 일반 객체는 데이터를 즉시 로딩
- 메소드 호출 제어: 프록시 객체는 메소드 호출을 가로채서 부가적인 동작을 수행할 수 있음(AOP, 트랜잭션 관리 등).
- 객체 생성 방식: 일반 객체는 개발자가 직접 생성/ 프록시 객체는 프레임워크(Spring, Hibernate 등)에 의해 자동 생성
- 성능 최적화: 프록시 객체는 지연 로딩을 통해 성능을 최적화, 필요할 때만 데이터를 로딩
- 객체 동작: 프록시 객체는 유연한 기능 추가 가능/ 일반 객체는 고정된 동작 수행

<br>

## 새끼 질문
- JDK 동적 프록시와 CGLIB 프록시의 차이는 무엇인가요?
- Spring에서 @Transactional이 적용되지 않는 상황은 언제인가요?

---
## Reference

https://pixx.tistory.com/397

https://yujin-17.tistory.com/entry/Spring-프록시Proxy란-AOP와-프록시-기반-동작-방식