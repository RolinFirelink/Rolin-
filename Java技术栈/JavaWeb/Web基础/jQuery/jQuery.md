# jQuery快速入门

学习完了JS之后，我们来学习jQuery，首先我们先来进行一个快速入门的学习

## jQuery的介绍

简单来说，jQuery就是一个JS库，所谓库就是一个JS文件，里面有许多预定义好的函数，只带用户去调用，类似于Java里的API，可以简化我们的编程步骤并提高效率

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE825434ea18adc0913da9fd712297c378.png)

## jQuery案例

接下来我们来做一个快速入门的测试，先看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf6c095d77c468b35eb68f7ca0f2adc6a.png)

然后我们首先要创建对应的js文件夹并复制我们的jQuery文件到其中去，然后我们构造代码时就将该代码引入，然后调用其中的方法即可，请看演示代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>快速入门</title>
</head>
<body>
<div id="div">我是div</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // JS方式，通过id属性值来获取div元素
    let jsDiv = document.getElementById("div");
    //alert(jsDiv);
    //alert(jsDiv.innerHTML);

    // jQuery方式，通过id属性值来获取div元素
    let jqDiv = $("#div");
    alert(jqDiv);
    alert(jqDiv.html());
</script>
</html>
```

我们这里可以看到用jQuery方式快多了就行了，至于个中原理，我们暂时按下不表

- 快速入门的小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb742dd1edbfe5b8aa9f8697566e2eb51.png)

# 基本语法

接着我们来学习我们jQuery的基本语法

## 对象转换

虽然说jQuery其实就是JS的类库，但JS对象和jQuery对象终究是两个对象，如果我们想要在js对象里调用jQuery对象的方法，或者反过来，就要利用到对象转换

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEacae86e3e381fb062bc5f1fba3079d4d.png)

我们可以构造其示例代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>对象转换</title>
</head>
<body>
<div id="div">我是div</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // JS方式，通过id属性值获取div元素
    let jsDiv = document.getElementById("div");
    alert(jsDiv.innerHTML);
    //alert(jsDiv.html());  JS对象无法使用jQuery里面的功能

    // 将 JS 对象转换为jQuery对象
    let jq = $(jsDiv);
    alert(jq.html());

    // jQuery方式，通过id属性值获取div元素
    let jqDiv = $("#div");
    alert(jqDiv.html());
    // alert(jqDiv.innerHTML);   jQuery对象无法使用JS里面的功能

    // 将 jQuery对象转换为JS对象
    let js = jqDiv[0];
    alert(js.innerHTML);
</script>
</html>
```

jQuery对象转换为JS对象只要使用$()并将对应的JS对象放进去就可以了，但如果反过来则需要使用索引进行对象的转换，由于我们这里只是获得一个对象，因此我们这里转换只需要使用[0]来转换就可以了，当然，其实如果获得的是多个的话，我也不知道该怎么办好

## 事件的基本使用

下面的这些事件里，在jQuery中就是将on给去除掉了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2d4baf155af0b1efa7c71f6e2136fc23.png)

那么我们可以写入其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件的使用</title>
</head>
<body>
<input type="button" id="btn" value="点我">
<br>
<input type="text" id="input">
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //单击事件
    $("#btn").click(function(){
        alert("点我干嘛?");
    });

    //获取焦点事件
    // $("#input").focus(function(){
    //     alert("你要输入数据啦...");
    // });

    //失去焦点事件
    $("#input").blur(function(){
        alert("你输入完成啦...");
    });
</script>
</html>
```

## 事件的绑定和解绑

绑定事件之后如何解绑的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe674d29d57cafa12bfbd65ad74d052d3.png)

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件的绑定和解绑</title>
</head>
<body>
<input type="button" id="btn1" value="点我">
<input type="button" id="btn2" value="解绑">
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //给btn1按钮绑定单击事件
    $("#btn1").on("click",function(){
        alert("点我干嘛?");
    });

    //通过btn2解绑btn1的单击事件
    $("#btn2").on("click",function(){
        $("#btn1").off("click");
    });
</script>
</html>
```

## 事件的切换

在某些情况下，我们可能需要给同一个对象绑定多个事件，而且这些事件还有先后的顺序关系，此时我们就需要给这些对象的事件进行一个切换

最简单的例子就是，假如我们的光标点进到框里，框的颜色就变化，否则就不变，这就是一种最简单的时间的切换，也是我们等会要实现的案例

事件的切换的具体内容请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEff7b864eead3766c6c71bb2985307493.png)

那么我们可以写入我们的演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件的切换</title>
    <style>
        #div{
            border: 1px solid black;
        }
    </style>
</head>
<body>
<div id="div">我是div</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //方式一 单独定义
    // $("#div").mouseover(function(){
    //     //背景色：红色
    //     //$("#div").css("background","red");
    //     $(this).css("background","red");
    // });
    // $("#div").mouseout(function(){
    //     //背景色：蓝色
    //     //$("#div").css("background","blue");
    //     $(this).css("background","blue");
    // });

    //方式二 链式定义
    $("#div").mouseover(function(){
        $(this).css("background","red");
    }).mouseout(function(){
        $(this).css("background","blue");
    });
</script>
</html>
```

这里我们值得一提的有两点，第一点是我们先使用了$("#div")的方式获得了我们所需要的对象，那么后续的对象里我们可以使用this来指代这个对象，而不需要重新获取一次。第二点是我们这里设定背景色时是使用的方法是css()，这个方法需要传入两个字符串，分别是要设定的背景项以及要设置的具体内容，这里更加具体的知识我们会在后面进行讲解

最后，所谓的单独定义就是定义两个方法，而链式定义就是直接定义一个新的方法加在前一个方法的尾部罢了，大同小异其实。

## 遍历操作

我们先来学习我们传统的遍历方式，这里我们要注意，容器对象指的是要遍历的数组或者是集合，index指的是下标，ele指的是对应的对象内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1263c754c052a9e2a625205b7d2274da.png)

然后是不传统的新方式，其实也就那样吧，都大差不差

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5ee70500ec667060a83761eb69d01362.png)

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>遍历</title>
</head>
<body>
<input type="button" id="btn" value="遍历列表项">
<ul>
    <li>传智播客</li>
    <li>黑马程序员</li>
    <li>传智专修学院</li>
</ul>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //方式一：传统方式
    /*
    $("#btn").click(function(){
        let lis = $("li");
        for(let i = 0 ; i < lis.length; i++) {
            alert(i + ":" + lis[i].innerHTML);
        }
    });
    */


    //方式二：对象.each()方法
    /*
    $("#btn").click(function(){
        let lis = $("li");
        lis.each(function(index,ele){
            alert(index + ":" + ele.innerHTML);
        });
    });
    */

    //方式三：$.each()方法
    /*
    $("#btn").click(function(){
        let lis = $("li");
        $.each(lis,function(index,ele){
            alert(index + ":" + ele.innerHTML);
        });
    });
    */

    //方式四：for of 语句遍历
    $("#btn").click(function(){
        let lis = $("li");
        for(ele of lis){
            alert(ele.innerHTML);
        }
    });
</script>
</html>
```

- 基本语法的小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc60e864603e566e58d39848de335bac3.png)

# 选择器

学习完了jQuery的基本语法之后，我们来学习其对应的选择器，这一节的内容可以结合着我们之前在CSS里学习过的选择器来学习，由于选择器很多，我们我们要分节来介绍

## 基本选择器

首先我们来介绍基本选择器，其语法如下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5c9c9d11bf2e333d3c01c012cbc7b523.png)

然后我们可以构造其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>基本选择器</title>
</head>
<body>
<div id="div1">div1</div>
<div class="cls">div2</div>
<div class="cls">div3</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.元素选择器   $("元素的名称")
    let divs = $("div");
    //alert(divs.length);

    //2.id选择器    $("#id的属性值")
    let div1 = $("#div1");
    //alert(div1);

    //3.类选择器     $(".class的属性值")
    let cls = $(".cls");
    alert(cls.length);

</script>
</html>
```

有一点值得一提，那就是基本的选择器也是我们后面最常用的选择器，所以说基本的才是最重要的

## 层级选择器

层级选择器中的AB指代的是在实际编程的中的标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc10a4d6f77f9ccbb9642affafe81557f.png)

那么我们可以构造其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>层级选择器</title>
</head>
<body>
<div>
        <span>s1
            <span>s1-1</span>
            <span>s1-2</span>
        </span>
    <span>s2</span>
</div>

<div></div>
<p>p1</p>
<p>p2</p>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // 1. 后代选择器 $("A B");      A下的所有B(包括B的子级)
    let spans1 = $("div span");
    //alert(spans1.length);

    // 2. 子选择器   $("A > B");    A下的所有B(不包括B的子级)
    let spans2 = $("div > span");
    //alert(spans2.length);

    // 3. 兄弟选择器 $("A + B");    A相邻的下一个B
    let ps1 = $("div + p");
    //alert(ps1.length);

    // 4. 兄弟选择器 $("A ~ B");    A相邻的所有B
    let ps2 = $("div ~ p");
    alert(ps2.length);

</script>
</html>
```

## 属性选择器

知识点如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE75e87388c8916b55a50097808ca3c729.png)

其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>属性选择器</title>
</head>
<body>
<input type="text">
<input type="password">
<input type="password">
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.属性名选择器   $("元素[属性名]")
    let in1 = $("input[type]");
    //alert(in1.length);


    //2.属性值选择器   $("元素[属性名=属性值]")
    let in2 = $("input[type='password']");
    alert(in2.length);

</script>
</html>
```

## 过滤器选择器

语法如图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4f2aaf875990e5f2b827bf6afcc23873.png)

演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>过滤器选择器</title>
</head>
<body>
<div>div1</div>
<div id="div2">div2</div>
<div>div3</div>
<div>div4</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // 1.首元素选择器    $("A:first");
    let div1 = $("div:first");
    //alert(div1.html());

    // 2.尾元素选择器    $("A:last");
    let div4 = $("div:last");
    //alert(div4.html());

    // 3.非元素选择器    $("A:not(B)");
    let divs1 = $("div:not(#div2)");
    //alert(divs1.length);

    // 4.偶数选择器     $("A:even");
    let divs2 = $("div:even");
    //alert(divs2.length);
    //alert(divs2[0].innerHTML);
    //alert(divs2[1].innerHTML);

    // 5.奇数选择器     $("A:odd");
    let divs3 = $("div:odd");
    //alert(divs3.length);
    //alert(divs3[0].innerHTML);
    //alert(divs3[1].innerHTML);

    // 6.等于索引选择器    $("A:eq(index)");
    let div3 = $("div:eq(2)");
    //alert(div3.html());

    // 7.大于索引选择器    $("A:gt(index)");
    let divs4 = $("div:gt(1)");
    //alert(divs4.length);
    //alert(divs4[0].innerHTML);
    //alert(divs4[1].innerHTML);

    // 8.小于索引选择器    $("A:lt(index)");
    let divs5 = $("div:lt(2)");
    alert(divs5.length);
    alert(divs5[0].innerHTML);
    alert(divs5[1].innerHTML);

</script>
</html>
```

## 表单属性选择器

所谓表单属性选择器，我们要搞清楚什么是表单，表单也就是我们的选择项，可选用和不可选用很好理解。而单选/复选框指的是获取默认被选中的单选或者是复选框中的元素，而下拉框中的元素指的是我们下拉中选择的元素，实在不懂就自己去看看我们代码中的样例，很好理解的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6cb365e48e9d752572961dcb01f3e954.png)

然后我们构造其示例代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单属性选择器</title>
</head>
<body>
<input type="text" disabled>
<input type="text" >
<input type="radio" name="gender" value="men" checked>男
<input type="radio" name="gender" value="women">女
<input type="checkbox" name="hobby" value="study" checked>学习
<input type="checkbox" name="hobby" value="sleep" checked>睡觉
<select>
    <option>---请选择---</option>
    <option selected>本科</option>
    <option>专科</option>
</select>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    // 1.可用元素选择器  $("A:enabled");
    let ins1 = $("input:enabled");
    //alert(ins1.length);

    // 2.不可用元素选择器  $("A:disabled");
    let ins2 = $("input:disabled");
    //alert(ins2.length);

    // 3.单选/复选框被选中的元素  $("A:checked");
    let ins3 = $("input:checked");
    //alert(ins3.length);
    //alert(ins3[0].value);
    //alert(ins3[1].value);
    //alert(ins3[2].value);

    // 4.下拉框被选中的元素   $("A:selected");
    let select = $("select option:selected");
    alert(select.html());

</script>
</html>
```

最后我们要注意的是，我们39中填入的是select option，这么写是因为我们要的option标签是select的子标签，这样告诉我们，如果我们要其下的子标签，那么就要将父标签也写上，同时A也其实本质表示的是一个标签路径，中间用空格分隔

- 选择器小结

最后我们对选择器做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd403cd581318898ac118bcfd1312cc84.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE866defb6f97845dc940b08e9428ee220.png)

最后我们放入一个js文件，该文件就是我们本章节使用的jQuery文件，我们的案例都需要这个才能运行，创建一个js文件夹然后将这个放进去就可以了

[jquery-3.3.1.min.js](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-attachments/WEBRESOURCE2e7933f1842469b69f374b0e3fa02305jquery-3.3.1.min.js)

# DOM

本节我们来学习DOM，其实无非就是学习里面的各种方法而已 

## 操作文本

操作文本的方法我们之前就用过了，这里就不赘述了，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2929d547489e06c48114fdbf91f3469b.png)

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

## 操作对象

操作对象的方法的第一个就是创建指定元素，这里值得注意的是，如果元素一开始不存在则会创建对应元素，若存在则会获取对应的元素对象。

后续的append和appendTo无非是调用者不同的分别，具体到代码的不同就是代码的顺序反过来了，其他的没啥不同

而before方法一流添加的元素是兄弟关系的，remove方法可以实现自己删除自己，同时empty可以清空某个元素的所有子元素，注意其自身元素仍然是在的，只是子元素没了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc8873e4d7bb0ab69b37bedcb3e3d1865.png)

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

## 操作样式

这里属于是回收之前的css的伏笔了，来看看知识图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2e9f07f1208008795651efa8a8f89bba.png)

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

## 操作属性

最后我们来学习操作属性的方法，如图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE274f4ff9168d0b0c6307d676d299cf9f.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE853bb823975b96e19a6c34963aac1dfb.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE38f7787e93ac23efcf26a5082c95fbf7.png)

# 复选框案例

学习完了必须的语法之后，我们接着来做一个复选框的案例

## 案例效果的介绍

首先照例还是做一个案例效果的介绍，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfeb5374c82ad86cfd2eb752ddc6621ed.png)

然后我们已经有了基础的代码，接着我们去实现其选择部分的代码就可以了

## 案例的分析和实现

先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8f8f6f5fc039d402eaa21f1ea01c9e82.png)

那么我们可以构造其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>复选框</title>
</head>
<body>
<table id="tab1" border="1" width="800" align="center">
    <tr>
        <th style="text-align: left">
            <input style="background:lightgreen" id="selectAll" type="button" value="全选">
            <input style="background:lightgreen" id="selectNone" type="button" value="全不选">
            <input style="background:lightgreen" id="reverse" type="button" value="反选">
        </th>

        <th>分类ID</th>
        <th>分类名称</th>
        <th>分类描述</th>
        <th>操作</th>
    </tr>
    <tr>
        <td><input type="checkbox" class="item"></td>
        <td>1</td>
        <td>手机数码</td>
        <td>手机数码类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" class="item"></td>
        <td>2</td>
        <td>电脑办公</td>
        <td>电脑办公类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" class="item"></td>
        <td>3</td>
        <td>鞋靴箱包</td>
        <td>鞋靴箱包类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" class="item"></td>
        <td>4</td>
        <td>家居饰品</td>
        <td>家居饰品类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
</table>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //全选
    //1.为全选按钮添加单击事件
    $("#selectAll").click(function(){
        //2.获取所有的商品复选框元素，为其添加checked属性，属性值true
        $(".item").prop("checked",true);
    });


    //全不选
    //1.为全不选按钮添加单击事件
    $("#selectNone").click(function(){
        //2.获取所有的商品复选框元素，为其添加checked属性，属性值false
        $(".item").prop("checked",false);
    });


    //反选
    //1.为反选按钮添加单击事件
    $("#reverse").click(function(){
        //2.获取所有的商品复选框元素，为其添加checked属性，属性值是目前相反的状态
        let items = $(".item");
        items.each(function(){
            $(this).prop("checked",!$(this).prop("checked"));
        });
    });
</script>
</html>
```

这里值得一提的是，我们得到的对象自然是集合对象，我们只有反选的时候需要对集合进行一个判断并特别设置其相反的值。我们如果想对其进行一个统一的设置，直接对集合对象调用对应的设置方法就完了，不需要跟java中一样还要特别拿出每一项来设置

# 随机图片案例

然后我们来做一个随机图片的案例，这是最后一个案例

## 案例效果的介绍

点击开始会在大图小图里不断切换图片，选择停止就展示一个，就这么简单

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf458da068e6f2c8cd9ea8190edcdbbf5.png)

## 循环显示图片的分析和实现

先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE26e160b9f54bd19356a36e8875f10923.png)

然后我们来看看大图的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc37faf7ff38c164b084b5c9af6bc11fb.png)

最后我们可以写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>随机图片</title>
</head>
<body>
<!-- 小图 -->
<div style="background-color:red;border: dotted; height: 50px; width: 50px">
    <img src="img/01.jpg" id="small" style="width: 50px; height: 50px;">
</div>
<!-- 大图 -->
<div style="border: double ;width: 400px; height: 400px; position: absolute; left: 500px; top:10px">
    <img src="" id="big" style="width: 400px; height: 400px; display:none;">
</div>

<!-- 开始和结束按钮 -->
<input id="startBtn" type="button" style="width: 150px;height: 150px; font-size: 20px" value="开始">
<input id="stopBtn" type="button" style="width: 150px;height: 150px; font-size: 20px" value="停止">
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.准备一个数组
    let imgs = [
        "img/01.jpg",
        "img/02.jpg",
        "img/03.jpg",
        "img/04.jpg",
        "img/05.jpg",
        "img/06.jpg",
        "img/07.jpg",
        "img/08.jpg",
        "img/09.jpg",
        "img/10.jpg"];

    //2.定义计数器变量
    let count = 0;

    //3.声明定时器对象
    let time = null;

    //4.声明图片路径变量
    let imgSrc = "";

    //5.为开始按钮绑定单击事件
    $("#startBtn").click(function(){
        //6.设置按钮状态
        //禁用开始按钮
        $("#startBtn").prop("disabled",true);
        //启用停止按钮
        $("#stopBtn").prop("disabled",false);

        //7.设置定时器，循环显示图片
        time = setInterval(function(){
            //8.循环获取图片路径
            let index = count % imgs.length; // 0%10=0  1%10=1  2%10=2 .. 9%10=9  10%10=0  

            //9.将当前图片显示到小图片上
            imgSrc = imgs[index];
            $("#small").prop("src",imgSrc);

            //10.计数器自增
            count++;
        },10);
    });


    //11.为停止按钮绑定单击事件
    $("#stopBtn").click(function(){
        //12.取消定时器
        clearInterval(time);

        //13.设置按钮状态
        //启用开始按钮
        $("#startBtn").prop("disabled",false);
        //禁用停止按钮
        $("#stopBtn").prop("disabled",true);

        //14.将图片显示到大图片上
        $("#big").prop("src",imgSrc);
        $("#big").prop("style","width: 400px; height: 400px;");
    });



</script>
</html>
```

