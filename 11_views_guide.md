# 🖼️ 視圖全攻略｜Views Guide

## 子章節索引
- [Creating Views|創建視圖](#creating-views創建視圖)
- [Altering or Dropping Views|更改或刪除視圖](#altering-or-dropping-views更改或刪除視圖)


---

### Creating Views|創建視圖
視圖是一種「虛擬表格」，不儲存資料，只在查詢時動態執行底層的SELECT語句。    
可用視圖簡化複雜查詢，例如封裝多表 JOIN 或常用過濾邏輯。    
若底層表結構或欄位變動，視圖會失效，需要重新 ALTER VIEW 或 DROP/CREATE。    

### 📌 語法結構
```sql
CREATE VIEW 別名 AS

(要儲存的查詢)
SELECT *
FROM 資料表
```

### 📘 範例
```sql
CREATE VIEW clients_balance AS

SELECT c.client_id,
       c.name,
       SUM(i.invoice_total-i.payment_total) AS balance
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY i.client_id, name;
```
---

### Altering or Dropping Views|更改或刪除視圖

### 📌 語法結構
```sql
CREATE OR REPLACE VIEW 別名 AS

(要更新的查詢)
SELECT *
FROM 資料表
```
```sql
刪除視圖->再重新建立
DROP VIEW 別名
```

### 📘 範例
```sql
CREATE OR REPLACE VIEW clients_balance AS

SELECT c.client_id,
       c.name,
       SUM(i.invoice_total-i.payment_total) AS balance
FROM clients c
JOIN invoices i USING(client_id)
GROUP BY i.client_id, name
order by balance DESC
```
---

