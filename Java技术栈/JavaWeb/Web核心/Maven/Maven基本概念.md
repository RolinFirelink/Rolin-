本章节我们来学习Maven，Maven分为基础和高级两节，我们这次先学习基础，之后我们再来学习高级内容

- Maven的概念与作用

首先我们来介绍下Maven的概念和它的作用，再正式介绍之前，我们先来约定下我们课件的资料的格式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE20197680547b051f23bb741014650fc4.png)

我们先来看看我们传统项目的管理存在什么问题，首先，我们存在的第一个问题就是jar包不统一或者不兼容的问题，因为我一个工程可能用了很多个jar包，而我们使用的jar包是可能会升级的，然后如果这些jar包有联系的话，那么可能我们的整个工程的底部就要改来改去，那就突出一个麻烦。而且还有的问题类似于是工程升级维护过程太繁琐等等等，可以看出传统的项目管理存在各种各样的问题

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE193219a83b827276d2c1952095a8cf5e.png)

这时，我们的Maven就派上用场了，首先我们的Maven本质是一个项目管理工具，其会将项目开发和管理过程抽象成一个项目对象模型（POM）。首先其将我们的项目抽象成模型依赖的是我们的文件pom.xml，如果我们要做八个项目，那么我们就要有八个pom.xml文件。然后我们的项目对象模型的管理则依赖其另一个称之为是依赖管理，也就是Depen dency的模块，我们的依赖管理可以从项目对象模型里获取我们需要的信息，项目对象模型里需要什么可以由依赖管理来帮忙做，同时项目对象模型也能从依赖管理中去获取想要的东西，也就是说项目对象模型本身做完之后可以成为一种能被依赖管理所使用的资源，因此其箭头是双向的。而依赖管理用到的资源是在我们的本地仓库，而一个公司往往会有很多人，不可能全部资源都放在我的本地仓库吧，因此其还需要一个地方来保存公共的信息，也就是私服，也可以理解为私服仓库，有了这个之后那么大家的东西就都能共享了。而私服中的内容又是从中央中获取到的，所以本质上我们的依赖管理所需要的东西是从中央仓库中获取到的，这点我们先记住就行了。

Maven除了可以项目管理之外还可以做项目构建，而项目构建依赖于构建生命周期，这个模块运行时自己是没有功能的，其依赖于插件，利用插件可以完成我们这些功能要执行的操作。其与插件之间是相互对应的，一个构建过程可能包含若干个插件，一个插件也可以应用到若干个构建过程中。利用插件我们可以生成jar包、源代码、帮助文档等等等等

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa8ea56bc6bdad0e5839cf5cdc3fbe22e.png)

 在上图中，蓝框所框柱的部分就是Maven，其他的不是啊，可以被替换的。中间的部分我们再用蓝色虚线再进行划分，上面是构建生命周期和管理的模块，下面则是Maven帮我们做好的无数个好用的插件

最后我们来看看Maven的作用，其核心作用就在于项目构建和依赖管理，前者提供了标准的、跨平台的自动化项目构建方式而后者则可以让程序员方便快捷的管理项目中依赖的资源，避免资源间的版本冲突问题

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1f479256618ada72cc80807c984f1260.png)

- 下载安装与环境变量配置

了解了Maven的作用之后，我就来正式安装和使用Maven，我们解压对应的安装包，可以看到以下目录，我们简单来介绍下

首先bin目录里面存放了Maven的所有的可运行指令，然后是boot目录，里面存放了一个jar包和一个文件，那么jar包就是maven的类加载器，maven本身也是java程序做的，所以他自己也有jar包和java程序。然后是conf目录，该目录主要用于配置管理，在该目录下有maven的配置文件，而lib则有maven运行所依赖的所有jar包

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa910fee481506cff6b92027b1e4f20fd.png)

然后我们还需要配置maven自身的运行环境，需要配置JAVA_HOME和MAVEN_HOME，由于前者我们已经配置过了，因此我们这里只需要配置后者就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe401890cd8064cdffdc998f82e46df9c.png)

搞定了这些之后，我们的Maven就算是安装完成了

- 仓库

接着我们先来介绍maven的第一个概念，也就是仓库，仓库用于存储资源和各种jar包，我们的本地计算机上有本地仓库，如果我们想要使用jar包，就会先从本地仓库中获取，本地没有就从私服中去获取，私服没有就从中央拿，我们私服的仓库是由企业维护的，而中央仓库则是由maven管理团队维护的。我们之所以需要私服，是为了提高我们获取jar包的效率，如果没有私服，所有人都直接从中央拿，那么我们的效率就会变得非常慢

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2918bc79c67830752102a26fa3e23488.png)

最后我们来看看仓库的文字说明

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE470c779bf78ac6141954921b83634efc.png)

- 坐标

首先我们先介绍下什么是坐标，坐标其实就是maven中用于描述仓库中资源的位置

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE740eb0f4ad5c286ef0e085b006656ead.png)

坐标的主要组成部分有以下四个，分别额是groupId、artifactId、version以及packaging

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE32d11cc0469478263df55f5d557d3de6.png)

上面的内容稍微了解下就行了，下面的内容才是重量级的内容。首先我们要记住一个网址，就是https://mvnrepository.com/，这个网址，我们一定要记住，只要我们从事java开发，我们就必然无法离开这个网站。这里我们要提一下的是，以后我们的maven都用mvn简称来代替，然后repository其意为仓库

如果我们想要查找某一个jar包，我们就直接在上面的搜索栏上输入对应的名字，然后其会在下面显示对应的结果，我们找到我们所需要的，就可以看到其具体信息，包括其不同的版本以及各种版本的热门程度，一般来说，热门程度越高的就越好。然后找到我们需要的版本再次点击，就可以在下面找到这个结果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE77bc136c6db2ab1d609df5be2d6bc3f8.png)

将这个结果复制，然后就可以在仓库中获得我们所需要的jar包了，上面的内容其实就是我们的坐标的书写方式，中间有个scope，这个我们以后再说，不过我们可以先知道其代表的意义是范围

最后我们来看看maven中坐标的作用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd53a7c4e4487b8e7e279b4c8339af947.png)

- 仓库配置

接着我们进入本章的最后一节，仓库配置，本节我们要完成的内容是对我们的仓库进行一定的配置。首先我们的默认仓库是放到了我们的C盘中的，而这显然不行，因为C盘里放资源是很可怕的事情，满了到时候就寄了，所以我们要更改我们的仓库位置

如果我们希望其将所有文件放置于另外一个仓库，当然首先第一步我们要创建出这个仓库，假设我们的创建的仓库位置为E:\maven\repository，此时其还只是一个普通的文件夹，要令其变成仓库，我们需要将maven的目录下的conf目录下的settings文件复制到我们仓库的同层位置中，这样那个文件夹就成为一个仓库了。但是还有一个问题，那就是我们怎么样才能让我们的maven知道我们的东西是要放到这个仓库中呢？此时我们就需要对settings这个配置文件进行设定，我们进入到给配置文件中，可以看到如下内容

```
<!-- localRepository
 | The path to the local repository maven will use to store artifacts.
 |
 | Default: ${user.home}/.m2/repository
<localRepository>/path/to/local/repo</localRepository>
-->
```

这里面的内容就定义了仓库的位置，我们可以看到里面的注释写着仓库的默认位置就是在我们的user.home目录下的，如果我们要设定我们的仓库位置，我们可以将<localRepository>/path/to/local/repo</localRepository>这一句单独拿出来，并在中间加入我们的仓库位置，修改完毕之后不要忘记我们自定义的仓库里的配置文件也要进行对应的修改，我们无论做什么修改，我们都尽量保持其子配置和主配置相同

这个配置完之后，我们maven的自定义仓库就做好了，之后maven下载jar包等资源就会放在我们自定义的仓库中了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8fcf80e39270538d655295afb84f4ec0.png)

我们更改了我们的仓库，那我们的中央仓库需要进行对应更改吗？这个其实不用我们担心，因为maven已经帮我们做好了，具体我们直接来看其配置的代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7c47f92775a1d695d8f32134675f7f59.png)

repositories和repository指代仓库，id标签里填写我们的仓库的唯一标识，这里一定要写central，否则连接的就不是中央仓库，下面的名字随便写，写yongshiwuai,jumupobai都可以，最后的url就是我们中央仓库的地址了

我们要解决的问题还有一个，就是中央仓库并不在中国境内，下载很慢，我们要怎么办才能让他下载变快呢？一个简单的想法是使用镜像来下载，这个镜像就是中国境内的镜像远程仓库，即是阿里云的仓库，阿里帮我们搞好了这事了，所以我们直接用就完了，而让我连接到中央仓库同样需要修改我们的配置文件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaefb172ccb2c4e7b937e9dac4dd9d0f4.png)

我们同样进入配置文件，可以找到以下内容

```
<mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
</mirrors>
```

我们可以看到其就是设置镜像的位置，具体的设置格式甚至都有在注释里给出，我们依葫芦画瓢就行了，在mirrors标签中写入如下内容

```
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>central</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

这里我们的id写入的是镜像的唯一标识，而mirrorOf则是写入要镜像的仓库，name随便写，url写我们的镜像的地址。

那么到此为止，我们的镜像仓库也配置好了