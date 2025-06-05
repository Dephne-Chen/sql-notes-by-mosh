# ➕ UNION 合併多段查詢結果
(將多個查詢結果垂直合併，只保留「不重複」的資料)  
(合併的查詢必須有相同的欄位數量與資料型態相容)  
(欄位名稱以「第一個 SELECT 查詢」的名稱為主)  

## 📌 語法結構

```sql

SELECT 欄位
FROM 資料表A
[WHERE 條件A]

UNION 

SELECT 欄位
FROM 資料表B
[WHERE 條件B];
```


## 📘 範例

```sql
SELECT customer_id,
       first_name,
       points,
       'Bronze' AS type
FROM customers
WHERE points < 2000

UNION

SELECT customer_id,
       first_name,
       points,
       'SILVER' 
FROM customers
WHERE points BETWEEN 2000 AND 3000

UNION

SELECT customer_id,
       first_name,
       points,
       'Gold' 
FROM customers
WHERE points > 3000

ORDER BY first_name;
```
