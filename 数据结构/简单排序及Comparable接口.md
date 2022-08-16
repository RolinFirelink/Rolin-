关于简单排序，直接上图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd9dc50aac0228b23ccd55f51f0998da6.png)

这里我们特别提一下API，API（Application Programming Interface）即应用程序接口，是一些预先定义的函数，或指软件系统不同组成部分衔接的约定

我个人觉得可以简单理解就是已经封装好的可以供给我们使用的方法，后面我们学习的时候都是要顺便看看它的API内部的，也就是看看方法里面具体是怎么实现的



Comparable接口介绍

我们这里讲到了排序，排序的基本方法就是比较，Java中提供了一个Comparable接口用来定义排序规则，这里我们用一个案例来对Comparable接口进行简单的讲解

假设我们定义一个学生类，具有age和username两个属性，并通过Comparable接口提供比较规则

则我们可以先定义学生类如下

```javascript
package algorithm.sort;
//让学生类继承Comparable接口并使用泛型将Comparable接口的类型限定在Student里
public class Student implements Comparable<Student>{
    private String username;
    private int age;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "username='" + username + '\'' +
                ", age=" + age +
                '}';
    }
    
    @Override
    public int compareTo(Student o) {
        return this.getAge()-o.getAge();
    }
    //重写之后的比较方法两个简单的年龄相减
}

```

从上面的代码我们可以看出对于实现了Comparable接口类的类来说，是要重写compareTo方法的，具体重写方式按自己的需求来，比如我们这里是要判断其大小，则可以简单使用减法来进行比较

然后我们用测试类来试试

```javascript
package algorithm.sort;

public class test {
    public static void main(String[] args) {
        Student s1 = new Student();
        s1.setUsername("张三");
        s1.setAge(18);

        Student s2 = new Student();
        s2.setUsername("李四");
        s2.setAge(20);

        Comparable max = getMax(s1,s2);
        System.out.println(max);
        //Student{username='李四', age=20}
    }
    public static Comparable getMax(Comparable c1,Comparable c2){
        int result = c1.compareTo(c2);
        if(result>=0){
            return c1;
        }else {
            return c2;
        }
    }
}

```

在这里测试类里我们定义了返回类型Comparable引用数据类型的静态方法，然后里面调用了重写过的CompareTo方法，定义了比较返回age最大的比较结果的类型

最后我们也的确返回了我们所需要的对象，这就是我们Comparable一个接口的使用，未来我们所有的排序很多时候都要用Comparable对对象进行排序