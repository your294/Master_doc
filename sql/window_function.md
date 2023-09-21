## window_function.md
## 窗口函数语法如下
```sql
<window func> (<exp>) over (
    [partition by <columns name>]
    [order by <> asc | desc]
    [rows <window_scope>]
)
-- partition by 将结果集分为多个分区, 每个分区内的行共享值, 未指定时每行作为单独分区
```
### example code
use window function which will used in there group and every row;
if LIKE **over()** it will set the whole as a group
```sql
SELECT
         year, country, product, profit,
         SUM(profit) OVER() AS total_profit,
         SUM(profit) OVER(PARTITION BY country) AS country_profit
       FROM sales
       ORDER BY country, year, product, profit;
```

该OVER子句允许用于许多聚合函数，因此可以用作窗口或非窗口函数，具体取决于该 OVER子句是否存在：

```sql
AVG()
BIT_AND()
BIT_OR()
BIT_XOR()
COUNT()
JSON_ARRAYAGG()
JSON_OBJECTAGG()
MAX()
MIN()
STDDEV_POP(), STDDEV(), STD()
STDDEV_SAMP()
SUM()
VAR_POP(), VARIANCE()
VAR_SAMP()
```

**MySQL 还支持仅用作窗口函数的非聚合函数。对于这些，该OVER条款是强制性的：**
```sql
CUME_DIST()
DENSE_RANK()
FIRST_VALUE()
LAG()
LAST_VALUE()
LEAD()
NTH_VALUE()
NTILE()
PERCENT_RANK()
RANK()
ROW_NUMBER()

```

```sql
-- problem lc https://leetcode.cn/problems/rank-scores/submissions/

# Write your MySQL query statement below
SELECT score, 
        DENSE_RANK() OVER w AS 'rank' 
        from Scores
        WINDOW w as (order by score desc)
        ;

-- another ways to solve this problem
SELECT score, DNESE_RANK() OVER (
    order by 
        score desc
) as 'rank'
FROM Scores;

-- use subquery to solve this problem
SELECT score, (
    SELECT count(DISTINCT S2.score) from Scores as S2 
    where S1.score <= S2.score
    ) as 'rank'
    from Scores as S1
    order by S1.score desc;

-- use join to solve the problem
SELECT S.score, 
        count(DISTINCT T.score) as 'rank'
      FROM Scores S 
      Inner join Scores T ON (S.score <= T.score)
      GROUP BY S.id, S.score
      ORDER by S.score desc;
```
