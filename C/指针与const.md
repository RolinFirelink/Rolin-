

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE56399cbe8009825d65ec456624550652.png)

目前没有发现给同一个指针赋予另外一个地址的方法



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE42ed0669957759edbf97753e9d70df9b.png)

图上表明的意思是不可以通过指针去修改i，但i本身仍然可以修改，除非i本身也指明说过是const形式的变量



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa810ed70328c8e4c5de2f187c13d4cc2.png)

第一个与第二个的形式不同，但其表达的结果是一样的

const在*的前面表示其所指的东西不能被修改，在*后面表示指针不可被修改



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE37ea5a04ae93eb9fa9e155d2c4d36de2.png)

上图的意思是在函数里可以采用(const int*x)的方式，表示进入函数里的地址在函数内部不会再进行修改，无论原先的变量是否是const的类型的，都可以以这种形式进行转换



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaa0c9f5c536c3f1e9a7af63793dd3d8d.png)

一旦定义一个const类型的指针，对他进行初始化只能通过传统的方式嗯写



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEecb4e55f60a2b718c5db460234ab4f39.png)

该图与上文同理