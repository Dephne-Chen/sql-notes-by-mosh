# 📊 ORDER BY / LIMIT 排序與筆數限制

## ORDER BY 子句
(在MySQL中可以用任何列排序，無論是否在SELECT子句中)

## 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
ORDER BY 欄位(排序依據);
```

## 📘 範例

```sql
SELECT * FROM customers
ORDER BY first_name ;

-- DESC 降序
SELECT * FROM customers
ORDER BY customer_id DESC ;

-- 依據多列排序
SELECT * FROM customers
ORDER BY state, first_name ;

-- 透過列位置給數據排序(避免使用，因列順序可能改變)
SELECT first_name, last_name 
FROM customers
ORDER BY 1, 2 ;
# 相當於 ORDER BY first_name, last_name 
```
---
## LIMIT 子句
(限制回傳筆數)
(只取部分資料，常用於預覽或前 N 筆資料)

## 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
LIMIT 筆數;
```

## 📘 範例

```sql
SELECT * FROM customers
LIMIT 5;

-- 搭配 offset 進行分頁查詢
-- (大量資料查詢中使用 OFFSET 效能可能會下降，可考慮 Key-based 分頁策略)
SELECT * FROM customers
LIMIT 3 OFFSET 5;
# 跳過前5筆，取6-8筆

SELECT * FROM customers
LIMIT 5,3 ;
# 同上
```
