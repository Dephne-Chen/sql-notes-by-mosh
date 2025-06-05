# 🔍 SQL 筆記：SELECT & FROM



## 📌 語法結構

```sql
SELECT 欄位名稱1, 欄位名稱2, ...
FROM 資料表名稱;
```

## 📘 範例

```sql
SELECT customer_id, phone
FROM customers;
```

```sql
-- 使用 * 代表查詢所有欄位：
SELECT * FROM customers;
```

```sql
-- 可對欄位進行運算
SELECT customer_id, (points + 10) *100
FROM customers;
```

```sql
-- 使用 AS 給運算結果命名（別名） → 提升可讀性
SELECT product_name, price * quantity AS total_price
FROM products;
# 如果希望名稱裡是空格 可加單引號 'total price'
```

```sql
-- 查詢「不重複的唯一值」
SELECT DISTINCT state
FROM customers;
```
