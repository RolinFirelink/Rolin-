本章节我们来学习Git，因为Git是程序员的必备管理工具，因此学习SpringBoot之前，我们先来学习他

- 学习目标

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7f5464f8056d33d80f9f6b6b35615dfe.png)

这里我们学习最终要的就是最后一点，就是使用idea来操作git

- 版本控制器的方式

我们Git的作用有以下四点

1. 备份

1. 代码还原

1. 协同开发

1. 追溯问题代码的编写人和编写时间

其之所以能够实现这些，是通过版本控制达到的，简单来说就是每个人写一份都添加一个版本号。

我们有两种版本控制器的方式，一种是集中式版本控制工具，比如说SVN和CVS，其原理是将代码都放到一个中心服务器中，然后程序员需要就往中心服务器里拿，但是这样的话，如果哪一天我们的中心服务器突然寄了就全完了，断网了也是全完了，不方便。

而我们的Git则是分布式版本控制工具，其没有什么中央服务器，每个人的电脑上都是完整的版本库，无需联网，多人协作时只要将各自的修改推送给对方就完了，任何两个人之间都可以互相推送自己的代码版本，不过虽然可以这样做，不过一般我们都还是有一个共享版本库来控制的。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe7e232671f6fe8a671a9234feeaaf2cb.png)

最后我们来看看Git的特点

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE01e1cbcb597c39a491ea3f3ccdce9a73.png)

- git工作流程简述

简单来说，git有远程仓库和本地仓库的概念，其他的我们先了解下就完了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb4387a14dfc2d5982553817dba98770b.png)

- git环境的配置与安装

接着我们来学习git环境的配置和安装，首先我们要提一嘴，我们安装过程中是会用到基本的Linux的命令的，因此我们提前将其列举了，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEff0906450d81f6f6b979480fcaf36bf7.png)

首先是安装，安装其实就是傻瓜式安装，我们一直安装就完了，如果安装成功了我们右键点击桌面可以看到Git GUI和Git Bash，前者是Git提供的图形界面工具，而后者是命令行工具

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2d5d7b89d4024ecaa0285921dfb41e25.png)

接着我们就到了经典的配置环节了，首先我们要配置我们的用户名和邮箱，因为Git每次提交都会使用该用户的信息，因此先将这个两个信息配置好非常重要。我们来看看我们设置的步骤

> 1. 打开Git Bash

> 2. 设置用户信息

> git confifig --global user.name “Rolin”

> git confifig --global user.email “2259286198@qq.com”

> 查看配置信息

> git confifig --global user.name

> git confifig --global user.email

这里值得一提的是，我们这里设置要使用的双引号是中文的双引号，而不是英文的，这点和我们之前的习惯背道而驰。同时，我们直接在里面打这些对应的命令总能出各种稀奇古怪的异常，我这里推荐的方法是在文本中先打好自己的命令代码，然后拷贝过去进行一个执行就完了

接着我们来整一个本地仓库，要使用git必须要搞这一步。我们在任意位置创建一个我们要将其作为仓库的文件夹，然后进入该文件夹中右键打开命令行工具，执行命令git init，然后在对应的仓库中看到其生成了一个隐藏文件夹.git，就说明此时我们的本地仓库已经设置成功了

- git常用命令

接着我们就来讲讲git的常用命令，在讲解之前，我们要先来学习Git的工作状态。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE984acad0f71f27fe20220a413aa51661.png)

首先我们要理解工作区的概念，除了我们的.git文件夹之外的文件都数去工作区。然后如果我们新创建一个文件，那么其是处于未跟踪状态的，我们可以用git add命令令其进入暂存区并处于已暂存状态，我们的暂存区是仓库之前的缓存区，我们可以再次调用git commit方法令其进入仓库中

而对于修改文件来说也是大同小异，如果我们修改文件，那么其首先处于工作区中且是未暂存状态，同样调用对应的方法能够令其进入暂存区并最终到达仓库中

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE28b5712f3a06a5090fce1c9ae8200c90.png)

那么接着我们就来做一个简单的案例，首先我们进入对应的仓库中然后创建一个文档文件，我们可以调用命令touch file01.txt，然后我们来查看我们的文件的状态，这需要调用git status，我们可以看到如下内容

> $ git status

> On branch master

> No commits yet

> Untracked files:

> (use "git add <file>..." to include in what will be committed)

> file01.txt

我们可以看到这里显示了一个未跟踪的文件，在里面可以看到我们的file01.txt，这就是我们的未跟踪的文件。

然后我们就令其进入我们的缓存区里工作，我们调用对应的git add .命令，这里的.意为所有，我们将所有未跟踪的文件都进入到暂存区中，实际上我们在后面输入对应的文件的名字也是可以的，我们这里贪方便直接令其提交所有了。然后我们再次调用查询状态的命令，可以看到如下内容

> $ git status

> On branch master

> No commits yet

> Changes to be committed:

> (use "git rm --cached <file>..." to unstage)

> new file:   file01.txt

可以看到我们这里显示的内容是准备提交的文件，里面就有我们的file01.txt，然后我们输入git commit -m "add file01"，注意我们这里要输入-m的选项。而后面的双引号的内容纯粹是注释，便于我们后面查看所加入的，爱写什么写什么

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4d208c2df64f6151c48803bd25533db9.png)

然后我们调用git log方法

> $ git log

> commit 6631dc6073db65619138904ac0304ca47f86cc36 (HEAD -> master)

> Author: “Rolin” <“2259286198@qq.com”>

> Date:   Mon May 16 15:29:10 2022 +0800

> add file01

我们通过这个方法可以看到我们仓库内存放的内容的具体记录，其中commit是唯一标识，Author则是对应的我们的ID和邮箱，Date是提交的时间，最后是我们当初写入的注释

然后我们来讲解下我们的修改的情况，首先我们写入命令vi file01.txt，我们进入到对应的编辑位置中然后随便编辑点啥，接着点击保存，然后同样查看状态能看到如下内容

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file01.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

上面的内容就表示我们的改变了，但是还没有暂存的文件。然后我们同样进行原来的两个步骤，就可以在我们的git log中看到两个我们的历史文件了。

最后我们来单独讲解下git-log这个参数，其下有很多选项，其中--all是显示所有分支，--graph是将信息以图的形式来显示，更多的命令及其作用请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEccc3e0b658d18327ea4046fc357aeb6d.png)

接下来我们一个个来讲解这些命令的具体的选项的作用，首先我们进入一个别人的开源项目里康康，我们使用git log，我们可以看到里面有一大堆仓库的提交记录，这他妈要一个个找就太折磨人了，我们希望其全部都显示到一行中，此时我们可以使用git log --pretty=oneline命令，让所有的提交信息显示为一行，但即使如此，我们的得到的信息展示页面还是太他妈乱了，这里最乱的是就是我们每次提交的唯一标识，它太长了主要，实际上，我们每次使用唯一标识的时候我们没必要每次都拿到其全部的长度，拿到其前五位的就可以了，一般也不会有重复。此时我们可以写入git log --pretty=oneline --abbrev-commit，abbrev代表优化，而后面的commit代表我们要优化的位置，这样我们在界面中得到的唯一标识就简短不多了，我们的页面也变得清爽不少

我们还可以在后面加入显示所有分支的--all，这样令其同时显示所有分支，其命令如下git log --pretty=oneline --abbrev-commit --all，其中分支以红色显示，当然分支是什么我们先按下不表。但是这时只是单纯的显示分支就不够直观，我们希望我们看到其合并分支时具体是哪些进行了合并并成为了一个分支，那么我们可以在后面再加上以图的形式来显示的--graph命令，其命令如下git log --pretty=oneline --abbrev-commit --all --graph，这样最后我们的得到的页面就会直观不少了。

我们以后查看仓库，我们都推荐用上面的命令，然而我们每次都输入这个玩意显然不太方便，因此我们要进行取别名的操作，先进入到我们的用户的文件夹中，然后创建.bashrc的文件，在内部写入代码如下

```
#用于输出git提交日志 
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit' 
#用于输出当前目录所有文件及基本信息 
alias ll='ls -al'
```

其代表的意思就是取别名，这里我们取了两个别名，一个是将git log --pretty=oneline --all --graph --abbrev-commit命令取为git-log，另一个也是如法炮制。然后这时如果我们想要获得更好的页面展示效果的话，我们就只需要输入git-log命令就可以了

那么此时我们的提交日志的内容就讲完了，接着我们来讲版本回退的内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa05bb127ab15eb21f353b1ee3538be34.png)

我们要使用版本回退就要使用git reset --hard commitID命令，其中commitID指的是我们的上传的内容或者是某一个操作的唯一标识，举个简单的例子，我们可以让我们的仓库的内容回到第一次添加的时候，那么我们的仓库就会回到那会，那时我们连修改都没有做。我们也可以再回退回来，只需要输入对应的修改之后的commitID就行了，这个东西我们可以通过之前我们的git log记录中查看到，但可能有一种情况，我们不但回退了，我们还将我们的屏幕给情况了，那我们此时应该怎么办才能将我们的仓库恢复到原来的样子呢？我们可以调用git reflog方法，调用该方法可以查看到我们的已经删除的提交记录，我们利用这个命令可以查看到我们回溯之前的状态的唯一标识，并利用这个唯一标识将我们的仓库恢复到那时的状态

最后我们来说下我们如何添加文件到我们的忽略列表中，我们有这么一种需求，就是我们希望对大部分的文件进行同样的处理，但是我们同时又希望忽略掉一些文件，此时就需要使用到忽略列表

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE570c927372f56174ecaea7a6f8d38c22.png)

我们首先在工作目录中创建一个名为.gitignore的文件，然后写入对应的要忽略的文件名，如实*.a代表的就是忽略所有.a格式的文件，创建完毕并输入完毕之后，我们只要调用对应的执行所有的方法，我们就会发现这些方法自动都自动忽略了我们写入进忽略列表里的文件了。如果我们希望该忽略列表失去效果的话，给其改名或者是删除应该都是可以的。

最后我们再来做一套练习，具体请看下面的内容

> #####################仓库初始化###################### 

> # 创建目录（git_test01）并在目录下打开gitbash 

> 略

> # 初始化git仓库 

> git init 

> #####################创建文件并提交##################### 

> # 目录下创建文件 file01.txt 

> 略

> # 将修改加入暂存区 

> git add . 

> # 将修改提交到本地仓库，提交记录内容为：commit 001 

> git commit -m 'commit 001' 

> # 查看日志 

> git log 

> ####################修改文件并提交###################### 

> # 修改file01的内容为：count=1 

> 略

> # 将修改加入暂存区 

> git add . 

> # # 将修改提交到本地仓库，提交记录内容为：update file01 

> git commit --m 'update file01' 

> # 查看日志 

> git log 

> # 以精简的方式显示提交记录 

> git-log 

> ####################将最后一次修改还原################## 

> # 查看提交记录 

> git-log 

> # 找到倒数第2次提交的commitID 

> 略

> # 版本回退 

> git reset commitID --hard

