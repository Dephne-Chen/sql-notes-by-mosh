# 🔢 聚合與分組操作｜Aggregation & Grouping

## 子章節索引
- [Aggregation|聚合函數](#aggregation聚合函數)


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

