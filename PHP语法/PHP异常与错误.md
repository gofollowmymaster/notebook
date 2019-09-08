## PHP异常与错误

PHP中异常时程序运行中不符合预期的情况,与正常流程不同的状况,就是按照正常逻辑不应该出现的状况,这属于业务流程的一种中断,而不是语法错误.

> 这是和java中异常不同,java中异常是唯一的错误报告方式,充PHP角度看即:错误也是以异常形式报告出来.
>
> 两种语言对异常和错误的定意不同



### PHP异常/错误配置

- display_errors=On    On将错误显示到浏览器上

- error_reporting 设置错误报告级别，这个与display_errors和error_log有关，只会将对应的错误记录在错误日志或者打印到浏览器上
- log_errors = On   打开错误日志，默认off不启用
- error_log = /usr/local/php/logs/php_errors.log      写一个绝对路径，作为错误日志的存放目录

### 常用错误级别

~~~
     Fatal Error:致命错误（脚本终止运行）运行到出错代码时才终止脚本
          E_ERROR         // 致命的运行错误，错误无法恢复，暂停执行脚本
          E_CORE_ERROR    // PHP启动时初始化过程中的致命错误
          E_COMPILE_ERROR // 编译时致命性错，就像由Zend脚本引擎生成了一个E_ERROR
          E_USER_ERROR    // 自定义错误消息。像用PHP函数trigger_error（错误类型设置为：E_USER_ERROR）
  
     Parse Error：编译时解析错误，语法错误（脚本终止运行）
          E_PARSE  //编译时的语法解析错误
  
     Warning Error：警告错误（仅给出提示信息，脚本不终止运行）
         E_WARNING         // 运行时警告 (非致命错误)。
         E_CORE_WARNING    // PHP初始化启动过程中发生的警告 (非致命错误) 。
         E_COMPILE_WARNING // 编译警告
         E_USER_WARNING    // 用户产生的警告信息
 
     Notice Error：通知错误（仅给出通知信息，脚本不终止运行）
         E_NOTICE      // 运行时通知。表示脚本遇到可能会表现为错误的情况.
         E_USER_NOTICE // 用户产生的通知信息。
         
   
    注意（notice），这不会阻止脚本的执行，并且可能不一定是一个问题；
    警告（warning），这指示一个问题，但是不会阻止脚本的执行；
    错误（error），这会阻止脚本继续执行（包括常见的解析错误，它从根本上阻止脚本运行）。

    注：&表示并且、~表示非、L表示或者
~~~



### 注册异常/错误处理函数

- set_error_handler()

>  当程序出现错误的时候自动调用此方法，不过需要注意一下两点：第一，如果存在该方法，相应的error_reporting()就不能在使用了。所有的错误都会交给自定义的函数处理。第二，此方法不能处理以下级别的错误：E_ERROR、 E_PARSE、 E_CORE_ERROR、 E_CORE_WARNING、 E_COMPILE_ERROR、 E_COMPILE_WARNING，set_error_handler() 函数所在文件中产生的E_STRICT，该函数只能捕获系统产生的一些Warning、Notice级别的错误。

- set_exception_handler

> 设置默认的异常处理程序，用在没有用try/catch块来捕获的异常，也就是说不管你抛出的异常有没有人捕获，如果没有人捕获就会进入到该方法中，并且在回调函数调用后异常会中止。

- register_shutdown_function

> 捕获PHP的错误：Fatal Error、Parse Error等，这个方法是PHP脚本执行结束前最后一个调用的函数，比如脚本错误、die()、exit、异常、正常结束都会调用，多么牛逼的一个函数啊！通过这个函数就可以在脚本结束前判断这次执行是否有错误产生，这时就要借助于一个函数：error_get_last()；这个函数可以拿到本次执行产生的所有错误。error_get_last();返回的信息：
> 　　[type]           - 错误类型
> 　　[message] - 错误消息
> 　　[file]              - 发生错误所在的文件
> 　　[line]             - 发生错误所在的行

### 异常处理和异常嵌套



### PHP7中的Throwable







### 参考

- [再谈PHP错误与异常处理](https://www.zhaoyafei.cn/content.html?id=170641242245)