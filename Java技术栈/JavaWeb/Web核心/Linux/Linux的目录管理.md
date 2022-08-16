- Linux命令 --- 关机命令，这里主要依赖的语法是shutdown

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4fe48c19b6b6110dd5651e2889ca2ea9.png)

直接使用shutdown命令，在centos6以前的版本中是立刻关机，而在centos7中（也就是我们现在的版本里），是延迟一分钟之后关机，我们可以在这个时间里给出取消关机的命令，语法是shutdown -c

如果我们要立刻关机的话，我们应该要使用shutdown -h now，这个命令会令其立刻关机

我们还可以用这样的语法shutdown +数字 "警告信息字符串，由用户输入"，这个命令会使我们的Linux系统在我们指定的分钟数之后关机，并且给出警告信息，这个警告信息是由用户自行设定的

如果我们想要令其再指定时间里重启并给出警告信息的话，可以用shutdown -r +数字 "警告信息字符串"

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE087b801e66c7eb401224aa032b938935.png)

- Linux命令 --- 重启命令，这个我们只记住使用reboot可以直接重启就完了，其他的只做了解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2ef354f8bd4a80c02129d2ea4d5b16cc.png)

- Linux命令 --- who命令，这里主要依赖的是who命令，该命令可以显示当前登录系统的用户，有什么用呢？比方说我们关机了，但如果还有人在使用服务器的话，这样就会给另外一个人造成影响，但我们先查看当前登录系统的用户，确定了没人登入之后再关机，这里就要用到who命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc931f4cb58786f8ff43eda605ef164a7.png)

直接输入who，会在界面中显示当前登入的用户的名称，线路，时间，和ip地址

输入who -H，同样会在界面中显示上述内容，不过上面多了注释行，用于解释每行对应的内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6b050f9f7210b9e2ae89a7aeba352fd6.png)

- Linux命令 --- timedatetl命令，这里主要依赖的已经是timedatetl，这个命令的作用是用于校正服务器的时间或时区。我们之前学习过date命令，该命令也有同样的作用，但这个命令是手动输入的，难免会有误差，如果我们想要准确的时间，我们就应该利用timedatetl命令从ntp时间服务器中获取到准确的时间

直接输入timedatetl，可以展示出以下信息（输入timedatetl status也有同样的效果）

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc982fe243fd430b479a8737c69b403ef.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2acb6eea13dbd391f12982627e25d79e.png)

最后科普几个小概念

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa3f206009893e47255b13bc72d56dbcb.png)

- Linux命令 --- clear命令，这个命令就不多谈了，用多了，值得一提的是这个命令不是清空屏幕，而是将内容顶到屏幕上面去让我们的界面变得清爽

- Linux命令 --- 目录常用命令，目录常用的命令主要有以下八个，我们接下来一个个介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE97216de4616094d1aafb6416725ca6af.png)

首先是ls命令，ls命令的主要作用是列出目录，其就相当于是windows系统中打开文件夹看到的目录以及文件的明细

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdb92f66891dd39159b0be46e0709be7b.png)

直接输入ls会只在界面中展示目录中的文件，而输入ls -l则会在界面中展示目录中的所有文件的详细信息

输入ls -a会在界面中将隐藏的文件也一并展示，但是比较简介的版本

但我们一般是将上面的两个命令合并起来用，就是ls -al，其表达的意义是展示目录中的所有文件（包括隐藏文件），并且以详细信息的方式来进行展示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4a2860c83173e3881585db132c09b1b2.png)

最后我们来科普下详细展示信息里的各项信息分别代表的意思

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbd056d6315c2cca979c46540be47ffb7.png)

注意这里名称里以.开头的代表是隐藏文件，权限相关的内容后面再讲，这里先简单了解下

- Linux命令 --- pwd命令，这个命令我们我们只要学习输入pwd -P或者谁pwd就行了，输入pwd或者pwd -P，可以在界面中显示出当前所在的目录，这个命令的主要作用也是用于查询当前所在的目录

- Linux命令 --- cd命令，这个命令的作用是切换当前的目录，切换方法分绝对路径和相对路径

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE48df8275e91f5aaf48be0cb61b906cf4.png)

简单来说相对路径就是从本路径开始不断进入下一个路径，而绝对路径只要输入全部的路径名，就可以直接进入到指定路径

这里提一点，我们创建的用户都是存放到home目录下的，想要返回上一级的目录，可以使用命令cd ..

接下来我们来讲讲绝对路径的使用方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE660993d210e4b48f462eb5e987619b24.png)

这类要注意的是我们第一个/表达的意思是根目录，后面的/表达的则是目录转换符，虽然说我也不理解为什么第一个/就不用加目录转换符就是了，不过这里直接记着就可以了。可以看到如果我们要切换到根目录，我们直接cd /就可以了，这里的/代表的其实就是根目录的绝对路径，因此可以直接切换到根目录

- Linux命令 --- mkdir命令，调用这个命令可以创建一个文件夹

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE99d28b0547cc8bd293be239dc1a7a28a.png)

如果我们想要创建名为aaa的文件夹，则语法是mkdir aaa，如果想要创建一个文件夹里还有一个文件夹的文件夹，则语法是mkdir -p aaa/bbb，其代表的意义是会在aaa文件夹里再创建一个文件夹bbb，如果aaa不存在，会将aaa也一并创建出来

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa7ff437c2fdab5a90104d1f085fc0bd0.png)

- Linux命令 --- rmdir命令，该命令用于删除空目录，注意，这个删除能够执行的前提是文件夹内部是空的。我们还有另外一个命令，其将文件夹内部的文件夹删除之后还会判断删除之后的父文件夹是否为空，若为空则一起删除掉

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaee4f637f967e8c6bfbb1cb4f8913569.png)

- Linux命令 --- rm命令，该命令可以用于删除文件或者是目录

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5efd787d67cc5e69339caf3b7521724f.png)

这里简单提一个知识点，调用toucch a.txt命令可以创建名字为a的txt文件，会默认创建在当前用户的所在目录中，实际上，我们可以生成各种名字和格式的文件，这取决于用户填入了什么

如果我们要删除这个txt文件，我们只要输入rm a.txt就可以了，同理我们要删除某个特定文件也是输入对应的名字和文件格式名

如果我们要删除某个目录的话，我们只要输入rm -r 目录名，rm命令的删除会直接删除指定目录，不会管目录中是否有文件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE36bc6671fdcabcdf8d41d2745a132758.png)

- Linux命令 --- cp命令，该命令的作用是文件复制，其语法是cp [选项] 数据源 目的地

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE374e6c4565f6eb54a8e49169beab8d56.png)

如果我们想要复制一个在aaa文件夹下的文件a.txt到文件夹ccc中，则其语法是cp aaa/a.txt ccc

如果我们想要复制aaa文件夹下的所有文件到文件夹ccc中，则其语法是cp aaa/* ccc，但是这种方式会跳过文件夹的复制

如果我们是想要连文件夹也一起复制的话，则语法是cp -r aaa/* ccc

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE015bb8fc9f6df4889f3a5e5a888e1ab1.png)

- Linux命令 --- mv命令，这个命令有两个作用，一是移动文件，相当于windows里的剪切，二是对文件进行改名

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6fc0d8b90b27c762bb2e72d031345fac.png)

如果我们想要移动aaa文件夹的所有文件到ccc文件夹的话，其语法是mv aaa/* ccc，这里我们就不需要-r了

而mv ccc/a.txt aaa，其语法代表的意义是移动ccc文件夹中的a.txt文件到aaa文件夹中

接下来我们来看看各种不同的命令格式的实际代表意义

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7516ff7caadfe106591d1a4e8ac30003.png)

- Linux文件的基本属性

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5c6060448640c243be4f78ede0b2e59a.png)

我们之前调用过ls相关命令展示出所有文件的详细信息，其实这里的详细信息就类似于windows中的文件属性，接着我们来讲讲关于权限的相关知识，先来看看其各种字母表达的意思吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5970f8da59bd7c6006ee625ff6a14a81.png)

我们的文件的表示方式总是rwx的形式的，其中哪一个变成了-，就说明没有该项权限。第一个可以为l，其代表链接文档，其意义就类似于我们windows里的快捷方式

2-4位代表属主权限，也就是创建其的用户的权限，5-7代表属组权限，也就是创建该文件的用户的所在组的其他成员所有的对这个文件的权限，而8-10位则代表其他用户，既不是其创建者也不是与创建者同属一组的的用户所拥有的权限

最后我们做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe42af4430f5bc04dda7134fefba44f2d.png)

我们在实际的生活场景里总是会遇到我们需要其他用户拥有某些文件的权限的时候，比方说我们希望其他用户也有等同于创建者/创建者所在的组的权限，此时我们可以将该文件所属组或者是所属用户给更改来达到这个目的。接下来我们就来学习更改文件的所属组的命令

- Linux命令 --- chgrp命令，该命令可以更改一个文件或者是目录的所属的组

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1bcdd82d3daf39892ef878fff8993645.png)

假如我们想要修改aaa文件夹的所属组为root，那么其语法是chgrp root aaa。如果我们输入chgrp -v root aaa，那么其不但会将aaa所属组修改为root，还会打印一个提示信息，提示更改组已经完成

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5a18173de7f80c2d7af4cc81dee275c8.png)

- Linux命令 --- chown命令，该命令不但可以更改文件的属主，还可以一并更改其属组

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa952213c695b57be3d2247ca86360d68.png)

假如我们想要修改aaa文件夹的所属主为root，那么其语法是chown root aaa，如果我们想要将aaa的所属主和所属组一并修改，那么其语法是chown root:root aaa。如果我们想要修改aaa中的所有文件包括aaa的所属组和所属主的话，那么其语法是chown -R root:root aaa

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE47639ea298b89c9efd724bb527796de1.png)

但实际上，还有另外一个想法可以完成我们的需求，那就是直接更改文件的权限，接下来我们就学习更改文件权限的命令

- Linux命令 --- chmod命令，该命令可以修改某个文件的开放给不同用户的权限

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE92bb49976c1b2fc5b2038e4a1be9a8d1.png)

有两种修改方式，分别是数字方式和符号方式，我们这里先来讲数字方式

- chmod命令的数字方式，在学习数字方式之前，我们要先理解数字表示权限的方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaf9f12a67354ba7242234d347a628141.png)

这里设置数字5就代表其只有读和执行的权限，了解了这些内容之后，我们就可以正式学习命令了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0f8d43e43b2d0ed634eac5266f20bbcc.png)

比方说我们希望将aaa文件夹内的所有内容（包括其本身）的权限设置为对属主和属组可读可写可执行，对其他用户不开放任何权限，那么其语法是chmod -R 770 aaa

- chmod命令的符号方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE04475bf1401a022d3ebfa5cc6e856df0.png)

同样在学习符号方式之前，我们也要学习其对应的知识，首先是属主属组和其他用户的表示方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8420553269c3481ce291bfddbf25ff32.png)

其他是修改权限的方法有

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE63642dd87411c9dc110a17022a0e3a0a.png)

这里值得一提的是，如果我们不想修改任何权限，那么可以在对应的运算符号后什么都不加，这也是可行的

同样的，假设我们希望将aaa文件夹内的所有内容（包括其本身）的权限设置为对属主和属组可读可写可执行，对其他用户不开放任何权限，那么其语法是chmod -R u=rwx,o= bbb，如果我们只希望设置某个权限，那么只要在设置时只写入对应的要修改权限的表达符号和要修改内容就可以了

如果我们要修改的所属组所属主和其他用户的权限都是一样的，那么我们还有一个简化的写法，就是chmod -R a=rwx aaa，其表示的意思是将aaa里的所有内容的三个所属权限都修改为rwx

这个-R可加可不加，加了就表示修改某个文件夹的所有内容（包括其本身），不加则说明只修改指定的某个文件夹或文件