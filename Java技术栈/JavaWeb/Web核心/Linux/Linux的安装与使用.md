虚拟机简介

我们实际运行是先安装虚拟机，在虚拟机上运行我们的Linux系统，这样方便我们的学习

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbd2a8ea5f3a39f8b6038132b2d5a79d8.png)

那么虚拟机有什么优点呢？请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb6dc8f635d2ffffb2251f16c06bd7c33.png)

了解了这些之后，我们就安装虚拟机，这里卡顿了整整一天多啊，就为了这个逼虚拟机，最后还是得靠云服务器

在window操作系统中，文件是存放在不同的盘符里的，但是在linux系统下，没有盘符这个概念，所有的东西都被存放到一个根目录下，所有文件都在这个根目录的下面，在这个根目录下，有几个目录是我们要记忆的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0d8c7b99d30340a824986dafcf3bf1f3.png)

首先是etc，其是系统中的配置文件，我们如果将该目录下的文件修改了，可能导致我们的系统打不开

接着是bin、usr、sbin这三个目录，这三个目录里存放的是系统预设执行文件的放置目录，简而言之就是系统的简单命令都是靠这里的目录下的文件来执行的，比方说我们后面要用的一个ls命令，其内部调用的就是usr/bin下的ls目录里的文件内容

最后我们要了解的var目录，这个目录是运行日志的存放目录，非常重要，我们在centos里会运行许多程序，而每个程序都要它的日志文件，这些日志文件都会存放到var/log目录下

接着我们来实际演示一下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE686d7a7af1f31329ed003c19f24719f4.png)

注意这里我们最开始的~代表当前的目录在默认的home目录下，我通过cd / 命令，将我们的目录进入到根目录/中，接着调用ls -l命令打印该目录下所有的文件并展示