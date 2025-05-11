## 면접 답변

- static이란 고정된이라는 의미로 static 키워드를 통해 정적 변수와 정적 메소드를 만들 수 있습니다. 프로그램이 종료되기 전까지 사용되며, 정적변수와 정적 메소드는 가비지콜렉터가 회수하지 않습니다.(정적객체는 회수함)
    
    동적은 객체를 런타임 중 힙 영역에 할당하지만 정적은 프로그램 시작 지점에 클래스 로더가 클래스를 해석하여 메소드 영역 혹은 힙 영역에 클래스 메타 데이터 및 정적 변수를 적재합니다.

# 개념 설명

## Static 변수

- 클래스 변수
- 공통적인 값을 유지할 때 선언함
- 클래스가 메모리에 로딩될 때 생성되어 프로그램이 종료될 때까지 유지됨
    
    ```java
    public static final String EXAMPLE="예시";
    ```
    

## Static 메소드

- 클래스 메소드
- 인스턴스 변수를 사용할 수 없음
    
    ```java
    public class User {
        private String name;
        private int age;
        
        public User(String name, int age) {
            this.name = name;
            this.age = age;
        }
    
    	public static User fromBirthYear(String name, int birthYear) {
    	      int currentYear = 2025;
    	      int age = currentYear - birthYear;
    	      return new User(name, age);
    	}
    	
    }
    ```
    

## Static 객체(Object)

- 프로그램이 시작될 때 메모리에 한번만 올라가고, 프로그램 종료 때까지 살아있음
    
    ```java
    class Database {
        // 정적 객체
        public static final Database INSTANCE = new Database();
    
        private Database() {
        }
    }
    ```
    

## Static 저장 위치

- Java8이후
    - Permanent 영역이 사라지고 힙 외부 ‘메타스페이스’ 네이티브 메모리에서 Permanent가 저장하던 **클래스 메타 데이터**를 들고 있음
        - Permanent가 들고있던 정보를 모두 메타스페이스가 관리하는 것은 아님
    - interned String과 클래스 정적 변수는 힙 영역에 옮겨짐

→ static을 힙 영역에서 관리함

## Static 활용 방안

- 상수: 객체 내에서 정적 변수로 정의하면 메모리를 아낄 수 있음
- 유틸리티 클래스: 인스턴스 메소드와 인스턴스 변수를 제공하지 않고, 데이터 처리를 위한 정적 메소드만 존재하는 클래스
    - 예시: java Math 클래스
- 싱글턴 패턴: 객체를 하나만 생성해서 전역적으로 사용하는 패턴
    - 로그를 기록하거나 캐싱할 때 사용
- 정적 클래스: 정적 메소드만 가지는 클래스를 정적 클래스라고 부름
    - 프로그램내에서 전역적으로 사용되고, 인스턴스를 생성하지 않기 때문에 유일성이 보장됨
    - 싱글턴패턴과 차이: 인스턴스를 생성할 수 없기 때문에 클래스 메소드를 이용함

## Static을 지양해야하는 이유

- 메모리 문제: static이 프로그램 실행 시점에 메모리를 할당하고 프로그램 종료 시점까지 메모리에서 해제되지 않음
- 동시성 문제: 전역에서 접근할 수 있기 때문에 동시성 문제가 일어날 수 있는 가능성이 큼
- 런타임 다형성 불가: 객체를 메모리로 할당하여 사용하는 것이 아니라 객체, 메소드로 바로 접근해서 호출
- 객체 상태 이용 불가: 정적 메소드는 필요로 하는 인자를 모두 외부에서 주입해야함 → 정적 메소드 안에 클래스의 인스턴스 필드를 사용할 수 없기 때문
    - 정적 메소드 안에서는 정적 변수만 사용할 수 있음
    - 정적 메소드가 많아지면 외부 값에 의존하는 수동 객체가 됨
        - 수동 객체: 자기 스스로 행동하거나 상태를 관리하지 못하는 객체
    - 객체를 이용할 수 없다는 뜻은?
        
        ```java
        class User {
            private String name;
            
            public String getName() {
                return this.name;
            }
        }
        ```
        
        - 일반적으로 클래스는 자신의 인스턴스 변수를 관리하면서 동작함(this.name)
        - 정적 메소드는 this 키워드를 사용할 수 없음 → 객체가 없이 클래스 자체에 속해있기 때문
        - 정적 메소드 안에서는 정적 변수, 메소드의 파라미터로 받은 값만 사용가능
- 테스트 어려움
    - 테스트 대상 메소드가 static이면 그 안에서 static 호출한 것도 진짜 호출이 되어서 단위 테스트가 어려움
    - static 변수나 static 상태를 가지고 있으면 상태 공유때문에 위험할 수 있음 → 예시: 병렬로 테스트할 때 예측 불가능한 테스트 실패 발생 가능

- static 변수는 왜 객체 생성 없이 접근 가능한가요?
    
    static 변수는 클래스 변수이기 때문입니다. static 변수는 클래스를 로딩할 때 메서드 영역에 저장되고, 객체와 무관하게 클래스 자체에 속하게 됩니다. 그렇기 때문에 static 변수는 여러 객체가 공유할 수 있습니다.
    
- static을 사용하는 경우, 메모리 관리에 어떤 영향을 미치나요?
    
    정적 변수나 메서드는 클래스가 로드될 때 메모리에 올라가고, GC 대상이 아니기 때문에 메모리 누수 위험이 있습니다.
    
- static은 왜 상속과 다르게 동작하나요?
    
    ```java
    class Parent {
        static void staticMethod() {
            System.out.println("Parent static");
        }
    
        void instanceMethod() {
            System.out.println("Parent instance");
        }
    }
    
    class Child extends Parent {
        static void staticMethod() {
            System.out.println("Child static");
        }
    
        @Override
        void instanceMethod() {
            System.out.println("Child instance");
        }
    }
    
    Parent obj = new Child();
    obj.staticMethod(); //Parent static
    obj.instanceMethod(); //Child instance
    ```
    static은 클래스 레벨에서 동작하고 상속은 인스턴스(객체) 레벨에서 동작합니다. static에는 객체 다형성이 적용되지 않고, 숨겨진 형태로 동작합니다. 반면 인스턴스는 다형성이 적용됩니다.
    
- static 변수 초기화 시 주의사항은 무엇인가요?
    정적 변수는 최초 로딩될 때 한 번만 초기화되며, 정적 변수로 선언되는 경우, 값을 초기화해주지 않으면 쓰레기값이 아니라 int의 경우에는 0 string은 null로 자동 초기화됩니다.

## 참고 자료
https://steady-coding.tistory.com/603