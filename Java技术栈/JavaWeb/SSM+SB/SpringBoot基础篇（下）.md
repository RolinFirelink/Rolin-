- 表现层数据一致性处理

接着我们来做表现层数据和后端返回数据的一致性处理，也就是进行后盾与前端的数据格式的统一，也被称为前后端数据协议。这个内容我们在SpringMVC中已经做过了，这里我们快速过一遍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5900ee471da9abb7b8587673ee3fd6e8.png)

首先我们创建对应的用于统一格式的类并提供其所有的构造方法

```
package com.itheima.controller.utils;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.omg.CORBA.PUBLIC_MEMBER;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class R {
    private Boolean flag;
    private Object data;

    public R(Boolean flag){
        this.flag=flag;
    }

    public R(Object data) {
        this.data = data;
    }
}

```

然后将我们的表现层返回的结果都封装到我们的统一格式类中并返回给前端人员就完了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc0d7d150ede39d0b02aefe7d5c634ebc.png)

- 前后端调用（axios发送异步请求）

因为这一节的内容又让我不得不回去把Vue和Element的内容给快速看完了，我真的佛了，横竖都跑不开这些内容。总之我们花了半天时间总算是迅速过了一遍了，那么现在我们就正式开始我们的学习吧

首先我们来看看我们的步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8fbbc27e1944337c97b90812fc2882ba.png)

这里我们要特别提一下的是，一般来说我们的单体工程是放在resourece下的static目录下的，同时一般我们的前端和后端是有两个不同的服务器的，不过我们显然没这么好的资源，所以我们这里就直接一个服务器就开整了，就不搞这么多有的没的了。最后我们这里还要提一下的是，如果我们的工程中出现了什么奇怪的问题，那么建议先clean一下重新编译试试，idea是有bug的，很多东西就是指不定重新编译下就好了

然后我们将对应的工程引入到对应的文件夹中，然后我们这里有已经先整好了的books.html文件，里面有提前构造好的函数和页面。我们利用其下的钩子函数create，只要vue初始化完成就必定会执行这个方法，这里调用我们的查询全部的方法，然后我们查询全部的方法里利用axios发送了一个get的异步请求，其请求的服务是查询去所有数据，然后利用then获得返回的结果接着在控制台上输出

```
//钩子函数，VUE对象初始化完成后自动执行
created() {
    //调用查询全部数据的操作
    this.getAll();
},

methods: {
    //列表
    getAll() {
        //发送异步请求
        axios.get("/books").then((res)=>{
            console.log(res.data);
            });
    },
```

实际上我们在控制台上和我们的idea的日志上都可以找到对应的数据，此时就说明我们的文件已经成功了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE17e7596ebaa259eb36e436c4d4573d38.png)

- 列表功能

想必大家也是很容易猜到，我们之后的要做的内容就是往我们的books.html中不断添加我们的功能。本节我们来实现列表功能，接着我们就要将我们的数据库的数据内容读取到我们的页面中去，我们通过分析很容易能知道，我们的页面展示的数据是通过dataList里的数据来展示的，因此我们的思路就是每次我们的Vue对象产生时，就自动查询全部数据并将对应的数据加到dataList中去，这样我们的页面就能正确展示我们的数据了

那么我们可以将我们的getAll的函数的代码修改如下

```
getAll() {
    //发送异步请求
    axios.get("/books").then((res)=>{
        // console.log(res.data);
        this.dataList = res.data.data;
        });
},
```

这里我们要注意，我们取出的数据是先从我们的res响应对象中取出其data数据，然后再取出其下真正保存数据的data对象，因为我们的数据一开始是被我们进行了封装的，因此我们取的时候也是要拆除掉那些封装，拿出我们真正需要的东西

然后这时我们页面上进行刷新，可以看到我们的数据真的被取出来了，此时就说明我们的列表功能就完成了

- 添加功能

接着我们来实现添加功能，我们的添加功能当然是从我们的案例中的添加按键中执行的，我们首先查看我的源代码的新建功能

```
<el-button type="primary" class="butT" @click="handleCreate()">新建</el-button>
```

可以看到其已经绑定了一个handleCreate()的事件，然后我们查看其事件，会发现这个事件函数啥也没写，那么我们就要从头开始构造这个功能了

首先我们的点击新建功能时，我们肯定希望其弹出一个添加窗口，而这个添加窗口其已经帮我们准备好了

```
<div class="add-form">

    <el-dialog title="新增图书" :visible.sync="dialogFormVisible">

        <el-form ref="dataAddForm" :model="formData" :rules="rules" label-position="right" label-width="100px">

            <el-row>

                <el-col :span="12">

                    <el-form-item label="图书类别" prop="type">

                        <el-input v-model="formData.type"/>

                    </el-form-item>

                </el-col>

                <el-col :span="12">

                    <el-form-item label="图书名称" prop="name">

                        <el-input v-model="formData.name"/>

                    </el-form-item>

                </el-col>

            </el-row>


            <el-row>

                <el-col :span="24">

                    <el-form-item label="描述">

                        <el-input v-model="formData.description" type="textarea"></el-input>

                    </el-form-item>

                </el-col>

            </el-row>

        </el-form>

        <div slot="footer" class="dialog-footer">

            <el-button @click="cancel()">取消</el-button>

            <el-button type="primary" @click="handleAdd()">确定</el-button>

        </div>

    </el-dialog>

</div>
```

这个添加窗口因为我们的一开始设置展示属性为false，因此其不展示。而我们可以看到里面的model属性定义了其对象为formData，且在下面都有对应的代码将用户提交的数据封装到这个对象里面，那么用户提交的数据就保存在formData对象里了

```
dialogFormVisible: false,//添加表单是否可见
```

那么我们可以再在我们的handleCreate函数中将该属性设置为true，这样其就可以展示了。同时我们每次进入添加页面时，我们都希望先清除掉预留在该窗口中的数据，因此我们让用户每次进入对应的添加窗口时就调用resetForm重置表单参数

```
//弹出添加窗口
handleCreate() {
    this.dialogFormVisible = true;
    this.resetForm();
},
```

可以看到重置表单的参数内容也很简单，就是将对应的对象内容给赋值为空而已

```
//重置表单
resetForm() {
    this.formData = {};
},
```

然后我们可以具体写入我们的添加和取消窗口的代码如下，我们这里利用axios向服务器发送post请求，我们这里同时将用户提交的数据也一并传过去了，接着我们获得其传过来的响应对象，然后先判断当前操作是否成功，若成功则关闭弹层并提示添加成功，若失败就提示添加失败，但无论结果如何，其都必须要重新加载一次我们的数据，这样可以让我们的新增的数据被正确展示，我们的这里重新加载数据的方式就再次调用我们的getAll函数就可以了

```
//添加
handleAdd () {
    axios.post("/books",this.formData).then((res)=>{
        //判断当前操作是否成功
        if(res.data.flag){
            //1.关闭弹层
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else{
            this.$message.error("添加失败");
        }
    }).finally(()=>{
        //2.重新加载数据
        this.getAll();
    });
},

//取消
cancel(){
    this.dialogFormVisible = false;
    this.$message.info("当前操作取消");
},
```

最后我们的当前操作取消的取消函数也是有设置对应的事情的，首先让弹窗关闭，然后提示当前操作取消

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5ec3b6e9ad8fe5dc385dbf3520e69c58.png)

最后有一点值得一提的是，我们的案例不知道为什么写那个逼代码写一半就直接废了，样式都不加载了，后面将问题代码注释掉也没有用，给我蚌埠住了，反正不是什么重要的项目，那就先这样吧

- 删除功能

接着我们来实现我们的删除功能，我们首先找到我们的删除功能绑定的对应事件的代码

```
<el-button type="danger" size="mini" @click="handleDelete(scope.row)">删除</el-button>
```

我们可以看到，我们的删除功能绑定了handleDelete的事件，并且将要删除的该行的数据都传入到事件中了，该对象中有该行的各种数据

然后我们进入删除方法的代码，写入其代码如下

```
// 删除
handleDelete(row) {
    this.$confirm("此操作永久删除当前信息，是否继续？","提示",{type:"info"}).then(()=>{
        axios.delete("/books",row.id).then((res)=>{
            if(res.data.flag){
                this.$message.success("删除成功");
            }else{
                this.$message.error("删除失败");
            }
        }).finally(()=>{
            //2.重新加载数据
            this.getAll();
        });
    }).catch(()=>{
        this.$message.info("取消操作");
    });
},

```

我们这里先进行一个确定，让用户进行一个确定选择，如果用户选择是，我们再执行后面的操作，也就是正式的删除操作，我们的删除操作需要传入要删除的行的id进去，用户进行删除，后面同样是进行数据的加载和取消，这些重复的代码就不提了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd0ad5e72a7fd5f12ff7dfaf958becd7c.png)

- 修改功能

接着我们来将我们的修改功能，我们首先来看看我们的修改功能绑定的事件，可以看到我们的修改功能绑定的事件是handleUpdate，同时其也向该函数中传入了当前的行对象

```
<el-button type="primary" size="mini" @click="handleUpdate(scope.row)">编辑</el-button>
```

然后我们可以写入其对应的代码如下，我们这里首先发送我们的异步请求，然后我们对返回的值进行了判断（因为可能存在另一个人先删除了数据的情况），然后如果其返回的flag为真且返回的数据不为空时，我们才执行编辑操作，之所以需要一个并的条件，是因为我们查询的语句如果查询不到对应的结果，那么就会返回空数据，但是查询过程本身是执行完毕的，只是没有数据而已，所以我们这里单纯去判断查询方法的成功是不行的，还需要将其返回的数据一起判断

```
//弹出编辑窗口
handleUpdate(row) {
    //发送异步请求
    axios.get("/books",row.id).then((res)=>{
        if(res.data.flag && res.data.data != null){
            this.dialogFormVisible4Edit = true;
            this.formData = res.data.data;
        }else {
            this.$message.error("数据同步失败，自动刷新");
        }
    }).finally(()=>{
        //2.重新加载数据
        this.getAll();
    });
},
```

然后如果其为真，我们就立刻打开编辑的窗口，接着将对应的响应的数据赋予到编辑窗口上，这里我们的编辑窗口的数据对象和添加窗口的数据对象是一样的，都是叫formData，这点其实也很好理解，能复用的东西就不新整一个，要不然不方便。然后我们同样也是让其总是会重新加载我们的数据

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0d0aac4ee0753669f90dae48749a28b9.png)

然后我们来实现我们的修改操作，修改操作绑定的世界是handleEdit，这个函数绑定的是put操作，我们往里面传入我们的数据，接着我们获得其返回对象，同样是进行对应的判断然后是加载数据，没了

```
//修改
handleEdit() {
    axios.put("/books",this.formData).then((res)=>{
        //判断当前操作是否成功
        if(res.data.flag){
            //1.关闭弹层
            this.dialogFormVisible4Edit = false;
            this.$message.success("修改成功");
        }else{
            this.$message.error("修改失败");
        }
    }).finally(()=>{
        //2.重新加载数据
        this.getAll();
    });
},
```

还有一点值得一提的是，我们点击取消的时候当然也需要将我们的编辑窗口也取消，这里我们就我们的取消代码将两个窗口都关闭就可以了，代码上这样写实际产生的效果里不会有什么问题，我们这么方便就这么写就行了

```
//取消
cancel(){
    this.dialogFormVisible = false;
    this.dialogFormVisible4Edit = false;
    this.$message.info("当前操作取消");
},
```

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE92fac47d48806a040aaf69568df0c1c7.png)

- 异常消息处理

此时我们还有事情没有做完，就是对异常消息的统一处理，我们在SpringMVC中学过，即使是抛出异常，我们也需要让我们返回给前台的格式保持统一，因此我们需要将我们的异常格式也进行统一

那么我们可以将我们的统一格式类里加一个String属性用于添加异常信息，然后提供其对应的构造方法，接着我们利用注解来构造我们的异常类并进行对应的处理，我们可以写入我们的代码如下

```
package com.itheima.controller.utils;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

//作为Springmvc的异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {

    //拦截所有的异常信息
    @ExceptionHandler
    public R doException(Exception e){
        //记录日志
        //通知运维
        //通知开发
        e.printStackTrace();
        return new R("服务器故障，请稍微再试");
    }
}
```

可以看到我们这里使用RestControllerAdvice令该类成为我们的异常处理类，这一节的知识如果遗忘了，可以回去看SpringMVC的笔记进行对应的复习。然后我们获得对应的异常处理对象之后将该异常信息打印到后台，然后返回前台一个告知的数据，这里要注意，一定要记住将对应的异常信息进行打印，要不然我们都看到有什么异常，那就完犊子了

然后我们返回给前台的错误提示的信息也不该是我们自己在页面上写的代码，而是我们在异常处理类里写的代码，因此我们可以将其代码修改如下

```
this.$message.error(res.data.msg);
```

接着我们要注意一件事，我们这里成功的时候是通过页面发送的成功信息，而失败的时候则是通过异常类发送的，这里前者是前端，后者是后端，这样一下子前端处理一下子后端处理是不好的，我们最好将其统一。统一的方式一般而言有两种，第一种是全部统一到前端，第二种是全部统一到后端，我们这里选择后者

那么我们就要让我们的后端返回的信息里要不但要返回执行成功的信息，还要返回执行失败的信息，那么我们可以将表现层的代码修改如下

```
@PostMapping
public R save(@RequestBody Book book){
    boolean flag = bookService.save(book);
    return new R(flag,flag ? "添加成功" : "添加失败");
}
```

可以看到我们这里先获得对应的结果，然后结合三目运算符来实现我们的需求，令其返回含有对应信息的对象，然后我们在页面中就全部返回响应对象里的这个字符串数据就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE82ca3b01b4a8894b2e19443480f60ecd.png)

- 分页

接着我们来制作我们的分页内容，首先我们我们的分页内容，其实简单来说，就是我们的页面一开始展示的就是我们分页查询的数据，这样就可以实现分页查询的效果了。

我们先来看看我们的分页查询的代码

```
<!--分页组件-->
<div class="pagination-container">

    <el-pagination
            class="pagiantion"

            @current-change="handleCurrentChange"

            :current-page="pagination.currentPage"

            :page-size="pagination.pageSize"

            layout="total, prev, pager, next, jumper"

            :total="pagination.total">

    </el-pagination>

</div>
```

可以看到上面有当前页码、数据总量等值，这些都是我们以后要用的，所以我们现在先进行对应的了解

```
pagination: {//分页相关模型数据
    currentPage: 1,//当前页码
    pageSize:10,//每页显示的记录数
    total:0//总记录数
}
```

同时我们可以看到我们在对应Vue的构造位置里也先行设定了其初始值

然后我们直接对我们的查询所有的方法进行改造，将其改造为一个分页查询的代码，那么我们可以将代码修改如下

```
//分页查询
getAll() {
    //发送异步请求
    axios.get("/books"+this.pagination.currentPage+"/+"+this.pagination.pageSize).then((res)=>{
        this.pagination.pageSize = res.data.data.size;
        this.pagination.currentPage = res.data.data.current;
        this.pagination.total = res.data.data.total;
        this.dataList = res.data.data.records;
    });
},
```

我们可以看到我们这里分页查询的代码是进行了一个字符串的拼接，发送的请求是我们要定位的页码和总页码数量，然后我们获得的响应对象里，我们将其查询到的分页结果赋我们这里设置的分页属性，然后最后将分页查询到的数据的具体内容赋给我们的展示的位置

然后我们要来设置我们的切换页码的函数

```
//切换页码
handleCurrentChange(currentPage) {
    //修改页码值为当前选中的页码值
    this.pagination.currentPage = currentPage;
    //执行查询
    this.getAll();
},
```

我们这里进行的设置很简单，就是对页码值进行一个修改，然后再次执行一个查询所有的函数，这个很好理解。

最后我们还要解决一个bug，那就是我们的目前的分页情况是，如果用户删除最后一页的最后一个数据，那么就会出现没有数据的情况。之所以会出现这样的情况，是因为我们的总页码此时只有2个了，而用户进行的查询却是从3那边查询的，相当于只有两页的数据，而用户却要查看第三页的数据，那自然是什么都没有了。

```
@GetMapping("{currentPage}/{pageSize}")
public R getPage(@PathVariable int currentPage,@PathVariable int pageSize){
    IPage<Book> page = bookService.getPage(currentPage, pageSize);
    //如果当前页码值大于总页码值，那么重新执行查询操作，使用最大页码值作为当前页码值
    if(currentPage>page.getPages()){
        page = bookService.getPage((int)page.getPages(),pageSize);
    }
    return new R(true,page);
}
```

解决这个问题的方法也很简单，我们直接在表现层里进行一个排查，如果当前页码比总页码还大，那么我们就令其重新查询，并以最大页码值为当前页码值，再将结果返回，这样就能正确的获得最后一个结果了

- 条件查询

然后我们来实现我们SB基础篇的最后一个内容，条件查询。

首先我们来查看下我们的条件查询的数据，我们会发现条件查询的数据有，但是没有绑定什么东西，也没有事先将数据封装到一个对象中，所以我们要自己动手做，将我们的数据封装到某些属性中，这样我们自己才可以在后续的内容中使用他

```
pagination: {//分页相关模型数据
    currentPage: 1,//当前页码
    pageSize:10,//每页显示的记录数
    total:0, //总记录数
    type: "",
    name: "",
    description: ""
}
```

这里我们将我们的属性保存到我们自己定义的type、name、description中，我们将这些数据保存在分页相关的模型对象中，我们保存在这里是因为我们的这些东西总是在分页查询的时候用得到的，所以我们将其定义在此中，实际上定义到其他位置也是可以的，没什么影响。

然后我们将对应的数据绑定到我们的对应的代码位置处

```
<div class="filter-container">
    <el-input placeholder="图书类别" v-model="pagination.type" style="width: 200px;" class="filter-item"></el-input>
    <el-input placeholder="图书名称" v-model="pagination.name" style="width: 200px;" class="filter-item"></el-input>
    <el-input placeholder="图书描述" v-model="pagination.description" style="width: 200px;" class="filter-item"></el-input>
    <el-button @click="getAll()" class="dalfBut">查询</el-button>
    <el-button type="primary" class="butT" @click="handleCreate()">新建</el-button>
</div>
```

可以看到我们这里将对应的代码绑定到了我们的图书类别等具有输入标签的代码中，这样我们用户输入的数据就会被正确保存到我们的预先设定的属性中了

然后我们在我们的分页查询之前，我们先定义一个param参数，接着拼接对应的字符串，最后将该字符串传入到我们的地址中

```
//分页查询
getAll() {

    //组织参数，拼接url请求地址
    let param;
    param = "?type"+this.pagination.type;
    param += "&name"+this.pagination.name;
    param += "&description="+this.pagination.description;

    //发送异步请求
    axios.get("/books"+this.pagination.currentPage+"/+"+this.pagination.pageSize+param).then((res)=>{
```

这里我们值得一提的是，可能会出现用户啥也不传入的情况。此时我们的出现的问题就是，如果用户传入的数据为空，那么我们要不要将对应的数据作为数据传入？这其实传不传都行，不传的话他最多就是将数据传入为空而已，不会对我们的业务有什么实际的影响。实际上我们解决这个问题一般是使用swagger工具类，不过这是我们SB案例之后要学习的内容了，我们这里先按下不表、

然后我们进入到数据层中，先创建对应的新的数据层接口，然后实现该方法，我们往里面直接传入一个Book对象，其会自动将数据封装到该对象中，之所以其会这样，是因为在SpringMVC中，当我们传入参数和实体类的类名一致时，其会将数据自动注入到我们的参数中（这里如果忘了这些知识，可以自己去进行单独的复习，我们这里的内容实际上也是推测的，不保证对，因为我也忘得七七八八了）

```
@Override
public IPage<Book> getPage(int currentPage, int pageSize) {
    IPage page = new Page(currentPage,pageSize);
    bookDao.selectPage(page,null);
    return page;
}

@Override
public IPage<Book> getPage(int currentPage, int pageSize, Book book) {
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<>();
    lqw.like(Strings.isNotEmpty(book.getType()),Book::getType,book.getType());
    lqw.like(Strings.isNotEmpty(book.getName()),Book::getName,book.getName());
    lqw.like(Strings.isNotEmpty(book.getDescription()),Book::getDescription,book.getDescription());
    IPage page = new Page(currentPage,pageSize);
    bookDao.selectPage(page,null);
    return page;
}
```

然后我们在其下利用LambdaQueryWrapper对象进行对应的模糊查询，然后我们这里设置的查询是，当其传入的参数不为空时我们再执行对应的查询条件，同样，忘记了这个对象怎么用的记得回去复习

最后我们经过测试会发现我们这个方法是可以实现的

- 基础篇总结

那么到这里我们的基础篇就正式完结了，我们最后来做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE19b7ae069ff92246a7d762b8fd2f7652.png)

