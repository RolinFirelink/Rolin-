- 网页的构成

一个网页主要由三部分技术构造而成，分别是HTML，CSS和JavaScript。其中HTML的作用是制作网页基础内容和基本结构，CSS是用于美化网页，而JavaScipt用于制作网页的动态效果。本节我们主要学习HTML

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd9b8cb8f13764b08c46ab0d597fac99e.png)

- HTML的介绍

HTML其实是HyperText Markup Language（超文本标记语言）的缩写，超文本的意思就是比普通文本更加强大，而标记其实就是标签的意思，可以使用一系列的标签使得网络上的文档格式统一，使得分散的资源连接为一个逻辑整体

2014.10月，万维网联盟HTML5的规则制作完成，其不但提供了一套完整的规范，还提供了一套免费的教学文档，后续学习中我们会使用到这个文档

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2786dab7d3c1ed14a2b8657d8d07e023.png)

- HTML的组成

HTML页面是由一系列的元素组成的，而元素是通过标签创建出来的，因此我们先来讲解标签

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb9816b4641da1e28d6588b4a98cc8305.png)

那么什么是标签呢？其又有什么作用呢？其实标签简单来说就是两个<自定义内容>包括而成的内容，这个内容和我们以前学习图书馆管理系统的时候接触到的FXML界面相关内容其实挺相似的，他们都是有前必有后而且后者要加</自定义内容>来作为结尾。那么标签有什么作用呢？标签可以用于设置文本样式、图片样式、超链接样式等。一个元素包括一个开始标签和一个结束标签，中间有存放我们定义的内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe935ffec52fa75a3536a5a66a622e3ea.png)

标签的另外一个重要的内容就是属性，属性可以为标签提供更多的信息，但是要注意的是属性只能添加到开始标签中，而且其格式为属性名=属性值，具体使用方式可以看下图的例子

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE47dafcfe5c0259519e66a2589f547958.png)

- HTML入门案例

学习了上面的内容之后，我们话不多说，速速来试试新技术，趁热打铁。请看我们要实现的案例效果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEac1a1d7554e471971e7c4cc24054733b.png)

可以看到我们在左上角中要显示01-入门案例，在中间居中的地方要显示这是我的第一个HTML入门案例，要实现这个案例，我们要按照以下步骤进行案例构造，这里使用的工具是idea

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE898a32601f54628a153b1357f1faf438.png)

注意这里创建的内容是HTML5，不是其他的啊，创建完成之后我们可以看到如下内容

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

这里我们一个个对其代码进行说明，首先第一行的代码是H5的文档声明（有什么作用还不知道），以前的版本文档声明往往很长，H5中对其进行了优化，使其文档声明只有一句，方便不少

第二行是根标签，注意一个文件中只能有一个根标签

第三行<head>是头部标签，其实就是我们的页面的右上角显示的东西，第五行Title内写入什么，就会在右上角的窗口中显示什么，我们这里应该要写入01-入门案例

第七行<body>，是代表身体，我们这里其实可以理解为将一个页面分为了头和身体两部分，头部主要是上面的窗口内容，而身体则是页面中的大窗口的所有的内容，我们的具体构造页面就可以在这个body中进行构造

第九行和第十行时身体和根标签的结束标签

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE28263e3266c830926a03c1f2be589585.png)

那么最后我们可以将代码写成如下形式

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-入门案例</title>
</head>
<body>
<h1 align="center">这是我的第一个HTML入门案例</h1>
</body>
</html>
```

最后我们只要点击右上角用对应的浏览器打开就可以看到我们所构造的页面了

- HTML概念小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9d0c8f3e90cdade6044af33fab18b0ad.png)

