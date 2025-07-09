DROP TABLE IF EXISTS "order_details" CASCADE;
DROP TABLE IF EXISTS "cart_products" CASCADE;
DROP TABLE IF EXISTS "order_products" CASCADE;
DROP TABLE IF EXISTS "reviews" CASCADE;
DROP TABLE IF EXISTS "orders" CASCADE;
DROP TABLE IF EXISTS "carts" CASCADE;
DROP TABLE IF EXISTS "products" CASCADE;
DROP TABLE IF EXISTS "categories" CASCADE;
DROP TABLE IF EXISTS "members" CASCADE;

CREATE TABLE "members" (
"member_no" BIGSERIAL PRIMARY KEY,
"role" VARCHAR(20) NOT NULL,
"membership_tier" VARCHAR(200) NOT NULL,
"username" VARCHAR(300) NOT NULL,
"email" VARCHAR(254) NOT NULL,
"password" VARCHAR(255) NOT NULL,
"name" VARCHAR(100) NOT NULL,
"phone" VARCHAR(20) NOT NULL,
"addr1" VARCHAR(255) NOT NULL,
"addr2" VARCHAR(255) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"created_by" VARCHAR(36) NOT NULL,
"modified_at" TIMESTAMPTZ NULL,
"modified_by" VARCHAR(36) NULL
);

CREATE TABLE "categories" (
"category_no" BIGSERIAL PRIMARY KEY,
"category_name" VARCHAR(100) NOT NULL,
"category_code" VARCHAR(50) NOT NULL,
"sort_order" INTEGER NULL,
"created_by" VARCHAR(25) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"modified_by" VARCHAR(25) NULL,
"modified_at" TIMESTAMPTZ NULL
);

CREATE TABLE "products" (
"product_no" BIGSERIAL PRIMARY KEY,
"name" VARCHAR(200) NOT NULL,
"original_price" NUMERIC(15, 5) NOT NULL,
"sale_price" NUMERIC(15, 5) NOT NULL,
"qty" NUMERIC(10) NOT NULL,
"description" TEXT NOT NULL,
"category_no" BIGINT NOT NULL,
"created_by" VARCHAR(25) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"modified_by" VARCHAR(25) NULL,
"modified_at" TIMESTAMPTZ NULL,
CONSTRAINT fk_category FOREIGN KEY ("category_no") REFERENCES "categories" ("category_no")
);

CREATE TABLE "carts" (
"cart_no" BIGSERIAL PRIMARY KEY,
"member_no" BIGINT NOT NULL,
"created_by" VARCHAR(25) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"modified_by" VARCHAR(25) NULL,
"modified_at" TIMESTAMPTZ NULL,
CONSTRAINT fk_member_cart FOREIGN KEY ("member_no") REFERENCES "members" ("member_no")
);

CREATE TABLE "cart_products" (
"cart_no" BIGINT NOT NULL,
"product_no" BIGINT NOT NULL,
PRIMARY KEY ("cart_no", "product_no"),
CONSTRAINT fk_cart FOREIGN KEY ("cart_no") REFERENCES "carts" ("cart_no"),
CONSTRAINT fk_product FOREIGN KEY ("product_no") REFERENCES "products" ("product_no")
);

CREATE TABLE "orders" (
"order_no" BIGSERIAL PRIMARY KEY,
"member_no" BIGINT NOT NULL,
"order_status" VARCHAR(30) NOT NULL,
"date" TIMESTAMPTZ NOT NULL,
"total_price" INTEGER NOT NULL,
"created_by" VARCHAR(25) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"modified_by" VARCHAR(25) NULL,
"modified_at" TIMESTAMPTZ NULL,
CONSTRAINT fk_member_order FOREIGN KEY ("member_no") REFERENCES "members" ("member_no")
);

CREATE TABLE "order_products" (
"order_product_no" BIGSERIAL PRIMARY KEY,
"order_no" BIGINT NOT NULL,
"product_no" BIGINT NOT NULL,
"created_by" VARCHAR(25) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"modified_by" VARCHAR(25) NULL,
"modified_at" TIMESTAMPTZ NULL,
CONSTRAINT fk_order FOREIGN KEY ("order_no") REFERENCES "orders" ("order_no"),
CONSTRAINT fk_product_order FOREIGN KEY ("product_no") REFERENCES "products" ("product_no")
);

CREATE TABLE "order_details" (
"order_detail_no" BIGSERIAL PRIMARY KEY,
"order_no" BIGINT NOT NULL,
CONSTRAINT fk_order_detail FOREIGN KEY ("order_no") REFERENCES "orders" ("order_no")
);

CREATE TABLE "reviews" (
"review_no" BIGSERIAL PRIMARY KEY,
"product_no" BIGINT NOT NULL,
"member_no" BIGINT NOT NULL,
"rating" INTEGER NULL,
"title" VARCHAR(200) NOT NULL,
"content" TEXT NOT NULL,
"review_image" TEXT NULL,
"review_status" VARCHAR(20) NOT NULL,
"created_by" VARCHAR(25) NOT NULL,
"created_at" TIMESTAMPTZ NOT NULL,
"modified_by" VARCHAR(25) NULL,
"modified_at" TIMESTAMPTZ NULL,
CONSTRAINT fk_product_review FOREIGN KEY ("product_no") REFERENCES "products" ("product_no"),
CONSTRAINT fk_member_review FOREIGN KEY ("member_no") REFERENCES "members" ("member_no")
);