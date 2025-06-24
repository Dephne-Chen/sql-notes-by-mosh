# ⚙️ 函數與運算式｜Functions & Expressions

## 子章節索引
- [Numeric Functions|數值函數](#numeric-functions數值函數)
- [String Functions|字符串函數](#string-functions字符串函數)

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

