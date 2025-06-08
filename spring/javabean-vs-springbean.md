# Java Bean vs Spring Bean


## 1. Java Bean
- Java에서 객체를 표현하는 표준 규약
- 주로 POJO(Plain Old Java Object) 형태로 작성됨
- 특징:
  - 기본 생성자(default constructor)를 가짐
  - 프로퍼티(property)를 private으로 선언하고, public getter/setter 메서드를 제공함
  - 직렬화 가능(Serializable)해야 함
  - equals(), hashCode(), toString() 메서드를 오버라이드할 수 있음
  - 상태(state)를 가지며, 상태를 변경할 수 있는 메서드를 가짐



## 2. Spring Bean
- Spring Framework에서 관리되는 객체
- Java Bean을 기반으로 하며, Spring IoC(Inversion of Control) 컨테이너에 의해 생성되고 관리됨
- 의존성 주입(Dependency Injection)을 통해 객체 간의 관계를 설정함
- 주로 @Component, @Service, @Repository, @Controller 등의 어노테이션을 사용하여 정의됨
- 생성자 주입(Constructor Injection), 세터 주입(Setter Injection), 필드 주입(Field Injection) 등의 방식으로 의존성을 주입함
- 스코프(Scope)를 설정할 수 있어, 싱글톤(Singleton), 프로토타입(Prototype), 리퀘스트(Request), 세션(Session) 등의 생명주기를 관리할 수 있음
- AOP(Aspect-Oriented Programming)를 지원하여, 공통 관심사를 분리할 수 있음
- 트랜잭션 관리, 이벤트 처리, 메시징 등 다양한 기능을 제공함
- Spring Bean은 Java Bean의 확장 개념으로, Spring Framework의 다양한 기능을 활용할 수 있음


## 3. Java Bean vs Spring Bean 비교
| 구분       | Java Bean                          | Spring Bean                        |
|------------|------------------------------------|------------------------------------|
| 정의       | Java 객체 표현 표준 규약            | Spring Framework에서 관리되는 객체 |
| 관리 방식   | 개발자가 직접 생성 및 관리          | Spring IoC 컨테이너에 의해 생성 및 관리 |
| 의존성 주입 | 없음                               | 생성자, 세터, 필드 주입 방식 지원   |
| 스코프     | 없음                               | 싱글톤, 프로토타입, 리퀘스트, 세션 등 지원 |
| AOP 지원   | 없음                               | 지원                               |
| 기능       | 기본적인 객체 표현 기능 제공        | 트랜잭션 관리, 이벤트 처리, 메시징 등 다양한 기능 제공 |
- Java Bean은 단순한 객체 표현을 위한 규약이며, Spring Bean은 Spring Framework의 다양한 기능을 활용할 수 있는 객체로 확장된 개념임
- Spring Bean은 Java Bean을 기반으로 하여, 의존성 주입, AOP, 트랜잭션 관리 등 다양한 기능을 제공함
- Spring Bean은 Java Bean의 확장 개념으로, Spring Framework의 다양한 기능을 활용할 수 있음
- Spring Bean은 Java Bean의 규약을 따르면서, Spring Framework의 IoC 컨테이너에 의해 관리되는 객체임
- Spring Bean은 Java Bean의 규약을 따르면서, Spring Framework의 다양한 기능을 활용할 수 있는 객체로 확장된 개념임
