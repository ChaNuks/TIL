# 🗂️ Java Collection Framework 정리

자바 컬렉션 프레임워크는 데이터를 효율적으로 저장하고 처리하기 위한 **자료구조 클래스들의 집합**

---

## ✅ 주요 인터페이스 계층 구조

- `Collection` 인터페이스
    - `List`
        - `ArrayList`
        - `LinkedList`
        - `Vector`
    - `Set`
        - `HashSet`
        - `LinkedHashSet`
        - `TreeSet`
    - `Queue`
        - `PriorityQueue`
        - `ArrayDeque`
        - `LinkedList`
- `Map` 인터페이스 *(Collection과는 별도 계층)*
    - `HashMap`
    - `LinkedHashMap`
    - `TreeMap`
    - `Hashtable`

---

## 🔹 List

| 구현체         | 특징                                |
|----------------|-------------------------------------|
| `ArrayList`    | 배열 기반, 조회에 강함              |
| `LinkedList`   | 노드 연결 기반, 삽입/삭제에 유리    |
| `Vector`       | `ArrayList`와 비슷, 동기화 지원     |

- **순서 있음 (index 접근 가능)**
- **중복 허용**

---

## 🔹 Set

| 구현체           | 특징                                           |
|------------------|------------------------------------------------|
| `HashSet`        | 순서 없음, 중복 제거                          |
| `LinkedHashSet`  | 입력한 순서 유지                              |
| `TreeSet`        | 정렬된 순서 유지 (이진 탐색 트리 기반)        |

- **순서 없음 (단, `LinkedHashSet`은 유지)**
- **중복 불가**

---

## 🔹 Queue & Deque

| 구현체        | 특징                           |
|---------------|--------------------------------|
| `PriorityQueue` | 우선순위에 따라 요소 처리       |
| `ArrayDeque`  | 배열 기반 양방향 큐            |
| `LinkedList`  | 리스트이자 큐/덱 모두 사용 가능 |

- **Queue는 FIFO(선입선출)**
- **Deque는 양방향 입출력 가능**

---

## 🔹 Map

| 구현체           | 특징                                    |
|------------------|-----------------------------------------|
| `HashMap`        | 순서 없음, 빠른 검색                     |
| `LinkedHashMap`  | 입력 순서 유지                          |
| `TreeMap`        | 키 기준 자동 정렬                        |
| `Hashtable`      | 동기화 지원, `HashMap`보다 느림          |

- **Key 중복 불가 / Value 중복 허용**
- **Collection 인터페이스와는 별개**

---

## 🔧 핵심 차이 요약

| 분류   | 순서 보장 | 중복 허용 | 대표 구현체               |
|--------|------------|-------------|----------------------------|
| List   | ✅ O        | ✅ O         | ArrayList, LinkedList      |
| Set    | ❌ X        | ❌ X         | HashSet, TreeSet           |
| Queue  | ✅ O (FIFO) | ✅ O         | LinkedList, ArrayDeque     |
| Map    | ❌ X (Key)  | ✅ O (Value) | HashMap, TreeMap, HashTable|

---