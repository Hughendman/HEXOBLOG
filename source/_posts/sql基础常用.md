---
title: sql基础常用
categories: sql
tags: sql
keywords: sql
abbrlink: 4f05048c
date: 2018-07-06 16:15:00
---

### 日期格式化(DATE_FORMAT() 函数)

```
DATE_FORMAT(date,format)

DATE_FORMAT(NOW(),'%Y%m%d')//20180704
DATE_FORMAT(NOW(),'%b %d %Y %h:%i %p')
DATE_FORMAT(NOW(),'%m-%d-%Y')
DATE_FORMAT(NOW(),'%d %b %y')
DATE_FORMAT(NOW(),'%d %b %Y %T:%f')

```

### 设置自增日期

类型：timestamp

默认：CURRENT_TIMESTAMP


### CREATE

```

SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for user_info
-- ----------------------------
DROP TABLE IF EXISTS `user_info`;
CREATE TABLE `user_info` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `tel` varchar(255) DEFAULT NULL,
  `address` varchar(255) DEFAULT NULL,
  `date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user_info
-- ----------------------------
INSERT INTO `user_info` VALUES ('1', 'yinxs', '24', '18811428452', '北京', '2018-07-04 16:21:09');
INSERT INTO `user_info` VALUES ('2', 'yinxs', '24', '18811428452', '北京', '2018-07-04 16:21:56');

```

```

SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for option_info
-- ----------------------------
DROP TABLE IF EXISTS `option_info`;
CREATE TABLE `option_info` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) DEFAULT NULL,
  `option` varchar(255) DEFAULT NULL,
  `time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of option_info
-- ----------------------------
INSERT INTO `option_info` VALUES ('1', '1', '选项一', '2018-07-04 16:29:03');


```

### INSERT

```
INSERT INTO user_info (name,age,tel,address) VALUES ('yinxs','24','18811428452','北京')

```

### left join

```
SELECT * FROM user_info AS a LEFT JOIN option_info AS b ON(b.user_id = a.id)

```

### HAVING

```
SELECT * FROM user_info HAVING SUM(age) > 1000

```

### inner join

同join

```
SELECT * FROM user_info AS a INNER JOIN option_info AS b ON (b.user_id = a.id)

```

### right join 

```
SELECT * FROM user_info AS a RIGHT JOIN option_info AS b ON(b.user_id = a.id)

```

### update

```
UPDATE user_info SET tel = '18811428453' WHERE id = 1

```

### DELETE

```
DELETE FROM user_info WHERE id = 2

```

### DISTINCT

去重

```
SELECT DISTINCT age FROM user_info

```

### LIKE

```
以y开始
SELECT * FROM user_info WHERE name LIKE 'y%'

SELECT * FROM user_info WHERE name LIKE '%x%'

以s结束
SELECT * FROM user_info WHERE name LIKE '%s'

```

### ORDER BY

```
正序
SELECT * FROM user_info AS a LEFT JOIN option_info AS b ON(b.user_id = a.id) ORDER BY a.id 
逆序
SELECT * FROM user_info AS a LEFT JOIN option_info AS b ON(b.user_id = a.id) ORDER BY a.id DESC

```

### function(SQL Server)

```

AVG(column)	返回某列的平均值
COUNT(column)	返回某列的行数（不包括NULL值）
COUNT(*)	返回被选行数
COUNT(DISTINCT column)	返回相异结果的数目
FIRST(column)	返回在指定的域中第一个记录的值（SQLServer2000 不支持）
LAST(column)	返回在指定的域中最后一个记录的值（SQLServer2000 不支持）
MAX(column)	返回某列的最高值
MIN(column)	返回某列的最低值
SUM(column)	返回某列的总和

```

### GROUP BY

```
SELECT * FROM user_info AS a LEFT JOIN option_info AS b ON (b.user_id = a.id) ORDER BY age

```

### UNION  && UNION ALL

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

```
SELECT * FROM user_info UNION SELECT * FROM user_info

SELECT * FROM user_info UNION ALL SELECT * FROM user_info

```

### AND & OR 

```
SELECT * FROM user_info WHERE id = 1 AND age = 24

SELECT * FROM user_info WHERE id = 1 OR age = 24

```

### in

```
SELECT * from user_info WHERE id in (1,2,3)

SELECT * from user_info WHERE id not in (1,2,3)

```

### case when zhen

```
--简单case函数
case sex
  when '1' then '男'
  when '2' then '女’
  else '其他' end
--case搜索函数
case when sex = '1' then '男'
     when sex = '2' then '女'
     else '其他' end

```
实例：

```
SELECT (CASE when uid = 1 THEN '男' WHEN uid = 2 THEN '女' ELSE '其他' END)性别  FROM `session` 

```

### Round


```
SELECT ROUND(column_name,decimals) FROM table_name


column_name	必需。要舍入的字段。
decimals	必需。规定要返回的小数位数。
```


舍入为最接近的整数

```
SELECT ProductName, ROUND(UnitPrice,0) as UnitPrice FROM Products

```


### 标准函数中的日期函数

```
1、day(date_expression)

返回date_expression中的日期值

 

2、month(date_expression)

返回date_expression中的月份值

 

3、year(date_expression)

返回date_expression中的年份值

```


### BETWEEN ... AND

操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

```
select * from tj_md WHERE date BETWEEN 20180705 AND 20180706

```