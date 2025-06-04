# 🎯 WHERE 條件過濾

## 📌 語法結構
```sql
SELECT 欄位名稱1, 欄位名稱2, ...
FROM 資料表名稱
WHERE 篩選條件;
```

## 🧠 支援的運算子包括：
數字比較：=, !=, >, <, >=, <=
邏輯運算：AND, OR, NOT
字串比對：=, LIKE, IN, NOT IN

## 📘 範例
```sql
SELECT * FROM customers
WHERE points > 3000;

SELECT * FROM orders
WHERE order_date >= '2019-01-01';
# 日期和字串一樣要加單引號
