# ORM (Object-Relational Mapping)

## ORM이란?
- 객체 지향 프로그래밍 언어와 관계형 데이터베이스 간의 데이터를 `매핑`하는 기술
- 즉, `자바 객체 <-> DB 테이블 간 매핑`을 자동으로 해줌으로써 SQL을 직접 작성하지 않아도 데이터를 조작할 수 있게 해주는 추상화 계층

---

## ORM 동작 방식
1. **Entity 클래스 정의**: 데이터베이스 테이블과 매핑되는 자바 클래스를 정의
   - 예: `@Entity` 어노테이션을 사용하여 클래스 정의
   - 각 필드는 데이터베이스 테이블의 컬럼과 매핑됨
   - 예시:
   ```java
    @Entity
    public class User {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;

        @Column(nullable = false, length = 50)
        private String name;

        @Column(nullable = false, unique = true)
        private String email;

        // Getters and Setters
    }
    ```
2. **Entity 매핑**: Entity 클래스의 필드와 데이터베이스 테이블의 컬럼을 매핑
   - `@Column`, `@Id`, `@GeneratedValue` 등의 어노테이션을 사용하여 매핑 설정
   - 기본적으로 필드 이름과 컬럼 이름이 동일하면 자동으로 매핑됨
   - 필드 이름이 다를 경우 `@Column(name = "컬럼명")` 어노테이션을 사용하여 매핑 가능
   - 예시:
   ```java
    @Column(name = "user_name", nullable = false)
    private String name;
   ```
3. **Entity 매니저**: Entity 매니저를 사용하여 데이터베이스와 상호작용
4. **CRUD 작업**: Entity 매니저를 통해 Create, Read, Update, Delete 작업 수행
   - 예시:
   ```java
    @Autowired
    private EntityManager entityManager;

    public User save(User user) {
        entityManager.persist(user);
        return user;
    }

    public User findById(Long id) {
        return entityManager.find(User.class, id);
    }
    public void delete(User user) {
        entityManager.remove(user);
    }
    public List<User> findAll() {
        return entityManager.createQuery("SELECT u FROM User u", User.class).getResultList();
    }
    ```
5. **트랜잭션 관리**: 트랜잭션을 사용하여 데이터베이스 작업의 원자성 보장
6. **쿼리 생성**: ORM 프레임워크가 자동으로 SQL 쿼리를 생성하여 데이터베이스와 상호작용
   - JPQL(Java Persistence Query Language) 또는 Criteria API를 사용하여 쿼리 작성 가능
   - 예시:
   ```java
    List<User> users = entityManager.createQuery("SELECT u FROM User u WHERE u.name = :name", User.class)
                                     .setParameter("name", "John")
                                     .getResultList();
   ```
7. **캐싱**: ORM 프레임워크는 1차 캐시(세션 캐시)와 2차 캐시를 제공하여 성능 향상
8. **지연 로딩과 즉시 로딩**: 연관된 엔티티를 지연 로딩 또는 즉시 로딩으로 설정하여 성능 최적화
   - 예시:
   ```java
    @OneToMany(fetch = FetchType.LAZY)
    private List<Order> orders;
   ```
9. **엔티티 상태 관리**: 엔티티의 상태(영속성, 비영속성 등)를 관리하여 데이터베이스와의 동기화
10. **엔티티 리스너**: 엔티티의 생명주기 이벤트(삽입, 수정, 삭제 등)에 대한 리스너를 정의하여 추가 작업 수행
    - 예시:
    ```java
    @PrePersist
    public void prePersist(User user) {
        // 엔티티가 저장되기 전에 실행되는 로직
    }
    ```
11. **엔티티 그래프**: 복잡한 엔티티 관계를 정의하여 필요한 데이터만 로딩할 수 있도록 최적화
12. **DTO와 Entity 매핑**: DTO(Data Transfer Object)와 Entity 간의 매핑을 통해 데이터 전송 최적화
    - 예시:
    ```java
    public class UserDTO {
        private Long id;
        private String name;
        private String email;

        // Getters and Setters
    }
    ```
---

## 왜 ORM을 사용하는가?
| 장점          | 설명                                                         |
|---------------|--------------------------------------------------------------|
| 객체 지향적 접근 | SQL 쿼리를 직접 작성하지 않고 객체 지향적으로 데이터베이스 작업 수행 |
| 생산성 향상     | 반복적인 SQL 쿼리 작성 없이 데이터베이스 작업을 간편하게 처리 가능 |
| 유지보수 용이성  | 데이터베이스 구조 변경 시 코드 수정 최소화, 매핑 어노테이션만 수정 |
| 데이터베이스 독립성 | 다양한 데이터베이스에 대해 동일한 코드로 작업 가능, 데이터베이스 변경 시 최소한의 수정으로 대응 |
| 성능 최적화     | 1차 캐시, 2차 캐시, 지연 로딩 등 다양한 최적화 기법 제공          |
| 트랜잭션 관리   | 트랜잭션을 자동으로 관리하여 데이터 무결성 보장                  |
| 쿼리 최적화     | JPQL, Criteria API 등을 사용하여 복잡한 쿼리 작성 가능          |
| 테스트 용이성   | Entity와 DTO 간의 매핑을 통해 테스트 코드 작성 용이            |
| 확장성         | 플러그인 및 확장 기능을 통해 다양한 기능 추가 가능            |

---

## 그렇다면 JPA는 ORM인가?
- JPA(Java Persistence API)는 ORM(Object-Relational Mapping) 기술의 표준 인터페이스로, ORM 프레임워크를 구현하기 위한 규약을 제공
- JPA는 ORM의 일종으로, JPA를 구현한 프레임워크(예: Hibernate, EclipseLink 등)를 사용하여 ORM 기능을 제공
- JPA는 객체와 관계형 데이터베이스 간의 매핑을 정의하고, 데이터베이스 작업을 추상화하여 개발자가 SQL 쿼리를 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있도록 함
- JPA는 ORM의 표준 인터페이스로, JPA를 구현한 ORM 프레임워크를 사용하여 객체-관계 매핑을 수행함

---
## JPA와 ORM 프레임워크
| JPA (Java Persistence API) | ORM 프레임워크 (예: Hibernate, EclipseLink) |
|-----------------------------|-------------------------------------------|
| 표준 인터페이스               | 구현체                                      |
| 객체-관계 매핑 규약 제공         | JPA를 구현하여 ORM 기능 제공                  |
---
## 요약
| 구분          | JPA (Java Persistence API) | ORM 프레임워크 (예: Hibernate, EclipseLink) |
|----------------|-----------------------------|-------------------------------------------|
| 정의            | 객체-관계 매핑을 위한 표준 인터페이스 | JPA를 구현하여 ORM 기능을 제공하는 프레임워크 |
| 역할            | ORM 기능을 추상화하여 제공         | JPA를 구현하여 실제 데이터베이스 작업 수행       |
| 주요 기능       | 객체 매핑, 쿼리 생성, 트랜잭션 관리 등 | JPA의 기능을 구현하여 데이터베이스와 상호작용     |
