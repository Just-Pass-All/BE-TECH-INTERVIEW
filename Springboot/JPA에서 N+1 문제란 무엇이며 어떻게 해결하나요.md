# JPA에서 N+1 문제란 무엇이며 어떻게 해결하나요?
## 면접 답변
JPA에서 N+1 문제는 1개의 쿼리를 실행한 뒤, **연관된 엔티티를 조회하기 위해 추가로 N개의 쿼리가 실행되는 문제**입니다. 
예를 들어 회원 목록을 조회한 후 각 회원의 주문 정보를 불러올 때, 회원 수만큼 쿼리가 추가로 발생하게 되고, 이로 인해 성능 저하가 나타날 수 있습니다.
이러한 N+1 문제는 **Fetch Join, Batch Size 설정** 등을 활용해 해결할 수 있습니다.

<br>

## 개념 정리
### N+1 문제란
  - JPA의 기본 설정인 지연 로딩(LAZY LOADING)은 연관된 엔티티를 실제로 접근할 때 데이터를 조회한다. <br>
    → 이로 인해 하나의 쿼리로 부모 엔티티를 조회한 후, 연관된 자식 엔티티를 N번 조회하는 문제가 발생한다.

**예시 코드**
```java
// 1번 쿼리) 회원 리스트 조회
List<Member> members = memberRepository.findAll();

for (Member member : members) {
    // N번 쿼리) 회원마다 주문 조회
    List<Order> orders = member.getOrders();
}
```
이 경우, 회원이 10명이면 총 11번의 쿼리가 실행된다. <br>
**→ N+1 문제 발생**

<br>

### 해결 방법
| 방법               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| Fetch Join         | JPQL에서 `JOIN FETCH`를 사용해 연관 엔티티를 한 번에 조회하는 방식  |
| Batch Size 설정    | Hibernate의 `default_batch_fetch_size` 설정을 통해 여러 건을 IN 쿼리로 묶어 조회 |

<br>

#### 1. Fetch Join
Fetch Join은 JPQL에서 연관 엔티티를 한 번에 조회하기 위해 사용하는 방법으로, 단일 쿼리로 연관된 모든 데이터를 조회할 수 있다.

**예시 코드**
```java
@Query("SELECT m FROM Member m JOIN FETCH m.orders")
List<Member> findAllWithOrders();
```
<br>

#### 2. Batch Size 설정
Hibernate의 default_batch_fetch_size 또는 @BatchSize를 통해 Lazy Loading 컬렉션을 IN 쿼리로 묶어서 조회하는 방식으로, 연관된 엔티티들을 한 번에 조회해 N+1 쿼리 문제를 줄일 수 있다.
- Batch Size는 @OneToOne, @ManyToOne, @OneToMany, @ManyToMany에 모두 적용 가능하다.
- 설정한 사이즈보다 데이터가 많으면 여러 번에 나눠서 쿼리가 발생할 수 있다.

<br>

**예시 코드** <br>
**application.yml**
```yml
spring:
  jpa:
    properties:
      hibernate.default_batch_fetch_size: 100
```

**Entity에 직접 지정**
```java
@BatchSize(size = 100)
@OneToMany(mappedBy = "member")
private List<Order> orders;
```

<br>

**활용되는 예시**
```java
List<Member> members = memberRepository.findAll(); // LAZY 상태

for (Member member : members) {
    List<Order> orders = member.getOrders(); // 배치 사이즈에 따라 IN 쿼리 발생
}
```

**실행 SQL**
```sql
SELECT * FROM orders WHERE member_id IN (?, ?, ?, ...)
```

<br>

## 꼬리 질문
1. Fetch Join과 일반 Join의 차이점은 무엇인가요?  
   `일반 Join`은 SQL 수준에서 테이블을 조인하지만, 연관된 엔티티를 JPA가 영속성 컨텍스트에 직접 로딩하지는 않습니다.
   반면, `Fetch Join`은 JPA에서 연관된 엔티티를 즉시 로딩하여 연관 객체까지 한 번의 쿼리로 모두 불러옵니다. 즉, Fetch Join은 조인과 동시에 연관 객체를 메모리에 올리는 차이점이 있습니다.
2. EAGER 로딩을 사용하면 N+1 문제가 해결할 수 있을까요?  
   EAGER 로딩은 연관 객체를 즉시 로딩하지만 N+1 문제는 발생할 수 있는데, 이는 JPA가 컬렉션이나 연관 필드를 개별적으로 쿼리하는 구조로 처리하기 때문입니다.
   따라서 N+1 문제는 Fetch Join이나 Batch Size 설정을 통해 해결하는 것이 좋습니다.
3. Fetch Join을 사용할 때 발생할 수 있는 문제점은 무엇인가요?  
   Fetch Join은 여러 연관 엔티티를 한 번에 가져오므로 데이터 중복이나, 모든 행의 가능한 조합을 만드는 Cartesian Product가 발생할 수 있습니다.
   특히 OneToMany 관계에서 Fetch Join을 사용할 경우, 페이징이 불가능하거나 잘못된 결과가 나올 수 있습니다.
