# 📎 JOIN 多表資料查詢

## 子章節索引
- [(INNER) JOIN](#inner-join)
- [跨資料庫連接](#跨資料庫連接)
- [Self Joins (自連接)](#self-joins-自連接)


---
### (INNER) JOIN
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
### 跨資料庫連接
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

### Self Joins (自連接)
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
