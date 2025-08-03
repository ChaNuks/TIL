# QueryDSL

## QueryDSL이란?
- JPQL을 JAVA 코드로 타입 안정성있게 작성할 수 있는 빌더형 DSL
- `Q도메인 클래스`를 생성하여 `.where()`, `.select()` 등의 메서드 체인으로 쿼리 구성

---

## 왜 사용함?
| 장점            | 설명                                                                 |
|-----------------|----------------------------------------------------------------------|
| 타입 안정성      | JPQL은 문자열 기반 → 컴파일 타임에 오류 못 잡음, QueryDSL은 IDE에서 에러 탐지 가능 |
| IDE 지원        | `.where()` 등의 메서드 자동완성 지원으로 생산성 향상                             |
| 동적 쿼리 용이   | `BooleanBuilder`나 `where 다중 파라미터` 방식으로 조건적 필터링이 쉬움             |
| 재사용성 높음    | 쿼리 빌더 조각을 따로 메서드로 빼서 조합 가능  

---

## 설정 방법 (Spring Boot 3.x + QueryDSL 5.1.0)
### Gradle 설정
```groovy
ext {
    querydslVersion = "5.1.0"
}

dependencies {
    // QueryDSL JPA
    implementation "com.querydsl:querydsl-jpa:${querydslVersion}"
    annotationProcessor "com.querydsl:querydsl-apt:${querydslVersion}:jakarta"

    // jakarta 의존성 (Spring Boot 3.x는 javax -> jakarta)
    annotationProcessor "jakarta.persistence:jakarta.persistence-api:3.1.0"
    annotationProcessor "jakarta.annotation:jakarta.annotation-api:2.1.1"

    // 기타 Spring Data JPA 의존성
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
}
```

---

## 그럼 Q타입 클래스가 뭔데?
- 각 Entity에 대해 Q엔티티명 클래스가 생성됨 (`QBook`, `QMember`, `QOrder`, `QOrderItem`)
- 기본적으로 엔티티 필드들이 그대로 Q타입 필드로 매핑됨
```java
public class QBook extends EntityPathBase<Book> {
    public static final QBook book = new QBook("book");
```

---

## 어떻게 사용하는데?
```java
public class BookRepository {
    public List<Book> findBookList() {
        QBook book = QBook.book;
        return queryFactory
                .select(book)
                .from(book)
                .fetch();
    }
}
```