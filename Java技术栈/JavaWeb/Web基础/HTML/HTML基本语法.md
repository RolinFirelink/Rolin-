- HTML的注释

我们先来学习HTML的注释相关内容，这里只有一点需要讲，就是快速注释内容的快捷键是ctrl+shift+/，无了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaeee8fb03997950ea0ccfe9e1f1a3921.png)

- HTML的标签

接着我们来学习HTML的标签，<h1></h1>是一级标题，随着数字的变换，还有二级标题和三级标题

然后是自闭和标签，自闭和标签有<br/>和<hr/>，前者的作用是换行，后者的作用是在界面上加一个下划线

类似于<b>和<u>一类的标签是行内元素，可以给我们的文字加上属性（比如加粗或者是加下划线），其嵌套格式是前对前后对后，i标签是添加斜体

块级元素里<p>的作用是作为一个段落标签

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd8ab0faa4c2200fe98467a3cb561d631.png)

最后我们可以构造一个学习代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-入门案例</title>
</head>
<body>
<!--这是一个注释，它的内容不会被浏览器所解析-->
<h1 align="center">这是我的第一个HTML入门案例，是一级标题</h1>
<h2 align="center">这是我的第二个HTML入门案例，是二级标题</h2>
<br/><!--这是一个自闭和标签，其作用是换行-->
<hr/><!--这是一个自闭和标签，其作用是添加一个下划线-->
<span>这是span行内元素，两个标签添加的文字不会令其换行</span>
<span>这是span行内元素，两个标签添加的文字不会令其换行</span>
<div>这是div块内元素，自己独占一行，后面的元素会自动换行</div>
<div>这是div块内元素，自己独占一行，后面的元素会自动换行</div>
<br/><!--这是一个自闭和标签，其作用是换行-->
<hr/>
<div><u>u的作用是给文字加上下划线</u></div>
<div><b>u的作用是给文字加粗</b></div>
<div><i>u的作用是给文字添加斜体</i></div>
<p>p代表的是一个段落标签</p>
</body>
</html>
```

- HTML的属性

所谓属性就是可以提供不会直接显示在内容中的信息，可以作为改变标签样式或者是提供数据使用，其具体案例在我们学习图书馆管理系统的时候其实已经有所接触了。其定义格式为属性名=属性值，这里要注意的是在同一个标签中属性的名称必须要唯一，即是比方说我们不可以提供了一个中间属性之后再提供一个中间属性，这是不允许的

常用的属性有class、id、name、value、style，其中class的作用是定义元素的类名，用于选择和访问特定元素。而id是定义元素的唯一标识，前者可以重复使用，后者不可以，style可以定义元素的css样式，比如添加背景颜色什么的，更多的其他内容我们用到再解释

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE654b422d7e733bfcc7657980bbf89eda.png)

最后我们可以定义的代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>属性</title>
</head>
<body>
    <div class="cls">第一个div</div>
    <div class="cls">第二个div</div><!--可以看到class重复使用cls是允许的-->
    <div id="d1">第三个div</div>
    <div id="d2">第四个div</div><!--如果我们还是定义d1，那么编译会不通过-->
    <div style="background-color: red">第五个div</div>
</body>
</html>
```

- HTML的特殊字符

像< > " ' 空格 &一类的符号都是特殊字符，我们在HTML中无法直接用其本身来表示他们，或者表示完全，因此我们要学习使用等价字符来进行引用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE564566763867255dc6a86eea298f29f8.png)

最后我们可以写入一个简单的测试代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>特殊字符</title>
</head>
<body>
    本网站有     最终解释权 <br/><!--可以看到虽然打入了五个空格，但实际只显示一个-->
    本网站有&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最终解释权
</body>
</html>
```

