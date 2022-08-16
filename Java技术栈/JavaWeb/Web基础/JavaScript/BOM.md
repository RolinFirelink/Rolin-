本章节我们来学习BOM，照例我们先来进行对BOM的介绍

- BOM的介绍

我们之前学习过DOM，其为文档对象模型，即是将我们的一个个标签封装成一个个文档对象模型，然后可以调用其对应的方法获得我们想要的东西。而BOM则是浏览器对象模型，其是将浏览器的各个组成部分封装成不同的对象，方便我们进行操作

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3665aa37b3870896a09835edfd8d0289.png)

我们这里重点学习Window窗口对象和Location地址栏对象，其他的不学

- Window窗口对象

我们先来学习Window窗口对象，具体内容如下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfbcb1b3638e150777a0930b2c2215c0e.png)

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

- Location地址栏对象

关于地址栏对象，我们最常用的就是可以设置新的地址并令其读取并显示新的地址的内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE558c49c91f9df79769782836a2f40a51.png)

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

- 动态广告案例

我们要实现的动态广告案例很简单，打开页面，三秒后在最上方显示广告，再过三秒就消失。我们来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb08414583c7b484fffcc69eb358bc7dd.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE07c359aec76e9e34154ae4c57984630b.png)

- 封装的思想

最后我们来学习一个封装，这个简单来说就是我们可以将方法封装到另一个文件中，然后我们引用这个文件的方法就可以调用原来的方法，这个跟java的思路差不多，将复杂的内部方法封装起来，留给用户的只有简单的调用方法的方法，这里就不多谈了。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE941668cd77c902cd8318d31107b7276a.png)

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