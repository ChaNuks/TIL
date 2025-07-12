# 지연 로딩(Lazy Loading)과 즉시 로딩(Eager Loading)

## 1. 즉시 로딩(Eager Loading)

### 1.1 정의
- 엔티티를 조회할 때 연관된 엔티티를 즉시 로딩하여 함께 가져오는 방식

### 1.2 특징
- 연관된 엔티티를 즉시 로딩하므로, 데이터베이스 쿼리가 한 번에 처리됨
- 연관된 엔티티가 항상 필요할 때 사용
- 데이터베이스 쿼리가 많아질 수 있어 성능 저하가 발생할 수 있음
- 엔티티가 로딩될 때 연관된 엔티티도 함께 로딩되므로, N+1 문제를 피할 수 있음
- JPA에서는 `@OneToMany(fetch = FetchType.EAGER)`와 같이 설정하여 즉시 로딩을 지정할 수 있음

### 1.3 예시
```java
@Entity
public class User {
    @Id
    private Long id;
    
    @OneToMany(fetch = FetchType.EAGER)
    private List<Order> orders;

    // Getters and Setters
}
```

## 2. 지연 로딩(Lazy Loading)
### 2.1 정의
- 엔티티를 조회할 때 연관된 엔티티를 필요할 때까지 로딩하지 않는 방식
- 연관된 엔티티에 접근할 때 데이터베이스에서 조회하여 로딩함

### 2.2 특징
- 연관된 엔티티를 필요할 때만 로딩하므로, 초기 로딩 시 성능이 향상됨
- 데이터베이스 쿼리가 적게 발생하여 성능이 향상될 수 있음
- 연관된 엔티티에 접근할 때마다 데이터베이스 쿼리가 발생하므로, N+1 문제를 유발할 수 있음
- JPA에서는 `@OneToMany(fetch = FetchType.LAZY)`와 같이 설정하여 지연 로딩을 지정할 수 있음
- 프록시 객체를 사용하여 실제 엔티티에 접근할 때 데이터베이스에서 조회함
- 프록시 객체는 실제 엔티티와 동일한 인터페이스를 구현하며, 실제 엔티티에 접근할 때 데이터베이스에서 데이터를 조회하여 채워짐

### 2.3 예시
```java
@Entity
public class User {
    @Id
    private Long id;
    
    @OneToMany(fetch = FetchType.LAZY)
    private List<Order> orders;

    // Getters and Setters
}
```
---

## N+1 문제란?
- 지연 로딩을 사용할 때 발생할 수 있는 성능 저하 문제
- 예를 들어, 100개의 `User` 엔티티가 있고, 각 `User`가 여러 개의 `Order` 엔티티를 가지고 있을 때, 각 `User`에 대한 `Order`를 조회하기 위해 100번의 추가 쿼리가 발생함
- 이로 인해 총 101번의 쿼리가 발생하게 되어 성능 저하가 발생함
- 이를 해결하기 위해 `@EntityGraph`를 사용하거나, `JOIN FETCH`를 사용하여 연관된 엔티티를 한 번에 로딩할 수 있음
- `@EntityGraph`를 사용하면 필요한 연관 엔티티만을 지정하여 로딩할 수 있어 성능을 최적화할 수 있음
- `JOIN FETCH`를 사용하면 연관된 엔티티를 조인하여 한 번의 쿼리로 가져올 수 있음
- 예시:
```java
@Entity
public class User {
    @Id
    private Long id;

    @OneToMany(fetch = FetchType.LAZY)
    private List<Order> orders;

    // Getters and Setters
}
@EntityGraph(attributePaths = {"orders"})
public List<User> findAllWithOrders() {
    return entityManager.createQuery("SELECT u FROM User u", User.class)
                        .setHint("javax.persistence.fetchgraph", "User.orders")
                        .getResultList();
}
```
---

# JPA - 즉시 로딩(EAGER) vs 지연 로딩(LAZY) 비교

| **비교 항목** | **즉시 로딩 (EAGER)**              | **지연 로딩 (LAZY)**                    |
|---------------|-------------------------------------|------------------------------------------|
| **조회 시점**  | 즉시                                | 실제 사용 시                             |
| **성능**       | 느림 (JOIN 많음)                    | 빠름 (필요할 때만 조회)                  |
| **위험 요소**  | N+1 문제                             | `LazyInitializationException` 가능       |
| **권장 여부**  | ❌ 실무에서는 주의 필요              | ✅ 실무에서 선호됨                        |

> 💡 실무에서는 대부분 `LAZY`를 기본으로 설정하고, 
> **Fetch Join** 등으로 필요한 시점에 조절하는 것이 일반적

---

## 지연 로딩과 즉시 로딩의 선택
- 연관된 엔티티가 항상 필요할 때 == ```즉시(EAGER) 로딩```
- 연관된 엔티티가 필요할 때만 로딩하고 싶을 때 == ```지연(LAZY) 로딩```

