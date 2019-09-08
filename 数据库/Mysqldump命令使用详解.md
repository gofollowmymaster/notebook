## Mysqldump命令使用详解

> mysqldump是一个客户端逻辑备份的工作，备份的SQL文件可以在其他MySQL服务器上进行还原。
>
> 权限要求:
>
> 如需备份，则至少需要对该表的select权限，需要备份视图则需要改账户具有SHOW VIEW权限，触发器需要TRIGGER。如需锁表，则不可使用--single-transaction选项。其他权限暂未列出。
>
> 如需还原，则需要对应的执行权限，如create表，则需要对该库的create权限。

### 命令选项:

- -u -p   mysql  账户 密码
- --no-create-db, -n：在给出--databases 或 --all-databases选项时，不带create databasename ，不带use databasename  (mysql的数据库导入导出时更改不同的数据库名字时有用)
- --add-drop-databases：在CREATE DATABASE之前写入drop databases。这个选项通常是配合--all-databases或--database使用，因为CREATE DATABASE语句执行时至少选定一个选项。
-  --add-drop-table:在每一条CREATE TABLE之前写入DROP TABLE命令
- --add-drop-tigger:在每一条CREATE TIGGER之前写入DROP TIGGER。注意：这个选项只有mysqldump作为MySQL集群时提供。
-  --all-databases, -A :备份所有库中的所有表，效果等同于--database 后跟随所有库名。
- --compress，-C：在客户端与服务器都支持的压缩算法中，选择压缩数据进行通信。
-  --database，-B：备份制定数据库。一般来说，mysqld对待name参数时，第一个参数作为数据库名，紧随其后的作为表名。但是在使用这个选项时，会将所有name参数作为数据库名进行备份。在每一个数据库备份前都会添加CREATE DATABASE 与 USE指令。
- --tables：--databases后使用，此选项会使mysqldump将之后参数视为表名。
- -d or -no-data 这个选项使的mysqldump命令不创建INSERT语句。
- -w "WHERE Clause" or -where = "Where clause "  如前面所讲的，您可以使用这一选项来过筛选将要放到 导出文件的数据。
- --ignore-table=db_name.tbl_name：忽略要被备份的表，如果忽略多个则需要使用多次此选项，此选项还可以忽略VIEW。
- --result-file=file_name, -r file_name：直接输出到指定的文件。创建结果文件及其之前的内容覆盖,即使发生错误而生成转储。这个选项应该在Windows上使用,以防止换行符“\ n”字符被转换为“\ r \ n”回车/换行符序列
- --quick, -q：在备份数据量比较大的表时有用。会将数据读入内存，在输出完成之前会存在内存缓冲区。
- -T path or -tab = path 这个选项将会创建两个文件，一个文件包含DDL语句或者表创建语句，另一个文件包含数据。DDL文件被命名为table_name.sql,数据文件被命名为table_name.txt.路径名是存放这两个文件的目录。目录必须已经存在，并且命令的使用者有对文件的特权。　

###　一些选项组的缩写：



--opt 此选项将打开所有会提高文件导出速度和创造一个可以更快导入的文件的选项。

> 使用--opt指令相当于--add-drop-table, --add-locks, --create-options, --disable-keys, --extended-insert, --lock-tables, --quick, a和 --set-charset，以上这些选项是默认的，因为--opt是默认使用的选项。



使用--compact相当于同时使用--skip-add-drop-table, --skip-add-locks, --skip-comments, --skip-disable-keys, 和 --skip-set-charset。

如果需要选择--opt选项组不生效的选项，使用--skip加选项。