那么现在我们正式来学习JS里的第一个内容，DOM，首先我们来说明下什么是DOM

- DOM的介绍

现在我们正式来介绍下我们的DOM，我们的DOM简而言之就是将我们的标签再内存中转为一个个对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE03172161b7a9c4da4811562436e2573f.png)

比如左边的HTML文件，那么其在内存中所对应创建出来的就会是如右边所示的树形结构，可以看到我们先创建文档对象，该对象下有许多元素，元素就是那些标签，然后某些元素下会有属性对象，然后最后他们基本都是要连接到一个文本对象的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1a750ed2d170d004413f41fbc4b24026.png)

- 元素的获取操作

我们先来学习元素对象Element，其下有五个方法，分别可以根据id、标签名称、name属性值、class属性值以及其子元素对象的某个属性来获取对应的结果，具体请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE84105af3c50364d2ee497460840dc84c.png)

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

- 元素的增删改操作

元素还有增删改操作的对应方法，具体请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf4bfc003ff9948cc5f286b71b11f7625.png)

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

- 属性的操作

接着我们来演示Attribute属性的操作，我们可以设置属性，通过属性名获取或者是移除指定的属性，最后还有一个方法是可以通过属性为元素添加样式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcd653535bb2e0b714a0ff219154c02db.png)

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

- 文本的操作

最后我们来讲解文本内的操作和其对应的方法，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0d2d006456d6439d2e1dfe252cf629ac.png)

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

- DOM的小结

最后我们可以对DOM做一个总结，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc507a747df66df901657f1ed2712a025.png)

然后来看看属性的总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3129ade46edf3e84a62d018b8fdc08fb.png)

