# Paging

## 페이징(Paging)이란?
- 표준화된 페이징을 사용하여 데이터를 직렬화하여 제한하기

---

## 페이징 개념
- `Pageable` 인터페이스
    - `Page<T>` 인터페이스
        - `Content` : 데이터
        - `Pageable` : 페이징 정보
        - `PageRequest` : 페이징 정보
        - `PageRequest.of(int page, int size)` : 페이징 정보 생성
        - `PageRequest.of(int page, int size, Sort sort)` : 페이징 정보 생성
        - `PageRequest.of(int page, int size, Sort.Direction direction, String property)` : 페이징 정보 생성

---

## 예시
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Page<User> findByName(String name, Pageable pageable); // SELECT * FROM users WHERE name = ? ORDER BY ?
}
```

```java
@GetMapping("/users")
public ResponseEntity<Page<User>> findAll(
        PageDefault pageDefault,
        @PageableDefault(size = 10, sort = "id", direction = Sort.Direction.ASC) Pageable pageable) 
{
    Page<User> users = userService.findAll(pageable);
    return ResponseEntity.ok().body(users);
}
```

--- 

## Page vs Slice vs List
- `Page<T>` : 전체 개수, 페이지 정보 포함 (count 쿼리 발생)
- `Slice<T>` : 다음 페이지 여부만 판단 (count 쿼리 없음, 더 빠름)
- `List<T>` : 페이징 X

---

## Page 객체 내부 메서드
```java
Page<User> users = userService.findAll(pageable);

page.getCount(); // 전체 개수
page.getTotalElements(); // 전체 개수
page.getTotalPages(); // 전체 페이지 수
page.getNumber(); // 평가 페이지 번호
page.hasNext(); // 다음 페이지 여부
page.isFirst(); // 첫 페이지 여부
page.isLast(); // 마지막 페이지 여부
```