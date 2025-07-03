# 🖼️ 視圖全攻略｜Views Guide

## 子章節索引
- [Creating Views|創建視圖](#creating-views創建視圖)
- [Altering or Dropping Views|更改或刪除視圖](#altering-or-dropping-views更改或刪除視圖)
- [Updattable Views|可更新視圖](#updattable-views可更新視圖)
- [The WITH OPTION CHECK Clause|WITH OPTION CHECK 子句](#the-with-option-check-clausewith-option-check-子句)


---

### Creating Views|創建視圖
視圖是一種「虛擬表格」，不儲存資料，只在查詢時動態執行底層的SELECT語句。    
可用視圖簡化複雜查詢，例如封裝多表 JOIN 或常用過濾邏輯。    
若底層表結構或欄位變動，視圖會失效，需重新`ALTER VIEW`或`DROP`/`CREATE`。    

### 📌 語法結構
```sql
CREATE VIEW 別名 AS

(要儲存的查詢)
SELECT *
FROM 資料表
```

### 📘 範例
```sql
CREATE VIEW clients_balance AS

SELECT c.client_id,
       c.name,
       SUM(i.invoice_total-i.payment_total) AS balance
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY i.client_id, name;
```
---

### Altering or Dropping Views|更改或刪除視圖

### 📌 語法結構
```sql
CREATE OR REPLACE VIEW 別名 AS

(要更新的查詢)
SELECT *
FROM 資料表
```
```sql
刪除視圖->再重新建立
DROP VIEW 別名
```

### 📘 範例
```sql
CREATE OR REPLACE VIEW clients_balance AS

SELECT c.client_id,
       c.name,
       SUM(i.invoice_total-i.payment_total) AS balance
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY i.client_id, name
order by balance DESC
```
---

### Updattable Views|可更新視圖
如果視圖中沒有 `DISTINCT`/`Aggregate Functiions`/`Group by`/`Having`/`UNION`     
我們稱為可更新視圖->能在 `INSERT`/`UPDATE`/`DELETE` 語句中使用。

### 📘 範例
```sql
-- 建立一個可更新的視圖，只包含 order_items 表的欄位
CREATE VIEW order_items_view AS
SELECT 
  order_id,
  product_id,
  quantity,
  unit_price
FROM order_items;

-- 在視圖上執行 UPDATE
UPDATE order_items_view
SET quantity = quantity + 1
WHERE order_id = 101 AND product_id = 5;

-- 在視圖上執行 DELETE
DELETE FROM order_items_view
WHERE order_id = 102 AND product_id = 2;

-- 在視圖上執行 INSERT
INSERT INTO order_items_view (order_id, product_id, quantity, unit_price)
VALUES (103, 8, 3, 19.99);
```
---

### The WITH OPTION CHECK Clause|WITH OPTION CHECK 子句
視圖的默認行為，當通過視圖更新或刪除數據時，有一些行會不見。    
WITH CHECK OPTION 可防止更新或刪除語句時將行從視圖刪除。

### 📌 語法結構
```sql
CREATE VIEW 別名 AS
SELECT *
FROM 資料表
WITH CHECK OPTION
```

### 📘 範例
```sql
-- 建立視圖，加入 WITH CHECK OPTION
CREATE VIEW active_customers AS
SELECT customer_id, name, status
FROM   customers
WHERE  status = 'active'
WITH  CHECK OPTION;

UPDATE active_customers
SET    status = 'inactive'
WHERE  customer_id = 42;
-> 嘗試透過視圖更新，若結果不再符合 status='active'，將會失敗報錯
```
---

