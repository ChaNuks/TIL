# 계층형 데이터 모델링 (Tree Structure)

## 계층형 데이터 모델링
- 데이터가 부모-자식 관계로 구성된 구조를 나타냄
- 예: 조직도, 카테고리 트리 등
- 트리 구조를 사용하여 데이터의 계층적 관계를 표현
- 각 노드는 부모 노드와 자식 노드를 가질 수 있음
- 루트 노드에서 시작하여 하위 노드로 내려가는 방식으로 탐색 가능

--- 

## 트리 구조의 특징
- `부모-자식 관계`: 각 노드는 하나의 부모 노드와 여러 자식 노드를 가질 수 있음
- `루트 노드`: 트리의 최상위 노드로, 부모가 없는 노드
- `리프 노드`: 자식 노드가 없는 노드
- `노드`: 트리의 각 요소로, 데이터와 연결된 객체
- `서브트리`: 특정 노드와 그 자식 노드들로 구성된 트리의 부분

---

## 트리 구조의 장점
- `계층적 데이터 표현`: 데이터의 계층적 관계를 명확하게 표현할 수 있음
- `효율적인 탐색`: 트리 구조를 사용하여 데이터 검색 및 탐색이 용이함
- `구조적 데이터`: 데이터의 구조가 명확하여 이해하기 쉬움
- `유연성`: 새로운 노드를 추가하거나 삭제하는 것이 용이함
- `재사용성`: 공통된 부모 노드를 가진 자식 노드들을 그룹화하여 재사용 가능


## 트리 구조의 단점
- `복잡성`: 트리 구조가 깊어질수록 탐색 및 관리가 복잡해질 수 있음
- `성능`: 트리의 깊이가 깊어지면 탐색 성능이 저하될 수 있음
- `순환 참조`: 트리 구조에서 순환 참조가 발생할 수 있어 데이터 무결성에 영향을 줄 수 있음
- `데이터 중복`: 동일한 데이터를 여러 노드에서 참조할 경우 데이터 중복이 발생할 수 있음
- `제한된 관계`: 부모-자식 관계로만 표현되므로 복잡한 관계를 표현하기 어려움

---

## 트리 구조의 예시
### 조직도 예시
| 예시                 | 설명 |
|----------------------|------|
| 상품 카테고리        | ex. 대분류 → 라면, 음료 → 신라면, 진라면 등 |
| 지역/행정구역        | 서울 → 동대문구 → 이문동 |
| 메뉴 트리            | 대메뉴 → 서브 메뉴 |
| 조직도              | CEO → 부서장 → 팀원 |
| 댓글 트리 구조       | 댓글 → 대댓글 |
| 폴더/파일 구조       | 폴더 → 하위폴더 → 파일 |

---

## 트리 구조의 구현
### 데이터베이스에서의 트리 구조 구현
- **Adjacency List**
  - 각 노드가 부모 노드의 ID를 참조하는 방식
  - 예시: `id`, `parent_id` 컬럼을 가진 테이블
  - ```sql
    CREATE TABLE category (
        id INT PRIMARY KEY,
        name VARCHAR(100),
        parent_id INT,
        FOREIGN KEY (parent_id) REFERENCES category(id)
    );
    ```
    

- **Nested Set**
  - 각 노드에 왼쪽과 오른쪽 값을 할당하여 트리 구조를 표현
  - 예시: `id`, `left`, `right` 컬럼을 가진 테이블
  - ```sql
    CREATE TABLE category (
        id INT PRIMARY KEY,
        name VARCHAR(100),
        left INT,
        right INT
    );
    ```
    
- **Closure Table**
  - 모든 노드 간의 관계를 저장하는 별도의 테이블을 사용
  - 예시: `ancestor`, `descendant`, `depth` 컬럼을 가진 테이블
  - ```sql
    CREATE TABLE category_closure (
        ancestor INT,
        descendant INT,
        depth INT,
        PRIMARY KEY (ancestor, descendant),
        FOREIGN KEY (ancestor) REFERENCES category(id),
        FOREIGN KEY (descendant) REFERENCES category(id)
    );
    ```
    
---

## 트리 구조의 탐색
### 깊이 우선 탐색 (DFS)
- 트리의 각 노드를 깊이 우선으로 탐색
- 재귀적으로 구현 가능
- 예시:
```java
public void depthFirstSearch(Node node) {
    if (node == null) return;
    System.out.println(node.data); // 현재 노드 처리
    for (Node child : node.children) {
        depthFirstSearch(child); // 자식 노드 탐색
    }
}
```

### 너비 우선 탐색 (BFS)
- 트리의 각 노드를 너비 우선으로 탐색
- 큐를 사용하여 구현
- 예시:
```java
public void breadthFirstSearch(Node root) {
    Queue<Node> queue = new LinkedList<>();
    queue.add(root);
    
    while (!queue.isEmpty()) {
        Node node = queue.poll();
        System.out.println(node.data); // 현재 노드 처리
        for (Node child : node.children) {
            queue.add(child); // 자식 노드 큐에 추가
        }
    }
}
```

---

## 트리 구조의 활용
- **조직도**: 기업의 조직 구조를 표현
- **카테고리 트리**: 상품 카테고리를 계층적으로 표현
- **파일 시스템**: 폴더와 파일의 계층 구조를 표현
- **댓글 시스템**: 댓글과 대댓글의 계층 구조를 표현
- **게임 맵**: 게임 내 지역과 하위 지역의 계층 구조를 표현
