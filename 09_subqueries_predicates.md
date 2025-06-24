# 🔍 子查詢與謂詞｜Subqueries & Predicates

## 子章節索引
- [Subqueries|子查詢](#subqueries子查詢)
- [The IN Operator|IN 運算符子查詢](#the-in-operatorin-運算符子查詢)
- [Subqueries vs Joins|子查詢 vs 連接](#subqueries-vs-joins子查詢-vs-連接)
- [The ALL Keyword|ALL 關鍵字](#the-all-keywordall-關鍵字)
- [The ANY Keyword|ANY 關鍵字](#the-any-keywordany-關鍵字)
- [Correlated Subquaries|相關子查詢](#correlated-subquaries相關子查詢)
- [The EXISTS Operator|Exists 運算符](#the-exists-operatorexists-運算符)
- [Subqueries in the SELECT Clause|SELECT 子句中的子查詢](#subqueries-in-the-select-clauseselect-子句中的子查詢)

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

### The ALL Keyword|ALL 關鍵字
(用於多值子查詢，表示「欄位值要符合所有子查詢返回結果」，相當於比較該值與子查詢所有值的關係)

### 📌 語法結構
(多值子查詢)
```sql
SELECT 欄位
FROM 資料表
WHERE 欄位+運算符+ALL(
    SELECT 欄位
    FROM 資料表
    WHERE 條件
);
```

### 📘 範例
```sql
SELECT *
FROM invoices
WHERE invoice_total > ALL(
    SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
);
```
---

### The ANY Keyword|ANY 關鍵字
(`ANY`（或同義的 `SOME`）用於多值子查詢，表示「只要欄位值符合子查詢結果中**任一**值即為真」。)

### 📌 語法結構
(多值子查詢)
```sql
SELECT 欄位
FROM 資料表
WHERE 欄位+運算符+ANY(
    SELECT 欄位
    FROM 資料表
    WHERE 條件
)
```

### 📘 範例
```sql
SELECT *
FROM clients
WHERE client_id = ANY(
    SELECT client_id
    FROM invoices
    GROUP BY client_id
    HAVING COUNT(*) >= 2
);
```
---

### Correlated Subquaries|相關子查詢
(相關子查詢是在內層子查詢中引用外層查詢的欄位，對每一筆外層資料重複執行，通常會較耗時)    
(當資料量大時，可考慮使用 JOIN 加分組聚合代替，提高效能)    

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表 t
WHERE 欄位+運算符+(
    SELECT 欄位
    FROM 資料表
    WHERE 欄位 = t.欄位
)
```

### 📘 範例
```sql
SELECT *
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE office_id = e.office_id
);
```
```sql
SELECT * 
FROM invoices i
WHERE invoice_total > (
    SELECT AVG(invoice_total)
    FROM invoices
    WHERE client_id = i.client_id
);
```
---

### The EXISTS Operator|Exists 運算符
(當IN運算符的集合中資料量太大，可改用EXISTS)     
(EXISTS會回傳TRUE/FALSE)     

### 📌 語法結構
```sql
SELECT *
FROM 資料表 t
WHERE EXISTS(
    SELECT 欄位
    FROM 資料表
    WHERE 欄位 = t.欄位
)
```

### 📘 範例
```sql
SELECT * 
FROM products p
WHERE NOT EXISTS (
    SELECT product_id
    FROM order_items
    WHERE product_id = p.product_id
);
```
---

### Subqueries in the SELECT Clause|SELECT 子句中的子查詢

### 📌 語法結構
```sql
SELECT 欄位,
       (SELECT 欄位
        FROM 資料表) AS 別名
FROM 資料表
```

### 📘 範例
```sql
SELECT invoice_id,
       invoice_total,
       (SELECT AVG(invoice_total)
       FROM invoices) AS invoice_average,
       invoice_total - (SELECT invoice_average) AS difference
FROM invoices ; 
```

```sql
SELECT client_id,
       name,
       (SELECT SUM(invoice_total)
       FROM invoices
       WHERE client_id = c.client_id) AS total_sales,
       (SELECT AVG(invoice_total)
       FROM invoices) AS average,
       (SELECT total_sales - average) AS difference
FROM clients c ;
```
---
