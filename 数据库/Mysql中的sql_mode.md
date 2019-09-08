## Mysql中的sql_mode

> 定义了你MySQL应该支持的sql语法，对数据的校验等等

### SQL语法支持类

1. **ONLY_FULL_GROUP_BY：**

   对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么将认为这个SQL是不合法的，因为列不在GROUP BY从句中

   因为有only_full_group_by，所以我们要在MySQL中正确的使用group by语句的话，只能是select column1 from tb1 group by column1(**即只能展示group by的字段，其他均都要报1055的错**)

2. **ANSI_QUOTES** 启用 ANSI_QUOTES 后，不能用双引号来引用字符串，因为它被解释为识别符，作用与 ` 一样。设置它以后，`update t set f1="" ...`，会报 Unknown column '' in 'field list 这样的语法错误。

3. **PIPES_AS_CONCAT** 将 `||` 视为字符串的连接操作符而非 或 运算符，这和Oracle数据库是一样的，也和字符串的拼接函数 CONCAT() 相类似。

4. **NO_TABLE_OPTIONS** 使用 `SHOW CREATE TABLE` 时不会输出MySQL特有的语法部分，如 `ENGINE` ，这个在使用 mysqldump 跨DB种类迁移的时候需要考虑。

5. **NO_AUTO_CREATE_USER** 字面意思不自动创建用户。在给MySQL用户授权时，我们习惯使用 `GRANT ... ON ... TO dbuser` 顺道一起创建用户。设置该选项后就与oracle操作类似，授权之前必须先建立用户。5.7.7开始也默认了。

###  数据检查类

2. **NO_ZERO_IN_DATE：**

   在严格模式，不接受月或日部分为0的日期。如果使用IGNORE选项，我们为类似的日期插入'0000-00-00'。在非严格模式，可以接受该日期，但会生成警告。

3. **NO_ZERO_DATE：**

   在严格模式，不要将 '0000-00-00'做为合法日期。你仍然可以用IGNORE选项插入零日期。在非严格模式，可以接受该日期，但会生成警告

4. **ERROR_FOR_DIVISION_BY_ZERO：**

   在严格模式，在INSERT或UPDATE过程中，如果被零除(或MOD(X，0))，则产生错误(否则为警告)。如果未给出该模式，被零除时MySQL返回NULL。如果用到INSERT IGNORE或UPDATE IGNORE中，MySQL生成被零除警告，但操作结果为NULL

5. **NO_AUTO_CREATE_USER：**

   防止GRANT自动创建新用户，除非还指定了密码。

6. **NO_ENGINE_SUBSTITUTION：**

   如果需要的存储引擎被禁用或未编译，那么抛出错误。不设置此值时，用默认的存储引擎替代，并抛出一个异常

### 组合类

> 为方便配置,支持的几个模式组别.

1. **ANSI模式**

   宽松模式，更改语法和行为，使其更符合标准SQL。对插入数据进行校验，如果不符合定义类型或长度，对数据类型调整或截断保存，报warning警告。对于本文开头中提到的错误，可以先把sql_mode设置为ANSI模式，这样便可以插入数据，而对于除数为0的结果的字段值，数据库将会用NULL值代替。

   ~~~
   Equivalent to REAL_AS_FLOAT, PIPES_AS_CONCAT, ANSI_QUOTES, IGNORE_SPACE.
   ~~~

   

2. **TRADITIONAL严格模式**

   当向mysql数据库插入数据时，进行数据的严格校验，保证错误数据不能插入，报error错误，而不仅仅是警告。用于事物时，会进行事物的回滚。 注释：一旦发现错误立即放弃INSERT/UPDATE。如果你使用非事务存储引擎，这种方式不是你想要的，因为出现错误前进行的数据更改不会“滚动”，结果是更新“只进行了一部分”。

   ~~~
   Equivalent to STRICT_TRANS_TABLES, STRICT_ALL_TABLES, NO_ZERO_IN_DATE, NO_ZERO_DATE, ERROR_FOR_DIVISION_BY_ZERO, NO_AUTO_CREATE_USER, and NO_ENGINE_SUBSTITUTION.
   ~~~

3. [`ORACLE`](https://dev.mysql.com/doc/refman/5.6/en/sql-mode.html#sqlmode_oracle)

   ~~~
   Equivalent to PIPES_AS_CONCAT, ANSI_QUOTES, IGNORE_SPACE, NO_KEY_OPTIONS, NO_TABLE_OPTIONS, NO_FIELD_OPTIONS, NO_AUTO_CREATE_USER.
   ~~~

4. POSTGRESQL

   ~~~
   Equivalent to PIPES_AS_CONCAT, ANSI_QUOTES, IGNORE_SPACE, NO_KEY_OPTIONS, NO_TABLE_OPTIONS, NO_FIELD_OPTIONS.
   ~~~



###　Strict SQL Mode

> Strict mode controls how MySQL handles invalid or missing values in data-change statements such as [`INSERT`](https://dev.mysql.com/doc/refman/5.6/en/insert.html) or [`UPDATE`](https://dev.mysql.com/doc/refman/5.6/en/update.html). A value can be invalid for several reasons. For example, it might have the wrong data type for the column, or it might be out of range. A value is missing when a new row to be inserted does not contain a value for a non-`NULL` column that has no explicit `DEFAULT` clause in its definition. (For a `NULL` column, `NULL` is inserted if the value is missing.) Strict mode also affects DDL statements such as [`CREATE TABLE`](https://dev.mysql.com/doc/refman/5.6/en/create-table.html).

1. STRICT_TRANS_TABLES：

   只对支持事务的表启用严格模式

2.  STRICT_ALL_TABLES



#### Homestead修改mysql配置

/etc/mysql/my.ini   中包含  !include  的两个目录  

~~~
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/
~~~

可在里面配置,注意配置文件权限不能为777  修改为644,不然Mysql会忽略改配置文件的配置

可以修改的地方在

~~~
 /etc/mysql/mysql.conf.d/mysqld.cnf
 #添加一行
 sql_mode=STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
~~~

重启mysql

~~~
service mysql restart
~~~





