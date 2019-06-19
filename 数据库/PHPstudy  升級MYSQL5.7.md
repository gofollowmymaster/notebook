## PHPstudy  升級MYSQL5.7

一、备份原来 phpStudy 中 MySQL 安装目录

二、把下载的 MySQL 压缩文件解压至 phpStudy 下的 MySQL目录，复制 my-default.ini ，重命名为 my.ini。

打开 my.ini，找到 #basedir 处编辑：

```
`basedir=D:/phpStudy/MySQL``datadir=D:/phpStudy/MySQL/data`
```

三、把 MySQL 安装路径添加至系统环境变量

四、在 cmd 下进入 MySQL 的 bin 目录（我的是 D:/phpStudy/MySQL/bin），执行：

```
`mysqld --initialize`
```

初始化数据库

五、安装服务：

```
`mysqld -install`
```

启动服务：

```
`   ` `net start MySQL`
```

六、此时登入 MySQL 报错：

```
`C:\Users\dell>mysql -uroot -p``Enter password: ****``ERROR 1045 (28000): Access denied ``for` `user ``'root'``@``'localhost'` `(using password: YES)`
```

尝试修改 root 用户密码：

打开 my.ini，找到 [mysqld]，在下面添加：

```
`skip-grant-tables`
```

此时重启Mysql;使用 root 账号，密码处按回车即可登录。

修改密码：

```
`   ` `mysql>update mysql.user set authentication_string=password(``'new_password'``) where user=``'root'` `and` `Host =``'localhost'``mysql> ALTER USER USER() IDENTIFIED BY ``'news_password'``;`
```

刷新权限：

```
`FLUSH` `PRIVILEGES;`
```



注释掉 my.ini 中刚才添加的

```
`skip-grant-tables`
```

重新登录。

此时查看 mySQL 版本：

```
`mysql> select version();``+-----------+``| version() |``+-----------+``| 5.7.17  |``+-----------+`
```



### 附录:

#### phpstudy安装服务

打开phpstudy找到服务管理-->mysql-->安装服务！

#### 使用phpstudy启动mysql始终失败

解决办法 很简单，打开CMD命令 输入 sc delete MySQL ;

> 因为自己创建的MYSQL服务和phpstudy创建的服务MYSQLa冲突



### 参考:

[phpStudy中MySQL版本升级到5.7.17方法](<http://phpstudy.php.cn/jishu-php-3131.html?tdsourcetag=s_pctim_aiomsg>)



