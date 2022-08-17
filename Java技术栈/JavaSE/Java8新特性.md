Java8新特性具有许多好处

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/java8.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/并行流.png)

# Lambda表达式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/Lambda.png)

光说肯定感受不到，下面来看看实际的例子

```java
public class AppTest
{
    @Test
    public void test1(){
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京天安门");
            }
        };

        r1.run();

        System.out.println("*****************************");

        Runnable r2 = () -> System.out.println("我爱北京故宫");

        r2.run();

        //普通写法
        Comparator<Integer> com1 = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1,o2);
            }
        };

        int compare = com1.compare(12, 12);
        System.out.println(compare);

        System.out.println("*****************************");

        //Lambda表达式写法
        Comparator<Integer> com2 = ((o1, o2) -> Integer.compare(o1,o2));

        int compare1 = com2.compare(32, 21);
        System.out.println(compare1);

        System.out.println("*****************************");

        //方法引用
        Comparator<Integer> com3 = (Integer::compare);

        int compare3 = com2.compare(32, 21);
        System.out.println(compare1);
    }
}
```

可以看到使用Lambda表达式写法是真的能方便非常多，真的太爽了，无论是从代码的编写还是阅读上，Lambda表达式都很舒服

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/lambda2.png)

我们这里重点来介绍下第四点，其是这里应该写为函数式接口的实例，只能用于实现我们的接口的实现具体实现中才能使用，而且要求接口要实现的抽象方法只有一个，如果有多个那这玩意就没得用了

而什么是函数式接口呢？其实只要我们的接口只声明了一个抽象方法，那么该接口就被称为函数式接口

具体的内容的写法都写在下面了，我们就不再赘述了

```java
public class AppTest
{
    @Test
    public void test1(){
        //语法格式一: 无参，无返回值
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京天安门");
            }
        };

        r1.run();

        System.out.println("*****************************");

        Runnable r2 = () -> System.out.println("我爱北京故宫");

        Runnable r3 = () -> {
            System.out.println("我爱北京故宫");
        };

        r2.run();
        r3.run();
    }

    //语法格式二:Lambda需要一个参数，但是没有返回值
    @Test
    public void test2(){
        Consumer<String> con = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        con.accept("谎言和誓言的区别是什么");

        System.out.println("*****************************");

        //Lambda表达式的省略大括号的写法,只有一句语法要执行时可省略大括号
        Consumer<String> con1 = (String s) -> System.out.println(s);
        //Lambda表达式的不省略大括号的写法,只有一句语法要执行时可省略大括号
        Consumer<String> con2 = (String s) -> {
            System.out.println(s);
        };
        //方法引用，一些满足条件的特殊情况下Lambda表达式可进一步简化成这样
        Consumer<String> con3 = System.out::println;
        //语法格式三：数据类型可以省略，因为可由编译器推断得出，称为"类型推断"
        Consumer<String> con4 = (s) -> System.out.println(s);
        //语法格式四：Lambda 若只需要一个参数，参数的小括号可以省略
        Consumer<String> con5 = s -> System.out.println(s);
        con1.accept("test1");
        con2.accept("test2");
        con3.accept("test3");
        con4.accept("test4");
        con5.accept("test5");

        //类型推断举例
        List<String> list = new ArrayList<>(); //泛型的新特性，也是类型推断之一

        int[] arr = {1,2,3}; //类型推断
    }

    //语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值
    @Test
    public void test6(){

        Comparator<Integer> com1 = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                System.out.println(o1);
                System.out.println(o2);
                return o1.compareTo(o2);
            }
        };

        System.out.println(com1.compare(12,21));

        System.out.println("**************************************");

        Comparator<Integer> com2 = (o1, o2) -> {
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        };

        System.out.println(com2.compare(31,21));

        System.out.println("**************************************");

        //语法格式六: 当 Lambda 体只有一条语句时，return与大括号若有，都可以省略
        Comparator<Integer> com3 = (o1, o2) -> o1.compareTo(o2);
        //方法引用在Lambda只有一条语句也有返回值时也可以使用
        Comparator<Integer> com4 = Integer::compareTo;

        System.out.println(com3.compare(31,21));
    }
}
```

最后我们来做个总结

左边的Lambda形参列表的参数类型可以省略（类型推断），如果Lambda形参列表只有一个参数，其一对()也可以省略

右边的Lambda体应该使用一对{}包裹，如果Lambda体只有一条执行语句（可能是return语句），可以省略这一对{}和return关键字

## 函数式接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/函数式接口.png)

Java给我们提供了内置的四大核心函数式接口![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/函数式接口2.png)

然后是对其中的两个接口进行举例运用的代码

```
public class AppTest
{
    //消费型接口Consumer<T> void accept(T t)
    @Test
    public void test1(){
        happyTime(500, new Consumer<Double>() {
            @Override
            public void accept(Double aDouble) {
                System.out.println("捡到了钱"+aDouble);
            }
        });

        System.out.println("********************************");

        happyTime(400,money -> System.out.println("捡到了钱"+money));
    }

    public void happyTime(double money,Consumer<Double> consumer){
        consumer.accept(money);
    }

    //断定型接口 Predicate<T> boolean test(T t)
    @Test
    public void test2(){
        List<String> list = Arrays.asList("北京","南京","东京","西京","普京","吴京");
        List<String> filterString = filterString(list, new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return s.contains("京");
            }
        });
        System.out.println(filterString);

        System.out.println("**************************************");

        List<String> filter = filterString(list,s -> s.contains("京"));
        System.out.println(filter);
    }

    //根据给定的规则，过滤集合中的字符串，此规则由Predicate的方法决定
    public List<String> filterString(List<String> list, Predicate<String> predicate){
        List<String> filterList = new ArrayList<>();
        for (String s : list) {
            if(predicate.test(s)){
                filterList.add(s);
            }
        }
        return filterList;
    }
}
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/函数式接口3.png)

## 方法引用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/方法引用.png)

```java
public class AppTest
{
    //情况一：对象 :: 实例方法
    //Consumer中的void accept(T t)
    //PrintStream中的void println(T t)
    @Test
    public void test1(){
        Consumer<String> con1 = str -> System.out.println(str);
        con1.accept("test");

        System.out.println("************************");

        PrintStream ps = System.out;
        Consumer<String> con2 = ps::println;
        con2.accept("test2");

        System.out.println("************************");

        Consumer<String> con3 = System.out::println;
        con2.accept("test2");
    }

    //Supplier中的T get()
    //Employee中的String getName()
    @Test
    public void test2() {
        Employee emp = new Employee(1001,"Tom",23,5600);

        Supplier<String> sup1 = () -> emp.getName();
        System.out.println(sup1.get());

        System.out.println("*****************************");
        Supplier<String> sup2 = emp::getName;
        System.out.println(sup2.get());
    }

    //情况二：类 :: 静态方法
    //Comparator中的int compare(T t1,T t2)
    //Integer中的int compare(T t1,T t2)
    @Test
    public void test3() {
        Comparator<Integer> com1 = (t1,t2) -> Integer.compare(t1,t2);
        System.out.println(com1.compare(12,21));

        System.out.println("***********************************");

        Comparator<Integer> com2 = Integer::compare;
        System.out.println(com2.compare(12,21));
    }

    //Function中的R apply(T t)
    //Math中的Long round(Double d)
    @Test
    public void test4() {
        Function<Double,Long> func = new Function<Double, Long>() {
            @Override
            public Long apply(Double aDouble) {
                return Math.round(aDouble);
            }
        };
        System.out.println(func.apply(12.9));

        System.out.println("******************************");

        Function<Double,Long> func1 = d -> Math.round(d);
        System.out.println(func1.apply(12.3));

        System.out.println("******************************");

        Function<Double,Long> func2 = Math::round;
        System.out.println(func1.apply(12.6));
    }

    //情况三：类 :: 实例方法 (有难度)
    //Comparator中的int compare(T t1,T t2)
    //String中的int t1.compareTo(t2)
    @Test
    public void test5() {
        Comparator<String> com1 = (s1,s2) -> s1.compareTo(s2);
        System.out.println(com1.compare("abc","abd"));

        System.out.println("**********************************");

        Comparator<String> com2 = String::compareTo;
        System.out.println(com2.compare("abd","abm"));
    }

    //BiPredicate中的boolean test(T t1,T t2)
    //String中的boolean t1.equals(t2)
    @Test
    public void test6() {
        BiPredicate<String,String> pre1 = (s1,s2) -> s1.equals(s2);
        System.out.println(pre1.test("abc","abc"));

        System.out.println("****************************");
        BiPredicate<String,String> pre2 = String::equals;
        System.out.println(pre2.test("abc","abc"));
    }

    //Function中的R apply(T t)
    //Employee中的String getName()
    @Test
    public void test7() {
        Employee employee = new Employee(1001, "Jerry", 23, 6000);
        Function<Employee,String> func1 = e -> e.getName();
        System.out.println(func1.apply(employee));

        System.out.println("*******************************");

        Function<Employee,String> func2 = Employee::getName;
        System.out.println(func2.apply(employee));
    }
}

class Employee{
    private int id;
    private String name;
    private int age;
    private int salary;

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", salary=" + salary +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return id == employee.id && age == employee.age && salary == employee.salary && Objects.equals(name, employee.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, age, salary);
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        this.salary = salary;
    }

    public Employee(int id, String name, int age, int salary) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
    }
}
```

## 构造器引用与数组引用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/构造器.png)

```java
public class AppTest
{
    //构造器引用
    //Supplier中的T get()
    //Employee的空参构造器: Employee()
    @Test
    public void test1(){
        Supplier<Employee> sup = new Supplier<Employee>() {
            @Override
            public Employee get() {
                return new Employee();
            }
        };
        System.out.println(sup.get());

        System.out.println("**********************************");

        Supplier<Employee> sup1 = () -> new Employee();
        System.out.println(sup1.get());

        System.out.println("**********************************");

        Supplier<Employee> sup2 = Employee::new;
        System.out.println(sup2.get());
    }

    //Function中的R apply(T t)
    @Test
    public void test2(){
        Function<Integer,Employee> func1 = id -> new Employee(id);
        Employee employee = func1.apply(1001);
        System.out.println(employee);

        System.out.println("*******************************");

        Function<Integer,Employee> func2 = Employee::new;
        Employee employee1 = func2.apply(1001);
        System.out.println(employee1);
    }

    //BiFunction中的R apply(T t,U u)
    @Test
    public void test3(){
        BiFunction<Integer,String,Employee> func1 = (id,name) -> new Employee(id,name);
        System.out.println(func1.apply(1001,"Tom"));

        System.out.println("*****************************");

        BiFunction<Integer,String,Employee> func2 = Employee::new;
        System.out.println(func2.apply(1002,"Tom"));
    }

    //数组引用
    //Function中的R apply(T t)
    @Test
    public void test4(){
        Function<Integer,String[]> func1 = length -> new String[length];
        String[] apply = func1.apply(5);
        System.out.println(Arrays.toString(apply));

        System.out.println("*********************************");
        Function<Integer,String[]> func2 = String[]::new;
        String[] apply2 = func2.apply(5);
        System.out.println(Arrays.toString(apply2));
    }
}

class Employee{
    private int id;
    private String name;
    private int age;
    private int salary;

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", salary=" + salary +
                '}';
    }

    public Employee() {
        System.out.println("无参构造器被调用");
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return id == employee.id && age == employee.age && salary == employee.salary && Objects.equals(name, employee.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, age, salary);
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        this.salary = salary;
    }

    public Employee(int id, String name, int age, int salary) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public Employee(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public Employee(int id) {
        System.out.println("id形参构造器被调用");
        this.id = id;
    }
}
```

最后我们要提一下关于构造器引用，其会自动引用不同的构造方法，但写法都是类名::new的形式，而具体引用什么构造方法取决于我们实现的抽象方法中有什么参数，当然，要保证不出错的前提是我们的内部的确提供了正确且合适的构造方法，连顺序都不能有不对应的地方才行，反之，如果没有合适的，或者是只有顺序不合适的，那么我们就只能用Lambda表达式来解决问题

简单来说，越简单的写法，可以应用的地方就越少，其灵活性就越小

# Stream流

## Stream API

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/stream2.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/stream.png)

那么最后我们来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/stream3.png)

## Stream的实例化

接着我们先来学习Stream的创建，Stream的创建首先可以通过集合来创建

首先我们定义了一个获得对应集合的静态方法和员工类

```java
class Employee{
    private int id;
    private String name;
    private int age;
    private double salary;

    public Employee(int id, String name, int age, double salary) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public static List<Employee> getEmployees(){
        List<Employee> list = new ArrayList<>();
        list.add(new Employee(1001,"马化腾",34,6000.38));
        list.add(new Employee(1002,"马云",12,9876.12));
        list.add(new Employee(1003,"刘强东",33,3000.82));
        list.add(new Employee(1004,"雷军",26,7657.37));
        list.add(new Employee(1005,"李彦宏",65,5555.32));
        list.add(new Employee(1006,"比尔盖茨",42,9500.43));
        list.add(new Employee(1007,"任正非",26,4333.32));
        list.add(new Employee(1008,"扎克伯格",35,2500.32));
        return list;
    }
}
```

然后我们来正式学习如何通过集合来创建Stream流

### 通过集合实例化

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/数组实例化.png)

下面是演示代码

```java
//创建 Stream方式一：通过集合
@Test
public void test1(){
    List<Employee> employees = Employee.getEmployees();

    //default Stream<E> stream() : 返回一个顺序流
    Stream<Employee> stream = employees.stream();

    //default Stream<E> parallelStream() : 返回一个并行流
    Stream<Employee> parallelStream = employees.parallelStream();
}
```

### 通过数组实例化

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/of.png)

```java
//创建 Stream方式二：通过数组
@Test
public void test2() {
    int[] arr = {1,2,3,4,5,6};
    //调用Arrays类的static <T> Stream<T> stream(T[] array): 返回一个流
    IntStream stream = Arrays.stream(arr);

    Employee e1 = new Employee(1001, "Tom");
    Employee e2 = new Employee(1002, "Jerry");

    Employee[] arr1 = {e1,e2};
    Stream<Employee> stream1 = Arrays.stream(arr1);
}
```

### Stream.of()

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/无限流.png)

创建代码如下

```java
//创建 Stream方式三： 通过Steam的of()
@Test
public void test3(){
    Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6);
}
```

### 无限流

该方式可以创建一个无限流，用得比较少，了解即可

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/集合实例化.png)

那么我们创建无限流的测试代码如下，这里为了能看到效果我们这里还一并写了停止条件

```java
//创建 Stream方式四: 创建无限流
@Test
public void test4(){
    // 迭代
    // public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)
    //遍历前十个偶数
    Stream.iterate(0, t ->t + 2).limit(10).forEach(System.out::println);

    // 生成
    // public static<T> Stream<T> generate(Supplier<T> s)
    Stream.generate(Math::random).limit(10).forEach(System.out::println);
}
```

第一个0代表的是提供的种子，具体有啥用我也不知道，总是写0总没错，而后面是使用Lambda表达式，本质上是要提供一个接口，然后自己定义需要返回什么对象

## Stream流的中间操作

### 筛选和切片

我们先来学习中间操作的第一部分，筛选和切片，其下提供了四个方法用于中间操作，如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/映射.png)

我们可以写入我们的测试代码如下，其中distinct()元素能发挥作用需要我们集合内存放的元素实现equals方法

```java
public class AppTest
{

    //1-筛选与切片
    @Test
    public void test1(){
        List<Employee> list = Employee.getEmployees();
        //filter(Predicate p)--接收 Lambda，从流中排除某些元素
        Stream<Employee> stream = list.stream();
        //练习：查询员工表中薪资大于7000的员工信息
        stream.filter(e->e.getSalary()>7000).forEach(System.out::println);

        System.out.println("******************************");
        //limit(n)--截断流，使其元素不超过给定数量
        //stream.limit(3).forEach(System.out::println);
        //上面的代码是会报错的，因为我们的流在结束之后只执行一次，再次调用流会抛出异常
        list.stream().limit(3).forEach(System.out::println);

        //skip(n) -- 跳过元素，返回一个扔掉了前n个元素的流，若流中元素不足n个，则返回一个空流，与limit(n)截断流互补
        System.out.println("******************************");
        list.stream().skip(3).forEach(System.out::println);

        //distinct()--筛选，通过流所生成元素的hashCode()和equals()去除重复元素
        System.out.println("******************************");
        list.add(new Employee(1,"test",1,1));
        list.add(new Employee(1,"test",1,1));
        list.add(new Employee(1,"test",1,1));
        list.add(new Employee(1,"test",1,1));
        list.add(new Employee(1,"test",1,1));
        list.add(new Employee(1,"test",1,1));
        list.stream().distinct().forEach(System.out::println);
    }
}
```

### 映射

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/排序.png)

下面是示例代码，map方法可以获得接受一个函数作为参数，将函数处理应用到每一个元素上并最终映射成一个新的的元素，具体请看练习1的示例

而flatMap方法可以接受一个函数作为参数，将流中的每个值都换成另外一个流，然后将所有流连接成一个流，我们这里定义了一个传入一个字符串返回一个流的方法，如果我们在上面的字符串集合中用该方法处理流，那么就会获得一个全新的流，该流存放了一个个返回的新流，但是没有自动连接成一个流，因此我们如果要看到里面的效果，就需要在结束方法中取出内部的每一个流再次调用一次结束方法打印流中内容才能看到

而我们的flatMap函数不但能实现同样的效果，还能令其变得更加间接，其下传入对应的返回流的函数，然后其会自动将流连接成一个流，然后我们可以打印该流

这里的区别其实就类似于集合中的add和addall方法，前者直接将整个集合当成一个对象添加，后者会取出其元素添加到集合中（可以这么理解，但本质的运行过程并不完全相同）

当然，我们自己需要提供一个返回Stream流的方法，要不然这个方法都无法启动

```java
    //映射
    @Test
    public void test2(){
        //map(Function f)--接受一个函数作为参数，将元素转换为其他形式或提取信息，该函数会被应用到每个元素上，并将其映射形成一个新的元素
        List<String> list = Arrays.asList("aa", "bb", "cc", "dd");
        list.stream().map(String::toUpperCase).forEach(System.out::println);
        System.out.println("****************************************");

        //练习1:获取员工姓名长度大于3的员工的名字
        List<Employee> employees = Employee.getEmployees();
        Stream<String> nameStream = employees.stream().map(Employee::getName);
        nameStream.filter(name -> name.length()>3).forEach(System.out::println);
        System.out.println("****************************************");

        //练习2:
        Stream<Stream<Character>> streamStream = list.stream().map(AppTest::fromStringToStream);
        streamStream.forEach(s->{
            s.forEach(System.out::println);
        });
        System.out.println("****************************************");

        //flatMap(Function f)--接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
        Stream<Character> characterStream = list.stream().flatMap(AppTest::fromStringToStream);
        characterStream.forEach(System.out::println);
    }

    //将字符串中的多个字符构成的集合转换为对应的Stream的实例
    public static Stream<Character> fromStringToStream(String str){
        List<Character> list = new ArrayList<>();
        char[] chars = str.toCharArray();
        for (char c : chars) {
            list.add(c);
        }
        return list.stream();
    }

    //为了便于理解flatMap的作用的类比方法
    @Test
    public void test3(){
        List list1 = new ArrayList();
        list1.add(1);
        list1.add(2);
        list1.add(3);

        List list2 = new ArrayList();
        list2.add(4);
        list2.add(5);
        list2.add(6);

        list1.add(list2);
        System.out.println(list1);

        list1.addAll(list2);
        System.out.println(list1);
    }
}
```

### 排序

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/归约.png)

这一节比较简单，直接看示例代码吧

```java
    //3-排序
    @Test
    public void test4() {
        //sorted()--自然排序
        List<Integer> list = Arrays.asList(12, 45, 65, 34, 87, 0, -98, 7);
        list.stream().sorted().forEach(System.out::println);

        //抛出异常，原因是Employee没有是吸纳Comparable接口
//        List<Employee> employees = Employee.getEmployees();
//        employees.stream().sorted().forEach(System.out::println);

        //sorted(Comparator com)--定制排序
        List<Employee> employees = Employee.getEmployees();
        employees.stream().sorted(Comparator.comparingInt(Employee::getAge)).forEach(System.out::println);
    }
```

## Stream流终止操作

### 匹配和查找

我们先来学习匹配与查找，给我们提供了五个方法![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/筛选.png)

都是比较简单的内容，直接看下面的示例代码即可

```java
//1-匹配与查找
@Test
public void test1(){
    List<Employee> employees = Employee.getEmployees();
    //allMatch(Predicate p)--检查是否匹配所有元素
    //练习：是否所有的员工年龄都大于18
    boolean allMatch = employees.stream().allMatch(e -> e.getAge() > 18);
    System.out.println(allMatch);

    //anyMatch(Predicate p)--检查是否至少匹配一个元素
    //练习:是否存在员工的工资大于10000
    boolean anyMatch = employees.stream().anyMatch(e -> e.getSalary() > 10000);
    System.out.println(allMatch);

    //noneMatch(Predicate p)--检查是否没有匹配的元素
    //练习:是否存在员工姓"雷"
    boolean noneMatch = employees.stream().noneMatch(e -> e.getName().contains("雷"));
    System.out.println(noneMatch);

    //findFirst--返回第一个元素
    Optional<Employee> employee = employees.stream().findFirst();
    System.out.println(employee);

    //findAny--返回当前流中的任意元素
    Optional<Employee> employee1 = employees.stream().findAny();
    System.out.println(employee1);

    //count--返回流中元素的总个数
    long count = employees.stream().filter(e -> e.getSalary() > 5000).count();
    System.out.println(count);

    //max(Comparator c)--返回流中最大值
    //练习:返回最高的工资
    Stream<Double> salaryStream = employees.stream().map(e -> e.getSalary());
    Optional<Double> max = salaryStream.max(Double::compare);
    System.out.println(max);

    //min(Comparator c)--返回流中最小值
    //练习:返回最低工资的员工
    Optional<Employee> employee2 = employees.stream().min(Comparator.comparingDouble(Employee::getSalary));
    System.out.println(employee2);

    //forEach(Consumer c)--内部迭代
    employees.stream().forEach(System.out::println);

    //使用集合遍历操作，因为我们这里连流都没创建
    employees.forEach(System.out::println);
}
```

### 归约

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/匹配.png)

```java
//归约
@Test
public void test3() {
    //reduce(T identity, BinaryOperator)--可以将流中元素反复结合起来，得到一个值，返回T
    //练习1：计算1-10的自然数的和
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    Integer sum = list.stream().reduce(0, Integer::sum);
    System.out.println(sum);

    //reduce(BinaryOperator)--可以将流中元素反复结合起来，得到一个值。返回Optional<T>
    //练习2:计算所有员工工资的总和
    List<Employee> employees = Employee.getEmployees();
    Stream<Double> salaryStream = employees.stream().map(Employee::getSalary);
    Optional<Double> reduce = salaryStream.reduce(Double::sum);
    System.out.println(reduce);
}
```

当然，这里出现了Optional类，这是我们后面要学习的内容，我们现在可以将其简单理解为一个容器，一个简单的装载数据的容器类即可

### 收集

最后我们来看Stream的终止操作的最后一个，也就是收集

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/收集.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/收集2.png)

```java
//3-收集
@Test
public void test4() {
    //collect(Collector c)--将流转换为其他形式，接收一个Collector接口的实现，用于给Stream中元素汇总的方法
    //练习1:查找工资大于6000的员工，结果返回一个List或Set
    List<Employee> employees = Employee.getEmployees();
    List<Employee> collect = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toList());
    collect.forEach(System.out::println);

    System.out.println("************************************************");

    Set<Employee> set = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toSet());
    set.forEach(System.out::println);
}
```

# Optional类

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/Option.png)

```java
/**
 * 为了在程序中避免出现空指针异常而创建的类
 */
public class OptionalTest {
    /**
     * Optional.of(T t) : 创建一个 Optional 实例，t必须非空
     * Optional.empty() : 创建一个空的Optional实例
     * Optional.ofNullable(T t) : t可以为null
     */
    @Test
    public void test(){
        Girl girl = new Girl();
        girl = null;
        //of(T t):保证t是非空的，若不是则会抛出异常
        Optional<Girl> optionalGirl = Optional.of(girl);
    }

    @Test
    public void test2(){
        Girl girl = new Girl();
        girl = null;
        //Optional.ofNullable(T t) : t可以为null
        Optional<Girl> optionalGirl = Optional.ofNullable(girl);
        System.out.println(optionalGirl);
        //orElse(T t):如果当前的Optional内部封装的t是非空的，则返回内部的t
        //如果内部的t是空的，则返回orElse()方法中的参数t1
        Girl girl1 = optionalGirl.orElse(new Girl("赵丽颖"));
        System.out.println(girl1);
    }

    //优化之前的方法，会报空指针异常
    public String getGirlName(Boy boy){
        return boy.getGirl().getName();
    }

    @Test
    public void test3(){
        Boy boy = new Boy();
        String girlName = getGirlName(boy);
        System.out.println(girlName);
    }

    //优化之后的方法
    public String getGirlName1(Boy boy){
        if(boy!=null){
            Girl girl = boy.getGirl();
            if(girl!=null){
                return girl.getName();
            }
        }
        return null;
    }

    @Test
    public void test4(){
        Boy boy = new Boy();
        boy=null;
        String girlName = getGirlName1(boy);
        System.out.println(girlName);
    }

    //使用Optional类的getGirlName()
    public String getGirlName2(Boy boy){

        Optional<Boy> boyOptional = Optional.ofNullable(boy);
        //此时的boy1一定非空

        Boy boy1 = boyOptional.orElse(new Boy(new Girl("迪丽热巴")));

        Girl girl = boy1.getGirl();

        Optional<Girl> girlOptional = Optional.ofNullable(girl);
        //girl1一定非空
        Girl girl1 = girlOptional.orElse(new Girl("古力娜扎"));
        return girl1.getName();
    }

    @Test
    public void test5() {
        Boy boy = null;
        boy = new Boy();
        boy = new Boy(new Girl("苍井空"));
        String girlName = getGirlName(boy);
        System.out.println(girlName);
    }
}
```