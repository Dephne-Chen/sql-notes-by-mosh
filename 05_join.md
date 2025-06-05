 # 📎 JOIN 多表資料查詢

## 子章節索引
- [Inner Joins|內連接](#inner-joins內連接)
- [Joining Across Databases|跨資料庫連接](#joining-across-databases跨資料庫連接)
- [Self Joins|自連接](#self-joins自連接)
- [Joining Multiple Tables|多表連接](#joining-multiple-tables多表連接)
- [Compound Join Conditions|複合連接條件](#compound-join-conditions複合連接條件)
- [Implicit Join Syntax|隱式連接語法(不建議使用!!)](#implicit-join-syntax隱式連接語法 )
- [Outer Joins|外連接](#outer-joins外連接)


---
### Inner Joins|內連接
(INNER 可省略，預設就是內部連接，只顯示兩邊條件「都符合」的資料)

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表1
JOIN 資料表2
    ON 資料表1.欄位 = 資料表2.欄位
```

### 📘 範例
```sql
SELECT order_id, o.customer_id, first_name, last_name 
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id ;

SELECT order_id, p.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN products p
    ON oi.product_id = p.product_id ;
```
---
### Joining Across Databases｜跨資料庫連接
(用資料庫的名稱給欄位加上前綴)  
(只能跨同一台伺服器上的不同 database，不是跨伺服器)  

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表1
JOIN 資料庫.資料表2
    ON 資料表1.欄位 = 資料表2.欄位
```

### 📘 範例
```sql
SELECT order_id, p.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN sqi_inventory.products p
    ON oi.product_id = p.product_id ;
```
---

### Self Joins|自連接
(同一個資料表自己跟自己 JOIN，通常用來找「同一張表裡的關係（如：上下屬、父子節點）」)

### 📌 語法結構
```sql
SELECT a.欄位, b.欄位
FROM 資料表1 a
JOIN 資料表1 b
    ON a.欄位 = b.欄位
(使用不同的前綴表示資料表)  
(別名不可省略，否則 SQL 會混淆)  
```
### 📘 範例
```sql
SELECT e.employee_id, 
       e.first_name, 
       m.first_name AS manager
FROM employees e
JOIN employees m
    ON e.reports_to = m.employee_id ;
```
---

### Joining Multiple Tables|多表連接
(一次查詢連接三張（或更多）資料表，常見於需要從多個表格擷取相關資訊時)  
(每次 JOIN 都是兩表之間的關係條件，連誰都可以，不用拘泥「一定要跟第一個表」)  

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表1
JOIN 資料表2
    ON 資料表1.欄位 = 資料表2.欄位
JOIN 資料表3
    ON 資料表X.欄位 = 資料表3.欄位
```
	
### 📘 範例
```sql
SELECT o.order_id,
	   o.order_date,
       c.first_name,
       c.last_name,
       os.name AS status
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id
JOIN order_statuses os
    ON o.status = os.order_status_id ;

SELECT p.payment_id,
	   c.name,
       p.date,
       pm.name AS payment_method
FROM payments p
JOIN clients c
    ON p.client_id = c.client_id
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id ;
```
---

### Compound Join Conditions|複合連接條件
(兩張資料表的關聯不只一個欄位（如：複合主鍵、複合外鍵）時，JOIN 條件需同時比對多個欄位。)


### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表1
JOIN 資料表2
    ON 資料表1.欄位A = 資料表2.欄位A 
    AND 資料表1.欄位B = 資料表2.欄位B
```

### 📘 範例
```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    ON oi.order_id = oin.order_id 
    AND oi.product_id = oin.product_id;
```
---

### Implicit Join Syntax|隱式連接語法 
(舊式 SQL 寫法，容易出錯，不建議使用!!!)   
(記寫 WHERE 條件，會直接產生交叉連接)  


### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表1, 資料表2
WHERE 資料表1.欄位 = 資料表2.欄位
```

### 📘 範例
```sql
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id ;
```
---

### Outer Joins|外連接
(允許查詢時「保留一邊所有資料，另一邊沒有對應時以 NULL 補齊」)   
Left Join：返回左邊表的所有內容   
Right Join：返回右邊表的所有內容   

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表1
LEFT/RIGHT JOIN 資料表2
    ON 資料表1.欄位 = 資料表2.欄位
```
	
### 📘 範例
```sql
SELECT p.product_id,
       p.name,
       oi.quantity
FROM products p
LEFT JOIN order_items oi
    ON p.product_id = oi.product_id;
```
---

