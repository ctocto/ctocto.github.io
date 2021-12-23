---
title: 实用的 MySql Sql语句记录
date: 2021-12-23 19:30:00
tags: [Sql,MySql]
categories: 饭碗(技术)
---

### 按日分组查询
场景：统计一个月内每天各停车场的停车数量

```sql
SELECT
	t.parking_name,
	substr( t.gmt_create, 1, 10 ) AS date,
	COUNT( * ) AS count 
FROM
	parking_flow t
WHERE
  t.gmt_create LIKE '2021-12%' 
GROUP BY
	substr( t.gmt_create, 1, 10 ), # 使用 substr 函数
	t.parking_id 
ORDER BY
	date,
  t.parking_name;
```

### 查询 N 天前的数据
场景：查询3天以前创建的订单

```sql
SELECT
	* 
FROM
	order 
WHERE
	NOW( ) > SUBDATE( gmt_modify, INTERVAL - 3 DAY );
```

### 查询结果值转化
场景：性别显示汉字

```sql
SELECT
	name,
	age,
CASE # 使用控制流语句
	gender 
	WHEN '1' THEN '男' 
	WHEN '2' THEN '女'
  ELSE '其他' 
	END AS zh_gender
FROM
	user;
  
# 或者

SELECT
	name,
	age,
CASE WHEN gender = '1' THEN '男' 
	WHEN gender = '2' THEN '女'
  ELSE '其他' 
	END AS zh_gender
FROM
	user;
```

### 查询几点到几点的数据
场景：查询0点到6点的订单

```sql
SELECT
	*
FROM
	order t
WHERE
	t.gmt_create LIKE '2021-09%' 
	AND t.gmt_create >= CONCAT( substr( t.gmt_create, 1, 10 ), ' ', '00:00:00' ) # CONCAT 字符串拼接函数
	AND t.gmt_create <= CONCAT( substr( t.gmt_create, 1, 10 ), ' ', '06:00:00' );
```

### 时长范围查询
场景：查询停车时长为两个小时、3个小时、5个小时、8个小时内的分别的车辆数

CASE 的一种使用场景

```sql
SELECT
CASE
	WHEN
		( t.billing_duration ) <= 7200 THEN '0-2' 
			WHEN ( t.billing_duration ) <= 14400 THEN '2-4' 
			WHEN ( t.billing_duration ) <= 21600 THEN '4-6' 
			WHEN ( t.billing_duration ) <= 28800 THEN '6-8' 
			WHEN ( t.billing_duration ) <= 36000 THEN '8-10' 
			WHEN ( t.billing_duration ) <= 43200 THEN '10-12' 
			WHEN ( t.billing_duration ) <= 50400 THEN '12-14' 
			WHEN ( t.billing_duration ) <= 57600 THEN '14-16' 
			WHEN ( t.billing_duration ) <= 64800 THEN '16-18' 
			WHEN ( t.billing_duration ) <= 72000 THEN '18-20' 
			WHEN ( t.billing_duration ) <= 79200 THEN '20-22' 
			WHEN ( t.billing_duration ) <= 86400 THEN '22-24'
      WHEN ( t.billing_duration ) > 86400 THEN '24~' 
		END AS biid,
		COUNT( * ) 
	FROM
		parking_order t 
	WHERE
		t.gmt_create >= '2021-12-01 00:00:00' 
		AND t.gmt_create <= '2021-12-31 23:59:59'
    AND t.billing_duration > 0
	GROUP BY
		biid 
ORDER BY
	t.billing_duration DESC # 停车时长倒序
```
