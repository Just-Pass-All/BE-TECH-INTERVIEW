# String, StringBuilder, StringBuffer 각각의 차이에 대해 설명해주세요.

## 면접용 답변

Java에서 문자열을 다룰 때 주로 사용되는 클래스는 `String`, `StringBuilder`, `StringBuffer`입니다. 이들 각각의 차이점은 다음과 같습니다.  
String은 불변 객체로, 값이 변경될 때마다 새로운 객체가 생성됩니다. StringBuilder와 StringBuffer는 가변 객체로 문자열을 효율적으로 수정할 수 있으며, StringBuffer만 스레드 안전합니다. 단일 스레드 환경에서는 StringBuilder, 멀티스레드 환경에서는 StringBuffer를 사용하는 것이 적합합니다.

## 개념 설명
### String
  - 불변(immutable) 객체로, 한번 생성된 후에는 변경할 수 없습니다.
  - 문자열을 수정할 때마다 새로운 객체가 생성됩니다.
  - 메모리 사용량이 많고 성능이 떨어질 수 있습니다.
  - 예시:
    ```java
    String str1 = "Hello";
    String str2 = str1.concat(" World");
    // str1은 여전히 "Hello", str2는 "Hello World"
    ```
- 주요 함수 및 예시:
    - `length()`: 문자열의 길이를 반환합니다.
        ```java
        String str = "Hello";
        System.out.println(str.length()); // 출력: 5
        ```
    - `charAt(int index)`: 지정한 인덱스의 문자를 반환합니다.
        ```java
        String str = "Hello";
        System.out.println(str.charAt(1)); // 출력: e
        ```
    - `substring(int beginIndex, int endIndex)`: 지정한 범위의 부분 문자열을 반환합니다.
        ```java
        String str = "Hello World";
        System.out.println(str.substring(0, 7)); // 출력: Hello W
        // 0부터 7 전까지의 인덱스에 해당하는 문자열을 반환
        ```
    - `toUpperCase()`, `toLowerCase()`: 대소문자 변환
        ```java
        String str = "Hello";
        System.out.println(str.toUpperCase()); // 출력: HELLO
        System.out.println(str.toLowerCase()); // 출력: hello
        ```
    - `equals(Object obj)`: 문자열 내용 비교
        ```java
        String str1 = "Hello";
        String str2 = "Hello";
        System.out.println(str1.equals(str2)); // 출력: true
        ```
    - `replace(CharSequence target, CharSequence replacement)`: 문자열 치환
        ```java
        String str = "Hello World";
        System.out.println(str.replace("World", "Java")); // 출력: Hello Java
        ```
    - `split(String regex)`: 정규표현식 기준으로 문자열 분리
        ```java
        String str = "a,b,c";
        String[] arr = str.split(",");
        System.out.println(Arrays.toString(arr)); // 출력: [a, b, c]
        ```

### StringBuilder
  - 가변(mutable) 객체로, 생성된 후에도 문자열을 변경할 수 있습니다.
  - 문자열을 수정할 때 새로운 객체가 생성되지 않고, 기존 객체의 내용을 변경합니다.
  - 메모리 사용량이 적고 성능이 향상될 수 있습니다.
  - 스레드 안전하지 않으므로(Thread-unsafe) 멀티스레드 환경에서는 주의가 필요합니다.
- 예시 (쓰레드 안전하지 않은 상황):
    ```java
    StringBuilder sb = new StringBuilder();
    Runnable task = () -> {
      for (int i = 0; i < 1000; i++) {
        sb.append("a");
      }
    };
    Thread t1 = new Thread(task);
    Thread t2 = new Thread(task);
    t1.start();
    t2.start();
    try {
      Thread.sleep(1000); // 스레드가 작업을 완료할 시간을 줍니다.
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println(sb.length()); // 결과값은 2000이 아닐 수 있습니다. (예: 1567, 1983 등)
    // 여러 스레드가 동시에 접근하면 데이터가 꼬일 수 있습니다.
    ```
- 주요 함수 및 예시:
    - `append(String str)`: 문자열을 추가합니다.
        ```java
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World");
        System.out.println(sb.toString()); // 출력: Hello World
        ```
    - `insert(int offset, String str)`: 지정한 위치에 문자열을 삽입합니다.
        ```java
        StringBuilder sb = new StringBuilder("Hello");
        sb.insert(5, " World");
        System.out.println(sb.toString()); // 출력: Hello World
        ```
    - `replace(int start, int end, String str)`: 지정한 범위의 문자열을 치환합니다.
        ```java
        StringBuilder sb = new StringBuilder("Hello");
        sb.replace(0, 5, "Hi");
        System.out.println(sb.toString()); // 출력: Hi
        ```
    - `delete(int start, int end)`: 지정한 범위의 문자열을 삭제합니다.
        ```java
        StringBuilder sb = new StringBuilder("Hello World");
        sb.delete(5, 11);
        System.out.println(sb.toString()); // 출력: Hello
        ```

### StringBuffer
  - 가변(mutable) 객체로, 생성된 후에도 문자열을 변경할 수 있습니다.
  - 문자열을 수정할 때 새로운 객체가 생성되지 않고, 기존 객체의 내용을 변경합니다.
  - 문자열을 수정할 때 기존 객체의 내용을 직접 변경합니다.
  - 예시:
    ```java
    StringBuffer sb = new StringBuffer("Hello");
    sb.append(" World");
    System.out.println(sb.toString()); // 출력: Hello World
    sb.insert(5, ",");
    System.out.println(sb.toString()); // 출력: Hello, World
    sb.replace(0, 5, "Hi");
    System.out.println(sb.toString()); // 출력: Hi, World
    sb.delete(3, 5);
    System.out.println(sb.toString()); // 출력: Hi,World
    ```
  - 메모리 사용량이 적고 성능이 향상될 수 있습니다.
  - 스레드 안전(Thread-safe)하여 멀티스레드 환경에서도 안전하게 사용할 수 있습니다.
  - 예시 (쓰레드 안전한 상황):
    ```java
    StringBuffer sb = new StringBuffer();
    Runnable task = () -> {
        for (int i = 0; i < 1000; i++) {
            sb.append("a");
        }
    };
    Thread t1 = new Thread(task);
    Thread t2 = new Thread(task);
    t1.start();
    t2.start();
    t1.join();
    t2.join();
    System.out.println(sb.length()); // 결과값: 2000
    ```
- 주요 함수 및 예시:
    - `append(String str)`: 문자열을 추가합니다.
        ```java
        StringBuffer sb = new StringBuffer("Hello");
        sb.append(" World");
        System.out.println(sb.toString()); // 출력: Hello World
        ```
    - `insert(int offset, String str)`: 지정한 위치에 문자열을 삽입합니다.
        ```java
        StringBuffer sb = new StringBuffer("Hello");
        sb.insert(5, " World");
        System.out.println(sb.toString()); // 출력: Hello World
        ```
    - `replace(int start, int end, String str)`: 지정한 범위의 문자열을 치환합니다.
        ```java
        StringBuffer sb = new StringBuffer("Hello");
        sb.replace(0, 5, "Hi");
        System.out.println(sb.toString()); // 출력: Hi
        ```
    - `delete(int start, int end)`: 지정한 범위의 문자열을 삭제합니다.
        ```java
        StringBuffer sb = new StringBuffer("Hello World");
        sb.delete(5, 11);
        System.out.println(sb.toString()); // 출력: Hello
        ```
    - `reverse()`: 문자열을 반전합니다.
        ```java
        StringBuffer sb = new StringBuffer("Hello");
        sb.reverse();
        System.out.println(sb.toString()); // 출력: olleH
        ```

### 요약

- 문자열의 변경이 자주 발생하지 않는 경우 : `String` 사용
- 문자열의 변경이 자주 발생하는 경우 : `StringBuilder` 사용
- 멀티스레드 환경  : `StringBuffer` 사용 권장
- 싱글스레드 환경 : `StringBuilder` 사용 권장

## 꼬리 질문

- String, StringBuilder, StringBuffer의 메모리 사용량 차이는 어떻게 되나요?

- String은 불변 객체인데, 반복적인 문자열 연결 작업에서 어떤 문제가 발생하나요?

- StringBuffer는 왜 thread-safe한가요? 내부적으로 어떤 방식으로 동기화를 하나요?

- 문자열 리터럴은 어디에 저장되며, 그로 인한 장단점은?