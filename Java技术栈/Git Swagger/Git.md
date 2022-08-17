# Git

本章节我们来学习Git，因为Git是程序员的必备管理工具，因此学习SpringBoot之前，我们先来学习他

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe7e232671f6fe8a671a9234feeaaf2cb.png)

这里我们学习最重要的就是最后一点，就是使用idea来操作git

## Git作用

我们Git的作用有以下四点

1. 备份

1. 代码还原

1. 协同开发

1. 追溯问题代码的编写人和编写时间

其之所以能够实现这些，是通过版本控制达到的，简单来说就是每个人写一份都添加一个版本号。

我们有两种版本控制器的方式，一种是集中式版本控制工具，比如说SVN和CVS，其原理是将代码都放到一个中心服务器中，然后程序员需要就往中心服务器里拿，但是这样的话，如果哪一天我们的中心服务器突然寄了就全完了，断网了也是全完了，不方便。

而我们的Git则是分布式版本控制工具，其没有什么中央服务器，每个人的电脑上都是完整的版本库，无需联网，多人协作时只要将各自的修改推送给对方就完了，任何两个人之间都可以互相推送自己的代码版本，不过虽然可以这样做，不过一般我们都还是有一个共享版本库来控制的。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7f5464f8056d33d80f9f6b6b35615dfe.png)

最后我们来看看Git的特点

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb4387a14dfc2d5982553817dba98770b.png)

## git工作流程简述

简单来说，git有远程仓库和本地仓库的概念，其他的我们先了解下就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE01e1cbcb597c39a491ea3f3ccdce9a73.png)

## git环境的配置与安装

接着我们来学习git环境的配置和安装，首先我们要提一嘴，我们安装过程中是会用到基本的Linux的命令的，因此我们提前将其列举了，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE28b5712f3a06a5090fce1c9ae8200c90.png)

首先是安装，安装其实就是傻瓜式安装，我们一直安装就完了，如果安装成功了我们右键点击桌面可以看到Git GUI和Git Bash，前者是Git提供的图形界面工具，而后者是命令行工具

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2d5d7b89d4024ecaa0285921dfb41e25.png)

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

## git常用命令

接着我们就来讲讲git的常用命令，在讲解之前，我们要先来学习Git的工作状态。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE984acad0f71f27fe20220a413aa51661.png)

首先我们要理解工作区的概念，除了我们的.git文件夹之外的文件都数去工作区。然后如果我们新创建一个文件，那么其是处于未跟踪状态的，我们可以用git add命令令其进入暂存区并处于已暂存状态，我们的暂存区是仓库之前的缓存区，我们可以再次调用git commit方法令其进入仓库中

而对于修改文件来说也是大同小异，如果我们修改文件，那么其首先处于工作区中且是未暂存状态，同样调用对应的方法能够令其进入暂存区并最终到达仓库中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEff0906450d81f6f6b979480fcaf36bf7.png)

## git案例实现

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4d208c2df64f6151c48803bd25533db9.png)

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

## git-log讲解

最后我们来单独讲解下git-log这个参数，其下有很多选项，其中--all是显示所有分支，--graph是将信息以图的形式来显示，更多的命令及其作用请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEccc3e0b658d18327ea4046fc357aeb6d.png)

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

## 版本回退

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa05bb127ab15eb21f353b1ee3538be34.png)

我们要使用版本回退就要使用git reset --hard commitID命令，其中commitID指的是我们的上传的内容或者是某一个操作的唯一标识，举个简单的例子，我们可以让我们的仓库的内容回到第一次添加的时候，那么我们的仓库就会回到那会，那时我们连修改都没有做。我们也可以再回退回来，只需要输入对应的修改之后的commitID就行了，这个东西我们可以通过之前我们的git log记录中查看到，但可能有一种情况，我们不但回退了，我们还将我们的屏幕给情况了，那我们此时应该怎么办才能将我们的仓库恢复到原来的样子呢？我们可以调用git reflog方法，调用该方法可以查看到我们的已经删除的提交记录，我们利用这个命令可以查看到我们回溯之前的状态的唯一标识，并利用这个唯一标识将我们的仓库恢复到那时的状态

## 添加文件到忽略列表

最后我们来说下我们如何添加文件到我们的忽略列表中，我们有这么一种需求，就是我们希望对大部分的文件进行同样的处理，但是我们同时又希望忽略掉一些文件，此时就需要使用到忽略列表

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbf6b076f360957c27f3f73e5cca63bb5.png)

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

# Git分支

接着我们来学习Git里面的一个重要内容，分支。首先我们来介绍下什么是分支，我们之前说过协同开发，但是难道我们的协同开发到底是怎么实现的呢？其实我们一般是不动我们的主线开发，而是让不同的人开发不同的分支，然后最后确定功能没有问题了，我们再来将分支上的代码合并到我们的主线上。

所以分支简单来说就是你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。接着我们来讲讲我们的关于分支的常用方法

首先我们先来进行一个分支的页面的科普，我们输入git-log，可以看到如下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa6c6d345c1dc1fd6f986912c7c970d4c.png)

我们这里可以在最后我们的括号内看到master，这个master就代表的是当前分支，如果我们进入了其他分支，那么该名字也会正确转换为其他分支的名字，一般来说，我们的master是作为主支来使用的。其次我们可以在下面看到HEAD和是一个箭头，其代表的就是指向当前的使用的支点，目前我们使用的支点是master，因此其指向master

## git分支中的常用方法

接着我们来正式讲分支中的常用方法，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE570c927372f56174ecaea7a6f8d38c22.png)

其中我们最重要的要学习的命令就是创建分支和合并分支，这里我们值得一提的是，如果我在主支上创建了一个文件，又在分支上创建了一个文件，那么这两个分支上在仓库上是否显示与否取决于我们是否切换到了对应的分支，否则其就不显示（但主支上原来就存在的文件会在分支上同样创建一次然后能在分支中查看到同名的文件，但是两者的修改却是不一样的）。

如果我们都希望其能在一个分支上进行显示的话，就要进行分支的合并，一般我们都是将我们的其他分支加到我们的master分支上，一般我们是切换到master分支中，然后写入git merge 分支名称，这样的命令，这样就可以将我们的其他分支合并到当前分支中了，我们调用这个命令其会进入到一个提示页面，这个页面类似于vim，我们先按下不表，直接按shift+冒号，然后输入wq就完了

那么合并之后我们查看其提交记录的显示样式如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6b1c9358102d790d9adac628be92b0db.png)

我们可以看到我们执行的合并操作，并且其下合并的分支就有dev01

## 解决冲突

接着我们来学习解决冲突，什么是冲突呢？简单来说，就是当两个分支上都对同一个文件的同一行进行了修改之后，又对其进行了合并，那git根本就不知道你到底要用哪个，此时就会出现冲突，git往往会将这个冲突交给程序员自己来解决，他自己只负责合并，我们具体来看看其解决冲突的过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6af2aae31aca137a09f509c0c54e92a1.png)

当我们出现冲突后，其会提示合并失败，并提示出现冲突的位置，然后我们进入到冲突位置的文件中查看，可以看到其中已经有了git编辑器给我们写上的冲突位置的标记，HEAD到======代表的是我们当前的分支的代码，而======到后面的分支代表是我们要合并的分支上的代码，两者的差异就代表其冲突的位置，我们修改的方式也很简答，直接将对应的提示代码全删掉，然后将该行改为我们所需要的具体内容就可以了。

之后我们通过同样的添加和提交命令，其会自动识别出我们正在进行合并的动作，在提交完成后会将我们的文件的分支进行一个成功合并。同时如果我们的合并提交时没有使用-m命令，其会提示一个窗口，也类似于文本编辑器，我们直接wq就完了

注意我们这里的冲突一般都是同一行的冲突，如果不是同一行的那他就自动进行一个合并了

## 分支使用流程

接着我们来学习我们的分支开发的规范，实际上我们开发肯定不可能跟我们这里一样瞎几把生成分支是吧。我们先来讲一种最普遍的分支的使用情况，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeaabbe6085d553af31b2c8bb703f9657.png)

首先我们的主支上有一个develop分支，develop分支下有许多的feature分支，develop分支主要用于进行阶段开发功能，开发完成之后我们会将这些功能合并到我们的主支上发布，开发功能当然不是在develop自身上开发的，而是在其下的feature分支上开发的，在该分支上不但开发功能，而且会经过测试，测试通过后就合成到develop分支上，然后为了节省空间，我们的feature分支就可以删除了。

在我们的主支上还有release分支，其不但可以用来标记每次上先的时间点，还可以从中生成新的子支用于bug修复，修复完的版本进行了开发之后要经过测试，测试完成之后就和主支合并，该动作意为bug修复，然后再将其合并到develop中，代表我们的新功能也在我们的develop中了，另外我们一般是不做主支里往分支里合并这种操作的，所以我们总是要从其他的用于bug修复的子支合并到develop上

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3cc51454ddc4acccb6543676cfb9c5bf.png)

当然实际的开发中还有一些其他的分支，比如用于测试的分支，或者是pre预上线分支等，不过这些分支是根据我们不同公司的情况自己设置的，我们这里就不赘述了

最后我们来做一些对应的练习

> ###########################创建并切换到dev01分支，在dev01分支提交 

> # [master]创建分支dev01 

> git branch dev01 

> # [master]切换到dev01 

> git checkout dev01 

> # [dev01]创建文件file02.txt 

> 略

> # [dev01]将修改加入暂存区并提交到仓库,提交记录内容为：add file02 on dev 

> git add . 

> git commit -m 'add file02 on dev' 

> # [dev01]以精简的方式显示提交记录 

> git-log 

> ###########################切换到master分支，将dev01合并到master分支 

> # [dev01]切换到master分支 

> git checkout master 

> # [master]合并dev01到master分支 

> git merge dev01 

> # [master]以精简的方式显示提交记录 

> git-log 

> # [master]查看文件变化(目录下也出现了file02.txt) 

> 略

> ##########################删除dev01分支 

> # [master]删除dev01分支 

> git branch -d dev01 

> # [master]以精简的方式显示提交记录 

> git-log

## 强制删除分支

接着我们来讲下我们需要强制删除分支的场景，这个使用场景我们了解下就好了，并不做强制要求。最经典的应用场景就是我们新创建了一个分支并写入了信息，然后提交，接着我们回到主支上删除这个分支（注意分支不能自己删除自己，因此要删除分支必须到其他分支中），此时普通删除就会提示你还没有合并这个分支而无法删除，因为git认为你这个分支创建出来但是又什么都还没有干，所以你这个很可能是误操作，因此提供这个信息，相当于是要你确定删除这个分支，此时如果我们一定要删除这个分支，我们就直接用-D的选项调用命令来删除这个分支就可以了。其命令是git branch -D 分支名

最后我们可以来做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0ef658dc2a6aa0cd021e62366d44d04d.png)

## 合并的快进模式

最后我们再补充一个内容，我们先来说这么一种场景，假如我们我们在主干上创建了一个分支，然后切换到分支上创建了一个文件，此时我们将分支提交，我们此时查看我们的提交记录会发现，从上往下，第一个提交记录是我们的分支的提交记录，第二个记录是我们的原来的主干的提交记录，此时我们的第一个提交和第二个提交只差了只是第一个提交的内容有变动而已，此时如果我们再执行合并操作的话，git会智能的识别到我们要做的事，此时就会启动合并的快进模式，直接将主干的位置移动到分支上，此时的结构就不会有原来的那种树型结构了，而是在主干上创建了一个分支的结构，虽然结构不一样，但是他们的作用是一样的

合并之后的结构图如下所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE235f675384f354e48d9421ac13c6833d.png)

当我们的一个分支做了修改另一个没有做且要对这两个分支进行合并时git会自动启动合并的快进模式，反过来，如果我们的两个分支都做了修改，此时要合并，那就只能使用普通方式来进行合并了

## 仓库托管

我们之前说过我们的git除了有本地仓库之外还有远程仓库，我们一般现在已经把本地仓库的内容给讲完了，接着我们来讲远程仓库的内容。

首先，我们的远程仓库有以下三种

> 前面我们已经知道了Git中存在两种类型的仓库，即本地仓库和远程仓库。那么我们如何搭建Git远程仓库 

> 呢？我们可以借助互联网上提供的一些代码托管服务来实现，其中比较常用的有GitHub、码云、GitLab等。 

> gitHub（ 

> 地址：https://github.com/ ）是一个面向开源及私有软件项目的托管平台，因为只支持 

> Git 作为唯一的版本库格式进行托管，故名gitHub 

> 码云（地址： https://gitee.com/ ）是国内的一个代码托管平台，由于服务器在国内，所以相比于 

> GitHub，码云速度会更快 

> GitLab （地址： https://about.gitlab.com/ ）是一个用于仓库管理系统的开源项目，使用Git作 

> 为代码管理工具，并在此基础上搭建起来的web服务,一般用于在企业、学校等内部网络搭建git私服。

我们这里使用码云来作为我们的远程仓库，因为码云是国内的一个平台，速度比较快。

首先我们进行一个码云的注册，注册过程就不谈了，注册完之后我们创建仓库

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE603e849edcd886676a1d5a949c0715c8.png)

创建完之后我们要将我们的仓库推到我们的git中，我们应该要怎么做呢？一种通用的方式就是使用SSH公钥来推送，首先我们按照下面的步骤在我们的git窗口中配置我们的公钥，接着获取公钥并复制

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEda1599e37ef1dd82260ef61d8e0cc830.png)

然后我们将ssh的公钥的内容全部赋值下来，然后我们进入码云的个人首页的设置页面中，选择SSH公钥，接着复制进去就完了，上面的名字随便写

然后我们在在git窗口中输入，ssh -T git@gitee.com，然后输入yes（第一次访问要输入yes），然后就能看到对应的欢迎文本了，此时就说明我们的ssh已经配置成功了，我们已经可以连接到码云中了

## 远程仓库添加

接着我们就来正式添加我们的远程仓库，要添加远程仓库，首先我们要进入我们码云中的设置，复制其中的SSH，然后我们调用命令 git remote add <远端名称> <仓库路径>

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEce6b24f2f94474399961c955b2d76ab1.png)

然后我可以通过git remote命令来查看我们当前所连接的远程仓库

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0ba04873064919c567e2481bc482f184.png)

这里值得一提的是，我们的一个本地仓库是可以连接多个远程仓库的，不过一般我们都连接一个，不贪多。

然后我们来细说一下我们的推送本地仓库到远程仓库的方法，其完整方法是git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名] ]

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1a66875784e5b6c1968289626a7f560d.png)

但如果远程分支名和本地分支名相同，我们可以只写本地分支。这里我们就要搞清楚，本地仓库有多个分支，远程仓库也是能有多个分支的，一般来说我们第一个指定的名称是远程仓库的名称，然后我们要指定我们这里要推送的本地仓库的分支，接着是要更新的远程仓库的分支

而-f表示的是强制覆盖，不过这个用不太上，因为一般情况下我们都是只允许添加代码而不允许覆盖代码，要不然公司来个小白把代码覆盖了那就全完了

然后是我们的--set-upstream选项，其代表的是推送到远端的同时建立起推送的本地仓库的分支和远端仓库的关联关系，使用了这个代码，我们就可以在推送本地仓库的同时建立起相关的联系，这样我们进入到对应的分支，如果该分支已经建立了联系，那我们直接输入git push就可以直接将该分支推送到我们的远程仓库中去了

另外使用git branch -vv命令可以查看到本地仓库和远程仓库的联系

## 从远程仓库克隆

实际开发的时候，往往是多个人的本地仓库要连接到一个远程仓库的，而且其要进行开发，其就需要对远程仓库做一个内容上的复制，将远程仓库的东西下载到自己的仓库上来，此时就需要用到仓库克隆的指令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfe01d922b24712275df1da80e2271a35.png)

其命令是git clone <仓库路径> [本地目录]，仓库路径就是在码云中仓库旁边点击克隆后看到的ssh，里面的内容就是我们的远程仓库的路径，本地目录如果不指定其会在当前目录下自动生成，且名字与远程仓库的仓库名保持一致

## 抓取和拉取

在我们实际的协同开发里，是有多个人操纵一个远程仓库的，那不可能远程仓库更新一次我们本地仓库就克隆一次吧？这不折磨人么，所以一般来说，我们都是使用一次克隆，后续我们的仓库更新了，我们就在本地仓库中做对应的更新就可以了。

接着我们就来讲更新本地仓库的两种方法，抓取和拉取。前者只是将更新的内容下载到本地，而没有对本地的对应内容进行一个覆盖更新，如果我们要进行覆盖更新的话，还需要自己手动进行合并操作，后者则是将下载和更新操作合二为一

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfcdb76fec8c42204854be569814ff098.png)

介绍这两种方法之前我们先要认识到一件事情，那就是当我们的将本地的修改推送到我们的远程仓库中时，此时我们可以认为本地仓库和远程仓库为两个分支，此时我们就只对我们的本地仓库做了更新，远程仓库则没有。而我们所谓的推送其实就是合并，那么当我们将我们的本地仓库与远程仓库合并时，同样也会启用快进模式，反过来当我们将远程仓库和本地仓库合并时，也会开启快速合并（这个是我边看边猜测的，不保证对）

我们这里抓取的命令是将仓库中的更新都抓取到本地，不会进行合并，当然，我们这里需要指定我们的远端名称和分支名，用/隔开，如果不指定就抓取所有分支，不过一般来说我们抓取之后可能会需要合并，那我们还有手动进行一个merge命令，这样就太麻烦了，因此我们又拉取命令，这个命令就等同于我们的抓取+合并，同样的如果不指定远端名称和分支名则默认抓取所有并更新当前分支

## 远程冲突解决

有这么一种业务情况，A和B用户都修改了同一个文件的同一行代码，此时如果我们进行合并就会发生合并冲突。举个简单的例子，我修改了A文件中的代码，此时我要将其更新都云端，然后我发现云端仓库更新了，那么我们就应该先更新云端仓库，后提交，但是我们执行对应的pull指定的时候，其报了合并异常错误，那么此时我们就还是跟之前学习的一样，进入对应的发生冲突的文件，然后将该文件的发生冲突的内容修改为我们最后要保留的内容，保存之后进行添加和提交我们就合并成功了，之后我们再往云端中推入我们的本地仓库就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE23e13789fe6a950fb5a65697179d99a2.png)

最后我们提一嘴，我们即使执行了fetch操作，也可以执行pull操作，这个不影响，无非是其发现远程仓库里没什么需要更新的，会直接执行合并操作就完了

最后我们来做一下对应的练习

> ##########################1-将本地仓库推送到远程仓库 

> # 完成4.1、4.2、4.3、4.4的操作 

> 略

> # [git_test01]添加远程仓库 

> git remote add origin git@gitee.com/**/**.git 

> # [git_test01]将master分支推送到远程仓库,并与远程仓库的master分支绑定关联关系 

> git push --set-upstream origin master 

> ###########################2-将远程仓库克隆到本地 

> # 将远程仓库克隆到本地git_test02目录下 

> git clone git@gitee.com/**/**.git git_test02 

> # [git_test02]以精简的方式显示提交记录 

> git-log 

> ###########################3-将本地修改推送到远程仓库 

> # [git_test01]创建文件file03.txt 

> 略

> # [git_test01]将修改加入暂存区并提交到仓库,提交记录内容为：add file03 

> git add . 

> git commit -m 'add file03' 

> # [git_test01]将master分支的修改推送到远程仓库 

> git push origin master 

> ###########################4-将远程仓库的修改更新到本地 

> # [git_test02]将远程仓库修改再拉取到本地 

> git pull 

> # 以精简的方式显示提交记录 

> git-log 

> # 查看文件变化(目录下也出现了file03.txt) 

> 略 

# IDEA配置Git

本章节我们来学习如何用IDEA来配置Git，这就是重量级内容了，也是我们学习Git中最为重要的一部分

## 添加忽略

我们要导入我们的项目，我们这里导入的项目是我们的SpringMVC的利用注解进行整合的Maven项目，这里要注意的是，对于Maven项目而言，在idea中不可使用导入，否则容易出问题，我们要直接用Open，打开对应的文件夹就可以了，我们选择用This Windows，因为我们原来的窗口已经没有作用了。接着我们选择file选项里的settings，搜索git，可以看到里面的第一行会自动寻找我们的git的目录并自动指定，如果没有自定指定的话我们就需要手动指定了。然后我们创建对应的.gitignore文件到项目中，在其中添加如下代码

```
*.class

.mtj.tmp/

*.jar
*.war
*.ear
*.zip

has_err_pid

.idea

*.iml

*.bak
*.class
*.rar
*.log
.project
.settings
.classpath
target
lib
*.DS_Store
.gradle
build
out
log
```

这些代码都是我们上传时要忽略掉的代码，以后我们要做项目的话也可以直接将这些代码构建一个忽略项目的文件，然后直接拿来用就行了。

## 具体配置过程

然后我们在码云中创建对应的仓库，接着复制其ssh，以后我们会用得上。

最后我们点击上面的VCS，直接选择Create Git Repository，选择初始化我们的git仓库，这里我们的git仓库就选择是我们的项目的原地址就行了。然后我们的项目就成为了一个本地仓库了，同时再我们的右上角会显示对应的提交与否的选项

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfbde298e886c998a55ea739d7020f3ac.png)

这里的勾选即是我们的在git命名行中的提交功能，选中之后其会提示我们那些文件是想要提交的，我们一般来说直接勾选全部就可以了，因为我们事先做好过忽略文件，此时选择提交全部其会自动帮我们忽略掉那些我们本就想忽略掉而不去提交的文件。

同时我们在上方的窗口选项中会发现有对应的git选项卡，我们点击之后可以使用其中提供给我们的各种功能，如果我们想要将我们的本地仓库推送到我们的远程仓库的话，我们就要使用Git选项卡里的push操作，如果其中出现Empty（即远程仓库显示Empty，且无法操作）问题，我们只要到我们对应的项目的目录中，打开Git Bash命令行，然后输入git commit -m 'xxx'然后运行之后就能解决这个问题了。之后我们如法炮制，就会看到我们的远程仓库等待指定

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc19ae8d289e05e250fe5bb60a7bcff51.png)

我们点击Define remote，其会自动给我们提供一个origin的名字，我们在下面的url中黏贴我们的之前复制的ssh，然后点击下面的Push，此时我们就可以正确推送到我们的远程仓库中了，中间还会有不少的提示，我们全部都选是就完了。

## idea克隆更新解决冲突

然后我们来学习idea的克隆，克隆首先要进入我们的码云对应的仓库，然后复制克隆下面的ssh链接，然后我们点击idea上面的git，接着选择clone...选项，然后我们复制ssh链接到url上然后选择clone就完了，目录可以自己选，接着我们开启一个新的窗口。然后我们接着要解决更新和冲突的问题，我们两个窗口对一个远端仓库进行更新，自然也会发生冲突问题

我们要做的事情是对同一个文件的代码上添加对应的代码，然后两个窗口上我们就进行提交和推送，然后第二次推送的时候idea就会提示我们这里出现了冲突问题并弹出一个窗口，一般来说我们可以直接将问题在这个窗口中解决，但是我们作为初学者，可以先不使用这种方式，我们先将窗口关闭掉，然后进入我们的文件中，我们会看到冲突的文件都报红了，我们进入对应的报红文件，可以看到里面发生冲突的内容也被添加了对应的分支提示，我们将对应的分支提示删除掉，只留下我们想要保留的代码，然后右键选中我们的报红的文件，然后选择git-add，这就相当于是将未跟踪的文件加入到待提交中去，接着我们再执行对应的commit and push就可以正确执行更新了

而另一个用户想要获得更新的话，只需要选择Git框中的第一个↙的箭头就可以完成更新了。最后这里再提一嘴，我们如果想要查看我们的Git提交的历史记录，只需要点击下面的Git框，然后选择log就完了

## 分支操作

接着我们要学习分支操作，我们只记住一个就可以了，就是我们进入git选项卡里的log，查看其中的提交记录，然后我们可以在对应的提交记录上右键选择New Branch...，然后我们就可以创建对应的分支了，我们只记住这个就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE01a7949a956f63a2257efeac5f9e65bb.png)

如果我们想要合并的话，那就没法了，只能选择右下角的分支选项卡，然后选择对应的分支接着选择Merge into Current合并分支就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc6a0d6a597e95f555a95d60291fee31b.png)

## idea快速操作入口

我们接着来讲一些idea进行快速操作的入口选项，不过我们的全新版本的idea，可能图里的选项已经过时了就是，到时候对不上，不过没关系，我么也可以先看看，然后在对应的位置自己选就行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE471c8b9845000a07fb0fc76c491ef7b7.png)

更新的idea里上端窗口里没有VCS选项卡，但是有Git选项卡，我们可以在里面做对应的操作，下端也是，没有Version Control选项卡，但是有Git

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc354c42319e73e49023937c331887387.png)

第二个窗口是我们的另外的选项，简而言之就是替代我们第一张图的选项，我们可以从二张图中挑一个适合自己的，这里我们只做了解就可以了

## 场景分析

接着我们来做对应的使用Git的场景分析，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdf257a3bfe53e5d9c8d3cc314c981685.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1b8800dfeffe8640ca7f82c18ddf2761.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa7b5fdfaf339289658b2c054dcdbd5cb.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE81a898a3344e074c52eed53e0e59b974.png)

## 几条铁令

这个内容直接主要是为了防止我们新手搞这个把我们的代码搞丢了，所以我们要记住这几条铁令。第一条是切换分支钱要先提交本地的修改，这一点很重要，瞎几把切换分支会导致我们的代码disappear了，所以别乱切换分支，一定要注意先提交本地的修改，提交完了之后再切换。其他的铁令自己看吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE27d572d54fe7a642eeec9b81ad120408.png)

最后是我们的Terminal选项卡，也就是idea下面的选项卡是我们的命令窗口，默认设置的cmd的命令窗口，其位置就在我们的当前的目录的位置，我们如果想要在此处使用Git Bash的命令窗口，只要进入我们的File中选择Setting，然后选择Terminal，在Shell path中将路径配置成我们的GitBash的路径就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9bda5fc560ce9301c57dc08d570641d2.png)

## 指令速查

接着我们来看看在我们的工作流程上的常用的指令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE83868141760c93e0a38c3b92fb3d8c63.png)

最后的内容无非是一些练习的内容，以及讲解使用Git的好处，这里就不提了

# Git问题总结

想要执行命令时出现提示

$ git remote

fatal: detected dubious ownership in repository at 'D:/Rolin的学习笔记'

To add an exception for this directory, call:

git config --global --add safe.directory 'D:/Rolin的学习笔记'

Set the environment variable GIT_TEST_DEBUG_UNSAFE_DIRECTORIES=true and run

again for more information.

简单来说就是提示我们这个仓库是不安全的仓库，解决方法： 参考这篇文章 https://blog.csdn.net/yxzone/article/details/125761932

