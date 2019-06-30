# Churn Rate(订户下跌率)
- 流失率是指在一定时期内（通常是一个月）取消订阅的用户占所有订阅用户(包含取消用户)的百分比。
- 要保证用户群增长，用户流失率必须低于同一时期的新用户流入率（retention rate)。
- 为了计算用户流失率，我们只会考虑“在月初已订阅的用户”。
- 流失率是指在月内取消的用户数除以总数：**Cancellations / Total subscribers**

## 思考
- 熟悉公司业务
- 公司经营了几个月？ 需要足够几个月的信息来计算客户流失率？
- 公司有哪些用户群？
- 自公司成立以来，整体流失趋势是怎样的？
- 比较用户群之间的流失率。
- 根据客户流失率，公司应该关注哪些用户群并针对不同客户群进行不同的扩展营销手段？

## 单月用户订阅流失率
```
计算任一月份的用户流失率(Churn rate)
# 说明：
- 两个temporary table:enrollmens和status
- status: 是从enrollments中衍生的temporary table.
- enrollments: (2017年前开始订阅,且在1月1日扣费后才取消订阅或没有取消订阅的用户)(代表订阅了2017年1月份的所有用户)

WITH enrollments AS
(SELECT * FROM subscriptions
 WHERE subscription_start < '2017-01-01'
AND (
(subscription_end >= '2017-01-01')
OR (subscription_end IS NULL)
)),
### status(CASE 1:只登记该月取消订阅的人数(结果为1) CASE 2:只登记该月活跃用户(结果为1))
status AS
### CASE 1:取消订阅在1月份后或无取消订阅标记为0，否则标记为1*/
(SELECT
 CASE 
 		WHEN (subscription_end > '2017-01-31')
 			OR (subscription_end IS NULL) THEN 0
 			ELSE 1
		END as is_canceled,
 ### CASE 2:开始订阅在1月份前，且在1月1日扣费后才取消订阅或没有取消订阅的用户标记为1，否则标记为0.
 CASE WHEN subscription_start < '2017-01-01'
 				AND (
        (subscription_end >= '2017-01-01')
        OR (subscription_end IS NULL)
        )	THEN 1
 			ELSE 0
		END as is_active
 FROM enrollments
)
SELECT 1.0 * SUM(is_canceled)/
SUM(is_active)
FROM status;
```

## 多月用户订阅流失率
```
计算3个月份的用户订阅流失率(Churn rate)
# 说明：
- 四个temporary table: months, cross_join, status，status_aggregate。
- months: 创建该临时表格为所需查询的n个月的第一天和最后一天进行堆叠连接。(3 * 2)。
- cross_join: 创建该临时表格将months表格和用户订阅信息所在表格subscriptions(1000*3)进行交叉连接获得(3000*5)的表格。
- status: 创建该临时表格将cross_join中信息进行统计判断，判断每个用户在相应月中是否活跃或是否取消。
- status_aggregate:创建该临时表格对status中信息进行统计，统计各个月活跃用户的总数和取消订阅的用户总数。

WITH months AS (
  SELECT 
    '2017-01-01' AS first_day, 
    '2017-01-31' AS last_day 
  UNION 
  SELECT 
    '2017-02-01' AS first_day, 
    '2017-02-28' AS last_day 
  UNION 
  SELECT 
    '2017-03-01' AS first_day, 
    '2017-03-31' AS last_day
), 
cross_join AS (
  SELECT *
  FROM subscriptions
  CROSS JOIN months
), 
status AS (
  SELECT 
    id, 
    first_day AS month, 
    CASE
      WHEN (subscription_start < first_day) 
        AND (
          subscription_end > first_day 
          OR subscription_end IS NULL
        ) THEN 1
      ELSE 0
    END AS is_active, 
    CASE
      WHEN subscription_end BETWEEN first_day AND last_day THEN 1
      ELSE 0
    END AS is_canceled 
  FROM cross_join
), 
status_aggregate AS (
  SELECT 
    month, 
    SUM(is_active) AS active, 
    SUM(is_canceled) AS canceled 
  FROM status 
  GROUP BY month
) 
SELECT
  month, 
  1.0 * canceled / active AS churn_rate 
FROM status_aggregate;
```

