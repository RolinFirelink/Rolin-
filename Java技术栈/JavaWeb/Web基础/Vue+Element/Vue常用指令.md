接着我们进入到Vue的常用指令的学习，首先我们来对我们的指令进行一个简单的介绍

- 指令的介绍

首先我们来看看我们的指令介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe1ce9f4aa953d426e1394bbfa83ee669.png)

- 文本插值

其实v-html的作用简单来说就是可以让我们的文本正确解析我们最初传入的html的代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1ea966a1c75d0f39f4ba2faacdd522ee.png)

具体请看下面的例子

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>文本插值</title>
</head>
<body>
<div id="div">
    <div>{{msg}}</div>
    <div v-html="msg"></div>
</div>
</body>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el:"#div",
        data:{
            msg:"<b>Hello Vue</b>"
        }
    });
</script>
</html>
```

- 绑定属性

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE799abb95d0eeb04409481e1a9d810d35.png)

我们可以利用v-bind来个我们的某个HTML标签绑定属性值，属性值包括但不限于超链接以及css样式

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>绑定属性</title>
    <style>
        .my{
            border: 1px solid red;
        }
    </style>
</head>
<body>
<div id="div">
    <a v-bind:href="url">百度一下</a>
    <br>
    <a :href="url">百度一下</a>
    <br>
    <div :class="cls">我是div</div>
</div>
</body>
<script src="/..js/vue.js"></script>
<script>
    new Vue({
        el:"#div",
        data:{
            url:"https://www.baidu.com",
            cls:"my"
        }
    });
</script>
</html>
```

另外我们这里绑定是可以将v-bind省略的，只写其后面的内容也可以正确绑定。（另外不知道为啥这例子在idea上会报红，由于我们是了解为主，所以就不管了）

- 条件渲染

所谓的条件渲染可以简单理解为我们的代码是否加载，这里我们要特别讲一下v-if和v-show的不同，前者如果条件为真是才会将对应的代码加载到页面中，而后者是为真时才展示，但不为真时不展示，其代码是会正确加载到页面

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7d266baa83851136c57769cc53bc659c.png)

那么我们可以写入我们的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>条件渲染</title>
</head>
<body>
<div id="div">
    <!-- 判断num的值，对3取余  余数为0显示div1  余数为1显示div2  余数为2显示div3 -->
    <div v-if="num % 3 == 0">div1</div>
    <div v-else-if="num % 3 == 1">div2</div>
    <div v-else="num % 3 == 2">div3</div>

    <div v-show="flag">div4</div>
</div>
</body>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el:"#div",
        data:{
            num:1,
            flag:false
        }
    });
</script>
</html>
```

- 列表渲染

我们的列表渲染简单来说，就是遍历，其可以遍历容器，也可以遍历对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3f27d201a492e428eb04e5b423a31b3d.png)

其类似于增强for循环，具体的演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>列表渲染</title>
</head>
<body>
<div id="div">
    <ul>
        <li v-for="name in names">
            {{name}}
        </li>
        <li v-for="value in student">
            {{value}}
        </li>
    </ul>
</div>
</body>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el:"#div",
        data:{
            names:["张三","李四","王五"],
            student:{
                name:"张三",
                age:23
            }
        }
    });
</script>
</html>
```

- 事件绑定

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5da2b62ee80603387a921f119f330bd7.png)

这一节我们值得一提的是，我们可以将v-on:省略，用一个@符号作为代替，后面接我们要绑定的事件的开启方式以及事件的具体内容（也就是函数）就完了

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件绑定</title>
</head>
<body>
<div id="div">
    <div>{{name}}</div>
    <button v-on:click="change()">改变div的内容</button>
    <button v-on:dblclick="change()">改变div的内容</button>

    <button @click="change()">改变div的内容</button>
</div>
</body>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el:"#div",
        data:{
            name:"黑马程序员"
        },
        methods:{
            change(){
                this.name = "传智播客"
            }
        }
    });
</script>
</html>
```

- 表单绑定

我们的表单绑定简单来说就是使用这个绑定可以让我们的表单数据和内存中的数据实现双向绑定，一边的更改会导致另一边的更改

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8310cb785f4887c02a34757361306b89.png)

最后我们可以看看其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单绑定</title>
</head>
<body>
<div id="div">
    <form autocomplete="off">
        姓名：<input type="text" name="username" v-model="username">
        <br>
        年龄：<input type="number" name="age" v-model="age">
    </form>
</div>
</body>
<script src="js/vue.js"></script>
<script>
    new Vue({
        el:"#div",
        data:{
            username:"张三",
            age:23
        }
    });
</script>
</html>
```

值得一提的是，我们这里说的双向绑定不是说我们的页面数据一改结果后台代码的数据要跟着改动啊，其意思是前台数据和后台的放在内存中的数据跟着改而已，这点不要搞错了

- 常用指令的小结

最后我们来做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE280e42dca3cd06193ea70a5b3fd80ee7.png)

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE849366bec5d40f4c66fc4057ba6d0ee8.png)

