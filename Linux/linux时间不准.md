

## Linux时间不准

1. 修改系统时区

   ~~~
   rm -f /etc/localtime
   ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
   ~~~

2. 安装ntpdate

   ~~~
   yum install -y ntp
   ~~~

3. 网络时间同步

   ~~~
   ntpdate -u ntp.api.bz
   ~~~

   

