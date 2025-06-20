# Java Stream API
Java Stream API는 데이터를 선언형(무엇을 할 것인가)으로 처리할 수 있도록 지원하는 기능으로, 함수형 프로그래밍을 Java에 도입한 대표적인 예입니다.

---

## 1. 📌 Stream이란?
- 컬렉션, 배열 등의 데이터를 **순차적/병렬적**으로 처리할 수 있도록 지원하는 API
- **내부 반복**을 통해 코드 간결화와 병렬 처리 가능

```java
List<String> names = List.of("Alice", "Bob", "Charlie");
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println);
```
---
## 2. 📌 Stream의 생성 방법
```java
// 리스트 기반
List<String> list = List.of("a", "b", "c");
Stream<String> stream1 = list.stream();

// 배열 기반
String[] arr = {"a", "b", "c"};
Stream<String> stream2 = Arrays.stream(arr);

// Stream.of
Stream<Integer> stream3 = Stream.of(1, 2, 3);

// 숫자 범위
IntStream.range(1, 5); // 1~4
IntStream.rangeClosed(1, 5); // 1~5
```
---
## 3. 📌 Stream의 주요 연산
### 3-1. 중간 연산 (Intermediate Operations)
- **지연(lazy)**: 최종 연산이 호출될 때까지 실행되지 않음
- **체이닝 가능**: 여러 중간 연산을 연결하여 사용할 수 있음
- 예시:
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
Stream<String> filteredNames = names.stream()
    .filter(name -> name.startsWith("A")) // 필터링
    .map(String::toUpperCase); // 대문자로 변환
```
### 3-2. 최종 연산 (Terminal Operations)
- **스트림을 소비**하여 결과를 생성
- 스트림을 닫고, 더 이상 사용할 수 없음
- 예시:
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println); // 출력
```
### 3-3. 주요 중간 연산 메서드
| 메서드          | 설명                                       |
|-----------------|------------------------------------------|
| `filter`        | 조건에 맞는 요소만 필터링                |
| `map`           | 각 요소를 변환                           |
| `flatMap`       | 중첩된 스트림을 평탄화                   |
| `distinct`      | 중복 요소 제거                           |
| `sorted`        | 요소를 정렬                              |
| `limit`         | 스트림의 크기를 제한                      |
| `skip`          | 처음 N개의 요소를 건너뛰기               |
### 3-4. 주요 최종 연산 메서드
| 메서드          | 설명                                       |
|-----------------|------------------------------------------|
| `forEach`       | 각 요소에 대해 작업 수행                  |
| `collect`       | 스트림의 요소를 컬렉션으로 수집           |
| `reduce`        | 스트림의 요소를 하나로 결합                |
| `count`         | 스트림의 요소 개수 반환                   |
### 3-5. 예시 코드
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
List<String> filteredNames = names.stream()
    .filter(name -> name.startsWith("A")) // "A"로 시작하는 이름 필터링
    .map(String::toUpperCase) // 대문자로 변환
    .collect(Collectors.toList()); // 리스트로 수집
System.out.println(filteredNames); // [ALICE]
```
---
## 4. 📌 병렬 스트림
- `parallelStream()` 메서드를 사용하여 병렬 처리 가능
- 병렬 스트림은 멀티코어 CPU를 활용하여 성능을 향상시킬 수 있음
- 예시:
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
names.parallelStream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println); // 병렬로 출력
```
---
## 5. 📌 Stream vs for-each vs iterator
| 특징            | Stream                          | for-each                        | Iterator                        |
|-----------------|----------------------------------|----------------------------------|----------------------------------|
| 선언형/명령형   | 선언형 (무엇을 할 것인가)         | 명령형 (어떻게 할 것인가)         | 명령형 (어떻게 할 것인가)         |
| 코드 간결성      | 간결하고 읽기 쉬움                | 반복문 사용으로 복잡해질 수 있음   | 반복문 사용으로 복잡해질 수 있음   |
| 병렬 처리 가능   | 가능 (parallelStream)            | 불가능                           | 불가능                           |
| 성능            | 최적화된 내부 반복 사용            | 성능 저하 가능성 있음              | 성능 저하 가능성 있음              |
| 상태 유지       | 상태를 유지하지 않음               | 상태를 유지함                     | 상태를 유지함                     |
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
// Stream
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println);
// for-each
for (String name : names) {
    if (name.startsWith("A")) {
        System.out.println(name);
    }
}
// Iterator
Iterator<String> iterator = names.iterator();
while (iterator.hasNext()) {
    String name = iterator.next();
    if (name.startsWith("A")) {
        System.out.println(name);
    }
}
```
---
## 6. 📌 Stream API 사용 시 주의사항
- **스트림은 한 번만 사용할 수 있음**: 스트림은 최종 연산 후 닫히므로, 재사용 불가
- **병렬 스트림 사용 시 주의**: 병렬 스트림은 멀티스레드 환경에서 동기화 문제를 일으킬 수 있으므로, 상태를 유지하는 객체를 사용할 때 주의해야 함
- **성능 고려**: 작은 데이터셋에서는 스트림 사용이 오히려 성능 저하를 일으킬 수 있음
- **메모리 관리**: 스트림을 사용한 후에는 자원을 해제하기 위해 `close()` 메서드를 호출하거나, try-with-resources 구문을 사용하는 것이 좋음
