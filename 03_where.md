# 🎯 WHERE 條件過濾

## 📌 語法結構
```sql
SELECT 欄位名稱1, 欄位名稱2, ...
FROM 資料表名稱
WHERE 篩選條件;

-- 支援的運算子包括：
數字比較：=, !=, >, <, >=, <=
邏輯運算：AND, OR, NOT
字串比對：=, LIKE, IN, NOT IN
```

## 📘 範例
```sql
SELECT * FROM customers
WHERE points > 3000;

SELECT * FROM orders
WHERE order_date >= '2019-01-01';
# 日期和字串一樣要加單引號

```
## 子章節索引
- [AND, OR, NOT 邏輯運算符](#and-or-not-邏輯運算符)
- [IN / NOT IN 運算符](#in--not-in-運算符)
- [BETWEEN 運算符](#between-運算符)
- [LIKE / REGEXP 運算符](#like--regexp-運算符)
- [IS NULL / IS NOT NULL 運算符](#is-null--is-not-null-運算符)

---
### AND, OR, NOT 邏輯運算符

### 📌 語法結構
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

### 📘 範例
```sql
SELECT * FROM order_items
WHERE order_id = 6 AND (quantity * unit_price) > 30 ;

SELECT * FROM customers
WHERE NOT(birth_date > '1990-01-01' OR points >1000);

# 跟上面同義但更直觀
SELECT * FROM customers
WHERE birth_date <= '1990-01-01' AND points <=1000 ;
```
---
### IN / NOT IN 運算符

### 📌 語法結構
```sql
IN：欄位值「包含在集合內」
SELECT 欄位
FROM 資料表
WHERE 欄位 IN (值1, 值2, 值3);

NOT IN：欄位值「不包含在集合內」
SELECT 欄位
FROM 資料表
WHERE 欄位 NOT IN (值1, 值2);
```
### 📘 範例
```sql
SELECT * FROM customers
WHERE state IN ('VA','FL','GA')

SELECT * FROM products
WHERE quantity_in_stock IN (49,38,72)
```
---

### BETWEEN 運算符

### 📌 語法結構
```sql
BETWEEN：範圍查詢，包含邊界值
SELECT 欄位
FROM 資料表
WHERE 欄位 BETWEEN 起始值 AND 結束值;
```

### 📘 範例
```sql
SELECT * FROM customers
WHERE points BETWEEN 1000 AND 3000;

SELECT * FROM customers
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```
---

### LIKE / REGEXP 運算符

#### 💫 LIKE (模糊比對)  
% ：任意數量的任意字元  
_ ：任意單一字元  

#### 💫 REGEXP (正則表達式比對)   
^ ：開頭（例如 ^a → a 開頭）  
$ ：結尾（例如 y$ → y 結尾）  
| ：代表多個搜尋 OR   
[]：字元集合 ex:[abc]e ->ae、be、ce =[a-b]e   

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
WHERE 欄位 LIKE/REGEXP 字符串樣式;
```

### 📘 範例
```sql
SELECT * FROM customers
WHERE last_name LIKE '___y';

SELECT * FROM customers
WHERE address LIKE '%TRAIL%' OR 
	    address LIKE '%AVENUE%';
	    
SELECT * FROM customers
WHERE address REGEXP 'TRAIL|AVENUE';
	    
SELECT * FROM customers
WHERE last_name LIKE '^field';	

SELECT * FROM customers
WHERE last_name LIKE 'field|mac$|^rose';
```  
---
### IS NULL / IS NOT NULL 運算符
(尋找有/無缺失值的資料)  
(= NULL 或 != NULL 是無效的，必須使用 IS NULL 或 IS NOT NULL)  

### 📌 語法結構
```sql
SELECT 欄位
FROM 資料表
WHERE 欄位 IS NULL;
```
  
### 📘 範例
```sql
SELECT * FROM customers
WHERE phone IS NULL;

SELECT * FROM orders
WHERE shipped_date IS NOT NULL
```  
