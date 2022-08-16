接下来我们就学习Linux基础中的最后一个内容，Shell

- 初识Shell，那么什么是Shell呢？其实Shell就是一个命令解释器，是位于操作系统和应用程序之间的一个解释器，其作用是将应用程序的输入命令信息解释给操作系统或者是将操作系统处理后的结果解释给应用程序。简单来说，其实Shell就是在操作系统和应用程序之间的一个命令翻译工具

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE66db040bc2cad3c9d2b96126cb97bfd4.png)

Windows中的Shell其实就是cmd，也就是命令提示字符。而在Linux系统中的shell则有很多种，但我们只需要记住，Linux系统默认的shell都是bash

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE62546aa367f401ee0e2b23c64c49f187.png)

如果我们想要查看我们当前的shell，可以输入echo $SHELL，这个语句中的SHELL是变量，至于什么是变量，别着急，这个我们很快就会学到了，这里我们先记住。按下回车之后就可以看到我们的shell了

那么现在还有最后一个问题，就是我们的shell要怎么使用呢？一般而言，有两种方式，一种是手工方式，一种是脚本方式，所谓手工方式就是直接手打输入，而脚本方式是先将一系列需要执行的命令写入到一个文件中，然后执行这个文件，达到执行命令的效果，这个文件就叫做脚本文件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEeacdb1579da5c380967edf605bf478fb.png)

- 编写第一个shell，那么现在我们就要来编写第一个shell文件了，我们编写这个命令文件，然后可以通过执行这个命令文件来执行我们所需要的命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE388c34c170b4e54cb990b40bef25223d.png)

注意看我们这里的第一行的内容是指定我们现在用的shell是bash，而第二行是注释，后面两行则是比较命令内容

我们首先输入touch a.sh，生成一个指定的文件，接着我们输入vim a.sh，就可以进入到这个文件中进行编写了，我们输入指定内容，然后输入wq!保存文件，接着我们输入./a.sh就可以运行了，但是我们会发现其报出了权限不足的问题，我们查看其权限会发现我们生成的文件不具有执行的权限，因此我们还有修改这个文件的权限，输入chmod 777 a.sh，就可以将其权限全部修改为允许，这时我们再运行就没有问题了

- shell注释，注释相关内容很好理解，这里就不多提了，直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe6f06fd9ca4834d96a062302201cf348.png)

注释分单行注释和多行注释，单行注释就直接加#就完了，多行注释的话则是按照:<<字符 内容 字符，这样的格式来进行定义，虽然说这个字符只要保证前后一样不管写什么都可以，不过一般我们的字符是使用!来进行代替，这样易于我们的程序员的理解

- shell变量，这是shell的第二个语法，其内容比较多，我们一个个来

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3c32e067b923bebbe95bab4a49ceb436.png)

- 定义变量，定义变量有三种方式，第一种方式是变量名=变量值，但这种方式要求变量值必须是一个整体，中间不能有特殊字符（如空格），第二种方式是变量名='变量值'，这种方式会将单引号中的内容原封不动的赋值给变量，第三种方式是变量名="变量值"，这种方式的特点是如果双引号中有其他变量，其会先获得变量的结果与原来的内容进行拼接，然后赋值给变量。一般来说，如果我们的变量是数字，我们选择第一种方式，如果不是，则选择第三种方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE99cfa59126eedf88241746d7b10c862e.png)

如果我们在我们的a.sh文件中写入以下内容

#!/bin/bash

# #这是临时的shell脚本

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE12113fc9ef7c751e980d483f102ff46f.png)

假如我们在a.sh中输入以下内容

c=`date`

echo $c

d=$(date)

echo $d

那么按下回车之后其就会正确显示两次当前的时间，其本质是先调用date命令获得当前时间，然后将时间赋值给对应变量，然后输出变量的值。我们之前学过启动脚本命令的方式是./a.sh，但其实我们输入bash a.sh，也是同样的效果

- 使用变量，使用变量有四种方式，第一种方式是$变量名，这也是我们之前一直用的方式，但这种使用变量的方式是不标准的，第二种方式是"$变量名"，第三种方式是${变量名}，第四种方式是"${变量名}"，第四种方式是最标准的使用方式。我们这里只需要记住第三种和第四种方式就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEae9270d100297be876c38601d0c9f9d4.png)

比如说我们在文件中写入以下内容

result="现在的时间为${d}"

echo "${result}"

这样我们运行的时候其就是先使用变量，将获得变量的值然后与前面的字符串拼接然后赋值给result，接着我们在打印result变量的值

- 只读变量，其实就是加个readonly就完了，一旦一个变量被readonly修饰，那么其就不能够再进行任何的赋值操作，否则会报错。比如果我们写入readonly result，再写入result="aaa"，那么执行这个脚本文件，会报错

- 删除变量，其实就是加个unset就完了，一旦一个变量被unset修饰，则说明该变量被删除了，此时如果调用该变量则无法获得任何值

- shell数组，这个内容在java里已经品鉴得太多了，就不重复叙述了，直接上结论和测试吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE32d585ba3d218f7e12b12d0dd24c3849.png)

接下来是测试内容，假如我们写入了如下内容并运行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3daa780c70a534044f3a55b5534f891f.png)

那么我们可以的得到如下内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2f97bd2d6a7d438b984bf14a915e59c6.png)

这里值得一提的是虽然说echo{#arr[*]}和echo{#arr[@]}都可以获得数组的长度，但我们一般倾向于使用第一种方式来获取数组长度

- shell算术运算符，这个也是品鉴得足够多了，直接来吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd2949e61be84e11b321223c6f1062529.png)

这里要注意的是由于原生的bash不支持简单的数学运算，我们可以通过其他命令来实现，这个命令就是.expr，其次是表达式和运算符之间要有空格，最后是完整的表达式要被反引号包括。我们可以举个例子`expr 2 + 2`，这个例子就满足了上面三个条件，是一个正确的表达句

假如我们输入以下命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE426dedd1d29f04b09c113303a983cc16.png)

这里值得一提的是我们的乘法运算是加入了一个\的，这个\用于转义，因为在Linux系统中的*其实有其他的意思，因此我们这里是要将*进行转义，令其意义变成一个无比单纯的乘法运算符

假如我们又输入以下命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE70377743771330aec3f0e7e35053f7ab.png)

这类值得一提的是我们的自增命令只要加两个括号的

最后我们能得到结果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb21986cf26c1f5fc1cac3a20697cd8bb.png)

- shell字符串运算符

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE568b03ba23be73f6052a627580236281.png)

这里要注意的是我们调用运算符的命令时都要放在中括号内，且中括号的前后都要有空格，换言之，即使不能够输入[$a]这种命令，是会报错的

假如我们先定义了a="aaa"，b="bbb"，接着我们输入以下命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa2a5c4e2233074da07360b402ddd9c0a.png)

这里我们要提一下的是，$?可以获取上一个变量的结果，其次是在Linux系统中，1为假，0为真，不用true和false表示，而且居然还和我们一般想到的1为真和0为假反过来的，我真的吐了。还有就是注意到当我们使用判断字符串长度是否为0时我们要写入-z，这里我们-z写在前面而且是前后都带有空格的。接着我们继续写入如下命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7d53417c5067f0cdf8f923080540e7ca.png)

最后我们就可以获取到下面的结果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb2118271916a628b8432337a5c7fd95a.png)

这里值得重点提一下的是我们获取字符串的长度的方法是"${#a}"，这个方法和获取数组长度的方法很像，是额外我们增加的知识，也需要记忆

- shell关系运算符，注意这个关系运算符只能用于判断数字，不能用于判断字符串，除非字符串的值是一个数字

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE696a79390cad99c2cacfbf31abfafa56.png)

我们就以第一个运算符为例，写入如下命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0efa45c360031b6584c96246b940b6fe.png)

最后得到的结果必然是1，为假。其他的就依葫芦画瓢了，这里就不再赘述了。至于这类的记忆方式，我觉得用英语方式来记忆是最好的，比如大于符号是grater than，那么其命令语句就是-gt，这样就很好记忆了，这一节里其他的类似命令也可以这样记忆

- shell布尔运算符

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6b7ec160fba5a70a0b9b9f29678b634e.png)

这里取反其实就相当于是java中的!，而-o则相当于java中的||，-a相当于是java中的&&，我们可以写入如下代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE449f50880fe3ae023b3ac9f63e453aea.png)

最后我们获得的结果会是

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc6c50d95adfa5f0dee8cac3d27b06217.png)

- shell逻辑运算符

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe28fc473c811bed6fa0adcd55ae17df9.png)

和java中的&&与||基本没有分别，与上一个布尔运算符不同的是逻辑运算符必须写在两个中括号之中

比方说我们输入如下代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa5c5ae4cb8f9e6a2c50c49eb57173cf3.png)

其结果会是1和0

可以看到我们的逻辑运算符都是放到了两个中括号之中的

- shell判断语句，这个判断语句就类似于我们java中的if语句，其用法也跟java中的非常类似

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2495e61f220642fc8741af31317ff2b2.png)

不同的是在Linux系统中，我们是用[]来放置判断条件的，而进入if条件后面要加上then，结束时要加上fi，而且代码块直接不需要大括号，最后是else if的语句被合并成了语句elif并加上判断条件

我们可以写入如下代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc673fef627a9e29322c371eaf4371709.png)

这里我们解释一下$(ps -ef | grep -c "ssh") -gt 1的意思，其意义是获取所有的进程，然后过滤出所有带有ssh关键字的进程，接着获取这些进程的数量和1进行比较，若大于1则进入判断条件 

再写入如下代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2e9b6998ff44ea10c78cef2cbdda4469.png)

最后我们得到的结果是true,true,a小于b

- shell选择语句，其实就是Switch，不赘述

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3af142ebe6ad3d093ab1a91e3338738c.png)

不过这里仍然值得一提的是这里我们只要先定义一个变量，里面存储值，然后其会每次将变量的值取出来与我们在括号内定义的值进行对比，如果相等则会执行下面的语句。而;;就相当于是java中的break，最后esac则是结束语句

- shell循环语句，其实就是java中的for和while循环，我们先来讲for循环

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEebf29d1e2a72217a39213e9bc28ee3d0.png)

这里要注意的是for是我们定义的变量，而in后面则是其变化的范围，这里我们规定其变化为ABC，那么其会先变为A，然后执行do里面的语句，然后变为B执行......直到循环结束，注意我们定义的范围直接是要有空格的

接下来我们来讲while循环

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd8591953844ee1622a0f88bd03df6446.png)

while循环要注意的是，当条件满足时，我们就继续执行里面的语句，而当条件不满足时，我们就会退出循环，上面右边的代码就演示了打印1-10的数字的代码

- shell函数，这里的理解也是类似于java，这里一共有三种函数，分别是无参无返回值，有参无返回值以及有参有返回值，我们接下来就分别学习这三种方法

首先是无参无返回值的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEac939f95805efbff2b298a6b7850b86b.png)

这里要注意的是，函数被写出来还需要调用的，我们调用无参无返回值的函数直接写函数名就完了，不需要写其他的

接着是有参无返回值的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5bce916a06ad9e1d157fddc70bc33d9c.png)

注意有参无返回值的方法调用时不但要写函数名，而且要传入对应的值，可以传入变量，也可以直接传入值。而在定义时，与java中直接在小括号内写参数的方式不同，shell中是要在函数体中用echo来接受对应的参数（我怀疑其实字是可以省略的，直接写$1和$2都可以），然后$1和$2接受到的参数的到底是什么取决于我们传入的顺序

在有参有返回值的函数中我们还可以在return中做对应的运算再返回。这里我们最后echo $?可以获得上一个命令的结果，其最后会准确将函数的值打印出来

最后我们来看一个练习

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe0ba1a4ad66bf5bc0691be950e4a0f60.png)

这个练习最主要的作用是告诉我们read可以获取用户输入的数字并将其赋值给我们定义的变量，后续操作中我们还可以使用这个变量