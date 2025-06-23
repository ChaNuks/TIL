# 제네릭 (Generic)

## 제네릭이란?
- 클래스나 메서드에서 사용할 데이터 타입을 미리 지정하지 않고, 사용하는 시점에 타입을 지정할 수 있는 기능
- 예시
```java
public class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}
```
```java
public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.setItem("Hello");
        System.out.println(stringBox.getItem()); // Hello

        Box<Integer> intBox = new Box<>();
        intBox.setItem(123);
        System.out.println(intBox.getItem()); // 123
    }
}
```
---
## 제네릭의 장점
- **타입 안정성**: 컴파일 타임에 타입 체크를 수행하여, 잘못된 타입의 객체를 사용하는 것을 방지함
- **코드 재사용성**: 다양한 데이터 타입에 대해 동일한 코드를 재사용할 수 있어, 코드 중복을 줄일 수 있음
- **가독성 향상**: 코드에서 데이터 타입을 명시적으로 표현할 수 있어, 코드의 가독성이 향상됨
- **런타임 오류 감소**: 잘못된 타입의 객체를 사용하는 경우, 컴파일 타임에 오류를 발견할 수 있어 런타임 오류를 줄일 수 있음
- **컬렉션 프레임워크와의 통합**: Java의 컬렉션 프레임워크에서 제네릭을 사용하여, 다양한 데이터 타입의 객체를 안전하게 저장하고 처리할 수 있음
- **유연성**: 제네릭을 사용하면 다양한 데이터 타입에 대해 동일한 로직을 적용할 수 있어, 코드의 유연성이 향상됨
- **타입 파라미터**: 제네릭 클래스나 메서드에서 사용할 데이터 타입을 파라미터로 지정할 수 있어, 다양한 타입에 대해 동일한 로직을 적용할 수 있음
---
## 와일드카드
- 제네릭 타입에서 특정 타입을 지정하지 않고, 다양한 타입을 허용할 때 사용
- `?` 기호를 사용하여 와일드카드를 나타냄
- 예시
```java
public class WildcardExample {
    public static void printList(List<?> list) {
        for (Object item : list) {
            System.out.println(item);
        }
    }

    public static void main(String[] args) {
        List<String> stringList = Arrays.asList("A", "B", "C");
        List<Integer> intList = Arrays.asList(1, 2, 3);

        printList(stringList); // A B C
        printList(intList); // 1 2 3
    }
}
```