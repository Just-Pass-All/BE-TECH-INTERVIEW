# Optional 클래스는 왜 생겼고 어떻게 쓰이나요?

## 면접용 답변
Optional 클래스는 NullPointerException을 방지하고 코드의 명확성을 높이기 위해 만들어졌습니다. <br>
값이 존재할 수도 있고 없을 수도 있는 상황에서 null 대신 Optional을 반환함으로써, 호출자가 값의 존재 여부를 명시적으로 처리하도록 유도합니다.
<br>

## 개념 설명

### NullPointerException
- null을 가지고 있는 객체/변수를 호출할 때 발생하는 예외
- NPE를 피하려면 null 여부를 검사해야 하는데, null 검사를 해야하는 변수가 많은 경우 코드가 복잡해지고 번거로워짐

→ 명시적으로 "값이 있을 수도, 없을 수도 있다"는 것을 표현하고, 안전하게 처리하도록 하기 위해 `Optional<T>` 등장 
  
<br>

### Optional
- null이 올 수 있는 값을 감싸는 Wrapper 클래스
- "이 값이 없을 수도 있다"를 타입 자체로 표현

```java
public final class Optional<T> {

  // If non-null, the value; if null, indicates no value is present
  private final T value;
   
  ...
}
```
<br>


#### 주요 메서드
| 메서드                   | 설명                                |
| --------------------- | --------------------------------- |
| `of(value)`           | 절대 null이 아닌 값을 감쌈 (null이면 NPE 발생) |
| `ofNullable(value)`   | null일 수도 있는 값을 감쌈                 |
| `isPresent()`         | 값이 존재하는지 확인 (null 아닌지)            |
| `ifPresent(Consumer)` | 값이 있을 때만 실행                       |
| `orElse(value)`       | 값이 없으면 기본값 반환                     |
| `orElseGet(Supplier)` | 값이 없으면 람다로 기본값 생성                 |
| `orElseThrow()`       | 값이 없으면 예외 발생                      |

ex) Optinal 생성
```java
Optional<String> name = Optional.of("DuDu");           // of() : null 불가
Optional<String> maybeName = Optional.ofNullable(null); // ofNullable() : null 가능
Optional<String> empty = Optional.empty();              // empty(): 비어 있음
```
ex) 값 접근
```java
Optional<String> name = Optional.of("DuDu");

name.get();                // "Alice" 반환 (값이 없으면 예외)
name.isPresent();          // true
```
ex) 기본값 처리
```java
Optional<String> empty = Optional.empty();

String result1 = empty.orElse("Default");               // 기본 값 "Default"
String result2 = empty.orElseGet(() -> "Generated");    // 람다로 생성
String result3 = empty.orElseThrow(() -> new IllegalArgumentException("No value!")); // 예외 발생
```
<br>

#### 주의사항
`리턴 전용`으로 쓰는 것이 가장 일반적이고 안전함
- 메서드 인자로 쓰는 경우, 불필요한 오버헤드가 발생하고 API 가독성이 저하됨
- 엔티티/DTO 등 필드로 쓰는 경우, 직렬화 문제, JPA 등과 호환성 문제가 발생함

<br>

#### 올바른 사용법

- Optional 변수에 Null을 할당하지 말아라 
- 값이 없을 때 Optional.orElseX()로 기본 값을 반환하라 
- 단순히 값을 얻으려는 목적으로만 Optional을 사용하지 마라 
- 생성자, 수정자, 메소드 파라미터 등으로 Optional을 넘기지 마라 
- Collection의 경우 Optional이 아닌 빈 Collection을 사용하라 
- 반환 타입으로만 사용하라

<br>
<br>

## 새끼 질문
-  Optional을 메서드 매개변수로 사용하는 것에 대해 어떻게 생각하나요?
- Optional을 사용할 때 get()을 써도 괜찮은가요?
