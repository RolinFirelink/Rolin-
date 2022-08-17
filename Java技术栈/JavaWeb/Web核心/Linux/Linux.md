# 初识Linux

在学习Linux操作系统之前，我们应该要先学习操作系统本身的作用，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE757c6babb8422008afd30d555677fd08.png)

比方说我们的windows操作系统，一个操作系统必然有用户，而且操作系统下有很多应用程序，接着应用程序，用户想要执行应用程序的对应操作，是要告诉操作系统的，操作系统再根据用户的命令对硬件下达对应的指令最终达成用户的需求

接下来我们来介绍下主流的操作系统

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE29d5bfcac290c252c138d12aa8d1c250.png)

操作系统可以总分成四大类，分别是

- 桌面操作系统

- 服务器操作系统

- 嵌入式操作系统

- 移动设备操作系统

桌面操作系统：桌面操作系统主要分三大类，分别是Window系列，macOS，Linux，这三种，其中Window是目前用户范围最普遍的操作系统，macOS系统则主要出现在苹果系列的产品中，其中细节优化非常好，但是里面使用软件要收费，而且还有很多软件不适配。Linux操作系统则更加少人用

嵌入式操作系统：虽然名字听起来非常高大上，其实简单来说类似于我们家里的智能微波炉，智能手表一类里面的操作系统就是嵌入式操作系统，其中常用的只有一种，就是Linux

服务器操作系统：这个操作系统就是专门管理服务器的，主要使用的操作系统有两种，分别是Linux和Windows Server，但一般我们都使用Linux，因为后者要收费

移动设备操作系统：移动设备的操作系统主要是Unix，在其下演化出了两大分类，分为时Linux和ios系统，其中Linux下又延伸出了两大分类，分别是Android和华为鸿蒙

我们可以看到四大操作系统分类里，都有Linux系统，在这里我们就能够一窥Linux系统的重要性

## Linux系统的历史

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe50606a9c18035b8a75ad4466c50a1dd.png)

左人的领域是前两个，后面的历史都是后面那位的

## Linux系统特点

适记性的内容，了解下就好了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3f31508cae1ff629b9b8f6c7c4f10fe5.png)

这里面类Unix的意思其实基于Unix操作系统的基础之上通过改进完善得来的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5fff055b3d61b717102eb90503144036.png)

## Linux系统与其他系统的区别

首先是Linux操作系统与Unix操作系统的区别

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4d50754818775680068910c9ffad0423.png)

Linux是开源的，是免费的，用户的自主权高，而且对各种不同的硬件都具有适配性，同样的系统能在你的电脑上运行，在其他的电脑上也可以

而Unix则反过来，是要收费的，用户的自主权几乎为0，而且对硬件不具有较广的适配性

接着我们来说说Linux操作系统与Unix操作系统的区别

- 系统界面，首先window的系统界面都是微软公司提供给我们的，用户几乎只能使用微软定好的，而Linux系统由于是开源的，所以每个版本的图形化界面都不一样，不过们某些基本命令还是保持一致的

- 驱动程序，window的驱动程序种类繁多更新勤快，用户可以很快就找到适配的驱动程序并安装，而Linux系统的驱动程序则比较少，一般都是由志愿者自己研发并由官方小组进行收编发布的，初次使用Linux系统配置驱动文件要花费的时间往往比较多

- 系统使用，windows操作系统是只使用图形界面进行操作的，非常简单便捷，而Linux操作系统也类似，但不同的是Linux操作系统是有命令行界面的，用户可以同学学习命令行语法来对使用对应命令来执行相应的操作

- 学习成本，window操作系统的由于更新频率高，技术更新迭代快，底层代码复杂等原因，深入学习困难不说，还随时面临技术淘汰的风险，而在Linux中这些问题就不复存在，因为Linux系统底层代码简单，学习起来比较容易，而且更新频率不快，因此不用太担心技术淘汰的问题

- 软件使用，在window操作系统中，大多数商用软件是要收费的，但是其种类非常多，也非常好用。而Linux系统中则是不需要收费的，但是其种类是真的少

- 开放性，window的操作系统的内部是不开放的，用户不可以随意修改底层的代码，而Linux系统则不同，用户可以随意修改底层代码

- 文件格式不同

- 免费与收费，个人使用window和Linux都是免费的，但是企业使用window是要收费的，而使用Linux则是免费的

- 技术支持，由于window使用的人比较多，因此其技术支持比较好，而Linux只在服务器上使用，其技术支持差，深入使用需要有足够深入的版块知识

- 安全性，由于window不开放源代码，因此其在后台运行程序是用户是发现不了的，而Linux系统则反过来，由于开源，任何修改都无所遁形，因此其安全性远胜windows

- 开源，window不开源，Linux开源

- 使用习惯，window主要使用图形化界面来操作，而不是命令行，而Linux中则反过来，主要使用命令行操作，图形化界面比较少用

- 软件和支持，window几乎可以使用所有的软件和应用，而在Linux系统中则少之又少

最后我们可以做一个总结，Windows更加适合家庭个人使用，Linux更适用于企业服务器使用

## Linux发行版本



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3d9fba205324221d020e4ba0df428e15.png)

Linux非常火热，因此其发行商也发行版本也非常多，其中最著名的两个就是Ubuntu和CentOS，前者是各种Linux版本里图形化界面做得最好的，而后者则是我们要学习的版本

## CentOS历史

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6fb07e3d658f1300af3b5f696db7fd5d.png)

中间的版本要收费，因此发明了后面的版本

接着来看看CentOS的特点有三，一是主流，二是免费，三是更新方便

# Linux的安装与使用

## 虚拟机简介

我们实际运行是先安装虚拟机，在虚拟机上运行我们的Linux系统，这样方便我们的学习

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbd2a8ea5f3a39f8b6038132b2d5a79d8.png)

那么虚拟机有什么优点呢？请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb6dc8f635d2ffffb2251f16c06bd7c33.png)

了解了这些之后，我们就安装虚拟机，这里卡顿了整整一天多啊，就为了这个逼虚拟机，最后还是得靠云服务器，所以我们这里其实不是用虚拟机，其实是用的云服务器

## Linux系统介绍

在window操作系统中，文件是存放在不同的盘符里的，但是在linux系统下，没有盘符这个概念，所有的东西都被存放到一个根目录下，所有文件都在这个根目录的下面，在这个根目录下，有几个目录是我们要记忆的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE686d7a7af1f31329ed003c19f24719f4.png)

首先是etc，其是系统中的配置文件，我们如果将该目录下的文件修改了，可能导致我们的系统打不开

接着是bin、usr、sbin这三个目录，这三个目录里存放的是系统预设执行文件的放置目录，简而言之就是系统的简单命令都是靠这里的目录下的文件来执行的，比方说我们后面要用的一个ls命令，其内部调用的就是usr/bin下的ls目录里的文件内容

最后我们要了解的var目录，这个目录是运行日志的存放目录，非常重要，我们在centos里会运行许多程序，而每个程序都要它的日志文件，这些日志文件都会存放到var/log目录下

接着我们来实际演示一下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f4abc05303e61b55d8cb3e900c31c2c.png)

注意这里我们最开始的~代表当前的目录在默认的home目录下，我通过cd / 命令，将我们的目录进入到根目录/中，接着调用ls -l命令打印该目录下所有的文件并展示

# 系统设置与命令

再成功安装完了linux之后，我们紧接着来学习Linux的相关的系统设置和命令

## 用户命令

- Linux命令 --- 账号管理，具体的创建命令如下图，这里要注意的是创建用户可能会出现用户权限不足的情况，此时我们要切回到最高权限的账户进行对应的用户创建。其次是我们的密码设置必须是8位数的英文和数字的组合而且不能够是回文。这里的用户名指的就是我们要进行对应的用户进行的管理的用户名，各个单词间要有空格。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd4b98364c7c0be270dd387f6cf1367bf.png)

修改用户需要使用使用usermod语法，但是这里我们的选项是必须要选择的，那么这里有多少个选项呢？我们只要在对应的窗口下直接输入usermod，然后按下回车就可以查看到了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE14e487d34c4e095dfa709bfcde89ca50.png)

这里我们先简单使用下-l，这个语法，使用这个语法可以修改对应的用户名称，那么我们的修改语法不是usermod -l user1，而是usermod -l aaa user，其中usermod -l代表我们要调用对应的语法，然后aaa表示我们设置的用户名称，而user代表被修改的用户名

当然，这里也同样会存在权限的问题，我们可以通过快捷键ctrl+d，快捷退出用户，达到我们拥有最高权限的用户然后修改用户名，值得一提的是，如果我们一直退出，那么最后会将整个进程包括linux都退出掉。当然实际上我们还是用我们的老方法切换用户也行

最后我们要将的就是删除方法userdel，其用法是userdel 要删除的用户名，这是最简单最常用的用法，当然，实际上其还有其他用法，但是用的比较少，我们了解下就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE91c5fbd3cd10a641796a22db0bbdf8c5.png)

其中-r的意思是删除一个用户的同时还会将该用户对应的所有文件一起删除掉，而-f的意思是强制删除，不管这个用户是谁的，都要删除，那么我们就可以利用这两个方法来让我们的删除变得更加彻底，其语法是userdel -r -f 要删除的用户名

## 用户组命令

- Linux用户组命令，我们可以在我们的Linux中创建多个用户并且给用户分组，这样便于我们的管理，比方说我们如果就9个用户，我们可以将其分为不同的三组，如果我们需要将其中的一组用户修改权限，我们只要调用组的对应命令就可以了，就不用一个个用户修改权限了，对比下来就方便不少

用户组的方法一共有以下四个

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc30967b8bf3a2b55fa2bbb1ee1dddc72.png)

首先是创建用户组groupadd方法，其使用方式和创建用户的使用方式差不多，都是groupadd 用户组名就完了

接着是修改用户组groupmod方法，先来看看这个方法内部的各种不同版本

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE16b3613b96b039cec3ff32bdc7b03dcf.png)

虽然有很多，但是我们这里只做了解，来试试这个-n，也就是修改用户组的名字就可以了，其使用语法和修改用户名字的语法是一样的，groupmod -n 设置的用户组名 要修改的用户组的名字

接着是查询用户所属组的groups方法，其语法是 groups 用户名，值得一提的是如果用户不属于任何组，那么默认显示的组名就是其本身，我觉得也可以理解为其自成一个组

最后是删除用户组的groupdel方法，没什么打不同的，不多谈

- Linux命令 --- 管理组内成员，这里我们主要使用的命令是gpasswd，该命令可以将用户添加或将成员从组中删除。内部还有更加具体的方法，不过我们先记住删除和添加就可以了，添加是-a，删除是-r

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE334b9a2afba076f472ca9539964e8bfb.png)

在命令行里的方法展示如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE94928c49afa3cf376395da0bfa4a37e2.png)

将用户添加到用户组的命令语法是gpasswd -a 用户名 用户组名，每执行一次这个命令就可以将一个用户添加到一个组中，当然，我们应该要实现先将这个组和用户给生成，不然都没有那怎么添加是吧。这里提一嘴，用快捷键↑，可以直接回到上一个执行的命令，善用这个快捷键能够有效提高我们的命令执行速度，不用重复输入命令

## 日期管理命令

- Linux日期管理相关命令，我们的日期管理的相关命令主要是靠date命令实现的，该命令下一共有以下子命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6b896c13de9c3e58024a130efa6febff.png)

首先，如果我们只输入date，然后回车，那么其会显示Linux系统下的系统时间，CST是北京时间

而date -d "日期格式的字符串" 字符串里的日期和时间是什么，输入这个命令之后其就会显示什么

而date -s "日期格式的字符串"其作用是将Linux的时间设置成我们想设置的时间，字符串的内容是日期格式的字符串

## 切换与显示用户

- Linux命令 --- 显示用户，这里我们主要依赖logname命令实现，其作用是显示登录的账号的信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE61dde0160329c787900118ee03435f25.png)

其实虽然图上挺多东西的，但实际上我们只学logname一个就完了，调用这个会显示当前用户的对应信息

- Linux命令 --- 切换用户，这个不用多说了，su 用户名，就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE20500775db77a1dacdb6f17d422fcfd4.png)

不过值得一提的是，这个命令之下有很多子命令，不过都是了解内容，我们只要记住一个最常用的切换用户的命令就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE712f5ab4a73839f28f68c53fd61820a1.png)

如果我们哪天忘了具体的方法有哪些，那也不要紧，我们只要调用su命令里的help命令就可以显示出su内部的所有方法了，其语法是su --help

还有一个要讲解的子命令是-c，其意思是向shell传递一条命令，其作用是切换用户执行命令，执行完毕之后再变回原来的使用者。这有什么用呢？不急，我们先看下面的语句，su -c ls root，这个语句是什么意思呢？要理解这个语句的意思，我们要先理解ls的作用是什么，ls的作用是展示目录内的所有东西，那么这整个语句的意思就是，先切换到root用户展示其内部的所有东西，接着再把用户切换回来，只需要一条语句就可以完成这些东西，但如果是普通方法的话，我们就得搞很多次了

## id命令

- Linux命令 --- ID命令，id命令下有很多子命令，不过我们这里都只做了解，因为id命令最经常使用的还是单独使用的时候，直接输入id，就会显示当前用户的详细信息了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7ec99c1772da2add929258cc15666dca.png)

比方说我们输入id命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEefc84f305cd85c0bfde6c39eed1fd76d.png)

这里显示的uid就是用户itcast自己的id，而gid是itcast的组id，后面的组具体是显示itcast在哪个组

## sudo命令

- Linux命令 --- sudo命令，这个命令的主要作用是提高普通用户的操作权限

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5ecc964fc53b2a041e7446ca9e047c82.png)

比方说我们可以输入以下命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8af336ade8b90b768c2c110ce0067827.png)

像上面第一个命令sudo ls，本来ls对于子用户而言是无法执行的，因为没有足够的权限，但是这里先用了sudo之后就可以执行了，这是因为sudo命令先提高了子用户的权限，不过需要输入密码（如果有密码的话）。而后一句的代表的意思是是直接指定root来执行ls命令

## 进程相关命令

- Linux进程相关命令

- Top命令，同样也是子命令很多，但是我们只要学习几个就够了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8926fbca667a5ee5a076ada66bcf367d.png)

top命令最常用的命令方式是直接输入top，会显示出一个类似于window窗口的进程表，而且还在不断更新，按Q可以退出，这个命令可以实时监控所有进程的状态

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe9b782547a70ea3c6dc96e8393ba03dc.png)

这里的PID是每个进程的id，每个进程的id都是不一样的，我们可以通过这些id来定位到某个进程或是对某个进程进行指定的操作

User指的是这个进程时属于哪个用户的

PR表示进程的优先级，NI也是，不过NI的优先级是数字越小，优先级越高，而PR我们还不知道

VIRT指的是当前进程使用虚拟内存的容量，S表示当前进程的运行状态，S是sleep，代表睡眠状态，R是running，代表运行状态

COMMAND表示命令的名字

这个整体和命令管理器是很像的，具体对比我们会发现其真的是非常类似的

由于其是实时监控的，因此我们要输入其他命令的话必须要先按Q退出才做得到

然后我们输入命令top -c，可以看到如下界面

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdcf1b6c1545bcaaeb3c4f48891f53a28.png)

可以看到COMMAND行变多了，其实这个命令就是让COMMAND行显示完整的命令，我的理解是带上了路径

如果只要监控指定的一条进程，我们可以调用top -p 对应进程的id，这样就能监控我们指定的进程了

top命令以后我们会使用得比较多，但最常用的命令还是我们最开始学习的直接输入监控整个进程的命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8365b3db7f8543b31f70ea5d92ad5399.png)

## PS命令

- Linux命令 --- PS命令，这里同样也是只有几个我们需要学习的子命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6fac58fb1b405d826704dcc3cc7f686c.png)

ps命令与top命令不同的是，ps命令只会显示出进程的一个时间点的进程信息，也就是不是动态的，类似于这样

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2b5495d0924cdd9b90b308485b56532f.png)

比方说在itcast用户下只有两个进程在运行，那么此时其就只产生显示这两个进程信息

如果我们希望它显示所有进程的信息，我们可以输入ps -A，但这种方式展示出来的信息不够全面，如果我们希望展示更加全面的信息，我们可以输入ps -ef，这样就会显示所有进程的完整信息了。如果我们希望只显示某个用户的所有进程的信息，我们还可以输入ps -u 用户名，这样就会只显示某个用户的所有进程的信息了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1bfef6bafdbc02e90afd123120eadb15.png)

## kill命令

- Linux命令 --- kill命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0dae0c5e65ec882d2418c35f6448d675.png)

虽然看起来挺复杂的，但我们其实可以简单记忆，简而言之，我们可以直接用这样的语法kill pid，来结束某个被指定的进程，如果我们中间加入的编号，那么就相当于我们指定了进程结束的方式

比方说我们可以用kill 1111，代表我们要结束pid为1111的进程，而有些进程可以用这种方式无法结束，无法结束我们可以强制结束，强制结束的命令是 kill -9 pid，kill的编号其实很多，总共有64个，但我们最常用的其实就只有-9的这个强制结束的子命令

如果我们想要结束某个用户的所有进程，我们可以用下面的语法kill -9 $(ps -ef | grep 用户名)，这里代表的意思是先将所有的进程打印，然后过滤出我们所需要的进程然后全部强制结束。我们也可以用killall -u 用户名，这样的形式来结束一个用户的所有进程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE087b801e66c7eb401224aa032b938935.png)

# Linux的目录管理

## 关机以及相关命令

- Linux命令 --- 关机命令，这里主要依赖的语法是shutdown

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6b050f9f7210b9e2ae89a7aeba352fd6.png)

直接使用shutdown命令，在centos6以前的版本中是立刻关机，而在centos7中（也就是我们现在的版本里），是延迟一分钟之后关机，我们可以在这个时间里给出取消关机的命令，语法是shutdown -c

如果我们要立刻关机的话，我们应该要使用shutdown -h now，这个命令会令其立刻关机

我们还可以用这样的语法shutdown +数字 "警告信息字符串，由用户输入"，这个命令会使我们的Linux系统在我们指定的分钟数之后关机，并且给出警告信息，这个警告信息是由用户自行设定的

如果我们想要令其再指定时间里重启并给出警告信息的话，可以用shutdown -r +数字 "警告信息字符串"

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4fe48c19b6b6110dd5651e2889ca2ea9.png)

- Linux命令 --- 重启命令，这个我们只记住使用reboot可以直接重启就完了，其他的只做了解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc931f4cb58786f8ff43eda605ef164a7.png)

- Linux命令 --- who命令，这里主要依赖的是who命令，该命令可以显示当前登录系统的用户，有什么用呢？比方说我们关机了，但如果还有人在使用服务器的话，这样就会给另外一个人造成影响，但我们先查看当前登录系统的用户，确定了没人登入之后再关机，这里就要用到who命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc982fe243fd430b479a8737c69b403ef.png)

直接输入who，会在界面中显示当前登入的用户的名称，线路，时间，和ip地址

输入who -H，同样会在界面中显示上述内容，不过上面多了注释行，用于解释每行对应的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2ef354f8bd4a80c02129d2ea4d5b16cc.png)

## 时间校正命令

- Linux命令 --- timedatetl命令，这里主要依赖的已经是timedatetl，这个命令的作用是用于校正服务器的时间或时区。我们之前学习过date命令，该命令也有同样的作用，但这个命令是手动输入的，难免会有误差，如果我们想要准确的时间，我们就应该利用timedatetl命令从ntp时间服务器中获取到准确的时间

直接输入timedatetl，可以展示出以下信息（输入timedatetl status也有同样的效果）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2acb6eea13dbd391f12982627e25d79e.png)

Local time意为本地时间，其显示的内容是当前操作系统的本地时间，这里显示的是CST北京时间

Universal time意为世界时间，其显示的是世界时间，其与北京时间有8个小时的时差

RTC time意为主板时间，有些情况下操作系统本身是不能记录时间的，这时就需要从主板上去获取时间，这个内容只做了解

Time zone意为时区，这里显示的内容是我们当前操作系统所在的时区

NTP enabled代表NTP协议是否开启，no表示没有开启

NTP synchronized代表NTP时间是否同步，no代表没有同步

接着我们继续来讲其他子命令

输入timedatectl list-timezones，可以获取到当前操作系统可以设置的所有时区，按q可以退出

输入timedatectl set-timezone "时区"，可以手动设置操作系统的时区，但是注意这里字符串内的时区内容要与系统中给出的设置时区的字符串保持一致才可设置

输入timedatectl set-ntp false可以禁止操作系统使用ntp协议，反之使用true则是令操作系统使用ntp协议

在禁用了ntp协议之后，我们就可以用命令timedatectl set-time "yyyy-mm-dd hh:mm:ss"的格式来设置时间

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa3f206009893e47255b13bc72d56dbcb.png)

最后科普几个小概念

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE97216de4616094d1aafb6416725ca6af.png)

- Linux命令 --- clear命令，这个命令就不多谈了，用多了，值得一提的是这个命令不是清空屏幕，而是将内容顶到屏幕上面去让我们的界面变得清爽

## 目录管理命令

- Linux命令 --- 目录常用命令，目录常用的命令主要有以下八个，我们接下来一个个介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4a2860c83173e3881585db132c09b1b2.png)

### ls命令

首先是ls命令，ls命令的主要作用是列出目录，其就相当于是windows系统中打开文件夹看到的目录以及文件的明细

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdb92f66891dd39159b0be46e0709be7b.png)

直接输入ls会只在界面中展示目录中的文件，而输入ls -l则会在界面中展示目录中的所有文件的详细信息

输入ls -a会在界面中将隐藏的文件也一并展示，但是比较简介的版本

但我们一般是将上面的两个命令合并起来用，就是ls -al，其表达的意义是展示目录中的所有文件（包括隐藏文件），并且以详细信息的方式来进行展示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbd056d6315c2cca979c46540be47ffb7.png)

最后我们来科普下详细展示信息里的各项信息分别代表的意思

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE48df8275e91f5aaf48be0cb61b906cf4.png)

注意这里名称里以.开头的代表是隐藏文件，权限相关的内容后面再讲，这里先简单了解下

### pwd与cd命令

- Linux命令 --- pwd命令，这个命令我们我们只要学习输入pwd -P或者谁pwd就行了，输入pwd或者pwd -P，可以在界面中显示出当前所在的目录，这个命令的主要作用也是用于查询当前所在的目录

- Linux命令 --- cd命令，这个命令的作用是切换当前的目录，切换方法分绝对路径和相对路径

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE660993d210e4b48f462eb5e987619b24.png)

简单来说相对路径就是从本路径开始不断进入下一个路径，而绝对路径只要输入全部的路径名，就可以直接进入到指定路径

这里提一点，我们创建的用户都是存放到home目录下的，想要返回上一级的目录，可以使用命令cd ..

接下来我们来讲讲绝对路径的使用方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE99d28b0547cc8bd293be239dc1a7a28a.png)

这类要注意的是我们第一个/表达的意思是根目录，后面的/表达的则是目录转换符，虽然说我也不理解为什么第一个/就不用加目录转换符就是了，不过这里直接记着就可以了。可以看到如果我们要切换到根目录，我们直接cd /就可以了，这里的/代表的其实就是根目录的绝对路径，因此可以直接切换到根目录

### mkdir命令

- Linux命令 --- mkdir命令，调用这个命令可以创建一个文件夹

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa7ff437c2fdab5a90104d1f085fc0bd0.png)

如果我们想要创建名为aaa的文件夹，则语法是mkdir aaa，如果想要创建一个文件夹里还有一个文件夹的文件夹，则语法是mkdir -p aaa/bbb，其代表的意义是会在aaa文件夹里再创建一个文件夹bbb，如果aaa不存在，会将aaa也一并创建出来

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaee4f637f967e8c6bfbb1cb4f8913569.png)

- Linux命令 --- rmdir命令，该命令用于删除空目录，注意，这个删除能够执行的前提是文件夹内部是空的。我们还有另外一个命令，其将文件夹内部的文件夹删除之后还会判断删除之后的父文件夹是否为空，若为空则一起删除掉

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5efd787d67cc5e69339caf3b7521724f.png)

### rm命令

- Linux命令 --- rm命令，该命令可以用于删除文件或者是目录

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE36bc6671fdcabcdf8d41d2745a132758.png)

这里简单提一个知识点，调用toucch a.txt命令可以创建名字为a的txt文件，会默认创建在当前用户的所在目录中，实际上，我们可以生成各种名字和格式的文件，这取决于用户填入了什么

如果我们要删除这个txt文件，我们只要输入rm a.txt就可以了，同理我们要删除某个特定文件也是输入对应的名字和文件格式名

如果我们要删除某个目录的话，我们只要输入rm -r 目录名，rm命令的删除会直接删除指定目录，不会管目录中是否有文件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE374e6c4565f6eb54a8e49169beab8d56.png)

### cp命令

- Linux命令 --- cp命令，该命令的作用是文件复制，其语法是cp [选项] 数据源 目的地

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE015bb8fc9f6df4889f3a5e5a888e1ab1.png)

如果我们想要复制一个在aaa文件夹下的文件a.txt到文件夹ccc中，则其语法是cp aaa/a.txt ccc

如果我们想要复制aaa文件夹下的所有文件到文件夹ccc中，则其语法是cp aaa/* ccc，但是这种方式会跳过文件夹的复制

如果我们是想要连文件夹也一起复制的话，则语法是cp -r aaa/* ccc

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7516ff7caadfe106591d1a4e8ac30003.png)

### mv命令

- Linux命令 --- mv命令，这个命令有两个作用，一是移动文件，相当于windows里的剪切，二是对文件进行改名

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5970f8da59bd7c6006ee625ff6a14a81.png)

如果我们想要移动aaa文件夹的所有文件到ccc文件夹的话，其语法是mv aaa/* ccc，这里我们就不需要-r了

而mv ccc/a.txt aaa，其语法代表的意义是移动ccc文件夹中的a.txt文件到aaa文件夹中

接下来我们来看看各种不同的命令格式的实际代表意义

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6fc0d8b90b27c762bb2e72d031345fac.png)

## Linux文件的基本属性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1bcdd82d3daf39892ef878fff8993645.png)

我们之前调用过ls相关命令展示出所有文件的详细信息，其实这里的详细信息就类似于windows中的文件属性，接着我们来讲讲关于权限的相关知识，先来看看其各种字母表达的意思吧

## 权限相关内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe42af4430f5bc04dda7134fefba44f2d.png)

我们的文件的表示方式总是rwx的形式的，其中哪一个变成了-，就说明没有该项权限。第一个可以为l，其代表链接文档，其意义就类似于我们windows里的快捷方式

2-4位代表属主权限，也就是创建其的用户的权限，5-7代表属组权限，也就是创建该文件的用户的所在组的其他成员所有的对这个文件的权限，而8-10位则代表其他用户，既不是其创建者也不是与创建者同属一组的的用户所拥有的权限

最后我们做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5c6060448640c243be4f78ede0b2e59a.png)

我们在实际的生活场景里总是会遇到我们需要其他用户拥有某些文件的权限的时候，比方说我们希望其他用户也有等同于创建者/创建者所在的组的权限，此时我们可以将该文件所属组或者是所属用户给更改来达到这个目的。接下来我们就来学习更改文件的所属组的命令

## 权限相关命令

- Linux命令 --- chgrp命令，该命令可以更改一个文件或者是目录的所属的组

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5a18173de7f80c2d7af4cc81dee275c8.png)

假如我们想要修改aaa文件夹的所属组为root，那么其语法是chgrp root aaa。如果我们输入chgrp -v root aaa，那么其不但会将aaa所属组修改为root，还会打印一个提示信息，提示更改组已经完成

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE47639ea298b89c9efd724bb527796de1.png)

- Linux命令 --- chown命令，该命令不但可以更改文件的属主，还可以一并更改其属组

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa952213c695b57be3d2247ca86360d68.png)

假如我们想要修改aaa文件夹的所属主为root，那么其语法是chown root aaa，如果我们想要将aaa的所属主和所属组一并修改，那么其语法是chown root:root aaa。如果我们想要修改aaa中的所有文件包括aaa的所属组和所属主的话，那么其语法是chown -R root:root aaa

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f8d43e43b2d0ed634eac5266f20bbcc.png)

但实际上，还有另外一个想法可以完成我们的需求，那就是直接更改文件的权限，接下来我们就学习更改文件权限的命令

- Linux命令 --- chmod命令，该命令可以修改某个文件的开放给不同用户的权限

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE92bb49976c1b2fc5b2038e4a1be9a8d1.png)

有两种修改方式，分别是数字方式和符号方式，我们这里先来讲数字方式

- chmod命令的数字方式，在学习数字方式之前，我们要先理解数字表示权限的方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaf9f12a67354ba7242234d347a628141.png)

这里设置数字5就代表其只有读和执行的权限，了解了这些内容之后，我们就可以正式学习命令了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE04475bf1401a022d3ebfa5cc6e856df0.png)

比方说我们希望将aaa文件夹内的所有内容（包括其本身）的权限设置为对属主和属组可读可写可执行，对其他用户不开放任何权限，那么其语法是chmod -R 770 aaa

- chmod命令的符号方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE63642dd87411c9dc110a17022a0e3a0a.png)

同样在学习符号方式之前，我们也要学习其对应的知识，首先是属主属组和其他用户的表示方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8420553269c3481ce291bfddbf25ff32.png)

其他是修改权限的方法有

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9ee47a081f62ad7896e11b2bc7bf24bf.png)

这里值得一提的是，如果我们不想修改任何权限，那么可以在对应的运算符号后什么都不加，这也是可行的

同样的，假设我们希望将aaa文件夹内的所有内容（包括其本身）的权限设置为对属主和属组可读可写可执行，对其他用户不开放任何权限，那么其语法是chmod -R u=rwx,o= bbb，如果我们只希望设置某个权限，那么只要在设置时只写入对应的要修改权限的表达符号和要修改内容就可以了

如果我们要修改的所属组所属主和其他用户的权限都是一样的，那么我们还有一个简化的写法，就是chmod -R a=rwx aaa，其表示的意思是将aaa里的所有内容的三个所属权限都修改为rwx

这个-R可加可不加，加了就表示修改某个文件夹的所有内容（包括其本身），不加则说明只修改指定的某个文件夹或文件

# Linux的文件管理

## touch命令

- Linux命令 --- touch命令，该命令可以用于生成指定的文件，若要生成的文件已经存在，则会更新其对应的时间属性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4955276f1214650a3c59cfc6025afcb6.png)

输入touch a.txt，会在当前目录下创建一个名为a.txt的文件，a是名字,txt是文件格式

如果想要批量创建的话，可以输入touch a{1..10 }.txt，注意这里先写入touch命令，然后我们先输入文件名，接着输入想要生成的序号的开始和结尾，用两个.分隔并用大括号括起来，然后接上文件格式名就可以正确生成了

这里提一下，调用stat 文件名.文件格式名，可以查看一个文件的详细信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa99cc32ecfbf82dd242c0e64b2a64119.png)

## vim编辑器与命令

- Linux的vi/vim编辑器，简单来说就是程序员常用的工具，其中我们主要介绍vim，简单可以理解成windows中的文本编辑器，其中vi和vim的区别如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEed8c0a999223f08f927a080ebbb2a4c1.png)

如同windows中的文本编辑器有三种模式一样，vim打开文件之后也有三种模式，分别是命令模式（只能读不能写），编辑模式（能读能写），末行模式（保存修改内容）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7d1470a8cabe3dc5a9bc3fb3c2ac1c68.png)

这三者的进入和退出的方式如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE832a091453b0a10c376fc4b9e9710455.png)

- Linux命令 --- vim命令，该命令可以用于打开一个文件并进行相应的编辑，如果文件已经存在，则会直接进入，若文件不存在，则会打开一个临时文件，同样进行相应的编辑之后可以选择保存与否，选择保存退出则会新建一个文件。注意冒号是shift+:的方式来使用的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf0bbd6c212e4684b5ee2701e72a18a7b.png)

而在进入文件之后，我们先进入的是命令模式，如果要进入编辑模式，则要按下对应的按键

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6e23085314d7c7943cc97b7f0db909db.png)

虽然看起来很多，但实际上只记住一个i就可以了，其他的比较少用

当我们进入编辑模式后，就可以进行对应的编辑了。当我要从编辑模式中切换到末行模式时，我们是要先按ESC返回到命令模式，然后按:进入末行模式，接着输入对应的命令就可以进行对应的保存操作了。末行模式里可以使用的命令如下所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb2af14da9a53a5b65d6dfabb09255f1b.png)

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8c483d95d30272912676fd9ddedb8a50.png)

- Linux文件查看相关命令，如果我们只是想要查看文件而不进行编辑的话，那么我们有更加方便的命令，这里的命令有五个，我们分开来讲

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeba04ad681619c6b45a21a89ccfb2433.png)

## 查看文件内容相关命令

- Linux命令 --- cat命令，该命令可以直接在控制台上打印出文件的所有内容，超越了屏幕就直接往下顶，直到全部打印出为止。还有一个cat -n 文件名.格式名的命令，可以打印出内容的同时显示行号

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb7b89067b06600ab1be58d6096d0593c.png)

- Linux命令 --- less命令，该命令可以查看文件的内容，但其不是在控制台上打印出来，而是将文本内容展示出来，用户可以通过↑↓按键自行阅读，退出则按q，加上-N可以显示行号

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4732ef8d8813048a17f55ea47caf51c4.png)



- Linux命令 --- tail命令和head命令，我们先来讲tail命令，该命令可以查看文件的最后部分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2deb6b661aa48f638c6cd0e636ad0d90.png)

如果我们输入tail a.txt，那么会默认打印该文件的末位十行内容，如果我们想要指定打印的行数，就应该输入tail -数字 a.txt，输入对应的数字可以指定末位需要打印的行数

如果我们输入tail -f a.txt，其会动态展示文件的末尾十行内容，比方说我们打开的是一个日志文件，那么我们可以在展示界面中不断看到其写入的内容，按ctrl+c退出。如果我们想要指定其动态显示的行数的话，我们只要在f前面加上数字就行了，比如tail -4f a.txt，则意为着动态展示a.txt文件的末位四行的内容

如果我们输入tail -n+2 a.txt，该命令会将文件从指定的行数开始打印直到末尾，这里的意思是从文件的第二行之后开始打印直到末尾，如果我们输入其他的数字那么其也会对应从我们指定的行数之后开始打印

如果我们输入tail -c 45 a.txt，该命令会将文件的最后的45位字符正确打印出来且会自动分行，我们可以通过改变数字的大小来改变我们所需要打印的字符数量

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE594fe5a53840b8c94fd36a6b03c28c93.png)

而head命令和tail命令其实是差不多的，我们依葫芦画瓢就行了，这里就不再演示了

## 查找相关命令

- Linux命令 --- grep命令，该命令主要用于查找，其不但可以用于查找文本内容，还可以用于查找进程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE605cb28ea7d82ceae6c13a0a802a187d.png)

如果我们输入grep 日 a.txt，那么该命令会将a.txt文本中含有“日”字符的行给找出来并打印到命令行上。如果我们还想要其显示行号，那么命令应该改成grep -n 日 a.txt，这样打印的符合条件的行中就含有行号了

如果我们输入grep -i a a.txt，-i这个命令会令我们查找时忽略大小写，那么其会将文本中含有a或者A的对应行全部打印出来

如果我们输入grep -v 日 a.txt，这个命令会将除了含有日字符的行之外的行全部打印出来，相当于是一个排除命令

接下来我们来讲如何用grep命令查找我们所需要的进程，这里要与ps命令结合起来

比如说，我们输入ps -ef | grep sshd，就会出现以下结果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc1acb57a010fb3b08849196b12aeee88.png)

可以猜测这个命令的意义是先查出所有的进程，然后中间的 | 应该意味着要执行下一个命令，而后面的grep命令则起到了从这些进程里查找的作用，这里我们查找的进程是带有sshd字符的，所以最后我们的打印出来的进程也的确是带有sshd字符的，最后我们可以看到最后一个sshd进其实是我们的查找进程，如果我们不想要看到这个查找进程，那我们应该怎么办呢？我们可以输入ps -ef | grep sshd | grep -v "grep"，这里则代表最后会在前面的结果的基础上再移除掉含有grep的进程，那么最后就会只显示前面三个进程了。如果我们只是想知道进程数量，而不想知道其具体进程，我们可以输出ps -ef | grep -c sshd，那么最后其就会打印数量而不会打印详细信息了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEadd52af9d2aab53376f19886536e2841.png)

## 补充知识

- Linux命令 --- vim定位行，该命令语法是 vim 文件名.文件格式 +数字，使用该数字打开文件可以让光标在指定的行数中出现，而不会出现在第一行中。用该命令主要可以快速定位到我们认为的需要修改的行中
- Linux的vim的异常处理，在我们编辑文件时，如果我们编辑文件没有保存，而我们的Linux系统又直接关了，这样我们如果要再次编辑同样的文件，其就会出现异常提示信息，该提示信息会提示说查找到同名的swp文件，请求处理方式。为什么会这呢？这其实是因为我们的Linux系统为了保证源文件的安全，其编辑时并不是直接在源文件上编辑的，而是先生成一个swp格式的交换文件，当我们保存文件时，其该swp文件才会被删除掉，并且对源文件进行修改。而如果我们直接在编辑时退出系统，那么swp文件就不会被处理，因此会提示异常信息。我们处理该异常的方法很简单，一是按A退出然后调用rm方法删除该swp文件再进行修改文件，二是在异常提示中直接按D删除，就可以继续正常编辑文件了

## 写入相关命令

- Linux命令 --- echo命令，该命令可以用于展示输入的字符串以及将我们写入的字符串写入到文件当中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe3cd28aaa792c87ba73d1705cc9676c1.png)

如果我们输入echo "用户输入字符串"，那么会直接在控制台上打印用户写入的字符串

如果我们输入echo "用户输入的字符串" > 文件名.文件格式，那么该命令会将我们写入的字符串覆盖写入到我们指定的文件中

如果我们不想要覆盖，只是想要继续写到该文件的后面那我们该怎么办呢？我们可以输入echo "用户输入的字符串" >> 文件名.文件格式

如果我们希望将报错信息不覆盖写入到某一个文件中，我们可以输入cat 不存在的文件 &>> 文件名.文件格式，这样就可以写入了

## 切割相关命令

- Linux命令 --- awk命令，这是一种用于处理文本文件的命令，往往要与其他命令组合使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE08ae21dd469253a353d3c5333a6ee928.png)

比方说，我们有一个文档，文档里存放了一些姓名和数字，如果直接用cat a.txt可以直接查看所有内容，但如果我们只是希望查看符合条件的部分内容的话，我们可以用cat a.txt | awk '/zhang|li/'，这个表达的意思是其会先获得文本中的所有内容，然后将文本中存在"zhang"或"li"的字符串的一行给展示出来，注意，我们在awk中的两个//中间写入的条件，在中间的 | 上是不用加空格的，如果我们加了空格，那么其会将空格一并当做条件用于搜索。而且可以看到我们的awk语法后面是加了单引号和两个斜杆，之后再写入具体命令的，这点也要记下。

awk命令的第二个作用是将每一行按照指定方式切割，并且可以返回指定行的内容。在学习这个语法之前我们要先学习其相关知识

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd7025c3cc8aad057e9c097dc96e05753.png)

首先使用-F ''命令可以知道你使用指定字符切割，''要输入我们要用于切割的字符，图中使用的是逗号，因此是','，而我们使用的是空格，因此我们要写入' '，其次是切割之后其会将切割后的内容用$数字的方式来表示切割的第几个纵列，从1开始。而最后我们输入$数字第几行的内容就表示我们要打印哪一个纵列的内容

那么学习完上面的知识之后，我摸容易构造出cat a.txt | awk -F ' ' '{print $1,$2,$3,$4}'的命令，这个命令表示的是我们先获取到a.txt中的内容，然后调用切割命令，我们用空格将其借个，然后我们要打印我们指定的纵列，打印命令我们放到单引号里面写，先输入大括号，大括号内部输入print打印接着跟一个空格之后写入要打印的纵列，用逗号隔开。最后我们得到的内容和直接用cat a.txt是一样的，虽然是一样的，但是其内部执行的过程却是不同的，这点要搞清楚

如果我们希望我们切割之后的字符串按照一定的规则打印出来，此时就要用到我们的OFS语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8e4b2e542b0d896409f9fff411fa9df2.png)

其语法是OFS="字符"，假设我们希望我们的字符串切割后对于每一个纵列都用===间隔，那么我们的语法就是cat a.txt | awk -F ' ' '{OFS="==="}{print $1,$2,$3,$4}'，语句本身就不再解释了，这里要注意的是，两个大括号之间不需要口空格，单引号内部的第一个大括号里写OFS语句，=号后面用双引号写入自己用于间隔的字符串。如果我们想要用制表符来间隔的话，只要把上面的===改成/t就行了

如果我们还有更多的需求 ，比如说我们希望返回第一个字符串的大写形式，小写形式或者是长度的话，我们就应该学习进一步的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEce92dec7cf9c3b0a1ac6a2d4fa3005da.png)

这里面其实有点调用函数的意思，这些函数方法都是awk里的方法

如果我们希望将字符串的第一个切割内容转成大写并打印，那么我们应该输入cat a.txt | awk -F ' ' '{print toupper($1)}'

如果我们希望将字符串的第一个切割内容转成小写并打印，那么我们应该输入cat a.txt | awk -F ' ' '{print tolower($1)}'

如果我们希望将字符串的第一个切割内容转成字符数量并打印，那么我们应该输入cat a.txt | awk -F ' ' '{print length($1)}'

接着我们就要实现更加复杂的需求了，同样的我们也要学习新的知识

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE908f520b50ed7c1779704bbce23eae93.png)

对于第一个需求，我们可以输入代码cat a.txt | awk -F ' ' 'BEGIN{}{totel=totel+$4}END{print totel}'，这个语句表达的意思是其先获得文件中的内容，然后将文件中的内容分割开之后一行一行获取到（这里位置执行的是BEGIN的语句），每获取一行语句，我们就会将其第四纵列的数字加到totel变量中，之后等到BEGIN语句结束之后指定EDN语句，此时打印出totel，就是我们所需要的第四列的总分了

而对于第二个需求我们可以输入代码cat a.txt | awk -F ' ' 'BEGIN{}{totel=totel+$4}END{print totel,NR}'就可以了，NR代表的意思其实是处理到第几行的行号，而好巧不巧我们这里的行号正好能够代表人数，因此这里用行号来表达人数正好

对于第三个需求我们可以输入代码cat a.txt | awk -F ' ' 'BEGIN{}{totel=totel+$4}END{print totel,NR,(totel/NR)}'就可以了，我们求平均分的方式其实就是简单的将总分除于人数再打印而已

## Linux系统的软连接

- Linux系统的软连接，所谓软连接，其实就是快捷方式。值得一提的是在Linux系统中，文件的名字和文件的数据是分开存储的，因此其软连接打开文件的方式是先通过软连接然后找到对应名字的地址，接着找到文件的名字然后通过文件名字找到文件中的数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf408b294c909f9605903825f2ad460e1.png)

其语法是ln -s

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd23718b7efda392ef7e0aebca15a856c.png)

比方说我们写入如下语句

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE29f4a8cd4daa2aa4f514925b1830bb05.png)

第一行是我们写入的语句，ln -s 具体路径 要生成的快捷方式名字,这样就会在其当前的目录下生成一个指定名字的快捷方式，这个快捷方式可以直接打开目标文件（其实名字到底能不能自定义是我猜的，我猜测其可以自定义，但我不保证对）

# Linux的压缩命令

## 搜索命令

- Linux命令 --- 查找命令，该命令就类似于是windows系统里的搜索命令，这里主要依赖find语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2ef7a31ae9e2d78a68b1f8e2f641db50.png)

如果输入find . -name "*.txt"，其代表意义是在当前目录及当前目录的子目录下寻找所有的txt格式的文件，如果我们想要寻找指定文件名的文件，只要将前面的*替换成对应要查找的名字就可以了

如果输入find . -ctime -1，其代表的意义是要在当前目录及其子目录下寻找所有一天之内用户操作过的文件，如果想要找两天的，把1改成2就完了

如果我们想要寻找根目录下的而非当前目录下的，需要将.改成/，其他根据我们的搜索需要进行相应改动就可以了

## 压缩相关命令

### gzip命令

- Linux命令 --- 压缩命令，windows里常用的压缩格式有zip和rar，而后者在Linux系统是无法识别的，因此在Linux系统中是采用zip格式进行压缩的，压缩的命令主要依赖于gzip命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdbb98dafe3d187f599b4470aa984b7a7.png)

如果我们输入gzip a.txt，其代表的意义是会将当前目录下的a.txt文件压缩，而且这个压缩会将源文件覆盖掉，如果我们想要压缩当前目录的所有文件，那么可以将a.txt改为*，注意这里同样是覆盖压缩，而且是独立对象的压缩，即假如当前目录下一共有十个文件，那么最终就会生成十个压缩包，每个压缩包对应每一个文件的内容，而不是一个压缩包内压缩了十个文件。而且已经处于压缩状态的文件无法被再次压缩

那么如果我们需要解压文件，那么我们可以输入gzip -dv *，-dv表示的意义是解压文件时显示对应的解压信息，而*代表的意义是所有文件，整个命令代表的意义是解压当前目录下的所有文件并显示对应的解压信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2c6a397181cfd5241b451d184c075bd6.png)

还有一个gunzip命令，该命令是真正用于解压缩的命令，在当前目录下解压所有文件的命令是，gunzip *

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE70711eb83a7407baa4c54fddf18ea149.png)

### tar命令

- Linux命令 --- tar命令，该命令主要用于打包、压缩和解压文件，里面的命令往往比较复杂，需要调用许多参数，因此这里我们也需要对其内部的各种可选参数进行学习

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE383457062863c7aa739277b7f11e6b7e.png)

参数-c代表建立新的压缩文件，简单而言就是不会压缩覆盖了，会新创建处压缩文件

参数-v代表显示命令的执行过程

参数-f代表要指定一个压缩文件

参数-z代表要调用gzip指令来处理压缩文件

参数-t代表要列出压缩文件中的内容

参数-x代表解压

假如我们输入tar -cvf a.tar a.txt，tar本身代表的命令是打包，而后面的参数代表的意义是建立新的压缩文件（-c）且要显示该命令的执行过程（-v），并且指定压缩文件（-f），这里指定的名字就是后面写入的a.tar，而一开始要打包的目标就是我们命令最后的a.txt，而a.tar则是我们自己调用的重命名命令，指定新生成的文件的名字，最后按回车我们就可以看到在当前目录下已经生成了对应的a.tar文件了

假如我们输入tar -zcvf b.gz b.txt，tar本身代表的命令是打包，而后面参数代表的意义是对指定文件进行压缩（-z）建立新的压缩文件（-c）且要显示该命令的执行过程（-v），并且指定压缩文件（-f），这里指定的名字就是后面写入的b.gz，而一开始要打包的目标就是我们命令最后的b.txt，最后按回车我们就可以看到在当前目录下已经生成了对应的b.gz文件了

如果我们想要压缩整个文件夹而不是一个单一的文件，我们首先要进入到包含该文件夹的目录下，然后输入以下命令tar -zcvf aaa.gz aaa，这个命令就不再次解释了，调用成功之后会在对应目录下生成对应的文件

如果我们想要查看某一个压缩文件内的内容，我们可以输入tar -ztvf aaa.gz，该命令多了个t，代表要查看压缩文件内的内容，后面的aaa.gz则代表我们要查看的压缩文件，具体的其他指定的结合意义课程里没说，我个人觉得按说这个-z应该是可以省略的，因为我们这类并不压缩（我能想到的一个可能的解释就是-z代表调用gzip的方法，而查看压缩文件的内容的方法是gzip内的），但是它做的肯定是对的，先记着吧

如果我们想要解压某个文件，那么我们就可以输入tar -zxvf aaa.gz，这里的-x则代表解压的意义，而aaa.gz则代表我们指定的要解压的文件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE96cc915f0094653e295350e42c84955e.png)

### zip命令

- Linux命令 --- zip命令，这也是一个压缩命令，不过有所不同的是这个压缩命令压缩的格式统一只能是zip格式的，同样要压缩某个文件的话，必须先进入到文件的当前目录中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE430bfcb27809ab165e15a1dd91a4164c.png)

如果我们输入zip -q -r aaa.zip aaa，这就意味着我们要将当前目录下的aaa文件夹压缩，并且因为其实文件夹，我们同时是要将其内部的文件也一并压缩的，因此加上-r，-q代表不显示该命令的执行过程，最后我们创建的压缩文件的名字就是我们指定的aaa.zip，注意这里是新创建一个压缩文件，而不会将源文件删除或者是覆盖

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8093943e430706453c45541c1cdcdeae.png)

- Linux命令 --- unzip命令，这个命令是一个解压命令，主要用于解压，不过只能够解压zip格式的压缩包

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE226cf8354ce9be976576b8461bb1698e.png)

如果我们输入unzip -l aaa.zip，其代表的意义是我们要查看aaa.zip内的所有内容

如果我们输入unzip -d bbb aaa.zip，其代表的意义是我们要解压aaa.zip的内容，并且我们指定解压的内容存放到我们所指定的bbb的目录中去，没有对应目录则会创建出来

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf11841465d78069b26dcf434866f5cfe.png)

- Linux命令 --- bzip2命令，这个命令和上面的zip命令大同小异，不同的是这个命令采用新的压缩算法，压缩后产生的文件更小，但是耗费的时间更长，同时其生成的压缩文件后会删除原始的文件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfb86cf8bf2734e67c6f209398d21be29.png)

如果我们输入bzip2 a.txt，其意义是我们要用新的压缩算法压缩a.txt文件，最后我们生成的压缩文件的名字是a.txt.bz2，可以看到格式是bz2格式，前面的格式名则被当成了文件名使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE759ce9a378d83ce9a534e3ec14746b9b.png)

- Linux命令 --- bunzip2命令，同样我们也有解压的命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2e9461b95a0ea420bcef94f81e7f6ea.png)

如果我们输入bunzip2 a.txt.bz2，其意味着我们要解压指定的文件，如果我们想要解压时同时显示详细信息，则可以输入bunzip2 -v a.txt.bz2

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8edae76a1cfd0638dc2646b43a508e8e.png)

# 网络与磁盘管理

## ifconfig命令

- Linux命令 --- ifconfig命令，这个命令类似于windows中打开我们的网络连接窗口，可以查看我们Linux系统中的网络信息以及设置对应的配置参数

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7b2418fa3fb6d8cc564c707d7b7b7f09.png)

如果我们直接输入ifconfig，则会在界面中显示对应的网卡信息，如图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa9523204309f4f0c075618a9fd05b4a2.png)

ens33代表一个网卡，一个Linux系统里可以有多个网卡。这里inet后面的是网卡的ip，而netmask则是网卡的子网掩码

如果我们输入ifconfig ens37 down，其代表的意义是我们要关闭ens37这张网卡，down代表关闭，而ens37则是我们具体指定的网卡，如果我们想关闭其他网卡那就指定其他网卡的名字就可以了，反之将down改成up则是开启网卡的意思

如果我们输入ifconfig ens37 192.168.23.199，其代表的意义是我们要将ens37的ip设置为192.168.23.199

如果我们想要同时修改子网掩码，可以输入ifconfig ens37 192.168.23.199 netmask 255.255.255.0，这样就可以将子网掩码也一并修改了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE60e83eade9870c28eb5531ebc5e9155a.png)

## ping命令

- Linux命令 --- ping命令，该命令可以检测系统本身是否与主机连通

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE947c7af3318ef3925c6fcc9d5d00dcfc.png)

比方说我们输入ping www.baidu.com，就会按秒逐行显示如下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfde3c42c77ff354defc883a5af5fe1e9.png)

这里我们只关注time，也就是相应时间，time里面的秒数越低，代表联通速度越快，如果我们想要取消连通，按ctrl+c就可以了

如果我们只希望其显示指定的次数，那么我们可以输入ping -c 2 www.baidu.com，其代表的意义是我们与指定网址进行连通，且只相应两次

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf64d1ed135a82e0abcabd095dc5f238d.png)

## netstat命令

- Linux命令 --- netstat命令，该命令可以用于显示网络状态

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe6804eb8691ad8154b6998ef8201d41b.png)

如果我们输入netstat -a，该命令会显示Linux系统中所有的详细的连接情况

如果我们输入netstat -i，该命令会显示Linux系统中各个网卡的连接情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9431cf4067a51c275dbac3c145e93390.png)

## 硬盘相关命令

- Linux命令 --- lsblk命令，该命令可以列出硬盘的使用情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE39b79665702fa072afb18eb35378dcce.png)

如果我们直接输入lsblk，那么会在界面中显示对应的系统信息，如图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3b5633ce73d269ce91ad64efe4f3fc18.png)

接着我们来解释下图上的内容，我们在Linux系统里给系统总共分了一块硬盘，这个硬盘就是sda，如果我们分了两块硬盘，那么图还会有一个叫做sdb的另外一个硬盘，但是这里我们没有，所以只有一个sda，在sda硬盘下又被分为了两块，分别是sda1和sda2，sda2下又被分为了两块。其中RM表示其对应的设备是否为可移动设备，0表示否，1表示是，可以看到sr0为可移动设备，其实sr0就是我们对应的Linux系统上的可移动光驱。接着是SIZE，SIZE表示的是对应的硬盘所占据的大小，比如在上图中sda总共占据20G，而其下的sda1和sda2分别占据1G和19G

我们还可以输入lsblk -f，输入该命令会在界面中显示系统信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeb23faf2a4c543e6ddc8c8f49c0d136a.png)

这其中NAME表示系统的名字，而UUID表示设备的编号

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbf7eb0484744a6a65f8fc7440c97947a.png)

- Linux命令 --- df命令，该命令同样是用于列出硬盘的使用情况，不过于上一个lsblk命令有细节上的不同

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd94a46d8a36b76abe17afa577a474d27.png)

如果我们直接输入df，则会显示以下信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0cf8cc1b838a87e12b717171f0d97a41.png)

可以看到虽然同样是显示硬盘的使用情况，但是展示的信息却与lablk命令大不相同，这是因为lsblk命令是从硬盘的使用角度去展示的，而df命令是从文件的使用角度去展示的，简而言之其实就是df命令是将系统分为很多个盘然后去展示的，上面文件系统里的每一个文件名我们都可以简单理解为一个盘，而后面的1K-块则是显示其占用的数据的大小，单位是K，后面的不解释了，懂的都懂，而挂载点我们后面会学习到

如果我们输入df aaa，那么其会只显示一个aaa文件夹的在硬盘里的各种信息。如果我们输入df --total，则其会展示硬盘的所有文件的详细信息，但是实际从显示的界面中的内容来看，似乎和直接输入df没啥区别，除了后者多了一个total行之外

最后如果我们输入df -h，那么其不但会展示硬盘中的所有信息，还会将其大小换算成MB和GB的形式，便于程序员的阅读和理解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEac040adb4216cd6f804bc0c879a5ac20.png)

最后做个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7feca232bcb4ca79de9b5f5f74c40592.png)

- Linux命令 --- mount命令，该命令的作用是用于挂载Linux系统外的设备，什么是挂载系统外的设备？可以简单理解为在windows系统里的U盘一类的东西，就是系统外的设备。当我们在Linux系统中挂载U盘时，windows系统会自动创建一个盘符，我们可以通过这个对应的盘符来访问U盘设备，而在Linux系统中我们则需要手动去实现这个创建盘符和通过盘符来访问外部设备的操作

## 挂载相关命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc8ec006abae1b88c6663a1c6519723e5.png)

我们挂载系统外的设备的大体操作是先创建一个文件夹，然后将文件夹与U盘关联起来，这样我们的文件夹就成了一个挂载点，我们可以通过访问这个文件夹来访问U盘（关联的过程称之为挂载），如果我们不想要通过文件夹来访问U盘了，也可以将其取消关联（取消关联的过程称之为卸载）

关于挂载点，我们有以下几点需要注意

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0ec63acd1060c7d4616c07426891b91a.png)

那么现在我们就是实现文件夹和外部设备的关联，我们首先要创建一个文件夹，那么我们输入mkdir aaa，创建一个名为aaa的文件夹。然后我们输入mount -t auto /dev/cdrom aaa，这样我们就挂载成功了

mount -t auto /dev/cdrom aaa，这句挂载命令起作用的代码是mount -t auto，而/dev/cdrom则是外部设备的对应地址，而aaa则是我们要进行关联的文件夹。如果我们想要取消关联的话，只要输入unmout aaa，unmout指令+想要解除关联的挂载点就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2446e9edc6579071aed318be62fd894.png)

## 安装相关命令

- Linux系统 --- yum，在Linux系统当中，如果我们想要查找，安装，下载或者卸载软件，需要通过yum来操作。yum的全称是Yellow dog Updater,Modifier，我们可以称之为小黄狗

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa68e046e4931b41c81067079f45d7293.png)

- yum概念，那么yum到底是什么呢？其实yum就是互联网的软件包服务器，各个服务器都可以连接到这个yum源来下载各种软件包，而且其会自动解决软件的依赖关系。什么是软件的依赖关系？比方说我们启动软件A之前必须要启动软件B，那么这个关系我们就称之为是依赖关系。其实最最最简单的理解就是应用商店，需要什么直接下载就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7206d43c6b4e55718bd6731de7e09f02.png)

- yum的常用命令，虽然很多，但是不需要记，用到那些我们再来查看就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6e2c2e518d079984d2dd94276b5cb9af.png)

那么接下来我们就来正式试试下载软件，首先我要从yum源中下载一个tree的工具，值得注意的是，这必须要有root权限才可以。首先我们输入yum -y install tree，这里-y代表的意义是在安装过程中跳出任何选择支我们都默认选择yes，install是安装，而tree则是我们要安装的软件的名字，按下回车之后等待一段时间，我们就将这个软件安装完毕了，然后我们可以直接输入tree来调用这个软件。

而如果我们想要卸载这个软件的话，我们只要输入yum remove tree，就可以了。

除了下载卸载之外，其还有一个实用功能就是可以查找对应的软件，比如说我们想要查找以tom开头的软件，我们可以输入yum list tom*，这样就会在界面中显示出所有的以tom开头的软件了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE41eb3be6381965656bdc73c7473f71a0.png)

- 更改yum源，刚刚我们已经用yum命令下载了对应软件，但现在还有一个问题，那就是我们的yum源默认是连接的国外的，这样的话我们下载的速度就比较慢，因此我们应该要将其更改为国内的yum源（阿里、腾讯、搜狐）

为了实现这个目标，我们首先要下载wget，因此我们先输入yum -y install wget，接下来我们要对原来的yum源进行一个备份，因此我们要先进入其对应的文件夹（这个文件夹就是/etc/yum.repos.d/），因此我们输入cd /etc/yum.repos.d/，这样就可以进入到yum源的对应文件夹了。那么接下来我们要对该配置文件做一个改名，我们输入mv CentOS-Base.repo CentOS-Base.repo.back，这里我们输入mv之后输入文件原来的名字+文件要设置的名字，按下回车就完成了一个改名动作了，那么到此为止，我们的备份动作就完成了。接下来我们就要更改yum源了，我们输入wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo，这样就把我们的yum源更改为阿里云的yum源了，如果我们输入cat CentOS-Base.repo，能够看到很多的aliyun关键词，说明我们已经成功将我们的yum源更改为阿里云的yum源了。但是这样还没完，我们还要清理下我们之前的缓存并重新加载下这个新的yun源，因此我们要再输入yum clean all，这样我们之前的缓存文件就清理完毕了。接下来我们还要给我们的yum源新建一个缓存文件，我们再输入yum makecache，等待其完毕之后就新建完毕了。如果我们想要验证的话，我们可以验证下我们之前看过的tomcat文件，我们输入yum search tomcat，在界面中会显示对应的信息，可以看到里面已经变成了aliyun的关键字了，这就说明我们已经成功了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE62546aa367f401ee0e2b23c64c49f187.png)

- yum和rpm工具的区别，这个是适记性内容，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE48f5e5b63db13bed8d29b538350773e3.png)

# Shell

接下来我们就学习Linux基础中的最后一个内容，Shell

## 初识Shell

- 初识Shell，那么什么是Shell呢？其实Shell就是一个命令解释器，是位于操作系统和应用程序之间的一个解释器，其作用是将应用程序的输入命令信息解释给操作系统或者是将操作系统处理后的结果解释给应用程序。简单来说，其实Shell就是在操作系统和应用程序之间的一个命令翻译工具

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE66db040bc2cad3c9d2b96126cb97bfd4.png)

Windows中的Shell其实就是cmd，也就是命令提示字符。而在Linux系统中的shell则有很多种，但我们只需要记住，Linux系统默认的shell都是bash

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE388c34c170b4e54cb990b40bef25223d.png)

如果我们想要查看我们当前的shell，可以输入echo $SHELL，这个语句中的SHELL是变量，至于什么是变量，别着急，这个我们很快就会学到了，这里我们先记住。按下回车之后就可以看到我们的shell了

那么现在还有最后一个问题，就是我们的shell要怎么使用呢？一般而言，有两种方式，一种是手工方式，一种是脚本方式，所谓手工方式就是直接手打输入，而脚本方式是先将一系列需要执行的命令写入到一个文件中，然后执行这个文件，达到执行命令的效果，这个文件就叫做脚本文件

## Shell的使用方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeacdb1579da5c380967edf605bf478fb.png)

- 编写第一个shell，那么现在我们就要来编写第一个shell文件了，我们编写这个命令文件，然后可以通过执行这个命令文件来执行我们所需要的命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe6f06fd9ca4834d96a062302201cf348.png)

注意看我们这里的第一行的内容是指定我们现在用的shell是bash，而第二行是注释，后面两行则是比较命令内容

我们首先输入touch a.sh，生成一个指定的文件，接着我们输入vim a.sh，就可以进入到这个文件中进行编写了，我们输入指定内容，然后输入wq!保存文件，接着我们输入./a.sh就可以运行了，但是我们会发现其报出了权限不足的问题，我们查看其权限会发现我们生成的文件不具有执行的权限，因此我们还有修改这个文件的权限，输入chmod 777 a.sh，就可以将其权限全部修改为允许，这时我们再运行就没有问题了

## shell注释

- shell注释，注释相关内容很好理解，这里就不多提了，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE99cfa59126eedf88241746d7b10c862e.png)

注释分单行注释和多行注释，单行注释就直接加#就完了，多行注释的话则是按照:<<字符 内容 字符，这样的格式来进行定义，虽然说这个字符只要保证前后一样不管写什么都可以，不过一般我们的字符是使用!来进行代替，这样易于我们的程序员的理解

## shell变量

- shell变量，这是shell的第二个语法，其内容比较多，我们一个个来

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3c32e067b923bebbe95bab4a49ceb436.png)

- 定义变量，定义变量有三种方式，第一种方式是变量名=变量值，但这种方式要求变量值必须是一个整体，中间不能有特殊字符（如空格），第二种方式是变量名='变量值'，这种方式会将单引号中的内容原封不动的赋值给变量，第三种方式是变量名="变量值"，这种方式的特点是如果双引号中有其他变量，其会先获得变量的结果与原来的内容进行拼接，然后赋值给变量。一般来说，如果我们的变量是数字，我们选择第一种方式，如果不是，则选择第三种方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE12113fc9ef7c751e980d483f102ff46f.png)

如果我们在我们的a.sh文件中写入以下内容

#!/bin/bash

##这是临时的shell脚本

number=10

echo $number

a='$number'

echo $a

b="$number"

echo $b

运行这个脚本之前我们先来解释下这个脚本的内容，首先我们这个=号的左右两边是不可以加空格的，如果加了那么会导致运行报错。其次是之所以我们的echo里都要加$，是因为只有加了$，其才能正确显示变量里的值，如果不加$那么其会原样显示number

然后我们运行这个脚本，我们可以看到以下内容

10

$number

10

可以看到第一个内容是10，说明我们第一种方式就输出成功了。第二种方式直接输出$number，而不是先获取number的值再输出，是因为第二种方式是单引号输出的方式，其会将值原封不动的赋予给指定变量，因此这里的变量输出的单引号中的$number，而第三种方式则会正确获取变量的值之后再将值赋予变量，因此这里输出的内容是10

- 定义命令，其也有两种方式，第一种方式是变量名=`命令`（注意，这里的`是反引号，什么是反引号？其实就是tab上面的那一个键就是反引号）。第二种方式是变量名=$(命令)。这两个命令的作用和原理都是一样的，其都是先执行命令，然后将命令执行的结果赋值给变量

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE32d585ba3d218f7e12b12d0dd24c3849.png)

假如我们在a.sh中输入以下内容

c=`date`

echo $c

d=$(date)

echo $d

那么按下回车之后其就会正确显示两次当前的时间，其本质是先调用date命令获得当前时间，然后将时间赋值给对应变量，然后输出变量的值。我们之前学过启动脚本命令的方式是./a.sh，但其实我们输入bash a.sh，也是同样的效果

- 使用变量，使用变量有四种方式，第一种方式是$变量名，这也是我们之前一直用的方式，但这种使用变量的方式是不标准的，第二种方式是"$变量名"，第三种方式是${变量名}，第四种方式是"${变量名}"，第四种方式是最标准的使用方式。我们这里只需要记住第三种和第四种方式就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEae9270d100297be876c38601d0c9f9d4.png)

比如说我们在文件中写入以下内容

result="现在的时间为${d}"

echo "${result}"

这样我们运行的时候其就是先使用变量，将获得变量的值然后与前面的字符串拼接然后赋值给result，接着我们在打印result变量的值

- 只读变量，其实就是加个readonly就完了，一旦一个变量被readonly修饰，那么其就不能够再进行任何的赋值操作，否则会报错。比如果我们写入readonly result，再写入result="aaa"，那么执行这个脚本文件，会报错

- 删除变量，其实就是加个unset就完了，一旦一个变量被unset修饰，则说明该变量被删除了，此时如果调用该变量则无法获得任何值

- shell数组，这个内容在java里已经品鉴得太多了，就不重复叙述了，直接上结论和测试吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2f97bd2d6a7d438b984bf14a915e59c6.png)

接下来是测试内容，假如我们写入了如下内容并运行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd2949e61be84e11b321223c6f1062529.png)

那么我们可以的得到如下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3daa780c70a534044f3a55b5534f891f.png)

这里值得一提的是虽然说echo{#arr[*]}和echo{#arr[@]}都可以获得数组的长度，但我们一般倾向于使用第一种方式来获取数组长度

## shell的运算符

- shell算术运算符，这个也是品鉴得足够多了，直接来吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE426dedd1d29f04b09c113303a983cc16.png)

这里要注意的是由于原生的bash不支持简单的数学运算，我们可以通过其他命令来实现，这个命令就是.expr，其次是表达式和运算符之间要有空格，最后是完整的表达式要被反引号包括。我们可以举个例子`expr 2 + 2`，这个例子就满足了上面三个条件，是一个正确的表达句

假如我们输入以下命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb21986cf26c1f5fc1cac3a20697cd8bb.png)

这里值得一提的是我们的乘法运算是加入了一个\的，这个\用于转义，因为在Linux系统中的*其实有其他的意思，因此我们这里是要将*进行转义，令其意义变成一个无比单纯的乘法运算符

假如我们又输入以下命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE70377743771330aec3f0e7e35053f7ab.png)

这类值得一提的是我们的自增命令只要加两个括号的

最后我们能得到结果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2a5c4e2233074da07360b402ddd9c0a.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE568b03ba23be73f6052a627580236281.png)

这里要注意的是我们调用运算符的命令时都要放在中括号内，且中括号的前后都要有空格，换言之，即使不能够输入[$a]这种命令，是会报错的

假如我们先定义了a="aaa"，b="bbb"，接着我们输入以下命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7d53417c5067f0cdf8f923080540e7ca.png)

这里我们要提一下的是，$?可以获取上一个变量的结果，其次是在Linux系统中，1为假，0为真，不用true和false表示，而且居然还和我们一般想到的1为真和0为假反过来的，我真的吐了。还有就是注意到当我们使用判断字符串长度是否为0时我们要写入-z，这里我们-z写在前面而且是前后都带有空格的。接着我们继续写入如下命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0efa45c360031b6584c96246b940b6fe.png)

最后我们就可以获取到下面的结果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb2118271916a628b8432337a5c7fd95a.png)

这里值得重点提一下的是我们获取字符串的长度的方法是"${#a}"，这个方法和获取数组长度的方法很像，是额外我们增加的知识，也需要记忆

- shell关系运算符，注意这个关系运算符只能用于判断数字，不能用于判断字符串，除非字符串的值是一个数字

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE696a79390cad99c2cacfbf31abfafa56.png)

我们就以第一个运算符为例，写入如下命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6b7ec160fba5a70a0b9b9f29678b634e.png)

最后得到的结果必然是1，为假。其他的就依葫芦画瓢了，这里就不再赘述了。至于这类的记忆方式，我觉得用英语方式来记忆是最好的，比如大于符号是grater than，那么其命令语句就是-gt，这样就很好记忆了，这一节里其他的类似命令也可以这样记忆

- shell布尔运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE449f50880fe3ae023b3ac9f63e453aea.png)

这里取反其实就相当于是java中的!，而-o则相当于java中的||，-a相当于是java中的&&，我们可以写入如下代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc6c50d95adfa5f0dee8cac3d27b06217.png)

最后我们获得的结果会是

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe28fc473c811bed6fa0adcd55ae17df9.png)

- shell逻辑运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2495e61f220642fc8741af31317ff2b2.png)

和java中的&&与||基本没有分别，与上一个布尔运算符不同的是逻辑运算符必须写在两个中括号之中

比方说我们输入如下代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa5c5ae4cb8f9e6a2c50c49eb57173cf3.png)

其结果会是1和0

可以看到我们的逻辑运算符都是放到了两个中括号之中的

## shell相关语句

- shell判断语句，这个判断语句就类似于我们java中的if语句，其用法也跟java中的非常类似

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3af142ebe6ad3d093ab1a91e3338738c.png)

不同的是在Linux系统中，我们是用[]来放置判断条件的，而进入if条件后面要加上then，结束时要加上fi，而且代码块直接不需要大括号，最后是else if的语句被合并成了语句elif并加上判断条件

我们可以写入如下代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc673fef627a9e29322c371eaf4371709.png)

这里我们解释一下$(ps -ef | grep -c "ssh") -gt 1的意思，其意义是获取所有的进程，然后过滤出所有带有ssh关键字的进程，接着获取这些进程的数量和1进行比较，若大于1则进入判断条件 

再写入如下代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2e9b6998ff44ea10c78cef2cbdda4469.png)

最后我们得到的结果是true,true,a小于b

- shell选择语句，其实就是Switch，不赘述

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEebf29d1e2a72217a39213e9bc28ee3d0.png)

不过这里仍然值得一提的是这里我们只要先定义一个变量，里面存储值，然后其会每次将变量的值取出来与我们在括号内定义的值进行对比，如果相等则会执行下面的语句。而;;就相当于是java中的break，最后esac则是结束语句

- shell循环语句，其实就是java中的for和while循环，我们先来讲for循环

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd8591953844ee1622a0f88bd03df6446.png)

这里要注意的是for是我们定义的变量，而in后面则是其变化的范围，这里我们规定其变化为ABC，那么其会先变为A，然后执行do里面的语句，然后变为B执行......直到循环结束，注意我们定义的范围直接是要有空格的

接下来我们来讲while循环

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe0ba1a4ad66bf5bc0691be950e4a0f60.png)

while循环要注意的是，当条件满足时，我们就继续执行里面的语句，而当条件不满足时，我们就会退出循环，上面右边的代码就演示了打印1-10的数字的代码

- shell函数，这里的理解也是类似于java，这里一共有三种函数，分别是无参无返回值，有参无返回值以及有参有返回值，我们接下来就分别学习这三种方法

首先是无参无返回值的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEac939f95805efbff2b298a6b7850b86b.png)

这里要注意的是，函数被写出来还需要调用的，我们调用无参无返回值的函数直接写函数名就完了，不需要写其他的

接着是有参无返回值的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5bce916a06ad9e1d157fddc70bc33d9c.png)

注意有参无返回值的方法调用时不但要写函数名，而且要传入对应的值，可以传入变量，也可以直接传入值。而在定义时，与java中直接在小括号内写参数的方式不同，shell中是要在函数体中用echo来接受对应的参数（我怀疑其实字是可以省略的，直接写$1和$2都可以），然后$1和$2接受到的参数的到底是什么取决于我们传入的顺序

在有参有返回值的函数中我们还可以在return中做对应的运算再返回。这里我们最后echo $?可以获得上一个命令的结果，其最后会准确将函数的值打印出来

最后我们来看一个练习

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d8c7b99d30340a824986dafcf3bf1f3.png)

这个练习最主要的作用是告诉我们read可以获取用户输入的数字并将其赋值给我们定义的变量，后续操作中我们还可以使用这个变量