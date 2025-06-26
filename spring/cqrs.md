# CQRS (Command Query Responsibility Segregation)

## 정의
- 명령(Command)과 조회(Query)를 **책임을 분리**해서 처리하는 설계 패턴

> 하나의 모델로 읽기/쓰기 둘 다 처리하지 않고, 읽기(조회)는 Query 모델,  
> 쓰기(명령)는 Command 모델이 따로 존재

--- 
## 특징
- **명령과 조회의 분리**: 명령은 상태를 변경하고, 조회는 상태를 읽음
- **모델의 분리**: 명령 모델과 조회 모델이 별도로 존재
- **스케일링**: 읽기와 쓰기를 독립적으로 스케일링 가능
- **복잡한 도메인 처리**: 복잡한 도메인 로직을 명령 모델에서 처리하고, 단순한 조회는 조회 모델에서 처리
- **이벤트 소싱과의 연계**: 명령 처리 후 이벤트를 발생시켜 상태 변경을 기록하고, 조회 모델은 이 이벤트를 기반으로 업데이트

---

## 장점
| 장점 | 설명 |
| --- | --- |
| **성능 최적화** | 읽기와 쓰기를 분리하여 각각 최적화 가능 |
| **유연성** | 명령과 조회 모델을 독립적으로 변경 가능 |
| **복잡성 관리** | 복잡한 도메인 로직을 명령 모델에서 처리하여 단순화 |
| **스케일링 용이** | 읽기와 쓰기를 독립적으로 스케일링 가능 |
| **이벤트 소싱과 통합** | 이벤트 소싱과 함께 사용하여 상태 변경 이력 관리 가능 |
| **테스트 용이성** | 명령과 조회 모델을 독립적으로 테스트 가능 |
| **보안 강화** | 명령과 조회 모델을 분리하여 보안 강화 가능 |
| **API 최적화** | API를 명령과 조회로 분리하여 최적화 가능 |

## 단점
| 단점 | 설명 |
| --- | --- |
| **복잡성 증가** | 명령과 조회 모델을 분리하여 설계와 구현이 복잡해질 수 있음 |
| **데이터 일관성 문제** | 명령과 조회 모델 간의 데이터 일관성을 유지하기 어려울 수 있음 |
| **추가적인 인프라 필요** | 이벤트 소싱과 같은 추가적인 인프라가 필요할 수 있음 |
| **개발 비용 증가** | 명령과 조회 모델을 분리하여 개발 비용이 증가할 수 있음 |
| **학습 곡선** | CQRS 패턴을 이해하고 적용하는 데 시간이 필요할 수 있음 |

---

## 사용 예시
- **이커머스 시스템**: 주문 처리(명령)와 상품 조회(조회)를 분리하여 성능 최적화
- **소셜 미디어 플랫폼**: 사용자 게시물 작성(명령)과 피드 조회(조회)를 분리하여 확장성 향상
- **금융 시스템**: 거래 처리(명령)와 계좌 조회(조회)를 분리하여 보안 강화 및 성능 최적화
- **게임 서버**: 플레이어 행동(명령)과 게임 상태 조회(조회)를 분리하여 실시간 처리 성능 향상
- **IoT 시스템**: 센서 데이터 수집(명령)과 상태 모니터링(조회)을 분리하여 데이터 처리 성능 향상
- **헬스케어 시스템**: 환자 기록 업데이트(명령)와 환자 정보 조회(조회)를 분리하여 데이터 일관성 유지
- **교육 플랫폼**: 강의 등록(명령)과 강의 목록 조회(조회)를 분리하여 사용자 경험 향상
- **물류 관리 시스템**: 배송 요청(명령)과 배송 상태 조회(조회)를 분리하여 운영 효율성 향상
- **고객 지원 시스템**: 티켓 생성(명령)과 티켓 조회(조회)를 분리하여 고객 서비스 향상
---

## 예시 코드
```java
public class CommandQueryExample {

    // Command 모델
    public void createOrder(Order order) {
        // 주문 생성 로직
        System.out.println("Order created: " + order);
    }

    // Query 모델
    public Order getOrderById(String orderId) {
        // 주문 조회 로직
        return new Order(orderId, "Sample Item", 1);
    }

    public static void main(String[] args) {
        CommandQueryExample example = new CommandQueryExample();
        
        // 명령 처리
        example.createOrder(new Order("123", "Item A", 2));
        
        // 조회 처리
        Order order = example.getOrderById("123");
        System.out.println("Retrieved Order: " + order);
    }
}
class Order {
    private String id;
    private String item;
    private int quantity;

    public Order(String id, String item, int quantity) {
        this.id = id;
        this.item = item;
        this.quantity = quantity;
    }

    @Override
    public String toString() {
        return "Order{id='" + id + "', item='" + item + "', quantity=" + quantity + '}';
    }
}
```
