# JVM의 구조와 Java의 실행 방식에 대해 설명해주세요.

## 면접용 답변  
Java는 플랫폼 독립적인 언어로, 작성된 `.java` 파일은 `javac` 컴파일러를 통해 바이트코드(`.class`)로 변환되고, 이 바이트코드는 JVM에 의해 실행됩니다.  

JVM은 Class Loader, Runtime Data Area, Execution Engine, Garbage Collector 등의 구성 요소로 이루어져 있습니다.
Class Loader는 바이트코드를 로드하며, Runtime Data Area는 실행에 필요한 메모리 공간을 제공합니다.  
Execution Engine은 바이트코드를 인터프리터 또는 컴파일러를 통해 실행하고, Garbage Collector는 불필요한 객체를 자동으로 수거합니다.  
이 과정을 통해 Java 프로그램이 실행됩니다.

<br/>

## 개념 설명

### Java의 구성 요소: JDK, JRE, JVM
![image](https://velog.velcdn.com/images/sgwon1996/post/f133fb06-0be5-4d28-96b1-ee28bba9604e/image.png)

#### JDK (Java Development Kit)
- 자바 개발 도구 세트 (javac, java, javadoc 등)
- JRE 포함

#### JRE (Java Runtime Environment)
- 자바 실행 환경
- JVM + 실행에 필요한 클래스 라이브러리

#### JVM (Java Virtual Machine)
- 자바 바이트코드(.class 파일) 실행하는 가상머신
  - Runtime Data Area
  - Interpreter
  - JIT Compiler
  - Garbage Collector

---

### JVM의 구성 요소

| 구성 요소               | 설명                                                                                  |
|----------------------|-------------------------------------------------------------------------------------|
| **Class Loader**       | 바이트코드(.class)를 메모리에 로딩함<br>로딩 → 링크 → 초기화 과정 수행                             |
| **Runtime Data Area**  | JVM 실행 시 사용되는 메모리 공간<br>(Method Area, Heap, Stack, PC Register, Native Method Stack 등) |
| **Execution Engine**   | 바이트코드를 실제로 실행<br>(인터프리터, JIT 컴파일러 포함)                                           |
| **Garbage Collector**  | Heap 메모리에서 사용되지 않는 객체를 탐지해 자동으로 수거                                           |
| **Native Interface**   | 네이티브 코드(C/C++ 등 다른 언어)와 상호작용할 수 있도록 지원                                       |

---

### Runtime Data Area 구조

| 메모리 영역             | 생성되는 시점               | 저장되는 것                                  |
|------------------------|-----------------------|-----------------------------------------|
| **Method Area**         | 클래스가 로딩될 때            | 클래스 정보, static 변수, 메서드 코드 등 (모든 스레드 공유) |
| **Stack**               | 메서드가 호출될 때 (스레드마다 생성) | 지역 변수, 매개변수, 연산 중간값 등 (메서드 단위로 관리)      |
| **Heap**                | 런타임 중 `new` 키워드 사용 시  | 인스턴스(객체) 데이터 (모든 스레드 공유)                |
| **PC Register**         | JVM 명령 실행 시           | 현재 수행 중인 바이트코드 명령의 주소                   |
| **Native Method Stack** | 네이티브 메서드 호출 시         | C/C++ 등의 네이티브 코드 실행 정보                  |


### Execution Engine 구성 요소

| 구성 요소              | 설명                                                                                      |
|----------------------|-----------------------------------------------------------------------------------------|
| **Interpreter**        | 바이트코드를 한 줄씩 해석하며 실행. 빠르게 시작 가능하나 느림                                                     |
| **JIT Compiler**       | 자주 실행되는 코드(핫스팟)를 기계어로 변환해 캐싱.<br>처음엔 느릴 수 있으나, 한 번 컴파일된 코드는 기계어로 캐시되기 때문에 실행 속도가 매우 빠름. |

---

### Garbage Collector

| 특징 | 설명 |
|------|------|
| **자동 메모리 관리** | Heap 영역의 객체 중 더 이상 사용되지 않는 객체를 탐지해 제거 |
| **동작 시점** | JVM이 필요하다고 판단할 때 또는 메모리가 일정 수준 이상 찼을 때 동작 |
| **장점** | 개발자가 직접 메모리 해제하지 않아도 되므로 안정성과 생산성이 향상됨 |

---

### Java 실행 흐름
![image](https://velog.velcdn.com/images/sgwon1996/post/8f1cd49e-a059-4ac1-af27-ead1470caaa9/image.png)

1. `.java` 파일 작성  
2. `javac` 컴파일러로 `.class` 바이트코드 생성  
3. JVM의 Class Loader가 바이트코드를 메모리에 로딩  
4. Execution Engine이 바이트코드를 실행 (인터프리터 또는 JIT)  
5. Runtime Data Area에서 메모리 관리 (Heap, Stack 등)  
6. Garbage Collector가 사용되지 않는 객체를 수거  
7. 프로그램 종료  

---

## 추가 질문

### Stack과 Heap의 차이는 무엇인가요?  
Stack은 메서드 호출 시 생성되는 일시적인 메모리 공간으로, 지역 변수, 매개변수, 연산 정보 등이 저장됩니다.  
스레드마다 독립적으로 존재하며, 메서드 실행이 끝나면 자동으로 메모리가 회수됩니다.  
반면, Heap은 new 키워드로 생성된 객체가 저장되는 공간으로, 모든 스레드가 공유하며,  
사용이 끝난 객체는 JVM의 Garbage Collector가 자동으로 회수합니다.

### GC는 언제 동작하나요?
GC는 Heap 영역에서 더 이상 참조되지 않는 객체를 탐지해 자동으로 메모리를 회수합니다.  
주로 메모리가 부족할 때나 객체가 참조되지 않게 되었을 때 동작합니다.  
이처럼, Java에서는 명시적으로 메모리를 해제하지 않고 GC가 자동으로 처리합니다.

### JVM은 언제 Interpreter를 쓰고 언제 JIT Compiler를 쓰나요?

JVM은 처음에는 **Interpreter**를 사용해 바이트코드를 한 줄씩 해석하며 실행합니다.  
그러다 특정 메서드나 루프처럼 **자주 실행되는 코드(Hotspot)** 가 감지되면, **JIT(Just-In-Time) Compiler**가 해당 코드를 기계어로 컴파일해 캐싱합니다.  
이를 통해 처음엔 빠르게 실행을 시작하고, 반복되는 부분은 최적화된 기계어로 더 빠르게 실행할 수 있습니다.

### Java는 왜 플랫폼 독립적인 언어라고 하나요?
Java는 바로 기계어로 컴파일되지 않고, 중간 형태인 바이트코드(.class)로 컴파일됩니다.  
이 바이트코드는 OS마다 다른 실행 환경 대신, 운영체제 위에서 동작하는 JVM(Java Virtual Machine)에서 실행되며,
운영체제마다 다른 JVM이 같은 바이트코드를 실행할 수 있도록 해줍니다.  
즉, 개발자는 코드를 한 번만 작성해도, 다양한 플랫폼에서 실행할 수 있으므로
Java는 '플랫폼 독립적인 언어'라고 불립니다.

#Write Once, Run Anywhere

