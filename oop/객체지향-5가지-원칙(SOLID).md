## SOLID (객체지향 5가지 원칙)

## SOLID란?
- 객체지향 프로그래밍에서 소프트웨어 설계를 위한 5가지 원칙을 의미
- 이 원칙들은 소프트웨어의 유지보수성과 확장성을 높이는 데 도움을 줌


## 1. SRP - 단일 책임 원칙 (Single Responsibility Principle, SRP)
- 클래스는 하나의 책임만 가져야 하며, 그 책임을 완전히 캡슐화해야 함
- 즉, 클래스는 하나의 기능만을 수행해야 하며, 그 기능에 대한 변경이 있을 때 해당 클래스만 수정하면 되도록 설계해야 함
- 예시 : `User` 클래스는 사용자 정보를 관리하는 책임만 가지고, `UserService` 클래스는 사용자 저장과 이메일 전송의 책임을 가지고 있음
```java
public class User {
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }
}
public class UserService {
    public void saveUser(User user) {
        // 사용자 저장 로직
    }

    public void sendEmail(User user) {
        // 이메일 전송 로직
    }
}
```


## 2. OCP - 개방-폐쇄 원칙 (Open/Closed Principle, OCP)
- 소프트웨어 엔티티(클래스, 모듈 등)는 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 함
- 즉, 기존 코드를 수정하지 않고도 새로운 기능을 추가할 수 있어야 함
- 예시 : `Shape` 인터페이스를 구현하는 `Circle`과 `Rectangle` 클래스를 만들어, 새로운 도형을 추가할 때 기존 코드를 수정하지 않고도 확장할 수 있음"
```java
public interface Shape {
    double area();
}
public class Circle implements Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}
public class Rectangle implements Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}
```


## 3. LSP - 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)
- 자식 클래스는 부모 클래스를 대체할 수 있어야 하며, 부모 클래스의 기능을 확장하거나 변경하지 않아야 함
- 즉, 자식 클래스는 부모 클래스의 계약을 준수해야 하며, 부모 클래스의 메서드를 오버라이드할 때는 부모 클래스의 동작을 변경하지 않아야 함
- 예시 : `Bird` 클래스를 상속받은 `Sparrow`와 `Penguin` 클래스가 부모 클래스의 메서드를 오버라이드할 때, 부모 클래스의 동작을 변경하지 않도록 구현해야 함
```java
public class Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }
}
public class Sparrow extends Bird {
    @Override
    public void fly() {
        System.out.println("Sparrow is flying");
    }
}
public class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}
```


## 4. ISP - 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)
- 클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 함
- 즉, 인터페이스는 클라이언트가 필요로 하는 메서드만 포함해야 하며, 불필요한 메서드를 포함하지 않아야 함
- 예시 : `Animal` 인터페이스를 `FlyingAnimal`과 `SwimmingAnimal` 인터페이스로 분리하여, 각 인터페이스는 해당 동물이 수행할 수 있는 행동만을 포함하도록 함
```java
public interface FlyingAnimal {
    void fly();
}
public interface SwimmingAnimal {
    void swim();
}
public class Duck implements FlyingAnimal, SwimmingAnimal {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
public class Fish implements SwimmingAnimal {
    @Override
    public void swim() {
        System.out.println("Fish is swimming");
    }
}
```


## 5. DIP - 의존 역전 원칙 (Dependency Inversion Principle, DIP)
- 고수준 모듈은 저수준 모듈에 의존해서는 안 되며, 둘 다 추상화에 의존해야 함
- 즉, 구체적인 클래스에 의존하지 않고, 인터페이스나 추상 클래스에 의존해야 함
- 예시 : `UserRepository` 인터페이스를 정의하고, 이를 구현하는 `DatabaseUserRepository`와 `FileUserRepository` 클래스를 만들어, `UserService` 클래스는 `UserRepository` 인터페이스에 의존하도록 함
```java
public interface UserRepository {
    void save(User user);
}
public class DatabaseUserRepository implements UserRepository {
    @Override
    public void save(User user) {
        // 데이터베이스에 사용자 저장 로직
    }
}
public class FileUserRepository implements UserRepository {
    @Override
    public void save(User user) {
        // 파일에 사용자 저장 로직
    }
}
public class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void registerUser(User user) {
        userRepository.save(user);
    }
}
```


## 요약
- **SRP**: 클래스는 하나의 책임만 가져야 함
- **OCP**: 소프트웨어 엔티티는 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 함
- **LSP**: 자식 클래스는 부모 클래스를 대체할 수 있어야 함
- **ISP**: 클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 함
- **DIP**: 고수준 모듈은 저수준 모듈에 의존해서는 안 되며, 둘 다 추상화에 의존해야 함
- 이 원칙들을 따르면 소프트웨어의 유지보수성과 확장성을 높일 수 있으며, 코드의 가독성과 재사용성을 향상시킬 수 있음
- 이 원칙들은 객체지향 프로그래밍의 핵심 원칙으로, 소프트웨어 개발에서 널리 사용됨