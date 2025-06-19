# 🔍 子查詢與謂詞｜Subqueries & Predicates

## 子章節索引
- [Subqueries|子查詢](#subqueries子查詢)
- [The IN Operator|IN 運算符子查詢](#the-in-operatorin-運算符子查詢)
- [Subqueries vs Joins|子查詢 vs 連接](#subqueries-vs-joins子查詢-vs-連接)

---

### Subqueries|子查詢
(子查詢會先在內層執行，將結果傳回外層查詢，再由外層使用該結果進行篩選或計算。)

### 📌 語法結構
(單一值子查詢)

```sql
SELECT 欄位
FROM 資料表
WHERE 條件 + 運算符 + (
    SELECT 欄位
    FROM 資料表
    WHERE 條件
)
```

### 📘 範例
```sql
SELECT *
FROM products
WHERE unit_price > (
    SELECT unit_price
    FROM products
    WHERE product_id = 3
);
```

```sql
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```
---

### The IN Operator|IN 運算符子查詢
(使用多值子查詢，判斷某欄位是否存在於子查詢結果集合中；也可用 `NOT IN` 排除集合中的值。)


### 📌 語法結構
(多值子查詢)
```sql
SELECT 欄位
FROM 資料表
WHERE 欄位 IN / NOT IN (
    SELECT 欄位
    FROM 資料表
    WHERE 條件
)
```

### 📘 範例
```sql
SELECT *
FROM products
WHERE product_id NOT IN(
    SELECT DISTINCT product_id
    FROM order_items
);
```

```sql
SELECT *
FROM clients
WHERE client_id NOT IN(
    SELECT DISTINCT client_id
    FROM invoices
);
```
---

### Subqueries vs Joins|子查詢 vs 連接
(有時用子查詢表示，同樣也能改寫成連接（JOIN）方式；依可讀性與效能需求而定。)

### 子查詢版本
```sql
SELECT customer_id,
       first_name,
       last_name
FROM customers
WHERE customer_id IN (
    SELECT DISTINCT customer_id
    FROM orders
    WHERE order_id In (
        SELECT order_id
        FROM order_items
        WHERE product_id = 3
        )
);
```

### JOIN 版本
```sql
SELECT DISTINCT c.customer_id,
       c.first_name,
       c.last_name
FROM customers c
JOIN orders o USING (customer_id)
JOIN order_items oi USING (order_id)
WHERE oi.product_id = 3 ;
```
---


