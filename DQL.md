###### tags: `MySQL`
# DQL 
>Data Query Language，用以查詢Table內的資料，僅查詢，不會改變資料

## select 敘述的子句
敘述句順序**不能更換**
==select > from > where > group by > having > order by > limit==
- select：顯示的欄位
- from：資料的來源
- where：過濾條件
- group by：資料分組的依據
- having：**分組欄位**的過濾條件
- order by：查詢結果的排序方式
- limit：查詢結果的忽略/限制筆數
> select子句和from子句是select敘述句的必要子句
```sql=
select
    欄位名1,  -- 「*」代表全部column
    ...,
    欄位名N
from
    欲查詢的表格(Table)名
```

> #、{} 內的項目一定要有其中之一，[]內的項目可省略。
> 實際打語法時{}、[]不須打出


- **「*」** 表示列出全部的資料，不管重複與否，預設即為all
- **「distinct」** 表示去除重複資料，只列出不同的資料
- 「distinctrow」 為distinct的同義字

### where子句
> select 找出符合的column (欄)，where找出符合的row(列)

```sql=
select
    ..
from
    tableName
where
    conditions (條件)
```
「條件」必須是可以判斷**true or false**的運算式，EX. 比較運算式、邏輯運算式、SQL特定運算式。

### order by子句 (排序)
- order by 子句撰寫於limit子句之前
- 可設定多個排序依據
- asc/desc 設定排序的方向(遞增/遞減)，預設為asc
- 可依照欄位名、別名、欄位序、運算式進行排序
> 實作時不建議使用欄位序設定排序，如果資料庫欄位順序變換會受到影響
```sql=
select ..
from ..
order by
    欄位名/別名/欄位序/運算式 asc/desc
```

==null 值排序會被視為最小==
```sql    
select * from EMP 
order by 
    SAL + IFNULL(COMM,0) 
-- 獎金 + 薪水的排序，需先將null值替換為0
```
![](https://i.imgur.com/424g6ia.png)

### group by子句 
```sql=
select ...
from ...
[where ...]
group by
    欄位名/欄位序/運算式,
    ...,
    欄位名/欄位序/運算式
    [with rollup]
```
- 欄位名：select子句指定的非聚合函數欄位
- 欄位序：select子句指定的非聚合函數欄位序
- 運算式：select子句指定的非聚合函數運算式
- with rollup：在資料下方加上一筆總計或小計(多個分組依據時)的資料
:::warning
group by後到底開寫什麼？\
select子句所指定的欄位，**去掉聚合函數欄位**，其餘都複製到group by子句
:::

```sql=
-- 每個部門，每數量的獎金金額，各有多少人領取
select
    DEPTNO,
    IFNULL(COMM,0) as COMM,
    COUNT(*) as COUNT    -- 聚合函數
from
    EMP
group by
    DEPTNO,  -- 欄位名
    2        -- select中的欄位序，即INFULL(COMM,0)
```