本章节我们就来正式学习Spring了，首先我们来介绍什么是Spring

- 框架的概念

所谓Spring就是我们平时开发的用的框架的一种，是一种经过验证的具有一定功能的半成品软件，我们的开发就可以基于其进行开发。使用框架进行开发开发具有以下好处

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE542628d135ecd23790e596c36f7f703f.png)

- Spring概念与体系结构

我们先来介绍Spring是什么，我们之前说过Spring其实就是一种分层的JavaSE/EE应用的full-stack框架，那么其自己也有框架本身的好处，具体到Spring中就是就是有以下五个好处，分别是分层、JavaSE/EE、full-stack、轻量级、开源。接下来我们分别说说这五个好处

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8169c865ed24077dcf863b0f39406d16.png)

首先是分层，分层的好处意味着在Spring中，其下的各种层级的功能都能被独立拿出来使用，而不是说我们要用spring就必须要用他的所有，我们可以只使用他的一部分

第二个是JavaSE/EE，在Spring中不仅能用于JavaSE，还能用于JavaEE的开发。我们都知道在JavaEE中我们是将我们开发的项目分为三层的，分别是Servlet、service以及DAO，而对于Spring而言，其不仅在Dao层中有解决方案，其在Dao层之上的两层也有解决方案，也就是其技术覆盖度宽

第三个是full-stack，也就是一栈式，又称全栈，简而言之就是Spring中提供了一套的解决方案，各种问题的解决方案包括连接数据库等等等等一类的问题其都有，而且其还有一点特点，那就是如果我们用的一套技术或解决方案全部都是Spring的，那么我们的项目的整体效率就会得到显著的提高，原因在于内部的各种问题都先帮我们考虑到了，而如果我们只使用其一部分的话，那些问题就要自己去解决了，这个特点叫做粗粒度封装

第四个是轻量级，这个大家都懂了，占用内存小，资源消耗小

第五个是开源，这个最主要的作用就是无论谁都可以给这个项目发展，加功能

接着我们来讲下Spring的体系结构

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5b3ee2d334f5e9a6ae182844d8ef9c50.png)

首先在Spring中，最重要的是核心容器，也就是Core Container，其下有Beans、Core、Conetext以及SpEL，后续我们会对这个四个进行逐一讲解，现在我们可以简单理解成Spring如果想要工作那么就要靠这四个内容。

在其之上是中间层，中间层里最重要的AOP，其也是Spring核心功能的体现之一，而Aspects简单来说，如果我们把AOP理解为接口，那么Aspects就是其实现类。后面的两个都是小功能，无关紧要

再往上就是其应用层技术，其第一块应用层技术就是Data Access，其意为数据访问，其下有ORM和JDBC，这两个我们都比较熟悉了，其整个结构可以理解为Spring的数据解决方案。其数据解决方案是在内部进行了接口的规范，而具体的实现类是要我们自己导入的，这样我们需要用什么样的解决方案，就导入对应的jar包就可以了。比如之前我们也说过，不同的数据库的JDBC的实现类也不一样，但都有一样的规范，那么我们在Spring中想用不同的数据库的话，就对应导入JDBC的对应实现类就可以了，内部已经帮我们准备好了。而整个这个部分在做的事情就是Integration，也就是集成，集成可以简单理解为其集合了多个技术，用户需要什么对应的技术直接拿来用就完了

除了数据层之外，其还有Web层的解决方案，其也做集成，其下有WebSocket、Servlet、Web、Portlet技术，都已经有了，我们需要直接拿来用就完了。而且Spring自己也有自己的Web实现，当然，我们用不用取决于我们

同时为了保证其自己做的功能的确能够使用，所以其提供了一套Test测试功能，用这个功能可以测试其功能是否可以用，是否完好

- Spring发展史

这个了解下就行，直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa157cd34dc91aa6a48864680b08fff7c.png)

然后是Spring的优势

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaf32e8cd8f1fccb1e88a20fcc21ded07.png)

