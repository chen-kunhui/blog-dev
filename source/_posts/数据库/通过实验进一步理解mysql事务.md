---
title: 通过实验进一步理解 mysql 事务
date: 2023-04-16 08:00:00
updated: 2023-04-16 08:00:00
tags:
  - db
  - 事务
categories:
  - db
---
# 实验

## 关于显式事务的一些实验

以下实验都是基于事务隔离级别为`可重复读(REPEATABLE-READ)`的情况下进行的

### 实验一

实验环境：有A客户端，B客户端和C客户端连接到了同一个 mysql 实例，有张user表，表中有 id 和 name 字段，表的初始值如下:

| id | name |
| -- | ---- |
| 1  | 001  |

1. 在 A 客户端运行 `SET autocommit=0;`
  - 问题一: 这时分别在三个客户端运行 `SHOW VARIABLES LIKE 'autocommit';`, 三个客户端 autocommit 字段的值分别是什么?
2. 在 A 客户端运行 `UPDATE user SET name="002" WHERE id=1;`
  - 问题二：这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题三：如果这时 A 客户端运行 `COMMIT`, 三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题四：如果这时 A 客户端运行 `ROLLBACK`, 三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题五：在 B 客户端运行 `UPDATE user SET name="003" WHERE id=1;`，语句会被执行吗？
3. 在 A 客户端运行 `SET autocommit=1;`
  - 问题六：这时分别在三个客户端运行 `SHOW VARIABLES LIKE 'autocommit';`, 三个客户端 autocommit 字段的值分别是什么?
  - 问题七：这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题八：在 B 客户端运行 `UPDATE user SET name="003" WHERE id=1;`，语句会被执行吗？

> 问题一答案：A 输出的是 OFF, B 和 C 输出的都是 ON
> 问题二答案：A 输出的是 002, B 和 C 输出的都是 001
> 问题三答案：A ，B 和 C 输出的都是 002
> 问题四答案：A ，B 和 C 输出的都是 001
> 问题五答案：不会，这时会发现sql像被阻塞了一样，然后会报"ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction"错误
> 问题六答案：A , B 和 C 输出的都是 ON
> 问题七答案：A ，B 和 C 输出的都是 002，说明当将autocommit的值从新设置回1时，会有类似 `COMMIT` 的动作
> 问题八答案：会被正常执行

tips：当将 `SET autocommit=0;` 改为 `BEGIN`, `SET autocommit=1;` 改为 `COMMIT` 时，结果说一样的，也就是当处于事务中时，现象是一样的

### 实验二

实验环境：有A客户端，B客户端和C客户端连接到了同一个 mysql 实例，有张user表，表中有 id 和 name 字段，表的初始值如下:

| id | name |
| -- | ---- |
| 1  | 001  |

1. 在 A 客户端运行 `SET autocommit=0;`
2. 在 A 客户端运行 `UPDATE user SET name="002" WHERE id=1;`
3. 在 B 客户端运行 `UPDATE user SET name="003" WHERE id=1;`
4. 这时通过实验一我们知道，B 客户端的更新语句会被阻塞，需要过一段时间才会报错
  - 问题一: 在这个期间, 这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题二：在这个期间，A 客户端运行 `SET autocommit=1;`, B 客户端会怎么样，这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题三：在这个期间，A 客户端运行 `COMMIT`, B 客户端会怎么样，这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题四：在这个期间，A 客户端运行 `ROLLBACK`, B 客户端会怎么样，这时三个客户端查询 user 表中 name 的值时，分别是什么?

> 问题一答案：A 输出的是 002，B 无法执行查询语句，C 输出 001
> 问题二，问题三，问题四的答案都是，B 客户端的 update 语句会被立即执行，不会报错，从 user 表中查出 name 的值都是 003

tips： 当将 `SET autocommit=0;` 改为 `BEGIN`, `SET autocommit=1;` 改为 `COMMIT` 时，结果说一样的，也就是当处于事务中时，现象是一样的

### 实验三

实验环境：有A客户端，B客户端和C客户端连接到了同一个 mysql 实例，有张user表，表中有 id 和 name 字段，表的初始值如下:

| id | name |
| -- | ---- |
| 1  | 001  |

1. 在 A 客户端运行 `SET autocommit=0;`
2. 在 B 客户端运行 `UPDATE user SET name="002" WHERE id=1;`
  - 问题一：这时三个客户端查询 user 表中 name 的值时，分别是什么?
3. 在 A 客户端运行 `SELECT name FROM user`
4. 在 B 客户端运行 `UPDATE user SET name="001" WHERE id=1;`
  - 问题二：这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题三：这时如果A执行了 `SET autocommit=1;` 或者 `COMMIT` 或者 `ROLLBACK`， 这时三个客户端查询 user 表中 name 的值时，分别是什么?
5. 在 A 客户端运行 `UPDATE user SET name="003" WHERE id=1;`
  - 问题四：这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题五：这时如果A执行了 `SET autocommit=1;` 或者 `COMMIT` 这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题六：这时如果A执行了 `ROLLBACK` 这时三个客户端查询 user 表中 name 的值时，分别是什么?


> 问题一答案：A ，B ，C 输出的都是 002
> 问题二答案：A 输出的是 002 ，B ，C 输出的都是 001
> 问题三答案：A ，B ，C 输出的都是 001
> 问题四答案：A 输出的是 003 ，B ，C 输出的都是 001
> 问题五答案：A ，B ，C 输出的都是 003
> 问题六答案：A ，B ，C 输出的都是 001

从实验可知，如果在某个连接中关闭了自动提交事务，那么最好在执行每次查询前，提交一次事务`COMMIT`或`ROLLBACK`都行，或者重新打开自动提交事务，也就是`SET autocommit=1;`，不然在关闭了自动提交事务下，只要执行过任意sql，不管是查询还是更新，如果没有执行过提交事务动作，那么这时查询出来的结果可能会存在数据不一致的问题

tips： 当将 `SET autocommit=0;` 改为 `BEGIN`, `SET autocommit=1;` 改为 `COMMIT` 时，结果说一样的，也就是当处于事务中时，现象是一样的

### 实验四

实验环境：有A客户端，B客户端和C客户端连接到了同一个 mysql 实例，有张user表，表中有 id 和 name 字段，表的初始值如下:

| id | name |
| -- | ---- |
| 1  | 001  |


1. 在 A 客户端运行 `SET autocommit=0;`
2. 在 A 客户端运行 `UPDATE user SET name="002" WHERE id=1;`
  - 问题一：如果这时在 A 客户端运行 `UPDATE user SET names="003" WHERE id=1;`，这时sql报错，接着运行 `SET autocommit=1;`，这时三个客户端查询 user 表中 name 的值时，分别是什么?
  - 问题二：如果这时在 A 客户端运行 `SELECT names FROM user`，这时sql报错，接着运行 `SET autocommit=1;`，这时三个客户端查询 user 表中 name 的值时，分别是什么?


> 问题二答案: A,B,C  输出的都是 002
> 问题二答案: A,B,C  输出的都是 002

由此可见 `SET autocommit=1;` 同时，也执行了一次 `COMMIT` 动作，但这仅限于处于显式事务状态下，如果当前autocommit=1，用户是通过 `BEGIN` 进入到的事务模式，运行 `SET autocommit=1;`，并不会自动运行 `COMMIT` 动作；
