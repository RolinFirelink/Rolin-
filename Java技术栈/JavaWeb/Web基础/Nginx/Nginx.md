# Nginx介绍

最后我们来学习Nginx服务器，首先我们来看看什么是Nginx服务器

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE845e081911138f1224e7b8575c6de668.png)

# Nginx安装

接下来我们来看看Nginx在Linux系统里的安装过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEded06708c1bb7e2aa4c2d58abaeee6c5.png)

完整的安装过程如下显示

------- 安装Nginx ------

1.上传安装包

2.解压安装包

3.进入Nginx目录

4.安装依赖环境

yum -y install pcre pcre-devel

yum -y install zlib zlib-devel

yum -y install openssl openssl-devel

5.安装Nginx

./configure

make

make install

安装后在/usr/local下就会有一个nginx目录

6.启动Nginx

cd /usr/local/nginx/sbin

启动

./nginx

停止

./nginx -s stop

重启

./nginx -s reload

7.查看服务状态

ps -ef | grep nginx

8.测试Nginx服务是否成功启动

http://ip地址:80

------- 发布项目 ------

1.创建一个toutiao目录

cd /home

mkdir toutiao

2.将项目上传到toutiao目录

3.解压项目

unzip web.zip

4.编辑Nginx配置文件nginx-1.17.5/conf/nginx.conf

server {

listen       80;

server_name  localhost;

```
#charset koi8-r;

#access_log  logs/host.access.log  main;

location / {
	root   /home/toutiao;
	index  index.html index.htm;
}

```

5.关闭nginx服务

./nginx -s stop

6.启动服务并加载配置文件

/usr/local/nginx/sbin/nginx -c /home/nginx-1.17.5/conf/nginx.conf

7.浏览器打开网址

http://192.168.203.138

# Nginx发布项目

最后我们来看看Nginx服务器发布项目的过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE13d88442fafacb30239d9ada4c103bcd.png)

当然实际上我发布项目的时候不能完全的依葫芦画瓢，起码IP地址得改改是吧

那么到此为止，我们的JavaWeb基础就学习完了