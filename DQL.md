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
### having 子句
- 同where子句一樣是過濾資料的條件，不過是針對**群組/聚合函數運算出的結果**作過濾\
EX. COUNT(*) > 10

- where子句不能寫在having後面，因為select敘述句順序不能調換\
select > from > where > group by > having > order by > limit
```sql=
select ...
from ...
[where ...]
group by ...
having
    條件 conditions
```
### from 子句
- 用來指定資料的來源，若來源有多個，需用**join on**語法
- 業界在select欄位的別名通常會加as，但在資料來源不會加
```sql=
select ...
from
    資料來源A 別名A
    [連結方式 inner/left/right] join 資料來源B 別名B  
    -- 連結方式可不寫，預設為inner
        on 條件
```
:::info
要使「資料A + 資料B + 資料C +資料D」，應該把前述的資料包在一起後join後面資料，例如：\
( (A join B) join C ) join D
:::

#### 連結方式- 交叉連結 cross join 無條件連結
>業界不常有需要使用的情境，老師也不建議用)
```sql=
select *
from
    EMP join DEPT

-- EMP裡的14筆資料*DEPT裡的4筆資料
-- 把所有組合配合一次
```
#### 連結方式- 內部連結 inner join
- 自然連結 Natural Join\
依兩資料來源的相同欄位名連結
>容易產生資料錯誤，老師不建議用
```sql=
-- Table EMP、DEPT各有一欄位 DEPTNO
-- 依此連結 
select * 
from 
    EMP 
    natural join DEPT
```
- **相等連結 equal join**\
on語法(條件式)後有等於(=)運算子
:::warning
如果選擇的欄位只在其中一個Table內出現，連結後該筆資料**不會**出現
:::
```sql=
select *
from
    EMP e
    join DEPT d
        on e.DEPTNO = d.DEPTNO
```
- 不相等連結 non-qual join\
on語法後無等於(=)運算子
:::warning
select下選擇的欄位須明確定義是來自哪個table，否則會出錯
:::
```sql=
-- 連結條件為 員工薪水 
--「介於」 最低薪水 和 最高薪水
select 
    e.ENAME,  -- 不可只寫ENAME
    e.SAL,
    sl.LEVEL  -- 「sl.*」可顯示sl表的全部欄位
from
    EMP e
    join SAL_LEVEL sl
        on e.SAL between sl.SAL_MIN and sl.SAL_MAX
```

- **自我連結 self join**\
資料來源A和資料來源B是同一個物件或子查詢
```sql=
-- 列出員工姓名 和 主管姓名
-- 在同一個資料表中列出每一位員工的主管姓名
-- 類似vlookup
select 
    e1.*,
    e2.ENAME
from
    EMP e1
    join EMP e2
        on e1.MGR = e2.EMPNO
```

#### 連結方式- 外部連結 outer join 
- 左側連結 left join\
依左側資料來源為主查詢右邊資料，連結後會**含有**判斷條件結果為null值的資料
- 右側連結 right join\
依右側資料來源為主查詢左邊資料，連結後會**含有**判斷條件結果為null值的資料
```sql=
select  
    d.DNAME, 
    COUNT(EMPNO)   
from 
    EMP e 
    right join DEPT d 
        on e.DEPTNO = d.DEPTNO 
group by 
    d.DEPTNO

-- 如果d.DEPTNO有的資料group在e.DEPTNO內沒有 
-- 資料count(*)會計算錯誤，所以要使用count(EMPNO)
```
![](https://i.imgur.com/RXLhizk.png)

![](https://i.imgur.com/4XIl67z.png)

:::danger
inner join **不會**包含條件判斷值null的資料\
left/right join **會**包含條件判斷值null的資料，但不是所有，看是以左側或右側表單為主
:::
## Exercise
### where & order by子句
列出薪資不介於1000到2000元的員工之姓名和薪資
```sql=
select * from EMP 
where 
    SAL < 1000 or SAL > 2000;
```

列出到職日為1981年的員工之姓名、職稱、到職日，並依到職日遞減排序
```sql=
select * from EMP 
where 
    HIREDATE like '1981%' 
order by 
    HIREDATE;
```

列出薪資超過2000元 且 部門編號為10或30 的員工之姓名、薪資，並依序取別名為"EMPLOYEE_NAME"、"SALARY"
```sql=
select 
    ENAME as EMPLOYEE_NAME, 
    SAL as SALARY, 
    DEPTNO 
from EMP 
where 
    SAL > 2000 and DEPTNO in(10,30);
```

列出有獎金的員工之姓名、薪資、獎金，並排序，排序依據為薪資加上獎金
```sql=
select 
    ENAME as 姓名, 
    SAL as 薪資, 
    COMM as 獎金, 
    SAL + COMM as 所得
from EMP 
where 
    SAL + COMM is not null 
order by 
    SAL + IFNULL(COMM,0);
```

列出員工姓名最後一個字是"S"的員工之姓名、職稱
```sql=
select 
    ENAME as 姓名, 
    JOB as 職稱
from EMP 
where 
    ENAME like '%S';
```

列出職稱為CLERK、SALESMAN，且薪資不等於1100、1300、1500的員工之姓名、職稱、薪資
```sql=
select 
    ENAME as 姓名, 
    JOB as 職稱, 
    SAL as 薪資
from EMP 
where 
    JOB in('CLERK','SALESMAN')  
    and SAL != 1100 
    and SAL != 1300 
    and SAL != 1500; 
-- 另一種寫法
 where 
     JOB in('CLERK','SALESMAN')  
     and SAL not in(1100,1300,1500);
```

列出獎金大於薪資1.05倍的員工之姓名、薪資、獎金
```sql=
select 
    ENAME as 姓名, 
    SAL as 薪資, 
    COMM as 獎金
from EMP 
where 
    COMM > (SAL * 1.05);
```
### group by子句
請列出公司每年需發出薪資總和 (不含獎金)
```sql=
select 
    sum(SAL) * 12 as 薪資總和
from 
    EMP
```

請列出公司平均月薪
```sql=
select 
    sum(SAL) / count(ENAME) as 平均月薪
from 
    EMP
```

請列出各部門編號、部門每月發出薪資總和，並依部門編號遞增排序
```sql=
select 
    DEPTNO, 
    sum(SAL) as Total
from 
    EMP 
group by 
    DEPTNO 
order by 
    DEPTNO
```

請列出職稱、各職稱平均薪資、各職稱人數
```sql=
select 
    JOB, 
    count(ENAME) as 人數, 
    sum(SAL) / count(ENAME) as 平均薪資 
from 
    EMP 
group by 
    JOB
```

請列出部門編號、部門最低薪資、部門最高薪資
```sql=
select 
    DEPTNO, 
    max(SAL), 
    min(SAL) 
from 
    EMP 
group by 
    DEPTNO
```

請列出到職年、到職年當年人數
```sql=
select 
    year(HIREDATE) as year, 
    count(ENAME) as 人數
from 
    EMP 
group by 
    year
```
### from子句
請列出所有員工的員工編號、姓名、職稱、部門編號及部門名稱
```sql=
select 
    e.EMPNO, 
    e.ENAME, 
    e.JOB, 
    e.DEPTNO, 
    d.DNAME 
from 
    EMP e 
    join DEPT d 
	on e.DEPTNO = d.DEPTNO
```

請列出所有部門編號為30且職稱為"SALESMAN"之部門名稱、員工姓名、獎金
```sql=
select 
    d.DNAME, 
    e.ENAME, 
    e.COMM 
from  
    EMP e 
    join DEPT d 
	on e.DEPTNO = e.DEPTNO 
where 
    e.DEPTNO = 30 
    and e.JOB = 'SALESMAN'
```

請列出薪水等級為"B"的所有員工之員工編號、員工姓名、薪資
```sql=
select 
    e.EMPNO, 
    e.ENAME, 
    s.LEVEL 
from  
    EMP e 
    join SAL_LEVEL s 
	on e.SAL between s.SAL_MIN and s.SAL_MAX 
where 
    LEVEL = 'B'
```

請列出部門編號、部門名稱及部門人數
```sql=
select  
    e.DEPTNO as 部門編號, 
    d.DNAME as 部門人數, 
    count(e.ENAME) as 部門人數 
from  
    EMP e 
    join DEPT d 
	on e.DEPTNO = d.DEPTNO 
group by 
    部門編號
```

請列出每個主管之姓名、管理人數、發出平均薪資，
並依管理人數遞減、主管姓名遞增 排序
```sql=
select 
    e2.ENAME as MGR_NAME, 
    count(*) as 管理人數, 
    -- 也可以用e1.ENME
    -- 但都在同一張資料表所以選擇*計算總資料筆數即可  
     
    avg(e1.SAL) 
    -- 以e1(員工，不重複的資料)為主加入e2(主管，重複的資料)
    -- e2的姓名就代表了e1(員工)的主管(有重複值) 
    -- 如果用e2.SAL是後方資料，代表的是主管「個人」的薪資(且有重複值)
    -- 所以不應該使用來計算部門平均值 
from 
    EMP e1 
    join EMP e2 
    -- 如果有人沒有主管，後方加入的資料值為null  
    -- inner join不含有判斷條件結果為null值的資料 
    -- 若使用left/right join就會含有判斷條件結果為null值的資料 
    on e1.MGR = e2.EMPNO 
    -- 用「主管編號」找出「每個員工」對應到的主管
    -- e的員工編號欄重複值(vlook概念)  
group by 
	MGR_NAME 
order by 
	管理人數 desc, 
    MGR_NAME 
```
:::warning
如果from...on條件式寫的是「e1.EMPNO = e2.MGR」，意思是用「員工編號」找出「誰是主管」，e1的員工編號欄有重複值，所以select需換成e1.ENAME as MGR_NAME、avg(e2.SAL)
:::
![](https://i.imgur.com/SyCMdAM.png)
