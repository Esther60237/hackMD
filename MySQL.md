###### tags: `MySQL`

資料庫常用物件
==

- DBMS： 資料庫管理系統，DataBase Management System，安裝在DB Server上
- DB：資料庫，在DBMS裡的單位。在一個DBMS內可建立多個DB
- Schema：亦可說是資料庫的藍圖，描述資料庫由哪些物件組成，屬邏輯上(虛擬)的階層。
- Table：關聯式資料庫中的主角，以欄與列方式儲存資料，所有資料皆儲存在資料表中

> 大陸的欄與列和台灣相反，查詢簡體中文相關文件時須注意

> MySQL的Schema等價於DB

SQL
==
Structured Query Language，操作關聯式資料庫的語言。
#### SQL語句
- 分成5種子語言：DML、TCL、DQL、DDL、DCL
- 每個子語言又分成數個敘述 (Staement)
- 每個敘述又分成多個子句 (Clause)

#### 撰寫SQL敘述的好習慣
- 指令預設不區分大小寫，但應保持一致風格
- 敘述用分號結尾
- 可寫成多行，每個子句換新行
- 用縮排增加可讀性
- 單行註解使用 # 或 -- 開頭
- 多行註解使用 /* 註解文字 */



# DQL (Data Query Language)
用以查詢Table內的資料，僅查詢，不會改變資料

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
>「條件」必須是可以判斷true or false的運算式，EX. 比較運算式、邏輯運算式、SQL特定運算式。

### 運算子
#### 比較運算子 Comparison Operators
- <>、! 不等於

#### 邏輯運算子 Logical Operators
- and：且 return TRUE if **both** conditions are **TRUE**
- or：或 return TRUE if **either** conditions **is** TRUE
> EX. 要找出生日期在5月份或6月份的人
> 
1. Month  = 5 or Month = 6 (O)
2. Month = 5 or 6  (X) 
3. Month= 5 and Month = 6 (X)
:::warning
第3項的意思為同時符合5月和6月的人，但「要判斷的目標」為月份，不會有人同時符合5月和6月
:::

- not：反向 return TRUE if conditions **is FALSE**

#### SQL特定運算子
- between A值 and B值：介於或等於，等同於「>= A值 and <= B值」。 
```sql=
-- 顯示所有「SAL介於1000~2000」的資料
select * from EMP
where
    SAL between 1000 and 2000;
```
:::warning
！數字順序須由小到大，左到右排列
:::
- in(A, ... , N)：任一 Match and of a list of values，**參數不能為null**，實用狀況常用 IFNULL(判斷欄位, 要替換的內容)含式將null代換。
:::success
EX. 要找出符合null 或 0
- 判斷欄位 is null **or** 判斷欄位 = 0
  
EX. 要找出符合null 或 Month等於5或6
- Month is null **or** Month **in**(5,6);
  
EX. 要找出薪資不等於1100和1300和1500
- SAL != 1100 **and** SAL != 1300 **and** SAL != 1500
- SAL **not in**(1100, 1300 ,1500)
:::
- like：模糊比對，支援萬用字元，如下表。
![](https://i.imgur.com/qdkzvtN.png)
- is null：如果是null 便返回TRUE，等同於 <=> null，但不等於 = null
- rlike：支援Regular Expression的比對
## 空值 (null)
- 意義為沒有值，不等於0，不等於空字串，也不等於自己
- insert資料時，若無指定某欄位之值時，就會放入null



如果資料值是null，可以用 IFNULL 篩選出後代換需要的內容
```
select 
    ENAME as '姓名', 
    SAL + IFNULL(COMM,0) '收入', 
    NOW() as '統計點' 
from 
    EMP
```







## 字串串接
- MySQL的「+」運算子雖可用在字串上，但意義非串接，而是試著將字串轉數字，然後再做加法運算
- 字串串接需使用函數CONCAT()





