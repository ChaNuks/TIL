# JPA - 영속성 컨텍스트 (Persistence Context)

## 영속성 컨텍스트란?
- JPA에서 엔티티(Entity)를 관리하는 환경을 의미
- Entity의 생명주기를 관리하며, DB의 상호작용을 최적화함
- 일종의 **1차 캐시 역할**을 하며, 엔티티 매니저(EntityManager)가 관리
- 엔티티 매니저가 **데이터베이스와의 중간 매개체**로 동작하면서 엔티티 객체의 상태를 추적·관리

--- 

## 생명 주기
1. **비영속 상태 (Transient)**
    - 엔티티가 영속성 컨텍스트에 존재하지 않는 상태
    - 데이터베이스에 저장되지 않음
    - 예: `new User()`


2. **영속 상태 (Managed)**
    - 엔티티가 영속성 컨텍스트에 존재하는 상태
    - 데이터베이스에 저장되며, 변경 사항이 자동으로 반영됨
    - 동일성 보장(== 연산자 사용 가능)
    - 1차 캐시에 저장됨
    - 변경 감지(Dirty Checking) 기능을 통해 엔티티의 상태 변화가 자동으로 감지됨
    - 예: `entityManager.persist(user)`


3. **준영속 상태 (Detached)**
    - 엔티티가 영속성 컨텍스트에서 분리된 상태
    - 데이터베이스와의 연결이 끊어짐
    - 변경 사항이 자동으로 반영되지 않음
    - 예: `entityManager.detach(user)`, `entityManager.clear()`


4. **삭제 상태 (Removed)**
    - 엔티티가 영속성 컨텍스트에서 제거된 상태
    - 데이터베이스에서 삭제됨
    - 예: `entityManager.remove(user)`

---
## 영속성 컨텍스트의 특징
- **1차 캐시**
  - **엔티티 식별자**를 기준으로 엔티티를 관리 
  - 동일 트랜잭션 내에서 같은 Entity는 한 번만 DB에서 조회됨
  - Entity를 1차  캐시에 저장하여, 동일한 엔티티에 대한 중복 조회를 방지
  - 예: `entityManager.find(User.class, id)` 호출 시, 영속성 컨텍스트에 해당 엔티티가 존재하면 새로 조회하지 않고 캐시된 엔티티를 반환


- **변경 감지 (Dirty Checking)**
  - 영속성 컨텍스트에 있는 엔티티의 상태를 추적하여, 변경된 부분만 데이터베이스에 반영
  - 트랜잭션 커밋 시점에 변경된 엔티티를 자동으로 업데이트
  - 예: `user.setName("New Name")` 후, `entityManager.flush()` 호출 시 변경된 내용이 DB에 반영됨
  - 변경 감지는 `equals()`와 `hashCode()` 메서드를 사용하여 엔티티의 상태를 비교함
  - 엔티티의 필드 값이 변경되면, JPA는 해당 엔티티를 자동으로 업데이트할 준비를 함


- **엔티티 동등성 (Identity)**
  - 영속성 컨텍스트 내에서 동일한 엔티티는 동일한 인스턴스로 관리됨
  - `==` 연산자를 사용하여 엔티티의 동일성을 비교할 수 있음
  - 예: `user1 == user2`가 true인 경우, 두 엔티티는 동일한 인스턴스를 참조함
  - `equals()` 메서드를 오버라이드하여 엔티티의 동등성을 정의할 수 있음
  - 예: 
    ```java
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User)) return false;
        User user = (User) o;
        return Objects.equals(id, user.id);
    }
    ```


- **트랜잭션 범위**
  - 영속성 컨텍스트는 트랜잭션 범위 내에서 관리됨
  - 트랜잭션이 커밋되면 영속성 컨텍스트의 변경 사항이 데이터베이스에 반영됨
  - 트랜잭션이 롤백되면 영속성 컨텍스트의 변경 사항은 무시됨
  - 예: `@Transactional` 어노테이션을 사용하여 메서드나 클래스에 트랜잭션을 적용
  - ```java
    @Transactional
    public void saveUser(User user) {
        entityManager.persist(user);
    }
    ```


- **영속성 컨텍스트의 범위**
  - 영속성 컨텍스트는 `EntityManager`의 생명주기에 따라 다름
  - `EntityManager`가 생성될 때 영속성 컨텍스트가 시작되고, `EntityManager`가 닫힐 때 영속성 컨텍스트도 종료됨
  - 예: `@PersistenceContext`를 사용하여 `EntityManager`를 주입받아 사용
  - ```java
    @PersistenceContext
    private EntityManager entityManager;
    ```


- **Flush**
  - 영속성 컨텍스트의 변경 사항을 데이터베이스에 반영하는 작업
  - `entityManager.flush()`를 호출하여 영속성 컨텍스트의 변경 사항을 즉시 데이터베이스에 반영할 수 있음
  - 트랜잭션 커밋 시 자동으로 호출됨
  - 예: 
    ```java
    @Transactional
    public void updateUser(User user) {
        user.setName("Updated Name");
        entityManager.flush(); // 변경 사항을 즉시 DB에 반영
    }
    ```

---
## 영속성 컨텍스트의 활용
- **엔티티 조회**: `find()`, `getReference()` 메서드를 사용하여 영속성 컨텍스트에서 엔티티를 조회
  - `find()`는 실제 데이터를 조회하고, `getReference()`는 프록시 객체를 반환
  - 예: 
    ```java
    User user = entityManager.find(User.class, userId); // 실제 엔티티 조회
    User userProxy = entityManager.getReference(User.class, userId); // 프록시 객체 반환
    ```

- **엔티티 저장**: `persist()`, `merge()` 메서드를 사용하여 엔티티를 영속성 컨텍스트에 저장
  - `persist()`는 새로운 엔티티를 영속성 컨텍스트에 추가하고, `merge()`는 준영속 상태의 엔티티를 영속성 컨텍스트에 병합
  - 예: 
    ```java
    User user = new User("John Doe");
    entityManager.persist(user); // 새로운 엔티티 저장
    ```
    ```java
    User detachedUser = new User("Jane Doe");
    entityManager.merge(detachedUser); // 준영속 상태의 엔티티 병합
    ```
    ```java
    User user = entityManager.merge(detachedUser); // 병합 후 영속 상태로 변경
    ```
    

- **엔티티 삭제**: `remove()` 메서드를 사용하여 영속성 컨텍스트에서 엔티티를 삭제
  - 예: 
    ```java
    User user = entityManager.find(User.class, userId);
    entityManager.remove(user); // 엔티티 삭제
    ```

- **트랜잭션 관리**: `@Transactional` 어노테이션을 사용하여 트랜잭션 범위 내에서 영속성 컨텍스트를 관리
  - 트랜잭션이 커밋되면 영속성 컨텍스트의 변경 사항이 데이터베이스에 반영됨
  - 예: 
    ```java
    @Transactional
    public void saveUser(User user) {
        entityManager.persist(user); // 트랜잭션 범위 내에서 엔티티 저장
    }
    ```
    ```java
    @Transactional
    public void updateUser(User user) {
        user.setName("Updated Name");
        // 트랜잭션 커밋 시 변경 사항이 자동으로 DB에 반영됨
    }
    ```
    ```java
    @Transactional
    public void deleteUser(Long userId) {
        User user = entityManager.find(User.class, userId);
        entityManager.remove(user); // 트랜잭션 범위 내에서 엔티티 삭제
    }
    ```
---

## 정리 표
| 항목               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| 영속성 컨텍스트     | JPA에서 엔티티를 관리하는 환경, 1차 캐시 역할을 함                        |
| 생명 주기           | 비영속(Transient), 영속(Managed), 준영속(Detached), 삭제(Removed) 상태로 구분 |
| 특징               | 1차 캐시, 변경 감지(Dirty Checking), 엔티티 동등성(Identity) 등              |
| 트랜잭션 범위       | 트랜잭션 범위 내에서 영속성 컨텍스트가 관리됨                               |

---

## 관련 메서드
| 메서드              | 설명                                          |
|------------------|---------------------------------------------|
| `persist()`      | 새로운 엔티티를 영속성 컨텍스트에 추가                       |
| `find()`         | 영속성 컨텍스트에서 엔티티를 조회, 실제 데이터를 반환              |
| `getReference()` | 영속성 컨텍스트에서 엔티티의 프록시 객체를 반환, 실제 데이터는 조회하지 않음 |
| `merge()`        | 준영속 상태의 엔티티를 영속성 컨텍스트에 병합                   |
| `remove()`       | 영속성 컨텍스트에서 엔티티를 삭제                          |
| `flush()`        | 영속성 컨텍스트 -> DB 반영 (커밋 X)                    |
| `clear()`        | 모든 영속성 컨텍스트 비우기                             |
| `detach()`       | 특정 엔티티만 준영속 상태로 전환                          |

