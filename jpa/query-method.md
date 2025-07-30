# Query Method

## 쿼리 메서드(Query Method)
- Spring Data JPA와 JPA 쿼리 메서드를 이용한 쿼리 구현
- 메서드 이름만으로 자동으로 SQL/JPQL 쿼리를 생성

---

## 쿼리 메서드 패턴
- `find` : SELECT (조회)
- `save` : INSERT (생성)
- `delete` : DELETE (삭제)
- `update` : UPDATE (수정)
- `count` : SELECT COUNT (조회 개수)
- `exists` : EXISTS (조회 여부)

---

## 예시
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email); // SELECT * FROM users WHERE email = ?
    List<User> findByName(String name); // SELECT * FROM users WHERE name = ?
    List<User> findByNameOrEmail(String name, String email); // SELECT * FROM users WHERE name = ? OR email = ?
    List<User> findByNameAndEmail(String name, String email); // SELECT * FROM users WHERE name = ? AND email = ?
    List<User> findByNameOrEmailOrderByEmail(String name, String email); // SELECT * FROM users WHERE name = ? OR email = ? ORDER BY email
    List<User> findByName(String name, Sort sort); // SELECT * FROM users WHERE name = ? ORDER BY ?
    Page<User> findByName(String name, Pageable pageable); // SELECT * FROM users WHERE name = ? ORDER BY ? 
}
```