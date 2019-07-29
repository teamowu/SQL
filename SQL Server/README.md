# SQL Server简介
- 属于关系型数据库管理系统(RDBMS)
- 以SQL为基础作交互得标准编程语言。
- Transact-SQL,又名T-SQL，用于SQL SERVER转悠的内置编程结构。

**发展历程(版本重大变化)**


**SQL Server的体系结构(如图)** 


## 两个主成分
由上图可看出，sql server的两大组成部分为：
- **Database Engine**
  - **Relational Engine（简称RE）**
    - 功能：执行查询（queries）
    - 说明：基于查询从SE中请求数据并处理结果。
    - 备注：部分RE包含查询处理，内存管理，线程和任务管理，缓存管理和分布式查询处理。
  - **Storage Engine（简称SE）**
    - 功能： 负责存储和检索来自存储系统（如磁盘和SAN）的数据。
- **SQLOS（SQL Server Operation System）**
  - SQLOS提供许多操作系统服务，如内存和I / O管理。其他服务包括异常处理和同步服务。
  
## SQL Server的服务与工具
- For Data Management
  - Sql Server Integration Services(SSIS)
  - Sql Server Data Quality Services
  - Sql Server Master Data Services
- For developing Databases
  - Sql Server Data Tools
- For managing, deploying and monitoring Databses
  - **Sql Server Management Studio(SSMS)**
- For Data Analysis
  - Sql Server Analysis Services（SSAS）
- For Data Visualiztion
  - Sql Server Reporting Services（SSRS)
- For Machine Learning(Based on R)
  - ML Services(after the version of Sql Server 2016)
  
## SQL Server的版本
刚开始接触的时候，被若干个版本搅和得一脸茫然，虽然付费的肯定是最好的，但是拿来练练手，应该
如何选择呢？
- 免费
  - Sql Server Developer edition：适用于数据库开发和测试
  - Sql Server Expression：适用于大小高达10GB的磁盘存储容量的小型数据库。
  - Sql Server Standard Edition：具有Enterprise Edition的部分功能集，并且在服务器上限制了可配置的处理器核心和内存的数量
- 付费
  - Sql Server Enterprise：集成sql Server所有的功能与特征。
  
# SQL语法补充
### OFFSET FETCH
- 功能：返回跳过指定行数后所需要的指定的若干行。
- 注意：`OFFSET` and `FETCH`要和`ORDER BY`一起使用，否则会报错。
- 备注：`OFFSET`和`FETCH`子句比实现`TOP`子句更适合实现查询分页解决方案。
  
```
ORDER BY column_list [ASC |DESC]
OFFSET offset_row_count {ROW | ROWS}
(FETCH {FIRST | NEXT} fetch_row_count {ROW | ROWS} ONLY)
```
- The `OFFSET` clause specifies the number of rows to skip before starting to return rows from the query. The `offset_row_count` can be a constant, variable, or parameter that is greater or equal to zero.
- The `FETCH` clause specifies the number of rows to return after the `OFFSET` clause has been processed. The `offset_row_count` can a constant, variable or scalar that is greater or equal to one.
- The `OFFSET` clause is mandatory while the `FETCH` clause is optional. Also, the `FIRST` and `NEXT` are synonyms respectively so you can use them interchangeably. Similarly, you can use the `FIRST` and `NEXT` interchangeably.

### TOP
- 功能：返回指定行数或其行数百分比

```
SELECT TOP (expression) [PERCENT]
    [WITH TIES]
FROM 
    table_name
ORDER BY 
    column_name;
```
- `expression`:
Following the TOP keyword is an expression that specifies the number of rows to be returned. The expression is evaluated to a float value if PERCENT is used, otherwise, it is converted to a BIGINT value.
- `PERCENT`:
The PERCENT keyword indicates that the query returns the first N percentage of rows, where N is the result of the expression.
- `WITH TIES`:
**表示用作排序的指标值相同时，返回所有相同指标值所在的行**
The `WITH TIES` allows you to return more rows with values match the last row in the in the limited result set. Note that WITH TIES may cause more rows to be returned than you specify in the expression.

### LIKE
功能：逻辑操作符。定义字符满足某一条件或形式。
备注：常用于`WHERE`,`SELECT`,`UPDATE`,`DELETE`。
```
column | expression LIKE pattern [ESCAPE escape_character]
```
5种常见通配符：
- `%`:any string of zero or more characters.
- `_`: any single character.
- `[list of characters]`: any single character within the specified set.
- `[character-character]`: any single character within the specified range.
- `^`: any single character not within a list or a range.





