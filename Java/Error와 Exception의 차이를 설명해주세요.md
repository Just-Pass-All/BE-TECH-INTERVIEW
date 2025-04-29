## 면접 답변

- 자바에서는 오류를 오류(Error)와 예외(Exception)로 나눕니다. 오류는 시스템이 종료되어야 할 수준과 같이 수습할 수 없는 심각한 문제를 의미하며 개발자가 미리 예측하여 방지할 수 없습니다. 반면 예외는 개발자가 구현한 로직에서 발생한 실수나 사용자의 영향에 의해 발생하며 개발자가 미리 예측하여 방지할 수 있습니다.
    
    오류와 예외 모두 Object 클래스를 상속받는 Throwable 클래스를 상속받습니다. Throwable 객체는 오류나 예외에 대한 메시지를 담고, 예외가 연결될 때 연결된 예외의 정보를 기록합니다.
    

# 개념 설명

## Throwable

- 오류와 예외 모두 자바의 최상위 클래스인 Object를 상속받음
    - Throwable이라는 클래스와 상속관계임
- Throwable 객체가 가진 정보와 할 수 있는 행위는 getMessage()와 printStackTrace()라는 메서드로 구현되어있음

## Error

### 대표적인 오류

- StackOverflowError: 응용 프로그램이 너무 많이 반복되어 StackOverflow가 발생할 때 던져지는 오류
- OutOfMemoryError: JVM이 할당된 메모리의 부족으로 더 이상 객체를 할당할 수 없을 때 던져지는 오류
    - Garbage Collector에 의해 추가적인 메모리가 확보되지 못하는 상황일 때
    - heap 사이즈 부족
    - 너무 많은 class를 로드할 때
    - 가용가능한 swap이 없을 때
    - 큰 메모리의 native메서드가 호출될 때

## Exception: 2가지 종류 → 꼭 처리를 해야하는지에 대한 것

### Checked Exception

- 예외처리가 필수이며, 처리하지 않으면 컴파일되지 않음
- 컴파일 단계에서 명확하게 Exception체크가 가능함
- 예외 발생시 트랜잭션을 roll-back 하지 않고 예외를 던져줌
- JVM 외부와 통신(네트워크, 파일시스템 등)할 때 주로 사용

### Unchecked Exception

- RuntimeException 하위의 모든 예외
    - NullPointerException, IndexOutOfBoundException
- 실행과정 중 어떠한 특정 논리에 의해 발견되는 Exception
- 예외 발생시 트랜잭션을 roll-back함

### **ArithmeticException**

- 정수를 0으로 나눌경우 발생

### **ArrayIndexOutOfBoundsException**

배열의 범위를 벗어난 index를 접근할 시 발생

### **NullPointException**

존재하지 않는 레퍼런스를 참조할 때 발생

### **ClassCastException**

변화할 수 없는 타입으로 객체를 반환 시 발생

## 꼬리질문

- StackOverflowError를 막기 위한 프로그래밍적인 방법은 뭐가 있을까요?
- printStackTrace() 대신 실무에서는 어떤 로깅 방법을 쓰나요?
- Checked Exception을 모두 Unchecked로 던지는 게 편하지 않나요?

[https://velog.io/@jipark09/Java-Error와-Exception-차이](https://velog.io/@jipark09/Java-Error%EC%99%80-Exception-%EC%B0%A8%EC%9D%B4)