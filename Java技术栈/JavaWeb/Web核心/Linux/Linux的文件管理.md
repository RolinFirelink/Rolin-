- Linux命令 --- touch命令，该命令可以用于生成指定的文件，若要生成的文件已经存在，则会更新其对应的时间属性

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4955276f1214650a3c59cfc6025afcb6.png)

输入touch a.txt，会在当前目录下创建一个名为a.txt的文件，a是名字,txt是文件格式

如果想要批量创建的话，可以输入touch a{1..10 }.txt，注意这里先写入touch命令，然后我们先输入文件名，接着输入想要生成的序号的开始和结尾，用两个.分隔并用大括号括起来，然后接上文件格式名就可以正确生成了

这里提一下，调用stat 文件名.文件格式名，可以查看一个文件的详细信息

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEed8c0a999223f08f927a080ebbb2a4c1.png)

- Linux的vi/vim编辑器，简单来说就是程序员常用的工具，其中我们主要介绍vim，简单可以理解成windows中的文本编辑器，其中vi和vim的区别如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9ee47a081f62ad7896e11b2bc7bf24bf.png)

如同windows中的文本编辑器有三种模式一样，vim打开文件之后也有三种模式，分别是命令模式（只能读不能写），编辑模式（能读能写），末行模式（保存修改内容）

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa99cc32ecfbf82dd242c0e64b2a64119.png)

这三者的进入和退出的方式如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7d1470a8cabe3dc5a9bc3fb3c2ac1c68.png)

- Linux命令 --- vim命令，该命令可以用于打开一个文件并进行相应的编辑，如果文件已经存在，则会直接进入，若文件不存在，则会打开一个临时文件，同样进行相应的编辑之后可以选择保存与否，选择保存退出则会新建一个文件。注意冒号是shift+:的方式来使用的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE832a091453b0a10c376fc4b9e9710455.png)

而在进入文件之后，我们先进入的是命令模式，如果要进入编辑模式，则要按下对应的按键

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf0bbd6c212e4684b5ee2701e72a18a7b.png)

虽然看起来很多，但实际上只记住一个i就可以了，其他的比较少用

当我们进入编辑模式后，就可以进行对应的编辑了。当我要从编辑模式中切换到末行模式时，我们是要先按ESC返回到命令模式，然后按:进入末行模式，接着输入对应的命令就可以进行对应的保存操作了。末行模式里可以使用的命令如下所示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6e23085314d7c7943cc97b7f0db909db.png)

最后我们可以做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb2af14da9a53a5b65d6dfabb09255f1b.png)

- Linux文件查看相关命令，如果我们只是想要查看文件而不进行编辑的话，那么我们有更加方便的命令，这里的命令有五个，我们分开来讲

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEeba04ad681619c6b45a21a89ccfb2433.png)

- Linux命令 --- cat命令，该命令可以直接在控制台上打印出文件的所有内容，超越了屏幕就直接往下顶，直到全部打印出为止。还有一个cat -n 文件名.格式名的命令，可以打印出内容的同时显示行号

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8c483d95d30272912676fd9ddedb8a50.png)

- Linux命令 --- less命令，该命令可以查看文件的内容，但其不是在控制台上打印出来，而是将文本内容展示出来，用户可以通过↑↓按键自行阅读，退出则按q，加上-N可以显示行号

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb7b89067b06600ab1be58d6096d0593c.png)



- Linux命令 --- tail命令和head命令，我们先来讲tail命令，该命令可以查看文件的最后部分

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4732ef8d8813048a17f55ea47caf51c4.png)

如果我们输入tail a.txt，那么会默认打印该文件的末位十行内容，如果我们想要指定打印的行数，就应该输入tail -数字 a.txt，输入对应的数字可以指定末位需要打印的行数

如果我们输入tail -f a.txt，其会动态展示文件的末尾十行内容，比方说我们打开的是一个日志文件，那么我们可以在展示界面中不断看到其写入的内容，按ctrl+c退出。如果我们想要指定其动态显示的行数的话，我们只要在f前面加上数字就行了，比如tail -4f a.txt，则意为着动态展示a.txt文件的末位四行的内容

如果我们输入tail -n+2 a.txt，该命令会将文件从指定的行数开始打印直到末尾，这里的意思是从文件的第二行之后开始打印直到末尾，如果我们输入其他的数字那么其也会对应从我们指定的行数之后开始打印

如果我们输入tail -c 45 a.txt，该命令会将文件的最后的45位字符正确打印出来且会自动分行，我们可以通过改变数字的大小来改变我们所需要打印的字符数量

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2deb6b661aa48f638c6cd0e636ad0d90.png)

而head命令和tail命令其实是差不多的，我们依葫芦画瓢就行了，这里就不再演示了

- Linux命令 --- grep命令，该命令主要用于查找，其不但可以用于查找文本内容，还可以用于查找进程

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE594fe5a53840b8c94fd36a6b03c28c93.png)

如果我们输入grep 日 a.txt，那么该命令会将a.txt文本中含有“日”字符的行给找出来并打印到命令行上。如果我们还想要其显示行号，那么命令应该改成grep -n 日 a.txt，这样打印的符合条件的行中就含有行号了

如果我们输入grep -i a a.txt，-i这个命令会令我们查找时忽略大小写，那么其会将文本中含有a或者A的对应行全部打印出来

如果我们输入grep -v 日 a.txt，这个命令会将除了含有日字符的行之外的行全部打印出来，相当于是一个排除命令

接下来我们来讲如何用grep命令查找我们所需要的进程，这里要与ps命令结合起来

比如说，我们输入ps -ef | grep sshd，就会出现以下结果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEadd52af9d2aab53376f19886536e2841.png)

可以猜测这个命令的意义是先查出所有的进程，然后中间的 | 应该意味着要执行下一个命令，而后面的grep命令则起到了从这些进程里查找的作用，这里我们查找的进程是带有sshd字符的，所以最后我们的打印出来的进程也的确是带有sshd字符的，最后我们可以看到最后一个sshd进其实是我们的查找进程，如果我们不想要看到这个查找进程，那我们应该怎么办呢？我们可以输入ps -ef | grep sshd | grep -v "grep"，这里则代表最后会在前面的结果的基础上再移除掉含有grep的进程，那么最后就会只显示前面三个进程了。如果我们只是想知道进程数量，而不想知道其具体进程，我们可以输出ps -ef | grep -c sshd，那么最后其就会打印数量而不会打印详细信息了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE605cb28ea7d82ceae6c13a0a802a187d.png)

- Linux命令 --- vim定位行，该命令语法是 vim 文件名.文件格式 +数字，使用该数字打开文件可以让光标在指定的行数中出现，而不会出现在第一行中。用该命令主要可以快速定位到我们认为的需要修改的行中

- Linux的vim的异常处理，在我们编辑文件时，如果我们编辑文件没有保存，而我们的Linux系统又直接关了，这样我们如果要再次编辑同样的文件，其就会出现异常提示信息，该提示信息会提示说查找到同名的swp文件，请求处理方式。为什么会这呢？这其实是因为我们的Linux系统为了保证源文件的安全，其编辑时并不是直接在源文件上编辑的，而是先生成一个swp格式的交换文件，当我们保存文件时，其该swp文件才会被删除掉，并且对源文件进行修改。而如果我们直接在编辑时退出系统，那么swp文件就不会被处理，因此会提示异常信息。我们处理该异常的方法很简单，一是按A退出然后调用rm方法删除该swp文件再进行修改文件，二是在异常提示中直接按D删除，就可以继续正常编辑文件了

- Linux命令 --- echo命令，该命令可以用于展示输入的字符串以及将我们写入的字符串写入到文件当中

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd7025c3cc8aad057e9c097dc96e05753.png)

如果我们输入echo "用户输入字符串"，那么会直接在控制台上打印用户写入的字符串

如果我们输入echo "用户输入的字符串" > 文件名.文件格式，那么该命令会将我们写入的字符串覆盖写入到我们指定的文件中

如果我们不想要覆盖，只是想要继续写到该文件的后面那我们该怎么办呢？我们可以输入echo "用户输入的字符串" >> 文件名.文件格式

如果我们希望将报错信息不覆盖写入到某一个文件中，我们可以输入cat 不存在的文件 &>> 文件名.文件格式，这样就可以写入了

- Linux命令 --- awk命令，这是一种用于处理文本文件的命令，往往要与其他命令组合使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc1acb57a010fb3b08849196b12aeee88.png)

比方说，我们有一个文档，文档里存放了一些姓名和数字，如果直接用cat a.txt可以直接查看所有内容，但如果我们只是希望查看符合条件的部分内容的话，我们可以用cat a.txt | awk '/zhang|li/'，这个表达的意思是其会先获得文本中的所有内容，然后将文本中存在"zhang"或"li"的字符串的一行给展示出来，注意，我们在awk中的两个//中间写入的条件，在中间的 | 上是不用加空格的，如果我们加了空格，那么其会将空格一并当做条件用于搜索。而且可以看到我们的awk语法后面是加了单引号和两个斜杆，之后再写入具体命令的，这点也要记下。

awk命令的第二个作用是将每一行按照指定方式切割，并且可以返回指定行的内容。在学习这个语法之前我们要先学习其相关知识

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe3cd28aaa792c87ba73d1705cc9676c1.png)

首先使用-F ''命令可以知道你使用指定字符切割，''要输入我们要用于切割的字符，图中使用的是逗号，因此是','，而我们使用的是空格，因此我们要写入' '，其次是切割之后其会将切割后的内容用$数字的方式来表示切割的第几个纵列，从1开始。而最后我们输入$数字第几行的内容就表示我们要打印哪一个纵列的内容

那么学习完上面的知识之后，我摸容易构造出cat a.txt | awk -F ' ' '{print $1,$2,$3,$4}'的命令，这个命令表示的是我们先获取到a.txt中的内容，然后调用切割命令，我们用空格将其借个，然后我们要打印我们指定的纵列，打印命令我们放到单引号里面写，先输入大括号，大括号内部输入print打印接着跟一个空格之后写入要打印的纵列，用逗号隔开。最后我们得到的内容和直接用cat a.txt是一样的，虽然是一样的，但是其内部执行的过程却是不同的，这点要搞清楚

如果我们希望我们切割之后的字符串按照一定的规则打印出来，此时就要用到我们的OFS语法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE08ae21dd469253a353d3c5333a6ee928.png)

其语法是OFS="字符"，假设我们希望我们的字符串切割后对于每一个纵列都用===间隔，那么我们的语法就是cat a.txt | awk -F ' ' '{OFS="==="}{print $1,$2,$3,$4}'，语句本身就不再解释了，这里要注意的是，两个大括号之间不需要口空格，单引号内部的第一个大括号里写OFS语句，=号后面用双引号写入自己用于间隔的字符串。如果我们想要用制表符来间隔的话，只要把上面的===改成/t就行了

如果我们还有更多的需求 ，比如说我们希望返回第一个字符串的大写形式，小写形式或者是长度的话，我们就应该学习进一步的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf408b294c909f9605903825f2ad460e1.png)

这里面其实有点调用函数的意思，这些函数方法都是awk里的方法

如果我们希望将字符串的第一个切割内容转成大写并打印，那么我们应该输入cat a.txt | awk -F ' ' '{print toupper($1)}'

如果我们希望将字符串的第一个切割内容转成小写并打印，那么我们应该输入cat a.txt | awk -F ' ' '{print tolower($1)}'

如果我们希望将字符串的第一个切割内容转成字符数量并打印，那么我们应该输入cat a.txt | awk -F ' ' '{print length($1)}'

接着我们就要实现更加复杂的需求了，同样的我们也要学习新的知识

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8e4b2e542b0d896409f9fff411fa9df2.png)

对于第一个需求，我们可以输入代码cat a.txt | awk -F ' ' 'BEGIN{}{totel=totel+$4}END{print totel}'，这个语句表达的意思是其先获得文件中的内容，然后将文件中的内容分割开之后一行一行获取到（这里位置执行的是BEGIN的语句），每获取一行语句，我们就会将其第四纵列的数字加到totel变量中，之后等到BEGIN语句结束之后指定EDN语句，此时打印出totel，就是我们所需要的第四列的总分了

而对于第二个需求我们可以输入代码cat a.txt | awk -F ' ' 'BEGIN{}{totel=totel+$4}END{print totel,NR}'就可以了，NR代表的意思其实是处理到第几行的行号，而好巧不巧我们这里的行号正好能够代表人数，因此这里用行号来表达人数正好

对于第三个需求我们可以输入代码cat a.txt | awk -F ' ' 'BEGIN{}{totel=totel+$4}END{print totel,NR,(totel/NR)}'就可以了，我们求平均分的方式其实就是简单的将总分除于人数再打印而已

- Linux系统的软连接，所谓软连接，其实就是快捷方式。值得一提的是在Linux系统中，文件的名字和文件的数据是分开存储的，因此其软连接打开文件的方式是先通过软连接然后找到对应名字的地址，接着找到文件的名字然后通过文件名字找到文件中的数据

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEce92dec7cf9c3b0a1ac6a2d4fa3005da.png)

其语法是ln -s

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE908f520b50ed7c1779704bbce23eae93.png)

比方说我们写入如下语句

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd23718b7efda392ef7e0aebca15a856c.png)

第一行是我们写入的语句，ln -s 具体路径 要生成的快捷方式名字,这样就会在其当前的目录下生成一个指定名字的快捷方式，这个快捷方式可以直接打开目标文件（其实名字到底能不能自定义是我猜的，我猜测其可以自定义，但我不保证对）