# 🛠️ 資料操作：INSERT／UPDATE／DELETE

## 子章節索引
- [Inserting a Row|插入單行](#inserting-a-row插入單行)
- [Inserting Multiple Rows|插入多行](#inserting-multiple-rows插入多行)
- [Inserting Hierarchical Rows|插入分層行](#inserting-hierarchical-rows插入分層行)
- [Creating a Copy of a Table|創建表複製](#creating-a-copy-of-a-table創建表複製)
- [Updating a Single Row|更新單行](#updating-a-single-row更新單行)
- [Updating Multiple Rows|更新多行](#updating-multiple-rows更新多行)
- [Using Subqueries in Updates|在更新中用子查詢](#using-subqueries-in-updates在更新中用子查詢)
- [Deleting Rows|刪除行](#deleting-rows刪除行)

---

### Inserting a Row|插入單行
(在 INSERT 前可先 SHOW COLUMNS（或 DESCRIBE）確認欄位順序與預設值)

### 📌 語法結構
```sql
-- 不指定欄位，需依表格欄位順序提供所有欄位值
INSERT INTO 資料表
VALUES (val1, val2, val3, …)

-- 指定欄位，依指定順序提供對應欄位值
INSERT INTO 資料表 (colA, colB, colC)
VALUES (valA, valB, valC)
```

### 📘 範例
```sql
INSERT INTO customers
VALUES (default,'John','Smith','1990-01-01','123 Main St');
```
```sql
INSERT INTO customers(first_name,last_name,address)
VALUES ('John','Smith','123 Main St');
# 可以任何順序陳列，不用按照表格順序
# 未被賦值的欄位，將使用預設值或保持 NULL
```
---

### Inserting Multiple Rows|插入多行
(多行插入可以提升效能，但單次 VALUES 列表過多時，可能會因為查詢長度或鎖表時間過長而影響資料庫效能)

### 📌 語法結構
```sql
(用逗號分隔多組 VALUES 列表)
INSERT INTO 資料表 (colA, colB, colC)
VALUES (valA1, valB1, valC1),
       (valA2, valB2, valC2),...
```

### 📘 範例
```sql
INSERT INTO shippers(name)
VALUES ('shipper1'),
       ('shipper2'),
       ('shipper3');
```
```sql	   
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES('product1',10,2.5), 
      ('product2',15,5),
      ('product3',20,3.8);
```
---

### Inserting Hierarchical Rows|插入分層行
(往多表插入數據)(有母子關係的表)   
(先插入母資料，再使用 `LAST_INSERT_ID()` 取得自增主鍵值，插入子資料。)   
(若一次插入多筆母資料，LAST_INSERT_ID() 只回傳第一筆的自增 ID。)   

### 📌 語法結構
```sql
-- 1. 插入父層資料
INSERT INTO parent_table (colA, colB, colC)
VALUES (valA1, valB1, valC1);

-- 2. 插入子層資料，使用 LAST_INSERT_ID() 取得父表自增主鍵
INSERT INTO child_table (parent_id, colX, colY)
VALUES (LAST_INSERT_ID(), valX1, valY1),
       (LAST_INSERT_ID(), valX2, valY2), …;
```

### 📘 範例
```sql
INSERT INTO orders(customer_id, order_date, status)
VALUES (1, '2025-01-02', 2);
			 
INSERT INTO order_iyems
VALUES (LAST_INSERT_ID(), 1, 2.95),
       (LAST_INSERT_ID(), 2, 5.50);
```
---

### Creating a Copy of a Table|創建表複製
(從現有資料表複製結構或資料到另一張表，常用於資料備份與分割歸檔。)


### 📌 語法結構
```sql
-- 將資料複製到新建立的表
(同時複製結構與資料，但不會複製索引與約束（需另行建立）)
CREATE TABLE 新資料表名稱 AS
SELECT 欄位 FROM 資料表
WHERE 條件

--使用 INSERT … SELECT 將資料複製到已存在表
INSERT INTO 資料表
SELECT 欄位 FROM 資料表
WHERE 條件
```

### 📘 範例
```sql
CREATE TABLE orders_archived AS
SELECT * FROM orders;
```
```sql
INSERT INTO orders_archived
SELECT * FROM orders
WHERE order_date < '2019-01-01';
```
```sql
CREATE TABLE invoices_archived AS
SELECT i.invoice_id,
       i.number,
       c.name AS client,
       i.invoice_total,
       i.payment_total,
       i.invoice_date,
       i.due_date,
       i.payment_date
FROM invoices i 
JOIN clients c
    USING(client_id)
WHERE payment_date IS NOT NULL;
```
---

### Updating a Single Row|更新單行
(修改資料表中符合條件的一筆資料)


### 📌 語法結構
```sql
UPDATE 資料表
SET 欄位1 = 值1,欄位2 = 值2
WHERE 識別資料列的條件
(若省略WHERE則會更新整張表，務必小心!!)
```

### 📘 範例
```sql
UPDATE invoices
SET payment_total = 10,payment_date = '2019-01-01'
WHERE invoice_id = 1;
```
```sql
UPDATE invoices
SET payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE invoice_id = 3;
```
---

### Updating Multiple Rows|更新多行
(根據條件一次更新多筆資料，常用於批次調整或資料修正)

### 📌 語法結構
```sql
UPDATE 資料表
SET 欄位1 = 值1,欄位2 = 值2
WHERE 識別資料列的條件(更通用的條件)
(若省略WHERE則會更新整張表)
```

### 📘 範例
```sql
UPDATE customers
SET points = points + 50
WHERE birth_date < '1990-01-01';
```
---

### Using Subqueries in Updates|在更新中用子查詢

### 📌 語法結構
```sql
UPDATE 資料表
SET 欄位1 = 值1,欄位2 = 值2
WHERE 關鍵欄位 = SELECT子查詢
(若子查詢返回結果為多行，則不能使用 = 要使用 IN)
```

### 📘 範例
```sql
UPDATE invoices
SET payment_total = invoice_total * 0.5
WHERE client_id = 
	(SELECT client_id
	 FROM clients
	 WHERE name = 'Myworks');
```
```sql
UPDATE orders
SET comments = 'Gold Customer'
WHERE customer_id IN 
	(SELECT customer_id
	 FROM customers
	 WHERE points > 3000);
```
---

### Deleting Rows|刪除行

### 📌 語法結構
```sql
DELETE FROM 資料表
WHERE 識別資料列的條件
(若省略WHERE則會刪除整張表的紀錄)

-- 運用子查詢
DELETE FROM 資料表
WHERE 關鍵欄位 = SELECT子查詢
(若子查詢返回結果為多行，則不能使用 = 要使用 IN)
```

### 📘 範例
```sql
DELETE FROM invoices
WHERE client_id = 
	(SELECT client_id
	 FROM clients
	 WHERE name = 'Myworks');
```
