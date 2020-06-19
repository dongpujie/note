如何从**MySQL中删除重复数据，只保留一条**呢？

假设有如下一个数据表：

```
mysql > SELECT * FROM users;

+----+--------+
| id | name   |
+----+--------+
| 1  | google |  <--
| 2  | yahoo  |  <--
| 3  | msn    |  <--
| 4  | google |
| 5  | google |
| 6  | yahoo  |
+----+--------+
```

后面`3`条都是重复的数据，如何用**一条命令**删掉这些重复数据呢？

## 1 查询重复记录数量

查询**全部重复记录**：

```mysql
SELECT * FROM users as a WHERE EXISTS (SELECT  1 FROM users as b WHERE b.name = a.name LIMIT 1, 1) order by name;
```

查询**重复记录和最小id**：

```mysql
SELECT min(id), name, count(*) as c FROM users group by name having c > 1;
```

查询**重复记录和最大id**：

```mysql
SELECT max(id), name, count(*) as c FROM users group by name having c > 1;
```

两个方法的区别就是一个用了`min()`函数，一个用了`max()`函数。

## 2 保留id最小的记录

删除重复记录，**保留id最小的记录**，命令：

```mysql
DELETE n1 FROM users n1, users n2 WHERE n1.id > n2.id AND n1.name = n2.name
```

执行结果就是：

```mysql
mysql > SELECT * FROM users;

+----+--------+
| id | name   |
+----+--------+
| 1  | google |
| 2  | yahoo  |
| 3  | msn    |
+----+--------+
```

## 3 保留id最大的记录

删除重复记录，**保留id最大的记录**，命令：

```mysql
DELETE n1 FROM users n1, users n2 WHERE n1.id < n2.id AND n1.name = n2.name
```

执行结果就是：

```mysql
mysql > SELECT * FROM users;

+----+--------+
| id | name   |
+----+--------+
| 3  | msn    |
| 5  | google |
| 6  | yahoo  |
+----+--------+
```

