本来我们这里还有一个学生案例的，但我们只是了解为主，所以我们这里就直接跳过什么几把学生案例了，直接来学习其高级使用吧

- 自定义组件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7ce3610048334eed7f586d001a3e12bb.png)

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

- Vue的生命周期

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE005148a79e1612bceda39bd5b1285f78.png)

上图就是Vue的生命周期，简单来说可以分成下面八个阶段

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0b93b78fc3a0e2d9253bc4aa6b088322.png)

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

- 异步操作

具体的内容就看图吧，其本质还是AJAX，不过要使用axios插件来简化操作

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9f4e625e2015954a99ba37fee83292da.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE19f62a68a4c5e3c3d0bc0e900057e136.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1bb73b71a8e96b48a4fd8574f890c881.png)

