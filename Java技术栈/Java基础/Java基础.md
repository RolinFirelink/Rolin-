# 方法

## 方法的介绍

使用方法可以让代码得到有效地重复利用

类似于C语言中的函数、

在Java中想要使用方法时，方法不需要提前进行声明，相当于是在类里面定义了一个新的方法，需要public [static] 返回类型名(不返回则填void) 方法名(数据类型 名字, ...){}的形式

在主方法里如果想要调用自己定义的方法，要以类名.方法名(传入数据,传入数据);的形式来进行调用

方法里面可以再调用方法，就如同C的函数里面可以再调用函数一样

只要是程序可以执行到的位置，都可以调用方法

## 方法调用何时可以省略类名

如果不在同一个类中，那自然就不能省略类型，否则编译器报错，因为编译器会默认搜索类中的方法

如果要在第一个类里调用其他类的方法，必须加上类名，否则报错。

同时如果要在不同类里面调用同名的方法，也必须加上类名，否则默认只调用第一个类

在有void的方法中，不能有返回值，但是可以有return;语句，这个语句在函数中存在主要作用是结束当前的该方法

方法的括号里面甚至可以什么都不写，即不传入任何的值，仍然可以正常运行

在不同的方法中，可以定义两个相同名词和类型的变量，他们不会有任何问题，因为他们在不同的空间里运行，但是不可以将方法传入的变量名再进行相同名字的重复定义

## 方法重载

方法重载(Overload)，对于多种功能相似的方法，方法重载能够使程序员再调用多个方法时就像在调用一种方法，不但更方便，也能够使代码美观

![方法重载](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/方法重载.png)

## 方法递归

简而言之就是，在方法里面继续调用同一个方法，方法递归要有结束条件，没有结束条件递归会不断运行，最后会发生栈内存溢出错误，JVM虚拟机停止、

**方法递归能不使用就不使用**

## 构造方法

java中的构造方法是对java中new一个对象的之所以然的解释，new后面所存在的东西，就是构造方法

构造方法的构建规则如下图所示![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/方法执行与内存分析.png)

1. 一个类中可以存在多个不同的构造方法，其可以构成方法重载
2. 如果类中没有提供任何构造方法，则会默认提供一个无参的构造方法，该机制称为**缺省构造器**
3. 如果类中存在构造方法，则缺省构造器将不再生效
4. 构造方法存在的意义有二，一是创建对象，二是初始化对象中的成员变量

![构造方法的作用](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/构造方法2.png)

当方法有static修饰符修饰的时候，用类名.方法名();调用

当没有static修饰符修饰的时候，用引用.方法名();调用，由于要用引用就得先有对象，因此有一个new的代码，其作用就是为了创建对象

## 构造方法、静态方法、实例方法

现在我们总结一下构造方法，实例方法和静态方法的异同

首先，它们三个都是方法，而且它们所传入的值所创造的都是局部变量

对于构造方法而言，它不需要填写返回值，也不用写static修饰符，我们通常是在类里面先定义好我们所需要的一个类的总体特征（简而言之就是先定义一个个不同类型的量）并对其进行封装，那么构造方法就是对这些量的一次赋值和修改，我们可以赋值，也可以不，还可以在构造方法里调用他们并进行对应的运算，无参的构造方法的存在主要是为了让用户能够在外部测试类中能够有办法调用构造方法

我们调用构造方法的方式就是new 方法名();

对于实例方法而言，我们需要填写返回值，返回值可以为空，但是我们不可以写入static修饰符，一旦写入了那么实例方法就成静态方法了，实例方法可以用于对封装的对象提供一个对外的入口，让外界有方法对实例变量进行相应的修改和读取，当然我们可以方法入口写入对应的安全保护代码，让用户在修改变量的时候有所限制，使得封装的对象处于安全状态，当然构造方法本身也可以做很多事情，比如打印或者是进行运算

我们调用实例方法的方式是引用.方法名();

对于静态方法而言，它就是带着static修饰符的，反正只要带了static修饰符的方法就是静态方法，静态方法的调用方式就是类名.方法名();

定义在类名后，方法前的变量是实例变量，定义在方法内部的变量是实局部变量

调用实例变量时是需要用引用.的方式的，也必须要有对象，而局部变量不用

**实例变量的存在时间是取决是类的存在与否的，而局部变量的存在时间则取决于方法的使用与否**

## 方法执行与内存分析

### Jvm特点

方法如果只定义不调用是不会执行的，在JVM中也不会给他分配空间

在JVM的划分上有三块主要的内存空间**方法区内存**、**堆内存**、**栈内存**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/构造方法.png)

栈内存具有以下特点

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/栈内存示意图.png)

### Jvm中方法的执行分析

字节码文件运行时先把class内容在方法区中执行，方法区中的方法虽然不重复存放，但是却可以重复调用

JVM虚拟机首先给主方法分配空间，主方法分配的空间首先进行压栈动作，当压栈动作执行之后执行方法内容，在内容中又调用了一个方法，则会给这个方法再分配空间，然后继续进行新的压栈动作，然后结束第二个方法的时候，发生弹栈动作，发生弹栈动作之后第二个内存空间释放出来，接着继续运行栈内存中的主方法，这是由于弹栈动作的发生，主方法所在的空间变成了栈顶，这时处于活跃状态，然后继续执行主方法，主方法结束后也发生弹栈动作，最后结束程序本身

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/栈内存特点.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/对象和引用的概念.png)

如果在主方法中定义一个int a=10;相当于是在主方法的分配存放在栈内存的空间里再分配一个空间，里面存储一个10，并且给这个变量取了名字，叫做a;

如果在主方法中继续调用一个方法，这个方法需要传入一个变量，此时将a传入，则在栈内存中会先分配新的空间给这个方法，同时这个方法空间内部也会分配一个空间，这个空间的取名就是其方法的变量名，而空间内部储存了传入的a的值

注意是传入值，即调用函数时是将值传入，空间是自己再创造的，相当于有两个克隆体，他们并不是等值的，实际他们的变量地址并不相同

在第二个方法中定义方法result，令a=result，则会在方法中创造新的空间，该空间会得到a的值，但他们的地址仍然是不相同的

令a=sum(b);方法会先执行，执行之后传回了值再创造空间，然后将b的值赋予给这个空间，接着将这个空间命名为a

**栈内存主要存放局部变量**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/栈内存特点2.png)

## 参数传递

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/栈内存中方法分析.png)

**成员变量又称为实例变量，new出的对象在堆内存中开辟空间**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/参数传递、.png)

## 方法重写

方法重载(Override)又称方法重写(Overwrite)，官方叫方法重载

**当父类的需求无法满足子类的时候，我们就有必要在对应的子类里进行方法重写，使得子类的需求能够被满足**

**方法重写是只能发生在父类与子类时间的**，换言之，如果没有extends继承语句，那么根本就不可能会有什么方法重写

方法重写的语句的写法是在子类写入和父类方法返回值类型相同，方法名相同，形参列表相同的方法，然后在方法里写入满足子类需求的语句就可以了

写方法重写要注意两点

1. 第一点是重写的方法访问权限不能更低，可以相等或更高。已知public是最高的访问权限，其他的都比它低，那么如果我们在父类里的方法用了public修饰，那么我们所继承的子类所要重写的方法都只能用public修饰了假如父类里写的是private，那么子类里可以用private，也可以用public，或者其他比他访问权限更高的

2. 第二点是重写的方法抛出异常不能更多，只能更少

最后提几点方法覆盖里要注意的事情

1. 私有方法不能继承，所以不能重写

2. 构造方法不能继承，所以不能重写

3. 静态方法不存在重写

4. 覆盖只针对方法，不能应用于属性

   ![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/方法重载2.png)

# 对象

## 面向对象和面向过程的区别

Java语言的核心机制就是**面向对象**，面向对象主要关注独立体能够完成什么功能，具有耦合度低，扩展力强的优点，更适合现实中复杂的业务逻辑，组件的复用性也强。

面向对象的三大方式分别是

- 面向对象的分析：OOA
- 面向对象的设计：OOD
- 面向对象的编程：OOP

## 类和对象的概念

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/类和对象的概念2.png)

类的具体定义如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/类和对象的概念.png)

下面是具体的学生类例子

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/类和对象的概念3.png)

接着运行这个文件会产生学生类，即使student.class供给于JVM使用

可以用类似于String的语句定义变量的方法来定义student，这时候student也是一个引用变量，只不过String的引用变量是sun公司编写的，而student是我编写的，本质上他们都是一样的

## 对象的创建和使用

如果变量定义在类体当中，方法体之外，那么这种变量称为成员变量，或者是实例变量

具有static关键字的变量称之为静态变量

不创建对象，对应变量的内存空间就不存在，只有创建了对象，对应变量内存空间才会被创建

new其实是一个运算符，其作用是创建一个对象，会在JVM的堆内存中开辟新的内存空间，注意是堆内存，不是栈内存

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/对象的创建和使用2.png)

在Student s = new Student()语句中，先执行new语句，在堆内存中先创造一个空间，在这个空间内也有各种空间，用于存放事先写在Student里的int，double，String类型的数据，先创造Student对象的大空间，然后在这个空间里会创造各种所需要的小空间并取名

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/对象的创建和使用3.png)

局部变量在栈内存中存储，而成员变量以及对象本身在堆内存的Java对象内部存储

在使用引用访问对象的实例变量的时候，可以在实例变量里再次使用引用访问另外一个对象，堆内存会开辟一个新的空间用于存放这个另外的对象。

相当于第一个堆内存的实例变量的空间里存放的是另外一个对象的地址，可以理解为其本身也变为了一个引用，有点类似于嵌套循环，这个过程中仍然需要使用new语句

但如果要在第一个对象的空间中放入一个String类型的数据，其本质也是再用一次引用，但是该对象是存放在方法区内存中的，使用这个语句并不需要new，直接令其="zifuchuan"就可以了，JVM内部会自动帮我们完成对象的创建并引用

下图是示例图，这里将String对象画到堆内存中了，自己心里知道其是在方法区内存中的即可

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/对象的创建和使用.png)

访问时必须要按顺序来访问，比如在这个程序里是由u起头进入到User对象，而在新的对象里才能访问到Address对象，因此如果要访问Address里的city等实例变量，语句的正确使用方法应该是u.addr.city

如果其原来的指引的地址不再使用了，可以直接在addr中再new一个address，直观理解为其搬家了，实际在程序里的意思是原来的对象不再有引用，被垃圾回收了，而在堆空间里会创造一个新的address对象并将其地址覆盖原来的地址

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/对象的创建和使用4.png)

# 封装、继承、多态

java面向对象有三大特征

—封装

—继承

—多态

## 封装

所谓封装就是私有的属性不能在外部直接访问，这就是封装

操作入口变成了只能通过set和get方法进行访问

在set方法和get方法执行过程中可以进行安全过滤

### 封装的好处

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/封装的好处.png)

### 封装的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/封装的步骤.png)

对于封装，要进行的第一步是用private关键字修饰需要进行封装的变量，同时提供getandset方法作为对外提供的访问接口

## 继承

继承的作用之一是可以实现代码复用，但其最重要的作用是在Java中只有继承之后才有**方法重写和多态机制**，

对于A extends B来说，把A类称之为子类，把B类称之为父类

java语言中只支持单继承，换言之就是一个类只能继承另外一个类，不可以同时继承两三个，但是在其他语言如C++中就支持多继承

子类继承父类，子类不可以直接调用父类的私有属性，因为这些属性被封装了，但是可以调用父类提供的getandset方法来获得父类中的属性，这相当于是子类中并没有继承父类中的这些属性，这些属性仍然只存在于父类对象中，如果真的继承了，则应该是子类和父类中各有一份这样的属性或者是方法

创建子类时会在堆内存中开辟对象所需的空间，其中其父类中拥有的成员变量也会一并在该对象的空间里开辟出来

在java中，任何一个类都默认继承了Object类，因此Object类是超级父类

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/继承.png)

## 多态

多态的基础是继承，多态只能发生在父类和子类之间，如果没有继承，那就没有多态一说

多态在Java中的最直观的使用是可以用父类型对象来表示子类型，即是父类型应用指向子类型对象

多态中我们分为向上转型和向下转型，都需要以继承为前提

向上转型就是子类型转换为父类型(upcasting)，又被称为自动类型转换

向下转型就是父类型转换为子类型(downcasting)，又被称为强制类型转换，需要加强制类型转换符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/多态.png)

Java程序分为编译阶段和运行阶段，运行阶段如果调用对象内的方法，则会调用真实对象的方法，但编译器会认为其对象是编译时指定的父类型对象，而非真实的子类型对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/static关键字.png)

父类型的引用，指向了一个子类型的对象，导致了编译阶段和运行阶段绑定了两种不同的对象，这种情况或者说这种机制，我们就称之为是多态

如果在子类中调用其继承父类的方法，其实际调用的还是子类的方法，相当于是子类和父类都有一份这样的代码，而不是调用父类的方法，如果非要调用父类的方法，可以使用super关键字或者是转型

**一般来说，当调用的方法是子类型中特有的，而父类型中不存在时，我们就要使用向下转型**

### instanceof运算符

假设Animal有子类Bird和Cat，分析下下面的代码为什么在编译时不会报错

```java
class Animal{

}

class Bird extends Animal{

}

class Cat extends Animal{

}

public class App {
    public static void main(String[] args) {
        Animal Cat = new Animal();
        Bird bird = (Bird) Cat;
    }
}
```

因为在编译阶段，编译器认为Cat是一个animal类型的引用，而不是bird类的，那么下面的强制类型转换的语句编译器会认为这是一个animal父类向cat子类转换的强制类型转换，所以编译不会报错

但是一旦运行就会发生异常，因为他们两者不能进行强制类型转换，控制台会输出java.lang.ClasssCastException类型转换异常

但是对于向上转型而言，只要它编译能够通过，那么在运行的时候就不会有问题

解决这个问题的方法是使用instanceof运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/静态代码块.png)

在进行任何的向下转换之前，都推荐使用instanceof语句来避免出现类型转换异常

多态的核心作用在于可以提高程序扩展力、降低程序耦合度

# Java中的关键字

## static关键字

在对应变量前加上static修饰符，其会随着类加载的时候在方法区内生成，调用的时候使用类名.方法名的方式就可以了正确调用了

为什么不把所有的方法都设置为静态方法？因为方法区的内存是有限的，只有一些用静态变量来代表的可以省略下更多空间的变量或者是几乎一定会使用到的方法我们才会将其设置会静态方法

**静态方法中无法直接访问实例变量和实例方法**，因为实例变量和实例方法是后于静态方法加载的，如果静态方法去访问这些方法，那么必然会发生空指针异常

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/多态2.png)

### 静态代码块

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/instanceof.png)

静态代码块是自上而下执行的，即使主方法在前面，实际运行程序的时候还是静态代码块先执行

**通常在静态代码块中完成数据的准备工具，比如初始化连接池或者是解析XML配置文件**

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/this.png)

## final关键字

final是一个修饰符，可以用在类，方法，和变量上，使用方法是在修饰符列表里加上final

被final关键字修饰的类不可以被继承

final修饰的变量一旦赋值之后就不可二次赋值，注意这里的一旦，比方说

final int a=10;这样的变量是表示a已经被赋值了的，那么这条语句之后a是不可以再被赋值的

final如果修饰实例变量的话，必须要是在修饰的同时完成赋值的语句才能够被编译器通过

如果用final修饰方法，那么被final修饰的存放内存地址的引用的空间里的内存地址不可被再次修改，同时被修饰的方法不可被重写

final关键字一般是用在什么时候呢？

- 当我们不希望某个变量被人修改时
- 我们希望用一个变量名来代表某个具体的数值时

用static和final修饰的变量我们称之为常量，常量的所有字符都要大写，如果中间有空格，那么我们应该用下划线（_）来隔开

## this关键字

this关键词本身是一个变量，就储存在对象在堆内存开辟的空间之中，在对象在堆内存中开辟空间之时，同时会在自己开辟的空间里开辟另外一个空间称之为this，这个this里面储存的是这个开辟的空间的内存地址，这个this是指向这个对象它自己的

每一个对象都有this，且this关键字在对象不同的时候其含义也不同，假设堆内存中有两个对象开辟的空间，当我们用第一个对象的时候，this所指的就是第一个对象的内存地址，但是当我们用第二个的时候，this就会指向第二个对象的内存地址

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/static关键字2.png)

平时我们直接调用类中的变量时，我们使用的都是一种省略的写法，即直接写变量名，而实际上完整的写法应该是写入类名.变量名的，但是由于类名指的就是其自身，因此省略

局部变量和成员变量可以重名，而为了将其进行区分，我们可以使用this关键字

同时this关键字还可以实现在一个类中的构造方法对另一个构造方法的简单调用

```
class Date{
    int year;
    int month;
    int day;

    public Date(int year, int month, int day) {
        this.year = year;
        this.month = month;
        this.day = day;
    }
    
    public Date() {
        this(1970,12,1);
    }
}
```

注意的是这种this()语法只能出现在构造函数的第一行，不能够重复出现，当然，调用的构造方法应该要实际存在

这种调用方式是sum公司提供给我们的一种调用机制

## super关键字

super的很多性质都与this一模一样，比如他们都只能出现在构造方法的第一行

super()是通过当前的构造方法区调用"父类"中的构造方法，存在的目的是为了在创建子类对象的时候，先初始化父类型特征，模拟的是现实里要有儿子就必先有父亲的情况，换言之，要想让子类运行，那么必须要先运行一次父类

所有的子类在进行初始化之前都要先调用super()方法，其意为调用父类的无参构造，先初始化父类，其后再初始化子类，若没有写入这个代码，则系统会默认提供，前提是你的父类里的确有无参构造器

无论是什么情况，父类的构造方法都一定会先于子类的构造方法执行完毕，且父类的方法空间一定比子类要在栈内存的更下面

this()和super()是不能同时出现的，它们中的任意一个都只能出现在构造方法的第一行，因此他们不可以调用两次及以上，而同时出现就会造成他们调用两次以上的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/super.png)

父类中如果只有有参构造方法，则继承的子类必须提供能够初始化父类构造方法的构造方法，其下调用super()方法来进行父类的初始化，如果有一些特殊的业务需求，比如需要将父类的对应属性进行初始化，那么也可以用上这种方式

```
class Date{
    private int year;
    private int month;

    public Date(int year, int month) {
        this.year = year;
        this.month = month;
    }
}

class myDate extends Date{

    private int day;
    
    public myDate(int year, int month,int day) {
        super(year, month);
        this.day=day;
    }
}
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/super2.png)

注意，虽然说调用了父类的构造方法，但实际上这个程序只创建了一个对象，这也是为什么只有一个this，因为只有一个对象

里面的开辟的空间部分只能说这个对象里有一定的空间用于存放父类的特征，但是这些特征子类已经继承过来了，是属于子类的特征了，空间上则呈现父类特征在子类对象中的特点

当父类型和子类型有同名变量的时候，为了区分，我们也不省略super关键字

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/访问控制权限修饰符.png)

**super不是引用，是一块具有父类对象特征的空间**

## 访问控制权限修饰符

关于访问控制权限修饰符

控制权限就是符其实就是，public，private，protected，缺省(default)这四种

访问控制权限修饰符可以修饰变量，方法以及类

我们通常用访问控制权限修饰符来控制元素的访问范围

public可以修饰变量，方法以及类

public表示公开的，在任何位置都可以访问

private不可以修饰类，其他都可以

private表示私有的，只能在本类中访问

protected表示不可以修饰类，其他都可以

被protected修饰的语句只能在同包或者是其子类的情况下访问

default表示缺省，可以修饰类，方法，以及变量

default在使用的时候，是什么都不写的，什么都不写就表示缺省，而不是说我们要加default来修饰它(内部类除外，但是我们还没有讲到这个知识点，所以不多提及)

缺省的语句只能在同包的情况下使用

他们的范围从大到小的排列是public，protected，缺省，private

还记得我们之前讲过的对于继承对象其范围只能越大不能越小吗？当时由于我们不知道访问控制权限修饰符的大小范围，所以只说了public和private，但是现在我们就有了确切的访问范围了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/super3.png)

# Java中的数据结构和管理内容

## 包和import

包存在的意义是为了方便管理，包的命名规范一般是公司域名倒序+项目名+模块名+功能名

包名不但要遵守标识符的命名规则，而且必须全部小写

一旦有了包名之后，我们要运行它就必须要把包名完整地写上去，并且加上类名，如果要创建的实例对象正好在同一个包下，那么可以将省略包名，若不是，则必须加上包名

我们可以使用import 语句，在包名下，类名前写上 import 包名.类名;，即可将给java类引入到指定类中，此时就可以省略包名了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/数组.png)

## 数组

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/import.png)

## 随机数

想要使用随机数，就要在头文件中加入import java.util.Random;

在方法中必须创建一个随机数对象

形式是Random 名称 = new Random();

没有特殊情况统一用下面的形式

Random r = new Random();

在调用随机数时，采用名称.nextInt(数字);的形式，其中的数字是随机的范围

比如 r.nextInt(10),表示要调出一个从0到9的随机数,将该数+1，可以获得1到10的随机数

这些随机数可以简单视为一个每次运行都会变化的变量，能够进行对应运算，也能够作为右值赋予给其他数

如何在令程序产生指定的随机数呢？

首先先定义两个int类型的max，min并赋予他们最大值和最小值，这个最大值最小值就是我们所需要的区间

接着我们的调用随机函数时用如下语句即可

r.nextInt(max)%(max-min+1) + min;即可返回我们所需要的随机数了

这个语句是实际意义是表示生成[0,max]之间的随机数，然后对(max-min+1)取模。

以生成[10,20]随机数为例，首先生成0-20的随机数，然后对(20-10+1)取模得到[0-10]之间的随机数，然后加上min=10，最后生成的是10-20的随机数

```
int max = 20;
int min = 10;
Random random = new Random();
int s = random.nextInt(max) % (max - min + 1) + min;
System.out.println(s);
```

