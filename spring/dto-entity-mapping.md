# DTO ↔ Entity 매핑 전략 정리

## DTO <-> Entity 분리
- `DTO`와 `Entity`는 서로 다른 목적을 가진 객체로, 분리하여 관리하는 것이 좋음
- `DTO`는 주로 데이터 `전송을 위한 객체`로, API 요청/응답에 사용되며, Entity는 `데이터베이스와의 상호작용을 위한 객체`로, JPA 등의 ORM 프레임워크에서 사용됨


- `DTO`와 `Entity`를 분리함으로써, 다음과 같은 장점을 얻을 수 있음
  1. **유지보수성 향상**: DTO와 Entity의 변경이 서로 영향을 주지 않음
  2. **테스트 용이성**: DTO와 Entity를 독립적으로 테스트할 수 있음
  3. **보안 강화**: DTO를 통해 필요한 데이터만 전송하여 보안성을 높일 수 있음
  4. **API 버전 관리 용이**: DTO를 사용하여 API 버전을 관리할 수 있음
---
## DTO(Entity) 매핑 전략
- DTO와 Entity 간의 매핑 전략은 애플리케이션의 구조와 요구사항에 따라 다를 수 있음
- 일반적으로 다음과 같은 전략을 사용함
  - `DTO`는 데이터 전송 객체로, 주로 `API 요청/응답`에 사용됨
  - `Entity`는 데이터베이스와의 상호작용을 위한 객체로, `JPA 등의 ORM 프레임워크`에서 사용됨
---
## 매핑 전략(BeanUtils, ModelMapper, MapStruct 등)
### 1. BeanUtils
- **설명**: Spring Framework에서 제공하는 유틸리티 클래스로, 객체 간의 프로퍼티를 복사하는 기능을 제공
- **장점**:
  - 간단하고 사용하기 쉬움
  - Spring Framework에 내장되어 있어 추가 라이브러리 필요 없음
  - 기본적인 매핑 기능을 제공
- **단점**:
  - 성능이 느릴 수 있음 (리플렉션 사용)
  - 복잡한 매핑이나 커스텀 매핑에 대한 지원이 부족함
- **사용 예시**:
```java
    import org.springframework.beans.BeanUtils;
    
    public class UserMapper {
        public static UserDTO toDTO(User user) {
            UserDTO userDTO = new UserDTO();
            BeanUtils.copyProperties(user, userDTO);
            return userDTO;
        }
    
        public static User toEntity(UserDTO userDTO) {
            User user = new User();
            BeanUtils.copyProperties(userDTO, user);
            return user;
        }
    }
```


### 2. ModelMapper
- **설명**: 객체 간의 매핑을 자동으로 처리해주는 라이브러리로, 복잡한 매핑을 지원
- **장점**:
  - 복잡한 매핑을 쉽게 처리할 수 있음
  - 커스텀 매핑 규칙을 정의할 수 있음
  - 성능이 BeanUtils보다 우수함
- **단점**:
  - 외부 라이브러리를 추가해야 함
  - 설정이 복잡할 수 있음
  - 리플렉션을 사용하므로 성능이 느릴 수 있음
- **사용 예시**:
```java
    import org.modelmapper.ModelMapper;
    
    public class UserMapper {
        private static final ModelMapper modelMapper = new ModelMapper();
        
        public static UserDTO toDTO(User user) {
            return modelMapper.map(user, UserDTO.class);
        }
    
        public static User toEntity(UserDTO userDTO) {
            return modelMapper.map(userDTO, User.class);
        }
    }
```


### 3. MapStruct
- **설명**: 컴파일 타임에 매핑 코드를 생성해주는 라이브러리로, 성능이 우수함
- **장점**:
  - 컴파일 타임에 매핑 코드를 생성하므로 성능이 우수함
  - 매핑 규칙을 명시적으로 정의할 수 있어 가독성이 좋음
  - 복잡한 매핑도 쉽게 처리할 수 있음
- **단점**:
  - 외부 라이브러리를 추가해야 함
  - 설정이 필요함 (애너테이션 기반)
  - 컴파일 타임에 매핑 코드를 생성하므로, 빌드 시간이 늘어날 수 있음
- **사용 예시**:
```java
    import org.mapstruct.Mapper;
    import org.mapstruct.factory.Mappers;
    
    @Mapper
    public interface UserMapper {
        UserMapper INSTANCE = Mappers.getMapper(UserMapper.class);
        
        UserDTO toDTO(User user);
        
        User toEntity(UserDTO userDTO);
    }
```
---
## 어떻게 사용해야 할까?
- **BeanUtils**: 간단한 매핑이 필요할 때 사용. 성능이 크게 중요하지 않은 경우에 적합
- **ModelMapper**: 복잡한 매핑이 필요하거나, 커스텀 매핑 규칙을 정의해야 할 때 사용. 성능이 중요한 경우에도 적합
- **MapStruct**: 성능이 중요한 경우에 사용. 컴파일 타임에 매핑 코드를 생성하므로, 런타임 성능이 우수함. 복잡한 매핑을 명시적으로 정의할 수 있어 가독성이 좋음


- **선택 기준**:
  - **성능**: `MapStruct` > `ModelMapper` > `BeanUtils`
  - **복잡성**: `BeanUtils` < `ModelMapper` < `MapStruct`
  - **유지보수성**: `MapStruct (명시적 매핑 규칙)` > `ModelMapper` > `BeanUtils`