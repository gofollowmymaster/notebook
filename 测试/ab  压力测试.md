## ab  压力测试



### 参数详解

~~~
-n  requests Number of requests to perform
在测试会话中所执行的请求个数（本次测试总共要访问页面的次数）。默认时，仅执行一个请求。
~~~

~~~
-c concurrency Number of multiple requests to make
一次产生的请求个数（并发数）。默认是一次一个。
~~~

~~~
-t timelimit Seconds to max. wait for responses
测试所进行的最大秒数。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。
~~~

~~~
-p postfile File containing data to POST
包含了需要POST的数据的文件，文件格式如“p1=1&p2=2”.使用方法是 -p 111.txt 。 （配合-T）
~~~

~~~
-T content-type Content-type header for POSTing
POST数据所使用的Content-type头信息，如 -T “application/x-www-form-urlencoded” 。 （配合-p）
~~~

~~~
-v verbosity How much troubleshooting info to print
设置显示信息的详细程度 – 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。
~~~

~~~
-C attribute Add cookie, eg. -C “c1=1234,c2=2,c3=3” (repeatable)
-C cookie-name=value 对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。此参数可以重复，用逗号分割。
提示：可以借助session实现原理传递 JSESSIONID参数， 实现保持会话的功能，如
-C ” c1=1234,c2=2,c3=3, JSESSIONID=FF056CD16DA9D71CB131C1D56F0319F8″ 。
~~~



### 示例:

- 对智新水控设备出水接口进行压测

~~~
./ab -n 1000 -c 100   -p postdata.txt  -T "application/json"  https://veinhw-api-beta.rocketbird.cn/Vein/BathWare/deviceWaterFlow
~~~

- 对需要登陆的接口进行压测

~~~

~~~

