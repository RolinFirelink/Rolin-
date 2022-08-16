先来看看我们的登录页面的效果

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE11b7e814a987ce15e68f5e5269d88029.png)

不得不说这个比上一次的几把登录页面舒服太多了，同样的我们先来进行必备知识的学习

我们先来看看我们的表格标签的知识介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0d8941da8f16b4ed96f42538119ac1c6.png)

首先我们的table标签作用是设置一个表格，这个标签具有设置宽度高度边框样式和对齐方式

其次是tr标签，突然标签是行标签，这个行标签可以设置对齐方式

接着是td标签，td标签是单元格标签，也就是列标签，其下有两个属性，可以用于合并行或者是合并列

然后是th标签，其实表头标签，本身就具有加粗和居中的效果

最后是thead、tbody、tfoot这三个标签，分别是表头语义标签、标体语义标签、表脚语义标签，这三个标签的简单来说就是我们在表格标签里将这三个标签再分别放置于表格的头部，身部，脚部的位置，不过只作为了解就可以了，因为即使我们不加这个，我们的代码也不会有什么问题

假设我们要完成如下效果的表格

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbf62497a74daf48ff59cab3126ee6882.png)

那么我们可以构造演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表格标签演示</title>
</head>
<body>
    <!--
        表格标签：<table>
        属性：
            width-宽度
            height-高度
            border-边框
            align-对齐方式

            行标签：<tr>
            属性：
                align-对齐方式

            单元格标签：<td>
            属性
               rowspan-合并行
               colspan-合并列

            表头标签：<th>自带居中和加粗效果

            表头语义标签：<thead>
            表体语义标签：<tbody>
            表脚语义标签：<tfoot>
    -->
    <table width="400px" border="1px" align="center">
        <thead>
            <tr>
                <th>姓名</th>
                <th>性别</th>
                <th>年龄</th>
                <th>数学</th>
                <th>语文</th>
            </tr>
        </thead>

        <tbody>
            <tr align="center">
                <td>张三</td>
                <td rowspan="2">男</td>
                <td>23</td>
                <td colspan="2">90</td>
                <!--因为合并列，因此省略该语句 <td>90</td>-->
            </tr>

            <tr align=" center">
                <td>李四</td>
                <!--因为合并行，因此省略该语句 <td>男</td>-->
                <td>24</td>
                <td>95</td>
                <td>90</td>
            </tr>
        </tbody>

        <tfoot>
            <tr>
                <td colspan="4">总分数:</td>
                <td>373</td>
            </tr>
        </tfoot>
    </table>

</body>
</html>
```

接着我们来学习样式控制，先来看看知识列表

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc952e7681d95711d38e10fe567d024e3.png)

首先是background-repeat样式名称是用于确定背景图片是否重复以及是上下重复还是左右重复的，默认是全重复的，这个其实不难理解，不多讲了

接着是outline样式，该样式用于控制输入框的轮廓，常与输入框的轮廓结合在一起使用，其下有四个属性，和我们之前讲过的边框样式非常相似

最后是display样式，该样式可以控制元素的属性，将其转换为内联元素或者是块级元素

根据上文我们可以写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>样式演示</title>
    <style>
        /*背景图片重复 no-repeat：不重复 repeat-x：水平重复 repeat-y：垂直重复 repeat：水平+垂直重复*/
        body{
            background: url("img/snow.gif");

            background-repeat: repeat;
        }

        /*轮廓控制 double：双实线 dotted：圆点 dashed：虚线 none：无*/
        input{
            outline: none;
        }
        /*元素显示 inline：内联元素(无换行、无长宽)  block：块级元素(有换行)  inline-block：内联元素(有长宽) none：隐藏元素*/
        div{
            color: white;
            display: inline-block;
            width: 400px;
        }

    </style>
</head>
<body>
    用户名：<input type="text">

    <div>春季</div>
    <div>夏季</div>
    <div>秋季</div>
    <div>冬季</div>
</body>
</html>
```

然后我们来讲CSS中的布局的一个重中之重的点，盒子模型。我们在java里曾经学习过，万物皆可看作对象，那么在CSS里，就是万物皆可看作盒子。先来看下面的例子

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1def873a3b9bcfa7aa8a0e533f0dd2bd.png)

首先盒子模型是通过设置边框和元素内容的边距来实现布局的，分为内边距和外边距两种方式，如果我们的视角是放在外面的大的红盒子，那么我们设置的就是内边距，如果我们的视角是放在里面的小的蓝盒子，那么我们设置的就是外边距。一般我们都采用设置外边距的方式来实现布局，因为内边距的方式不但不符合人类直觉，而且会导致外框发生变化，不方便

下面是外边距的方法图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf9d79e1d15ae7fc767618544eaf215f4.png)

可以看到外边距里我们可以手动设置各个不同方向的边距，也可以只设置一个，其会默认该距离用于四个方向，还可以直接赋予四个距离，但这个方法要注意的是其方向的赋值方式是上右下左，最后是auto方法，该方法会自动计算外边距并居中

然后是内边距的方法图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa1179ac53f64bf4a01c724605f3356c7.png)

这个其实和外边距的差不多，这里就不赘述了

最后我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>盒子模型演示</title>
    <style>
        .wai{
            border: 1px solid red;
            width: 200px;
            height: 200px;
            /*设置内边距*/
            padding: 50px;
        }

        .nei{
            border: 1px solid blue;
            width: 100px;
            height: 100px;

            /*设置外边距的第一种方式
            margin-top: 50px;
            margin-left: 50px;
            margin-right: 50px;
            margin-bottom: 50px;*/

            /*设置外边距的第二种方式
            margin:50px;*/

            /*上、右、下、左*/
            /*margin: 70px 35px 30px 65px;*/
        }
    </style>
</head>
<body>
    <div class="wai">
        <div class="nei">

        </div>
    </div>
</body>
</html>
```

那么学习完了上面的知识之后，我们现在就可以来实现我们的登录页面案例了，同样的，我们也是编写独立的css文件和HTML文件来完成页面的构造，我们先构造顶部和中间表单，先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE31da98f32652100137055319dbb90979.png)

有的同学可能有疑问，这不对吧？我们的案例里没有表格啊，其实是有的，就是我们的登录界面，这个表格分有四行两列，只是边框隐藏了而已，而且还有一些合并行列，比如第一行的账密登录就是合并了行

接着我们来看看中间表单样式的实现步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE57806d5c39b5195c55aa460528fd83cc.png)

最后我们来看看底部页脚的实现步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE950a3e21ed28763124d29c2f64822473.png)

最后我们可以写出演示的css文件如下

```
/*背景图片*/
body{
    background: url("../img/bg.png");
}

/*中间表单样式*/
.center{
    background: white;  /*背景色*/
    width: 40%;         /*宽度*/
    margin: auto;       /*水平居中外边距*/
    margin-top: 100px;  /*上外边距*/
    border-radius: 15px;/*边框弧度*/
    text-align: center;
}

/*表头样式*/
thead th{
    font-size: 30px; /*字体大小*/
    color: orangered; /*字体颜色*/

}

/*表体提示信息样式*/
tbody label{
    font-size: 20px; /*字体大小*/
}

/*表体输入框*/
tbody input{
    border: 1px solid gray; /*边框*/
    border-radius: 5px; /*边框弧度*/
    width: 90%; /*输入框的宽度*/
    height: 40px;/*输入框的高度*/
    outline: none;/*取消轮廓的样式*/
}

/*表底确定按钮的标签*/
tfoot button{
    border: 1px solid crimson; /*边框*/
    border-radius: 5px; /*边框弧度*/
    width: 95%; /*宽度*/
    height: 40px; /*高度*/
    background: crimson; /*背景色*/
    color: white; /*文字颜色*/
    font-size: 20px; /*字体大小*/
}

/*表行高度*/
tr{
    line-height: 60px;
}

/*底部页脚样式*/
.footer{
    width: 35%; /*宽度*/
    margin: auto; /*水平居中*/
    font-size: 15px; /*字体大小*/
    color: gray      /*字体颜色*/
}

/*超链接样式*/
a{
    text-decoration: none;
    color: blue;
}
```

演示的HTML文件的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录页面</title>
    <link rel="stylesheet" href="css/01.css">
</head>
<body>
    <!--顶部公司图标-->
    <div>
        <img src="img/logo.png"/>
    </div>

    <!--中间表单-->
    <div class="center">
        <form action="#" method="get" autocomplete="off">
            <table width="100%">
                <thead>
                    <tr>
                        <th colspan="2">账&nbsp;密&nbsp;登&nbsp;录<hr/></th>
                    </tr>
                </thead>

                <tbody>
                    <tr>
                        <td>
                            <label for="username">账号</label>
                        </td>
                        <td>
                            <input type="text" id="username" name="username" placeholder="请输入账号" required/>
                        </td>
                    </tr>
                    <tr>
                        <td>
                            <label for="password">密码</label>
                        </td>
                        <td>
                            <input type="text" id="password" name="password" placeholder="请输入密码" required/>
                        </td>
                    </tr>
                </tbody>

                <tfoot>
                    <tr>
                        <td colspan="2">
                            <button type="submit">确&nbsp;定</button>
                        </td>
                    </tr>
                </tfoot>
            </table>
        </form>
    </div>

    <!--底部页脚-->
    <div class="footer">
        <br/><br/>
        登录/注册即表示您统一&nbsp;&nbsp;
        <a href="#" target="_blank">用户协议</a>&nbsp;&nbsp;
        和&nbsp;&nbsp;
        <a href="#" target="_blank">隐私条款</a>&nbsp;&nbsp;&nbsp;&nbsp;
        <a href="#" target="_blank">忘记密码</a>
    </div>
</body>
</html>
```

