# UTM参数简介
UTM的全名为Urchin Tracking Module。通过设定好的UTM参数，即可追踪网站/活动的流量来源，不同流量来源所占的比例等。
假设我们在网站上或文章附带UTM参数的链接，那么我们即可获知每一篇公众号文章所带来的网页浏览量（PV）、独立访客数（UV）、新访问用户数、单次访问时长、访问深度、跳出率等指标。

## UTM五大参数
详情介绍如下:
- 来源(utm_source)：用来标识流量来源网站、搜索引擎或其他来源。示例：utm_source=baidu
- 媒介(utm_medium)：用来标识媒介，比如电子邮件或每次点击费用。示例：utm_medium=cpc
- 名称(utm_campaign)：用来标识特定的产品推广活动。示例：utm_campaign=summer_spread
- 关键字(utm_term)：常见于付费关键字广告所使用的字词或是连结名称/图片的替代文字。示例：utm_term = web+analysis
- 内容(utm_content)：使用utm_content区分指向同一个网址的广告或链接。示例：utm_content=logolink或utm_content=textlink

## SQL在网站分析中的实战应用

```
#查询用户第一次访问时的UTM参数
WITH first_touch AS (
    SELECT user_id,
       MIN(timestamp) AS 'first_touch_at'
    FROM page_visits
    GROUP BY user_id)
SELECT ft.user_id,
   ft.first_touch_at,
   pv.utm_source
FROM first_touch AS 'ft'
JOIN page_visits AS 'pv'
   ON ft.user_id = pv.user_id
   AND ft.first_touch_at = pv.timestamp;
   
# 查询用户最后一次访问时的UTM参数
WITH last_touch AS (
    SELECT user_id,
      MAX(timestamp) AS 'last_touch_at'
    FROM page_visits
    GROUP BY user_id)
SELECT lt.user_id,
    lt.last_touch_at,
    pv.utm_source
FROM last_touch AS 'lt'
JOIN page_visits AS 'pv'
  ON lt.user_id = pv.user_id
  AND lt.last_touch_at = pv.timestamp；
```

## 案例：营销归因
**业务背景：**CoolTShirts是一个销售各种服装的B2C网站。最近，该网站开展了一些营销活动，以增加网站访问的用户和提高购买量。通过营销归因来判断渠道的价值，他们希望可以勾画出客户旅程：从初次访问网站到完成购买。通过利用UTM参数来挖掘出优化其营销活动的方案。

现有数据库：**page_visits**

|  column    | Description|
|------------|:---------:|
|  user_id   | A unique identifier for each visitor to a page|
|  timestamp | The time at which the visitor came to the page|
|  page_name | The title of the section of the page that was visited
|  utm_source| Identifies which site sent the traffic (i.e.,google, newsletter, or facebook_ad)|
|utm_campaign| Identifies the specific ad or email blast (i.e., june-21-newsletter or memorial-day-sale)|

### 提出问题
- 1.CoolTShirts有多少种产品推广的活动？有多少条渠道进行这些活动的推广？每条渠道对应投放的推广活动是什么？
- 2.CoolTShirts的浏览页面分别为哪些？
- 3.什么是客户旅途？
- 4.每个产品推广活动与用户的首次互动分别为多少次？每个产品推广活动与用户的最终互动分别为多少次？
- 5.有多少用户访客完成购买？
- 6.每个产品推广活动引流至最终互动完成购买的次数？
- 7.如果重新投放5个新的产品推广活动？该如何选择？

```
# 问题1：
SELECT COUNT(DISTINCT utm_campaign) FROM page_visits;

SELECT COUNT(DISTINCT utm_source) FROM
page_visits;

SELECT DISTINCT utm_campaign,utm_source FROM
page_visits;

# 问题2：
SELECT DISTINCT page_name FROM page_visits;


# 问题4：
WITH first_touch AS (
    SELECT user_id,
      MIN(timestamp) as first_touch_at
    FROM page_visits
    GROUP BY user_id),
ft_attr AS (
  SELECT ft.user_id,
         ft.first_touch_at,
         pv.utm_source,
         pv.utm_campaign
  FROM first_touch ft
  JOIN page_visits pv
    ON ft.user_id = pv.user_id
    AND ft.first_touch_at = pv.timestamp
)
SELECT ft_attr.utm_source,
       ft_attr.utm_campaign,
       COUNT(*)
FROM ft_attr
GROUP BY 1, 2
ORDER BY 3 DESC;


# 问题4：
WITH last_touch AS (
    SELECT user_id,
      MAX(timestamp) as last_touch_at
    FROM page_visits
    GROUP BY user_id),
lt_attr AS (
  SELECT lt.user_id,
         lt.last_touch_at,
         pv.utm_source,
         pv.utm_campaign
  FROM last_touch lt
  JOIN page_visits pv
    ON lt.user_id = pv.user_id
    AND lt.last_touch_at = pv.timestamp
)
SELECT lt_attr.utm_source,
       lt_attr.utm_campaign,
       COUNT(*)
FROM lt_attr
GROUP BY 1, 2
ORDER BY 3 DESC;

SELECT COUNT(DISTINCT user_id)
FROM page_visits
WHERE page_name = '4 - purchase';

# 问题5
WITH last_touch AS (
SELECT user_id,
MAX(timestamp) AS last_touch_at
FROM page_visits
WHERE page_name = '4 - purchase'
)
SELECT * FROM last_touch limit 10;


# 问题6
WITH last_touch AS (
    SELECT user_id,
    MAX(timestamp) as last_touch_at
    FROM page_visits
  	WHERE page_name = '4 - purchase'
    GROUP BY user_id),
lt_attr AS (
  SELECT lt.user_id,
         lt.last_touch_at,
         pv.utm_source,
         pv.utm_campaign
  FROM last_touch lt
  JOIN page_visits pv
    ON lt.user_id = pv.user_id
    AND lt.last_touch_at = pv.timestamp
)
SELECT lt_attr.utm_source,
       lt_attr.utm_campaign,
       COUNT(*)
FROM lt_attr
GROUP BY 1, 2
ORDER BY 3 DESC;
```



