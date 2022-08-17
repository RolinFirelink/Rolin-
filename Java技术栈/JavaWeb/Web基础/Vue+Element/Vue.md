# Vue快速入门

学SpringBoot的时候发现这个还是躲不开，所以又回来了解了，当然理论上应该不学也还是可以的，但是马上就要到学习的最后一星期了，我希望全部搞定，所以还是学吧。

## Vue的介绍

首先我们来看看我们的Vue的介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7116c9ac566164b1420245a3442c85ae.png)

## Vue的快速入门

然后我们来做一个Vue的快速入门案例

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe1ce9f4aa953d426e1394bbfa83ee669.png)

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

## 快速入门的升级

首先我们来介绍下我们刚刚的案例

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6e56218ac3d0f7bcef5cd9da07bed8dc.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6823337692fc7285a08be8ebcde7ee22.png)

# Vue常用指令

接着我们进入到Vue的常用指令的学习，首先我们来对我们的指令进行一个简单的介绍

## 指令的介绍

首先我们来看看我们的指令介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb2264b89c473681481baa4fc3f42a416.png)

## 文本插值

其实v-html的作用简单来说就是可以让我们的文本正确解析我们最初传入的html的代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE799abb95d0eeb04409481e1a9d810d35.png)

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

## 绑定属性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1ea966a1c75d0f39f4ba2faacdd522ee.png)

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

## 条件渲染

所谓的条件渲染可以简单理解为我们的代码是否加载，这里我们要特别讲一下v-if和v-show的不同，前者如果条件为真是才会将对应的代码加载到页面中，而后者是为真时才展示，但不为真时不展示，其代码是会正确加载到页面

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7d266baa83851136c57769cc53bc659c.png)

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

## 列表渲染

我们的列表渲染简单来说，就是遍历，其可以遍历容器，也可以遍历对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE849366bec5d40f4c66fc4057ba6d0ee8.png)

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

## 事件绑定

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5da2b62ee80603387a921f119f330bd7.png)

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

## 表单绑定

我们的表单绑定简单来说就是使用这个绑定可以让我们的表单数据和内存中的数据实现双向绑定，一边的更改会导致另一边的更改

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3f27d201a492e428eb04e5b423a31b3d.png)

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

常用指令小结

最后我们来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8310cb785f4887c02a34757361306b89.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE280e42dca3cd06193ea70a5b3fd80ee7.png)

# Vue高级使用

本来我们这里还有一个学生案例的，但我们只是了解为主，所以我们这里就直接跳过什么几把学生案例了，直接来学习其高级使用吧

## 自定义组件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7ce3610048334eed7f586d001a3e12bb.png)

那么我们可以写入其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>自定义组件</title>
    <script src="../js/vue.js"></script>
</head>
<body>
<div id="div">
    <my-button>我的按钮</my-button>
</div>
</body>
<script>
    Vue.component("my-button",{
        // 属性
        props:["style"],
        // 数据函数
        data: function(){
            return{
                msg:"我的按钮"
            }
        },
        //解析标签模板
        template:"<button style='color:red'>{{msg}}</button>"
    });

    new Vue({
        el:"#div"
    });
</script>
</html>
```

## Vue的生命周期

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0b93b78fc3a0e2d9253bc4aa6b088322.png)

上图就是Vue的生命周期，简单来说可以分成下面八个阶段

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE19f62a68a4c5e3c3d0bc0e900057e136.png)

那么我们可以写入其演示代码如下，注意我们的演示代码要能够正确查看要去网页控制台上看

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>生命周期</title>
    <script src="../js/vue.js"></script>
</head>
<body>
<div id="app">
    {{message}}
</div>
</body>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            message: 'Vue的生命周期'
        },
        beforeCreate: function() {
            console.group('------beforeCreate创建前状态------');
            console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red", "data   : " + this.$data); //undefined 
            console.log("%c%s", "color:red", "message: " + this.message);//undefined
        },
        created: function() {
            console.group('------created创建完毕状态------');
            console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化 
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
        },
        beforeMount: function() {
            console.group('------beforeMount挂载前状态------');
            console.log("%c%s", "color:red", "el     : " + (this.$el)); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化  
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化  
        },
        mounted: function() {
            console.group('------mounted 挂载结束状态------');
            console.log("%c%s", "color:red", "el     : " + this.$el); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化 
        },
        beforeUpdate: function() {
            console.group('beforeUpdate 更新前状态===============》');
            let dom = document.getElementById("app").innerHTML;
            console.log(dom);
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        updated: function() {
            console.group('updated 更新完成状态===============》');
            let dom = document.getElementById("app").innerHTML;
            console.log(dom);
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        beforeDestroy: function() {
            console.group('beforeDestroy 销毁前状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        destroyed: function() {
            console.group('destroyed 销毁完成状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        }
    });


    // 销毁Vue对象
    //vm.$destroy();
    //vm.message = "hehe"; // 销毁后 Vue 实例会解绑所有内容

    // 设置data中message数据值
    vm.message = "good...";
</script>
</html>
```

## 异步操作

具体的内容就看图吧，其本质还是AJAX，不过要使用axios插件来简化操作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1bb73b71a8e96b48a4fd8574f890c881.png)

那么我们要发送异步请求，我们可以写入我们的处理请求响应的代码如下

```
package com.itheima;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet("/testServlet")
public class TestServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置请求和响应的编码
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        //获取请求参数
        String name = req.getParameter("name");
        System.out.println(name);

        //响应客户端
        resp.getWriter().write("请求成功");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}

```

然后我们写入我们的网页发起请求的代码如下，其下演示了图中的各种方法

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>异步操作</title>
    <script src="../js/vue.js"></script>
    <script src="../js/axios-0.18.0.js"></script>
</head>
<body>
<div id="div">
    {{name}}
    <button @click="send()">发起请求</button>
</div>
</body>
<script>
    new Vue({
        el:"#div",
        data:{
            name:"张三"
        },
        methods:{
            send(){
                // GET方式请求
                // axios.get("testServlet?name=" + this.name)
                //     .then(resp => {
                //         alert(resp.data);
                //     })
                //     .catch(error => {
                //         alert(error);
                //     })

                // POST方式请求
                axios.post("testServlet","name="+this.name)
                    .then(resp => {
                        alert(resp.data);
                    })
                    .catch(error => {
                        alert(error);
                    })
            }
        }
    });
</script>
</html>
```

- Vue高级使用的小结

最后我们可以来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE005148a79e1612bceda39bd5b1285f78.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9f4e625e2015954a99ba37fee83292da.png)

