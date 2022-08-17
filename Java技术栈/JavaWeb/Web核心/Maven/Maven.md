本章节我们来学习Maven，Maven分为基础和高级两节，我们这次先学习基础，之后我们再来学习高级内容

# Maven的概念与作用

首先我们来介绍下Maven的概念和它的作用，再正式介绍之前，我们先来约定下我们课件的资料的格式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133606.png)

我们先来看看我们传统项目的管理存在什么问题，首先，我们存在的第一个问题就是jar包不统一或者不兼容的问题，因为我一个工程可能用了很多个jar包，而我们使用的jar包是可能会升级的，然后如果这些jar包有联系的话，那么可能我们的整个工程的底部就要改来改去，那就突出一个麻烦。而且还有的问题类似于是工程升级维护过程太繁琐等等等，可以看出传统的项目管理存在各种各样的问题

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133607.png)

这时，我们的Maven就派上用场了，首先我们的Maven本质是一个项目管理工具，其会将项目开发和管理过程抽象成一个项目对象模型（POM）。首先其将我们的项目抽象成模型依赖的是我们的文件pom.xml，如果我们要做八个项目，那么我们就要有八个pom.xml文件。然后我们的项目对象模型的管理则依赖其另一个称之为是依赖管理，也就是Depen dency的模块，我们的依赖管理可以从项目对象模型里获取我们需要的信息，项目对象模型里需要什么可以由依赖管理来帮忙做，同时项目对象模型也能从依赖管理中去获取想要的东西，也就是说项目对象模型本身做完之后可以成为一种能被依赖管理所使用的资源，因此其箭头是双向的。而依赖管理用到的资源是在我们的本地仓库，而一个公司往往会有很多人，不可能全部资源都放在我的本地仓库吧，因此其还需要一个地方来保存公共的信息，也就是私服，也可以理解为私服仓库，有了这个之后那么大家的东西就都能共享了。而私服中的内容又是从中央中获取到的，所以本质上我们的依赖管理所需要的东西是从中央仓库中获取到的，这点我们先记住就行了。

Maven除了可以项目管理之外还可以做项目构建，而项目构建依赖于构建生命周期，这个模块运行时自己是没有功能的，其依赖于插件，利用插件可以完成我们这些功能要执行的操作。其与插件之间是相互对应的，一个构建过程可能包含若干个插件，一个插件也可以应用到若干个构建过程中。利用插件我们可以生成jar包、源代码、帮助文档等等等等

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133608.png)

 在上图中，蓝框所框柱的部分就是Maven，其他的不是啊，可以被替换的。中间的部分我们再用蓝色虚线再进行划分，上面是构建生命周期和管理的模块，下面则是Maven帮我们做好的无数个好用的插件

最后我们来看看Maven的作用，其核心作用就在于项目构建和依赖管理，前者提供了标准的、跨平台的自动化项目构建方式而后者则可以让程序员方便快捷的管理项目中依赖的资源，避免资源间的版本冲突问题

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133609.png)

## 下载安装与环境变量配置

了解了Maven的作用之后，我就来正式安装和使用Maven，我们解压对应的安装包，可以看到以下目录，我们简单来介绍下

首先bin目录里面存放了Maven的所有的可运行指令，然后是boot目录，里面存放了一个jar包和一个文件，那么jar包就是maven的类加载器，maven本身也是java程序做的，所以他自己也有jar包和java程序。然后是conf目录，该目录主要用于配置管理，在该目录下有maven的配置文件，而lib则有maven运行所依赖的所有jar包

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133610.png)

然后我们还需要配置maven自身的运行环境，需要配置JAVA_HOME和MAVEN_HOME，由于前者我们已经配置过了，因此我们这里只需要配置后者就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133611.png)

搞定了这些之后，我们的Maven就算是安装完成了

## 仓库

接着我们先来介绍maven的第一个概念，也就是仓库，仓库用于存储资源和各种jar包，我们的本地计算机上有本地仓库，如果我们想要使用jar包，就会先从本地仓库中获取，本地没有就从私服中去获取，私服没有就从中央拿，我们私服的仓库是由企业维护的，而中央仓库则是由maven管理团队维护的。我们之所以需要私服，是为了提高我们获取jar包的效率，如果没有私服，所有人都直接从中央拿，那么我们的效率就会变得非常慢

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133612.png)

最后我们来看看仓库的文字说明

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133613.png)

## 坐标

首先我们先介绍下什么是坐标，坐标其实就是maven中用于描述仓库中资源的位置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133614.png)

坐标的主要组成部分有以下四个，分别额是groupId、artifactId、version以及packaging

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133615.png)

上面的内容稍微了解下就行了，下面的内容才是重量级的内容。首先我们要记住一个网址，就是https://mvnrepository.com/，这个网址，我们一定要记住，只要我们从事java开发，我们就必然无法离开这个网站。这里我们要提一下的是，以后我们的maven都用mvn简称来代替，然后repository其意为仓库

如果我们想要查找某一个jar包，我们就直接在上面的搜索栏上输入对应的名字，然后其会在下面显示对应的结果，我们找到我们所需要的，就可以看到其具体信息，包括其不同的版本以及各种版本的热门程度，一般来说，热门程度越高的就越好。然后找到我们需要的版本再次点击，就可以在下面找到这个结果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133616.png)

将这个结果复制，然后就可以在仓库中获得我们所需要的jar包了，上面的内容其实就是我们的坐标的书写方式，中间有个scope，这个我们以后再说，不过我们可以先知道其代表的意义是范围

最后我们来看看maven中坐标的作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133617.png)

## 仓库配置

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133618.png)

我们更改了我们的仓库，那我们的中央仓库需要进行对应更改吗？这个其实不用我们担心，因为maven已经帮我们做好了，具体我们直接来看其配置的代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133619.png)

repositories和repository指代仓库，id标签里填写我们的仓库的唯一标识，这里一定要写central，否则连接的就不是中央仓库，下面的名字随便写，写yongshiwuai,jumupobai都可以，最后的url就是我们中央仓库的地址了

我们要解决的问题还有一个，就是中央仓库并不在中国境内，下载很慢，我们要怎么办才能让他下载变快呢？一个简单的想法是使用镜像来下载，这个镜像就是中国境内的镜像远程仓库，即是阿里云的仓库，阿里帮我们搞好了这事了，所以我们直接用就完了，而让我连接到中央仓库同样需要修改我们的配置文件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133620.png)

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

# 第一个Maven程序

配置完了我们的maven之后，我们现在就来做我们的第一个Maven程序

## Maven项目结构

首先我们来看看maven的工程目录结构

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133621.png)

我们就手动创建对应的包就行了，这里就不演示了。我们直接说吧，首先创建的第一个project文件夹并不是我们的maven工程的结构，其下创建的java-project才是我们maven工程的结构，之后我们在其下创建对应的src文件夹src下要有两个文件夹，分别是main和test，其下都要有对应的java和resoureces文件夹，java文件夹里放java文件，resources放配置文件，其中main是我们编写我们的源代码本身的地方，而test则是我们用于测试的地方。这里有一个问题就是，为啥test还要有一个配置文件，答案是真的要，测试的时候有时候也要用到一些特别的配置文件。

然后是我们创建的文件是在java里的，但是我们要注意在里面创建文件时也要有对应的包的分类。接着我们在对应的包里创建的代码和测试类

最后，我们的创建的maven工程还需要一个最重要的东西，就是pom.xml文件，没有这玩意的话maven工程根本就不成立，因此我们这里要构建一个xml文件，直接自己写显然不显示，我直接拿别人的然后稍微修改，具体请看代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133622.png)

开头的部分其实就是我们的文件头了，这个不多说了。接着在project标签下有三个属性，分别是xmlns、xmlns：xsi、xsi：schemaLocation，这里面的内容我们先了解下就行了，暂时不要求掌握。modelVersion则是maven对象模型的版本号，这个版本和我们目前使用的3.5.3的版本其实没有关系，其指的是maven的pom对象的版本升级到了4.0.0。后面的groupId、artifactId以及version我们都填写我们自己的工程里的相应内容，而packaging则代表我们的工程最后要生成什么，我们这里填写jar最后就生成一个jar包，实际上还能填写很多。然后后面的东西就填写对应的要使用到的jar包的坐标和版本就行了，这些大概就是我们一个pom.xml文件的组成了。

## Maven项目构建

首先我们来看看Maven的构建命令，其均是使用mvn开头，很后面添加功能参数，其中compile代表编译、clean代表清理、test代表测试、package代表打包、install代表安装到本地仓库，下面我们具体来演示一下这些命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133623.png)

注意，我们这些命令都是要在我们的cmd窗口里调用的，首先我们要进入到我们的文件对应的位置，也就是在我们项目的首位置，其实就是project-java文件夹中，首先要编译我们的文件，写入mvn compile，第一次运行会比较慢，因为我们要下载我们编译所需要的jar包或者是插件，之后就会很快了，在cmd里会有对应的下载信息和执行动作的日志，我们可以去查看

编译完成之后会在我们的项目中生成一个target文件夹，文件夹内存放的就是我们编译完成之后的内容，如果我们不想要编译完成之后的东西，也可以输入mvn clean命令来进行清理

然后是mvn test命令，该命令就是用于做测试的，该命令执行之后，target下会多两个文件夹

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133624.png)

而且我们可以在cmd中看到如下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133625.png)

上面的内容我们可以看到先显示了结果hello maven，然后测试结果里，运行成功一个，失败0个，错误0个，跳过0个，总共用时0.067秒，下面的Results也是同样的内容，那为什么要显示两个同样的内容？这其实是因为我们的测试程序里只有一个方法导致的，实际运行中，上面的结果会一个个显示，而下面的Results则会显示总和的结果

我们的结果就放在surefire-reports文件夹中，里面有两个文件，一个是txt文件，那个文件其实就是我们cmd中的结果内容，而另外一个xml文件则有着所有的具体内容，包括JDK的版本，测试执行的环境等等等，最后一段则说明其测试了什么东西，名字是什么，花费了多久等等

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133626.png)

我们以后也可以将该文件分为两部分，一种是环境，上图中testcase上的都是环境，而其他内容都是测试结果，这就是target提供的测试报告

classes内存放的是源程序生成的字节码，test-classes则是存放测试程序生成的字节码

接着我们来看第四个mvn package，其运行完成后会在target文件夹中生成一个jar包，该jar包就是我们的项目打包之后的结果，注意，打包只打包源程序，而不会打包测试程序，这是当然的，测试程序你打包来干嘛是吧，我们注意看打包命令执行了什么动作，我们会发现其先执行编译，这是当然的，你都没编译我怎么打包？编译了之后再进行测试，然后再执行测试程序，一切无误之后再执行打包，这些动作都是为了确保我们的打包的内容是没有错误的，确实有用的

最后mvn install，这个命令的内容是会将我们打包的东西放到仓库里，我们会发现其放到了com包itheima包下，之所以这样是因为最开始我们的配置文件是这样填写我们的坐标的，所以会有这样的放置效果，如果我们填写的是其他的内容那么也会有其他的效果

## 插件创建Maven工程

上一节里我们手动创建并运行了我的maven工程，本节我们来利用插件来完成我们的需求，这一节主要偏理论，了解下就行了

先来看看我们利用插件创建工程的两种方式，一种是创建java工程，另一种是创建web工程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133627.png)

我们输入对应的命令其就会自动帮我们创建了，其帮我们的创建的java工程内容都是默认没有配置文件的，如果我们需要的话，那么我们就要自己创建对应的resources文件夹。然后是web工程，创建web工程我们能看到其配置文件里多了一句话，这句话的内容就是指定其打包生成的文件类型是war包的，生成的web工程没有测试文件夹，也没有存放java文件的文件夹，但是有resources文件夹，同时其下还有webapp文件夹，还贴心的帮我们生成了jsp文件，当然，我们用不用他生成的jsp文件是另一回事。

最后我们来看看普通创建的maven工程的目录和插件创建web工程的目录的对比图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133628.png)

我们其实主要要记住的就是webapp的文件夹就行了，我们以后也比较常创建web工程，要记住web是有这个文件夹的

## idea创建Maven工程

手动的方式创建Maven工程演示完毕之后，接下来我们就要用idea来创建Maven工程了。我们先创建一个空的项目，然后我们在模块上设置我们的Maven工程，当然别忘了我们首先应该要在设置里的project里将我们的JDK的版本设定为我们当前使用的JDK，反正我的是1.8。然后我们在settings中搜索maven，进入对应的maven设置，我们这里要选择其使用我们自己的maven，也就是3.6.1的版本的maven，然后勾选下面的重写xml，选择我们的maven的conf目录下的setting的xml文件，接着点击ok。然后我们就可以正式来创建我们的maven工程了，按照步骤创建好之后可以看到这样的画面

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133629.png)

我们这里将对应的java文件夹设置为蓝色的，其代表里面存放源码，resources代表其里面存放资源文件，而test里的java文件夹则是设置为绿色的，其代表里面存放测试的代码，其下的resources是我们自己创建的，其是用于存放测试的资源的文件夹。我们除了可以在这里进行一个文件夹属性的标记之外，我们还可以在程序外边右键对应的文件夹，选择Mark Directory as来对文件夹的属性进行标记

然后我们在左侧可以看到maven标签，点击该标签可以看到如下内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133630.png)

这里我们要提一点的是我们在pom.xml配置文件中配置的东西，在这里是会有对应显示的，如果没显示那就刷新看看，一般刷新之后就会显示了。Lifecycle里都是我们的一些命令，我们双击就可以执行了，执行之后其也会执行对应的功能，会产生相应的文件并在控制台上打印日志信息。

我们还有一种启动方式，那就是点击idea右上角的Add Configuration...，然后点击左侧的Maven，然后我们的name可以自己设定，Command line里写入我们要执行的命令，Working directory则写入我们要执行测试的java文件的地址

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133631.png)

这种方式颇有种脱裤子放屁的感觉，但是这也是有它的用武之地的，最经典的场景莫过于我们需要进行debug的时候。我们实际的测试程序也是做出来了的，但是有些搞不明白的问题就是了。

- Idea版使用模板（骨架）创建Maven工程（3.6.1版）

我们接下来来学习用模板来创建对应的Maven工程，其实这里也就是我们创建Maven模块的时候勾选Create from archetype然后选择对应的模块就行了，我们要创建java工程时要选择的模块是

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133632.png)

我们可以看到其创建的目录有main和test，但是他们其下都没有resources文件夹，而且还没有对应的标记，我们这里创建对应的文件夹另外加上标记就完了。然后我们创建web工程，要选择的模板的是

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133633.png)

其目录下虽然有resources文件夹，但是其下没有java文件夹和test测试文件夹，不过多了一个webapp文件夹，内含WEB-INF，该文件夹里面有对应的配置文件，但是很多我们目前都用不上，暂时不需要看。另外还需要什么文件夹我们就自己创建就完了

## tomcat插件安装与web工程启动

接着我们来讲最后一种运行方式，利用tomcat插件启动，首先我们要安装tomcat的插件，其安装的配置文件的内容如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133634.png)

我们首先要去之前的网址里查找tomcat maven，然后选择第一个，接着找到org.apache.tomcat.maven，点击之后我们选择第一项，然后选择2.1版本，复制其坐标然后放置于我们的配置文件中就行了，刷新之后我们可以在maven的右侧看到对应插件，然后我们双击插件就可以运行了，运行之后我们可以在控制台中得到我们所需要的网址，此时我们程序就成功运行了。最后我们来看看整个配置文件的内容和注释

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <!--指定pom的模型版本-->
  <modelVersion>4.0.0</modelVersion>
  <!--打包方式，web工程打包为war，java工程打包为jar-->
  <packaging>war</packaging>

  <!--组织id-->
  <groupId>com.itheima</groupId>
  <!--项目id-->
  <artifactId>web01</artifactId>
  <!--版本号:release,snapshot，前者表示正式版，后者表示开发版-->
  <version>1.0-SNAPSHOT</version>



  <!--构建-->
  <build>
    <!--设置插件-->
    <plugins>
      <!--具体的插件配置-->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>

</project>

```

具体的解释都写在上面了，这里就不再赘述了。不过我们的项目又出问题，还是一些整不明白的问题，我们这里就不管了，懒得

# 依赖管理与生命周期

做完了第一个Maven程序之后，本节我们来学习Maven的依赖管理和生命周期，这也是maven管理的最后一个内容，本来是打算一节讲完的，发现内容真挺多，所以就分三节了

## 依赖配置与依赖传递

首先我们对什么是依赖已经不陌生了，所谓依赖指的就是当前项目运行所需的jar，一个项目可以设置多个依赖，我们设置依赖的方式都是通过如下所示的设置配置文件的方式来进行设置的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133635.png)

设置一个依赖最起码需要写入依赖，也就是jar包的所属群组id（groupId），所属项目id（artifactId）以及版本号（version），依赖的大标签为dependencies，而其设置具体依赖的子标签为dependency

请看下图的依赖配置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133636.png)

可以看到我们在内部配置了两个依赖，一个是log4j，一个是junit，左侧也有对应的其所使用的到的jar包的资源展示，可以看到有三个项目，但是后面两个我们这里为了版面就不展示了，我们主要看第一个，我们可以看到其下有log4j和junit的依赖，而junit又有对一个org.hamcrest:hamcrest-core:1.3的依赖，简单来说就是我们的第一个项目用到了junit和log4j这两个jar包，而junit这个jar包又要用到另外一个jar包

如果说我们的第二个工程里要用到第三个工程的资源的话，那我们应该要怎么做呢？很简单，我们只要将第三个工程的坐标复制下来，然后用配置依赖的方式将其配置到第二个工程中就可以了，此时我们可以看到右侧的maven管理窗口里，也会发生相应的变化

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133637.png)

容易看到第二个项目里已经有了第三个工程的资源，点开其下也有log4j的资源文件可以使用，我们现在做的这个动作就是依赖传递，其有直接依赖和间接依赖，同时直接依赖和间接依赖也具有相对性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133638.png)





不过是灰色的，这是因为这个log4j没用到，我们实际用到的是自己的log4j的资源，这样就涉及到了资源使用的优先级的问题，当依赖关系出现冲突时，maven总是按照路径优先、声明优先以及特殊优先这三条法则来解决，路径优先指的是当依赖中出现相同的资源时，层级越低，优先级越高。而声明优先指的是当其在相同层级时，配置顺序靠前的覆盖靠后的。而特殊优先指的是当同级配置了相同资源的不同版本时，后配置的覆盖先配置的。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133639.png)

接着我们来介绍可选依赖，假定我们项目二里使用到了项目三，而项目三里使用到了junit，而如果我们希望达到一种项目二无法知道项目三使用了junit的效果，这时就需要用到可选依赖，可选依赖指的就是对外隐藏当前所依赖的资源，也即是令其变得不透明

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133640.png)

使用可选依赖的方式很简单，就是在对应的配置依赖中加入一个optional标签再加上true就可以了，最后我们来看看效果，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133641.png)

可以看到我们这里log4j和junit都配置了可选依赖，此时项目二中使用了项目三，但是项目二中却无法查看到项目三里所使用的东西。再提一嘴，可选依赖要在被使用的资源上去配置

但是此时我们能够看到，我们在项目三中使用了junit的依赖，而我们就能查看到junit使用了另外一个依赖，如果我们希望隐藏junit的依赖，那我们应该怎么办呢？首先，用我们当前的方法肯定是行不通的，因为我们当前的方法隐藏依赖是要到对应的配置依赖中去设置的，而我们显然不能对junit做什么动作，此时我们就需要用到我们的排除依赖，先来看看什么是排除依赖

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133642.png)

所谓排除依赖指的是我们主动断开依赖的资源，被排除依赖的资源不需要指定版本，其默认会排除所有版本。排除依赖所用的大标签是eclusions，其本身又是dependency的子标签，该标签下可以配置多个排除依赖，具体配置排除依赖的标签是exclusion，还有一点是，排除依赖需要到使用资源的项目中去配置，下图是排除依赖之后的效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133643.png)

要注意的一点是，虽然说排除依赖和可选依赖效果大差不差，但是其本质效果是不同的，排除依赖指的是主动断开某些资源，而可选依赖则是自己因此掉自己的一些依赖。前者要在使用资源的项目中去配置，而后者要在被使用的项目中配置。这两者是有本质差别的，千万别简单理解为他们一模一样

## 依赖范围

学习完了依赖配置和依赖传递之后，接着我们来讲解依赖范围

- 依赖范围

还记得我们之前学习坐标的时候提到过的scope这个标签吗？当时我们先将这个标签按下不表，而现在，我们要来正式学习这个标签了。首先我们的scope标签是dependency的子标签，一般是配合我们的依赖一起来使用，其主要作用就是用于规定依赖的范围，规定依赖的范围一共有三种，分别是主程序范围有效、测试程序范围有效以及是否参与打包。具体如何设置其范围请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133644.png)

log4j是日志资源，无论执行什么动作都要进行，因此其范围是compile，compile同时也是默认的范围。

而我们的junit是测试资源，我们当然只希望其只能够在测试中使用，添加到其他地方都有浪费资源，降低效率的可能，因此我们设置其范围为test。

而servlet-api是我们的轻量型服务器应用，我们希望主代码和测试代码中都能使用，主代码能使用是因为我们的主代码运行需要用到该jar，而测试代码能使用是因为测试代码需要用到该jar，但是打包我们却不需要，因为如果我们将其也打包进去就可能发生Tomcat服务器和war包中的servlet-api的版本冲突的问题，因此我们设置其范围为provided，令其打包之后不会打入。

最后是jdbc，jdbc我们实际上并没有对其进行实现，我们在主代码和测试代码中也没有用到过，我们只不过是用一些字符串对其进行调用而已，但是我们发布的时候，就要想jdbc也打包发布上去，不然别人没这个框架它没法用不是，因此我们要配置成runtime，但实际上我们偷懒配置成compile也是可以的，但是标准来说就应该要配置成runtime。

同时值得一提的是，如果我们配置的范围是compile，那么在maven窗口中没什么变化，但如果我们配置的是其他的范围，那么在窗口中会对应显示该其范围信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133645.png)

接着我们要解决一个问题，那就是如果我们的配置了依赖范围的资源进行了依赖传递，那么其范围会不会受到影响呢？答案是肯定的。假设我们在项目一中配置了项目二并设置了范围，然后项目二中对其中一个配置，假设是mybatis，同样也设置了范围，那么最后其范围会是怎样的呢？此时就需要用到下面的表格了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133646.png)

直接依赖我们可以理解为是项目一种对项目二设置的范围，而间接依赖可以理解成是项目二对mybatis设置的范围。如果我们两个设置的范围有交集，那么最终在项目一中看到的mybatis的资源的依赖范围就会是交集的部分依赖范围，如果没有，那就直接在mybatis中看不到

这个知识还是作为了解的，我们主要要记住的还是第一个表格

## 生命周期与插件

接着我们来学习maven基础的最后一个内容，生命周期与插件，在这节中我们主要讲述两个东西，一是构建生命周期，二是详细剖析下什么是插件

- 生命周期与插件

我们先来介绍下什么是构建生命周期，maven中的构建生命周期描述的是一次构建过程经历了多少次事件，我们的一个基本的构建生命周期的过程就是从编译到进入仓库，一共要经历五个事件，而这五个事件本身又构成一个生命周期（这一句不保证对）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133647.png)

当然，我们实际的构建生命周期的过程当然不止这一种，我们可以将我们的项目构建生命周期大体划分为三套，分别是clean（清理工作）、default（编译、测试、打包、部署）、site（产生报告、发布站点）。我们这里最重要的过程就是default过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133648.png)

然后我们对我们划分的这三种生命周期进行分别讲解，首先是clean，还是比较简单的，总体就分为pre-clean、clean和post-clean

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133649.png)

然后中间的就比较多了，如果我们直接看那肯定是非常痛苦的，我们这里要分开来看，假设我们只进行compile编译命令，那么其就会将编译前的过程都执行一遍。如果我们执行的是test测试，那么其就会将测试过程中的动作都执行一遍。其他的都是依葫芦画瓢了，这样我们就好理解多了，同时这个概念也是我们要明确记忆的。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133650.png)

那么现在就知道了生命周期其实控制的是在进行构建任务的时候执行的过程都有哪些，每一个过程上都有一些对应的动作要做，而那些动作又是由谁做的呢？这就涉及到我们第二个要讲的东西了，也就是插件

插件与生命周期内的各个阶段绑定，在执行到对应声明周期时就会执行插件对应的功能，maven默认在各个生命周期上绑定有预设的功能，但是我们如果嫌他的功能不够，我们也可以加多点功能

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133651.png)

现在我们上图中配置的是对源码打包的插件，加插件首先要有build标签，其下有plugins的子标签，同时也是大标签，plugin则是添加具体的插件，同样也可以添加多个插件。其下的groupId到version显然是在配置我们的插件依赖，而executions和execution虽然不知道啥意思，但是其作用我们估计也猜得到了，就是设置插件的用法，同样可以设置多个，goals和goal应该也是同理，其中goal标签内的内容指的是我们要生成的目标文件，这里我们要生成的是jar包，也就是我们的源码包，但实际上还有多个选项，比如我们可以选择生成我们的测试代码包，这个标签不是每一个插件都有的，同时如果我们需要一次打多个不同的包或者是多个包，我们可以在goals标签下设置多个goal标签来达成这个目的。而后面的phase则是指定我们插件要在执行到哪个过程中运行，其下填入的内容要与我们前一张图里的各种过程保持一致

最后我们可以做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133652.png)

我们的生命周期指的是运行的阶段，而插件是为了支持生命周期所要完成的事，生命周期可以理解为是几岁，而插件可以理解为是几岁要干的事情。我们能很容易就知道，我们的构建过程主要靠生命周期和插件，而下面的产出则是可有可无的。

# 分模块开发与设计

接着我们来学习Maven高级，这也是我们的阶段性学习的最后一个内容了，加油，冲冲冲。

## 模块拆分思想与pojo模块拆分

接着我们来学习我们的模块拆分思想，所谓的模块拆分思想，也就是我们可以将我们的一个大的项目的几个部分拆分为不同的模块，然后通过maven令其协同工作，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133653.png)

而至于system.exception类，这一个类我们就具体使用到的时候再具体将其放到对应的地方去，目前我们先只对比较要紧的四层进行一个分模块。

首先我们来完成我们的工程里的实体类的一个分模块，那么我们首先创建一个maven工程，不需要指定任何模板，然后我们将对应的工程里的东西直接复制到我们的java文件夹中，值得一提的是，我们推荐先创建对应的文件夹，然后将对应的类放置进去，接着我们对其执行编译命令，可以发现其编译可以通过，那么我们的第一个模块就完成了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133654.png)

### Dao模块拆分

然后我们来做我们的Dao层模块的拆分，同样我们还是创建对应的工程，然后我们将对应的类复制过来，最终我们的模块目录就如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133655.png)

这里我们的UserDao文件是会报错的，因为我们的文件里既没有实体类，也没有MyBatis对应的注解依赖，我们后者的问题可以通过导入依赖解决，但是前者该如何解决呢？此时要知道，我们的实体类的模块，其实也是一个maven工程，既然它是maven工程，那么其自身就可以作为一个资源引入到我们的模块中，这样就可以解决报错了。但是这里我们要注意一个问题，虽然我们引入了对应的坐标，但这样只会让我们的idea的编译器能够通过，实际上我们对该类进行编译时，其是会从本地仓库里去寻找我们的资源的，因此我们要先让我们的实体类执行install指令，令其先安装到我们的本地仓库中，这样我们对Dao模块的编译才能够通过

其实这个执行install命令的过程，也是一个打包的过程，到这里我们对打包就又有了更深一步的理解了，打包简单来说就可以理解为把对应的资源下载到我们的本地仓库中，而打包的方式是有很多种的，最基础的两种是打jar包和打war包

最后我们可以改造我们的pom文件的代码如下，我们这里第18-23行就是引入我们的自定义资源的代码，同时第7-9行是我们该类自己的坐标，后续被人引用时需要用到该类，同时我们的组的坐标可以在我们创建对应模块的时候指定，如果后续想修改，直接到pom文件中，也就是这里的第七行去修改也可以

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>ssm_dao</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>

        <!--导入资源文件pojo-->
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>ssm_pojo</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <!--spring环境-->
        <!--spring环境-->
        <!--spring环境-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>


        <!--mybatis环境-->
        <!--mybatis环境-->
        <!--mybatis环境-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.3</version>
        </dependency>
        <!--mysql环境-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--spring整合jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <!--spring整合mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.3</version>
        </dependency>
        <!--druid连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.16</version>
        </dependency>
        <!--分页插件坐标-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.2</version>
        </dependency>

    </dependencies>

</project>
```

首先我们需要讲解一下，我们之前的整个pom文件是因为我们的最开始的Spring环境有下面的关联环境所以我们才没必要特别再导入一次依赖的，而这里我们的每一个模块都是对应的Spring工程，都需要Spring环境，所以我们的任何一个工程都需要引入Spring的依赖，否则其会失效

然后我们重点要讲一下的是，为什么我们的分页坐标我们也跟着导入进来了，一般我们的分页插件是做在业务层的，而我们这里是数据层，按说应该是不用加的才是，那我们这里为什么要加呢？这是因为我们的Dao层的配置文件中是配置了分页插件了的具体设置的，如果分页插件都没有了，那他配置个几把，所以我们这里一定要引入

接着我们还值得一提的是，我们的pom配置文件中，应该还要引入我们的junit依赖的，因为我们的模块总是需要做独立的测试的，但我们这里图个方便就不添加了，毕竟我们连test文件夹都给删除了

最后我们去对应的配置文件中删除掉一些不必要的配置代码

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--开启bean注解扫描-->
    <context:component-scan base-package="com.itheima"/>

    <!--加载properties文件-->
    <context:property-placeholder location="classpath*:jdbc.properties"/>

    <!--数据源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--整合mybatis到spring中-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="typeAliasesPackage" value="com.itheima.domain"/>
        <!--分页插件-->
        <property name="plugins">
            <array>
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">mysql</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
            </array>
        </property>
    </bean>

    <!--映射扫描-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.itheima.dao"/>
    </bean>
</beans>
```

我们这里去除了开启事务的代码，因为我这里不需要开启事务,一般事务都是在业务层里开启的，然后执行编译就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133656.png)

### service模块拆分

然后我们来拆分service的模块，同样的步骤，最后我们得到的目录结构如下所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133657.png)

这里我们为了区分我们不同模块里的配置文件，所以我们这里给我们的对应的配置文件加入service的标识用于区分

首先我们当然是要引入对应的依赖，那么我们可以写入其依赖代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>ssm_service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>


        <!--导入资源文件dao-->
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>ssm_dao</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <!--spring环境-->
        <!--spring环境-->
        <!--spring环境-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>


        <!--其他组件-->
        <!--其他组件-->
        <!--其他组件-->
        <!--junit单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <!--spring整合junit-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
    </dependencies>


</project>
```

这里我们引入了测试类，因为我们要进行测试，然后我们引入了dao模块的资源，由于dao模块下已经引入了pojo的资源了，因此pojo的资源也会被一并引入，此时我们就不需要额外配置pojo的依赖了

最后我们要提一下的是，由于我们的配置文件的名字都已经改了，而我们测试时又要引入对应的配置文件，我们的解决方式是可以在对应的注解里引入多个配置文件

```
@ContextConfiguration({"classpath:applicationContext-dao.xml","classpath:applicationContext-service.xml"})
```

然后我们同样配置我们的配置代码，我们这里service层是要开启事务的，所以我们留下开启事务的代码

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--开启bean注解扫描-->
    <context:component-scan base-package="com.itheima"/>

    <!--开启注解式事务-->
    <tx:annotation-driven transaction-manager="txManager"/>


    <!--事务管理器-->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>
```

最后我们要注意的是，其编译器会提示我们的这里压根没有dataSource，但这只是一个编译器的提示，虽然报红，但是没有影响，因为我们的数据源已经加载到我们的资源库当中了，因此其实际运行是没有问题的，报红我们不管他就行了（其实这里也能解释我们之前做SB案例的时候依赖中报红但是不影响我们的项目运行的原因）

然后我执行其test命令和install命令

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133658.png)

### controller模块拆分

最后我们来进行controller模块的拆分，首先我们要创建一个webapp的模板，我们的表现层的结构与其他的情况不同，所以我们这里要创建对应的webapp模块，最后我们保留其结构如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133659.png)

可以看到我们这里将控制层以及统一格式类都放到我们的控制层上去了，然后我们来配置我们的pom.xml文件，这里同样是保留自己所需要的对应配置并引入我们的对应依赖，我们这里直接引入springmvc的依赖，其下有spring的依赖，所以我们这里不用特别打开spring的依赖了

```
<dependencies>

    <dependency>
        <groupId>com.itheima</groupId>
        <artifactId>ssm_service</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    
    <!--springmvc环境-->
    <!--springmvc环境-->
    <!--springmvc环境-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.1.9.RELEASE</version>
    </dependency>
    <!--jackson相关坐标3个-->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.0</version>
    </dependency>

    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>

<build>
    <!--设置插件-->
    <plugins>
        <!--具体的插件配置-->
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
                <port>80</port>
                <path>/</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```

接着我们配置的代码就只保留一个springMVC的，然后我们web.xml的代码里要加载的配置文件由于名字发生了改动因此会报红，我们这里的解决方式是将对应的加载代码修改如下

```
<param-value>classpath*:applicationContext-*.xml</param-value>
```

这样就代表会加载所有有该固定前缀的配置文件了，当然其语法检查器还是会报红，还是那句话，我们不管他就可以了，这并不影响我们的正常运行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133700.png)

最后我们可以对本章节做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133701.png)

# 高级特性

## 模块聚合

我们之前将我们的项目进行了分模块的开发设计，其中我们构建了一个由上往下的线性的依赖关系，那么此时就会产生一个问题，如果我们的模块其中一个进行了更新怎么办？这时其他的依赖是不知道其进行了更新的，因此也没有进行对应的更新，那么此时就可能会产生依赖之间不匹配的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133702.png)

为了解决这个问题，于是我们有了聚合这个功能，所谓的聚合，其作用就是创建一个空模块，该模块的作用就是用于维护我们的多个模块之间的关系

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133703.png)

那么如果要创建这个一个聚合模块，那么我们首先要创建一个maven模块，然后在其下的pom文件中指定其打包类型为pom，接着写入代码如下

```
<!--管理的工程列表-->
<modules>
    <!--具体的工程名称-->
    <module>../ssm_pojo</module>
    <module>../ssm_dao</module>
    <module>../ssm_service</module>
    <module>../ssm_controller</module>
</modules>
```

我们这里写入了我们的管理的工程标签，然后其下加入了对应的工程名称，接着我们对这个模块进行编译，其就会更新我所管理的所有工程了

这里要注意一点的是我们的顺序问题，我们编译时会发现无论我们的配置顺序如何改变，我们的工程编译总是先是pojo、然后是dao、service、controler这样编译的。这是因为我们的依赖顺序是pojo被dao所依赖，所以我们总是要先编译pojo，其他的也是一样的道理。简而言之，即是参与聚合操作的模块最终执行顺序与模块间的而依赖关系有关，与配置顺序无关。

## 模块继承

接着我们来讲模块的继承，上一节我们实现了多模块的统一维护，但其仍然存在一个问题，那就是版本兼容问题。比方说其下的不同模块之间可能都依赖同一个资源，而有一天另一个模块更新了其使用的对应的资源，那么此时就可能会出现资源不兼容的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133704.png)

此时我们就期待有这么一个文件，其不但可以维护各个模块，而且还可以为其统一的资源做调配，如果他们都是使用一个资源，那么就按照其父类的资源来用，如果有一个需要更新版本，那么父类就会对其进行处理，看看能不能统一更换版本，这个其实就是我们本节要讲解的继承

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133705.png)

首先我们要使用继承，当然要先定义一个用于维护整个工程的父类模块，然后我们在其下利用dependencyManagement标签来声明此处进行父类的依赖管理，接着其下正常写入要进行管理的依赖，注意我们此处需要写入版本，而且其下的依赖资源应该最好都是以一个直接的形式存在，而不是我们的依赖依赖会默认引入另外一个依赖的隐式存在，前者的直接存在可以让子类更好的获得所需要的资源，我们维护起来也更快。

我们一般的依赖管理是在其下添加我们的工程中所有用得上的工程模块依赖的版本，然后我们再使用这个模块对我们的项目里的小模块进行统一的管理的维护，这里所有的资源不但可以写入别人的资源，甚至可以添加自己的工程模块的依赖。

而对于插件，我们也有对应的pluginManagement标签来进行对应的插件管理，使用方法和前面是一样的

然后我们对于子类的模块，如果其要使用父类的资源，那么其就要先定义该工程的父工程，其实就是填入父工程的坐标而已，最后还要写入父工程的pom文件的路径，注意，这里要填入的是寻找对应的pom文件的全虚拟路径，而不是直接整一个pom文件的名字下去

```
<!--定义该工程的父工程-->
<parent>
    <groupId>com.itheima</groupId>
    <artifactId>ssm</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--填写父工程的pom文件-->
    <relativePath>../ssm/pom.xml</relativePath>
</parent>
```

接着我们的子类只要引入了上面的内容，我们如果要进行统一管理，就可以不用标注自己要使用什么版本了，直接使用父类规定好的版本就可以了，如果的确有什么版本是父类无法提供的，或者是有什么坐标父类里没有规定的，那么到时候就手动引入依赖或者是规定版本就可以了

最后我们还看看我们可以在父类里提前设定的资源的管理的东西有哪些

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133706.png)

然后我们来说说继承和聚合的异同，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133707.png)

## 属性定义与使用

接着我们还有一个问题，那就是我们的在的父类中，有许多的版本可能都是一样的名称，其版本分布还是一样的，如果以后一个更新了，我们就得一个个配置慢慢改，那该多麻烦啊，有没有什么方法可以让我们的一下子一次修改就可以将下面的修改都修改完呢？当然有，此时我们就要讲我们的属性了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133708.png)

要使用属性，当然我们要先定义出属性，所以我们应该写入其代码如下

```
<properties>
    <spring.version>5.1.9.RELEASE</spring.version>
</properties>
```

然后我们可以在下面用${}的形式来使用我们上面自己定义的属性，这里我们的属性都要在properties标签下自己定义，我们的往里面可以自定义标签名，这就相当于是属性名，我们可以在下面引用我们的属性名

```
<!--添加自己的工程模块依赖-->
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>ssm_pojo</artifactId>
    <version>${version}</version>
</dependency>
```

而且在${}里面还有许多的内容我们可以自己引用，这些都是其事先提供好给我们的内容，我们了解下就差不多得了，这里我们就不特别提一下了

## 版本管理

接着我们来讲解我们的版本管理，这一节主要是理论上的内容，先来看看的两个版本的不同区别，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133709.png)

在具体到我们本项目，我们如果想要更新我的子模块的版本，那么我们就要具体给其version标签下的内容进行更改，这里我们值得一提的是，如果我们想要更改我们的子模块的版本号，那么我们就应该要先将我们的version标签写入再更改，因为我们之前的子模块的版本号是直接沿用父模块的，所以我们要更改的话自然也需要自定义标签，这个很好理解其实。还有一点要提一下的是，工程版本各个企业里是不同的，因此不用记忆，了解下就行了其实

最后来看看我们企业级开发里的工程版本号约定

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133710.png)

## 资源加载属性值

我们上一节里，我们们多文件的情况统一到一个变量中去管理，这样便于我们的维护，但实际上，不止pom配置文件可以这么做，我们的properties文件也可以进行这样的管理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133711.png)

我们首先在对应的用于维护的模块下的pom文件上写入其代码如下

```
<properties>
    <spring.version>5.1.9.RELEASE</spring.version>
    <jdbc.url>jdbc:mysql://localhost:3306/ssm_db</jdbc.url>
</properties>
```

可以看到我们这里定义了jdbc.url的变量，并且赋予了对应的值，然后我们可以将我们的jdbc.properties的文件代码修改如下

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=itheima
```

可以看到我们上面就是利用${}来获得对应的属性的值而已，但是只是这样还不行，我们还需要到我们的主维护文件中去配置资源文件的信息，其实就是开启解析

```
<!--配置资源文件对应的信息-->
<resources>
    <resource>
        <directory>../ssm_dao/src/main/resources</directory>
        <filtering>true</filtering>
    </resource>
</resources>
```

我们这里的代码都是在build的标签下写入的，这点要先明确，我们先创建一个resources大标签，然后创建对应的子标签，接着我们往里面先指定目录，注意我们这里的目录是先从我们当前的工程的文件下去寻找的，因此我们要先令其回退一级，接着我们再写入对应我们要进行加载的资源的文件路径，然后再设定其filtering属性为true，此时我们的解析就已经开启成功了，此时其能够正确寻找到我们的要开启注解的文件

接着存在的问题是，我们可能会其他模块也想要进行对应的修改，但是如果我们一个个配置的话，那就太麻烦了，此时我们可以将对应的directory标签的内容更改如下

```
<directory>${project.basedir}/src/main/resources</directory>
```

这里我们的project.basedir是其先提供好给我们的一个属性，其代表所有模块的根目录，将代码改为上面的格式，那么其就会自动寻找所有符合该结构的目录的内容并进行相应的修改，又因为我们都知道我们的模块有一些结构是一样的，所以后面的结构可以不做改动，从而实现我们的需求

最后如果我们需要我们的test里面的对应的配置文件也能同样管理的话，只需要再写入代码如下

```
<testResources>
    <testResource>
        <directory>${project.basedir}/ssm_dao/src/test/resources</directory>
        <filtering>true</filtering>
    </testResource>
</testResources>
```

这里我们使用testReources标签并且我们将目录改为test，然后我们的test文件夹里的配置文件就也能够使用我们规定的属性了

## 多环境配置

我们实际在企业开发的时候，我们的环境可能是很多遍的，在生成环境、开发环境以及测试环境中，都需要更改配置，那我们不能每次更换环境我们都手动更换一次环境吧？那就太麻烦了，为了解决这个问题，maven提供了多环境兼容的解决方案

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133712.png)

要使用多环境的解决方案，首先我们要创建多环境，多环境的标签是profiles，其下可以使用子标签profile来创建我们的环境，我们的环境需要唯一标识，因此我们需要给予其对应的id，然后我们再其下定义需要换用的属性值就好了

```
<!--创建多环境-->
<profiles>
    <!--定义具体的环境：生产环境-->
    <profile>
        <!--定义环境对应的唯一名称-->
        <id>pro_env</id>
        <!--定义环境中换用的属性值-->
        <properties>
            <jdbc.url>jdbc:mysql://localhost:3306/ssm_db</jdbc.url>
        </properties>
        <!--设置默认启动-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>

    <!--定义具体的环境：开发环境-->
    <profile>
        <id>pro_env</id>
        <properties>
            <jdbc.url>jdbc:mysql://localhost:3308/ssm_db</jdbc.url>
        </properties>
    </profile>
</profiles>
```

需要不同的环境，我们就自己去创建多个相同的标签然后指定我们所需要的内容。然后我们还需要指定其加载的环境，指定的方法很简单，我们点击右上方的启动标签，然后自己定义其方法，假如我们想让我们的install加载我们的环境的话，那么我们就要在其后面写入-p 环境定义id，然后我们执行该方法的时候就会通过我们指定的环境配置进行加载了

同时如果我们每次都要去自己指定的话，未免有些麻烦，其实我们还可以指定最开始默认的配置，那就是我们上面11-14行的代码，这里我们设置我们的命令默认启动时会加载的环境，这样我们就每次都去指定了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133713.png)

## 跳过测试

这是一个了解性的内容，我们实际开发的时候对于可能有很多的测试要做，而可能有些测试我们又已经能确定他们已经好了，我们不需要再测试了，此时我们就需要跳过这些测试，但是注意，跳过测试可能会造成一大堆有的没的问题，所以平时没事就不要跳过测试

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133714.png)

跳过测试有三种方式，我们这里都进行逐一的讲解，第一种是进入对应的maven窗口里直接选择对应的要跳过的命令，然后点击上面的闪电标就可以了，这样该方法就会被跳过了，我们执行编译的时候也不会执行这个命令，重新点击则可以恢复

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133715.png)

第二种方式是选择对应的pom文件，右键选择Run maven选项，然后在弹出的窗口里选择Create new goal，我们输入对应的命令就可以了，比如我们输入如下命令就可以执行安装并跳过测试

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133716.png)

第三种方式是最重要的方式，我们都知道我们的maven的生命周期的执行过程本质靠的还是插件，那么我们要配置的话，就从插件中来配就可以了，我们去其仓库中寻找，我们测试的插件都是依赖于一个maven-surefire-plugin的插件，那么我们直接在配置标签下写入该代码，这里值得一提的是，我们的群组id是可以不写的，因为我们这里使用的插件是maven自己的，当然你非要写也可以

```
<!--配置跳过测试-->
<plugin>
    <!--配置资源的群组id可以不写也可以写-->
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.12.4</version>
    <configuration>
        <!--设置跳过测试-->
        <!--<skipTests>true</skipTests>-->


        <includes>
            <!--包含指定的用例-->
            <include>**/User*Test.java</include>
        </includes>

        <excludes>
            <!--包含排除的用例-->
            <exclude></exclude>
        </excludes>
    </configuration>
</plugin>
```

然后我们在其下可以配置configuration标签，其下可以设置skipTests属性，设置为true则跳过所有测试，我们也可以使用includes或者是excludes标签进行包含性排除或者是排除性排除，里面的设定是可以使用通配符来进行对应的设置的

# 私服

接着我们进入到我们的Maven的最后一个内容，也是我们的短期技术栈学习的最后一个内容，也就是私服

## nexus服务器安装与启动

我们先来说下我们为什么需要私服，这其实很简单，比如所当我们的多个用户分开开发的时候，此时为了获得其他人开发的资源，这时就需要私服来让他们能够便利地获得其他人构建的模块。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133717.png)

这里要注意的是，我们的私服是要和我们的中央服务器分开的，这时当然的，我们公司的东西怎么能随便放到中央上呢是吧

私服的种类有很多，我们这里要使用的私服的名字为Nexus

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133718.png)

其实，对应的资源我们都已经放到学习资料里去了，我们将其拷贝出来然后进行一个解压，接着我们可以看到两个目录

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133719.png)

这两个目录，前者是我们的程序启动的执行目录，而后者可以理解是工作空间，其运行之后所有的数据都在里面。然后我们进入第一个目录，点击进入bin目录，接着运行cmd，敲入命令nexus /run nexus，接着等待一段时间后就可以看到我们的服务器已经启动了

其默认的网址是localhost:8081，如果我们想要更改端口号的话，我们可以进入nexus目录下的etc目录，里面有一个properties文件，我们可以在里面更改对应的配置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133720.png)

同时在安装路径的bin目录下的nexus.vmoptions文件保存有nexus服务器启动的对应的配置信息，这个了解下就好了

## 仓库分类与手动上传组件

接着我们来讲解下我们的私服存放资源的规则，我们一般会在私服里先创建一个仓库，专门存放从中央下载而来的资源，接着我们再创建若干个仓库用于存放我们程序员自己开发的模块资源，然后将这些仓库全部列到一个仓库组，接着有人需要找对应的资源时就会自动搜索该仓库组，搜索到了就给他，找不到就报没有

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133721.png)

简单来说就是我们会在我们的私服里创建宿主仓库（hosted）、代理仓库（proxy）、仓库组（group）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133722.png)

然后我们来进行正式的对仓库的操作，首先我们应该要进行登录，那么我们点击右上角的login，到其指定的位置找到密码，登录用户名指定是admin，然后重新设置用户名和密码，我们这里全设置为admin。最后其会问我们要不要允许我们的仓库被匿名访问，我们这里选择不允许

我们进入到对应的私服里，可以看到这里有很多设置，其都对应具体的功能，我们这里将最为重要的功能进行了对应的标注

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133723.png)

然后我们要自己创建一个仓库，我们选择服务器设置，然后选择左边的Repositories，再选择右边的create选项，接着我们选择创建maven3的host仓库，取个名字，然后选择存放的东西是快照还是稳定版本，我们这里选择稳定版本，然后点击创建，接着我们点击进入maven-public，将我们新创建的仓库加入到该仓库组中就可以了

然后我们就要往我们的创建的仓库里存放东西了，我们首先点击Browse，选择我们的仓库，接着我们点击左上方的upload component，进入如下的界面，上传我们的资源

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133724.png)

此时我们再点进该仓库我们就可以看到我们这里已经有了我们之前所存放的内容了，原版我们的内容是一个jar包，而在这里我们可以看到我们的jar包被以一个工程格式的方式进行了对应的展示，最后我们要注意的是我们之前如果设置仓库为保存稳定版本的内容，那么其就不会允许我们保存带有快照关键字的文件

## 本地仓库访问私服

接着我们来学习idea中资源的上传与下载，我们的上传需要访问私服的用户名和密码，以及我们需要上传的位置。而我们的下载不但需要前者，还需要下载的地址，idea自然是无法自己连接我们的私服里的仓库，我们一般需要我们的本地仓库作为媒介，也就是我们的maven仓库

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/20220817133725.png)

接着我们要先设置我们的本地仓库访问私服的用户名的密码，先进行我们的maven仓库中，编辑我们的settings.xml文件，然后在servers标签中写入代码如下，我们这里的id可以随便配置，但是一般来说我们最好和我们的仓库名保持一致

```
	<server>
			<id>heima-release</id>
			<username>admin</username>
			<password>admin</password>
	</server>
	
	<server>
			<id>heima-snapshots</id>
			<username>admin</username>
			<password>admin</password>
	</server>
```

接着我们要配置镜像，我们需要配置什么东西需要从我们的本地仓库中获取，那么我们可以配置我们的代码在mirrors标签如下

```
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
	
	<mirror>
        <id>nexus-heima</id>
        <mirrorOf>*</mirrorOf>
        <url>http://localhost/repository/maven-public/</url>
    </mirror>
```

这里我第二个配置的就是我们现在所配置的，其意味着所有的资源如果中央仓库找不到，或者非中央仓库的资源就会到这里被拦截，然后从我们的私服里去找，这里的私服就是我们的设置的公共仓库组的url

最后别忘了我们的本地仓库中的设置的文件要和我们安装的maven的仓库文件保持一致，因此不要忘了进行一个拷贝覆盖原来的设置文件

## idea访问私服与组件上传

最后我们来看看idea如何将我们的模块上传到我们的私服中，首先我们应该要在配置文件中去配置当前项目访问私服的资源保存位置，那么我们可以写入其代码如下

```
<!--发布配置管理-->
<distributionManagement>
    <repository>
        <id>heima-release</id>
        <url>http://localhost/repository/heima-release/</url>
    </repository>
    <snapshotRepository>
        <id>heima-snapshots</id>
        <url>http://localhost/repository/heima-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

我们这里进行了配置管理，在其下我们配置仓库的id和url，这样就可以让我们的项目正确上传到对应的仓库中，我们这里配置了两个，一个是快照版本的仓库，另外一个是稳定版的。

然后我们要上传就直接点击maven选项卡中的deploy选项就可以了，这样其就会自动将对应的资源推送我们的私服里的仓库了