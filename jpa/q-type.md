# QueryDSL Q-Type

## Q-Type이란?
- QueryDSL이 엔티티 클래스를 분석해서 자동으로 생성하는 클래스
- `Q도메인 클래스`를 생성하여 `.where()`, `.select()` 등의 메서드 체인으로 쿼리 구성
- 이름 앞에 Q가 붙음 (`QBook`, `QMember`, `QOrder`, `QOrderItem`)

---

## Q-Type 사용
### 1. 설정
```groovy
dependencies {
    
    // querydsl-apt:jakarta로 지정해야 Spring Boot 3.x에서 호환 가능
    
    api 'org.springframework.boot:spring-boot-starter-data-jpa'
    api "com.querydsl:querydsl-jpa:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"

    implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.11.0'

    // https://docs.spring.io/spring-boot/3.4/appendix/dependency-versions/coordinates.html
    annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"
    annotationProcessor "jakarta.annotation:jakarta.annotation-api"
    annotationProcessor "jakarta.persistence:jakarta.persistence-api"

    runtimeOnly 'org.postgresql:postgresql'
    runtimeOnly 'com.h2database:h2'
}
```
### 2. 생성 위치
- build/generated/source/annotationProcessor/java/main

### 3. Config파일에 빈 등록
```java
@Import(AuditConfig.class)
@EnableJpaRepositories(basePackageClasses = JpaBase.class, basePackages = "com.corepub.localmarket.bmarketapi.v3.*")
@EntityScan(basePackageClasses = JpaBase.class)
@Configuration
@RequiredArgsConstructor
public class JpaConfig {

    // @PersistenceContext
    // private EntityManager entityManager;

    private final EntityManagerFactory entityManagerFactory;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManagerFactory.createEntityManager());
    }

}
```

### 4. 사용
```java
public interface ProductRepository extends ProductJpaRepository, ProductRepositoryCustom {
}
```
```java
@NoRepositoryBean
public interface ProductRepositoryCustom {

    Page<ProductEntity> findProductsBy(Long categoryId, Pageable pageable);

}
```
```java
@Repository
@RequiredArgsConstructor
public class ProductRepositoryImpl implements ProductRepositoryCustom {

    // private final EntityManagerFactory entityManagerFactory;
    private final JPAQueryFactory factory;


    @Override
    public Page<ProductEntity> findProductsBy(Long categoryId, Pageable pageable) {
        /*
         * SELECT * FROM products;
         */
        List<ProductEntity> productEntities = factory
                // .select(QProductEntity.productEntity)
                // .from(QProductEntity.productEntity)
                .selectFrom(QProductEntity.productEntity)
                .fetch();

        return new PageImpl<>(productEntities);
    }
}
```

---
