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




