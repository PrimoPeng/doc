---
title: mysql数据库先关
tags: 数据库
grammar_cjkRuby: true
---

### mysql锁

#### Innodb存储引擎
加锁的方式：自动加锁。对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加排他锁；对于普通SELECT语句，InnoDB不会加任何锁；当然我们也可以显示的加锁：
InnoDB和MyISAM的最大不同点有两个：一，InnoDB支持事务(transaction)；二，默认采用行级锁。加锁可以保证事务的一致性，可谓是有人(锁)的地方，就有江湖(事务)；我们先简单了解一下事务知识。

 ***只有通过索引条件检索数据，InnoDB才使用行级锁，否则，InnoDB将使用表锁！***
 
 #### MyISAM存储引擎
 
 MyISAM 是默认采用表锁。加锁可以保证事务的一致性，可谓是有人(锁)的地方，就有江湖(事务）
 
表锁，读锁会阻塞写，不会阻塞读。而写锁则会把读写都阻塞。
表锁的加锁/解锁方式：MyISAM 在执行查询语句(SELECT)前,会自动给涉及的所有表加读锁,在执行更新操作 (UPDATE、DELETE、INSERT 等)前，会自动给涉及的表加写锁，这个过程并不需要用户干预，因此，用户一般不需要直接用LOCK TABLE命令给MyISAM表显式加锁。


