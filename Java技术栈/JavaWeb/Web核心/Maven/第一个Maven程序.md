配置完了我们的maven之后，我们现在就来做我们的第一个Maven程序

- Maven项目结构

首先我们来看看maven的工程目录结构

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2955328f7bc407ec2c1ea9803c72d96e.png)

我们就手动创建对应的包就行了，这里就不演示了。我们直接说吧，首先创建的第一个project文件夹并不是我们的maven工程的结构，其下创建的java-project才是我们maven工程的结构，之后我们在其下创建对应的src文件夹src下要有两个文件夹，分别是main和test，其下都要有对应的java和resoureces文件夹，java文件夹里放java文件，resources放配置文件，其中main是我们编写我们的源代码本身的地方，而test则是我们用于测试的地方。这里有一个问题就是，为啥test还要有一个配置文件，答案是真的要，测试的时候有时候也要用到一些特别的配置文件。

然后是我们创建的文件是在java里的，但是我们要注意在里面创建文件时也要有对应的包的分类。接着我们在对应的包里创建的代码和测试类

最后，我们的创建的maven工程还需要一个最重要的东西，就是pom.xml文件，没有这玩意的话maven工程根本就不成立，因此我们这里要构建一个xml文件，直接自己写显然不显示，我直接拿别人的然后稍微修改，具体请看代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5d3cbc5f5dc0ec03b39e25afd1bd0908.png)

开头的部分其实就是我们的文件头了，这个不多说了。接着在project标签下有三个属性，分别是xmlns、xmlns：xsi、xsi：schemaLocation，这里面的内容我们先了解下就行了，暂时不要求掌握。modelVersion则是maven对象模型的版本号，这个版本和我们目前使用的3.5.3的版本其实没有关系，其指的是maven的pom对象的版本升级到了4.0.0。后面的groupId、artifactId以及version我们都填写我们自己的工程里的相应内容，而packaging则代表我们的工程最后要生成什么，我们这里填写jar最后就生成一个jar包，实际上还能填写很多。然后后面的东西就填写对应的要使用到的jar包的坐标和版本就行了，这些大概就是我们一个pom.xml文件的组成了。

- Maven项目构建

首先我们来看看Maven的构建命令，其均是使用mvn开头，很后面添加功能参数，其中compile代表编译、clean代表清理、test代表测试、package代表打包、install代表安装到本地仓库，下面我们具体来演示一下这些命令

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE250c3f412f144b53f8ba6f3b761cb01e.png)

注意，我们这些命令都是要在我们的cmd窗口里调用的，首先我们要进入到我们的文件对应的位置，也就是在我们项目的首位置，其实就是project-java文件夹中，首先要编译我们的文件，写入mvn compile，第一次运行会比较慢，因为我们要下载我们编译所需要的jar包或者是插件，之后就会很快了，在cmd里会有对应的下载信息和执行动作的日志，我们可以去查看

编译完成之后会在我们的项目中生成一个target文件夹，文件夹内存放的就是我们编译完成之后的内容，如果我们不想要编译完成之后的东西，也可以输入mvn clean命令来进行清理

然后是mvn test命令，该命令就是用于做测试的，该命令执行之后，target下会多两个文件夹

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb4bc4535b759f11ef02cafcad699ff14.png)

而且我们可以在cmd中看到如下内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa9679bedbd2978c253119b83820fe617.png)

上面的内容我们可以看到先显示了结果hello maven，然后测试结果里，运行成功一个，失败0个，错误0个，跳过0个，总共用时0.067秒，下面的Results也是同样的内容，那为什么要显示两个同样的内容？这其实是因为我们的测试程序里只有一个方法导致的，实际运行中，上面的结果会一个个显示，而下面的Results则会显示总和的结果

我们的结果就放在surefire-reports文件夹中，里面有两个文件，一个是txt文件，那个文件其实就是我们cmd中的结果内容，而另外一个xml文件则有着所有的具体内容，包括JDK的版本，测试执行的环境等等等，最后一段则说明其测试了什么东西，名字是什么，花费了多久等等

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc807fced9c4845b37b4620ae5d4517e9.png)

我们以后也可以将该文件分为两部分，一种是环境，上图中testcase上的都是环境，而其他内容都是测试结果，这就是target提供的测试报告

classes内存放的是源程序生成的字节码，test-classes则是存放测试程序生成的字节码

接着我们来看第四个mvn package，其运行完成后会在target文件夹中生成一个jar包，该jar包就是我们的项目打包之后的结果，注意，打包只打包源程序，而不会打包测试程序，这是当然的，测试程序你打包来干嘛是吧，我们注意看打包命令执行了什么动作，我们会发现其先执行编译，这是当然的，你都没编译我怎么打包？编译了之后再进行测试，然后再执行测试程序，一切无误之后再执行打包，这些动作都是为了确保我们的打包的内容是没有错误的，确实有用的

最后mvn install，这个命令的内容是会将我们打包的东西放到仓库里，我们会发现其放到了com包itheima包下，之所以这样是因为最开始我们的配置文件是这样填写我们的坐标的，所以会有这样的放置效果，如果我们填写的是其他的内容那么也会有其他的效果

- 插件创建Maven工程

上一节里我们手动创建并运行了我的maven工程，本节我们来利用插件来完成我们的需求，这一节主要偏理论，了解下就行了

先来看看我们利用插件创建工程的两种方式，一种是创建java工程，另一种是创建web工程

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEceae572a789f3f3ef2fcfe831dd2f48d.png)

我们输入对应的命令其就会自动帮我们创建了，其帮我们的创建的java工程内容都是默认没有配置文件的，如果我们需要的话，那么我们就要自己创建对应的resources文件夹。然后是web工程，创建web工程我们能看到其配置文件里多了一句话，这句话的内容就是指定其打包生成的文件类型是war包的，生成的web工程没有测试文件夹，也没有存放java文件的文件夹，但是有resources文件夹，同时其下还有webapp文件夹，还贴心的帮我们生成了jsp文件，当然，我们用不用他生成的jsp文件是另一回事。

最后我们来看看普通创建的maven工程的目录和插件创建web工程的目录的对比图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd7b0c78ff32e2eefa1c0237c06d2a928.png)

我们其实主要要记住的就是webapp的文件夹就行了，我们以后也比较常创建web工程，要记住web是有这个文件夹的

- idea创建Maven工程

手动的方式创建Maven工程演示完毕之后，接下来我们就要用idea来创建Maven工程了。我们先创建一个空的项目，然后我们在模块上设置我们的Maven工程，当然别忘了我们首先应该要在设置里的project里将我们的JDK的版本设定为我们当前使用的JDK，反正我的是1.8。然后我们在settings中搜索maven，进入对应的maven设置，我们这里要选择其使用我们自己的maven，也就是3.6.1的版本的maven，然后勾选下面的重写xml，选择我们的maven的conf目录下的setting的xml文件，接着点击ok。然后我们就可以正式来创建我们的maven工程了，按照步骤创建好之后可以看到这样的画面

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc35d93415a7b306f9aaa40215275fb36.png)

我们这里将对应的java文件夹设置为蓝色的，其代表里面存放源码，resources代表其里面存放资源文件，而test里的java文件夹则是设置为绿色的，其代表里面存放测试的代码，其下的resources是我们自己创建的，其是用于存放测试的资源的文件夹。我们除了可以在这里进行一个文件夹属性的标记之外，我们还可以在程序外边右键对应的文件夹，选择Mark Directory as来对文件夹的属性进行标记

然后我们在左侧可以看到maven标签，点击该标签可以看到如下内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc99aec4abbc84519bfae7aa730fe4592.png)

这里我们要提一点的是我们在pom.xml配置文件中配置的东西，在这里是会有对应显示的，如果没显示那就刷新看看，一般刷新之后就会显示了。Lifecycle里都是我们的一些命令，我们双击就可以执行了，执行之后其也会执行对应的功能，会产生相应的文件并在控制台上打印日志信息。

我们还有一种启动方式，那就是点击idea右上角的Add Configuration...，然后点击左侧的Maven，然后我们的name可以自己设定，Command line里写入我们要执行的命令，Working directory则写入我们要执行测试的java文件的地址

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1cdb86e22f67f37664a26bd2b35aaf5a.png)

这种方式颇有种脱裤子放屁的感觉，但是这也是有它的用武之地的，最经典的场景莫过于我们需要进行debug的时候。我们实际的测试程序也是做出来了的，但是有些搞不明白的问题就是了。

- Idea版使用模板（骨架）创建Maven工程（3.6.1版）

我们接下来来学习用模板来创建对应的Maven工程，其实这里也就是我们创建Maven模块的时候勾选Create from archetype然后选择对应的模块就行了，我们要创建java工程时要选择的模块是

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE68c1961ed4666ee9a583fbff3808ad3b.png)

我们可以看到其创建的目录有main和test，但是他们其下都没有resources文件夹，而且还没有对应的标记，我们这里创建对应的文件夹另外加上标记就完了。然后我们创建web工程，要选择的模板的是

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc965f711e4accb1f2083f746b8203e90.png)

其目录下虽然有resources文件夹，但是其下没有java文件夹和test测试文件夹，不过多了一个webapp文件夹，内含WEB-INF，该文件夹里面有对应的配置文件，但是很多我们目前都用不上，暂时不需要看。另外还需要什么文件夹我们就自己创建就完了

- tomcat插件安装与web工程启动

接着我们来讲最后一种运行方式，利用tomcat插件启动，首先我们要安装tomcat的插件，其安装的配置文件的内容如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaeeacb93afb4ab2bb4c546c19b0e8bed.png)

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

