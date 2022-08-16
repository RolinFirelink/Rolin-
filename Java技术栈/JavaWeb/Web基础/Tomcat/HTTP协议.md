学习完了铸币的Tomcat之后，我们现在来进入HTTP协议的学习

- HTTP协议的介绍

首先我们来说说什么是HTTP协议，HTTP是Hyper Text Transfer Protocol的缩写，其意为超文本传输协议，其实基于TCP/IP协议的协议。

超文本即是比普通文本更加强大的文本，而传输协议指的是客户端与服务器端的通信规则，也被称为握手规则

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd574805ce850e7c574452b88fb4d2351.png)

HTTP协议最重要的两个部分就是请求和响应

注意客户端与服务器之间，是由客户端先向服务器发起请求，而服务器响应其请求的，而不会是服务器自顾自响应，客户端接收。假设我们输入bilibili的网址，就相当于我们请求bilibili的服务器，服务器会响应我们的请求。而我们向服务器手动发起网址请求是我们做的，而其他一类图片音像资源则是会自动发起请求的

- HTTP协议的请求

我们之前说过HTTP协议中最重要的两个部分就是请求和响应，我们现在讲讲请求这一部分

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE79305f2a8c5c7a24c6589a11ebca5cca.png)

请求一般分为四个部分，分别是请求行，请求头，请求空行和请求体。根据请求的方式不同还分为get请求方式和post请求方式，这两个方式是我们之前学习过的，get请求方式会将用户输入的内容直接显示在地址上，不但不安全而且有长度的限制，而post请求方式则不会，所以一般都使用post请求方式进行请求。这里值得注意的是，只有POST请求方式才有请求体

先来看看get请求方式的例子

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE97e4ad4a00b94c4e7fa716c77fbd3b22.png)

首先是请求行，请求行中的GET代表请求方式为GET方式，后面login.html代表我们处于的请求网址，而更后面的内容则代表我们的用户输入的内容，HTTP/1.1则分别代表我们的协议名称和目前使用的协议的版本号

接着是请求头的部分，Host代表我们请求的主机。User-Agent则代表我们浏览器的信息， 比如在这里我们能知道用户使用的64位系统，然后使用的是火狐浏览器。Accept则代表支持的资源类型，比如这里可以看到其支持text、html、xml等类型。Accept-Language则代表其支持的语言语言，比如这里能接受的语言有中文中国，中国繁体，英语等。Accept-Encoding则代表其能支持的压缩方式，一般默认是gzip压缩方式。Connection则代表连接状态，这里的keep-alive代表一直处于连接状态。Referer则代表请求来源，其实就相当于是我们在地址栏中输入的完整路径。后面的内容则都是与会话管理与缓存相关的，这些都非常重要，但是这些部分就由我们后期学习到那个内容的时候再来讲

然后是请求空行，其实请求空行就是个回车，主要作用就是将请求头和请求体分开的

最后是请求体的部分，由于我们这里使用的GET方式，因此我们没有请求体

再来看看POST请求方式的例子

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcf94f83ac2c91d3ce55fe4e16335a035.png)

可以看到POST方式也同样有请求行请求头请求空行以及请求体，与GET请求方式不同的是，POST的请求方式将用户输入的内容放置于请求体中

在火狐的开发者工具里可以按F12调用开发者工具，然后可以看到点击网络并点击第一行可以看到我们的内部GET请求方式的具体信息

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE95058c5b93f9ebd11a24fb2ebc4e1400.png)

同理对于Post方式也是一样的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2e84381e4f47a382167673bd08eed452.png)

这里我们选择了编辑，就可以看到请求主体了

最后我们来做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEef5d7da985910e44b27d621693144897.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE59cc45744a41292780eef31b0aba8923.png)

- HTTP协议的响应

与HTTP的请求类似，HTTP的响应也分为四部分，分别是响应行，响应头，响应空行以及响应体

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE99309288a1774f92a7e963d84173e508.png)

首先是响应行，POST代表的是响应的方式，请求的方式是怎么样的那么响应的方式就是怎么样的，接着HTTP/1.1分别代表的是协议的名词的协议版本，200则是代表成功的意思，OK则代表一切正常

接着是响应体，Accept-Ranges代表的是支持存储的类型，这里支持的单位是bytes字节。ETag则表示当前我们的响应在整个服务器中的一个标识，就相当于是序列号。Last-Modified代表的是当前的资源在服务器中最后的修改时间。Content-Type代表的是我们响应的类型。Content-Length代表的是我们的响应的长度。Date代表日期。Keep-Alive代表我们连接的状态，timeout代表的是一个超时时间。Connection代表我们的连接状态，这里我们的链接状态时要保持连接的。

响应空行的作用与请求空行一样，都是代表一个换行

响应体里则是有相应返回给客户端的资源文件

同样在火狐浏览器里调用开发者工具就可以看到我们的响应体，在响应体窗口上面选择响应栏目，可以看到响应之后获得的原始的代码内容，这里我们就不演示了

最后我们可以做一个总结，首先是关于状态码的一些了解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE16fb15b53d991880a44268c78023c17f.png)

然后是关于响应头的知识总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa4008b4792354ccfb43d17925df2b8b7.png)

最后是响应空行和响应体

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4b060afe61766467643dfd53cda9bbff.png)

