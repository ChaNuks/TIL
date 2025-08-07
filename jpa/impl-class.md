# Impl

## 따로 Impl 클래스 만드는 이유?
- 복잡한 쿼리(QueryDSL)를 Repository 인터페이스에서 분리해서 관리
- Repository는 선언만, Impl은 구현만 -> SRP 준수
- 구현체를 독립적으로 테스트 or 다른 구현체로 교체 용이


---

## 구성

```java
// 1. 선언부 (Custom 인터페이스)
public interface ProductRepositoryCustom {
    Page<ProductEntity> findProductsBy(Long categoryId, Pageable pageable);
}

// 2. 구현부 (Impl 클래스)
@Repository
@RequiredArgsConstructor
public class ProductRepositoryImpl implements ProductRepositoryCustom {
    private final JPAQueryFactory factory;

    @Override
    public Page<ProductEntity> findProductsBy(Long categoryId, Pageable pageable) {
        // QueryDSL 로직
    }
}

// 3. 실제 Repository
public interface ProductRepository extends JpaRepository<ProductEntity, Long>, ProductRepositoryCustom {
}
```

---

## 근데 왜 Service가 아니라 Repository를 이용?
- Service -> 비즈닉스 로직 담당
- Repository -> 쿼리 구성, 페이징 필터리 담당
- 즉, 데이터는 Repository가 책임지기 떄문

---

## 물론 Service에서도 따로 만들어 관리함
- Service 인터페이스 만들고, ServiceImpl 클래스 만들어서 관리