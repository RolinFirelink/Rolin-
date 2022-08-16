本节我们来学习DOM，其实无非就是学习里面的各种方法而已 

- 操作文本

操作文本的方法我们之前就用过了，这里就不赘述了，直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE866defb6f97845dc940b08e9428ee220.png)

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>操作文本</title>
</head>
<body>
<div id="div">我是div</div>
<input type="button" id="btn1" value="获取div的文本">
<input type="button" id="btn2" value="设置div的文本">
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1. html()   获取标签的文本内容
    $("#btn1").click(function(){
        //获取div标签的文本内容
        let value = $("#div").html();
        alert(value);
    });

    //2. html(value)   设置标签的文本内容，解析标签
    $("#btn2").click(function(){
        //设置div标签的文本内容
        //$("#div").html("我真的是div");
        $("#div").html("<b>我真的是div</b>");
    });
</script>
</html>
```

- 操作对象

操作对象的方法的第一个就是创建指定元素，这里值得注意的是，如果元素一开始不存在则会创建对应元素，若存在则会获取对应的元素对象。

后续的append和appendTo无非是调用者不同的分别，具体到代码的不同就是代码的顺序反过来了，其他的没啥不同

而before方法一流添加的元素是兄弟关系的，remove方法可以实现自己删除自己，同时empty可以清空某个元素的所有子元素，注意其自身元素仍然是在的，只是子元素没了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2929d547489e06c48114fdbf91f3469b.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>操作对象</title>
</head>
<body>
<div id="div"></div>
<input type="button" id="btn1" value="添加一个span到div"> <br><br><br>

<input type="button" id="btn2" value="将加油添加到城市列表最下方"> &nbsp;&nbsp;&nbsp;
<input type="button" id="btn3" value="将加油添加到城市列表最上方"> &nbsp;&nbsp;&nbsp;
<input type="button" id="btn4" value="将雄起添加到上海下方"> &nbsp;&nbsp;&nbsp;
<input type="button" id="btn5" value="将雄起添加到上海上方"> &nbsp;&nbsp;&nbsp;
<ul id="city">
    <li id="bj">北京</li>
    <li id="sh">上海</li>
    <li id="gz">广州</li>
    <li id="sz">深圳</li>
</ul>
<ul id="desc">
    <li id="jy">加油</li>
    <li id="xq">雄起</li>
</ul>  <br><br><br>
<input type="button" id="btn6" value="将雄起删除"> &nbsp;&nbsp;&nbsp;
<input type="button" id="btn7" value="将描述列表全部删除"> &nbsp;&nbsp;&nbsp;
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    /*
        1. $("元素")   创建指定元素
        2. append(element)   添加成最后一个子元素，由添加者对象调用
        3. appendTo(element) 添加成最后一个子元素，由被添加者对象调用
        4. prepend(element)  添加成第一个子元素，由添加者对象调用
        5. prependTo(element) 添加成第一个子元素，由被添加者对象调用
        6. before(element)    添加到当前元素的前面，两者之间是兄弟关系，由添加者对象调用
        7. after(element)     添加到当前元素的后面，两者之间是兄弟关系，由添加者对象调用
        8. remove()           删除指定元素(自己移除自己)
        9. empty()            清空指定元素的所有子元素
    */

    // 按钮一：添加一个span到div
    $("#btn1").click(function(){
        let span = $("<span>span</span>");
        $("#div").append(span);
    });


    //按钮二：将加油添加到城市列表最下方
    $("#btn2").click(function(){
        //$("#city").append($("#jy"));
        $("#jy").appendTo($("#city"));
    });

    //按钮三：将加油添加到城市列表最上方
    $("#btn3").click(function(){
        //$("#city").prepend($("#jy"));
        $("#jy").prependTo($("#city"));
    });


    //按钮四：将雄起添加到上海下方
    $("#btn4").click(function(){
        $("#sh").after($("#xq"));
    });


    //按钮五：将雄起添加到上海上方
    $("#btn5").click(function(){
        $("#sh").before($("#xq"));
    });

    //按钮六：将雄起删除
    $("#btn6").click(function(){
        $("#xq").remove();
    });


    //按钮七：将描述列表全部删除
    $("#btn7").click(function(){
        $("#desc").empty();
    });

</script>
</html>
```

这里要注意的是所谓的添加元素其实是在移动元素，尤其是对于append方法一流的，其实是将一个地方的元素移动到另一个地方，而不是创建一个新的并将其移动过去

- 操作样式

这里属于是回收之前的css的伏笔了，来看看知识图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2e9f07f1208008795651efa8a8f89bba.png)

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>操作样式</title>
    <style>
        .cls1{
            background: pink;
            height: 30px;
        }
    </style>
</head>
<body>
<div style="border: 1px solid red;" id="div">我是div</div>
<input type="button" id="btn1" value="获取div的样式"> &nbsp;&nbsp;
<input type="button" id="btn2" value="设置div的背景色为蓝色">&nbsp;&nbsp;
<br><br><br>
<input type="button" id="btn3" value="给div设置cls1样式"> &nbsp;&nbsp;
<input type="button" id="btn4" value="给div删除cls1样式"> &nbsp;&nbsp;
<input type="button" id="btn5" value="给div设置或删除cls1样式"> &nbsp;&nbsp;
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // 1.css(name)   获取css样式
    $("#btn1").click(function(){
        alert($("#div").css("border"));
    });

    // 2.css(name,value)   设置CSS样式
    $("#btn2").click(function(){
        $("#div").css("background","blue");
    });

    // 3.addClass(value)   给指定的对象添加样式类名
    $("#btn3").click(function(){
        $("#div").addClass("cls1");
    });

    // 4.removeClass(value)  给指定的对象删除样式类名
    $("#btn4").click(function(){
        $("#div").removeClass("cls1");
    });

    // 5.toggleClass(value)  如果没有样式类名，则添加。如果有，则删除
    $("#btn5").click(function(){
        $("#div").toggleClass("cls1");
    });

</script>
</html>
```

这里比较重要的是addClass方法和removeClass方法，这两个方法比较灵活，后面我们也可以先自己定义好多个样式，然后我们自己去调用这些方法来去添加或者移除对应的样式

- 操作属性

最后我们来学习操作属性的方法，如图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE853bb823975b96e19a6c34963aac1dfb.png)

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>操作属性</title>
</head>
<body>
<input type="text" id="username">
<br>
<input type="button" id="btn1" value="获取输入框的id属性">  &nbsp;&nbsp;
<input type="button" id="btn2" value="给输入框设置value属性">
<br><br>

<input type="radio" id="gender1" name="gender">男
<input type="radio" id="gender2" name="gender">女
<br>
<input type="button" id="btn3" value="选中女">
<br><br>

<select>
    <option>---请选择---</option>
    <option id="bk">本科</option>
    <option id="zk">专科</option>
</select>
<br>
<input type="button" id="btn4" value="选中本科">
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // 1.attr(name,[value])   获得/设置属性的值
    //按钮一：获取输入框的id属性
    $("#btn1").click(function(){
        alert($("#username").attr("id"));
    });

    //按钮二：给输入框设置value属性
    $("#btn2").click(function(){
        $("#username").attr("value","hello...");
    });


    // 2.prop(name,[value])   获得/设置属性的值(checked，selected)
    //按钮三：选中女
    $("#btn3").click(function(){
        $("#gender2").prop("checked",true);
    });

    //按钮四：选中本科
    $("#btn4").click(function(){
        $("#bk").prop("selected",true);
    });
</script>
</html>
```

- DOM操作的小结

最后我们可以做一个小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE274f4ff9168d0b0c6307d676d299cf9f.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfeb5374c82ad86cfd2eb752ddc6621ed.png)

