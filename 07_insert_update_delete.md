# 🛠️ 資料操作：INSERT／UPDATE／DELETE

## 子章節索引
- [Inserting a row|插入單行](#inserting-a-row插入單行)
- [Inserting Multiple rows|插入多行](inserting-multiple-rows插入多行)


---

### Inserting a row|插入單行
(在 INSERT 前可先 SHOW COLUMNS（或 DESCRIBE）確認欄位順序與預設值)

### 📌 語法結構
```sql
-- 不指定欄位，需依表格欄位順序提供所有欄位值
INSERT INTO 資料表
VALUES (val1, val2, val3, …)

-- 指定欄位，依指定順序提供對應欄位值
INSERT INTO 資料表 (colA, colB, colC)
VALUES (valA, valB, valC)
```

### 📘 範例
```sql
INSERT INTO customers
VALUES (default,'John','Smith','1990-01-01','123 Main St');
```
```sql
INSERT INTO customers(first_name,last_name,address)
VALUES ('John','Smith','123 Main St');
# 可以任何順序陳列，不用按照表格順序
# 未被賦值的欄位，將使用預設值或保持 NULL
```
---

### Inserting Multiple rows|插入多行
(多行插入可以提升效能，但單次 VALUES 列表過多時，可能會因為查詢長度或鎖表時間過長而影響資料庫效能)

### 📌 語法結構
```sql
(用逗號分隔多組 VALUES 列表)
INSERT INTO 資料表 (colA, colB, colC)
VALUES (valA1, valB1, valC1),
       (valA2, valB2, valC2),...
```

### 📘 範例
```sql
INSERT INTO shippers(name)
VALUES ('shipper1'),
       ('shipper2'),
       ('shipper3');
```
```sql	   
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES('product1',10,2.5), 
      ('product2',15,5),
      ('product3',20,3.8);
```
---

