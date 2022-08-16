- Linux命令 --- 查找命令，该命令就类似于是windows系统里的搜索命令，这里主要依赖find语法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2ef7a31ae9e2d78a68b1f8e2f641db50.png)

如果输入find . -name "*.txt"，其代表意义是在当前目录及当前目录的子目录下寻找所有的txt格式的文件，如果我们想要寻找指定文件名的文件，只要将前面的*替换成对应要查找的名字就可以了

如果输入find . -ctime -1，其代表的意义是要在当前目录及其子目录下寻找所有一天之内用户操作过的文件，如果想要找两天的，把1改成2就完了

如果我们想要寻找根目录下的而非当前目录下的，需要将.改成/，其他根据我们的搜索需要进行相应改动就可以了

- Linux命令 --- 压缩命令，windows里常用的压缩格式有zip和rar，而后者在Linux系统是无法识别的，因此在Linux系统中是采用zip格式进行压缩的，压缩的命令主要依赖于gzip命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE29f4a8cd4daa2aa4f514925b1830bb05.png)

如果我们输入gzip a.txt，其代表的意义是会将当前目录下的a.txt文件压缩，而且这个压缩会将源文件覆盖掉，如果我们想要压缩当前目录的所有文件，那么可以将a.txt改为*，注意这里同样是覆盖压缩，而且是独立对象的压缩，即假如当前目录下一共有十个文件，那么最终就会生成十个压缩包，每个压缩包对应每一个文件的内容，而不是一个压缩包内压缩了十个文件。而且已经处于压缩状态的文件无法被再次压缩

那么如果我们需要解压文件，那么我们可以输入gzip -dv *，-dv表示的意义是解压文件时显示对应的解压信息，而*代表的意义是所有文件，整个命令代表的意义是解压当前目录下的所有文件并显示对应的解压信息

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2c6a397181cfd5241b451d184c075bd6.png)

还有一个gunzip命令，该命令是真正用于解压缩的命令，在当前目录下解压所有文件的命令是，gunzip *

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdbb98dafe3d187f599b4470aa984b7a7.png)

- Linux命令 --- tar命令，该命令主要用于打包、压缩和解压文件，里面的命令往往比较复杂，需要调用许多参数，因此这里我们也需要对其内部的各种可选参数进行学习

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE70711eb83a7407baa4c54fddf18ea149.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE430bfcb27809ab165e15a1dd91a4164c.png)

- Linux命令 --- zip命令，这也是一个压缩命令，不过有所不同的是这个压缩命令压缩的格式统一只能是zip格式的，同样要压缩某个文件的话，必须先进入到文件的当前目录中

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE383457062863c7aa739277b7f11e6b7e.png)

如果我们输入zip -q -r aaa.zip aaa，这就意味着我们要将当前目录下的aaa文件夹压缩，并且因为其实文件夹，我们同时是要将其内部的文件也一并压缩的，因此加上-r，-q代表不显示该命令的执行过程，最后我们创建的压缩文件的名字就是我们指定的aaa.zip，注意这里是新创建一个压缩文件，而不会将源文件删除或者是覆盖

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE96cc915f0094653e295350e42c84955e.png)

- Linux命令 --- unzip命令，这个命令是一个解压命令，主要用于解压，不过只能够解压zip格式的压缩包

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8093943e430706453c45541c1cdcdeae.png)

如果我们输入unzip -l aaa.zip，其代表的意义是我们要查看aaa.zip内的所有内容

如果我们输入unzip -d bbb aaa.zip，其代表的意义是我们要解压aaa.zip的内容，并且我们指定解压的内容存放到我们所指定的bbb的目录中去，没有对应目录则会创建出来

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf11841465d78069b26dcf434866f5cfe.png)

- Linux命令 --- bzip2命令，这个命令和上面的zip命令大同小异，不同的是这个命令采用新的压缩算法，压缩后产生的文件更小，但是耗费的时间更长，同时其生成的压缩文件后会删除原始的文件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE226cf8354ce9be976576b8461bb1698e.png)

如果我们输入bzip2 a.txt，其意义是我们要用新的压缩算法压缩a.txt文件，最后我们生成的压缩文件的名字是a.txt.bz2，可以看到格式是bz2格式，前面的格式名则被当成了文件名使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfb86cf8bf2734e67c6f209398d21be29.png)

- Linux命令 --- bunzip2命令，同样我们也有解压的命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa2e9461b95a0ea420bcef94f81e7f6ea.png)

如果我们输入bunzip2 a.txt.bz2，其意味着我们要解压指定的文件，如果我们想要解压时同时显示详细信息，则可以输入bunzip2 -v a.txt.bz2

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE759ce9a378d83ce9a534e3ec14746b9b.png)

