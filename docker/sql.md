# SQL


## 초기 설정
### 1. data 디렉토리 삭제
```bash
  ZARD  ~  sudo rm -rf /Users/macbook/data/dbms/postgres/data 
  
  ZARD  ~  cd ~/Desktop/project/local-market
  ZARD  ~/Desktop/project/local-market   local ✚ ● ?  docker compose up -d
```
## 01-init-database.sql 파일
```sql
-- =====================================================
-- Local Market 데이터베이스 및 계정 초기화
-- =====================================================

--  시간대 설정
SET timezone = 'Asia/Seoul';

--  데이터베이스 연결
\c local_market;

-- 사용자 권한 부여
GRANT ALL PRIVILEGES ON DATABASE local_market TO "ChaNuks";
GRANT ALL ON SCHEMA public TO "ChaNuks";
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO "ChaNuks";
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO "ChaNuks";
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO "ChaNuks";
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON SEQUENCES TO "ChaNuks";
``

---

## 02-create-tables.sql 파일
```sql
-- =====================================================
-- Local Market 프로젝트 데이터베이스 스키마
-- =====================================================

DROP TABLE IF EXISTS "order_details" CASCADE;
DROP TABLE IF EXISTS "cart_products" CASCADE;
DROP TABLE IF EXISTS "order_products" CASCADE;
DROP TABLE IF EXISTS "reviews" CASCADE;
DROP TABLE IF EXISTS "carts" CASCADE;
DROP TABLE IF EXISTS "orders" CASCADE;
DROP TABLE IF EXISTS "products" CASCADE;
DROP TABLE IF EXISTS "categories" CASCADE;
DROP TABLE IF EXISTS "members" CASCADE;

CREATE TABLE "members" (
    "member_no"       BIGSERIAL    PRIMARY KEY,
    "role"            VARCHAR(20)  NOT NULL,
    "membership_tier" VARCHAR(200) NOT NULL,
    "username"        VARCHAR(300) NOT NULL,
    "email"           VARCHAR(254) NOT NULL,
    "password"        VARCHAR(255) NOT NULL,
    "name"            VARCHAR(100) NOT NULL,
    "phone"           VARCHAR(20)  NOT NULL,
    "addr1"           VARCHAR(255) NOT NULL,
    "addr2"           VARCHAR(255) NOT NULL,
    "created_at"      TIMESTAMPTZ  NOT NULL,
    "created_by"      VARCHAR(36)  NOT NULL,
    "modified_at"     TIMESTAMPTZ  NULL,
    "modified_by"     VARCHAR(36)  NULL
);

CREATE TABLE "categories" (
    "category_no"         BIGSERIAL    PRIMARY KEY,
    "category_name"       VARCHAR(100) NOT NULL,
    "category_code"       VARCHAR(20)  NOT NULL UNIQUE,
    "sort_order"          INTEGER      NULL,
    "parent_category_no"  BIGINT       NULL,
    "level"               INTEGER      NOT NULL DEFAULT 1,
    "status"              VARCHAR(20)  NOT NULL DEFAULT 'ACTIVE',
    "created_by"          VARCHAR(25)  NOT NULL,
    "created_at"          TIMESTAMPTZ  NOT NULL,
    "modified_by"         VARCHAR(25)  NULL,
    "modified_at"         TIMESTAMPTZ  NULL,
    CONSTRAINT "FK_categories_parent" FOREIGN KEY ("parent_category_no") 
        REFERENCES "categories"("category_no")
);

CREATE TABLE "products" (
    "product_no"      BIGSERIAL     PRIMARY KEY,
    "name"            VARCHAR(200)  NOT NULL,
    "original_price"  NUMERIC(15,5) NOT NULL,
    "sale_price"      NUMERIC(15,5) NOT NULL,
    "qty"             NUMERIC(10)   NOT NULL,
    "description"     TEXT          NOT NULL,
    "category_no"     BIGSERIAL     NOT NULL,
    "created_by"      VARCHAR(25)   NOT NULL,
    "created_at"      TIMESTAMPTZ   NOT NULL,
    "modified_by"     VARCHAR(25)   NULL,
    "modified_at"     TIMESTAMPTZ   NULL,
    CONSTRAINT "FK_products_category" FOREIGN KEY ("category_no") 
        REFERENCES "categories"("category_no")
);

CREATE TABLE "carts" (
    "cart_no"         BIGSERIAL    PRIMARY KEY,
    "member_no"       BIGSERIAL    NOT NULL,
    "created_by"      VARCHAR(25)  NOT NULL,
    "created_at"      TIMESTAMPTZ  NOT NULL,
    "modified_by"     VARCHAR(25)  NULL,
    "modified_at"     TIMESTAMPTZ  NULL,
    CONSTRAINT "FK_carts_member" FOREIGN KEY ("member_no") 
        REFERENCES "members"("member_no")
);

CREATE TABLE "orders" (
    "order_no"        BIGSERIAL    PRIMARY KEY,
    "member_no"       BIGSERIAL    NOT NULL,
    "order_status"    VARCHAR(30)  NOT NULL,
    "date"            TIMESTAMPTZ  NOT NULL,
    "total_price"     INTEGER      NOT NULL,
    "created_by"      VARCHAR(25)  NOT NULL,
    "created_at"      TIMESTAMPTZ  NOT NULL,
    "modified_by"     VARCHAR(25)  NULL,
    "modified_at"     TIMESTAMPTZ  NULL,
    CONSTRAINT "FK_orders_member" FOREIGN KEY ("member_no") 
        REFERENCES "members"("member_no")
);

CREATE TABLE "order_details" (
    "order_detail_no" BIGSERIAL    PRIMARY KEY,
    "order_no"        BIGSERIAL    NOT NULL,
    CONSTRAINT "FK_order_details_order" FOREIGN KEY ("order_no") 
        REFERENCES "orders"("order_no")
);

CREATE TABLE "cart_products" (
    "cart_no"         BIGSERIAL    NOT NULL,
    "product_no"      BIGSERIAL    NOT NULL,
    PRIMARY KEY ("cart_no", "product_no"),
    CONSTRAINT "FK_cart_products_cart" FOREIGN KEY ("cart_no") 
        REFERENCES "carts"("cart_no"),
    CONSTRAINT "FK_cart_products_product" FOREIGN KEY ("product_no") 
        REFERENCES "products"("product_no")
);

CREATE TABLE "order_products" (
    "order_product_no" BIGSERIAL    PRIMARY KEY,
    "order_no"         BIGSERIAL    NOT NULL,
    "product_no"       BIGSERIAL    NOT NULL,
    "created_by"       VARCHAR(25)  NOT NULL,
    "created_at"       TIMESTAMPTZ  NOT NULL,
    "modified_by"      VARCHAR(25)  NULL,
    "modified_at"      TIMESTAMPTZ  NULL,
    CONSTRAINT "FK_order_products_order" FOREIGN KEY ("order_no") 
        REFERENCES "orders"("order_no"),
    CONSTRAINT "FK_order_products_product" FOREIGN KEY ("product_no") 
        REFERENCES "products"("product_no")
);

CREATE TABLE "reviews" (
    "review_no"       BIGSERIAL    PRIMARY KEY,
    "product_no"      BIGSERIAL    NOT NULL,
    "member_no"       BIGSERIAL    NOT NULL,
    "rating"          INTEGER      NULL,
    "title"           VARCHAR(200) NOT NULL,
    "content"         TEXT         NOT NULL,
    "review_image"    TEXT         NULL,
    "review_status"   VARCHAR(20)  NOT NULL,
    "created_by"      VARCHAR(25)  NOT NULL,
    "created_at"      TIMESTAMPTZ  NOT NULL,
    "modified_by"     VARCHAR(25)  NULL,
    "modified_at"     TIMESTAMPTZ  NULL,
    CONSTRAINT "FK_reviews_product" FOREIGN KEY ("product_no") 
        REFERENCES "products"("product_no"),
    CONSTRAINT "FK_reviews_member" FOREIGN KEY ("member_no") 
        REFERENCES "members"("member_no")
);

CREATE INDEX "IDX_MEMBERS_EMAIL" ON "members" ("email");
CREATE INDEX "IDX_MEMBERS_USERNAME" ON "members" ("username");
CREATE INDEX "IDX_MEMBERS_ROLE" ON "members" ("role");
CREATE INDEX "IDX_CATEGORIES_CODE" ON "categories" ("category_code");
CREATE INDEX "IDX_CATEGORIES_NAME" ON "categories" ("category_name");
CREATE INDEX "IDX_CATEGORIES_PARENT" ON "categories" ("parent_category_no");
CREATE INDEX "IDX_CATEGORIES_LEVEL" ON "categories" ("level");
CREATE INDEX "IDX_CATEGORIES_STATUS" ON "categories" ("status");
CREATE INDEX "IDX_CATEGORIES_SORT" ON "categories" ("sort_order");
CREATE INDEX "IDX_PRODUCTS_NAME" ON "products" ("name");
CREATE INDEX "IDX_PRODUCTS_CATEGORY" ON "products" ("category_no");
CREATE INDEX "IDX_PRODUCTS_PRICE" ON "products" ("sale_price");
CREATE INDEX "IDX_ORDERS_MEMBER" ON "orders" ("member_no");
CREATE INDEX "IDX_ORDERS_STATUS" ON "orders" ("order_status");
CREATE INDEX "IDX_ORDERS_DATE" ON "orders" ("date");
CREATE INDEX "IDX_REVIEWS_PRODUCT" ON "reviews" ("product_no");
CREATE INDEX "IDX_REVIEWS_MEMBER" ON "reviews" ("member_no");
CREATE INDEX "IDX_REVIEWS_RATING" ON "reviews" ("rating");
```

---

## 03-init-data.sql 파일
```sql
-- =====================================================
-- Local Market 카테고리 샘플 데이터
-- =====================================================

-- 대분류 카테고리 (Level 1)
INSERT INTO categories (category_name, category_code, sort_order, parent_category_no, level, status, created_by, created_at) VALUES
('맛있는것', 'FOOD', 1, NULL, 1, 'ACTIVE', 'CP300686', NOW());

-- 중분류 카테고리 (Level 2) - 맛있는것 하위 (일부만)
INSERT INTO categories (category_name, category_code, sort_order, parent_category_no, level, status, created_by, created_at) VALUES
('밥·도시락', 'FOOD_001', 1, 1, 2, 'ACTIVE', 'CP300686', NOW()),
('라면·면', 'FOOD_002', 2, 1, 2, 'ACTIVE', 'CP300686', NOW()),
('음료·커피·생수', 'FOOD_009', 9, 1, 2, 'ACTIVE', 'CP300686', NOW()),
('과자·초콜릿·시리얼', 'FOOD_010', 10, 1, 2, 'ACTIVE', 'CP300686', NOW());

-- 소분류 카테고리 (Level 3) - 밥·도시락 하위 (일부만)
INSERT INTO categories (category_name, category_code, sort_order, parent_category_no, level, status, created_by, created_at) VALUES
('도시락', 'FOOD_101', 1, 2, 3, 'ACTIVE', 'CP300686', NOW()),
('김밥', 'FOOD_102', 2, 2, 3, 'ACTIVE', 'CP300686', NOW());

-- 소분류 카테고리 (Level 3) - 라면·면 하위 (일부만)
INSERT INTO categories (category_name, category_code, sort_order, parent_category_no, level, status, created_by, created_at) VALUES
('라면', 'FOOD_201', 1, 3, 3, 'ACTIVE', 'CP300686', NOW()),
('파스타', 'FOOD_202', 2, 3, 3, 'ACTIVE', 'CP300686', NOW());
```
