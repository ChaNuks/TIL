# 🔨 생성자(Constructor)란?

## ✅ 생성자 정의
- 클래스가 **객체로 생성될 때 호출되는 특수한 메서드**
- **객체의 초기 상태를 설정**하기 위해 사용
- 클래스 이름과 동일하고 **반환 타입이 없음**

---

## 🧱 생성자의 특징

| 항목       | 설명                                                       |
|------------|------------------------------------------------------------|
| 이름       | 클래스 이름과 동일해야 함                                   |
| 반환 타입 | 없음 (`void`도 쓰지 않음)                                   |
| 호출 시점 | 객체 생성 시 자동 호출                                     |
| 오버로딩   | 매개변수 개수/타입이 다르면 여러 개 정의 가능             |

---

## 🧪 기본 생성자(Default Constructor)

- 매개변수가 없는 생성자
- 생성자를 명시적으로 작성하지 않으면 **컴파일러가 자동으로 생성**

```java
public class User {
    public User() {
        System.out.println("기본 생성자 호출");
    }
}
```
---

## 🧪 매개변수 생성자(Parameterized Constructor)

- 객체 생성 시 필요한 값을 외부에서 받기 위해 사용
```java
public class User {
    private String name;

    public User(String name) {
        this.name = name;
    }
}
```
---

## 🧪 생성자 오버로딩(Overloading)
- 같은 이름의 생성자를 매개변수의 개수나 타입만 다르게 여러 개 정의 가능

```java
public class Product {
    private String name;
    private int price;

    public Product() {} // 기본 생성자

    public Product(String name) {
        this.name = name;
    }

    public Product(String name, int price) {
        this.name = name;
        this.price = price;
    }
}
```
---

## 🧪 this() 키워드로 생성자 간 호출
- 생성자 내부에서 다른 생성자 호출 가능
- 반드시 생성자의 첫 줄에서 사용해야 함

```java
public class Product {
    private String name;
    private int price;

    public Product(String name) {
        this(name, 0); // 아래 생성자 호출
    }

    public Product(String name, int price) {
        this.name = name;
        this.price = price;
    }
}
```
---