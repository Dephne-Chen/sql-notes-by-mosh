# ⚙️ 函數與運算式｜Functions & Expressions

## 子章節索引
- [Numeric Functions|數值函數](#numeric-functions數值函數)
- [String Functions|字符串函數](#string-functions字符串函數)
- [Date Functions|日期函數](#date-functions日期函數)
- [Formatting Dates and Times|格式化日期和時間](#formatting-dates-and-times格式化日期和時間)
- [Calculating Dates and Times|計算日期與時間](#calculating-dates-and-times計算日期與時間)
- [THE IFNULL and Coalesce|IFNULL 和 Coalesce 函數](#the-ifnull-and-coalesceifnull-和-coalesce-函數)

---

### Numeric Functions|數值函數

### 📌 語法結構
```sql
ROUND(數值, 小數後第幾位) 四捨五入

TRUNCATE(數值, 小數後第幾位) 無條件捨去

CEILING(數值) 返回>=此數的最小整數

FLOOR(數值) 返回<=此數的最大整數

ABS(數值) 計算絕對值

RAND() 生成0-1區間的隨機浮點數
```
### 📘 範例
```sql
SELECT
    ROUND(5.735, 2)    AS rounded_value,   -- → 5.74
    TRUNCATE(5.768, 1) AS truncated_value, -- → 5.7
    CEILING(5.2)       AS ceil_value,      -- → 6
    FLOOR(5.2)         AS floor_value,     -- → 5
    ABS(-3.8)          AS absolute_value,  -- → 3.8
    RAND()             AS random_value;    -- → e.g. 0.4721
```
---

### String Functions|字符串函數

### 📌 語法結構
```sql
LENGTH() 得到字符串的字符數

UPPER() / LOWER()  將字母轉大寫/小寫

LTRIM() / RTRIM() / TRIM() 清除前面空格/後面空格/前後方空格

LEFT(字串,數量) / RIGHT(字串,數量) 返回字符串前方/後方特定字符數

SUBSTRING(字串,起始位置,長度) 返回字符串中任何位置的字符

LOCATE(要搜索的內容,字串) 返回一個字符或一串字符的匹配位置

REPLACE(字串,被替換,替換) 替換一個字符或一串字符

CONCAT(字串,字串) 串連兩個字符串
```

### 📘 範例
```sql
SELECT 
    LENGTH('Sky')             AS len,       -- → 3
    UPPER('Sky')              AS upper_sky, -- → SKY
    LOWER('Sky')              AS lower_sky, -- → sky
    LTRIM('   Sky')           AS ltrimmed,  -- → 'Sky'
    RTRIM('Sky   ')           AS rtrimmed,  -- → 'Sky'
    TRIM('   Sky   ')         AS trimmed,   -- → 'Sky'
    LEFT('chocolate', 5)      AS left5,     -- → choco
    RIGHT('chocolate', 4)     AS right4,    -- → late
    SUBSTRING('tree', 2, 2)   AS substr,    -- → re
    LOCATE('h', 'chocolate')  AS pos,       -- → 2
    REPLACE('trea','ea','ee') AS fixed,     -- → tree
    CONCAT('first','last')    AS full       -- → firstlast
```
---

### Date Functions|日期函數

### 📌 語法結構
```sql
NOW() 當前日期與時間

CURDATE() 當前日期

CURTIME() 當前時間

YEAR()/MONTH()/DATE() 提取年份/月份/日期

HOUR()/MINUTE()/SECOND() 提取時刻/分鐘/秒數

DAYNAME()/MONTHNAME()  獲取字符串格式的星期數/月份

EXTRACT(想提取的單位 + FROM + 時間日期值)
```

### 📘 範例
```sql
SELECT *
FROM orders
WHERE YEAR(order_date) = YEAR(NOW());
```
```sql
SELECT EXTRACT(YEAR FROM NOW());
```
---

### Formatting Dates and Times|格式化日期和時間

### 📌 語法結構
```sql
DATE_FORMAT(日期值, 格式字符串)
DATE_FORMAT(時間點, 格式字符串)
```
```sql
# 常用格式符號
%y -> 兩位數的年份 25
%Y -> 四位數的年份 2025
%m -> 兩位數的月份 07
%M -> 月份名稱 July
%d -> 兩位數的日期 01
%D -> 日期名稱 1st
%a -> 星期 Tue    

%H -> 代表小時 24小時制
%h -> 代表小時 12小時制
%i -> 代表分鐘
%s -> 代表秒數
%p -> pm or am
```
### 📘 範例
```sql
SELECT DATE_FORMAT('2025-07-01', '%M %d %y');
```

```sql
SELECT TIME_FORMAT(NOW(),'%H : %i %p');
```
---

### Calculating Dates and Times|計算日期與時間

### 📌 語法結構
```sql
DATE_ADD(時間值,表達式) 給日期時間值加減值
DATE_SUB(時間值,表達式) 給日期時間值減值
DATEDIFF(日期1,日期2) 計算兩日期間隔(只返回天數)
TIME_TO_SEC(時間1) 返回從零點計算的秒數
```

### 📘 範例
```sql
SELECT DATE_ADD(NOW(),INTERVAL 1 DAY);
SELECT DATE_ADD(NOW(),INTERVAL -1 YEAR);
SELECT DATE_SUB(NOW(),INTERVAL 1 MONTH);
```
```sql
SELECT DATEDIFF('2025-07-04','2025-07-01');
SELECT TIME_TO_SEC('09:04') - TIME_TO_SEC('09:00');
```
---

### THE IFNULL and Coalesce|IFNULL 和 Coalesce 函數

### 📌 語法結構
```sql
IFNULL(欄位, 要返回的值) 如果欄位為空則返回值
COALESCE(欄位1, 欄位２, 要返回的值) 如果欄位1為空則返回欄位2,若欄位2為空則返回值
```
### 📘 範例
```sql
SELECT order_id,
       IFNULL(shipper_id,'未分配') AS Shipper 
FROM orders;
```
```sql
SELECT order_id,
       COALESCE(shipper_id,comments,'未分配') AS Shipper 
FROM orders;
```
---
