## 면접용 답변

Call by Value와 Call by reference는 메서드를 호출할 때 파라미터를 전달하는 방법입니다. Call by Value는 메서드를 호출할 때 값을 넘겨줍니다. 메서드를 호출하는 호출자의 변수와 호출 당하는 수신자의 파라미터는 복사된 서로 다른 변수입니다. 따라서 수신자의 파라미터를 수정해도 호출자의 변수에는 아무 영향이 없습니다.

반면 Call by Reference의 경우, 참조 주소를 직접 전달하는 방식을 씁니다. 참조를 직접 넘기기 때문에 호출자의 변수와 수신자의 파라미터가 동일한 변수입니다.

# 개념 설명

## Java와 C에서 파라미터 전달 방법

- **오직 Call by Value로 동작**
- Call By Reference는 C++문법
- 이유
    - Java에서 변수를 선언하면 스택 영역에 할당됨
    - 원시 타입은 스택 영역에 변수와 함께 저장됨
        - 원시타입=실제 데이터값을 저장하는 타입
    - 참조 타입은 실제 객체는 힙 영역에 저장되고 스택 영역에 있는 참조 타입의 변수는 객체의 주소값을 가지고 있음
        - 참조 타입=메모리 번지 값을 통해 객체를 참조하는 타입
    
    → 자바가 함수에 값을 복사해서 전달할 때, 원시 타입은 값 자체가 복사되고 참조 타입은 주소값이 복사됨
    
    - 복사된 값이 함수안에서 바뀌더라도 원래 변수는 바뀌지 않음

### 참조 타입은 주소값이 복사되는데 왜 Call by Reference가 아닐까?

- 참조 타입이면 **주소값**이 전달되는거임
    - 참조 주소를 직접 전달하는게 아니라는 뜻
- 헷갈리는 이유
    
    ```jsx
    void changeName(Person p) {
        p.name = "Lee";
    }
    
    Person p1 = new Person("Kim");
    changeName(p1);
    System.out.println(p1.name);
    ```
    
    - 출력 결과가 Lee라서 객체 속성을 바꾸면 원본 객체에 반영되는 것처럼 보이기 때문
- **하지만 진짜 Call by Reference라면 p 자체를 다른 객체로 바꿨을 때도 원본 변수에 영향을 줘야함**
    
    ```jsx
    void reset(Person p) {
        p = new Person("Lee");
    }
    
    Person p1 = new Person("Kim");
    reset(p1);
    System.out.println(p1.name);
    ```
    
    - 출력결과: Kim
    - 왜냐면 p는 주소값이 복사된 것 뿐이고 p가 가리키는 대상을 바꾼거임
    - p1은 여전히 처음 객체를 가리키고 있음

## Python 파라미터 전달 방법

- 파이썬은 **Call by assignment**

### **Call by assignment란?**

- Immutable Object는 Call by value형식으로 변수를 핸들링
- Mutable Object인 경우에는 Call by reference형식으로 변수를 핸들링
- Immutable Object: tuple, str, int 등 불가변적 포맷의 자료형
- Mutable Object: list, dictionary, set 등 가변적 포맷의 자료형

# 추가 질문

## **Java에서 Call by Value가 객체의 속성까지 바꿀 수 있는 이유는?**

- Java는 참조형 변수의 주소값을 복사해서 전달 → 함수 안에서 원래 객체와 같은 메모리 공간을 참조함
    - 객체의 속성을 변경하면 원본 객체에도 그 변경이 반영되는것

## Python에서 list와 str의 동작이 다른 이유는?

- list는 mutable object라서 함수 내부에서 리스트 내용을 변경하면 원본도 바뀜
- str은 imutable object라서 함수 내부에서 새로운 값을 할당해도 원본이 바뀌지 않음