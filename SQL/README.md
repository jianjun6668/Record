# SQL语句

## 四大操作语句-

1. 删	DELETE

```js
DELETE FROM 表 WHERE 条件
```

2. 增	INSERT

```js
INSERT INTO 表 (字段列表) VALUES(值列表)
```

3. 改	UPDATE

```js
UPDATE 表 SET 字段=值,字段=值,... WHERE 条件
```

4. 查	SELECT

```js
SELECT * FROM 表 WHERE 条件
```


子句：
### WHERE 条件

```js
WHERE name='blue'
WHERE age>18
WHERE age<=18
WHERE age>=18 AND score<60
WHERE cach>100 OR score>10000
```

### ORDER 排序

```js
ORDER BY age ASC/DESC
  ASC-升序(从小到大)
  DESC-降序(从大到小)
```

* 价格(price)升序排序，如果价格相同，再按销量(sales)降序排序

```js
ORDER BY price ASC, sales DESC
```


### GROUP	聚类-合并相同

```js
* 统计每个班人数
ID	class	name
"1"	"1"	"小明"
"2"	"2"	"小红"
"3"	"1"	"小刚"
"4"	"2"	"小华"
"5"	"3"	"小强"
"6"	"3"	"小四"
"7"	"1"	"小刘"
"8"	"1"	"小花"


SELECT * FROM student_table;
ID	class	name
"1"	"1"	"小明"
"2"	"2"	"小红"
"3"	"1"	"小刚"
"4"	"2"	"小华"
"5"	"3"	"小强"
"6"	"3"	"小四"
"7"	"1"	"小刘"
"8"	"1"	"小花"

SELECT * FROM student_table GROUP BY class;
ID	class	name
"1"	"1"	"小明"
"2"	"2"	"小红"
"5"	"3"	"小强"

SELECT class FROM student_table GROUP BY class;
```

* 统计每个班的平均分

```js
>SELECT * FROM student_table;
ID	class	name	score
1	1	小明	34
2	2	小红	98
3	1	小刚	26
4	2	小华	99
5	3	小强	18
6	3	小四	95
7	1	小刘	57
8	1	小花	100

>SELECT * FROM student_table GROUP BY class;
ID	class	name	score
1	1	小明	34
2	2	小红	98
5	3	小强	18

>SELECT class,AVG(score) FROM student_table GROUP BY class;
class	score
1	54.25
2	98.5
3	56.5
```

*每个班级的最高、最低分

```js
>SELECT class,MAX(score),MIN(score) FROM student_table GROUP BY class;
ID	class	name	score
1	1	小明	34
2	2	小红	98
3	1	小刚	26
4	2	小华	99
5	3	小强	18
6	3	小四	95
7	1	小刘	57
8	1	小花	100


*每个人的消费总额
ID	name	price
1	blue	3
2	blue	5
3	张三	28000
4	李四	81000
5	blue	4
6	张三	46000
7	李四	38000
8	赵六	18

SELECT name,SUM(price) FROM sales_table GROUP BY name;

SELECT name,SUM(price) FROM sales_table GROUP BY name ORDER BY SUM(price) DESC;
name	SUM(price)
李四	119000
张三	74000
赵六	18
blue	12

SELECT name,SUM(price) FROM sales_table GROUP BY name ORDER BY SUM(price) ASC;
```

### LIMIT-限制输出

分页：
1.所有数据给前端
2.后台只给一丁点数据

LIMIT 10;	前10条
LIMIT 5,8;	从5开始，要8个

分页：
每页20条

第1页	0,20	0~19
第2页	20,20	20~39
第3页	40,20
第n页	(n-1)*20,20


## 子句之间是有顺序
WHERE GROUP ORDER LIMIT
筛选  合并  排序  限制

```js
SELECT class,COUNT(class) FROM student_table
WHERE score>60
GROUP BY class
ORDER BY COUNT(class) DESC
LIMIT 2;
```
