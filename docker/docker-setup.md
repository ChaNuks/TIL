# 🐳 Docker 연동

## docker-compose.yml 파일
```yml
services:
  # https://hub.docker.com/r/oscarfonts/h2
  h2:
    image: oscarfonts/h2:2.2.224
    platform: linux/amd64
    container_name: 'local-market-h2-dbms'
    env_file:
      - .env
    ports:
      - "1521:1521"
      - "81:81"
    volumes:
      - ~/data/dbms/h2/data:/opt/h2-data

  postgres:
    image: postgres:15
    platform: linux/amd64
    container_name: 'local-market-postgres-dbms'
    env_file:
      - .env
    ports:
      - "5434:5432"
    volumes:
      - ~/data/dbms/postgres/data:/var/lib/postgresql/data
      - ~/data/dbms/postgres/init:/docker-entrypoint-initdb.d
```

---

## application.yml 파일
```yml
server:
  port: 9090

spring:
  application:
    name: b-market-api

  # spring.config.import=xxx
  config:
    import: optional:application-database.yaml

  profiles:
    default: local
```

---

## application-test.yaml 파일
```yml
spring:
  # spring.config.import=xxx in application.properties
  config:
    import: optional:application-test.yaml

#  profiles:
#    default: test
```
---

## application-database.yaml 파일
```yml
spring:
  profiles:
    default: local
    active: local

  # WARN: JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default.
  # Therefore, database queries may be performed during view rendering.
  # Explicitly configure spring.jpa.open-in-view to disable this warning
  jpa:
    open-in-view: false

---

spring:
  config:
    activate:
      on-profile: local

  datasource:
    url: jdbc:postgresql://localhost:5434/local_market
    driver-class-name: org.postgresql.Driver
    username: {}
    password: {}

  jpa:
    hibernate:
      ddl-auto: none
```
---

## application-database-test.yaml 파일
```yml
spring:
  datasource:
    url: jdbc:h2:tcp://localhost:1521/test:DB_CLOSE_DELAY=-1:DB_CLOSE_ON_EXIT=FALSE:MODE=PostgreSQL
    driver-class-name: org.h2.Driver
    username: sa
    password:

  h2:
    console:
      enabled: true
      # path: /h2-console

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        dialect: org.hibernate.dialect.H2Dialect
    show-sql: true
```

## PostgreSQL DB 연결
- Data Source -> PostgreSQL 선택
- Host: localhost
- Port: 5434
- Database: local_market
- User: {}
- Password: {}
- Test Connection

---

## H2 DB 연결
- Data Source -> H2 선택
- Driver: H2
- URL: jdbc:h2:tcp://localhost:1521/test
- User: sa
- Password: (비워둠)
- Test Connection

---

## H2 DBMS 접속
- 브라우저에서 `http://localhost:81` 접속
- JDBC URL: `jdbc:h2:tcp://localhost:1521/test`
- User Name:
- Password: (비워둠)
- Connect 클릭

