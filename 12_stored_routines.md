# 🗄️ 存儲例程與變量｜Stored Routines & Variables

## 子章節索引
- [Creating a Stored Procedure|創建存儲過程](#creating-a-stored-procedure創建存儲過程)
- [Dropping Procedures|刪除存儲過程](#dropping-procedures刪除存儲過程)
- [Parameters|參數](#parameters參數)
- [Parameters with Default Values|帶默認值的參數](#parameters-with-default-values帶默認值的參數)


---

### Creating a Stored Procedure|創建存儲過程

### 📌 語法結構
```sql
DELIMITER $$
CREATE PROCEDURE 別名(參數)
BEGIN
    SELECT *
    FROM 資料表;
END $$
DELIMITER ;
```

### 📘 範例
```sql
DELIMITER $$
CREATE PROCEDURE get_invoices_with_balance()
BEGIN
    SELECT *
    FROM invoices_with_balance
    WHERE balance > 0  ;
END $$
DELIMITER ;
```
---

### Dropping Procedures|刪除存儲過程

### 📌 語法結構
```sql
DROP PROCEDURE IF EXISTS 存儲過程名
```
---

### Parameters|參數

### 📌 語法結構
```sql
DELIMITER $$
CREATE PROCEDURE 別名(輸入參數 資料型態)
BEGIN
    SELECT *
    FROM 資料表;
END $$
DELIMITER ;
```

### 📘 範例
```sql
DELIMITER $$
CREATE PROCEDURE get_invoices_by_state
(
    state CHAR(2)
)
BEGIN
    SELECT *
    FROM clients c
    WHERE c.state = state ;
END $$
DELIMITER ;

CALL get_invoices_by_state('CA')
```
---

### Parameters with Default Values|帶默認值的參數

### 📘 範例
```sql
-- 參數為 NULL 時，預設查詢 CA 州
DELIMITER $$
CREATE PROCEDURE get_invoices_by_state
(
    state CHAR(2)
)
BEGIN
    IF stata IS NULL THEN
        SET state = 'CA';
    END IF;
    SELECT *
    FROM clients c
    WHERE c.state = state ;
END $$
DELIMITER ;
```
```sql
-- 參數為 NULL 時，返回所有客戶；否則按州過濾
DELIMITER $$
CREATE PROCEDURE get_invoices_by_state
(
    state CHAR(2)
)
BEGIN
    IF stata IS NULL THEN
        SELECT * FROM clients;
    ELSE
        SELECT *
        FROM clients c
        WHERE c.state = state ;
    END IF;
END $$
DELIMITER ;
```
```sql
-- 同上，以IFNULL改寫
DELIMITER $$
CREATE PROCEDURE get_invoices_by_state
(
    state CHAR(2)
)
BEGIN
        SELECT *
        FROM clients c
        WHERE c.state = IFNULL(state,c.state) ;
END $$
DELIMITER ;
```
---

