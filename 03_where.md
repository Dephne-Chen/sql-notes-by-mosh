# 🎯 WHERE 條件過濾

## 📌 語法結構
```sql
SELECT 欄位名稱1, 欄位名稱2, ...
FROM 資料表名稱
WHERE 篩選條件;
```

## 📘 範例
```sql
SELECT * FROM customers
WHERE points > 3000;

SELECT * FROM orders
WHERE order_date >= '2019-01-01';
# 日期和字串一樣要加單引號

```
## 🧠 支援的運算子包括：
數字比較：=, !=, >, <, >=, <=
邏輯運算：AND, OR, NOT
字串比對：=, LIKE, IN, NOT IN

### 子章節索引
[AND, OR, NOT 邏輯運算符](#and-or-not-邏輯運算符)

## AND, OR, NOT 邏輯運算符

## 📌 語法結構
```sql
AND：同時滿足兩個條件
SELECT 欄位
FROM 資料表
WHERE 條件1 AND 條件2;

OR：任一條件滿足即可
SELECT 欄位
FROM 資料表
WHERE 條件1 OR 條件2;

NOT：排除條件，等於「不符合」
SELECT 欄位
FROM 資料表
WHERE NOT 條件;
```

## 📘 範例
```sql
SELECT * FROM order_items
WHERE order_id = 6 AND (quantity * unit_price) > 30 ;

SELECT * FROM customers
WHERE NOT(birth_date > '1990-01-01' OR points >1000);

# 跟上面同義但更直觀
SELECT * FROM customers
WHERE birth_date <= '1990-01-01' AND points <=1000 ;
```
