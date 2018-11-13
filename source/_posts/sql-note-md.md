---
title: sql 之 case when 用法
date: 2018-11-13 20:37:25
tags:
  - sql
categories:
  - sql
---

## 简介
sql中的case和when可以嵌套到sql语句中，达到多重选择，按条件求和，值映射等效果。

通常写法为
```
case <列名>
when <值> then <满足条件要显示的值>
when <值> then <满足条件要显示的值>
else '都不满足时要显示的值'
end
```

```
case
when 列名 > 10 then <当列中的值大于10时要显示的值>
when 列名 > 20 then <当列中的值大于20时要显示的值>
else '都不满足时要显示的值'
end
```

## 例子

比如有一张 scores 表
```
mysql> select * from scores;
+----+---------+------+
| id | name    | score|
+----+---------+------+
|  1 | zhansan |   22 |
|  2 | zhansan |   40 |
|  3 | zhansan |   33 |
|  4 | zhansan |   55 |
|  5 | lisi    |   44 |
|  6 | lisi    |   50 |
|  7 | lisi    |   30 |
|  8 | lisi    |   40 |
+----+---------+------+
8 rows in set (0.00 sec)

mysql>
```

1. 按 name 分类，求 score 大于30的有几条，大于40的有几条。
```
mysql> select name,sum(case  when score > 30 then 1 else 0 end)gt30,sum(case  when score > 40 then 1 else 0 end)gt40 from scores group by name;
+---------+------+------+
| name    | gt30 | gt40 |
+---------+------+------+
| lisi    |    3 |    2 |
| zhansan |    3 |    1 |
+---------+------+------+
2 rows in set (0.00 sec)

mysql>

```

2. 在查询结果中新加一列，将score为40的显示为四十，非40的显示为其他
```
mysql> select name,score,(case scores.core when 40 then '四十' else '其他' end)备注 from scores;
+---------+------+--------+
| name    | score| 备注   |
+---------+------+--------+
| zhansan |   22 | 其他   |
| zhansan |   40 | 四十   |
| zhansan |   33 | 其他   |
| zhansan |   55 | 其他   |
| lisi    |   44 | 其他   |
| lisi    |   50 | 其他   |
| lisi    |   30 | 其他   |
| lisi    |   40 | 四十   |
+---------+------+--------+
8 rows in set (0.00 sec)

mysql>
```
