学SpringBoot的时候发现这个还是躲不开，所以又回来了解了，当然理论上应该不学也还是可以的，但是马上就要到学习的最后一星期了，我希望全部搞定，所以还是学吧。

- Vue的介绍

首先我们来看看我们的Vue的介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7116c9ac566164b1420245a3442c85ae.png)

- Vue的快速入门

然后我们来做一个Vue的快速入门案例

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb2264b89c473681481baa4fc3f42a416.png)

由于Vue本质也是一个框架，所以我们同样也是要引入对应的文件的，否则我们根本没法用这个框架。然后我们创建一个页面，并写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>快速入门</title>
</head>
<body>
<!-- 视图 -->
<div id="div">
    {{msg}}
</div>
</body>
<script src="../js/vue.js"></script>
<script>
    // 脚本
    new Vue({
        el:"#div",
        data:{
            msg:"Hello Vue"
        }
    });
</script>
</html>
```

可以看到我们这里就使用了对应的脚本，并且在脚本中提供对应的数据然后将其赋予给我们的创建的视图，当然，这看起来似乎有点脱裤子放屁，不过这是有用的，等我们学到后面就知道了。我们这里通过先new一个脚本出来，然后通过el结合属性选择器获取到我们所需要的视图，然后取到其对应的内容并赋值

注意我们这里引入vue文件的方式，也就是第14行代码，我们是令其回退一级，然后找到js文件夹然后寻找其下的vue属性就行了的（这是因为我们的路径是默认从当前路径开始往下查找的，那自然查找不到，所以要先回退一级），其他的全路径名都试过了，但是都不行，最后就采用这种方法了，以后我们要引入对应的文件我们也使用这种方式

- 快速入门的升级

首先我们来介绍下我们刚刚的案例

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6e56218ac3d0f7bcef5cd9da07bed8dc.png)

然后我们要做一个升级的案例，就是在我们的Vue中定义一个方法并使用他，那么我们可以写入我们的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>快速入门升级</title>
</head>
<body>
<!-- 视图 -->
<div id="div">
    <div>姓名：{{name}}</div>
    <div>班级：{{classRoom}}</div>
    <button onclick="hi()">打招呼</button>
    <button onclick="update()">修改班级</button>
</div>
</body>
<script src="../js/vue.js"></script>
<script>
    // 脚本
    let vm = new Vue({
        el:"#div",
        data:{
            name:"张三",
            classRoom:"黑马程序员"
        },
        methods:{
            study(){
                alert(this.name + "正在" + this.classRoom + "好好学习!");
            }
        }
    });

    //定义打招呼方法
    function hi(){
        vm.study();
    }

    //定义修改班级
    function update(){
        vm.classRoom = "传智播客";
    }
</script>
</html>
```

- 快速入门小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6823337692fc7285a08be8ebcde7ee22.png)

