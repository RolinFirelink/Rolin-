# JavaScript快速入门

本章我们来学习JavaScript，由于这是前端的内容，这里我们只做了解，截图就完了，后续有需要再来重新学吧，突出一个快

##  JavaScript的介绍

什么是JavaScript

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5ad7007381cedd0c2ff731ef129b8c4c.png)

## JavaScript的历史

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE324faccd83e46ece9734cea5f8efc86b.png)

关于JavaScript的组成部分，其分为三部分，分别是ECMAScript、DOM、BOM。我们之后的内容也要围绕这三个部分学习

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2f153848c71a0f074fd0573324c064d2.png)

## js的引入方式

接下来我们通过一个案例来理解JavaScript的作用，只是案例，原理先不做解释

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEae4286aeba4e7f489a42281e7b3f1774.png)

创建一个快速入门的html文件，使用script标签可以引入js，并令浏览器提示对应的输入框或警告框，这是第一种引入方式，称之为内部方式

第二种引入方式是通过外部文件的方式，将代码写在js文件类型的外部文件中，然后在srcript标签中的src属性下写入对应的地址，就可以实现外部文件的引入，其好处是写一个外部文件就可以让多个html文件使用这套配置

## 工具的安装

用idea无法实现实时刷新的功能，非常麻烦，我们这里使用VSCode进行编写，安装步骤有兴趣自己看吧，这里就不带一遍了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe6d7bfca890501e0c5526f1b18230c77.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8c8ea422bc29749efa2cf8a444cf9d61.png)

# JavaScript基本语法

接着我们来学习JavaScript的基本语法

## 注释

我们先来学习注释方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE405be3824583ed42bd30c468a4863c46.png)

这个就不演示了，大家都懂得的。这里值得一提的是VSCode并不会自动保存，因此我们要自己保存，多按ctrl+s，同时其默认保存的文件类型为txt文件，别忘了改成html格式的文件。还有一点是在VSC中创建的文件一开始不会有固定格式的前置内容，我们要输入!之后敲入回车就有了

其演示代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE28405bcab26853acb941f17aa2495cac.png)

## 输入输出语句

输入框会在浏览器上弹出弹窗并要求用户输入内容，而警告窗则弹出窗口并提示用户信息，控制台输出内容会在浏览器上的开发者工具中输入内容，而页面输出内容则是真正在浏览器输入内容的语句。另外输入两个语句不会自动换行，要手动输入br才可以换行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbbf7798edbff0f61d2f920cf06192de9.png)

其演示代码如下（这里省略了头和body的部分，直接看写在body后的script的代码）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcab83cf0ff0472d02291c3f6eb4cf5da.png)

## 变量和常量

我们先来看看演示的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5c16242165e629db1aa094462f05f267.png)

再来看看演示代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE30363781ddedba5305fa91c8e0f7315e.png)

注意，如果我们的代码中间报错了，那么后续代码就不会执行，比如没有初始化的值无法被打印，报错的内容会显示到控制台，我们可以通过开发者工具进入控制台查看到报错信息，如果我们希望其正确显示，那么就应该解决这个报错内容，比如直接去掉发生错误的代码

## 原始数据类型和typeof

虽然说在JavaScript中的数据类型是弱类型，我们可以用let随便定义，但实际上其仍然还是有原始数据类型的，在JS中其原始数据类型一共有这么几种

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc7b3398b8fb0758694cd4f3d30504f5a.png)

如果我们想要查看其原始数据类型，需要用到typeof

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE710e09701f1e7a76687bfe094f5305c4.png)

我们可以写入演示代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8d6be571308975839c8340cd7e8a7155.png)

最后我们得到的结果和我们预想中的一样，但是null类型的数据类型会被显示为object，这其实是js原始的一种错误，反正我们记住null会被认为是object就完了

## 运算符

首先是简单的算数运算符，其和我们在java中学习的大差不差

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9c43b49bfc0aebda3bac0da33ab26a3b.png)

然后是赋值运算符，照样是几乎一模一样

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE089515d382a8780fe9f2e99f416cd62f.png)

关于比较运算符，在JS中==号判断值是否相等，而===是判断数据类型和值是否相等，其会先判断数据类型是否相同，不同直接返回false，若相同再进行==的判断。其他的差不多，就不提了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE22caf3f7f9993dd143ca143eb5e8f566.png)

然后是逻辑运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb480c95cc24fad5f03303700347e3d80.png)

最后是三元运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE58b8253e11d2cbb088055f333df7a567.png)

最后我们来说一下一些需要注意的点，具体请看代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE460c4bb5535f54dda3282deb84788b3d.png)

可以看到在我们的代码中，进行运算时其也会进行自动类型转换，如果我们是+号，那么其自动类型转换会优先进行字符串的拼接，而如果是其他的，如-*/，那么会优先进行数字类型的运算并进行打印。而在JS中，两个==号，只会比较其具体的值而不会比较其数据类型，而===号会先比较数据类型，后面再比较数据内容，当然如果数据类型都不匹配那就直接返回false了

## 流程控制语句和循环语句

先来看看具体有哪些内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5cb91b7bda7a26eeb6c6aea7abdfc61a.png)

其实和java中的差不多，这里直接看代码吧

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>流程控制和循环语句</title>
</head>
<body>

</body>
<script>
//if语句
    let month = 3;
            if(month >= 3 && month <= 5) {
            document.write("春季");
            }else if(month >= 6 && month <= 8) {
            document.write("夏季");
            }else if(month >= 9 && month <= 11) {
            document.write("秋季");
            }else if(month == 12 || month == 1 || month == 2) {
            document.write("冬季");
            }else {
            document.write("月份有误");
            }

            document.write("<br>");

            //switch语句
            switch(month){
            case 3:
            case 4:
            case 5:
            document.write("春季");
            break;
            case 6:
            case 7:
            case 8:
            document.write("夏季");
            break;
            case 9:
            case 10:
            case 11:
            document.write("秋季");
            break;
            case 12:
            case 1:
            case 2:
            document.write("冬季");
            break;
default:
        document.write("月份有误");
        break;
        }

        document.write("<br>");

        //for循环
        for(let i = 1; i <= 5; i++) {
        document.write(i + "<br>");
        }

        //while循环
        let n = 6;
        while(n <= 10) {
        document.write(n + "<br>");
        n++;
        }
</script>
</html>
```

## 数组

在JS中的数组和java中的基本一样，但是在JS中其数据类型和长度都没有限制，其他的差不多。但是值得一提的是，其有高级运算符...，可以用于数组赋值，合并数组以及字符串转数组

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE94ff665d431c12c995b71d0246044eed.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数组</title>
</head>
<body>

</body>
<script>
    //定义数组
    let arr = [10,20,30];
    //arr[3] = 40;  js中的数组长度可变
    //遍历数组
    for(let i = 0; i < arr.length; i++) {
        document.write(arr[i] + "<br>");
    }
    document.write("==============<br>");

    // 数组高级运算符 ...
    //复制数组
    let arr2 = [...arr];
    //遍历数组
    for(let i = 0; i < arr2.length; i++) {
        document.write(arr2[i] + "<br>");
    }
    document.write("==============<br>");

    //合并数组
    let arr3 = [40,50,60];
    let arr4 = [...arr2 , ...arr3];
    //遍历数组
    for(let i = 0; i < arr4.length; i++) {
        document.write(arr4[i] + "<br>");
    }
    document.write("==============<br>");

    //将字符串转成数组
    let arr5 = [..."heima"];
    //遍历数组
    for(let i = 0; i < arr5.length; i++) {
        document.write(arr5[i] + "<br>");
    }
</script>
</html>
```

我们这里运用...高级运算符做了合并，赋值，以及将字符串转换为数组，如果我们想要复制数组到另外一个数组，直接采用let 数组名=[...数组名]的格式就行了，如果我们想要合并数组的话，那么我们输入的内容是let 数组名 = [...数组名,...数组名]，其实就相当于把两个数组的元素都复制过来罢了。最后是将字符串转换为数组，其格式是let arr = [...字符串]，其会自动将字符串转换为字符数组

## 函数

函数其实就类似于java中的方法，其定义的方式是function 方法名(参数列表){}，内部要有return和返回值，如果我们希望我们的参数能动态传入的话，那就写入...参数名到括号中，我们也可以不写入方法名，这样我们的函数就成了匿名函数

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE03172161b7a9c4da4811562436e2573f.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>函数</title>
</head>
<body>

</body>
<script>
    //无参无返回值的方法
    function println(){
        document.write("hello js" + "<br>");
    }
    //调用方法
    println();

    //有参有返回值的方法
    function getSum(num1,num2){
        return num1 + num2;
    }
    //调用方法
    let result = getSum(10,20);
    document.write(result + "<br>");

    //可变参数  对n个数字进行求和
    function getSum(...params) {
        let sum = 0;
        for(let i = 0; i < params.length; i++) {
            sum += params[i];
        }
        return sum;
    }
    //调用方法
    let sum = getSum(10,20,30);
    document.write(sum + "<br>");

    //匿名函数
    let fun = function(){
        document.write("hello");
    }
    fun();
</script>
</html>
```

返回值可以加也可以不加，函数中不需要写入返回值，接受可变参数为数组可以让我们完成n项求和运算，最后匿名函数是要赋值到我们自己定义的fun中，然后调用的时候又要用fun()，显然意义不大，其实匿名函数一般也是配合我们后期学习的事件来使用的，我们现在暂时了解下就可以了

基本语法的小结

最后我们可以做一个本章的小结，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9c73d79c6889c62810327cfb43eac194.png)

# DOM

那么现在我们正式来学习JS里的第一个内容，DOM，首先我们来说明下什么是DOM

## DOM的介绍

现在我们正式来介绍下我们的DOM，我们的DOM简而言之就是将我们的标签再内存中转为一个个对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeb293753d27df60e742e5d7462c0e759.png)

比如左边的HTML文件，那么其在内存中所对应创建出来的就会是如右边所示的树形结构，可以看到我们先创建文档对象，该对象下有许多元素，元素就是那些标签，然后某些元素下会有属性对象，然后最后他们基本都是要连接到一个文本对象的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1a750ed2d170d004413f41fbc4b24026.png)

## 元素的获取操作

我们先来学习元素对象Element，其下有五个方法，分别可以根据id、标签名称、name属性值、class属性值以及其子元素对象的某个属性来获取对应的结果，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf4bfc003ff9948cc5f286b71b11f7625.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>元素的获取</title>
</head>
<body>
<div id="div1">div1</div>
<div id="div2">div2</div>
<div class="cls">div3</div>
<div class="cls">div4</div>
<input type="text" name="username"/>
</body>
<script>
    //1. getElementById()   根据id属性值获取元素对象
    let div1 = document.getElementById("div1");
    //alert(div1);  //一个对象，不是数字形式

    //2. getElementsByTagName()   根据元素名称获取元素对象们，返回数组
    let divs = document.getElementsByTagName("div");
    //alert(divs.length);  //四个对象

    //3. getElementsByClassName()  根据class属性值获取元素对象们，返回数组
    let cls = document.getElementsByClassName("cls");
    //alert(cls.length);  //两个对象

    //4. getElementsByName()   根据name属性值获取元素对象们，返回数组
    let username = document.getElementsByName("username");
    //alert(username.length);  //一个对象，但仍然是数组形式

    //5. 子元素对象.parentElement属性   获取当前元素的父元素
    let body = div1.parentElement; //一个对象，不是数字形式，是该对象是body元素对象
    alert(body);
</script>
</html>
```

## 元素的增删改操作

元素还有增删改操作的对应方法，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE84105af3c50364d2ee497460840dc84c.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>元素的增删改</title>
</head>
<body>
<select id="s">
    <option>---请选择---</option>
    <option>北京</option>
    <option>上海</option>
    <option>广州</option>
</select>
</body>
<script>
    //1. createElement()   创建新的元素
    let option = document.createElement("option");
    //为option添加文本内容
    option.innerText = "深圳";

    //2. appendChild()     将子元素添加到父元素中
    let select = document.getElementById("s");
    select.appendChild(option);

    //3. removeChild()     通过父元素删除子元素
    //select.removeChild(option);

    //4. replaceChild()    用新元素替换老元素
    let option2 = document.createElement("option");
    option2.innerText = "杭州";
    select.replaceChild(option2,option);

</script>
</html>
```

我们这里要注意的是创建新元素的步骤，以及不要忘了给新元素赋值。特别要记忆的一点是，对某个标签进行的增删改都是需要获取其父标签然后调用父标签里对应的方法才能实现对子标签的修改的，否则是行不通的

## 属性的操作

接着我们来演示Attribute属性的操作，我们可以设置属性，通过属性名获取或者是移除指定的属性，最后还有一个方法是可以通过属性为元素添加样式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d2d006456d6439d2e1dfe252cf629ac.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>属性的操作</title>
    <style>
        .aColor{
            color: blue;
        }
    </style>
</head>
<body>
<a>点我呀</a>
</body>
<script>
    //1. setAttribute()    添加属性
    let a = document.getElementsByTagName("a")[0];
    a.setAttribute("href","https://www.baidu.com");

    //2. getAttribute()    获取属性
    let value = a.getAttribute("href");
    //alert(value);

    //3. removeAttribute()  删除属性
    //a.removeAttribute("href");

    //4. style属性   添加样式
    //a.style.color = "red";

    //5. className属性   添加指定样式
    a.className = "aColor";

</script>
</html>
```

我们这里可以通过JSP来删除和获取和删除属性，值得一提的是，添加样式是我们不但可以手动添加，我们还可以指定样式添加，换言之，我们可以先写入一个固定的样式，然后直接引入这个样式就可以将对应的样式添加进去了

## 文本的操作

最后我们来讲解文本内的操作和其对应的方法，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3129ade46edf3e84a62d018b8fdc08fb.png)

innerText只添加文本内容不解析标签，而innerHTML则可以，简单来说，如果我们希望我们写入的文本里含有加粗等一类的效果的话，我们必然是要往里面写入对应标签的，此时如果我们希望这些标签能正常作用，此时我们就需要使用innerHTML属性，而不是innerText，因为其会将该标签也一并打印，而不解析

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>文本的操作</title>
</head>
<body>
<div id="div"></div>
</body>
<script>
    //1. innerText   添加文本内容，不解析标签
    let div = document.getElementById("div");
    div.innerText = "我是div";
    //div.innerText = "<b>我是div</b>";

    //2. innerHTML   添加文本内容，解析标签
    div.innerHTML = "<b>我是div</b>";

</script>
</html>
```

DOM的小结

最后我们可以对DOM做一个总结，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcd653535bb2e0b714a0ff219154c02db.png)

然后来看看属性的总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE40ef05150cfd0290f51c0c901046c6ac.png)

# 事件

本章节我们来学习事件，先来进行事件的介绍

## 事件的介绍

所谓事件指的是执行某些操作之后会触发某些代码的执行，比如我们看图点击下一页可以触发代码让我们的图片进入到下一页

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc507a747df66df901657f1ed2712a025.png)

常用的事件如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe4e63e2dc73f5326e0ea4ed326129cbf.png)

还有一些事件用于了解，这些事件我们了解下就好了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc379a6327599ea0baaec5184998224a0.png)

## 事件的操作

我们接下来学习事件的绑定操作，有两种方式，一种通过标签中的事件属性进行绑定，另一种则是通过DOM元素属性绑定。我们平时推荐使用第二种方式，这样我们的耦合度更低

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0ab9cfbe220bc2665dd4b664c691b494.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件</title>
</head>
<body>
<img id="img" src="img/01.png"/>
<br>
<!-- <button id="up" onclick="up()">上一张</button> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<button id="down" onclick="down()">下一张</button> -->
<button id="up">上一张</button> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<button id="down">下一张</button>
</body>
<script>
    //显示第一张图片的方法
    function up(){
        let img = document.getElementById("img");
        img.setAttribute("src","img/01.png");
    }

    //显示第二张图片的方法
    function down(){
        let img = document.getElementById("img");
        img.setAttribute("src","img/02.png");
    }

    //为上一张按钮绑定单击事件
    let upBtn = document.getElementById("up");
    upBtn.onclick = up;

    //为下一张按钮绑定单击事件
    let downBtn = document.getElementById("down");
    downBtn.onclick = down;
</script>
</html>
```

第一种绑定的方式是通过属性绑定的，我们要在对应的按钮上设置id，并设置上对应的点击事件，我们这里采用两个函数来实现切换操作，这是一种取巧的方式，但这里为了方便就先这么做吧。第二种方式是先获取按钮对象，然后调用该对象的onclick属性，指定我们想调用的函数，这样只要用户一点击按钮，就会立刻触发对应的事件了。其他常用的事件的实现思路也大概如此，我们依葫芦画瓢就行了

事件小结

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE754d1329cf787593f0c6b07d38c2881f.png)

# 综合案例

## 案例效果的介绍

先来看看我们的案例效果，我们这里希望我们添加的时候输入相应信息然后点击添加就能立刻得到我们添加之后的列表，然后我们希望我们点击删除的时候也能立刻删除并且展示删除之后的列表

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe0a9a1be5ed5ddc195efc6c4f29769b9.png)

## 添加功能

我们对我们的添加功能进行分析，我们要如何完成我们的添加功能呢？其实很简单，我们添加的第一步是先创建一行，然后将一行分为四列，接着获得四列对应的数据将其填充进去，接着再将这四列数据组成的新的一行添加到我们原来的地方上去。

先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE401381fd335eb48990095910d06f03ce.png)

然后我们可以构造代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动态表格</title>

    <style>
        table{
            border: 1px solid;
            margin: auto;
            width: 500px;
        }

        td,th{
            text-align: center;
            border: 1px solid;
        }
        div{
            text-align: center;
            margin: 50px;
        }
    </style>

</head>
<body>

<div>
    <input type="text" id="name" placeholder="请输入姓名" autocomplete="off">
    <input type="text" id="age"  placeholder="请输入年龄" autocomplete="off">
    <input type="text" id="gender"  placeholder="请输入性别" autocomplete="off">
    <input type="button" value="添加" id="add">
</div>

<table id="tb">
    <caption>学生信息表</caption>
    <tr>
        <th>姓名</th>
        <th>年龄</th>
        <th>性别</th>
        <th>操作</th>
    </tr>

    <tr>
        <td>张三</td>
        <td>23</td>
        <td>男</td>
        <td><a href="JavaScript:void(0);" onclick="drop(this)">删除</a></td>
    </tr>

    <tr>
        <td>李四</td>
        <td>24</td>
        <td>男</td>
        <td><a href="JavaScript:void(0);" onclick="drop(this)">删除</a></td>
    </tr>

</table>

</body>
<script>
    //一、添加功能
    //1.为添加按钮绑定单击事件
    document.getElementById("add").onclick = function(){
        //2.创建行元素
        let tr = document.createElement("tr");
        //3.创建4个单元格元素
        let nameTd = document.createElement("td");
        let ageTd = document.createElement("td");
        let genderTd = document.createElement("td");
        let deleteTd = document.createElement("td");
        //4.将td添加到tr中
        tr.appendChild(nameTd);
        tr.appendChild(ageTd);
        tr.appendChild(genderTd);
        tr.appendChild(deleteTd);
        //5.获取输入框的文本信息
        let name = document.getElementById("name").value;
        let age = document.getElementById("age").value;
        let gender = document.getElementById("gender").value;
        //6.根据获取到的信息创建3个文本元素
        let nameText = document.createTextNode(name);
        let ageText = document.createTextNode(age);
        let genderText = document.createTextNode(gender);
        //7.将3个文本元素添加到td中
        nameTd.appendChild(nameText);
        ageTd.appendChild(ageText);
        genderTd.appendChild(genderText);
        //8.创建超链接元素和显示的文本以及添加href属性
        let a = document.createElement("a");
        let aText = document.createTextNode("删除");
        a.setAttribute("href","JavaScript:void(0);");
        a.setAttribute("onclick","drop(this)");
        a.appendChild(aText);
        //9.将超链接元素添加到td中
        deleteTd.appendChild(a);
        //10.获取table元素，将tr添加到table中
        let table = document.getElementById("tb");
        table.appendChild(tr);
    }

    //二、删除的功能
    //1.为每个删除超链接标签添加单击事件的属性
    //2.定义删除的方法
    function drop(obj){
        //3.获取table元素
        let table = obj.parentElement.parentElement.parentElement;
        //4.获取tr元素
        let tr = obj.parentElement.parentElement;
        //5.通过table删除tr
        table.removeChild(tr);
    }


</script>
</html>
```

对应的步骤都写在注释上了，这里就不再解释了，如果真的看不懂的话，可以回去看视频，因为这里也只是做一个了解，所以回顾的时候看不太懂也正常。这里值得一提的是我们的超链接href属性的内容是JavaScript:void(0);，其代表点击超链接之后不会刷新页面且不会递交数据，我们之前写入的是#，虽然其有不能提交数据的效果，但是其会刷新页面，这不是我们想要的，因此我们这里使用JavaScript:void(0);来代替#

## 删除功能

本来这里应该是还有的，但是向学长告诉我这些甚至都可以不学，那我们就直接跳过了，不跟他浪费时间。但是最后我们又回来了，因为我们发现后面的内容还是会需要用到这里的内容，所以我们还是迅速过一遍吧。

首先我们来进行我们的删除功能的分析，首先我们要知道，对于JS内部的元素而言，其是不能自己删除自己的，只能通过其父元素完成对自己的删除。而且我们执行删除并不是删除一个单元格，而是对应的那一个列

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEff9c28ab359641240f22937b0d126af5.png)

那么当我们点击删除时，我们首先通过该元素寻找到其对应的单元格，然后通过单元格寻找到单元行，通过单元行（也就是对应的列）找到我们的整个表，最后通过表来删除我们的对应的列，这就是我们的整个分析过程了

- 删除功能实现

先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1870fda0d9024c7d536c8a8aa4e8e37d.png)

那么首先我们给我们的删除的超链接提供单机事件属性，我们令其单机就会调用drop函数，并传入其自身所代表的单元格的对象给这个方法，这里的this就是代表其自身的单元格对象

```
<table id="tb">
    <caption>学生信息表</caption>
    <tr>
        <th>姓名</th>
        <th>年龄</th>
        <th>性别</th>
        <th>操作</th>
    </tr>

    <tr>
        <td>张三</td>
        <td>23</td>
        <td>男</td>
        <td><a href="JavaScript:void(0);" onclick="drop(this)">删除</a></td>
    </tr>

    <tr>
        <td>李四</td>
        <td>24</td>
        <td>男</td>
        <td><a href="JavaScript:void(0);" onclick="drop(this)">删除</a></td>
    </tr>

</table>
```

同时我们自己的添加的链接也需要令其拥有触发对应事件的功能，因此我们要给我们自己添加的行设定对应的属性

```
//8.创建超链接元素和显示的文本以及添加href属性
let a = document.createElement("a");
let aText = document.createTextNode("删除");
a.setAttribute("href","JavaScript:void(0);");
a.setAttribute("onclick","drop(this)");
a.appendChild(aText);
```

最后就是我们的删除功能的代码，我们这里传入的单元格对象赋予给了obj，其调用三次获取父类的方法可以获得对应的table表对象，然后又通过两次调用父类的方法获取对应的列，通过表对象实现移除对应的列

```
//二、删除的功能
//1.为每个删除超链接标签添加单击事件的属性
//2.定义删除的方法
function drop(obj){
    //3.获取table元素
    let table = obj.parentElement.parentElement.parentElement;
    //4.获取tr元素
    let tr = obj.parentElement.parentElement;
    //5.通过table删除tr
    table.removeChild(tr);
}
```

# 面向对象

到了本章我们就来学习JS的高级内容，同样我们还是快速过一遍。

## 类的定义和使用

我们在java中，我们学习过如何定义类以及类里的方法，同时还有面向对象的思想，那么在JS中也是同样存在的，我们本节就先来学习在JS中类的定义和使用，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE27fd61207363492811cb94b9b767cf56.png)

那么我们可以构造其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>类的定义和使用</title>
</head>
<body>

</body>
<script>
    //定义Person类
    class Person{
        //构造方法
        constructor(name,age){
            this.name = name;
            this.age = age;
        }

        //show方法
        show(){
            document.write(this.name + "," + this.age + "<br>");
        }

        //eat方法
        eat(){
            document.write("吃饭...");
        }
    }

    //使用Person类
    let p = new Person("张三",23);
    p.show();
    p.eat();
</script>
</html>
```

值得一提的是，我们的类中，其对应的方法不再需要加function关键词

## 字面量类的定义和使用

我们接着来学习字面量定义类和使用的方法，我们之前定义类是通过类的定义来定义的，提供了构造方法，如果我们要使用该类的话，就需要先new出来，而我们下面的则不需要，因为其定义时就相当于已经创建了该对象了。其调用格式如图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8b22444c538123af6082bbac64745894.png)

然后我们可以写入其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>字面量定义类和使用</title>
</head>
<body>

</body>
<script>
    //定义person
    let person = {
        name : "张三",
        age : 23,
        hobby : ["听课","学习"],

        eat : function() {
            document.write("吃饭...");
        }
    };

    //使用person
    document.write(person.name + "," + person.age + "," + person.hobby[0] + "," + person.hobby[1] + "<br>");
    person.eat();
</script>
</html>
```

## 继承

接下来我们来学习继承，具体看图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6c9920bdc47f135460b9b1e41e0e808f.png)

其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>继承</title>
</head>
<body>

</body>
<script>
    //定义Person类
    class Person{
        //构造方法
        constructor(name,age){
            this.name = name;
            this.age = age;
        }

        //eat方法
        eat(){
            document.write("吃饭...");
        }
    }

    //定义Worker类继承Person
    class Worker extends Person{
        constructor(name,age,salary){
            super(name,age);
            this.salary = salary;
        }

        show(){
            document.write(this.name + "," + this.age + "," + this.salary + "<br>");
        }
    }

    //使用Worker
    let w = new Worker("张三",23,10000);
    w.show();
    w.eat();
</script>
</html>
```

面向对象小结

最后我们可以来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE77681c628ed7b2b37374791202f03293.png)

# 内置对象

本章节我们来学习内置对象，其实内置对象我们可以简单理解为内部内置好的给我们的使用的方法

## Number

首先我们来介绍Number，其具有的方法如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9520e41d2915e12cfda4ee826f2537c8.png)

那么我们可以构造其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Number</title>
</head>
<body>

</body>
<script>
    //1. parseFloat()  将传入的字符串浮点数转为浮点数
    document.write(Number.parseFloat("3.14") + "<br>");

    //2. parseInt()    将传入的字符串整数转为整数
    document.write(Number.parseInt("100") + "<br>");
    document.write(Number.parseInt("200abc") + "<br>"); // 从数字开始转换，直到不是数字为止
    document.write(Number.parseInt("abc200") + "<br>"); // 从数字开始转换，直到不是数字为止

</script>
</html>
```

这里值得一提的是，我们厘米的ParseInt方法是从数字开始转换，直到不是数字位置，也就是说即使我们传入了完全不是数字的字符串，也是不会报错的，最多为空罢了。

## Math

接着我们来学习Math对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb3c1c51f413988efe9bfebcb0d65a4ab.png)

其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Math</title>
</head>
<body>

</body>
<script>
    //1. ceil(x) 向上取整
    document.write(Math.ceil(4.4) + "<br>");    // 5

    //2. floor(x) 向下取整
    document.write(Math.floor(4.4) + "<br>");   // 4

    //3. round(x) 把数四舍五入为最接近的整数
    document.write(Math.round(4.1) + "<br>");   // 4
    document.write(Math.round(4.6) + "<br>");   // 5

    //4. random() 随机数,返回的是0.0-1.0之间范围(含头不含尾)
    document.write(Math.random() + "<br>"); // 随机数

    //5. pow(x,y) 幂运算 x的y次方
    document.write(Math.pow(2,3) + "<br>"); // 8
</script>
</html>
```

## Date

先开看看其构造方法，注意我们这里的月份是0~11，即我们的1月是0月，2月是1月，最大的12月要填11

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6857fc55211bef12814c63991f01b338.png)

然后是该对象内部的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE39f007abae3f6c6b0939cb0eb6ceabe9.png)

然后我们可以构造其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Date</title>
</head>
<body>

</body>
<script>
    //构造方法
    //1. Date()  根据当前时间创建对象
    let d1 = new Date();
    document.write(d1 + "<br>");

    //2. Date(value) 根据指定毫秒值创建对象
    let d2 = new Date(10000);
    document.write(d2 + "<br>");

    //3. Date(year,month,[day,hours,minutes,seconds,milliseconds]) 根据指定字段创建对象(月份是0~11)
    let d3 = new Date(2222,2,2,20,20,20);
    document.write(d3 + "<br>");

    //成员方法
    //1. getFullYear() 获取年份
    document.write(d3.getFullYear() + "<br>");

    //2. getMonth() 获取月份
    document.write(d3.getMonth() + "<br>");

    //3. getDate() 获取天数
    document.write(d3.getDate() + "<br>");

    //4. toLocaleString() 返回本地日期格式的字符串
    document.write(d3.toLocaleString());
</script>
</html>
```

## String

首先是构造方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE892ebf37819afcb1657ce85f72fb3bc6.png)

然后是成员方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE70c28331b400fc13b2c09d378b33b4c4.png)

然后我们可以写入其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>String</title>
</head>
<body>

</body>
<script>
    //1. 构造方法创建字符串对象
    let s1 = new String("hello");
    document.write(s1 + "<br>");

    //2. 直接赋值
    let s2 = "hello";
    document.write(s2 + "<br>");

    //属性
    //1. length   获取字符串的长度
    document.write(s2.length + "<br>");

    //成员方法
    //1. charAt(index)     获取指定索引处的字符
    document.write(s2.charAt(1) + "<br>");

    //2. indexOf(value)    获取指定字符串出现的索引位置
    document.write(s2.indexOf("l") + "<br>");

    //3. substring(start,end)   根据指定索引范围截取字符串(含头不含尾)
    document.write(s2.substring(2,4) + "<br>");

    //4. split(value)   根据指定规则切割字符串，返回数组
    let s3 = "张三,23,男";
    let arr = s3.split(",");
    for(let i = 0; i < arr.length; i++) {
        document.write(arr[i] + "<br>");
    }

    //5. replace(old,new)   使用新字符串替换老字符串
    let s4 = "你会不会跳伞啊？让我落地成盒。你妹的。";
    let s5 = s4.replace("你妹的","***");
    document.write(s5 + "<br>");
</script>
</html>
```

## RegExp

所谓RegExp，其意为正则表达式，也就是对字符串进行匹配的一种规则，常用于确保用户输入的字符串符合我们的要求，如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE199d08d4c0312cf70d42551fde418dfa.png)

先是构造方法和成员方法，其核心方法是test方法，利用该方法来验证字符串是否符合

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9a6d0d74bcbff5408e9c6ca0d10d7fc8.png)

接着我们来看看正则表达式的规则

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1786df1f9c5eab76c3e7cdb03f055dd4.png)

然后我们来看看我们要实现的两个需求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd5385cb846a0487aeea544da99162377.png)

具体我们可以构造代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RegExp</title>
</head>
<body>

</body>
<script>
    //1.验证手机号
    //规则：第一位1，第二位358，第三到十一位必须是数字。总长度11
    let reg1 = /^[1][358][0-9]{9}$/;
    document.write(reg1.test("18688888888") + "<br>");

    //2.验证用户名
    //规则：字母、数字、下划线组成。总长度4~16
    let reg2 = /^[a-zA-Z_0-9]{4,16}$/;
    document.write(reg2.test("zhang_san123"));
</script>
</html>
```

上面的规则设定只是一个简单的入门了解，实际上的远不止于此，不过我们这里只做一个了解就可以了，更加具体的规则可以自己去查，或者去参照资料里的内容。

## Array

接着我们来学习Array，也就是数组对象，先来看看其拥有的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0a00ac1c0666351483fdd568b2c3d71f.png)

写入其测试代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Array</title>
</head>
<body>

</body>
<script>

    let arr = [1,2,3,4,5];

    //1. push(元素)    添加元素到数组的末尾
    arr.push(6);
    document.write(arr + "<br>");

    //2. pop()         删除数组末尾的元素
    arr.pop();
    document.write(arr + "<br>");

    //3. shift()       删除数组最前面的元素
    arr.shift();
    document.write(arr + "<br>");

    //4. includes(元素)  判断数组中是否包含指定的元素
    document.write(arr.includes(2) + "<br>");

    //5. reverse()      反转数组元素
    arr.reverse();
    document.write(arr + "<br>");

    //6. sort()         对数组元素排序
    arr.sort();
    document.write(arr + "<br>");

</script>
</html>
```

## Set

接着我们来学习Set集合对象，其特点是元素唯一，且存储顺序是一致的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfc7c46e1f6784fec10ea5897bc075b13.png)

然后我们可以构造其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set</title>
</head>
<body>

</body>
<script>
    // Set()   创建集合对象
    let s = new Set();

    // add(元素)   添加元素
    s.add("a");
    s.add("b");
    s.add("c");
    s.add("c");

    // size属性    获取集合的长度
    document.write(s.size + "<br>");    // 3

    // keys()      获取迭代器对象
    let st = s.keys();
    for(let i = 0; i < s.size; i++){
        document.write(st.next().value + "<br>");
    }

    // delete(元素) 删除指定元素
    document.write(s.delete("c") + "<br>");
    let st2 = s.keys();
    for(let i = 0; i < s.size; i++){
        document.write(st2.next().value + "<br>");
    }
</script>
</html>
```

## Map

然后我们可以来学习Map集合对象，key唯一，但是value可以不唯一，跟java中的差不多，先来看看其图解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE289cc96c12be0a060eedb5d1bcd4d092.png)

然后我们可以构造其示例代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Map</title>
</head>
<body>

</body>
<script>
    // Map()   创建Map集合对象
    let map = new Map();

    // set(key,value)  添加元素
    map.set("张三",23);
    map.set("李四",24);
    map.set("李四",25);

    // size属性     获取集合的长度
    document.write(map.size + "<br>");

    // get(key)     根据key获取value
    document.write(map.get("李四") + "<br>");

    // entries()    获取迭代器对象
    let et = map.entries();
    for(let i = 0; i < map.size; i++){
        document.write(et.next().value + "<br>");
    }

    // delete(key)  根据key删除键值对
    document.write(map.delete("李四") + "<br>");
    let et2 = map.entries();
    for(let i = 0; i < map.size; i++){
        document.write(et2.next().value + "<br>");
    }
</script>
</html>
```

## JSON

本节我们来学习JSON，简单来说JSON格式就是传输数据的一种格式，使用这种数据格式可以提升我们的效率

具体内容请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE55c6db4c93e4c239befaf5c745b445a2.png)

那么我们可以写入我们的演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JSON</title>
</head>
<body>

</body>
<script>
    //定义天气对象
    let weather = {
        city : "北京",
        date : "2088-08-08",
        wendu : "10° ~ 23°",
        shidu : "22%"
    };

    //1.将天气对象转换为JSON格式的字符串
    let str = JSON.stringify(weather);
    document.write(str + "<br>");

    //2.将JSON格式字符串解析成JS对象
    let weather2 = JSON.parse(str);
    document.write("城市：" + weather2.city + "<br>");
    document.write("日期：" + weather2.date + "<br>");
    document.write("温度：" + weather2.wendu + "<br>");
    document.write("湿度：" + weather2.shidu + "<br>");
</script>
</html>
```

# 表单校验案例

本章我们来完成一个综合案例——表单校验。

## 案例效果和分析

我们先来分析下我们的案例，我们的案例本身很简单，就是一个账户的提交，要求判断用户名和密码是否满足条件，具体如下图，同时下面也分析了实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfdc4c33f8d2b3ed01e326b1bce33e94d.png)

## 案例的实现

然后我们来正式去实现一下我们的案例，其实现代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单校验</title>
    <link rel="stylesheet" href="css/style.css"></link>
</head>
<body>
<div class="login-form-wrap">
    <h1>黑马程序员</h1>
    <form class="login-form" action="#" id="regist" method="get" autocomplete="off">
        <label>
            <input type="text" id="username" name="username" placeholder="Username..." value="">
        </label>
        <label>
            <input type="password" id="password" name="password" placeholder="Password..." value="">
        </label>
        <input type="submit" value="注册">
    </form>
</div>
</body>
<script>
    //1.为表单绑定提交事件
    document.getElementById("regist").onsubmit = function() {
        //2.获取填写的用户名和密码
        let username = document.getElementById("username").value;
        let password = document.getElementById("password").value;

        //3.判断用户名是否符合规则  4~16位纯字母
        let reg1 = /^[a-zA-Z]{4,16}$/;
        if(!reg1.test(username)) {
            alert("用户名不符合规则，请输入4到16位的纯字母！");
            return false;
        }

        //4.判断密码是否符合规则  6位纯数字
        let reg2 = /^[\d]{6}$/;
        if(!reg2.test(password)) {
            alert("密码不符合规则，请输入6位纯数字的密码！");
            return false;
        }

        //5.如果所有条件都不满足，则提交表单
        return true;
    }

</script>
</html>
```

然后是其CSS的样式代码

```css
* {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

html {
    background: #E2E2E2;
}

body {
    background: #E2E2E2;
    margin: 0;
    padding: 0;
    font-family: 'Lato', sans-serif;
}

.login-form-wrap {
    background: #5170ad;
    background: -moz-radial-gradient(center, ellipse cover, #5170ad 0%, #355493 100%);
    background: -webkit-gradient(radial, center center, 0px, center center, 100%, color-stop(0%, #5170ad), color-stop(100%, #355493));
    background: -webkit-radial-gradient(center, ellipse cover, #5170ad 0%, #355493 100%);
    background: -o-radial-gradient(center, ellipse cover, #5170ad 0%, #355493 100%);
    background: -ms-radial-gradient(center, ellipse cover, #5170ad 0%, #355493 100%);
    background: radial-gradient(ellipse at center, #5170ad 0%, #355493 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#5170ad', endColorstr='#355493',GradientType=1 );
    border: 1px solid #2d416d;
    box-shadow: 0 1px #5670a4 inset, 0 0 10px 5px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    position: relative;
    width: 360px;
    height: 380px;
    margin: 10px auto 20px auto;
    padding: 50px 30px 0 30px;
    text-align: center;
}
.login-form-wrap:before {
    /*background: url(http://i.imgur.com/0vLxyVB.png);*/
    display: block;
    content: '';
    width: 58px;
    height: 19px;
    top: 10px;
    left: 10px;
    position: absolute;
}
.login-form-wrap > h1 {
    margin: 0 0 50px 0;
    padding: 0;
    font-size: 26px;
    color: #fff;
}
.login-form-wrap > h5 {
    margin-top: 40px;
}
.login-form-wrap > h5 > a {
    font-size: 14px;
    color: #fff;
    text-decoration: none;
    font-weight: 400;
}

.login-form input[type="text"], .login-form input[type="password"] {
    width: 100%;
    border: 1px solid #314d89;
    outline: none;
    padding: 12px 20px;
    color: #afafaf;
    font-weight: 400;
    font-family: 'Lato', sans-serif;
    cursor: pointer;
}
.login-form input[type="text"] {
    border-bottom: none;
    border-radius: 4px 4px 0 0;
    padding-bottom: 13px;
    box-shadow: 0 -1px 0 #e0e0e0 inset, 0 1px 2px rgba(0, 0, 0, 0.23) inset;
}
.login-form input[type="password"] {
    border-top: none;
    border-radius: 0 0 4px 4px;
    box-shadow: 0 -1px 2px rgba(0, 0, 0, 0.23) inset, 0 1px 2px rgba(255, 255, 255, 0.1);
}


.login-form input[type="submit"] {
    font-family: 'Lato', sans-serif;
    font-weight: 400;
    background: #e0e0e0;
    background: -moz-linear-gradient(top, #e0e0e0 0%, #cecece 100%);
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%, #e0e0e0), color-stop(100%, #cecece));
    background: -webkit-linear-gradient(top, #e0e0e0 0%, #cecece 100%);
    background: -o-linear-gradient(top, #e0e0e0 0%, #cecece 100%);
    background: -ms-linear-gradient(top, #e0e0e0 0%, #cecece 100%);
    background: linear-gradient(to bottom, #e0e0e0 0%, #cecece 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#e0e0e0', endColorstr='#cecece',GradientType=0 );
    display: block;
    margin: 20px auto 0 auto;
    width: 100%;
    border: none;
    border-radius: 3px;
    padding: 8px;
    font-size: 17px;
    color: #636363;
    text-shadow: 0 1px 0 rgba(255, 255, 255, 0.45);
    font-weight: 700;
    box-shadow: 0 1px 3px 1px rgba(0, 0, 0, 0.17), 0 1px 0 rgba(255, 255, 255, 0.36) inset;
}
.login-form input[type="submit"]:hover {
    background: #DDD;
}
.login-form input[type="submit"]:active {
    padding-top: 9px;
    padding-bottom: 7px;
    background: #C9C9C9;
}

```

最后我们经过测试会发现我们的这个案例没有问题，此时就说明我们的案例已经成功实现了

- 内置对象的小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3665aa37b3870896a09835edfd8d0289.png)

# BOM

本章节我们来学习BOM，照例我们先来进行对BOM的介绍

## BOM的介绍

我们之前学习过DOM，其为文档对象模型，即是将我们的一个个标签封装成一个个文档对象模型，然后可以调用其对应的方法获得我们想要的东西。而BOM则是浏览器对象模型，其是将浏览器的各个组成部分封装成不同的对象，方便我们进行操作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6e4ad5a767051a64baa229c40789c633.png)

我们这里重点学习Window窗口对象和Location地址栏对象，其他的不学

## Window窗口对象

我们先来学习Window窗口对象，具体内容如下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfbcb1b3638e150777a0930b2c2215c0e.png)

那么我们可以写入我们的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>window窗口对象</title>
    <script>
        //一、定时器
        function fun(){
            alert("该起床了！");
        }

        //设置一次性定时器
        //let d1 = setTimeout("fun()",3000);
        //取消一次性定时器
        //clearTimeout(d1);

        //设置循环定时器
        //let d2 = setInterval("fun()",3000);
        //取消循环定时器
        //clearInterval(d2);

        //加载事件
        window.onload = function(){
            let div = document.getElementById("div");
            alert(div);
        }
    </script>
</head>
<body>
<div id="div">dddd</div>
</body>
<!-- <script>
    //一、定时器
    function fun(){
        alert("该起床了！");
    }

    //设置一次性定时器
    //let d1 = setTimeout("fun()",3000);
    //取消一次性定时器
    //clearTimeout(d1);

    //设置循环定时器
    //let d2 = setInterval("fun()",3000);
    //取消循环定时器
    //clearInterval(d2);

    //加载事件
    let div = document.getElementById("div");
    alert(div);
</script> -->
</html>
```

这里有两点是需要提的，第一点是如果我们想要调用Window对象里的setTimeout方法，这里面的Window前缀是可以省略不写的，但是我们要记住其方法的本质是来自于此的。

然后是为什么我们总是将我们的script标签写在body之后，这是因为如果我们要在script中执行某些事件的话，我们应该要先让事件加载出来，如果我们将script写在head标签中，则可能出现对应事件还没加载完呢，结果就已经先被调用的情况。为了避免这种情况所以我们总是将其下载body后

还有一种能够避免这种问题的方法是在script调用的对应事件的代码中使用window.onload，后面绑定一个函数，令该函数调用对应的事件，这样我们的事件就必须要等全部代码加载完才会自动触发，这样即使写在上面也不用担心我们的调用事件先有事件加载了

## Location地址栏对象

关于地址栏对象，我们最常用的就是可以设置新的地址并令其读取并显示新的地址的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE558c49c91f9df79769782836a2f40a51.png)

那么我们可以构造其演示代码如下，令其实现五秒自动减少并自动跳转到首页

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>location地址栏对象</title>
    <style>
        p{
            text-align: center;
        }
        span{
            color: red;
        }
    </style>
</head>
<body>
<p>
    注册成功！<span id="time">5</span>秒之后自动跳转到首页...
</p>
</body>
<script>
    //1.定义方法。改变秒数，跳转页面
    let num = 5;
    function showTime() {
        num--;

        if(num <= 0) {
            //跳转首页
            location.href = "index.html";
        }

        let span = document.getElementById("time");
        span.innerHTML = num;
    }

    //2.设置循环定时器，每1秒钟执行showTime方法
    setInterval("showTime()",1000);
</script>
</html>
```

## 动态广告案例

我们要实现的动态广告案例很简单，打开页面，三秒后在最上方显示广告，再过三秒就消失。我们来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb08414583c7b484fffcc69eb358bc7dd.png)

那么我们可以写入我们的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>头条页面</title>
    <link rel="stylesheet" href="css/news.css"/>
</head>
<body>
<!-- 广告图片 -->
<img src="img/ad_big.jpg" id="ad_big" width="100%" style="display: none;"/>

<!--顶部登录注册更多-->
<div class="top">
    <a href="#" target="_self">登录&nbsp;&nbsp;</a>
    <a href="#">注册&nbsp;&nbsp;</a>
    <a href="#">更多&nbsp;&nbsp;</a>
</div>

<!--导航条-->
<div class="nav">
    <img src="img/logo.png"/>
    <a href="#">首页&nbsp;&nbsp;</a>/
    <a href="#">科技&nbsp;&nbsp;</a>/
    <font color="gray">正文</font>
    <hr/>
</div>

<!--左侧分享-->
<div class="left">
    <a href="#"> <img src="img/cc.png"/> <span>&nbsp;评论</span> </a>
    <hr/>
    <a href="#"> <img src="img/repost.png"/> <span>&nbsp;转发</span> </a>  <br/>
    <a href="#"> <img src="img/weibo.png"/> <span>&nbsp;微博</span> </a>  <br/>
    <a href="#"> <img src="img/qq.png"/> <span>&nbsp;空间</span> </a>  <br/>
    <a href="#"> <img src="img/wx.png"/> <span>&nbsp;微信</span> </a>  <br/>
</div>

<!--中间正文-->
<div class="center">
    <div>
        <h1>支付宝特权福利！芝麻分600以上用户惊喜，网友：幸福来得突然？</h1>
    </div>

    <!--作者信息-->
    <div>
        <i><font size="2" color="gray">作者：itheima 2088-08-08</font></i>
        <hr/>
    </div>

    <!--副标题-->
    <div>
        <h3>支付宝特权福利！芝麻分600以上用户惊喜，网友：幸福来得突然？</h3>
    </div>

    <!--正文内容-->
    <div>
        <p>这些年，马云的风头正盛，但是上个月他毅然辞去了阿里巴巴的职务。而马云所做的很多事情也的确改变了这个世界，特别是在移动支付领域，更是走在了世界的前列。如今中国的移动支付已经成为老百姓的必备，支付宝对中国社会的变革都带来了深远的影响。不过马云依然没有满足，他认为移动互联网将会成为人类的基础设施，而且这里面的机会和各种挑战还非常多。</p>
        <p>支付宝的诞生就是为了解决淘宝网的客户们的买卖问题，而随着支付宝的用户的不断增加，支付宝也推出了一系列的附加功能。比如生活缴费、转账汇款、还信用卡、车主服务、公益理财等，往简单的说，支付宝既可以满足人们的日常生活，又可以利用芝麻信用进行资金周转服务。除了芝麻分能够进行周转以外，互联网信用体系下的产品多多，我们对比以下几个产品看看区别:
        <ol>
            <li>蚂蚁借呗，芝麻分600并且受到邀请开通福利，这个就是支付宝贷款，直接秒杀了银行贷款和线下金融公司，是现在支付宝用户使用最多的。</li>
            <li>微粒贷：于2015年上线，主要面向QQ和微信征信极好的用户而推出，受到邀请才能申请开通，额度最高有30万，难度较大</li>
            <li>蚂蚁巴士：这个在微信 蚂蚁巴士 公众平台申请,对于信用分要求530分以上才可以,额度1-30万不等，目前非常火爆</li>
        </ol>
        <img src="img/1.jpg" width="100%"/>
        </p>
        <p>说起支付宝中的芝麻信用功能，相信更是受到了许多人的推崇，因为随着自己使用的不断增多，信用分会慢慢提高，而达到了一个阶段，就可以获得许多的福利。而当我们的芝麻信用分可以达到600分以上的时候，会有令我们想象不到的惊喜，接下来就让我们一起来看看，具体都有哪些惊喜吧。</p>
        <p><b>一、芝麻分600以上福利之信用购。</b>网购相信大家都不陌生，但是很多时候，网购都有一个通病，就是没办法试用，导致很多人买了很多自己不喜欢的东西。但是只要你的支付宝芝麻分在650及以上，就能立马享有0元下单，收到货使用满意了再进行付款。还能享用美食的专属优惠，是不是很耐斯</p>
        <p><b>二、芝麻分600以上福利之信用免押。</b>芝麻信用与木鸟短租联合推出信用住宿服务，芝麻分600及以上的用户可享受免押入住特权。木鸟短租拥有全国50万套房源，是国内领先的短租民宿预订平台。包括大家知道的飞猪信用住，大部分酒店可以免押金入住，离店再交钱。</p>
        <img src="img/2.jpg" width="100%"/>
        <p><b>三、芝麻分600以上福利之国际驾照。</b>我们经常听说的可能只是中国驾照，但现在芝麻分已经应用到了国际领域，只要你的芝麻分够550就可以免费办理国际驾照，也有不少人非常佩服马云，一个简单的芝麻分居然有如此大的功能，也从侧面反应出来马云在国际上的地位，这个国际驾照是由新西兰、德国、澳大利亚联合认证，可以在全球200多个国家通行，相信大家一定都有一个自驾全球的梦想吧，而现在支付宝就给了你一把钥匙，剩下的就你自己搞定了！有没有想带着你的女神来一次浪漫之旅呢？</p>
        <p>随着互联网对我们生活的改变越来越大，信用这一词也被大家推上风口浪尖，不论是生活出行，还是其他的互联网服务，与信用体系已经密不可分了，马云当初说道，找老婆需要拼芝麻分，如今似乎也要成为现实，那么你们的芝麻分有多少了呢？</p>
    </div>
</div>

<!--右侧广告图片-->
<div class="right">
    <img src="img/ad1.jpg" width="100%"/>
    <img src="img/ad2.jpg" width="100%"/>
    <img src="img/ad3.jpg" width="100%"/>
    <img src="img/ad1.jpg" width="100%"/>
    <img src="img/ad2.jpg" width="100%"/>
    <img src="img/ad3.jpg" width="100%"/>
</div>

<!--底部页脚超链接-->
<div class="footer">
    <a href="#">关于黑马</a> &nbsp;
    <a href="#">帮助中心</a> &nbsp;
    <a href="#">开放平台</a> &nbsp;
    <a href="#">诚聘英才</a> &nbsp;
    <a href="#">联系我们</a> &nbsp;
    <a href="#">法律声明</a> &nbsp;
    <a href="#">隐私政策</a> &nbsp;
    <a href="#">知识产权</a> &nbsp;
    <a href="#">廉政举报</a> &nbsp;
</div>
</body>
<script>
    //1.设置定时器，3秒后显示广告图片
    setTimeout(function(){
        let img = document.getElementById("ad_big");
        img.style.display = "block";
    },3000);

    //2.设置定时器，3秒后隐藏广告图片
    setTimeout(function(){
        let img = document.getElementById("ad_big");
        img.style.display = "none";
    },6000);
</script>
</html>
```

这里我们的主要设置的代码就在于98-110行，其他的代码都是之前实现过的代码了。同时我们要注意，其计时器的执行顺序是，从上往下先执行第一个计时器，计时器开始计时，计时完毕之后执行对应方法，然后才到后面的代码，而不是一开始所有计时器都开始计时，这点要搞清楚。

- BOM的小结

最后我们可以来做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE07c359aec76e9e34154ae4c57984630b.png)

## 封装的思想

最后我们来学习一个封装，这个简单来说就是我们可以将方法封装到另一个文件中，然后我们引用这个文件的方法就可以调用原来的方法，这个跟java的思路差不多，将复杂的内部方法封装起来，留给用户的只有简单的调用方法的方法，这里就不多谈了。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE941668cd77c902cd8318d31107b7276a.png)

我们首先构造我们的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>封装</title>
</head>
<body>
<div id="div1">div1</div>
<div name="div2">div2</div>
</body>
<script src="my.js"></script>
<script>
    let div1 = getById("div1");
    alert(div1);

    // let div1 = document.getElementById("div1");
    // alert(div1);

    // let divs = document.getElementsByName("div2");
    // alert(divs.length);

    // let divs2 = document.getElementsByTagName("div");
    // alert(divs2.length);
</script>
</html>
```

可以看到我们这里第12行进行了一个js文件的引入，然后我们创建对应的js文件并写入代码如下

```
function getById(id){
return document.getElementById(id);
}

function getByName(name) {
return document.getElementsByName(name);
}

function getByTag(tag) {
return document.getElementsByTagName(tag);
}
```

然后我们只要调用我们自己新创建的方法就可以调用原来的方法了，用这玩意可以让我们的方法调用变得简便。

最后要注意的是，我们的封装方法的代码是放在js文件里的

