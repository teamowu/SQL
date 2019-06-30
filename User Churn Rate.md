# Churn Rate(订户下跌率)
Churn rate is the percent of subscribers that have canceled within a certain period, usually a month.
For a user base to grow, the churn rate must be less than the new subscriber rate for the same period.
To calculate the churn rate, we only will be considering `users who are subscribed at the beginning of the month`. 
The churn rate is the number of these users who cancel during the month divided by the total number:

**Cancellations / Total subscribers**

## SQL查询统计单月用户流失率
```计算任意月份的用户流失率(Churn rate)
# 说明：
- 两个temporary table:enrollmens和status
- status是从enrollments中衍生的temporary table.
- enrollments(2017年前开始订阅,且在1月1日扣费后才取消订阅或没有取消订阅的用户)(代表订阅了2017年1月份的所有用户)

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
