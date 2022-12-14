# IO流

我们的电脑上有硬盘和内存，硬盘里有文件，内存里也有文件，将硬盘上的文件传入到内存中的过程我们称之为读(Read)，又叫输入(Input)，输入的过程中必然会有数据的流动，在这个流动我们称之为输入流(InputStream)，反之如果要把内存中的文件传入到硬盘上，我们就叫写(Write)，又叫输出(Output)，所产生的流动的过程我们就称之为输出流(OutputStream)

最后我们我们说的IO流，其实是(Input Output Stream)的缩写，意为在内存和硬盘间互相读写文件的过程

最后值得一提的是通过IO可以完成对硬盘的读和写，然后IO流是只相对于内存而言的，往内存里写入文件就叫写，往内存里输出文件就叫读

## IO流的分类

IO流的分类有两种方式，第一种是按照输入输出的方式来分，往内存里输入叫输入流，往内存外输出叫输出流，这是按照输入输出的方式来分类的

第二种是按照读取数据的方式来分，有的流是按照字节的方式读取字符的，一次读取一个字节byte，等同于一次读取8个二进制位，这样的读取方式是什么文件都可以读取的，是万能的，我们将其称之为字节流

而有的流是按照字符的方式来读取数据的，一次只能读取一个字符，这种流的存在是为了方便读取普通文本文档的读取而存在的，也就是只能读取txt文档。这种流我们称之为字符流

## 流的四大家族

java.io流这块有四大家族

**java.io.InputStream 字节输入流**

**java.io.Outputstream 字节输出流**

**java.io.Reader 字符输入流**

**java.io.Writer 字符输出流**

直接看其类名最后的词，只要是以stream结尾的，都是字节流，只要是以reader或者是writer结尾的，都是字符流，通过英语自己分输入和输出

这四大家族的首领都是抽象类（abstract class）

## close与flush

java中所有的流都是实现了java.io.Closeable接口的，也就是说，所有的流都是有close();方法的，都是可关闭的，流说到底还是一个管道，我们使用完之后一定别忘了要将流关闭，否则会占用很多资源

所有的输入流都是实现了java.io.Flushable接口的，都是可刷新的，都有flush();方法，这个方法表示的意思是刷新管道，刷新表示的意思是将流中未输出的数据强制输出出去，我们在用完输入流之后只要记得使用flush();方法将剩余未输出数据输出出去，否则可能会导致数据丢失

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/流.png)

## FileInputStream

**java.io.FileInputStream**可以找到这个构造方法FileInputStream(String name),这个方法的作用是传入一个文件路径，这样来创建一个输入流

表示路径既可以用/，也可以用\来进行目录分割

FileInputStream类中有read()方法，可以从输入流中读取一个数据字节

当我们没有调用read()方法前，read的指针默认停留在其第一个字节的前一位，当我们调用之后，指针会指向a并取出a，返回a所代表的int类型的值，也就是97，继续调用就重复此过程，当调用到末位的时候，由于啥都没有了，所以会返回-1

通过构建while循环可以实现循环读，从而实现读入文件的数据

read()还有重砸方法，可以传入byte[]数组，来实现一次读入多份数据，同时调用String类里的byte数组转为String字符串里的构造方法，也就是String(byte[] bytes)，我们就可以将该byte数组转换为字符串输出，让每次转换的字符串长度是从0开始到读取到的长度为止，利用String类里的String(byte[] bytes,int offset, int length)重载方法完成目标

最终版本的代码如下，该版本需要记忆，以后均是使用该版本

```java
public class test {
    public static void main(String[] args) {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("此处存放文件地址");
            //先创建空间为4的byte数组
            byte[] bytes = new byte[4];
            int readCount = 0;
            while ((readCount = fis.read(bytes))!=-1){
                System.out.print(new String(bytes,0,readCount));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {//用finally语句进行流的关闭
            if(fis!=null){//若流不等于null则关闭流，若等于则无需关闭
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### **FileInputStream的其他方法**

int available()方法会返回文件中还未读取的元素数量，那么这个方法有什么用呢？一个简单的用法是可以预先读取文件中的元素的数量，然后直接创建适合它的byte数组的空间，然后我们只要一次读取就可以读取出全部文件内容了，但是要注意这个方法并不适用内容较多的文件，因为byte数组指定的空间不能太大

long skip(long n)方法可以跳过文件中指定的n个字节的数据不读取，从而去读取跳过数据之后的数据

## **FileOutputStream**

FileOutputStream是字节输出流，其作用则是将内存里的内容写入到硬盘中

其下的write(byte[] b)方法可以将指定的byte数组的内容全部写入到文件中

我们先创建了一个输出流，并给我们的文件指定了名字，但是没有具体指定位置，那么我们的文件就会默认生成在根目录中，接着我们自己创建了一个bytes数组，然后利用write();方法将bytes传入，再调用了指定位置的方法又写入了ab，那样我们的文件最终就会显示abcdab。当然，不要忘了写完之后要再调用fos.flush();方法刷新我们的输出流

但是问题是，new FileOutputStream()方法是会先将文件清空再写入的，这个方法也是比较危险的，因为他会先清空我们的文件内容，因此最好不调用，免得调用一次咱们重要的文件内容全无了那就直接裂开了

那如果我们是希望我们能够在文件中不断地写入我们的内容我们应该要怎么办呢？我们可以看看在FileOutputStream类里的FileOutputStream(String name,boolean append)构造方法

该构造方法就是能够实现我的目标的构造方法，调用它也很简单，直接在原先的构造方法上再加一个true就可以了

假设我们要传入一个写入一个中文字符串，那我们可以该字符串转换成byte数组，然后通过write方法传入就可以了

### 文件复制

假设我们要从D盘复制一份视频到C盘，那么我们实际的复制过程其实是先读取D盘中文件的数据到内存，然后内存再将这些数据写入到C盘

如果要在代码中实现复制，自然也应该要同时实现输入和输出，那么我们可以构造代码如下

```
public class test {
    public static void main(String[] args) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            //创建一个输入流对象，用于读取
            fis = new FileInputStream("需要复制的文件路径");
            //创建一个输出流对象，用于写入
            fos = new FileOutputStream("需要复制到的位置");

            //最核心的：一边读，一边写
            byte[] bytes = new byte[1024 * 1024];//1MB(一次最多拷贝1MB)
            int readCount = 0;//定义用于保存数量的变量
            while ((readCount = fis.read(bytes))!=-1){
                //判断符号的代码不但是判断条件，同时还执行了读的这一过程
                fos.write(bytes,0,readCount);
                //上一行代码主要是执行写的过程
            }

            //刷新，输出流最后一定要刷新
            fos.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭输入流
            if(fos!=null){
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //关闭输出流
            if(fis!=null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //之所以要分开写，是因为如果合起来写，如果第一个报了异常
            //那么第二个就不会执行了，那么第二个就不用关闭了，所以为
            //了安全起见所以我们要分开写
        }
    }
}
```

按照这个格式我们只要填入我们需要复制的文件和要复制到的位置然后运行该代码就可以了

## FileReader

字节的输入输出可以用于所有类型，虽然实用性广，但是效率方面上可能会有些不太高效，字符的输入输出虽然只可以用于txt的文本格式，但是其效率却会比较可观

FileReader的方法其实跟我们之前学习的FileInputStream类的方法是差不多的，唯一与之不同的是，在后者里我们是使用byte数组，而在前者，我们是要使用char数组

## FileWriter

FileWriter的类中查看其方法，发现啥方法都没有，那我们就去其父类的父类，Reader类中查看，能够发现其存在写入字符串的方法

分别用于写入单个字符，写入字符串，以及写入字符串的一部分，有了这三个方法，我们在文本里写入字符的时候就方便多了

**字符流只能读写文本，不能读写其他类型的内容**

## 带有缓冲区的字符流

### BufferedWriter

缓冲流有四个，但是我们这里重点将带有缓冲区的字符流，也就是java.io.BufferedReader和java.io.BufferedWriter

缓冲流的意思就是不用自动以数组用于读写的意思，比非缓冲流用起来要方便得多

### BufferedReader

进入BufferedReader类里，能看到其构造方法需要传入一个Reader对象，Reader是抽象类，因此传入其子类实现FileReader

**如果一个流的构造方法需要传入一个流的时候，被传入的流叫节点流，而接受流的流叫做包装流/处理流**

当我们需要关闭流的时候，只需要调用关闭包装流的close();方法就可以了，因为当我们调用外部包装流的close();方法之后，经过源码的层层递进，最终会在底层调用节点流的close();方法

BufferReader的方法，其中最重要的方法是这个String readLine();方法，该方法的作用是读取一个文本行并返回一个String类的对象，有了这个方法我们就可以一行一行地读取了，不用跟以前一样用数组一组一组地进行读取了

该方法如果读取到行的最后没有行可以读取了，那么其会返回null，知道了这个之后我们就可以构建关于这个方法的循环了

## 转换流

BufferedReader的构造方法是要求传一个字符流进去的，而如果我们创建了一个字节流，那我们将其传入的话编译器肯定是不会通过的，那此时我们就可以使用转换流来将字节流转换成字符流之后再传入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/Properties.png)

对于第一个转换流的方法而言，in是结点流，而reader是包装流，但是对于第二个缓冲流的方法而言，reader是节点流，而br是包装流

这些代码合并起来写到一起，括号内只需要填入我们所需要的读取的文件路径即可

## 数据流

利用数据流可以实现简单的加密操作，不过很少用，只作为了解

### DataOutputStream

同样当我们调用其构造方法时，需要传入一个流，需要传入一个OutputStream流进入，而正好该流是节点流，因此我们传入其子类FileOutputStream，选定一个文件路径即可

我们可以调用其下特定的写入数据的方法来写入对应的数据，写入的数据无法用txt文件打开进行查看

### DataInputStream

我们要从该文件读入数据的话，那么我们只能够使用java.io.DataInputStream来进行读入，而且我们必须按照我们之前写入的顺序来进行读入才能够顺利读入，否则是无法正常取出数据的

## 标准输出流

java.io.PrintStream是标准的字节输出流，默认输出到控制台，我们经常写的打印代码其实是直接调用了流中的println方法进行写入操作，然后其会默认输出在控制台上，因为System.out返回的是一个PrintStream的标准输出流

可以利用标准输出流来改写日志，先用代码指定了一个生成路径，然后利用System.SetOut();将输出方向修改至我们指定的log文件，这样我们下面输出的字符就会生成到我们指定的路径的文件上，再加入一些逻辑处理代码即可实现简单的日志记录

## File类的理解和常用方法

File类的直接父类是Object，其和四大家族没有什么关系，File也不是流，File就是文件和目录路径名的抽象描述，任何一个路径都可以是file

File(String pathname)方法可以传入一个路径来创造一个File对象

exists();方法可以用于判断指定目录中是否有该文件，若有则返回true，反之则返回false

createNewFile();方法可以在指定目录下创建指定的文件，可以通过前面的exists();方法联立使用，让我们指定的位置处如果没有指定文件那么就生成一个指定文件

mkdir();方法则是以目录的形式将文件生成出来，同样可以搭配exists();方法使用

使用mkdirs();方法还可以创建多重目录

getParent();方法可以获取文件的父路径，并且以字符串的形式返回

getParentFile();方法同样可以获得其父路径，但是此时是以File的形式返回的

getAbsolutePath();方法，可以获得文件的绝对路径

getName();方法，可以获取文件名并以字符串的形式返回

isDirectory();方法和isFile方法可以分别用于该文件是一个目录还是一个文件，前者判断是否是一个目录，后者判断是否是一个文件

lastModified();方法，调用该方法可以获取文件最后一次修改的时间，这个时间是以1970到最后一次修改的时间的毫秒数返回的，最好将其转换为便于程序员阅读的格式

先调用lastModified();方法获得总毫秒并将总毫秒传入日期对象的构造方法中创造一个日期对象，接着调用日期格式方法创造一个日期格式对象然后指定一个日期格式，接着将日期对象传入日期格式简化方法中获得一个字符串形式的日期接着将其打印就可以获得了

length();可以获取文件大小

File[] listFiles();方法会返回一个抽象路径名数组，并且数组里存放该目录下的所有文件的路径，当然，是File形式的，通过与其他方法和foreach循环的联立使用，我们可以获得该目录下的所有文件的文件名或绝对路径

## 序列化和反序列化

java对象保存在硬盘中过程是将java对象切分开来保存在硬盘中的，这是为了避免java对象太大而进行的处理方式，这个过程就需要使用到ObjectOutputStream方法，也就是对象输出流，这个过程我们称之为序列化

而反之则是将硬盘中切分的java对象文件复原到内存中，这里需要使用到对象输入流ObjectInputStream方法，这个复原的过程我们称之为反序列化

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/IO流.png)

要令对象序列化，必须令对线实现Serializable接口，其是标志性接口，其内部没有任何方法，其主要作用的是令实现其的类加入一个标志给JVM虚拟机看，JVM虚拟机发现其实现了可序列接口之后，会自动在对象内部生成一个序列化版本号，用来区分不同的类

先创建对象的原因是我们序列化的内容是对象，要想要序列化对象当然得先有个对象不是，调用ObjectOutputStream方法并创造对象输入流对象，然后传入一个FileOutputStream字节输出流对象并指定一个路径就完了，接着我们在调用其writeObject();方法就可以实现序列化了

实现反序列化先创建对象输入流并传入先前序列化的students文件，然后调用其readObject();方法就可以进行反序列化了，但要注意其返回的是一个学生对象，我们用Object承接自然没有问题，接着打印这个学生对象就会自动调用其tostring方法，我们可以再控制台正常看到学生信息就说明我们反序列化成功了，事实上我们也真的可以正常看到学生信息

一次序列化多个对象要借助集合，我们先将对象放入至集合中，然后将集合本身序列化就可以了

参与序列化的Arrarylist集合以及集合中的元素User都需要实现Serializable接口，否则会报异常

### transient关键字

transient关键字表示游离的，对于有该关键字修饰的变量，其不参与序列化操作的，如果说我们给我们的Student类的String name加上transient关键字修饰，那么最后我们反序列得到的结果会显示name会null，这是因为我们的name的数据并没有参与序列化，因此不会将数据传入到硬盘中导致的，而Student类中又有String name的变量，因此赋予其默认值null

### 序列化版本号

java虚拟机判断类是否相同的方法有两种，先进行类名上的判断看是否是同一个类，如果类名相同再判断其序列化版本号，如果序列化版本号相同则说明是同一类，反之则是不同的

**序列化版本号用于给JVM判断我们的定义的类是否是同一个类的**

假设我们对我们原来的学生类进行了代码上的修改，此时我们再进行反序列化的话，那么JVM就会抛出InvalidClassException异常，也就是无效的类异常。

这个异常的产生原因在于我们修改了学生类，JVM会对其进行重新编译，重新编译生成新的字节码文件的同时会产生新的序列版本号，与我们原先保存在硬盘上的序列化版本号是不一样的，这样我们用原来的文件进行反序列化时JVM会寻找对应的序列化版本号的学生类 ，显然它找不到，所以就会抛出这个异常，就是在告诉我们它找不到指定的类

假设有两个人在一个工程里面创造了两个名字完全相同的两个类，这时我们怎么区分？这时就要靠序列化版本号来区分，因为两个序列化版本号不一样，因此JVM虚拟机可以区分这二者

序列化版本号都是自动生成的，这也是有缺陷的，其中最大的缺陷就是我们不能对我们的源代码进行任何的改动，因为只要一改动那么我们的反序列过程就直接废了，但是在实际的开发需求中，我们总是有对同一个类进行修改的时候，那我们应该要怎么办呢？手动生成一个序列版本号可以解决这个问题

**只要我们对实现了Serializable接口的类提供一个固定不变的序列化版本号，这样即使这个类的代码修改了，但是版本号仍然不会改变，java虚拟机仍然会认为其实同一个类**

## IO和Properties文件联合使用

我们在IDEA中创建一个文件，命名为userinfo，随便写入一些键值对数据

Properties是一个Map集合，集合里有key和value

如果我们想要将userinfo里的内容直接写入到Porperties集合里我们该怎么做？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/序列化.png)

先创建一个字符输入流对象，其实字节输入流运行，将文件传入之后我们创建一个Properties的Map集合，然后调用集合里的load方法将文件传入即可将文件中的值对应传到集合里了，而且如果我们需要变化集合内的值，我们可以直接变换文档里的值就可以了，这样就省去了我们创建集合时往集合里添加元素的许多繁琐异常的方法，只要往文件里写入我们需要的值，然后利用IO流和Properties就可以直接将值传入了，而且可以随时修改

如果要获取文件中的内容的话，就用Properties对象中的getProperty();方法即可，将左值传入，就能够返回正确的=号右边的值了

对于经常改变的数据，我们可以单独写到一个文件中，用程序动态读取它

像类似于上面的将数据内容写入文件中然后在代码里直接读取添加到集合的文件被称为是配置文件，当其内容格式是key=value的时候，我们把这种配置文件叫做属性配置文件

而java规范中有要求，属性配置文件建议以.properties结尾。

且Properties是专门存放属性配置文件内容的一个类

- 在属性配置文件中，如果我们写入的内容的key相同，那么会覆盖value

- 在属性配置文件中，注释的符号是#

- 在属性配置文件中，=符号可以改成:号，但是我们不建议这样做

- 在属性配置文件中，我们建议=号左右不要加任何的空格

  ![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/IO.png)

  

  

  