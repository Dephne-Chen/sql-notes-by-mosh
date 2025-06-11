# 🔢 聚合與分組操作｜Aggregation & Grouping

## 子章節索引
- [Aggregation|聚合函數](#aggregation聚合函數)
- [The Group By Clause|Group By 子句](#the-group-by-clausegroup-by-子句)
- [The Having Clause|Having子句](#the-having-clausehaving子句)
- [The Rollup Operator|ROLLUP 運算符](#the-rollup-operatorrollup運算符)

---

### Aggregation|聚合函數
(MAX()、MIN()、AVG()、SUM()、COUNT()...)   
(聚合函數都要使用括號來調用與執行)   
(要注意聚合函數只運行「非空值」!!)   

### 📌 語法結構
```sql
SELECT 聚合函數(欄位/運算式)
FROM 資料表
WHERE 條件
```

### 📘 範例
```sql
SELECT AVG(invoice_total),
       SUM(invoice_total * 1.1),
       COUNT(DISCINCT client_id)
FROM invoices
WHERE invoice_data > '2019-07-01' ;
# 使用 DISCINCT 返回不重複值
```

### 📘 範例
```sql
SELECT 'First Half of 2019' AS date_range,
        SUM(invoice_total) AS total_sales,
        SUM(payment_total) AS total_payments,
        SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date BETWEEN '2019-01-01' AND '2019-6-30' 
UNION
SELECT 'Second Half of 2019' AS date_range,
        SUM(invoice_total) AS total_sales,
        SUM(payment_total) AS total_payments,
        SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date BETWEEN '2019-07-01' AND '2019-12-31'  
UNION
SELECT 'Total' AS date_range,
        SUM(invoice_total) AS total_sales,
        SUM(payment_total) AS total_payments,
        SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date BETWEEN '2019-01-01' AND '2019-12-31';
```
---

### The Group By Clause|Group By 子句
(使用 GROUP BY 將資料依指定欄位分組，常與聚合函數搭配)   
(數據預設是按照Group By子句中指定的列排序，可使用order by 改變順序)   

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
WHERE 條件
GROUP BY 分組依據
```

### 📘 範例
```sql
SELECT client_id,
       SUM(invoice_total) AS total_sales
FROM invoices
WHERE invoice_date > '2019-01-01'
GROUP BY client_id
ORDER BY total_sales;
```

### 📘 範例
```sql
SELECT state,
       city,
       SUM(invoice_total) AS total_sales
FROM invoices
JOIN clients USING(client_id)
WHERE invoice_date > '2019-01-01'
GROUP BY state,city ;
```

### 📘 範例
```sql
SELECT p.date,
       pm.name AS payment_method,
       SUM(p.amount) AS total_payments
FROM payments p 
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id
GROUP BY date,payment_method 
ORDER BY date ;
```
---

### The Having Clause|Having子句
(在分組後行篩選)   
(HAVING子句裡的欄位必須在SELECT中出現)  

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
WHERE 分組前篩選條件
GROUP BY 分組依據
HAVING 分組後篩選條件
```

### 📘 範例
```sql
SELECT client_id,
       SUM(invoice_total) AS total_sales
FROM invoices
GROUP BY client_id
HAVING total_sales > 500;
```

### 📘 範例
```sql
SELECT client_id,
       SUM(invoice_total) AS total_sales
       COUNT(*) AS number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 500 AND number_of_invoices > 5 ;
```

### 📘 範例
```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       SUM(oi.quantity * unit_price) AS total_sales
FROM customers c
JOIN orders o USING(customer_id)
JOIN order_items oi USING(order_id)
WHERE state = 'VA'
GROUP BY c.customer_id,
         c.first_name,
         c.last_name
HAVING total_sales > 100;
```
---

### The Rollup Operator|ROLLUP 運算符
(在GROUP BY之後加上 WITH ROLLUP，可自動產生「每個分組的小計」以及「整體總計」行)

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
WHERE 條件
GROUP BY 分組依據 WITH ROLLUP
```

### 📘 範例
```sql
-- ex
SELECT client_id,
       SUM(invoice_total) AS total_sales
FROM invoices
GROUP BY client_id WITH ROLLUP;
```

### 📘 範例
```sql
SELECT pm.name AS payment_methods,
       SUM(p.amount) AS total
FROM payments p
JOIN payment_methods pm
    on p.payment_method = pm.payment_method_id
GROUP BY payment_methods WITH ROLLUP;
```
