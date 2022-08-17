学习完了Vue之后，接着我们来学习Element

# Element介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaf3b8e91f8c66f156cff74dc66b71014.png)

简单来说，就是使用Element不但可以快速搭建网站，而且搭建的网站还很好看，别人提供的样式看起来很顺眼，默认的按钮看着就尼玛小儿弱智

# Element快速入门

接着我们做一个Element的快速入门，目标就是在网页中打印出我们上一节中展示的案例

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0fb6fea1ac98d56cfacdd48c20488a7d.png)

首先我们要引入我们的对应的核心文件，这里需要我们的下载Element的核心库，并引入相关文件，我们这里都进行了对应了引入了

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>快速入门</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
</head>
<body>
<button>我是按钮</button>
<br>
<div id="div">
    <el-row>
        <el-button>默认按钮</el-button>
        <el-button type="primary">主要按钮</el-button>
        <el-button type="success">成功按钮</el-button>
        <el-button type="info">信息按钮</el-button>
        <el-button type="warning">警告按钮</el-button>
        <el-button type="danger">危险按钮</el-button>
    </el-row>
    <br>
    <el-row>
        <el-button plain>朴素按钮</el-button>
        <el-button type="primary" plain>主要按钮</el-button>
        <el-button type="success" plain>成功按钮</el-button>
        <el-button type="info" plain>信息按钮</el-button>
        <el-button type="warning" plain>警告按钮</el-button>
        <el-button type="danger" plain>危险按钮</el-button>
    </el-row>
    <br>
    <el-row>
        <el-button round>圆角按钮</el-button>
        <el-button type="primary" round>主要按钮</el-button>
        <el-button type="success" round>成功按钮</el-button>
        <el-button type="info" round>信息按钮</el-button>
        <el-button type="warning" round>警告按钮</el-button>
        <el-button type="danger" round>危险按钮</el-button>
    </el-row>
    <br>
    <el-row>
        <el-button icon="el-icon-search" circle></el-button>
        <el-button type="primary" icon="el-icon-edit" circle></el-button>
        <el-button type="success" icon="el-icon-check" circle></el-button>
        <el-button type="info" icon="el-icon-message" circle></el-button>
        <el-button type="warning" icon="el-icon-star-off" circle></el-button>
        <el-button type="danger" icon="el-icon-delete" circle></el-button>
    </el-row>
</div>
</body>
<script>
    new Vue({
        el:"#div"
    });
</script>
</html>
```

我们可以看到我们使用的标签el-row（行标签），el-button（按钮标签）其都遵循我们之前的所学习的标签英文的表示方式和规则的，无非就是多了一个el关键字罢了。最后我们要注意的是，如果我们希望我们的样式能正确加载，那么我们的Vue是比不可少的，因此我们在最后要需要创建对应的Vue对象，并获得我们的div属性，这样我们的属性才能正确展示

而我们的正文中的div标签的下的内容，其就是不断去引用类库中的内容罢了，这里就不赘述了。最后我们要注意的是，我们的核心文件是我们以后每次都要进行引入的

# 基础布局

首先我们来学习我们的布局方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3b4d8be65f85f577f457f338affd8342.png)

然后我们要注意的是，我们这里至多就分成24分，如果超越了， 那就直接不显示了

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>基础布局</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
    <style>
        .el-row {
            /* 行距为20px */
            margin-bottom: 20px;
        }
        .bg-purple-dark {
            background: red;
        }
        .bg-purple {
            background: blue;
        }
        .bg-purple-light {
            background: green;
        }
        .grid-content {
            /* 边框圆润度 */
            border-radius: 4px;
            /* 行高为36px */
            min-height: 36px;
        }
    </style>
</head>
<body>
<div id="div">
    <el-row>
        <el-col :span="24"><div class="grid-content bg-purple-dark"></div></el-col>
    </el-row>
    <el-row>
        <el-col :span="12"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="12"><div class="grid-content bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
        <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="8"><div class="grid-content bg-purple-light"></div></el-col>
        <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
    <el-row>
        <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
        <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
    </el-row>
</div>
</body>
<script>
    new Vue({
        el:"#div"
    });
</script>
</html>
```

# 容器布局

首先我们来看看我们的容器布局的方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb4fbccec9c7d21066a9e3c566c3ba14b.png)

然后我们来看看其布局规则

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEde3c8b855186caf05e827dd33f981e18.png)

那么我们可以写入我们的演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>容器布局</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
    <style>
        .el-header, .el-footer {
            background-color: #d18e66;
            color: #333;
            text-align: center;
            height: 100px;
        }
        .el-aside {
            background-color: #55e658;
            color: #333;
            text-align: center;
            height: 580px;
        }
        .el-main {
            background-color: #5fb1f3;
            color: #333;
            text-align: center;
            height: 520px;
        }
    </style>
</head>
<body>
<div id="div">
    <el-container>
        <el-header>头部区域</el-header>
        <el-container>
            <el-aside width="200px">侧边栏区域</el-aside>
            <el-container>
                <el-main>主区域</el-main>
                <el-footer>底部区域</el-footer>
            </el-container>
        </el-container>
    </el-container>
</div>
</body>
<script>
    new Vue({
        el:"#div"
    });
</script>
</html>
```

其实这里面关于这玩意的使用规则，我也不是整得很明白，但我们只是了解而已，所以即使不整明白也可以，这里就不多提了。

# 表单组件

接下来我们来看看我们的表单组件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1613aee04bfc9a4562fe44db3c019471.png)

表单组件本身有许多规则，其实都是我们之前学习过的，比如需要和内存里的数据绑定的部分我们是使用了双向绑定的

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单组件</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
</head>
<body>
<div id="div">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="活动名称" prop="name">
            <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="活动区域" prop="region">
            <el-select v-model="ruleForm.region" placeholder="请选择活动区域">
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="活动时间" required>
            <el-col :span="11">
                <el-form-item prop="date1">
                    <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1" style="width: 100%;"></el-date-picker>
                </el-form-item>
            </el-col>
            <el-col class="line" :span="2">-</el-col>
            <el-col :span="11">
                <el-form-item prop="date2">
                    <el-time-picker placeholder="选择时间" v-model="ruleForm.date2" style="width: 100%;"></el-time-picker>
                </el-form-item>
            </el-col>
        </el-form-item>
        <el-form-item label="即时配送" prop="delivery">
            <el-switch v-model="ruleForm.delivery"></el-switch>
        </el-form-item>
        <el-form-item label="活动性质" prop="type">
            <el-checkbox-group v-model="ruleForm.type">
                <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
                <el-checkbox label="地推活动" name="type"></el-checkbox>
                <el-checkbox label="线下主题活动" name="type"></el-checkbox>
                <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
            </el-checkbox-group>
        </el-form-item>
        <el-form-item label="特殊资源" prop="resource">
            <el-radio-group v-model="ruleForm.resource">
                <el-radio label="线上品牌商赞助"></el-radio>
                <el-radio label="线下场地免费"></el-radio>
            </el-radio-group>
        </el-form-item>
        <el-form-item label="活动形式" prop="desc">
            <el-input type="textarea" v-model="ruleForm.desc"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
    </el-form>
</div>
</body>
<script>
    new Vue({
        el:"#div",
        data:{
            ruleForm: {
                name: '',
                region: '',
                date1: '',
                date2: '',
                delivery: false,
                type: [],
                resource: '',
                desc: ''
            },
            rules: {
                name: [
                    { required: true, message: '请输入活动名称', trigger: 'blur' },
                    { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                ],
                region: [
                    { required: true, message: '请选择活动区域', trigger: 'change' }
                ],
                date1: [
                    { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
                ],
                date2: [
                    { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
                ],
                type: [
                    { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
                ],
                resource: [
                    { required: true, message: '请选择活动资源', trigger: 'change' }
                ],
                desc: [
                    { required: true, message: '请填写活动形式', trigger: 'blur' }
                ]
            }
        },
        methods:{
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    });
</script>
</html>
```

当然，可能回来复习的时候就看不懂了，没关系，看不懂算了，反正一开始也是只图个了解

# 表格组件

然后我们来看看我们的表格组件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdab54c96efedac24a6bf1070d50c59f0.png)

其规则如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd19da64970c2bc431aa6eb6e9f08c8af.png)

我们可以写入其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表格组件</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
</head>
<body>
<div id="div">
    <template>
        <el-table
                :data="tableData"
                style="width: 100%">
            <el-table-column
                    prop="date"
                    label="日期"
                    width="180">
            </el-table-column>
            <el-table-column
                    prop="name"
                    label="姓名"
                    width="180">
            </el-table-column>
            <el-table-column
                    prop="address"
                    label="地址">
            </el-table-column>

            <el-table-column
                    label="操作"
                    width="180">
                <el-button type="warning">编辑</el-button>
                <el-button type="danger">删除</el-button>
            </el-table-column>
        </el-table>
    </template>
</div>
</body>
<script>
    new Vue({
        el:"#div",
        data:{
            tableData: [{
                date: '2016-05-02',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }, {
                date: '2016-05-04',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1517 弄'
            }, {
                date: '2016-05-01',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1519 弄'
            }, {
                date: '2016-05-03',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1516 弄'
            }]
        }
    });
</script>
</html>
```

# 顶部导航栏组件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5e76d864af32ec058644444607971ed7.png)

我们来看看其规则及其提供的部件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1fa668bc916c3742ba8ec105f94aed38.png)

最后我们来看看我们的演示代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>顶部导航栏</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
</head>
<body>
<div id="div">
    <el-menu
            :default-active="activeIndex2"
            class="el-menu-demo"
            mode="horizontal"
            @select="handleSelect"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b">
        <el-menu-item index="1">处理中心</el-menu-item>
        <el-submenu index="2">
            <template slot="title">我的工作台</template>
            <el-menu-item index="2-1">选项1</el-menu-item>
            <el-menu-item index="2-2">选项2</el-menu-item>
            <el-menu-item index="2-3">选项3</el-menu-item>
            <el-submenu index="2-4">
                <template slot="title">选项4</template>
                <el-menu-item index="2-4-1">选项1</el-menu-item>
                <el-menu-item index="2-4-2">选项2</el-menu-item>
                <el-menu-item index="2-4-3">选项3</el-menu-item>
            </el-submenu>
        </el-submenu>
        <el-menu-item index="3" disabled>消息中心</el-menu-item>
        <el-menu-item index="4"><a href="https://www.ele.me" target="_blank">订单管理</a></el-menu-item>
    </el-menu>
</div>
</body>
<script>
    new Vue({
        el:"#div"
    });
</script>
</html>
```

# 侧边导航栏组件

最后我们来实现我们的侧边导航栏

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEedcc9947d9c359e0d04ef9be976a5ab0.png)

然后我们来看看规则

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd17d319c6d43a83f1f49ac5eb385fda6.png)

最后我们来看看其演示代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>侧边导航栏</title>
    <link rel="stylesheet" href="../element-ui/lib/theme-chalk/index.css">
    <script src="../js/vue.js"></script>
    <script src="../element-ui/lib/index.js"></script>
</head>
<body>
<div id="div">
    <el-col :span="6">
        <el-menu
                default-active="2"
                class="el-menu-vertical-demo"
                @open="handleOpen"
                @close="handleClose"
                background-color="#545c64"
                text-color="#fff"
                active-text-color="#ffd04b">
            <el-submenu index="1">
                <template slot="title">
                    <i class="el-icon-location"></i>
                    <span>导航一</span>
                </template>
                <el-menu-item-group>
                    <template slot="title">分组一</template>
                    <el-menu-item index="1-1">选项1</el-menu-item>
                    <el-menu-item index="1-2">选项2</el-menu-item>
                </el-menu-item-group>
                <el-menu-item-group title="分组2">
                    <el-menu-item index="1-3">选项3</el-menu-item>
                </el-menu-item-group>
                <el-submenu index="1-4">
                    <template slot="title">选项4</template>
                    <el-menu-item index="1-4-1">选项1</el-menu-item>
                </el-submenu>
            </el-submenu>
            <el-menu-item index="2">
                <i class="el-icon-menu"></i>
                <span slot="title">导航二</span>
            </el-menu-item>
            <el-menu-item index="3" disabled>
                <i class="el-icon-document"></i>
                <span slot="title">导航三</span>
            </el-menu-item>
            <el-menu-item index="4">
                <i class="el-icon-setting"></i>
                <span slot="title">导航四</span>
            </el-menu-item>
        </el-menu>
    </el-col>
</div>
</body>
<script>
    new Vue({
        el:"#div"
    });
</script>
</html>
```

- Element小结

最后我们可以来做一个总结，这里值得一提的是，我们上面的很多代码都是CV的，实际上很多代码本来就是这样，我们没必要自己写，去官网找就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE00502d1c6a74797c2060281b097d6f3d.png)

