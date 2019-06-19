## SQL高级查询

### 关联表查询

> 数据库中的各个表中存放者不同的数据，往往需要用多个表中的数据组合查询出所需要的信息，即从多个数据表中查询数据。等值多表查询将按照**等值的条件**查询多个数据表中关联的数据。要求关联的多个数据表的某些字段具有相同的属性，即相同的数据类型,宽度和取值范围.使用**where**或**join**(这里使用**join**,语句简洁，逻辑清晰，性能更优)

- natural join  方式

  > NATURAL JOIN子句基于两个表之间有相同名字的所有列
  >
  > 它从两个表中选择在所有的匹配列中有相等值的行
  >
  > 如果有相同名字的列的数据类型不同，返回一个错误

```
select  * from studinfo natural join classinfo
```

- join tablename using(等值字段)

  > 用USING 子句创建连接
  >
  > 如果一些列有相同的名字，但数据类型不匹配，NATURAL JOIN子句能够用USING 子句修改以指定将被用于一个等值连接的列
  >
  > 当有多个列匹配时，用USING 子句匹配唯一的列
  >
  > 在引用列不要使用表名或者别名
  >
  > NATURAL JOIN 和USING子句是相互排斥的

```
select * from studinfo join classinf using (classid)
```

- join tablename on(table1.等值字段=table2.等值字段)

  > 用ON 子句创建连接
  >
  > 1.对于自然连接的连接条件，基本上是带有相同名字的所有列的等值连接
  >
  > 2.为了指定任意条件，或者指定要连接的列，可以使用ON 子句
  >
  > 3.连接条件从另一个搜索条件中被分开

###　子查询

> 在SQL语言中，当一个查询语句嵌套在另一个查询的查询条件之中称为**嵌套查询**，又称为**子查询**。嵌套查询是指在一个外层查询中包含另外一个内层查询，其中外层查询称为主查询，内层查询称为子查询，通常情况下嵌套查询中的子查询先挑选中部分数据，以作为外层查询的数据来源或搜素条件，子查询都是写在圆括号中，允许使用表大式的地方都可以嵌套子查询

1. 查询重修15门以上的学生信息

   ~~~
   select * from studinfo
   where studno in
   (
   select studno
   from studscoreinfo 
   where studscore<60
   having count(*)>15
   //子查询查询出重修15门以上的学生学号
   )
   //跟据获得的学号查找出这些学生的基本信息
   ~~~

   2. 查询同班同姓名的学生成绩信息

      ~~~ 
      select * from studscoreinfo
      where studno in
      (
      select studno from studinfo
      where classid||studname in
      (
      select classid||studname
      from studinfo
      group by classid||studname
      having count(*)>1
      )
      )
      ~~~

      

