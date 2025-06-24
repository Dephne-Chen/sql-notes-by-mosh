# ⚙️ 函數與運算式｜Functions & Expressions

## 子章節索引
- [Numeric Functions|數值函數](#numeric-functions數值函數)





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
