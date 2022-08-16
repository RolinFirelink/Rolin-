本章节我们来学习如何用IDEA来配置Git，这就是重量级内容了，也是我们学习Git中最为重要的一部分

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

然后我们在码云中创建对应的仓库，接着复制其ssh，以后我们会用得上。

最后我们点击上面的VCS，直接选择Create Git Repository，选择初始化我们的git仓库，这里我们的git仓库就选择是我们的项目的原地址就行了。然后我们的项目就成为了一个本地仓库了，同时再我们的右上角会显示对应的提交与否的选项

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE23e13789fe6a950fb5a65697179d99a2.png)

这里的勾选即是我们的在git命名行中的提交功能，选中之后其会提示我们那些文件是想要提交的，我们一般来说直接勾选全部就可以了，因为我们事先做好过忽略文件，此时选择提交全部其会自动帮我们忽略掉那些我们本就想忽略掉而不去提交的文件。

同时我们在上方的窗口选项中会发现有对应的git选项卡，我们点击之后可以使用其中提供给我们的各种功能，如果我们想要将我们的本地仓库推送到我们的远程仓库的话，我们就要使用Git选项卡里的push操作，如果其中出现Empty（即远程仓库显示Empty，且无法操作）问题，我们只要到我们对应的项目的目录中，打开Git Bash命令行，然后输入git commit -m 'xxx'然后运行之后就能解决这个问题了。之后我们如法炮制，就会看到我们的远程仓库等待指定

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc19ae8d289e05e250fe5bb60a7bcff51.png)

我们点击Define remote，其会自动给我们提供一个origin的名字，我们在下面的url中黏贴我们的之前复制的ssh，然后点击下面的Push，此时我们就可以正确推送到我们的远程仓库中了，中间还会有不少的提示，我们全部都选是就完了。

- idea克隆_更新_解决冲突

然后我们来学习idea的克隆，克隆首先要进入我们的码云对应的仓库，然后复制克隆下面的ssh链接，然后我们点击idea上面的git，接着选择clone...选项，然后我们复制ssh链接到url上然后选择clone就完了，目录可以自己选，接着我们开启一个新的窗口。然后我们接着要解决更新和冲突的问题，我们两个窗口对一个远端仓库进行更新，自然也会发生冲突问题

我们要做的事情是对同一个文件的代码上添加对应的代码，然后两个窗口上我们就进行提交和推送，然后第二次推送的时候idea就会提示我们这里出现了冲突问题并弹出一个窗口，一般来说我们可以直接将问题在这个窗口中解决，但是我们作为初学者，可以先不使用这种方式，我们先将窗口关闭掉，然后进入我们的文件中，我们会看到冲突的文件都报红了，我们进入对应的报红文件，可以看到里面发生冲突的内容也被添加了对应的分支提示，我们将对应的分支提示删除掉，只留下我们想要保留的代码，然后右键选中我们的报红的文件，然后选择git-add，这就相当于是将未跟踪的文件加入到待提交中去，接着我们再执行对应的commit and push就可以正确执行更新了

而另一个用户想要获得更新的话，只需要选择Git框中的第一个↙的箭头就可以完成更新了。最后这里再提一嘴，我们如果想要查看我们的Git提交的历史记录，只需要点击下面的Git框，然后选择log就完了

- 分支操作

接着我们要学习分支操作，我们只记住一个就可以了，就是我们进入git选项卡里的log，查看其中的提交记录，然后我们可以在对应的提交记录上右键选择New Branch...，然后我们就可以创建对应的分支了，我们只记住这个就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfbde298e886c998a55ea739d7020f3ac.png)

如果我们想要合并的话，那就没法了，只能选择右下角的分支选项卡，然后选择对应的分支接着选择Merge into Current合并分支就完了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc6a0d6a597e95f555a95d60291fee31b.png)

- idea快速操作入口

我们接着来讲一些idea进行快速操作的入口选项，不过我们的全新版本的idea，可能图里的选项已经过时了就是，到时候对不上，不过没关系，我么也可以先看看，然后在对应的位置自己选就行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE471c8b9845000a07fb0fc76c491ef7b7.png)

更新的idea里上端窗口里没有VCS选项卡，但是有Git选项卡，我们可以在里面做对应的操作，下端也是，没有Version Control选项卡，但是有Git

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc354c42319e73e49023937c331887387.png)

第二个窗口是我们的另外的选项，简而言之就是替代我们第一张图的选项，我们可以从二张图中挑一个适合自己的，这里我们只做了解就可以了

- 场景分析

接着我们来做对应的使用Git的场景分析，直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdf257a3bfe53e5d9c8d3cc314c981685.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa7b5fdfaf339289658b2c054dcdbd5cb.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1b8800dfeffe8640ca7f82c18ddf2761.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE27d572d54fe7a642eeec9b81ad120408.png)

- 几条铁令

这个内容直接主要是为了防止我们新手搞这个把我们的代码搞丢了，所以我们要记住这几条铁令。第一条是切换分支钱要先提交本地的修改，这一点很重要，瞎几把切换分支会导致我们的代码disappear了，所以别乱切换分支，一定要注意先提交本地的修改，提交完了之后再切换。其他的铁令自己看吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE81a898a3344e074c52eed53e0e59b974.png)

最后是我们的Terminal选项卡，也就是idea下面的选项卡是我们的命令窗口，默认设置的cmd的命令窗口，其位置就在我们的当前的目录的位置，我们如果想要在此处使用Git Bash的命令窗口，只要进入我们的File中选择Setting，然后选择Terminal，在Shell path中将路径配置成我们的GitBash的路径就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9bda5fc560ce9301c57dc08d570641d2.png)

- 指令速查

接着我们来看看在我们的工作流程上的常用的指令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE83868141760c93e0a38c3b92fb3d8c63.png)

最后的内容无非是一些练习的内容，以及讲解使用Git的好处，这里就不提了