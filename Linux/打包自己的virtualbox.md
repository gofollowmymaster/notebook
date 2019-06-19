##  Vagrant打包自己的virtualbox

我们搭了一套自己的虚拟环境，当需要在另一台机器上搭建相同的环境时，可以在源主机导出box，再在目的主机导入。

操作流程如下：

1. 查看虚拟机列表(添加全局变量或转到安装目录下)cmd打开运行
    `vboxmanage list vms`
2. 打包
    `vagrant package --base box_name_1503366286622_12977--output ./ubuntu_amd64.box`
    参数说明：

- --base 要打包的虚拟机名称
- --output 打包后的包名
- -- include 打包需要增加的文件，多个文件以逗号分隔
- --vagrantfile 指定vagrantfile文件

如果没有报错，会在当前文件夹下出现你指定名字、后缀为.box的文件，即导出成功。

 ## 导入方式

你来到了另一台电脑，你想把你的工作环境完全的copy一份到这台电脑，接下来就很关键了。

1. 创建一个你要的工作目录，我的是E:\workspace，把公共打好的包放进来

2. 添加box命令

   ~~~
   vagrant box add 你自定义的别名 远端的box地址或者本地的box文件名
   ~~~

3. 初始化工作环境

   ~~~
   vagrant init
   ~~~

   发现你的文件夹中自动生成了一个文件，Vagrantfile。

4. 安装启动

   ~~~
   vagrant up  这个命令在首次执行时为安装+启动，安装好以后只是单纯启动。
   ~~~

5. 登陆

   ~~~
   vagrant ssh
   ~~~

> 个人理解:
>
> 由于Homestead是对vagrant进行定制化的ruby脚本,vagrant相关配置又可以在Homestead.yaml中配置,但现在导入box后没有相关的ruby脚本,若要修改配置需要在vagrantfile文件中修改.

参考[Vagrant Virtual Box 搭建虚拟开发环境](https://www.jianshu.com/p/f63e40d10c08)



[参考链接](https://www.jianshu.com/p/c175f9d84348)



## 直接导出虚拟机

 Virtual Box 

管理->导出虚拟电脑

**开放式虚拟化格式1.0 需要勾选写入Manifest文件选项**



拓展:

box-disk001.vmdk   为虚拟机硬盘文件  比较大

homestead-7.ova   为导出的虚拟计文件