---
typora-root-url: JVM
---

# JVM与java体系结构

## java的体系结构

![image-20230106185117785](/../JVM.assets/image-20230106185117785.png)

## JVM

虚拟机就是一台虚拟的计算机。他就是一款软件，用来执行一系列虚拟计算机指令。大体上虚拟可以分为**系统虚拟机**和**程序虚拟机**。**吞吐量优先**

系统虚拟机：Visual Box、VMWare，完全是对物理计算机的仿真

程序虚拟机：JVM，专门为执行单个计算机程序而设计

### 从跨平台的语言到跨语言的平台

- java是一个跨平台的语言

![image-20230106185637569](/../JVM.assets/image-20230106185637569.png)

- jvm是一个跨语言的平台
  - java虚拟机平台上运行非java语言编写的程序
  - 只要能编译成class文件，就可以在虚拟机上运行
  - jvm和java是没有关系的，之和class文件有关系
- jvm是一种规范 
  - https://docs.oracle.com/javase/specs/index.html
- jvm是一个虚构出来的计算机 
  - 字节码指令集
  - 内存管理：栈、堆、方法区等
- java虚拟机就是二进制字节码的运行环境

**java不是最强大的语言，但是JVM是最强大的虚拟机**

各种语言之间的交互不存在任何困难，就像使用自己语言的原生API一样方便，因为他们始终都运行在一个虚拟机上

### 常用的JVM实现

- Sun Classic VM
  - 是世界上第一款商用的java虚拟机
  - 在内部只提供了解释器，没有JIT（即时编译器）
- Exact VM
  - 准确式的内存管理：可以知道内存中某个位置的数据具体是什么类型
  - 具备现代高性能虚拟机的雏形：热点探测、编译器与解释器混合工作模式
  - 最终被Hstspot虚拟机替代
- Jrocket
  - 专注于服务器端应用
  - 不太注重程序启动速度，因此内部不包含解析器，全部代码都是靠及时编译器编译后执行
- J9
  - 广泛用于IBM的各种java产品中
- Azul VM
  - 与特定硬件平台绑定、软硬件配合的专有虚拟机
  - 每个Auzl VM实例都可以管理至少数十个CPU和数百个GB内存的硬件资源，并提供在巨大内存范围内实现可控的GC时间的垃圾收集器、转悠硬件有花的线程调优等优秀特性
- Liquid VM
  - Liquid VM不需要操作系统的支持，或者说让它自己本身实现了一个专用操作系统的必要功能，如线程调度、文件系统、网络支持等。
- Taobao VM
  - 基于openJDK开发的自己的定制版本的AlibabaJDK
  - 深度定制且开源的高性能服务器
  - 即将生命周期较长的java对象从heap中移到heap之外，并且GC不能管理GCIH内部的java对象，以此达到降低GC的回收频率和提高GC的回收效率的目的
  - GCIH中给的对象还能够在多个java虚拟机进程中实现共享
  - 硬件严重依赖Intel的cpu，损失了兼容性，但是提高了性能（凡是和操作系统、硬件耦合高的，性能都强）
- Graal VM
  - 最有可能取代Hotspot的虚拟机
  - 跨语言全栈虚拟机，可以作为“任何语言”的运行平台使用

![image-20230106190830284](/../JVM.assets/image-20230106190830284.png)

- **常用的是Hotspot**
  - Hotspot占有绝对的市场地位
  - 大多数默认都是Hotspot，相关机制也主要指Hotspot的GC机制。（J9、Jrockit都没有方法区的概念）
  - 名称中的Hotspot指的是他的热点代码探测技术
    - 通过计数器找到最具编译价值代码，触发**即时编译器**或栈上替换
    - 通过编译器与解释器协同工作，在最优先的程序响应时间与最佳执行性能中取得平衡
    - 解释器：最优先的响应时间——速度快（假设去旅游，不管什么方法，即刻出发，只走路）
    - 编译器：最佳的执行性能——方法（假设去旅游，只考虑方法，只坐公交车，不走路）

![image-20230106191315638](/../JVM.assets/image-20230106191315638.png)

### JVM的位置

JVM试运行在操作系统之上的，他与硬件没有直接的交互

![image-20230106191326582](/../JVM.assets/image-20230106191326582.png)

### JDK、JER、JDK

![image-20230106191406149](/../JVM.assets/image-20230106191406149.png)

![image-20230106191415590](/../JVM.assets/image-20230106191415590.png)

- JDK：（Java Development Kit）java开发工具包（开发所用的包）
- JRE：（Java Runtime Envirment）java运行环境（核心类库，Object、String……）
- JVM:（Java Virtual Machine）java虚拟机

### JVM的整体结构

![image-20230106191521968](/../JVM.assets/image-20230106191521968.png)

多个线程共享方法区和堆

栈、本地方法栈、程序计数器是每个线程独有一份的

### java代码的执行流程

![image-20230106192056378](/../JVM.assets/image-20230106192056378.png)

xx.java文件通过javac编译得到xx.class文件。当执行java命令的时候，.class文件会被ClassLoader（类加载器）装载到JVM中。装载后，通过调用字节码解释器、JIT即时编译器对代码进行解释和编译，编译过后用执行引擎进行执行，与OS硬件交互。

所用到的类库，会在class文件被加载到 JVM的同时，也被装载到类加载器中。

常用的代码会在第一次编译后被即JIT做成本地编译，不会一次次的去进行解释和即时编译。

如果每一句代码都要进行解释和编译，java就无法做到跨平台了

### JVM的代码模型

java编译器输入的指令流基本上是一种基**于栈的指令集框架**，另外一种指令集架构则是基于**寄存器的指令集架构**。 

- 基于栈式架构的特点（八位为一个单位）
  - 设计和实现更简单，适用于资源受限的系统;
  - 避开了寄存器的分配难题:使用零地址指令方式分配。
  - 指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。指令集更小，编译器容易实现。
  - 不需要硬件支持，可移植性更好，更好实现跨平台
- 基于寄存器架构的特点（十六位为一个单位）
  - 典型的应用是x86的二进制指令集:比如传统的PC以及Android的Davlik虛拟机。
  - **指令集架构则完全依赖硬件，可移植性差**
  - **性能优秀和执行更高效;**
  - 花费更少的指令去完成一项操作。
  - 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令、二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主。

由于跨平台性的设计，Java的指令都是根据栈来设计的。

栈：跨平台性、指令集小、指令多：执行性能比寄存器差

### JVM的生命周期

- 虚拟机的启动
  - Java虚拟机的启动是通过引导类加载器(bootstrap class loader) 创建一个初始类(initial class) 来完成的，这个类是由虚拟机的具体实现指定的。

- 虚拟机的执行
  - 一个运行中的java虚拟机有着一个清晰的任务：执行java程序
  - 程序开始执行时他才运行，程序结束时他就停止
  - **执行一个所谓的java程序的时候，真真正正执行的是一个叫做java虚拟机的进程**
- 虚拟机的退出
  - 有如下几种情况：
    - 程序正常执行结束
    - 程序在执行过程中遇到了异常或错误而异常结束
    - 由于操作系统出现错误而导致java虚拟机进程终止
    - 某线程调用Runtime类或System类的exit方法，或Runtime类的halt方法，并且java安全管理器也允许这次exit或halt操作
    - 除此之外，JNI规范描述了用JNI Invocation API来加载或卸载java虚拟机时，java虚拟机的退出情况

# 类加载子系统

##  内存结构

![image-20230106194714883](/../JVM.assets/image-20230106194714883.png)

当自己手写一个java虚拟机的话，主要考虑哪些结构呢？

类加载器和执行引擎

### 方法的实现

1.首先利用获取到的16进制码去汇编表中查找对应数字所代表的含义

![image-20230106200322490](/../JVM.assets/image-20230106200322490.png)

![image-20230106200340261](/../JVM.assets/image-20230106200340261.png)

![image-20230106200349409](/../JVM.assets/image-20230106200349409.png)

查找此汇编语言的含义

![image-20230106200427883](/../JVM.assets/image-20230106200427883.png)

2.查询到是哪条指令后，再将这条指令翻译过来

3.查询下一个b7-->invokespecial，重复上述操作

读到2a的时候，this压栈，之后读取下一条指令，直到b1

#### 注意

2a是开始，b1是结束

![image-20230106200724972](/../JVM.assets/image-20230106200724972.png)

这五个字节是文件的构造方法的具体实现

2a是aload-0，表示把this压栈，表示整个语句的开始

然后再编译b7……

01表示常量池里的第一项，java.lang.Object

b1表return，是整个语句的结束

### 实例2

代码1：

![image-20230106200814485](/../JVM.assets/image-20230106200814485.png)

实现过程：

![image-20230106200826551](/../JVM.assets/image-20230106200826551.png)

先加载父类的构造方法，之后再将自己的成员变量初始化

0：把this压栈

3：第二次压栈，把变量压入栈，后赋值

## 类的加载器

![image-20230106200911583](/../JVM.assets/image-20230106200911583.png)

![image-20230106200931416](/../JVM.assets/image-20230106200931416.png)

Loading：将二进制文件，加载到内存

Linking-verification：判断是否符合class文件的标准 cafe babe

Linking-perparation：给class静态变量赋默认值

Linking-resolution：class文件用到的符号引用，转换为直接能访问到的内容（内存地址）

Initializing：静态变量赋值初始值

### 类的加载过程

![image-20230106201505037](/../JVM.assets/image-20230106201505037.png)

### Loading

![image-20230106201538652](/../JVM.assets/image-20230106201538652.png)

1.通过一个类的全限定名获取定义此类的二进制字节流

2.将这个字节流所代表的静态存储结构转化为元空间（方法区）的运行时数据结构

3.**在内存中生成一个代表这个类的java.lang.Class对象**，作为方法区这个类的各种数据的访问入口

### Linking

![image-20230106203239050](/../JVM.assets/image-20230106203239050.png)

#### 初始化过程

- Linking——Verification
  - 验证文件是否符合JVM规定 
- Linking——Preparation
  - 为静态成员变量分配内存并且设置该类变量的默认初始值
  - 这里不包含用final修饰的static，因为final在编译的时候就会分配了，准备阶段会显示初始化
  - 这里不会为实例变量分配初始化，静态变量会分配在方法区中，而实例变量是会随着变量一起分配到java堆中
- Linking——Resolution
  - 将类、方法、属性等符号引用解析为直接引用
  - 常量池中的各种符号引用解析为指针、偏移量等内存地址的直接引用
  - 解析操作往往会伴随着JVM在执行完初始化之后再执行
  - 解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等……
- Initializing
  - 调用类初始化代码，给静态成员变量赋初始值

**实例1：**

![image-20230106210408355](/../JVM.assets/image-20230106210408355.png)

运行结果：3

运行过程：调用T.count时，首先将T.class通过app加载到内存，再经过Verification进行校验，之后再通过Preparation对T里面的静态成员变量赋默认值，此时count是空0，t是空值，再进行Resolution，之后再通过Initializing对静态成员变量赋初始值，此时count是2，之后t会被赋值new T()，对count++，因此count变为3。

**实例2：**

![image-20230106210429513](/../JVM.assets/image-20230106210429513.png)

运行结果：2

运行过程：调用T.count时，首先将T.class通过app加载到内存，再经过Verification进行校验，之后再通过Preparation对T里面的静态成员变量赋默认值，此时t是空值,，count是0，再进行Resolution，之后再通过Initializing对静态成员变量赋初始值，此时t会被赋值new T()，对count++，此时count是1，之后count又被赋值为2，因此count为2。

#### new对象的过程

第一步：给new的对象申请内存，内存申请完毕后，先给成员变量赋默认值

第二步：调用构造方法，给成员变量赋初始值

**静态变量是在类加载的初始化时分为两步赋值，对象中的成员变量的赋值是在类被创建的时候被分为两步**

load：默认值—初始值

new：申请空间—默认值—初始值

#### 单例模式：双重检查

![image-20230106210907629](/../JVM.assets/image-20230106210907629.png)

两次检查 INSTANCE == null ，但在执行INSTANCE = new Mgr06()时，在初始化阶段将里面的成员变量赋值为0时，另一个线程来执行此方法，此时 INSTANCE != null ，并被返回，但是返回的成员变量均为初始值。

解决方法：加volatile关键词（**指令重排**）

![image-20230106211020755](/../JVM.assets/image-20230106211020755.png)

####  为什么要加volatile关键词？

![image-20230106211031915](/../JVM.assets/image-20230106211031915.png)

二进制码：

![image-20230106211129159](/../JVM.assets/image-20230106211129159.png)

1：创建内存

3：调用构造方法，给成员变量赋初始值

4：将引用值赋值给 "t" 

指令重排发生时，可能先发生4再发生3，此时赋的值为默认值

### Initializing

![image-20230106211848505](/../JVM.assets/image-20230106211848505.png)

![image-20230106212343967](/../JVM.assets/image-20230106212343967.png)

init：构造方法

main：main方法

clinit：类构造器方法

**类的构造器(init)**

+ 如果不手动的添加，都会默认的添加一个空参的构造器

**类的构造器方法(clinit)**

- 初始化阶段就是执行类构造器方法`<clinit> ()`的过程。
- 此方法不需定义，是javac编译器自动收集类中的所有类变量的赋值动作和**静态代码块**中的语句合并而来。**没有静态代码块就没有上述过程。**
- 构造器方法中指令按语句在源文件中出现的顺序执行。
- JVM会保证子类的clinit执行前，父类的clinit已经执行完毕。
- 虚拟机必须保证一个类的clinit方法在多线程下被同步加锁。
- `<clinit> ()`不同于类的构造器。(关联：构造器是虚拟机视角下的`<init>()`)

```java
public class ClassInitTest {
    private static int num = 1;
    private static int number;

    public ClassInitTest() {
    }
    
    public static void main(String[] args) {
        System.out.println(num);
        System.out.println(number);//如果没有声明全局变量，会报错，非法的前项引用
    }
    
    static {
        num = 2;
        number = 20;
        System.out.println(num);
        number = 10;
    }

}
```

![image-20230106212922334](/../JVM.assets/image-20230106212922334.png)

- 若该类具有父类，JVM会保证子类的<clinit>()执行前，父类的<clinit> ()已经执行完毕。

```java
public class ClinitTest1 {
    static class Father{
        public static int A = 1;
        static{
            A = 2;
        }
    }

    static class Son extends Father{
        public static int B = A;
    }

    public static void main(String[] args) {
        //加载Father类，其次加载Son类。
        System.out.println(Son.B);//2
    }
}
```

![image-20230106213152635](/../JVM.assets/image-20230106213152635.png)

- 虚拟机必须保证一个类的<clinit>() 方法在多线程下被同步加锁。(static代码块只能被执行一次)

```java
public class DeadThreadTest {
    public static void main(String[] args) {
        Runnable r = () -> {
            System.out.println(Thread.currentThread().getName() + "开始");
            DeadThread dead = new DeadThread();
            System.out.println(Thread.currentThread().getName() + "结束");
        };

        Thread t1 = new Thread(r,"线程1");
        Thread t2 = new Thread(r,"线程2");

        t1.start();
        t2.start();
    }
}

class DeadThread{
    static{
        if(true){
            System.out.println(Thread.currentThread().getName() + "初始化当前类");
            while(true){

            }
        }
    }
}
```

![image-20230106213205398](/../JVM.assets/image-20230106213205398.png)

### 类加载器的分类

JVM支持两种类型的类加载器，分别为**引导类加载器（Bootstrap ClassLoader）**和**自定义类加载器(User-Defind ClassLoader)**

从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是Java虚拟机规范却没有这么定义，而是**将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器。**

无论类加载器的类型如何划分，在程序中我们最常见的类加载器始终只有3个，如下所示：

![image-20230107214030553](/../JVM.assets/image-20230107214030553.png)

这里的四者之间的关系是包含关系，不是上下层关系，也不是父子类的继承关系。

```java
public class ClassLoaderTest {
    public static void main(String[] args) {

        //获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //获取其上层：扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader);//sun.misc.Launcher$ExtClassLoader@1540e19d

        //获取其上层：获取不到引导类加载器
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader);//null

        //对于用户自定义类来说：默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //String类使用引导类加载器进行加载的。---> Java的核心类库都是使用引导类加载器进行加载的。
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1);//null
        
    }
}
```

#### 类加载器之间的关系

![image-20230107214613206](/../JVM.assets/image-20230107214613206.png)

在创建的时候，会同时生成底层二进制码文件和一个class对象，当程序运行时，会通过这个class对象调用和访问底层的二进制码。过程是先自下而上（上图里的加载器）的查找，再自上而下的查找，当返回为null时，一定是Bootstrap加载器加载的

#### Bootstrap ClassLoader

- 启动类加载器/引导类加载器

- 这个类加载使用C/C++语言实现的，嵌套在JVM内部。
- 它用来加载Java的核心库(/../JAVA HOME/jre/lib/rt.jar/resources. jar或sun.boot.class.path路径下的内容) ,用于提供JVM自身需要的类
- 并不继承自java. lang.ClassLoader，没有父加载器。
- 加载扩展类和应用程序类加载器，并指定为他们的父类加载器。
- 出于安全考虑，Bootstrap启动类加载器只加载包名为java、javax、sun等开头的类。

#### Extension ClassLoader

- 扩展类加载器
- **Java语言编写**，由sun.misc. Launcher$ExtClassLoader实现。
- **派生于ClassLoader类**
- 父类加载器为启动类加载器
- 从java.ext. dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的jre/lib/ext子目录(扩展目录)下加载类库。**如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载。**

#### AppClassLoader

- 应用程序类加载器/系统类加载器
- java语言编写，由sun . misc. LauncherSAppClassLoader实现
- 派生于ClassLoader类
- 父类加载器为扩展类加载器
- 它负责加载环境变量classpath或系统属性java.class.path 指定路径下的类库
- 该类加载是程序中默认的类加载器，一般来说，Java应用的类都是由它来完成加载
- 通过ClassLoader #getSystemClassLoader ()方法可以获取到该类加载器

![image-20230107214924731](/../JVM.assets/image-20230107214924731.png)

运行结果：

被bootstrap加载的jar包

![image-20230107215001481](/../JVM.assets/image-20230107215001481.png)

被extension加载的内容

![image-20230107215042328](/../JVM.assets/image-20230107215042328.png)

被app加载的内容

![image-20230107215101643](/../JVM.assets/image-20230107215101643.png)

#### 自定义类加载器

在Java的日常应用程序开发中，类的加载几乎是由上述3种类加载器相互配合执行的，在必要时，我们还可以自定义类加载器，来定制类的加载方式。

##### 什么要自定义加载器？

- 隔离加载类
- 修改类的加载方法
- 扩展加载源
- 防止源码泄露

##### 自定义加载的步骤

1.继承ClassLoader

![image-20230107215248576](/../JVM.assets/image-20230107215248576.png)

2.重写模板方法findClass

**负责将底层二进制编码转换为class对象的方法叫做defineClass，底层必须有这个方法**

![image-20230107215259455](/../JVM.assets/image-20230107215259455.png)

3.调用defineClass（将二进制码文件转换为class类对象的方法）

![image-20230107215333755](/../JVM.assets/image-20230107215333755.png)

**测试类**

![image-20230107215502739](/../JVM.assets/image-20230107215502739.png)

执行过程：

先去app找，找不到再去ext找，找不到再去bootstrap找，找不到ext再找，找不到APP再找，找不到自定义找，在c://text目录下找到了class对象

##### 自定义类加载器加载自加密的class（扩展）

简单加密：二进制文件经过两次异或还是原值，在异或前再加入一个seed的二进制代码片段

自己编译的时候，先异或，再减去seed二进制代码片段

防止反编译

防止篡改

##### 自定义父加载类

![image-20230107220333535](/../JVM.assets/image-20230107220333535.png)

##### 自定义热部署（Tomcat热部署的简单实现）

重写loadClass之前：

方法类：

![image-20230107220344399](/../JVM.assets/image-20230107220344399.png)

运行结果为true，说明第一次编译后，第二次编译时通过双亲委派访问到了第一次编译的结果，因此值相同。

重写loadClass之后：

![image-20230107220524869](/../JVM.assets/image-20230107220524869.png)

![image-20230107220535805](/../JVM.assets/image-20230107220535805.png)

重写的方法中，第一步就是先查找需要编译的class文件，如果找到了就直接编译，如果没有找到，就交给父加载类编译。排除了原有的判断是否加载过的这一步骤，而是直接进行编译

方法类：

![image-20230107220808863](/../JVM.assets/image-20230107220808863.png)

运行结果为false，说明第一次编译后，第二次编译时直接重新编译了底层的class文件。

### ClassLoader

ClassLoader类，它是一个抽象类，其后所有的类加载器都继承自ClassLoader (不包括启动类加载器)

| 方法名称                                          | 描述                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| getParent（）                                     | 返回该类加载器的超类加载器                                   |
| loadClass（String name）                          | 加载名称为name的类，返回结果为java.lang.Class类的实例        |
| findClass（String name）                          | 查找名称为name的类，返回结果为java.lang.Class类的实例        |
| findLoadClass(String name)                        | 查找名称为name的已经被加载过的类，返回结果为java.lang.Class类的实例 |
| defineClass(String name.byte[] b,int off.int len) | 把字节数组b中的内容转换为一个java类，返回结果为java.lang.Class类的实例 |
| resolveClass(Class<?> c)                          | 链接一个指定的java类                                         |

##### 获取classloader的途径

- 方式一：获取当前类的ClassLoader
  clazz. getClassLoader ()

- 方式二：获取当前线程上下文的ClassLoader
  Thread. currentThread() .getContextClassLoader ()

- 方式三：获取系统的ClassLoader
  ClassLoader.getSystemClassLoader ()

- 方式四：获取调用者的ClassLoader
  DriverManager . getCallerClassLoader ()

### 双亲委派

#### 机制

Java虚拟机对class文件采用的是**按需加载**的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。而且加载某个类的class文件时，Java虚拟机采用的是双亲委派模式，即把请求交由父类处理，它是一种任务委派模式。

##### 工作原理

1. 如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器执行;
2. 如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达项层的启动类加载器;
3. 如果父类加载器可以完成类加载任务，就成功返回，倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载，这就是双亲委派模式。

![image-20230107223938527](/../JVM.assets/image-20230107223938527.png)

是一个孩子向父亲的方向，然后父亲向孩子方向的双亲委派过程

##### 父加载类

**父加载类不是“类加载器的加载器”，也不是“类加载器的父类加载器”**

![image-20230107224043401](/../JVM.assets/image-20230107224043401.png)

##### 为什么要使用双亲委派？

- 保护程序安全，防止核心API被随意篡改
  - 主要原因是为了安全，假设当有人自拟了一个java.lang.String类，直接上传到类库中将原有的类库覆盖，之后将自己的代码上传到互联网上，当用户设置账号密码时，就会用到这个类，万一这个类中有一个功能可以将该用户的所有账户信息发送给开发者，那么个人信息将暴露。而双亲委派就不会出现此类情况，当排查到bootstarp时，发现已经执行过了，就不会覆盖掉原有的类。
- 避免类的重复加载
  - 次要原因可以避免资源的浪费，在双亲委派的过程中，如果发现已经加载过了就不会再次进行加载。

##### 沙箱安全机制

自定义String类，但是在加载自定义String类的时候会率先使用引导类加载器加载，而引导类加载器在加载的过程中会先加载jdk自带的文件(rt.jar包中java\lang\String.class)，报错信息说没有main方法，就是因为加载的是rt.jar包中的String类。这样可以保证对java核心源代码的保护，这就是沙箱安全机制。

##### 如何打破双亲委派机制？

1. 重写loadClass()
2. 何时打破过？
   1. JDK1.2.之前，自定义ClassLoader都必须重写loadClass()
   2. ThreadContextClassLoader可以实现基本类调用实现类代码，通过thread.setContextClassLoader指定
   3. 热启动、热部署
      1. osgi tomcat都有自己的模块指定classloader（可以加载同一类库的不同版本）

#### 编译器（扩展）

##### 混合模式

解释器 —— bytecode intepreter

JIT ——Just In-Time compiler（把java代码编译成本地代码执行）

- 混合模式
  - ​	混合使用解释器+热点代码编译
  - ​	起始阶段采用解释执行
  - ​	热点代码检测
    - 多次被调用的方法(方法计数器:监测方法执行频率)
    - 多次被调用的循环(循环计数器:检测循环执行频率)
    - 进行编译

##### 热点代码编译

当一段代码被大量的重复使用时，解释器不停地解释相同的代码，有损效率，为了提升效率，热点代码编译将这段代码编译为本地代码，当下次再需要这段代码时，就不用再执行解释器解释了，直接执行本地代码，这样就提高了编译效率

###### 检测热点代码

-XX:CompileThreshold = 10000

##### 编译模式的选择

- -Xmixed默认为混合模式	开始解释执行，启动速度较快对热点代码实行检测和编译
- -Xint 使用解释模式，启动很快执行稍慢
- -Xcomp使用纯编译模式，执行很快，启动很慢（指需要大量类进行编译时）

![image-20230107224449508](/../JVM.assets/image-20230107224449508.png)

#### 懒加载（扩展）

什么时候需要什么时候加载

- 严格讲应该叫lazy lnitializing
- JVM规范并没有规定何时加载
- 但是严格规定了什么时候必须初始化
  - new getstatic putstatic invokestatic指令，访问final变量除外
  - java.lang.reflect对类进行反射调用时
  - 初始化子类的时候，父类首先初始化
  - 虚拟机启动时，被执行的主类必须初始化
  - 动态语言支持java.lang.invoke.MethodHandle解析的结果为REF getstatic REF putstatic REF invokestatic的方法句柄时，该类必须初始化访问内部类时，需要外部类/内部类的名字

![image-20230107225623096](/../JVM.assets/image-20230107225623096.png)

### 类的主动使用和被动加载

#### 判断两个class对象是否为同一个类的条件

在JVM中表示两个class对象是否为同一个类存在两个必要条件：

- 类的完整类名必须一致，包括包名。
- 加载这个类的ClassLoader (指ClassLoader实例对象)必须相同。

换句话说，在JVM中，即使这两个类对象(class对象)来源同一个Class文件，被同一个虚拟机所加载，但**只要加载它们的ClassLoader实例对象不同**，那么这两个类对象也是不相等的。

#### 对类加载器的引用——动态链接

JVM必须知道一个类型是由启动加载器加载的还是由用户类加载器加载的。如果一个类型是由用户类加载器加载的，那么JVM会**将这个类加载器的一个引用作为类型信息的一部分保存在方法区中。**当解析一个类型到另一个类型的引用的时候，JVM需要保证这两个类型的类加载器是相同的。

#### 主动使用/被动使用

- 主动使用，又分为七种情况:
  - **创建类的实例**
  - **访问某个类或接口的静态变量，或者对该静态变量赋值**
  - **调用类的静态方法**
  - **反射**(比如: Class . forName ("com. atguigu . Test") )
  - **初始化一个类的子类**
  - **Java虚拟机启动时被标明为启动类的类**
  - JDK 7开始提供的动态语言支持:
  - java. lang. invoke . MethodHandle实例的解析结果REF getStatic、REF putStatic、 REF invokeStatic句柄对应的类没有初始化，则初始化

除了以上七种情况，其他使用Java类的方式都被看作是对**类的被动使**用，都**不会导致类的初始化。**

# 字节码与类的加载器

## Class文件结构

### 概述

#### 字节码文件的跨平台性

1. java语言的跨平台性
   - java源代码编译成字节码文件后，如果在不同的平台上运行时，不用再此编译。
   - 这点不再那么吸引人了，大多数其他语言也可以做到
   - 跨平台已经快成为一门语言必选的特性
2. java虚拟机，跨语言的平台
   - java虚拟机不和包括java在内的任何语言绑定，他只和Class文件这种特定的二进制文件格式所关联
   - 所有的字节码文件必须遵守java虚拟机规范
3. 想要让规格程序正确地运行在JVM中，java虚拟机就必须要被编译为符合JVM规范的字节码
   - 前端编译器的主要任务就是负责将符合java语法规范的java代码转换为符合JVM规范的字节码文件
   - javac是一种能够将java原码编译为字节码的前端编译器
   - Javac编译器在将Java源码编译为一个有效的字节码文件过程中经历了4个步骤，分别是**词法解析、语法解析、语义解析以及生成字节码。**
   - ![image-20230109223339547](/../JVM.assets/image-20230109223339547.png)
4. Oracle的JDK软件包括两部分内容:
   - 一部分是将Java源代码编译成Java虚拟机的指令集的编译器；
   - 另一部分是用于实现Java虚拟机的运行时环境。

#### java的前端编译器 

![image-20230109223444628](/../JVM.assets/image-20230109223444628.png)

Java源代码的编译结果是字节码，那么肯定需要有种编译器能够将Java源码编译为字节码，承担这个重要责任的就是配置在path环境变量中的**javac编译器**。

- javac是一种能够将Java源码编译为字节码的**前端编译器**。
- Eclipse中内置的ECJ（Eclipse Compiler for Java）编译器是一种**增量式编译器**

**前端编译器并不会直接涉及编译优化等方面的技术，而是将这些具体优化细节移交给HotSpot的JIT编译器负责。**

#### 透过字节码指令看代码细节

- 类文件结构有几个部分？

  - 魔数
  - 版本号
  - 常量池
  - 访问表示
  - 类索引、父类索引、接口索引
  - 字段表
  - 方法表
  - 属性表

- 知道字节码吗？字节码都有哪些？Integerx=5;inty=5;比较x==y都经过哪些步骤？

  - 通过Jclasslib观察字节码文件

  - ```java
    public class IntegerTest {
        public static void main(String[] args) {
    
            Integer x = 5;
            int y = 4;
            System.out.println(x == y);
    
        }
    }
    ```

  - ![image-20230109224012119](/../JVM.assets/image-20230109224012119.png)

##### 实例

```java
/*
成员变量（非静态的）的赋值过程： 
	① 默认初始化
    ② 显式初始化 /代码块中初始化 
    ③ 构造器中初始化
    ④ 有了对象之后，可以“对象.属性”或"对象.方法"的方式对成员变量进行赋值。
 */
class Father {
    int x = 10;

    public Father() {
        this.print();
        x = 20;
    }
    
    public void print() {
        System.out.println("Father.x = " + x);
    }

}

class Son extends Father {
    int x = 30;

    //    float x = 30.1F;
    public Son() {
        this.print();
        x = 40;
    }
    
    public void print() {
        System.out.println("Son.x = " + x);
    }

}

public class SonTest {
    public static void main(String[] args) {
        Father f = new Son();
        System.out.println(f.x);
    }
}
```

运行结果

![image-20230109224051199](/../JVM.assets/image-20230109224051199.png)

字节码解析

![image-20230109224059601](/../JVM.assets/image-20230109224059601.png)

### Class文件:虚拟机的基石

- 字节码文件	
  - 源代码经过编译器去编译后便会生成一个字节码文件，他的内容就是JVM指令，而不是想C/C++一样生成机器码。
- 字节码指令
  - Java虚拟机的指令是由一个字节码长度、代表着某种特定操作含义的**操作码**以及跟随其后的零至多个代表此操作所需参数的**操作数**所构成。部分指令并不包含操作数，只有操作码
  - 操作数+操作码
- 解读方式
  - NotePade++和插件HEX-Editor/Binary Viewer
  - javap指令
  - JClasslib

#### Class文件本质

一组以8位字节为基础单位的二进制流

#### Class文件格式

- 字节码文件中的字节顺序、数量都是被严格限定的，不能随意修改
- 采用一种类似于C语言的方式进行数据存储，这种结构中只有两种数据类型：**无符号数**和**表**
- 无符号数属于基本的数据类型，以u1、u2、u4、u8来分别表示一个字节、两个字节、四个字节和八个字节的无符号数，可以用来描述**数字**、**索引引用**、**数量值**或者**按照UTF-8编码构成字符串值**
- 表是由多个无符号数或者其他表作为数据项构成的**复合数据类型**，所有表都习惯性地以“info"结尾。表用于描述有层次关系的复合结构的数据，整个Class 文件本质上就是一张表。 由于表没有固定长度，所以通常会在其前面加上个数说明

#### Class文件的数据类型

| 数据类型 | 定义                                                         | 说明                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 无符号数 | 无符号数可以用来描述数字、索引引用、数量值或按照utf-8编码构成的字符串值。 | 其中无符号数属于基本的数据类型。 以u1、u2、u4、u8来分别代表1个字节、2个字节、4个字节和8个字节 |
| 表       | 表是由多个无符号数或其他表构成的复合数据结构。               | 所有的表都以“_info”结尾。 由于表没有固定长度，所以通常会在其前面加上个数说明。 |

### Class文件结构

Class文件的结构并不是一成不变的，随着Java虚拟机的不断发展，总是不可避免地会对Class文件结构做出一些调整，但是其基本结构和框架是非常稳定的。

结构如下：

| 类型           | 名称                | 说明                   | 长度    | 数量                  |
| -------------- | ------------------- | ---------------------- | ------- | --------------------- |
| u4             | magic               | 魔数,识别Class文件格式 | 4个字节 | 1                     |
| u2             | minor_version       | 副版本号(小版本)       | 2个字节 | 1                     |
| u2             | major_version       | 主版本号(大版本)       | 2个字节 | 1                     |
| u2             | constant_pool_count | 常量池计数器           | 2个字节 | 1                     |
| cp_info        | constant_pool       | 常量池表               | n个字节 | constant_pool_count-1 |
| u2             | access_flags        | 访问标识               | 2个字节 | 1                     |
| u2             | this_class          | 类索引                 | 2个字节 | 1                     |
| u2             | super_class         | 父类索引               | 2个字节 | 1                     |
| u2             | interfaces_count    | 接口计数器             | 2个字节 | 1                     |
| u2             | interfaces          | 接口索引集合           | 2个字节 | interfaces_count      |
| u2             | fields_count        | 字段计数器             | 2个字节 | 1                     |
| field_info     | fields              | 字段表                 | n个字节 | fields_count          |
| u2             | methods_count       | 方法计数器             | 2个字节 | 1                     |
| method_info    | methods             | 方法表                 | n个字节 | methods_count         |
| u2             | attributes_count    | 属性计数器             | 2个字节 | 1                     |
| attribute_info | attributes          | 属性表                 | n个字节 | attributes_count      |

#### 实例

```java
public class Demo {
    private int num = 1;

    public int add(){
        num = num + 2;
        return num;
    }

}
```

![image-20230109224753608](/../JVM.assets/image-20230109224753608.png)

#### 魔数：Class文件的标识

每种文件都有类似于class文件开头的cafe babe的魔数，他是Class文件的标识符，只有开头为cafe babe的class文件才会被JVM所接受。

#### 版本号

| 主版本（十进制） | 副版本（十进制） | 编译器版本 |
| ---------------- | ---------------- | ---------- |
| 45               | 3                | 1.1        |
| 46               | 0                | 1.2        |
| 47               | 0                | 1.3        |
| 48               | 0                | 1.4        |
| 49               | 0                | 1.5        |
| 50               | 0                | 1.6        |
| 51               | 0                | 1.7        |
| 52               | 0                | 1.8        |
| 53               | 0                | 1.9        |
| 54               | 0                | 1.10       |
| 55               | 0                | 1.11       |

- **不同版本的Java编译器编译的Class文件对应的版本是不一样的。目前，高版本的Java虚拟机可以执行由低版本编译器生成的Class文件,但是低版本的Java虚拟机不能执行由高版本编译器生成的Class文件。否则JVM会抛出java. lang. UnsupportedClassVersionError异常。(向下兼容)**

#### 常量池：存放所有的常量

##### 常量池计数器

- 常量池是Class文件中内容最为丰富的区域之一。常量池对于Class文件中的字段和方法解析也有着至关重要的作用。

- 随着Java虚拟机的不断发展，常量池的内容也日渐丰富。可以说，常量池是整个Class文件的基石。
- 在版本号之后，紧跟着的是**常量池的数量**，以及**若干个常量池表项**
- 常量池中常量的数量是不固定的，所以在常量池的入口需要放置一项u2类型的无符号数，代表常量池容量计数值(constant_ pool_ count)。与Java中语言习惯不一样的是，这个容量计数是从1而不是0开始的。
- ![image-20230109232547413](/../JVM.assets/image-20230109232547413.png)
- 由上表可见，Class文件使用了一个前置的容量计数器(constant_pool_count) 加若干个连续的数据项(constant_pool) 的形式来描述常量池内容。我们把这一系列连续常量池数据称为常量池集合。
- **常量池表项**中，用于存放编译时期生成的各种**字面量**和**符号引用**，这部分内容将在类加载后进入方法区的**运行时常量池**中存放。

##### 常量池表

- constant_pool是一种表结构，以1 ~ constant_pool_count - 1为索引。表明了后面有多少个常量项。

- 常量池主要存放两大类常量：**字面量 (Literal)** 和**符号引用(Symbolic References)**

- 它包含了class文件结构及其子结构中引用的所有字符串常量、类或接口名、字段名和其他常量。常量池中的每一项都具备相同的特征。第1个字节作为类型标记，用于确定该项的格式，这个字节称为tag byte (标记字节、 标签字节)。

- | 类型                             | 标志(或标识) | 描述                   |
  | -------------------------------- | ------------ | ---------------------- |
  | CONSTANT_utf8_info               | 1            | UTF-8编码的字符串      |
  | CONSTANT_Integer_info            | 3            | 整型字面量             |
  | CONSTANT_Float_info              | 4            | 浮点型字面量           |
  | CONSTANT_Long_info               | 5            | 长整型字面量           |
  | CONSTANT_Double_info             | 6            | 双精度浮点型字面量     |
  | CONSTANT_Class_info              | 7            | 类或接口的符号引用     |
  | CONSTANT_String_info             | 8            | 字符串类型字面量       |
  | CONSTANT_Fieldref_info           | 9            | 字段的符号引用         |
  | CONSTANT_Methodref_info          | 10           | 类中方法的符号引用     |
  | CONSTANT_InterfaceMethodref_info | 11           | 接口中方法的符号引用   |
  | CONSTANT_NameAndType_info        | 12           | 字段或方法的符号引用   |
  | CONSTANT_MethodHandle_info       | 15           | 表示方法句柄           |
  | CONSTANT_MethodType_info         | 16           | 标志方法类型           |
  | CONSTANT_InvokeDynamic_info      | 18           | 表示一个动态方法调用点 |

###### 字面量和符号引用

常量池中存放着两大常量：字面量（Literal）和符号引用（Symbolic Rdference)，如下表：

| 常量     | 具体的常量          |
| -------- | ------------------- |
| 字面量   | 文本字符串          |
|          | 声明为final的常量值 |
| 符号引用 | 类和接口的全限定名  |
|          | 字段的名称和描述符  |
|          | 方法的名称和描述符  |

- 全限定名

  - com/atguigu/test/Demo这个就是类的全限定名，仅仅是把包名的"."替换成"/"，为了使连续的多个全限定名之间不产生混淆，在使用时最后一般会加入一个“;”表示全限定名结束。

- 简单名称

  - 简单名称是指没有类型和参数修饰的方法或者字段名称，上面例子中的类的add()方法和num字段的简单名称分别是add和num。

- 描述符

  - **描述符的作用是用来描述字段的数据类型、方法的参数列表（包括数量、类型以及顺序）和返回值**。根据描述符规则，基本数据类型以及代表无返回值的void类型都用一个大学的字符来表示，二对象类型则用字符加对象的全限定名来表示，如下表：

  - | 标志符 | 含义                                                 |
    | ------ | ---------------------------------------------------- |
    | B      | 基本数据类型byte                                     |
    | C      | 基本数据类型char                                     |
    | D      | 基本数据类型double                                   |
    | F      | 基本数据类型float                                    |
    | I      | 基本数据类型int                                      |
    | J      | 基本数据类型long                                     |
    | S      | 基本数据类型short                                    |
    | Z      | 基本数据类型boolean                                  |
    | V      | 代表void类型                                         |
    | L      | 对象类型，比如：`Ljava/lang/Object;`                 |
    | [      | 数组类型，代表一维数组。比如：`double[][][] is [[[D` |

用描述符来描述方法时，按照先参数列表，后返回值的顺序描述，参数列表按照参数的严格顺序放在一组小括号 “()”之内。如方法java. lang. String toString()的描述符为() Ljava/1ang/String;， 方法int abc(int[] X, int y)的描述符为([II) I。

> 补充：
>
> 虚拟机在加载Class文件时才会进行动态链接，也就是说，Class文件中不会保存各个方法和字段的最终内存布局信息，因此，这些字段和方法的符号引用不经过转换是无法直接被虚拟机使用的。**当虚拟机运行时，需要从常量池中获得对应的符号引用，再在类加载过程中的解析阶段将其替换为直接引用，并翻译到具体的内存地址中。**
>
> 这里说明下符号引用和直接引用的区别与关联：
>
> - 符号引用：符号引用以**一组符号**来描述所引用的目标，符号可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可。**符号引用与虚拟机实现的内存布局无关**，引用的目标并不一 定已经加载到了内存中。
> - 直接引用：直接引用可以是直接**指向目标的指针、相对偏移量或是一个能间接定位到目标的句柄。直接引用是与虚拟机实现的内存布局相关的**，同一个符号引用在不同虚拟机实例上翻译出来的直接引用一般不会相同。如果有了直接引用，那说明引用的目标必定经存在于内存之中了。

###### 常量类型和结构

![image-20230109232602142](/../JVM.assets/image-20230109232602142.png)

![image-20230109232612260](/../JVM.assets/image-20230109232612260.png)

##### 总结

- 这14种表(或者常量项结构)的共同点是:表开始的第一位是一个u1类型的标志位 (tag)，代表 当前这个常量项使用的是哪种表结构，即哪种常量类型。
- 在常量池列表中，CONSTANT_ Utf&_info常量项是一种使用改进过的UTF - 8编码格式来存储诸如文字字符串、类或者接口的全限定名、字段或者方法的简单名称以及描述符等常量字符串信息。
- 这14种常量项结构还有一个特点是，其中13个常量项占用的字节固定，只有CONSTANT_ Utf8_ info占用字节不固定，其大小由length决定。为什么呢？**因为从常量池存放的内容可知，其存放的是字面量和符号引用，最终这些内容都会是一个字符串，这些字符串的大小是在编写程序时才确定**，比如你定义一个类，类名可以取长取短，所以在没编译前，大小不固定编译后，通过utf-8编码，就可以知道其长度。
- 常量池:可以理解为Class文件之中的资源仓库，它是Class文件结构中与其他项目关联最多的数据类型(后面的很多数据类型都会指向此处)，也是占用Class文件空间最大的数据项目之一。
- 常量池中为什么要包含这些内容
  - Java代码在进行Javac编译的时候， 并不像C和C++那样有“连接”这一步骤， 而是在虚拟机加载Class文件的时候进行动态链接。也就是说，**在Class文件中不会保存各个方法、字段的最终内存布局信息，因此这些字段、方法的符号引用不经过运行期转换的话无法得到真正的内存入口地址，也就无法直接被虚拟机使用**。当虚拟机运行时，需要从常量池获得对应的符号引用，再在类创建时或运行时解析、翻译到具体的内存地址之中。关于类的创建和动态链接的内容，在虚拟机类加载过程时再进行详细讲解

#### 访问指识

##### 概述

- 在常量池后，紧跟着访问标记。该标记使用两个字节表示，用于识别一些类或者接口层次的访问信息，包括:这个Class是类还是接口;是否定义为public 类型;是否定义为abstract 类型;如果是类的话，是否被声明为final等。各种访问标记如下所示：

- | 标志名称       | 标志值 | 含义                                                         |
  | -------------- | ------ | ------------------------------------------------------------ |
  | ACC_PUBLIC     | 0x0001 | 标志为public类型                                             |
  | ACC_FINAL      | 0x0010 | 标志被声明为final，只有类可以设置                            |
  | ACC_SUPER      | 0x0020 | 标志允许使用invokespecial字节码指令的新语义，JDK1.0.2之后编译出来的类的这个标志默认为真。（使用增强的方法调用父类方法） |
  | ACC_INTERFACE  | 0x0200 | 标志这是一个接口                                             |
  | ACC_ABSTRACT   | 0x0400 | 是否为abstract类型，对于接口或者抽象类来说，次标志值为真，其他类型为假 |
  | ACC_SYNTHETIC  | 0x1000 | 标志此类并非由用户代码产生（即：由编译器产生的类，没有源码对应） |
  | ACC_ANNOTATION | 0x2000 | 标志这是一个注解                                             |
  | ACC_ENUM       | 0x4000 | 标志这是一个枚举                                             |

- 类的访问权限通常为ACC_ 开头的常量

- 每一种类型的表示都是通过设置访问标记的32位中的特定位来实现的。比如，若是public final的类，则该标记为ACC_ PUBLIC| ACC_ FINAL。

- 使用ACC_SUPER可以让类更准确地定位到父类的方法super.method(),现代编译器都会设置并且使用这个标记。

##### 补充

1. 带有ACC_ INTERFACE标志的class文件表示的是接口而不是类，反之则表示的是类而不是接口。
   1. **如果一个class文件被设置了ACC_ INTERFACE 标志，那么同时也得设置ACC_ ABSTRACT 标志。同时它不能再设置ACC_ FINAL、ACC_ SUPER或ACC_ ENUM标志。**
   2. **如果没有设置ACC_ INTERFACE标志，那么这个class文件可以具有上表中除ACC_ ANNOTATION外的其他所有标志。当然，ACC_ FINAL和ACC _ABSTRACT这类互斥的标志除外。这两个标志不得同时设置。**

2. ACC_ SUPER标志用于确定类或接口里面的invokespecial指令使用的是哪一种执行语义。**针对Java虚拟机指令集的编译器都应当设置这个标志**。对于Java SE 8及后续版本来说，无论class文件中这个标志的实际值是什么，也不管class文件的版本号是多少，Java虚拟机都认为每个class文件均设置了ACC_ SUPER标志。
1.  **ACC_ SUPER标志是为了向后兼容由旧Java编译器所编译的代码而设计的。目前的ACC_ SUPER标志在由JDK 1.0.2之 前的编译器所生成的access_ flags中是没有确定含义的，如果设置了该标志，那么Oracle的Java虛拟机实现会将其忽略。**
3. ACC_ SYNTHETIC标志意味着该类或接口是由编译器生成的，而不是由源代码生成的。
4. 注解类型必须设置ACC_ ANNOTATION标志。如果设置了ACC_ ANNOTATION标志，那 么也必须设置ACC_ INTERFACE标志。
5. ACC_ENUM标志 表明该类或其父类为枚举类型。
6. 表中没有使用的access_flags标志是为未来扩充而预留的，这些预留的标志在编译器中应该设置为0，Java虚拟机实现也应该忽略它们。

#### 类索引、父类索引、接口索引

- 在访问标记后，会指定该类的类别、父类类别记忆实现的接口，格式如下：

- | 长度 | 含义                         |
  | ---- | ---------------------------- |
  | u2   | this_class                   |
  | u2   | super_class                  |
  | u2   | interfaces_count             |
  | u2   | interfaces[interfaces_count] |

- 这三项数据来确定这个类的继承关系

  - 类索引用于确定这个类的全限定名
  - 父类索引用于确定这个类的父类的全限定名。由于Java语言不允许多重继承，所以父类索引只有一个，除了java.lang . 0bject之外，所有的Java类都有父类，因此除了java.lang . 0bject外，所有Java类的父类索引都不为0。
  - 接口索引集合就用来描述这个类实现了哪些接口，这些被实现的接口将按implements 语句(如果这个类本身是一个接口，则应当是extends 语句)后的接口顺序从左到右排列在接口索引集合中。

1. **this_class (类索引)**
   - 2字节无符号整数，指向常量池的索引。它提供了类的全限定名，如com/ atguigu/java1/Demo. this_ class的值必须是对常量池表中某项的一个有效索引值。常量池在这个索引处的成员必须为CONSTANT_ Class _info类型结构体，该结构体表示这个class文件所定义的类或接口。
2. **super_class(父类索引)**
   - 2字节无符号整数，指向常量池的索引。它提供了当前类的父类的全限定名。如果我们没有继承任何类，其默认继承的是java/lang/0bject类。同时，由于Java不支持多继承，所以其父类只有一个。
   - superclass指向的父类不能是final。
3. **interfaces**
   - 指向常量池索引**集合**，它提供了一个符号引用到所有己实现的接口
   - 由于一个类可以实现多个接口，因此需要以数组形式保存多个接口的索引，表示接口的每个索引也是一个指向常量池的CONSTANT_Class (当然这里就必须是接口，而不是类)。
   - 3.1 interfaces_count (接口计数器)
     - interfaces_count项的 值表示当前类或接口的直接超接口数量。
   - 3.2 interfaces [] (接口索引集合)
     - interfaces []中 每个成员的值必须是对常量池表中某项的有效索引值，它的长度为interfaces_ count。 每个成员interfaces [i]必须为CONSTANT_ Class_ info结构，其中0 <= i < interfaces_ count。 在interfaces[]中， 各成员所表示的接口顺序和对应的源代码中给定的接口顺序(从左至右) 一样，即interfaces[0]对 应的是源代码中最左边的接口。

#### 字段表集合

##### 概述

- 用于描述接口或类中声明的变量。字段（field）包括**类级变量以及实例级变量**，但是不包括方法内部、代码块内部声明的局部变量
- 只能引用常量池中的常量来描述
- 他指向常量池索引集合，他描述了每个字段的完整信息。（比如：字段修饰符、访问修饰符、类变量还是实例变量、是否常量……）

##### 注意事项

- 字段表集合中**不会列出从父类或者实现的接口中继承而来的字段**，但**有可能列出原本Java代码之中不存在的字段**。譬如在内部类中为了保持对外部类的访问性，会自动添加指向外部类实例的字段。
- 在Java语言中字段是无法重载的，两个字段的数据类型、修饰符不管是否相同，都必须使用不一样的名称，但是**对于字节码来讲，如果两个字段的描述符不一致，那字段重名就是合法的。**

##### 字段计数器

- fields_count的 值表示**当前class文件fields表的成员个数**。使用两个字节来表示。

- fields表中每个成员都是一个field_ info结构， 用于表示该类或接口所声明的所有类字段或者实例字段，不包括方法内部声明的变量，也不包括从父类或父接口继承的那些字段。

##### 字段表

- fields表中的每个成员都必须是一个fields _info结 构的数据项，用于表示当前类或接口中某个字段的完整描述。

- 一个字段的信息包括如下这些信息。这些信息中，**各个修饰符都是布尔值，要么有，要么没有。**

  - 作用域(public、private、protected修饰符 )
  - 是实例变量还是类变量( static修饰符)
  - 可变性( final)
  - 并发可见性(volatile修饰符，是否强制从主内存读写)
  - 可否序列化(transient修饰符)
  - 字段数据类型(基本数据类型、对象、数组) I
  - 字段名称
  - 字段表结构

- 字段表作为一个表，同样有他自己的结构

  - |      类型      |       名称       |    含义    |       数量       |
    | :------------: | :--------------: | :--------: | :--------------: |
    |       u2       |   access_flags   |  访问标志  |        1         |
    |       u2       |    name_index    | 字段名索引 |        1         |
    |       u2       | descriptor_index | 描述符索引 |        1         |
    |       u2       | attributes_count | 属性计数器 |        1         |
    | attribute_info |    attributes    |  属性集合  | attributes_count |

###### 字段表访问表示

| 标志名称      | 标志值 | 含义                       |
| ------------- | ------ | -------------------------- |
| ACC_PUBLIC    | 0x0001 | 字段是否为public           |
| ACC_PRIVATE   | 0x0002 | 字段是否为private          |
| ACC_PROTECTED | 0x0004 | 字段是否为protected        |
| ACC_STATIC    | 0x0008 | 字段是否为static           |
| ACC_FINAL     | 0x0010 | 字段是否为final            |
| ACC_VOLATILE  | 0x0040 | 字段是否为volatile         |
| ACC_TRANSTENT | 0x0080 | 字段是否为transient        |
| ACC_SYNCHETIC | 0x1000 | 字段是否为由编译器自动产生 |
| ACC_ENUM      | 0x4000 | 字段是否为enum             |

###### 字段名索引

根据字段名索引的值，查询常量池中的指定索引项即可

###### 描述符索引

描述符的作用使用来描述字段的数据类型、方法的参数列表和返回值。根据描述符规则，计本书籍类型及代表无返回值的void类型都用一个大写字符来表示，而对象则用字符L加对象的全限定名来表示。

| 标志符 | 含义                                                 |
| ------ | ---------------------------------------------------- |
| B      | 基本数据类型byte                                     |
| C      | 基本数据类型char                                     |
| D      | 基本数据类型double                                   |
| F      | 基本数据类型float                                    |
| I      | 基本数据类型int                                      |
| J      | 基本数据类型long                                     |
| S      | 基本数据类型short                                    |
| Z      | 基本数据类型boolean                                  |
| V      | 代表void类型                                         |
| L      | 对象类型，比如：`Ljava/lang/Object;`                 |
| [      | 数组类型，代表一维数组。比如：`double[][][] is [[[D` |

###### 属性表集合

一个字段还可能拥有一些属性，用于存储更多的额外信息。比如初始值、一些知识信息等。属性个数存放在attrinute_count中，属性具体内容存放在attributes数组中。

以常量属性为例，结构为:

```java
ConstantValue_ attribute{
	u2 attribute_ name_ index;
	u4 attribute_ length;
	u2 constantvalue_ index;
}
```

说明：对于常量属性而言，attribute_length值恒为2。

#### 方法表集合

##### 概述

methods:指向常量池索引集合，它完整描述了每个方法的签名。

- 在字节码文件中，**每一个method_ info项都对应着一 个类或者接口中的方法信息**。比如方法的访问修饰符(public、private或protected),方法的返回值类型以及方法的参数信息等。
- 如果这个方法不是抽象的或者不是native的，那么字节码中会体现出来。
- 一方面，**methods表只描述当前类或接口中声明的方法，不包括从父类或父接口继承的方法**。另一方面，methods表**有可能会出现由编译器自动添加的方法**，最典型的便是编译器产生的方法信息(比如:类(接口)初始化方法< clinit >()和实例初始化方法< init >())。

##### 使用注意事项:
在Java语言中，要重载(Overload)一个方法，除了要与原方法具有相同的简单名称之外，还要求必须拥有一个与原方法不同的特征签名，特征签名就是一个方法中各个参数在常量池中的字段符号引用的集合，也就是因为返回值不会包含在特征签名之中，因此Java语言里无法仅仅依靠返回值的不同来对一个已有方法进行重载。但在Class文件格式中，特征签名的范围更大一些，只要描述符不是完全一致的两 个方法就可以共存。也就是说，如果两个方法有相同的名称和特征签名，但返回值不同，那么也是可以合法共存于同一个class文件中。

也就是说，**尽管Java语法规范并不允许在一个类或者接口中声明多个方法签名相同的方法，但是和Java语法规范相反，字节码文件中却恰恰允许存放多个方法签名相同的方法，唯一的条件就是这些方法之间的返回值不能相同**。

##### 方法计数器

methods_ count的 值表示当前class文件methods表的成员个数。使用两个字节来表示。

methods表中每个成员都是一个method_info结构。

##### 方法表

###### 概述

- methods表中的每个成员都必须是一个method_ info结构，用于表示当前类或接口中某个方法的完整描述。如果某个method_ info结 构的access_ flags项既没有设置ACC_NATIVE标志也没有设置ACC_ ABSTRACT标志，那么该结构中也应包含实现这个方法所用的Java虚拟机指令。

- method_ info结 构可以表示类和接口中定义的所有方法，包括实例方法、类方法、实例初始化方法和类或接口初始化方法

- 方法表的结构实际跟字段表是一样的

###### 方法表访问标志

跟字段表一样，方法表也有访问标志，他们大部分标志是相同的，也有部分是不同的。

#### 属性表结合

![image-20230109232634534](/../JVM.assets/image-20230109232634534.png)

- 方法表集合之后的属性表集合，**指的是class文件所携带的辅助信息**，比如该class 文件的源文件的名称。以及任何带有RetentionPolicy. CLASS或者RetentionPolicy. RUNTIME的注解。这类信息通常被用于Java虚拟机的验证和运行，以及Java程序的调试，**一般无须深入了解**。
- 此外，字段表、方法表都可以有自己的属性表。用于描述某些场景专有的信息。
- 属性表集合的限制没有那么严格，不再要求各个属性表具有严格的顺序，并且只要不与已有的属性名重复，任何人实现的编译器都可以向属性表中写入自己定义的属性信息，但Java虛 拟机运行时会忽略掉它不认识的属性。

##### 属性计数器

attributes_ count的值 表示当前class文件属性表的成员个数。属性表中每一项都是一个attribute _info结构。

##### 属性表

属性表的每个项的值必须是attribute_info结构，属性表的结构比较灵活，各种不同的属性只要满足以下结构即可

###### 属性的通用格式

| 类型 | 名称                 | 数量             | 含义       |
| ---- | -------------------- | ---------------- | ---------- |
| u2   | attribute_name_index | 1                | 属性名索引 |
| u4   | attribute_length     | 1                | 属性长度   |
| u1   | info                 | attribute_length | 属性表     |

###### 属性类型

属性表实际上可以有很多类型，上面看到的Code属性只是其中一种，java8里面定义了23种属性，下面这些是虚拟机中预定义的属性：

| 属性名称                            | 使用位置           | 含义                                                         |
| ----------------------------------- | ------------------ | ------------------------------------------------------------ |
| Code                                | 方法表             | Java代码编译成的字节码指令                                   |
| ConstantValue                       | 字段表             | final关键字定义的常量池                                      |
| Deprecated                          | 类，方法，字段表   | 被声明为deprecated的方法和字段                               |
| Exceptions                          | 方法表             | 方法抛出的异常                                               |
| EnclosingMethod                     | 类文件             | 仅当一个类为局部类或者匿名类是才能拥有这个属性，这个属性用于标识这个类所在的外围方法 |
| InnerClass                          | 类文件             | 内部类列表                                                   |
| LineNumberTable                     | Code属性           | Java源码的行号与字节码指令的对应关系                         |
| LocalVariableTable                  | Code属性           | 方法的局部便狼描述                                           |
| StackMapTable                       | Code属性           | JDK1.6中新增的属性，供新的类型检查检验器检查和处理目标方法的局部变量和操作数有所需要的类是否匹配 |
| Signature                           | 类，方法表，字段表 | 用于支持泛型情况下的方法签名                                 |
| SourceFile                          | 类文件             | 记录源文件名称                                               |
| SourceDebugExtension                | 类文件             | 用于存储额外的调试信息                                       |
| Synthetic                           | 类，方法表，字段表 | 标志方法或字段为编译器自动生成的                             |
| LocalVariableTypeTable              | 类                 | 使用特征签名代替描述符，是为了引入泛型语法之后能描述泛型参数化类型而添加 |
| RuntimeVisibleAnnotations           | 类，方法表，字段表 | 为动态注解提供支持                                           |
| RuntimeInvisibleAnnotations         | 表，方法表，字段表 | 用于指明哪些注解是运行时不可见的                             |
| RuntimeVisibleParameterAnnotation   | 方法表             | 作用与RuntimeVisibleAnnotations属性类似，只不过作用对象为方法 |
| RuntimeInvisibleParameterAnnotation | 方法表             | 作用与RuntimeInvisibleAnnotations属性类似，作用对象哪个为方法参数 |
| AnnotationDefault                   | 方法表             | 用于记录注解类元素的默认值                                   |
| BootstrapMethods                    | 类文件             | 用于保存invokeddynamic指令引用的引导方式限定符               |

Code属性

Java程序方法体中的代码经过Javac编译处理后，最终变为字节码指令存储在Code属性中．Code属性出现在方法表的属性集合中，但是并非所有的方法表都有这个属性．例如接口或类中的方法就不存在Code属性了．

在字节码指令之后的是方法的是方法的显式异常处理表集合，异常表对于Code属性来说并不是必须参在的

| 类型           | 名称                   | 数量             |
| -------------- | ---------------------- | ---------------- |
| u2             | attribute_name_index   | 1                |
| u4             | attribute_length       | 1                |
| u2             | max_stack              | 1                |
| u2             | max_locals             | 1                |
| u4             | code_length            | 1                |
| u1             | code                   | code_length      |
| u2             | exception_table_length | 1                |
| exception_info | exception_table        | exception_length |
| u2             | attributes_count       | 1                |
| attribute_info | attributes             | attributes_count |

#### 小结

本章主要介绍了Class文件的基本格式。

随着Java平台的不断发展，在将来，Class文件的内容也一定会做进一步的扩 充，但是其基本的格式和结构不会做重大调整。

从Java虚拟机的角度看，通过Class文件，可以让更多的计算机语言支持Java虚拟机平台。因此，**Class 文件结构不仅仅是Java虛拟机的执行入口，更是Java生态圈的基础和核心**。

### 使用javap指令解析Class文件

#### 解析字节码的作用

通过反编译生成的字节码文件，我们可以深入的了解java代码的工作机制。但是，自己分析类文件结构太麻烦了!除了使用第三方的jclasslib工具之外，oracle官 方也提供了工具: javap。

javap是jdk自带的反解析工具。它的作用就是根据class字节码文件，反解析出当前类对应的code区(字节码指令)、局部变量表、异常表和代码行偏移量映射表、常量池等信息。

通过局部变量表，我们可以查看局部变量的作用域范围、所在槽位等信息，甚至可以看到槽位复用等信息。

#### javac -g操作

解析字节码文件得到的信息中，有些信息(如局部变量表、指令和代码行偏移量映射表、常量池中方法的参数名称等等)需要在使用javac编译成class文件时，指定参数才能输出。

比如，你直接javac xx.java， 就不会在生成对应的局部变量表等信息，如果你使用**javac -g xx. java**就可以**生成所有相关信息**了。如果你使用的eclipse或IDEA，则默认情况下，**eclipse、 IDEA在编译时会帮你生成局部变量表、指令和代码行偏移量映射表等信息的。**

#### javap的用法

**javap的用法格式:** 

**javap < options> < classes>**

其中，classes 就是你要反编译的class文件。

在命令行中直接输入javap或javap -help可 以看到javap的options有如下选项：

![image-20211128172109496](/../JVM.assets/image-20211128172109496.png)

#### 使用举例

源代码

```java
public class JavapTest {
    private int num;
    boolean flag;
    protected char gender;
    public String info;

    public static final int COUNTS = 1;
    static{
        String url = "www.atguigu.com";
    }
    {
        info = "java";
    }
    public JavapTest(){

    }
    private JavapTest(boolean flag){
        this.flag = flag;
    }
    private void methodPrivate(){

    }
    int getNum(int i){
        return num + i;
    }
    protected char showGender(){
        return gender;
    }
    public void showInfo(){
        int i = 10;
        System.out.println(info + i);
    }
}
```

字节码文件

```java
Classfile /C:/Users/songhk/Desktop/2/JavapTest.class    //字节码文件所属的路径
  Last modified 2020-9-7; size 1358 bytes		//最后修改时间，字节码文件的大小
  MD5 checksum 526b4a845e4d98180438e4c5781b7e88         //MD5散列值
  Compiled from "JavapTest.java"			//源文件的名称
public class com.atguigu.java1.JavapTest
  minor version: 0					//副版本
  major version: 52					//主版本
  flags: ACC_PUBLIC, ACC_SUPER				//访问标识
Constant pool:						//常量池
   #1 = Methodref          #16.#46        // java/lang/Object."<init>":()V
   #2 = String             #47            // java
   #3 = Fieldref           #15.#48        // com/atguigu/java1/JavapTest.info:Ljava/lang/String;
   #4 = Fieldref           #15.#49        // com/atguigu/java1/JavapTest.flag:Z
   #5 = Fieldref           #15.#50        // com/atguigu/java1/JavapTest.num:I
   #6 = Fieldref           #15.#51        // com/atguigu/java1/JavapTest.gender:C
   #7 = Fieldref           #52.#53        // java/lang/System.out:Ljava/io/PrintStream;
   #8 = Class              #54            // java/lang/StringBuilder
   #9 = Methodref          #8.#46         // java/lang/StringBuilder."<init>":()V
  #10 = Methodref          #8.#55         // java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #11 = Methodref          #8.#56         // java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
  #12 = Methodref          #8.#57         // java/lang/StringBuilder.toString:()Ljava/lang/String;
  #13 = Methodref          #58.#59        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #14 = String             #60            // www.atguigu.com
  #15 = Class              #61            // com/atguigu/java1/JavapTest
  #16 = Class              #62            // java/lang/Object
  #17 = Utf8               num
  #18 = Utf8               I
  #19 = Utf8               flag
  #20 = Utf8               Z
  #21 = Utf8               gender
  #22 = Utf8               C
  #23 = Utf8               info
  #24 = Utf8               Ljava/lang/String;
  #25 = Utf8               COUNTS
  #26 = Utf8               ConstantValue
  #27 = Integer            1
  #28 = Utf8               <init>
  #29 = Utf8               ()V
  #30 = Utf8               Code
  #31 = Utf8               LineNumberTable
  #32 = Utf8               LocalVariableTable
  #33 = Utf8               this
  #34 = Utf8               Lcom/atguigu/java1/JavapTest;
  #35 = Utf8               (Z)V
  #36 = Utf8               methodPrivate
  #37 = Utf8               getNum
  #38 = Utf8               (I)I
  #39 = Utf8               i
  #40 = Utf8               showGender
  #41 = Utf8               ()C
  #42 = Utf8               showInfo
  #43 = Utf8               <clinit>
  #44 = Utf8               SourceFile
  #45 = Utf8               JavapTest.java
  #46 = NameAndType        #28:#29        // "<init>":()V
  #47 = Utf8               java
  #48 = NameAndType        #23:#24        // info:Ljava/lang/String;
  #49 = NameAndType        #19:#20        // flag:Z
  #50 = NameAndType        #17:#18        // num:I
  #51 = NameAndType        #21:#22        // gender:C
  #52 = Class              #63            // java/lang/System
  #53 = NameAndType        #64:#65        // out:Ljava/io/PrintStream;
  #54 = Utf8               java/lang/StringBuilder
  #55 = NameAndType        #66:#67        // append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #56 = NameAndType        #66:#68        // append:(I)Ljava/lang/StringBuilder;
  #57 = NameAndType        #69:#70        // toString:()Ljava/lang/String;
  #58 = Class              #71            // java/io/PrintStream
  #59 = NameAndType        #72:#73        // println:(Ljava/lang/String;)V
  #60 = Utf8               www.atguigu.com
  #61 = Utf8               com/atguigu/java1/JavapTest
  #62 = Utf8               java/lang/Object
  #63 = Utf8               java/lang/System
  #64 = Utf8               out
  #65 = Utf8               Ljava/io/PrintStream;
  #66 = Utf8               append
  #67 = Utf8               (Ljava/lang/String;)Ljava/lang/StringBuilder;
  #68 = Utf8               (I)Ljava/lang/StringBuilder;
  #69 = Utf8               toString
  #70 = Utf8               ()Ljava/lang/String;
  #71 = Utf8               java/io/PrintStream
  #72 = Utf8               println
  #73 = Utf8               (Ljava/lang/String;)V
#######################################字段表集合的信息################################################
{						
  private int num;				//字段名
    descriptor: I				//字段描述符：字段的类型
    flags: ACC_PRIVATE				//字段的访问标识

  boolean flag;
    descriptor: Z
    flags:

  protected char gender;
    descriptor: C
    flags: ACC_PROTECTED

  public java.lang.String info;
    descriptor: Ljava/lang/String;
    flags: ACC_PUBLIC

  public static final int COUNTS;
    descriptor: I
    flags: ACC_PUBLIC, ACC_STATIC, ACC_FINAL
    ConstantValue: int 1				//常量字段的属性：ConstantValue

#######################################方法表集合的信息################################################
  public com.atguigu.java1.JavapTest();				//构造器1的信息		
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: ldc           #2                  // String java
         7: putfield      #3                  // Field info:Ljava/lang/String;
        10: return
      LineNumberTable:
        line 20: 0
        line 18: 4
        line 22: 10
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      11     0  this   Lcom/atguigu/java1/JavapTest;

  private com.atguigu.java1.JavapTest(boolean);			//构造器2的信息	
    descriptor: (Z)V
    flags: ACC_PRIVATE
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: ldc           #2                  // String java
         7: putfield      #3                  // Field info:Ljava/lang/String;
        10: aload_0
        11: iload_1
        12: putfield      #4                  // Field flag:Z
        15: return
      LineNumberTable:
        line 23: 0
        line 18: 4
        line 24: 10
        line 25: 15
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      16     0  this   Lcom/atguigu/java1/JavapTest;
            0      16     1  flag   Z

  private void methodPrivate();
    descriptor: ()V
    flags: ACC_PRIVATE
    Code:
      stack=0, locals=1, args_size=1
         0: return
      LineNumberTable:
        line 28: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       1     0  this   Lcom/atguigu/java1/JavapTest;

  int getNum(int);
    descriptor: (I)I
    flags:
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: getfield      #5                  // Field num:I
         4: iload_1
         5: iadd
         6: ireturn
      LineNumberTable:
        line 30: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       7     0  this   Lcom/atguigu/java1/JavapTest;
            0       7     1     i   I

  protected char showGender();
    descriptor: ()C
    flags: ACC_PROTECTED
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: getfield      #6                  // Field gender:C
         4: ireturn
      LineNumberTable:
        line 33: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lcom/atguigu/java1/JavapTest;

  public void showInfo();				
    descriptor: ()V				//方法描述符：方法的形参列表 、 返回值类型
    flags: ACC_PUBLIC				//方法的访问标识
    Code:					//方法的Code属性
      stack=3, locals=2, args_size=1		//stack:操作数栈的最大深度   locals:局部变量表的长度  args_size：方法接收参数的个数
   //偏移量 操作码	 操作数	
	 0: bipush        10
         2: istore_1
         3: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
         6: new           #8                  // class java/lang/StringBuilder
         9: dup
        10: invokespecial #9                  // Method java/lang/StringBuilder."<init>":()V
        13: aload_0
        14: getfield      #3                  // Field info:Ljava/lang/String;
        17: invokevirtual #10                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        20: iload_1
        21: invokevirtual #11                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
        24: invokevirtual #12                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        27: invokevirtual #13                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        30: return
      //行号表：指名字节码指令的偏移量与java源程序中代码的行号的一一对应关系
      LineNumberTable:
        line 36: 0
        line 37: 3
        line 38: 30
      //局部变量表：描述内部局部变量的相关信息
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      31     0  this   Lcom/atguigu/java1/JavapTest;
            3      28     1     i   I

  static {};
    descriptor: ()V
    flags: ACC_STATIC
    Code:
      stack=1, locals=1, args_size=0
         0: ldc           #14                 // String www.atguigu.com
         2: astore_0
         3: return
      LineNumberTable:
        line 15: 0
        line 16: 3
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
}
SourceFile: "JavapTest.java"			//附加属性：指名当前字节码文件对应的源程序文件名

```

#### 总结

1、通过javap命令可以查看一个java类反汇编得到的Class文件版本号、常量池、访问标识、变量表、指令代码行号表等等信息。不显示类索引、父类索引、接口索引集合、< clinit >()、 < init >()等结构

2、通过对前面例子代码反汇编文件的简单分析，可以发现，一个方法的执行通常会涉及下面几块内存的操作：

1. java栈中：局部变量表、操作数栈。
2.  java堆：通过对象的地址引用去操作。
3. 常量池。
4. 其他如帧数据区、方法区的剩余部分等情况，测试中没有显示出来，这里说明一下。

3、平常，我们比较关注的是java类中每个方法的反汇编中的指令操作过程，这些指令都是顺序执行的，可以参考官方文档查看每个指令的含义，很简单:

[Oracl官网 (oracle.com)](https://docs.oracle.com/pls/rdman/handle404?X_ORCL_404path=%2f%20javase%2fspecs%2f%20jvms%2fse7%2fhtml%2f%20jvms%20-6.html)

## 字节码指令集与解析举例

### 概述

- Java字节码对于虚拟机，就好像汇编语言对于计算机，属于基本执行指令。

- Java虚拟机的指令由**一个字节长度**的、代表着某种特定操作含义的数字(称为操作码，Opcode)以及跟随其后的零至多个代表此操作所需参数(称为操作数，Operands) 而构成。**由于Java 虚拟机采用面向操作数栈而不是寄存器的结构，所以大多数的指令都不包含操作数，只有一个操作码**。

- 由于限制了Java虚拟机操作码的长度为一个字节(即0~255)，这意味着指令集的操作码总数不可能超过256条。

- 官方文档: https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html

- 熟悉虚拟机的指令对于动态字节码生成、反编译Class文件、Class 文件修补都有着非常重要的价值。因此，阅读字节码作为了解Java虚拟机的基础技能，需要熟练掌握常见指令。

#### 执行模型

如果不考虑异常处理的话，那么Java虚拟机的解释器可以使用下面这个伪代码当做最基本的执行模型来理解

```java
do{
	自动计算PC寄存器的值加1;
	根据PC寄存器的指示位置，从字节码流中取出操作码;
	if(字节码存在操作数) 从字节码流中取出操作数;
	执行操作码所定义的操作;
}while(字节码长度>0);
```

#### 字节码与数据类型

在Java虚拟机的指令集中，大多数的指令都包含了其操作所对应的数据类型信息。例如，iload指令用于从局部变量表中加载int型的数据到操作数栈中，而fload指令加载的则是float类型的数据。

对于大部分与数据类型相关的字节码指令，**它们的操作码助记符中都有特殊的字符来表明专门为哪种数据类型服务**：

- i代表对int类型的数据操作，
- l代表long
- s代表short
- b代表byte
- c代表char
- f代表float
- d代表double

也有一些指令的助记符中**没有明确地指明操作类型的字母**，如arraylength指令，它没有代表数据类型的特殊字符，但操作数永远只能是一个数组类型的对象。

还有另外一些指令，如无条件跳转指令goto则是与**数据类型无关的**。

大部分的指令都没有支持整数类型byte、char和short,甚至没有任何指令支持boolean类型。编译器会在编译期或运行期将byte和short类型的数据带符号扩展(Sign- Extend)为相应的int类型数据，将boolean和char类型数据零位扩展(Zero-Extend) 为相应的int类型数据。与之类似，在处理boolean、byte、short和char类型的数组时，也会转换为使用对应的int类型的字节码指令来处理。因此，大多数对于boolean、byte、 short和char类型数据的操作，

#### 指令分类

由于完全介绍和学习这些指令需要花费大量时间。为了让大家能够更快地熟悉和了解这些基本指令，这里将JVM中的字节码指令集按用途大致分成9类。

- 加载与存储指令
- 算术指令
- 类型转换指令
- 对象的创建与访问指令
- 方法调用与返回指令
- 操作数栈管理指令
- 比较控制指 令
- 异常处理指令
- 同步控制指令

(说在前面)在做值相关操作时：

-  一个指令，可以从局部变量表、常量池、堆中对象、方法调用、系统调用中等取得数据，这些数据(可能是值，可能是对象的引用)被压入操作数栈。

- 一个指令， 也可以从操作数栈中取出一到多个值(pop多次)，完成赋值、 加减乘除、方法传参、系统调用等等操作。

### 加载与存储指令

### 算术指令

### 类型转换指令

### 对象的创建与访问指令

### 方法调用与返回指令

### 操作数栈管理指令

### 比较控制指令

### 异常处理指令

### 同步控制指令

## 类的加载器详解

### 类的生命周期

在java中数据类型分为基本数据类型和引用数据类型。**基本数据类型由虚拟机预先定义，引用数据类型则需要进行类的加载。**

按照java虚拟机规范，从class文件到加载到内存的类，到类卸载出内存为止，他的整个生命周期包括了七个阶段



（其中，验证、准备、解析三部分统称为链接）

从程序中类的使用过程看：

![image-20230110140245918](/../JVM.assets/image-20230110140245918.png)

### Loading（加载）

#### 完成加载的操作

- 加载的理解
  - 所谓加载，换言之就是将java类的字节码文件加载到及其内存中，并在内存中构建出java类的原型——类模版对象（java类在JVM中的一个快照）。
  - 反射机制就是基于这一基础。如果JVM没有将java类的声明信息存储起来，则JVM在运行期也无法反射。
- 加载完成的操作
  - 加载阶段，换言之就是查找并加载类的二进制数据，生成Class的实例
  - 加载类时，java虚拟机必须完成以下三件事：
    - 根据全限定名，获取二进制流
    - 将二进制流解析为方法区的数据结构
    - 创建类模板对象，作为在方法区中这个类的访问入口

#### 二进制流的获取方法

对于类的二进制数据流，虚拟机可以通过多种途径产生或获得。(只**要所读取的字节码符合JVM规范即可**)

- **虚拟机可能通过文件系统读入一个class后缀的文件(最常见)**
-  **读入jar、zip等归档数据包，提取类文件。**
- 事先存放在数据库中的类的二进制数据
- 使用类似于HTTP之类的协议通过网络进行加载
- 在运行时生成一段Class的二进制信息等

在获取到类的二进制信息后，Java虚拟机就会处理这些数据，并最终转为一个java. lang. Class的实例。

如果输入数据不是ClassFile的结构，则会抛出ClassFormatError.

#### 类模型与Class实例的位置

##### 概述

- 类模型的位置
  - 加载的类在JVM中创建相应的类结构，类结构会存储在**方法区**（JDK1.8之前：永久代；JDK1.8之后：元空间）
- Class实例的位置
  - 类将.Class文件加载至元空间后，会在堆中创建一个java.lang.Class对象，用来封装类位于方法区内的数据结构，该Class对象是在加载类的过程中创建的，每个类都对应有一个Class类型的对象
- 图示
  - ![image-20230110140540689](/../JVM.assets/image-20230110140540689.png)
  - 外部可以通过访问代表Order类的Class对象来获取Order的类数据结构
- Class类的构造方法时私有的，只有JVM能够创建
- **java.lang.Class实例是访问类型元数据的接口，也是实现反射的关键数据、入口**。通过Class类提供的接口，可以获得目标类所关联的. class文件中具体的数据结构：方法、字段等信息。

##### 实例

```java
public class LoadingTest {
    public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String");
            //获取当前运行时类声明的所有方法
            Method[] ms = clazz.getDeclaredMethods();
            for (Method m : ms) {
                //获取方法的修饰符
                String mod = Modifier.toString(m.getModifiers());
                System.out.print(mod + " ");
                //获取方法的返回值类型
                String returnType = m.getReturnType().getSimpleName();
                System.out.print(returnType + " ");
                //获取方法名
                System.out.print(m.getName() + "(");
                //获取方法的参数列表
                Class<?>[] ps = m.getParameterTypes();
                if (ps.length == 0) System.out.print(')');
                for (int i = 0; i < ps.length; i++) {
                    char end = (i == ps.length - 1) ? ')' : ',';
                    //获取参数的类型
                    System.out.print(ps[i].getSimpleName() + end);
                }
                System.out.println();
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }
}
```

#### 数组类的加载

创建数组类的情况稍微有些特殊，因为**数组类本身并不是由类加载器负责创建**，而是由JVM在运行时根据需要而直接创建的，但数组的元素类型仍然需要依靠类加载器去创建。创建数组类(下述简称A)的过程:

1. 如果数组的元素类型是引用类型，那么就遵循定义的加载过程递归加载和创建数组A的元素类型;
2. JVM使用指定的元素类型和数组维度来创建新的数组类。

如果数组的元素类型是引用类型，数组类的可访问性就由元素类型的可访问性决定。否则数组类的可访问性将被缺省定义为public。

### Linking（链接）

#### Verification(验证)

- **他的目的是保证加载的字节码是合法、合理并符合规范的**
- 验证的步骤比较复杂，实际要验证的项目也很繁多，大提升java虚拟机需要以下检查：
  - ![image-20230110141130457](/../JVM.assets/image-20230110141130457.png)
- 整体说明

  - 验证的内容则涵盖了类数据信息的格式验证、语义检查、字节码验证，以及符号引用验证等。
    - **格式验证会和加载阶段一起执行**。验证通过之后，类加载器才会成功将类的二进制数据信息加载到方法区中。
    - **格式验证之外的验证操作将会在方法区中进行**。
  - 链接阶段的验证虽然拖慢了加载速度，但是它避免了在字节码运行时还需要进行各种检查。(**磨刀不误砍柴工**)
- 具体说明

  1. **格式验证**：是否以魔数0xCAFEBABE开头，主版本和副版本号是否在当前Java虚拟机的支持范围内，数据中每一个项是否都拥有正确的长度等。
2. Java虚拟机会进行字节码的**语义检查**，但凡在语义上不符合规范的，虚拟机也不会给予验证通过。比如:
  
   - 是否所有的类都有父类的存在(在Java里，除了object外，其他类都应该有父类)
     - 是否一些被定义为final的方法或者类被重写或继承了
     - 非抽象类是否实现了所有抽象方法或者接口方法
     - 是否存在不兼容的方法(比如方法的签名除了返回值不同，其他都一样，这种方法会让虚拟机无从下手调度; abstract情况下的方法，就不能是final的了)
  3. java虚拟机还会进行**字节码验证**，字节码验证也是**验证过程中最为复杂的一个过程**。他视图通过对字节码流的分析，判断字节码是否可以被正确地执行。比如:

     - 在字节码的执行过程中，是否会跳转到一条不存在的指令
   - 函数的调用是否传递了正确类型的参数
     - 变量的赋值是不是给了正确的数据类型等
  - **栈映射帧**(StackMapTable)就是在这个阶段，**用于检测在特定的字节码处，其局部变量表和操作数栈是否有着正确的数据类型**。但遗憾的是，100%准确地判断一段字节码是否可以被安全执行是无法实现的，因此，该过程只是尽可能地检查出可以预知的明显的问题。如果在这个阶段无法通过检查，虚拟机也不会正确装载这个类。但是，如果通过了这个阶段的检查，也不能说明这个类是完全没有问题的。
     - **前面3次检查中，已经排除了文件格式错误、语义错误以及字节码的不正确性。但是依然不能确保类是没有问题的。**

  4. 校验器还将进行**符号引用的验证**。Class文件在其常量池会通过字符串记录自己将要使用的其他类或者方法。因此，**在验证阶段，虚拟机就会检查这些类或者方法确实是存在的**，并且当前类有权限访问这些数据，如果个需要使用类无法在系统中找到，则会抛出NoClassDefFoundError ,如果一个 方法无法被找到，则会抛出NoSuchMethodError。
     - **此阶段在解析环节才会执行。**

#### Preparation(准备)

- **准备阶段，换言之就是为类的静态变量分配内存，并将其初始化为默认值**

  - 当一个类验证通过时，虚拟机就会进入准备阶段。这和阶段，虚拟机就会为这个类分配相应的内存空间，并设置默认初始值。java虚拟机为各类型变量默认的初始值如表所示：

  - | 类型      | 默认初始值 |
    | --------- | ---------- |
    | byte      | (byte)0    |
    | short     | (short)0   |
    | int       | 0          |
    | long      | 0L         |
    | float     | 0.0f       |
    | double    | 0.0        |
    | char      | \u0000     |
    | boolean   | false      |
    | reference | null       |

  - 注意：java并不支持boolean类型，对于boolean类型，内部实现是int，由于int的默认值是0，故对应的，boolean的默认值就是false.

- 注意：

  1. **这里不包含基本数据类型的字段用static final修饰的情况，因为final在编译的时候就会分配了，准备阶段会显式赋值。**
     - 非final修饰的变量，在准备环节进行默认初始化赋值
     - final修饰以后，在准备环节进行显示赋值
  2. 注意这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到Java堆中。
  3. 在这个阶段并不会像初始化阶段中那样会有初始化或者代码被执行。

#### Resolution(解析)

- **解析阶段，换言之就是将类、接口、字段和方法的符号引用转换为直接引用**
- 具体描述
  - 符号引用就是一些字面量的引用，和虚拟机的内部数据结构和和内存布局无关。**比较容易理解的就是在Class类文件中，通过常量池进行了大量的符号引用**。但是在程序实际运行时，只有符号引用是不够的，比如当如下println()方法被调用时，**系统需要明确知道该方法的位置**。
  - ![image-20230110142014994](/../JVM.assets/image-20230110142014994.png)
  - 以方法为例，java虚拟机为每个类都准备了一张方法表，将其所有的方法都列在表中，当需要调用一个类的方法的时候，只要知道这个方法在方法表中的偏移量就可以直接调用该方法。**通过解析操作，符号引用就可以转变为目标方法在类中方法表中的位置，从而是的方法被成功调用。**
- 小结
  - 所谓解析就是将符号引用转为直接引用，也就是得到类、字段、方法在内存中的指针或者偏移量。因此，可以说，如果直接引用存在，那么可以肯定系统中存在该类、方法或者字段。但只存在符号引用，不能确定系统中一定存在该结构。
  - 不过Java虚拟机规范并没有明确要求解析阶段一定 要按照顺序执行。在HotSpot VM中， 加载、验证、准备和初始化会按照顺序有条不紊地执行，但链接阶段中的解析操作往往会伴随着JVM在执行完初始化之后再执行。
- 字符串的复习
  - 最后，再来看一下CONSTANT_String的解析。由于字符串在程序开发中有着重要的作用，因此，读者有必要了解一下String在Java虛拟机中的处理。**当在Java代码中直接使用字符串常量时，就会在类中出现CONSTANT_String**，它表示字符串常量，并且会引用一个CONSTANT_UTF8的常量项。**在Java虛拟机内部运行中的常量池中，会维护一张字符串拘留表(intern)，它会保存所有出现过的字符串常量，并且没有重复项**。只要以CONSTANT_String形式出现的字符串也都会在这张表中。使用String.intern()方法可以得到一个字符串在拘留表（**字符串常量池**）中的引用，因为该表中没有重复项，所以任和字面相同的字符串的String.intern()方法返回总是相同的。

### Initialzation（初始化）

#### 概述

**初始化阶段，换言之就是为类的静态变量赋予正确的初始值**

1. 具体描述
   - **类的初始化是类装载的最后一个阶段**。如果前面的步骤都没有问题，那么表示类可以顺利装载到系统中。此时，类才会开始执行Java字节码。(即: **到了初始化阶段，才真正开始执行类中定义的Java 程序代码**。)
   - **初始化阶段的重要工作是执行类的初始化方法:< clinit >()方法。**
     - 该方法仅能由Java编译器生成并由JVM调用，程序开发者无法自定义一个同名的方法，更无法直接在Java程序中调用该方法，虽然该方法也是由字节码指令所组成。
     - 它是由**类静态成员的赋值语句**以及**static语句块**合并产生的。
2. 说明
   - 在加载一个类之前，虚拟机总是会试图加载该类的父类，因此父类的< clinit> 总是在子类< clinit>之前被调用。也就是说，父类的static块优先级高于子类。
     - 由父及子，静态先行
   - Java编译器并不会为所有的类都产生< clinit>()初始化方法。哪些类在编译为字节码后，字节码文件中将不会包含< clinit >()方法？
     - 一个类中并没有声明任何的类变量，也没有静态代码块时
     - 一个类中声明类变量，但是没有明确使用类变量的初始化语句以及静态代码块来执行初始化操作时
     - 一个类中包含static final修饰的基本数据类型的字段，这些类字段初始化语句采用编译时场连败表达式。

#### static和final的搭配问题

**哪些场景下，java编译器不会生成< clint >()方法？**

```java
public class InitializationTest1 {
    //场景1：对应非静态的字段，不管是否进行了显式赋值，都不会生成<clinit>()方法
    public int num = 1;
    //场景2：静态的字段，没有显式的赋值，不会生成<clinit>()方法
    public static int num1;
    //场景3：比如对于声明为static final的基本数据类型的字段，不管是否进行了显式赋值，都不会生成<clinit>()方法
    public static final int num2 = 1;
}
```

**使用static + final修饰的字段的显式赋值的操作，到底是在哪个阶段进行的赋值？**

- 情况1：在链接阶段的准备环节赋值
- 情况2：在初始化阶段< clinit >()中赋值

+++

结论：

- 在链接阶段的准备环节赋值的情况：
  1. 对于**基本数据类型**的字段来说，如果使用static final修饰，则显式赋值(直接赋值常量，而非调用方法）通常是在链接阶段的准备环节进行
  2. 对于**String**来说，如果使用字面量的方式赋值，使用static final修饰的话，则显式赋值通常是在链接阶段的准备环节进行
- 在初始化阶段< clinit>()中赋值的情况：
  - 排除上述的在准备环节赋值的情况之外的情况。

**最终结论：使用static + final修饰，且显示赋值中不涉及到方法或构造器调用的基本数据类型或String类型的显式赋值，是在链接阶段的准备环节进行。**

```java
public class InitializationTest2 {
    public static int a = 1;//在初始化阶段<clinit>()中赋值
    public static final int INT_CONSTANT = 10;//在链接阶段的准备环节赋值

    public static final Integer INTEGER_CONSTANT1 = Integer.valueOf(100);//在初始化阶段<clinit>()中赋值
    public static Integer INTEGER_CONSTANT2 = Integer.valueOf(1000);//在初始化阶段<clinit>()中赋值

    public static final String s0 = "helloworld0";//在链接阶段的准备环节赋值
    public static final String s1 = new String("helloworld1");//在初始化阶段<clinit>()中赋值

    public static String s2 = "helloworld2";

    public static final int NUM1 = new Random().nextInt(10);//在初始化阶段<clinit>()中赋值
}
```

#### < clinit>()的线程安全性

##### 概述

- 对于< clinit >()方法的调用， 也就是类的初始化，虚拟机会在内部确保其多线程环境中的安全性。

- 虚拟机会保证一个类的< clinit>()方法在多线程环境中被正确地加锁、同步，如果多个线程同时去初始化一个类， 那么只会有一个线程去执行这个类的< clinit>()方法， 其他线程都需要阻塞等待，直到活动线程执行< clinit>()方法完毕。

- 正是**因为函数< clinit>( )带锁线程安全的**，因此，如果在一个类的< clinit>()方法中有耗时很长的操作，就可能造成多个线程阻塞，引发死锁。并且这种死锁是很难发现的，因为看起来它们并没有可用的锁信息。

- 如果之前的线程成功加载了类，则等在队列中的线程就没有机会再执行< clinit>()方法了。那么，当需要使用这个类时，虚拟机会直接返回给它已经准备好的信息。

##### 实例（第三点）

```java
class StaticA {
    static {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        try {
            Class.forName("com.atguigu.java1.StaticB");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        System.out.println("StaticA init OK");
    }
}

class StaticB {
    static {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        try {
            Class.forName("com.atguigu.java1.StaticA");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        System.out.println("StaticB init OK");
    }
}

public class StaticDeadLockMain extends Thread {
    private char flag;

    public StaticDeadLockMain(char flag) {
        this.flag = flag;
        this.setName("Thread" + flag);
    }

    @Override
    public void run() {
        try {
            Class.forName("com.atguigu.java1.Static" + flag);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        System.out.println(getName() + " over");
    }

    public static void main(String[] args) throws InterruptedException {
        StaticDeadLockMain loadA = new StaticDeadLockMain('A');
        loadA.start();
        StaticDeadLockMain loadB = new StaticDeadLockMain('B');
        loadB.start();
    }
}
```

#### 类的初始化情况：主动使用vs被动使用

**如果针对代码，设置参数-XX:+TraceClassLoading，可以追踪类的加载信息并打印出来。**

##### 主动使用

Class只有在必须要首次使用的时候才会被装载，Java虚拟机不会无条件地装载Class类型。Java虚拟机规定，一个类或接口在初次使用前，必须要进行初始化。这里指的“使用”，是指主动使用，主动使用只有下列几种情况：(即: **如果出现如下的情况，则会对类进行初始化操作。而初始化操作之前的加载、验证、准备已经完成**。)

1. 当创建一个类的实例时，比如使用new关键字，或者通过反射、克隆、反序列化。
2. 当调用类的静态方法时，即当使用了字节码invokestatic指令。
3. 当使用类、接口的静态字段时(final修饰特殊考虑)， 比如，使用getstatic或者putstatic指令。 (对应访问变量、赋值变量操作)
4. 当使用java. lang. reflect包中的方法反射类的方法时。比如: Class . forName( "com. atguigu. java. Test " )
5. 当初始化子类时，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。
   - 当Java虚拟机初始化一个类时，要求它的所有父类都已经被初始化，但是这条规则并不适用于接口。
     - 在初始化一个类时，并不会先初始化它所实现的接口
     - 在初始化一个接口时，并不会先初始化它的父接口
   - 因此，一个父接口并不会因为它的子接口或者实现类的初始化而初始化。只有当程序首次使用特定接口的静态字段时，才会导致该接口的初始化。
6. 如果一个接口定义了default方法，那么直接实现或者间接实现该接口的类的初始化，该接口要在其之前被初始化。
7. 当虚拟机启动时，用户需要指定一个要执行的主类(包含main()方法的那个类)，虚拟机会先初始化这个主类。
   - JVM启动的时候通过引导类加载器加载一个初始类。这个类在调用public static void main(String[])方法之 前被链接和初始化。这个方法的执行将依次导致所需的类的加载，链接和初始化。
8. 当初次调用MethodHandle 实例时，初始化该MethodHandle 指向的方法所在的类。 (涉及解析REF_ getStatic、 REF_ putStatic、 REF_ invokeStatic 方法句柄对应的类)

##### 被动使用

除了以上的情况属于主动使用，其他的情况均属于被动使用。**被动使用不会引起类的初始化**。

也就是说：**并不是在代码中出现的类，就一定会被加载或者初始化。如果不符合主动使用的条件，类就不会初始化**。

1. 当访问一个静态字段时，只有真正声明这个字段的类才会被初始化。
   - **当通过子类引用父类的静态变量，不会导致子类初始化。**
2. 通过数组定义类引用，不会触发此类的初始化
3. 引用常量不会触发此类或接口的初始化。因为常量在链接阶段就已经被显式赋值了。
4. 调用ClassLoader类的loadClass()方法加载一个类，并不是对类的主动使用，不会导致类的初始化。

##### 实例1：主动使用

```java
/**
 *
 * 测试类的主动使用：意味着会调用类的<clinit>()，即执行了类的初始化阶段
 *
 * 1. 当创建一个类的实例时，比如使用new关键字，或者通过反射、克隆、反序列化。
 * 2. 当调用类的静态方法时，即当使用了字节码invokestatic指令。
 */
public class ActiveUse1 {
    public static void main(String[] args) {
        Order order = new Order();
    }

    //序列化的过程：
    @Test
    public void test1() {
        ObjectOutputStream oos = null;
        try {
            oos = new ObjectOutputStream(new FileOutputStream("order.dat"));

            oos.writeObject(new Order());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (oos != null)
                    oos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    //反序列化的过程：（验证）
    //1. 当创建一个类的实例时，比如使用new关键字，或者通过反射、克隆、反序列化
    @Test
    public void test2() {
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("order.dat"));

            Order order = (Order) ois.readObject();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                if (ois != null)
                    ois.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    //2. 当调用类的静态方法时，即当使用了字节码invokestatic指令。
    @Test
    public void test3(){
        Order.method();
    }

}

class Order implements Serializable{
    static {
        System.out.println("Order类的初始化过程");
    }

    public static void method(){
        System.out.println("Order method()....");
    }
}
```

##### 实例2：主动使用

```java
/**
 *
 * 3. 当使用类、接口的静态字段时(final修饰特殊考虑)，比如，使用getstatic或者putstatic指令。（对应访问变量、赋值变量操作）
 */
public class ActiveUse2 {
    @Test
    public void test1(){
      //System.out.println(User.num);//不会
	  //System.out.println(User.num1);//不会
        System.out.println(User.num2);//会
    }

    @Test
    public void test2(){
	  //System.out.println(CompareA.NUM1);//不会
        System.out.println(CompareA.NUM2);//会
    }
}

class User{
    static{
        System.out.println("User类的初始化过程");
    }

    public static int num = 1;
    public static final int num1 = 1;
    public static final int num2 = new Random().nextInt(10);

}

interface CompareA{
    public static final Thread t = new Thread(){
        {
            System.out.println("CompareA的初始化");
        }
    };
    public static final int NUM1 = 1;
    public static final int NUM2 = new Random().nextInt(10);

}
```

##### 实例3：主动使用

```java
/**
 *
 * 4. 当使用java.lang.reflect包中的方法反射类的方法时。比如：Class.forName("com.atguigu.java.Test")
 * 5. 当初始化子类时，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。
 * 6. 如果一个接口定义了default方法，那么直接实现或者间接实现该接口的类的初始化，该接口要在其之前被初始化。
 * 7. 当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类。
 * 8. 当初次调用 MethodHandle 实例时，初始化该 MethodHandle 指向的方法所在的类。（涉及解析REF_getStatic、REF_putStatic、REF_invokeStatic方法句柄对应的类）
 */
public class ActiveUse3 {
    static{
        System.out.println("ActiveUse3的初始化过程");
    }
    @Test
    public void test1() {
        try {
            Class clazz = Class.forName("com.atguigu.java1.Order");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }

    @Test
    public void test2() {
        System.out.println(Son.num);
    }

    @Test
    public void test3(){
        System.out.println(CompareC.NUM1);
    }

    @Test
    public void test4() {
        System.out.println(Son.num);
    }

    public static void main(String[] args) {
        System.out.println("hello");
    }
}


class Father {
    static {
        System.out.println("Father类的初始化过程");
    }
}

class Son extends Father implements CompareB{
    static {
        System.out.println("Son类的初始化过程");
    }

    public static int num = 1;
}

interface CompareB {
    public static final Thread t = new Thread() {
        {
            System.out.println("CompareB的初始化");
        }
    };
    //默认方法
    public default void method1(){
        System.out.println("你好！");
    }

}

interface CompareC extends CompareB {
    public static final Thread t = new Thread() {
        {
            System.out.println("CompareC的初始化");
        }
    };

    public static final int NUM1 = new Random().nextInt();
}
```

##### 实例4：被动使用

```java
/**
 *
 * 关于类的被动使用，即不会进行类的初始化操作，即不会调用<clinit>()
 *
 * 1. 当访问一个静态字段时，只有真正声明这个字段的类才会被初始化。
 * 	  		当通过子类引用父类的静态变量，不会导致子类初始化
 * 2. 通过数组定义类引用，不会触发此类的初始化
 *
 * 说明：没有初始化的类，不意味着没有加载！
 */
public class PassiveUse1 {
    @Test
    public void test1(){
        System.out.println(Child.num);
    }

    @Test
    public void test2(){
        Parent[] parents = new Parent[10];
        System.out.println(parents.getClass());
        System.out.println(parents.getClass().getSuperclass());

        parents[0] = new Parent();
        parents[1] = new Parent();
    }
}

class Parent{
    static{
        System.out.println("Parent的初始化过程");
    }

    public static int num = 1;
}

class Child extends Parent{
    static{
        System.out.println("Child的初始化过程");
    }
}
```

##### 实例5：被动使用

```java
/**
 *
 *  * 3. 引用常量不会触发此类或接口的初始化。因为常量在链接阶段就已经被显式赋值了。
 *  * 4. 调用ClassLoader类的loadClass()方法加载一个类，并不是对类的主动使用，不会导致类的初始化。
 */
public class PassiveUse2 {
    @Test
    public void test1(){
//        System.out.println(Person.NUM);
        System.out.println(Person.NUM1);
    }

    @Test
    public void test2(){
//        System.out.println(SerialA.ID);
        System.out.println(SerialA.ID1);
    }

    @Test
    public void test3(){
        try {
            Class clazz = ClassLoader.getSystemClassLoader().loadClass("com.atguigu.java1.Person");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

}

class Person{
    static{
        System.out.println("Person类的初始化");
    }
    public static final int NUM = 1;//在链接过程的准备环节就被赋值为1了。
    public static final int NUM1 = new Random().nextInt(10);//此时的赋值操作需要在<clinit>()中执行
}

interface SerialA{
    public static final Thread t = new Thread() {
        {
            System.out.println("SerialA的初始化");
        }
    };

    int ID = 1;
    int ID1 = new Random().nextInt(10);//此时的赋值操作需要在<clinit>()中执行
}
```

### Using（使用）

- 任何一个类型在使用之前都必须经历过完整的加载、链接和初始化3个类加载步骤。一旦一个类型成功经历过这3个步骤之后，便“万事俱备，只欠东风”，就等着开发者使用了。

- 开发人员可以在程序中访问程调用它的静态类成员信息(比如:静态字段、静态方法)，或者使用new关键字为其创建对象实例。

### Unloading（卸载）

#### 一、 类、类的加载器、类的实例之间的引用关系
**在类加载器的内部实现中，用一个Java集合来存放所加载类的引用。另一方面，一个Class对象总是会引用它的类加载器调用Class对象的getClassLoader()方法，就能获得它的类加载器**。由此可见，代表某个类的Class实例与其类的加载器之间为**双向关联**关系。

一个类的实例总是引用代表这个类的Class对象。在0bject类中定义了getClass()方法，这个方法返回代表对象所属类的Class对象的引用。此外，所有的Java类都有一个静态属性class,它引用代表这个类的Class对象。 

#### 二、类的生命周期
当Samplq类被加载、链接和初始化后，它的生命周期就开始了。当代表Sample类的Class对 象不再被引用，即不可触及时，Class对象就会结束生命周期，Sample类在方法区内的数据也会被卸载，从而结束Samp1e类的生命周期。

**一个类何时结束生命周期，取决于代表它的Class对象何时结束生命周期。**

#### 三、具体例子

![image-20230110143627633](/../JVM.assets/image-20230110143627633.png)

loader1变量和obj变量间接应用代表Sample类的Class对象，而objClass变量则直接引用它。

如果程序运行过程中，将上图左侧三个引用变量都置为null,此时Sample对象结束生命周期，MyClassLoader对象结束生命周期，代表Sample类的Class对象也结束生命周期，Sample类在方法区内的二进制数据被卸载。

当再次有需要时，会检查Sample类的Class对象是否存在，如果存在会直接使用，不再重新加载；如果不存在Sample类会被重新加载，在Java虛拟机的堆区会生成一个新的代表Sample类的Class实例（可以通过哈希码查看是否是同一个实例）。

#### 类的卸载

1. 启动类加载器加载的类型在整个运行期间是不可能被卸载的(/../Jvm和jls规范)
2. 被系统类加载器和扩展类加载器加载的类型在运行期间不太可能被卸载，因为系统类加载器实例或者扩展类的实例基本上在整个运行期间总能直接或者间接的访问的到，其达到unreachable的可能性极小。
3. 被开发者自定义的类加载器实例加载的类型只有在很简单的上下文环境中才能被卸载，而且一般还要借助于强制调用虚拟机的垃圾收集功能才可以做到。可以预想，稍微复杂点的应用场景中(比如：很多时候用户在开发自定义类加载器实例的时候采用缓存的策略以提高系统性能)，被加载的类型在运行期间也是几乎不太可能被卸载的(至少卸载的时间是不确定的)。

**综合以上三点，一个已经加载的类型被卸载的几率很小至少被卸载的时间是不确定的。同时我们可以看的出来，开发者在开发代码时候，不应该对虚拟机的类型卸载做任何假设的前提下，来实现系统中的特定功能。**

#### 回顾：方法区的垃圾回收

方法区的垃圾收集主要回收两部分内容：**常量池中废弃的常量**和**不再使用的类型**。

##### **常量池中废弃的常量**

只要常量池中的常量没有被任何地方引用，就可以被回收

##### 不再使用的类型

需要同时满足三个条件：

- 该类所有的实例都已经被回收。也就是Java堆中不存在该类及其任何派生子类的实例。（下）

- 加载该类的类加载器已经被回收。这个条件除非是经过精心设计的可替换类加载器的场景，如OSGi、JSP的重加载等，否则通常是很难达成的。（上）

- 该类对应的java. lang. Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。（中）

## 类的加载器

### 概述

类加载器 JVM执行类加载机制的前提

ClassLoader的作用：

ClassLoader是Java的核心组件，所有的Class都是由ClassLoader进行加载的，ClassLoader 负责通过各种方式将Class信息的二进制数据流读入JVM内部，转换为一个 与目标类对应的java. lang. Class对象实例。然后交给Java虚拟机进行链接、初始化等操作。因此，**ClassLoader在整 个装载阶段，只能影响到类的加载，而无法通过ClassLoader去改变类的链接和初始化行为**。至于它是否可以运行，则由Execution Engine决定。

![image-20230110144039890](/../JVM.assets/image-20230110144039890.png)

类加载器最早出现在Java1.0版本中，那个时候只是单纯地为了满足JavaApplet应用而被研发出来。但如今类加载器却在**OSGi（热部署）**、**字节码加解密**领域大放异彩。这主要归功于Java虚拟机的设计者们当初在设计类加载器的时候，并没有考虑将它绑定在JVM内部，这样做的好处就是能够更加灵活和动态地执行类加载操作。

#### 大厂面试题

- 蚂蚁金服:
  - 深入分析ClassLoader,双亲委派机制
  - 类加载器的双亲委派模型是什么?
  - 一面:双亲委派机制及使用原因
- 百度：
  - 都有哪些类加载器，这些类加载器都加载哪些文件?
  - 手写一个类加载器Demo
  - Class的forName("java. lang. String" )和Class 的getClassLoader( )的loadClass("java. lang. String")有什么区别?
    - 使用 loadClass() 方法获得的 Class 对象只完成了类加载过程中的第一步：加载，后续的操作均未进行。
    - 使用 Class.forName() 方法获得 Class 对象是已经执行完初始化的了
- 腾讯:
  - 什么是双亲委派模型?
  - 类加载器有哪些?
- 小米:
  - 双亲委派模型介绍一下

#### 类加载的分类

##### 概述

类的加载分类:显式加载vs隐式加载

class文件的显式加载与隐式加载的方式是**指JVM加载class文件到内存的方式**。

- 显式加载指的是在代码中通过调用ClassLoader加载class对象， 如直接使用Class. forName(name)或this . getClass(). getClassLoader(). loadClass( )加载class对象。
- 隐式加载则是不直接在代码中调用ClassLoader的方法加载class对象，而是通过虚拟机自动加载到内存中，如在加载某个类的class文件时，该类的class 文件中引用了另外一个类的对象，此时额外引用的类将通过JVM自动加载到内存中。

在日常开发以上两种方式一般会混合使用。

##### 实例

```java
public class User {
    private int id;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                '}';
    }
}
```

```java
public class UserTest {
    public static void main(String[] args) {
        User user = new User(); //隐式加载

        try {
            Class clazz = Class.forName("com.atguigu.java.User"); //显式加载
            ClassLoader.getSystemClassLoader().loadClass("com.atguigu.java.User");//显式加载
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }
}
```

#### 类加载器的必要性

平时并不需要显示加地使用类加载器，但是了解类加载器的加载机制很重要：

- 避免在开发中遇到java.lang.ClasgNotFoundException异常或java.lang. NoClassDefFoundError异常时，手足无措。只有了解类加载器的加载机制才能够在出现异常的时候快速地根据错误异常日志定位问题和解决问题

- 需要支持类的动态加载或需要对编译后的字节码文件进行加解密操作时，就需要与类加载器打交道了。

- 开发人员可以在程序中编写自定义类加载器来重新定义类的加载规则，以便实现一些自定义的处理逻辑。

#### 命名空间

##### 概述

1. 何为类的唯一性?
   - 对于任意一个类，**都需要由加载它的类加载器和这个类本身一同确认其在Java虚拟机中的唯一性**。每一个类加载器，都拥有一个独立的类名称空间：**比较两个类是否相等，只有在这两个类是由同一个类加载器加载的前提下才有意义**。否则，即使这两个类源自同一个Class文件， 被同一个虚拟机加载，只要加载他们的类加载器不同，那这两个类就必定不相等。
2. 命名空间
   - 每个类加载器都有自己的命名空间，命名空间由该加载器及所有的父加载器所加载的类组成
   - 在同一命名空间中，不会出现类的完整名字(包括类的包名)相同的两个类
   - 在不同的命名空间中，有可能会出现类的完整名字(包括类的包名)相同的两个类

**在大型应用中，我们往往借助这一特性， 来运行同一个类的不同版本。**

##### 实例

```java
public class UserClassLoader extends ClassLoader {
    private String rootDir;

    public UserClassLoader(String rootDir) {
        this.rootDir = rootDir;
    }

    /**
     * 编写findClass方法的逻辑
     */
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        // 获取类的class文件字节数组
        byte[] classData = getClassData(name);
        if (classData == null) {
            throw new ClassNotFoundException();
        } else {
            //直接生成class对象
            return defineClass(name, classData, 0, classData.length);
        }
    }

    /**
     * 编写获取class文件并转换为字节码流的逻辑 * @param className * @return
     */
    private byte[] getClassData(String className) {
        // 读取类文件的字节
        String path = classNameToPath(className);
        try {
            InputStream ins = new FileInputStream(path);
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len = 0;
            // 读取类文件的字节码
            while ((len = ins.read(buffer)) != -1) {
                baos.write(buffer, 0, len);
            }
            return baos.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     * 类文件的完全路径
     */
    private String classNameToPath(String className) {
        return rootDir + "\\" + className.replace('.', '\\') + ".class";
    }

    public static void main(String[] args) {
        String rootDir = "E:\\百度云\\JVM\\JVM中\\代码\\JVMDemo1\\chapter04\\src\\";

        try {
            //创建自定义的类的加载器1
            UserClassLoader loader1 = new UserClassLoader(rootDir);
            Class clazz1 = loader1.findClass("com.atguigu.java.User");

            //创建自定义的类的加载器2
            UserClassLoader loader2 = new UserClassLoader(rootDir);
            Class clazz2 = loader2.findClass("com.atguigu.java.User");

            System.out.println(clazz1 == clazz2); //clazz1与clazz2对应了不同的类模板结构。
            System.out.println(clazz1.getClassLoader());
            System.out.println(clazz2.getClassLoader());

            //######################
            Class clazz3 = ClassLoader.getSystemClassLoader().loadClass("com.atguigu.java.User");
            System.out.println(clazz3.getClassLoader());


            System.out.println(clazz1.getClassLoader().getParent());

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

运行结果

![image-20230110145702854](/../JVM.assets/image-20230110145702854.png)

#### 类加载机制的基本特征

- 双亲委派模型。但不是所有类加载都遵守这个模型，有的时候，启动类加载器所加载的类型，是可能要加载用户代码的，比如JDK内部的ServiceProvider/ServiceLoader机制，用户可以在标准API框架上，提供自己的实现，JDK也需要提供些默认的参考实现。例如，Java 中JNDI、 JDBC、文件系统、Cipher等很多 方面，都是利用的这种机制，这种情况就不会用双亲委派模型去加载，而是利用所谓的上下文加载器。

- 可见性，子类加载器可以访问父加载器加载的类型，但是反过来是不允许的。不然，因为缺少必要的隔离，我们就没有办法利用类加载器去实现容器的逻辑。

- 单一性，由于父加载器的类型对于子加载器是可见的，所以父加载器中加载过的类型，就不会在子加载器中重复加载。但是注意，类加载器“邻居”间，同一类型仍然可以被加载多次，因为互相并不可见。

### 类加载器的分类

JVM支持两种类型的类加载器，分别为**引导类加载器**(Bootstrap ClassLoader)和**自定义类加载器**(User-Defined ClassLoader)。

从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是Java虚拟机规范却没有这么定义，而是将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器。无论类加载器的类型如何划分，在程序中我们最常见的类加载器结构主要是如下情况:

![image-20211130110955596](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211130110955596.png)

除了顶层的启动类加载器外，其余的类加载器都应当有自己的“父类”加载器

不同类加载器看似是继承关系，实际上是包含关系。在下层加载器中，包含着上层加载器的应用。

#### 引导类加载器

- 用C/C++编写，在JVM内部，它用来加载JAVA 的核心类库。用于提供JVM自身需要的类

- 并不继承自java.lang.ClassLoader，没有父加载器。

- 出于安全考虑，Bootstrap启动类加载器，只加载包名为java、javax、sun等开头的类

- 加载扩展类和应用程序类加载器，并指定为他们的父类加载器

#### 扩展类加载器

- java语言编写，有sun.misc.Launcher$ExtClassLoader实现
- 继承于ClassLoader类
- 父类加载器为启动加载器
- 从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的jre/lib/ext自录下加载类库。如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载

#### 系统类记载器(应用程序类加载器)

- java语言编写，由sun. misc. Launcher$AppClassLoader实现
- 继承于ClassLoader类
- 父类加载器为扩展类加载器
- 它负责加载环境变量classpath或系统属性java.class.path 指定路径下的类库
- **应用程序中的类加载器默认是系统类加载器。**
- 它是用户自定义类加载器的默认父加载器
- 通过ClassLoader的getSystemClassLoader( )方法可以获取到该类加载器

#### 用户自定义的加载器

- 在Java的日常应用程序开发中，类的加载几乎是由上述3种类加载器相互配合执行的。在必要时，我们还可以自定义类加载器，来定制类的加载方式。
- 体现Java语言强大生命力和巨大魅力的关键因素之一便是,Java开发者可以自定义类加载器来实现类库的动态加载，加载源可以是本地的JAR包，也可以是网络上的远程资源。
- **通过类加载器可以实现非常绝妙的插件机制**，这方面的实际应用案例举不胜举。例如，著名的OSGI组件框架，再如Eclipse的插件机制。类加载器为应用程序提供了一种动态增加新功能的机制，这种机制无须重新打包发布应用程
  序就能实现。
- 同时，**自定义加载器能够实现应用隔离**，例如Tomcat，Spring等中间件和组件框架都在内部实现了自定义的加载器，并通过自定义加载器隔离不同的组件模块。这种机制比C/C++程序要好太多，想不修改C/C++程序就能为其新增功能，几乎是不可能的，仅仅一个兼容性便能阻挡住所有美好的设想。
- 自定义类加载器通常需要继承于ClassLoader.

### 测试不同的类加载器

每个Class对象都会包含一个定义它的ClassLoader的一个引用

#### 获取ClassLoader的途径

- 方式一:获取当前类的ClassLoader
  clazz. getClassLoader ()
- 方式二:获取当前线程上下文的ClassLoader
  Thread. currentThread() .getContextClassLoader ()
- 方式三:获取系统的ClassLoader
  ClassLoader.getSystemClassLoader ()
- 方式四:获取调用者的ClassLoader
  DriverManager . getCallerClassLoader ()

#### 说明

站在程序的角度看，引导类加载器与另外两种类加载器(系统类加载器和扩展类加载器)并不是同一个层次意义上的加载器，引导类加载器是使用C++语言编写而成的，而另外两种类加载器则是使用Java语言编写而成的。由于引导类加载器压根儿就不是个Java类， 因此在Java程序中只能打印出空值。

数组类的Class对象，不是由类加载器去创建的，而是在Java运行期JVM根据需要自动创建的。对于数组类的类加载器来说，是通过Class. getClassLoader()返回的，与数组当中元素类型的类加载器是一样的；如果数组当中的元素类型是基本数据类型，数组类是没有类加载器的。

#### 实例

```java
public class ClassLoaderTest1 {
    public static void main(String[] args) {
        //获取系统该类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2
        //获取扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader);//sun.misc.Launcher$ExtClassLoader@1540e19d
        //试图获取引导类加载器：失败
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader);//null

        //###########################
        try {
            ClassLoader classLoader = Class.forName("java.lang.String").getClassLoader();
            System.out.println(classLoader);
            //自定义的类默认使用系统类加载器
            ClassLoader classLoader1 = Class.forName("com.atguigu.java.ClassLoaderTest1").getClassLoader();
            System.out.println(classLoader1);

            //关于数组类型的加载:使用的类的加载器与数组元素的类的加载器相同
            String[] arrStr = new String[10];
            System.out.println(arrStr.getClass().getClassLoader());//null:表示使用的是引导类加载器

            ClassLoaderTest1[] arr1 = new ClassLoaderTest1[10];
            System.out.println(arr1.getClass().getClassLoader());//sun.misc.Launcher$AppClassLoader@18b4aac2

            int[] arr2 = new int[10];
            System.out.println(arr2.getClass().getClassLoader());//null:不需要类的加载器


            System.out.println(Thread.currentThread().getContextClassLoader());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

运行结果

![image-20230110150541538](/../JVM.assets/image-20230110150541538.png)

### Classloader原码解析

ClassLoader与现有类加载器的关系：

![image-20230110150612208](/../JVM.assets/image-20230110150612208.png)

除了以上虚拟机自带的加载器外，用户还可以定制自己的类加载器。java提供了抽象类java.lang.ClassLoader，所有用户自动以的类加载器都应该继承ClassLoader类。

#### Classloader的主要方法

- public final ClassLoader getParent( )

  - 返回该类加载器的超类加载器

- public Class<?> loadClass(String name) throws ClassNotFoundException

  - 加载名称为name的类，返回结果为java.lang. Class类的实例。如果找不到类，则返回ClassNotFoundException异常。**该方法中的逻辑就是双亲委派模式的实现**。

  - ClassLoader的实现方法为先从已经加载的类中寻找，如没有则继续从parentClassLoader中寻找，如仍然没找到，则从SystemClassLoader中寻找，最后再调用findClass方法来寻找，如果要改变类的加载顺序，则可覆盖此方法。（满足双亲委派机制）

  - ```java
    protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
    {
        synchronized (getClassLoadingLock(name)) {//同步操作，保证只能加载一次
            // 首先，在缓存中判断是否已经加载同名的类
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    //获取当前类加载器的父类加载器
                    if (parent != null) {
                        //如果存在父类加载器，则调用父类加载器进行类的加载
                        c = parent.loadClass(name, false);
                    } else {
                        //parent为null:父类加载器是引导类加载器
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }
    
                if (c == null) {
                    // 当前类的加载器的父类加载器未加载此类 or 当前类的加载器未加载此类
                    // 调用当前ClassLoader的findClass()
                    long t1 = System.nanoTime();
                    c = findClass(name);
    
                    // this is the defining class loader; record the stats
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {//是否进行解析操作
                resolveClass(c);
            }
            return c;
        }
    }
    ```

- protected Class<?> findClass(String name) throws ClassNotFoundException

  - 查找二进制名称为name的类，返回结果为java. lang. Class类的实例。这是一个受保护的方法， JVM鼓励我们**重写**此方法，需要自定义加载器遵循双亲委托机制，该方法会在检查完父类加载器之后被loadClass()方法调用。

  - 在URLClassLoader中做了一次重写

  - >在JDK1.2之前，在自定义类加载时，总会去继承ClassLoader类并重写loadClass方法，从而实现自定义的类加载类。但是在JDK1. 2之后已不再建议用户去覆盖loadClass()方法，而是建议把自定义的类加载逻辑写在findClass()方法中，从前面的分析可知，findClass()方法是在loadClass()方法中被调用的， 当loadClass()方法中父加载器加载失败后，则会调用自己的findClass()方法来完成类加载，这样就可以保证自定义的类加载器也符合双亲委托模式。
    >
    >需要注意的是ClassLoader类中并没有实现findClass( )方法的具体代码逻辑，取而代之的是抛出ClassNotFoundException异常，同时应该知道的是findClass()方法通常是和defineClass()方法一起使用的。**一 般情况下，在自定义类加载器时，会直接覆盖ClassLoader的findClass()方法并编写加载规则，取得要加载类的字节码后转换成流，然后调用defineClass( )方法生成类的Class对象。**

  - ```java
    protected Class<?> findClass(String name) throws ClassNotFoundException{
    	//获取类的字节数组
    	byte[] classData = getClassData(name);
    	if ( classData == null) {
    		throw new ClassNotFoundException();
    	} else {
    		//使用defineClass生成cLass对象
    		return defineClass(name,classData,0,classData.length);
    }
    
    ```

- protected final Class<?> defineClass(String name, byte[] b, int off, int len)

  - 根据给定的字节数组b转换为Class的实例，off和len 参数表示实际Class信息在byte数组中的位置和长度，其中byte数组b是ClassLoader从外部获取的。这是受保护的方法，只有在自定义ClassLoader子类中可以使用。

  - 此方法负责将二进制的字节码转换为Class对象。

  - >defineClass()方法是用来将byte字节流解析成JVM能够识别的Class对象(ClassLoader中已实现该方法逻辑)，通过这个方法不仅能够通过class文件实例化class对象，也可以通过其他方式实例化class对象，如通过网络接收一个类的字节码，然后转换为byte 字节流创建对应的Class对象。
    >
    >**defineClass()方法通常与findClass()方法一起使用，一 般情况下，在自定义类加载器时， 会直接覆盖ClassLoader的findClass()方法并编写加载规则， 取得要加载类的字节码后转换成流， 然后调用defineClass()方法生成类的Class对象**

- protected final void resolveClass(Class<?> c)

  - 链接指定的一个Java类。使用该方法可以使用类的Class对象创建完成的同时也被解析。前面我们说链接阶段主要是对字节码进行验证，为类变量分配内存并设置初始值同时将字节码文件中的符号引用转换为直接引用。
  - 此方法负责完成Class对象的链接，如已链接过，则会直接返回。

- protected final Class<?> findLoadedClass(String name)

  - 查找名称为name的已经被加载过的类，返回结果为java. lang. Class类的实例。这个方法是final方法，无法被修改。
  - 此方法负责从当前ClassLoader实例对象的缓存中寻找已加载的类，调用的为native的方法。

- private final ClassLoader parent;

  - 它也是一个ClassLoader的实例，这个字段所表示的ClassLoader也称为这个ClassLoader的双亲。在类加载的过程中, ClassLoader可能会将某些请求交予自己的双亲处理。

#### SecureClassLoader与URLClassLoader

- 接着SecureClassLoader扩展了ClassLoader, 新增了几个与使用相关的代码源(对代码源的位置及其证书的验证)和权限定义类验证(主要指对class源码的访问权限)的方法，一般我们不会直接跟这个类打交道，更多是与它的子类URLClassLoader有所关联。
- 前面说过，ClassLoader是一个抽象类，很多方法是空的没有实现， 比如findClass()、 findResource()等。 而URLClassLoader这个实现类为这些方法提供了具体的实现。并新增了URLClassPath类协助取得Class字节码流等功能。**在编写自定义类加载器时，如果没有太过于复杂的需求，可以直接继承URLClassLoader类**， 这样就可以避免自己去编写findClass( )方法及其获取字节码流的方式，使自定义类加载器编写更加简洁。

#### ExtClassLoader与AppClassLoader

- 了解完URLClassLoader后接着看看剩余的两个类加载器，即拓展类加载器ExtClassLoader和系统类加载器AppClassLoader, 这两个类都继承自URLClassLoader, 是sun . misc . Launcher的静态内部类。sun. misc. Launcher主要被系统用于启动主应用程序，ExtClassLoader和AppClassLoader都是由sun. misc. Launcher创建的
- 我们发现ExtClassLoader并没有重写loadClass()方法，这足矣说明其遵循双亲委派模式，而AppClassLoader重载了loadClass()方法，但最终调用的还是父类loadClass()方法，因此依然遵守双亲委派模式。

#### Class.forName()与Class Loader.loadClass()

- Class. forName()
  - **是一个静态方法**，最常用的是Class. forName(String className); 根据传入的类的全限定名返回一个Class 对象。<font color=red>**该方法在将Class文件加载到内存的同时，会执行类的初始化**。</font>如:
  - Class . forName(" com. atguigu. java . HelloWorld");
- ClassLoader . loadClass():
  - **这是一个实例方法**，需要一个ClassLoader 对象来调用该方法。<font color=red>**该方法将Class文件加载到内存时，并不会执行类的初始化，直到这个类第一次使用时才进行初始化**。</font>该方法因为需要得到一个ClassLoader对象,所以可以根据需要指定使用哪个类加载器.如: ClassLoader c1=.......;不会调用< clinit >
  - cl. loadClass(" com. atguigu. java . HelloWorld");

### 双亲委派机制

#### 定义与本质

- 定义
  - 如果一个类加载器在接到加载类的请求时，它首先不会自己尝试去加载这个类，而是把这个请求任务委托给父类加载器去完成，依次递归，如果父类加载器可以完成类加载任务，就成功返回。只有父类加载器无法完成此加载任务时，才自己去加载。
- 本质
  - 规定了类加载的顺序是：引导类加载器先加载，若加载不到，由扩展类加载器加载，若还加载不到，才会由系统类加载器或自定义的类加载器进行加载。

![image-20230110152156163](/../JVM.assets/image-20230110152156163.png)

#### 优势与劣势

#### 优势

- 避免了类的重复加载，确保了类的全局唯一性
- 保护程序安全，防止核心API被随意篡改

#### 实现逻辑

双亲委派机制在java.lang.ClassLoader.loadClass(String, boolean)接口中体现。该接口的逻辑如下:

1. 先在当前加载器的缓存中查找有无目标类，如果有，直接返回。
2. 判断当前加载器的父加载器是否为空，如果不为空，则调用parent.loadClass(name, false)接口进行加载。
3. 反之，如果当前加载器的父类加载器为空，则调用findBootstrapClassOrNull(name)接口，让引导类加载器进行加载。
4. 如果通过以上3条路径都没能成功加载，则调用findClass(name)接口进行加载。该接口最终会调用 java . lang.ClassLoader 接口的defineClass系列的native接口加载目标Java类。

**双亲委派的模型就隐藏在这第2和第3步中。**

#### 思考

- 如果在自定义的类加载器中重写java. lang. ClassLoader . loadClass(String)或java. lang. ClassLoader. loadClass(String, boolean)方法, 抹去其中的双亲委派机制，仅保留上面这4步中的第1步与第4步，那么是不是就能够加载核心类库了呢?

- 这也不行! 因为JDK还为核心类库提供了一层保护机制。不管是自定义的类加载器，还是系统类加载器抑或扩展类加载器，最终都必须调用java. lang. ClassLoader . defineClass(String, byte[], int, int,ProtectionDomain)方法， 而该方法会执行**preDefineClass()**接口，该接口中提供了对JDK核心类库的保护。

![image-20211130144425091](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211130144425091.png)

#### 弊端

- 检查类是否加载的委托过程是单向的，这个方式虽然从结构上说比较清晰，使各个ClassLoader的职责非常明确，但是同时会带来一个问题，即**顶层的ClassLoader无法访问底层的ClassLoader所加载的类。**

- 通常情况下，启动类加载器中的类为系统核心类，包括一些重要的系统接口，而在应用类加载器中，为应用类。按照这种模式，**应用类访问系统类自然是没有问题，但是系统类访问应用类就会出现问题**。

- 比如在系统类中提供了一个接口，该接口需要在应用类中得以实现，该接口还绑定一个工厂方法，用于创建该接口的实例，而接口和工厂方法都在启动类加载器中。这时，就会出现该工厂方法无法创建由应用类加载器加载的应用实例的问题。

#### 结论

- **由于Java虚拟机规范并没有明确要求类加载器的加载机制一定要使用双亲委派模型，只是建议采用这种方式而已。**

- 比如在Tomcat中，类加载器所采用的加载机制就和传统的双亲委派模型有一定区别，当缺省的类加载器接收到一个类的加载任务时，首先会由它自行加载，当它加载失败时，才会将类的加载任务委派给它的超类加载器去执行，这同时也是Servlet规范推荐的一种做法。

#### 破坏双亲委派机制

双亲委派机制并不是一个具有强制性约束的模型，而是java设计者推荐给开发者们的类加载器实现方法

在java的世界中，发部分的类加载器都遵循这个模型，但也有例外的情况，直到java模块化出现为止，双亲委派模型主要出现过三次较大规模“被破坏”的情况。

##### 第一次破坏双亲委派机制

双亲委派模型的第一次“被破坏”其实发生在双亲委派模型出现之前——即JDK 1.2面世以前的“远古”时代。

由于双亲委派模型在JDK 1.2之后才被引入，但是类加载器的概念和抽象类java. lang.ClassLoader则在Java的第一个版本中就已经存在，面对已经存在的用户自定义类加载器的代码，Java设计者们引入双亲委派模型时不得不做出一些妥协，**为了兼容这些已有代码，无法再以技术手段避免loadClass()被子类覆盖的可能性**，只能在JDK1.2之后的java. lang. ClassLoader中添加一个新的protected方法findClass()， 并引导用户编写的类加载逻辑时尽可能去重写这个方法，而不是在loadClass( )中编写代码。之前已经分析过loadClass()方法，双亲委派的具体逻辑就实现在这里面，按照loadClass()方法的逻辑，如果父类加载失败，会自动调用自己的findClass()方法来完成加载，这样既不影响用户按照自己的意愿去加载类，又可以保证新写出来的类加载器是符合双亲委派规则的。

##### 第二次破坏双亲委派机制

双亲委派模型的第二次“被破坏”是由这个模型自身的缺陷导致的，双亲委派很好地解决了各个类加载器协作时基础类型的一致性问题(**越基础的类由越上层的加载器进行加载**)，基础类型之所以被称为“基础”，是因为它们总是作为被用户代码继承、调用的API存在，但程序设计往往没有绝对不变的完美规则，**如果有基础打型又要调用回用户的代码，那该怎么办呢?**

这并非是不可能出现的事情，一个典型的例子便是JNDI服务，JNDI现在已经是Java的标准服务，它的代码由启动类加载器来完成加载(在JDK 1.3时加入到rt.jar的)，肯定属于Java中很基础的类型了。但JNDI存在的目的就是对资源进行查找和集中管理，它需要调用由其他厂商实现并部署在应用程序的ClassPath下的JNDI服务提供者接口(Service Provider Interface, SPI)的代码，现在问题来了，**启动类加载器是绝不可能认识、加载这些代码的，那该怎么办**? (SPI:在Java平台中， 通常把核心类rt . jar中提供外部服务、可由应用层自行实现的接口称为SPI)

为了解决这个困境，Java的设计团队只好引入了一个不太优雅的设计：**线程上下文类加载器(Thread Context ClassLoader)** 。这个类加载器可以通过java. lang . Thread类的setContextClassLoader()方法进行设置， 如果创建线程时还未设置，它将会从父线程中继承一个，如果在应用程序的全局范围内都没有设置过的话，那这个类加载器默认就是应用程序类加载器。

有了线程上下文类加载器，程序就可以做一些“舞弊”的事情了。JNDI服务使用这个线程上下文类加载器去加载所需的SPI服务代码，**这是一种父类加载器去请求子类加载器完成类加载的行为，这种行为实际上是打通了双亲委派模型的层次结构来逆向使用类加载器，已经违背了双亲委派模型的一般性原则**，但也是无可奈何的事情。Java中涉及SPI的加载基本上都采用这种方式来完成，例如JNDI、JDBC、JCE、JAXB和JBI等。不过，当SPI的服务提供者多于一个的时候，代码就只能根据具体提供者的类型来硬编码判断，为了消除这种极不优雅的实现方式，在JDK6时，JDK提供了java. util. ServiceLoader类， 以META- INF/services中的配置信息， 辅以责任链模式，这才算是给SPI的加载提供了一种相对合理的解决方案。

![image-20211130154629466](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211130154629466.png)

默认上下文加载器就是应用类记载器，这样以上下文加载器为中介，使得启动类加载器中的代码访问应用类加载器中的类。

##### 第三次破坏双亲委派机制

双亲委派模型的第三次“被破坏”是由于用户对程序动态性的追求而导致的。如:**代码热替换(Hot Swap) 、模块热部署(Hot Deployment)**等

IBM公司主导的JSR-291 (即OSGi R4.2)实现模块化热部署的关键是它自定义的类加载器机制的实现，**每一个程序模央(OSGi中称为Bundle) 都有一个自己的类加载器**，当需要更换一个Bundle时， 就把Bundle连同类加载器一起换掉以实现代码的热替换。**在OSGi环境下，类加载器不再双亲委派模型推荐的树状结构，而是进步发展为更加复杂的网状结构**。

当收到类加载请求时，OSGi将按照下面的顺序进行类搜索:

1. **将以java. *开头的类，委派给父类加载器加载。**
2. **否则，将委派列表名单内的类，委派给父类加载器加载。**
3. 否则，将Import列表中的类，委派给Export这个类的Bundle的类加载器加载。
4. 否则，查找当前Bundle的ClassPath,使用自己的类加载器加载。
5. 否则，查找类是否在自己的Fragment Bundle中， 如果在，则委派给Fragment Bundle的类加载器加载。
6. 否则，查找Dynamic Import列表的Bundle, 委派给对应Bundle的类加载器加载。
7. 否则，类查找失败。

说明:只有开头两点仍然符合双亲委派模型的原则，其余的类查找都是在平级的类加载器中进行的

##### 小结

这里，我们使用了“被破坏”这个词来形容上述不符合双亲委派模型原则的行为，但**这里“被破坏”并不一定是带有贬义的。只要有明确的目的和充分的理由，突破旧有原则无疑是一种创新**。

正如：OSGi中的类加载器的设计不符合传统的双亲委派的类加载器架构，且业界对其为了实现热部署而带来的额外的高复杂度还存在不少争议，但对这方面有了解的技术人员计本还是能达到一个共识，认识**OSGi中对类加载器的运用是值得学习的，完全弄懂了OSGi的实现，就算是掌握了类加载器的精髓**

#### 热替代的实现

##### 概述

热替换是指在程序的运行过程中，不停止服务，只通过替换程序文件来修改程序的行为。**热替换的关键需求在于服务不能中断，修改必须立即表现正在运行的系统之中**。基本上大部分脚本语言都是天生支持热替换的，比如: PHP， 只要替换了PHP源文件，这种改动就会立即生效，而无需重启Web服务器。

但对Java来说，热替换并非天生就支持，如果一个类已经加载到系统中，通过修改类文件，并无法让系统再来加载并重定义这个类。因此，在Java中实现这一功能的一个可行的方法就是灵活运用ClassLoader。

注意：由不同ClassLoader加载的同名类属于不同的类型，不能相互转换和兼容。即两个不同的ClassLoader加载同一个类，在虚拟机内部，会认为这2个类是完全不同的。

根据这个特点，可以用来模拟热替换的实现，基本思路如下图所示：

![image-20230110154338733](/../JVM.assets/image-20230110154338733.png)

##### 实例

自定义的类加载器

```java
public class MyClassLoader extends ClassLoader {
    private String rootDir;

    public MyClassLoader(String rootDir) {
        this.rootDir = rootDir;
    }

    protected Class<?> findClass(String className) throws ClassNotFoundException {
        Class clazz = this.findLoadedClass(className);
        FileChannel fileChannel = null;
        WritableByteChannel outChannel = null;
        if (null == clazz) {
            try {
                String classFile = getClassFile(className);
                FileInputStream fis = new FileInputStream(classFile);
                fileChannel = fis.getChannel();
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                outChannel = Channels.newChannel(baos);
                ByteBuffer buffer = ByteBuffer.allocateDirect(1024);
                while (true) {
                    int i = fileChannel.read(buffer);
                    if (i == 0 || i == -1) {
                        break;
                    }
                    buffer.flip();
                    outChannel.write(buffer);
                    buffer.clear();
                }

                byte[] bytes = baos.toByteArray();
                clazz = defineClass(className, bytes, 0, bytes.length);


            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    if (fileChannel != null)
                        fileChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                try {
                    if (outChannel != null)
                        outChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return clazz;
    }

    /**
     * 类文件的完全路径
     */
    private String getClassFile(String className) {
        return rootDir + "\\" + className.replace('.', '\\') + ".class";
    }
}
```

被调用的类

```java
public class Demo1 {
    public void hot() {
        System.out.println("OldDemo1");
    }
}
```

方法类

```java
public class LoopRun {
    public static void main(String args[]) {
        while (true) {
            try {
                //1. 创建自定义类加载器的实例
                MyClassLoader loader = new MyClassLoader("E:\\百度云\\JVM\\JVM中\\代码\\JVMDemo1\\chapter04\\src\\");
                //2. 加载指定的类
                Class clazz = loader.findClass("com.atguigu.java1.Demo1");
                //3. 创建运行时类的实例
                Object demo = clazz.newInstance();
                //4. 获取运行时类中指定的方法
                Method m = clazz.getMethod("hot");
                //5. 调用指定的方法
                m.invoke(demo);
                Thread.sleep(5000);
            } catch (Exception e) {
                System.out.println("not find");

                try {
                    Thread.sleep(5000);
                } catch (InterruptedException ex) {
                    ex.printStackTrace();
                }

            }
        }
    }
}
```

运行结果

![image-20230110154526240](/../JVM.assets/image-20230110154526240.png)

![image-20230110154533605](/../JVM.assets/image-20230110154533605.png)

### 沙箱安全机制

- 保证程序安全

- 保护Java原生的JDK代码

**Java安全模型的核心就是Java沙箱(sandbox)** 。什么是沙箱?沙箱是一个限制程序运行的环境。

沙箱机制就是将Java代码**限定在虚拟机(/../JVM)特定的运行范围中，并且严格限制代码对本地系统资源访问**。通过这样的措施来保证对代码的有限隔离，防止对本地系统造成破坏。

沙箱主要限制系统资源访问，那系统资源包括什么?CPU、内存、文件系统、网络。不同级别的沙箱对这些资源访问的限制也可以不一样。  

所有的Java程序运行都可以指定沙箱，可以定制安全策略。

#### JDK1.0时期

![image-20230110154555191](/../JVM.assets/image-20230110154555191.png)

#### JDK1.1时期

![image-20230110154611057](/../JVM.assets/image-20230110154611057.png)

#### JDK1.2时期

![image-20230110154621561](/../JVM.assets/image-20230110154621561.png)

#### JDK1.6时期

引入了域的概念

系统域部分专门负责与关键资源进行交互

![image-20230110154631794](/../JVM.assets/image-20230110154631794.png)

### 自定义类的加载器

#### 概述

1. 为什么要自定义类加载器?
   - **隔离加载类**
     - 在某些框架内进行中间件与应用的模块隔离，把类加载到不同的环境。比如：阿里内某容器框架通过自定义类加载器确保应用中依赖的jar包不会影响到中间件运行时使用的jar包。再比如：Tomcat这类Web应用服务器，内部自定义了好几种类加载器，用于隔离同一个Web应用服务器上的不同应用程序。（类的仲裁-->类冲突
   - **修改类加载的方式**
     - 类的加载模型并非强制，除Bootstrap外，其他的加载并非一定要引入，或者根据实际情况在某个时间点进行按需进行动态加载
   - **扩展加载源**
     - 比如从数据库、网络、甚至是电视机机顶盒进行加载
   - **防止源码泄漏**
     - Java代码容易被编译和篡改，可以进行编译加密。那么类加载也需要自定义，还原加密的字节码。
2. 常见的场景
   - 实现类似进程内隔离，类加载器实际上用作不同的命名空间，以提供类似容器、模块化的效果。例如，两个模块依赖于某个类库的不同版本，如果分别被不同的容器加载，就可以互不干扰。这个方面的集大成者是Java EE和OSGI、JPMS等框架。
   - 应用需要从不同的数据源获取类定义信息，例如网络数据源，而不是本地文件系统。或者是需要自己操纵字节码，动态修改或者生成类型。
3. 注意：
   - 在般情况下， **使用不同的类加载器去加载不同的功能模块， 会提高应用程序的安全性**。 但是，如果涉及Java类型转换则加载器反而容易产生不美好的事情。在做Java类型转换时，只有两个类型都是由同一个加载器所加载，才能进行类型转换，否则转换时会发生异常。

#### 实例

自定义的类加载器

```java
public class MyClassLoader extends ClassLoader {
    private String byteCodePath;

    public MyClassLoader(String byteCodePath) {
        this.byteCodePath = byteCodePath;
    }

    public MyClassLoader(ClassLoader parent, String byteCodePath) {
        super(parent);
        this.byteCodePath = byteCodePath;
    }

    @Override
    protected Class<?> findClass(String className) throws ClassNotFoundException {
        BufferedInputStream bis = null;
        ByteArrayOutputStream baos = null;
        try {
            //获取字节码文件的完整路径
            String fileName = byteCodePath + className + ".class";
            //获取一个输入流
            bis = new BufferedInputStream(new FileInputStream(fileName));
            //获取一个输出流
            baos = new ByteArrayOutputStream();
            //具体读入数据并写出的过程
            int len;
            byte[] data = new byte[1024];
            while ((len = bis.read(data)) != -1) {
                baos.write(data, 0, len);
            }
            //获取内存中的完整的字节数组的数据
            byte[] byteCodes = baos.toByteArray();
            //调用defineClass()，将字节数组的数据转换为Class的实例。
            Class clazz = defineClass(null, byteCodes, 0, byteCodes.length);
            return clazz;
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (baos != null)
                    baos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (bis != null)
                    bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return null;


    }
}
```

测试类

```java
public class MyClassLoaderTest {
    public static void main(String[] args) {
        MyClassLoader loader = new MyClassLoader("d:/");

        try {
            Class clazz = loader.loadClass("Demo1");
            System.out.println("加载此类的类的加载器为：" + clazz.getClassLoader().getClass().getName());

            System.out.println("加载当前Demo1类的类的加载器的父类加载器为：" + clazz.getClassLoader().getParent().getClass().getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```



#### 扩展

##### 自定义加载的步骤

1.继承ClassLoader

![image-20230110155955415](/../JVM.assets/image-20230110155955415.png)

2.重写模板方法findClass

**负责将底层二进制编码转换为class对象的方法叫做defineClass，底层必须有这个方法**

![image-20230110160004220](/../JVM.assets/image-20230110160004220.png)

3.调用defineClass（将二进制码文件转换为class类对象的方法）

![image-20230110160023127](/../JVM.assets/image-20230110160023127.png)

测试类

![image-20230110160034688](/../JVM.assets/image-20230110160034688.png)

执行过程：

先去app找，找不到再去ext找，找不到再去bootstrap找，找不到ext再找，找不到APP再找，找不到自定义找，在c://text目录下找到了class对象

##### 自加密的class（扩展）

简单加密：二进制文件经过两次异或还是原值，在异或前再加入一个seed的二进制代码片段

自己编译的时候，先异或，再减去seed二进制代码片段

防止反编译

防止篡改

##### 自定义父加载类

![image-20230110160046161](/../JVM.assets/image-20230110160046161.png)

##### 自定义热部署（Tomcat热部署的简单实现）

重写loadClass之前：

方法类：

![image-20230110160054838](/../JVM.assets/image-20230110160054838.png)

运行结果为true，说明第一次编译后，第二次编译时通过双亲委派访问到了第一次编译的结果，因此值相同。

重写loadClass之后：

![image-20230110160110199](/../JVM.assets/image-20230110160110199.png)

![image-20230110160122410](/../JVM.assets/image-20230110160122410.png)

重写的方法中，第一步就是先查找需要编译的class文件，如果找到了就直接编译，如果没有找到，就交给父加载类编译。排除了原有的判断是否加载过的这一步骤，而是直接进行编译。

方法类：

![image-20230110160132689](/../JVM.assets/image-20230110160132689.png)

运行结果为false，说明第一次编译后，第二次编译时直接重新编译了底层的class文件。

### java9的新特性

**java9中引入了模块化**，为了保证兼容性，JDK9没有从根本上改变三层类加载器架构和双亲委派模型，但为了模块化系统的顺利进行，仍然做出来一些值得注意的改动。

1. 扩展机制被移除，扩展类加载器由于向后兼容新的怨言被保留，不过被重命名为平台类加载器（platform classloader）。可以通过ClassLoader的新方法getPlatformclassloader()来获取
   - JDK 9时基于模块化进行构建（原来的rt.jar和tools.jar被拆分成数十个JMOD 文件），其中的Java 类库就已天然地满足了可扩展的需求，那自然无须再保留<JAVA_ HOME>\lib\ext目录，此前使用这个目录或者java.ext.dirs系统变量来扩展JDK功能的机制已经没有继续存在的价值了。
2. 平台类加载器和应用类加载器都不再继承自java.net.URLClassLoader.
   - 现在启动类加载器、平台类加载器、应用程序类加载器全部继承于jdk.internal.loader.BuildtinClassLoader。
   - ![image-20230110160147186](/../JVM.assets/image-20230110160147186.png)
   - 如果有程序直接继承了这种继承关系，或者依赖里URLClassLoader类的特定方法，那代码很可能会在JDK9及更高版本的JDK中崩溃。
3. 在Java 9中，类加载器有了名称。该名称在构造方法中指定，可以通过getName()方法来获取。 平台类加载器的名称是platform，应用类加载器的名称是app。**类加载器的名称在调试与类加载器相关的问题时会非常有用。**
4. 启动类加载器现在是在**jvm内部和java类库共同协作实现的类加载器**(以前是C++实现)，但为了与之前代码兼容，在获取启动类加载器的场景中仍然会返回null， 而不会得到BootClassLoader实例。
5. 类加载的委派关系也发生了变动。
   - 当平台及应用程序类加载器收到类加载请求，在委派给父加载器加载前，要先判断该类是否能够归属到某一个 系统模块中，如果可以找到这样的归属关系，就要优先委派给负责那个模块的加载器完成加载。
   - 如下图所示：

![image-20230110160224737](/../JVM.assets/image-20230110160224737.png)

**java模块化系统明确规定了三个类加载器负责各自加载的模块**

# 运行时数据区

**一个java程序对应一个进程**

**一个进程对应一个jvm实例**

**一个 jvm实例中只有一个运行时数据区**

**一个运行时数据区只有一个方法区和堆**

**一个进程中的多个线程需要共享同一个方法区，堆空间**



**每一个线程在创建时都会创建一个虚拟机栈**

**每一个线程拥有独立的一套程序计数器，本地方法栈，虚拟机栈**

**每一个线程内部保存的一个个的栈帧对应着一次次的Java方法调用**

## 概述

![image-20230112210823495](/../JVM.assets/image-20230112210823495.png)

内存是非常重要的系统资源，是硬盘和CPU的中间仓库及桥梁，承载着操作系统和应用程序的实时运行。JVM内存布局规定了java在运行过程中内存申请、分配、管理的策略，保证了JVM的高效稳定运行。**不同的JVM对于内存的划分方式和管理机制存在着部分差异**。结合JVM虚拟机规范，来探讨一下经典的JVM内存分布。

![image-20230112210849718](/../JVM.assets/image-20230112210849718.png)

Java虚拟机定义了若干种程序运行期间会使用到的运行时数据区，其中有一些会随着虛拟机启动而创建，随着虚拟机退出而销毁。另外一些则是与线程一一对应的，这些与线程对应的数据区域会随着线程开始和结束而创建和销毀。

灰色的为单独线程私有的，红色的为多个线程共享的。即：

- 每个线程：单独包括程序计数器、栈、本地栈。

- 线程间共享：堆、堆外内存（永久代或元空间、代码缓存）

垃圾回收集中在堆区（95%）和方法区（5%）

每个JVM只有一个Runtime实例。即为运行时环境，相当于内存结构的中间的那个框框：运行时环境。

## 线程

- 线程是一个程序里的运行单元。JVM允许一个应用有多个线程并行的执行。

- 在Hotspot JVM里， 每个线程都与操作系统的本地线程直接映射。
  - 当一个Java线程准备好执行以后，此时一个操作系统的本地线程也同时创建。Java线程执行终止后，本地线程也会回收。

- 操作系统负责所有线程的安排调度到任何一个可用的CPU上。一旦本地线程初始化成功，它就会调用Java线程中的run()方法。

### 系统线程

- 主要的后台系统线程在Hotspot JVM里 主要是以下几个:
  - **虚拟机线程**：这种线程的操作是需要JVM达到安全点才会出现。这些操作必须在不同的线程中发生的原因是他们都需要JVM达到安全点，这样堆才不会变化。这种线程的执行类型包括"stop-the-world"的垃圾收集，线程栈收集，线程挂起以及偏向锁撤销。
  - **周期任务线程**：这种线程是时间周期事件的体现(比如中断)，他们一般用于周期性操作的调度执行。
  - **GC线程**：这种线程对在JVM里不同种类的垃圾收集行为提供了支持。
  - **编译线程**：这种线程在运行时会将字节码编译成到本地代码。
  - **信号调度线程**：这种线程接收信号并发送给JVM， 在它内部通过调用适当的方法进行处理。

## PC寄存器

![image-20230112211904512](/../JVM.assets/image-20230112211904512.png)

### 概述

![image-20230112211921054](/../JVM.assets/image-20230112211921054.png)

JVM中的程序计数寄存器(Program Counter Register) 中，Register 的命名源于CPU的寄存器，寄存器存储指令相关的现场信息。CPU只有把数据装载到寄存器才能够运行。

这里，并非是广义上所指的物理寄存器，或许将其翻译为PC计数器(或指令计数器)会更加贴切(也称为程序钩子)，并且也不容易引起一些不必要的误会。**JVM中的PC寄存器是对物理PC寄存器的一种抽象模拟。**

#### 作用

PC寄存器用来存储指向下一条指令的地址，也就是即将要执行的指令代码。由执行引擎读取下一条指令。

![image-20230112212142575](/../JVM.assets/image-20230112212142575.png)

- 它是一块很小的内存空间，几乎可以忽略不记。也是运行速度最快的存储区域。

- 在JVM规范中，每个线程都有它自己的程序计数器，是线程私有的，生命周期与线程的生命周期保持一致。

- 任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法。程序计数器会存储当前线程正在执行的Java方法的JVM指令地址；或者，如果是在执行native方法，则是未指定值(undefned)。

- 它是程序控制流的指示器，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

- 字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。

- 它是唯一一个在Java虚拟机规范中没有规定任何OutOtMemoryError情况的区域。

#### 实例

```java
public class PCRegisterTest {

    public static void main(String[] args) {
        int i = 10;
        int j = 20;
        int k = i + j;

        String s = "abc";
        System.out.println(i);
        System.out.println(k);

    }
}
```

![image-20230112212239300](/../JVM.assets/image-20230112212239300.png)

### 面试题

#### 使用PC寄存器存储字节码指令地址有什么用呢？

因为CPU需要不停的切换各个线程，这时候切换回来以后，就得知道接着从哪开始继续执行

#### 为什么使用PC寄存器记录当前线程的执行地址呢？

JVM的字节码解释器就需要通过改变PC寄存器的值来明确下一条应该执行什么样的字节码指令。

#### PC寄存器为什么会被设定为线程私有？

我们都知道所谓的多线程在一个特定的时间段内只会执行其中某一个线程的方法，CPU会不停地做任务切换，这样必然导致经常中断或恢复，如何保证分毫无差呢？**为了能够准确地记录各个线程正在执行的当前字节码指令地址，最好的办法自然是为每一个线程都分配一个PC寄存器**，这样一来各个线程之间便可以进行独立计算，从而不会出现相互干扰的情况。.

由于**CPU时间片**轮限制，众多线程在并发执行过程中，任何一个确定的时刻，一个处理器或者多核处理器中的一个内核，只会执行某个线程中的一条指令。

这样必然导致经常中断或恢复，如何保证分毫无差呢？每个线程在创建后，都会产生自己的程序计数器和栈帧，程序计数器在各个线程之间互不影响。

- 时间片
  - CPU时间片即CPU分配给各个程序的时间，每个线程被分配一个时间段，称作它的时间片。
  - 在宏观上：我们可以同时打开多个应用程序，每个程序并行不悖，同时运行。
  - 但在微观上：由于只有一个CPU，一次只能处理程序要求的一部分，如何处理公平，一种方法就是引入时间片，每个程序轮流执行。

## 虚拟机栈

![image-20230112212653902](/../JVM.assets/image-20230112212653902.png)

### 概述

#### 背景

由于跨平台性的设计，Java的指令都是根据栈来设计的。不同平台CPU架构不同，所以不能设计为基于寄存器的。

**优点是跨平台，指令集小，编译器容易实现，缺点是性能下降，实现同样的功能需要更多的指令。**

#### 栈与堆的关系

栈是运行时的单位，堆是存储的单位，PC寄存器是步骤

![image-20230112212912618](/../JVM.assets/image-20230112212912618.png)

#### 基本内容

- Java虚拟机栈是什么?
  - Java虚拟机栈(/../Java Virtual Machine Stack) ，早期也叫Java栈。每个线程在创建时都会创建一个虚拟机栈，其内部保存一个个的栈帧(Stack Frame) ，对应着一次次的Java方法调用。
  - 是线程私有的

- 生命周期
  - 生命周期和线程一致。

- 作用
  - 主管Java程序的运行，它保存方法的局部变量、部分结果，并参与方法的调用和返回。

- 特点
  - 栈是一种快速有效的分配存储方式，访问速度仅次于程序计数器
  - JVM直接对Java栈的操作只有两个:
    - 每个方法执行，伴随着进栈(入栈、压栈)
    - 执行结束后的出栈工作
  - 对于栈来说不存在垃圾回收问题
    - GC
    - OOM

#### 相关异常

- Java虚拟机规范允许**Java栈的大小是动态的或者是固定不变的**
  - 如果采用固定大小的Java虚拟机栈，那每一个线程的Java虛拟机栈容量可以在线程创建的时候独立选定。如果**线程请求分配的栈容量超过Java虚拟机栈允许的最大容量**，Java虚拟机将会抛出一个**StackOverflowError**异常。
  - 如果Java虚拟机栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者**在创建新的线程时没有足够的内存去创建对应的虚拟机栈**，那Java虚拟机将会抛出一个**OutOfMemoryError**异常。

#### 设置栈内存的大小

我们可以使用参数-Xss选项来设置线程的最大栈空间，栈的大小直接决定了函数调用的最大可达深度。

![image-20230112213656087](/../JVM.assets/image-20230112213656087.png)

### 栈的存储单位

- 每个线程都有自己的栈，栈中的数据都是以**栈帧(Stack Frame)为基本单位**存储的。

- 在这个线程上正在执行的每个方法都各自对应一个栈帧(Stack Frame)。

- 栈帧是一个内存区块，是一个数据集，维系着方法执行过程中的各种数据信息。

#### 运行原理

- JVM直接对Java栈的操作只有两个，就是对栈帧的压栈和出栈，遵循“先进
  后出”/“后进先出”原则。
- 在一条活动线程中，一个时间点上，只会有一个活动的栈帧。即只有当前正在执行的方法的栈帧(栈项栈帧)是有效的，这个栈帧被称为**当前栈帧，(Current Frame)** ，与当前栈帧相对应的方法就是**当前方法(Current Method)**，定义这个方法的类就是**当前类(Current Class)**。
- 执行引擎运行的所有字节码指令只针对当前栈帧进行操作。
- 如果在该方法中调用了其他方法，对应的新的栈帧会被创建出来，放在栈的顶端，成为新的当前帧。
- 不同线程中所包含的栈帧是不允许存在相互引用的，即不可能在一个栈帧之中引用另外一个线程的栈帧。(不同的线程的栈帧在不同的栈里，栈是私有的)
- 如果当前方法调用了其他方法，方法返回之际，当前栈帧会传回此方法的执行结果
- 给前一个栈帧，接着，虚拟机会丢弃当前栈帧，使得前一个栈帧重新成为当前栈帧。
- Java方法有两种返回函数的方式，**一种是正常的函数返回，使用return指令；另一种是抛出异常。不管使用哪种方法，都会导致栈帧被弹出 **

```java
public class StackFrameTest {
    public static void main(String[] args) {
        try {
            StackFrameTest test = new StackFrameTest();
            test.method1();

        } catch (Exception e) {
            e.printStackTrace();
        }

        System.out.println("main()正常结束");

    }

    public void method1(){
        System.out.println("method1()开始执行...");
        method2();
        System.out.println("method1()执行结束...");
}

    public int method2() {
        System.out.println("method2()开始执行...");
        int i = 10;
        int m = (int) method3();
        System.out.println("method2()即将结束...");
        return i + m;
    }

    public double method3() {
        System.out.println("method3()开始执行...");
        double j = 20.0;
        System.out.println("method3()即将结束...");
        return j;
    }

}
```

![image-20230112220510016](/../JVM.assets/image-20230112220510016.png)

#### 栈帧的内部结构

每个栈帧中存储着:

- **局部变量表(Local variables)**
- **操作数栈(operand stack) ( 或表达式栈)**
- 动态链接(Dynamic Linking) ( 或指向运行时常量池的方法引用)
- 方法返回地址(Return Address) (或方法正常退出或者异常退出的定义)
- 一些附加信息

![image-20230112220526819](/../JVM.assets/image-20230112220526819.png)

### 局部变量表

#### 概念

- 局部变量表也被称之为局部变量数组或本地变量表
- **定义为一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量**，这些数据类型包括各类基本数据类型、对象引用(reference) ，以及returnAddress类型。
- 由于局部变量表是建立在线程的栈上，是线程的私有数据，因此**不存在数据安全问题**
- **局部变量表所需的容量大小是在编译期确定下来的**，并保存在方法的Code属性的maximum local variables数据项中。在方法运行期间是不会改变局部变量表的大小的。
- **方法嵌套调用的次数由栈的大小决定。**一般来说，**栈越大，方法嵌套调用次数越多**。对一个函数而言，它的参数和局部变量越多，使得局部变量表膨胀,它的栈帧就越大，以满足方法调用所需传递的信息增大的需求。进而函数调用就会占用更多的栈空间，导致其嵌套调用次数就会减少。
- **局部变量表中的变量只在当前方法调用中有效。**在方法执行时，虚拟机通过使用局部变量表完成参数值到参数变量列表的传递过程。**当方法调用结束后，随着方法栈帧的销毁，局部变量表也会随之销毁。**

#### Solt

- 参数值的存放总是在局部变量数组的index0开始，到数组长度-1的索引结束。
- 局部变量表，**最基本的存储单元是Slot (变量槽)**
- 局部变量表中存放编译期可知的各种基本数据类型(8种)，引用类型(reference)，returnAddress类型的变量。
- 在局部变量表里，**32位以内的类型只占用一个slot (包括returnAddress类型)，64位的类型(long和double)占用两个slot。**
  - byte、short、char在存储前被转换为int，boolean 也被转换为int，0表示false ，非0表示true。
  - long和double则占据两个Slot。
- JVM会为局部变量表中的每一个slot都分配一个访问索引，通过这个索引即可成功访问到局部变量表中指定的局部变量值
- 当一个实例方法被调用的时候，它的方法参数和方法体内部定义的局部变量将**会按照顺序被复制**到局部变量表中的每一个slot上
- **如果需要访问局部变量表中一个64bit的局部变量值时，只需要使用起始索引（前一个索引）即可。**(比如：访问long或double类型变量)
- <font color='red'>如果当前帧是由构造方法或者实例方法创建的，那么**该对象引用this将会存放在index为0的slot处**，其余的参数按照参数表顺序继续排列。</font>

![image-20230112220642852](/../JVM.assets/image-20230112220642852.png)

#### 实例

```java
public void test1() {
    Date date = new Date();
    String name1 = "atguigu.com";
    this.test2(date, name1);
    System.out.println(date + name1);
}
```

![image-20230112223548023](/../JVM.assets/image-20230112223548023.png)

```java
public String test2(Date dateP, String name2) {
    dateP = null;
    name2 = "songhongkang";
    double weight = 130.5D;
    char gender = true;
    return dateP + name2;
}
```

![image-20230112223558600](/../JVM.assets/image-20230112223558600.png)

```java
public void test3() {
    ++this.count;
}
```

![image-20230112223608148](/../JVM.assets/image-20230112223608148.png)

```java
public void test4() {
    int a = 0;
    int b = false;
    int c = a + 1;
    c = a + 1;
}
```

![image-20230112223621355](/../JVM.assets/image-20230112223621355.png)

```java
public void test5Temp() {
}
```

![image-20230112223631545](/../JVM.assets/image-20230112223631545.png)

#### 静态变量和局部变量的对比

- 参数表分配完毕之后，再根据方法体内定义的变量的顺序和作用域分配。

- 我们知道类变量表有两次初始化的机会，第一次是在“准备阶段”，执行系统初始化，对类变量设置零值，另一次则是在“初始化”阶段，赋予程序员在代码中定义的初始值。

- 和类变量初始化不同的是，局部变量表不存在系统初始化的过程，这意味着一旦定义了局部变量则必须人为的初始化，否则无法使用。

```java
public void test() {
int i;
System. out. print1n(i);
}
```

- 这样的代码是错误的，没有赋值不能够使用。
- 在栈帧中，**与性能调优关系最为密切的部分就是前面提到的局部变量表**。在方法执行时，虚拟机使用局部变量表完成方法的传递。
- **局部变量表中的变量也是重要的垃圾回收根节点，只要被局部变量表中直接或间接引用的对象都不会被回收。**

#### 变量的分类

- 按照数据类型分：
  - ① 基本数据类型  
  - ② 引用数据类型

- 按照在类中声明的位置分：
  - ① 成员变量：在使用前，都经历过默认初始化赋值
    - 类变量： linking的prepare阶段：给类变量默认赋值  ---> initial阶段：给类变量显式赋值即静态代码块赋值
    - 实例变量：随着对象的创建，会在堆空间中分配实例变量空间，并进行默认赋值
  - ② 局部变量：在使用前，必须要进行显式赋值的！否则，编译不通过

### 操作数栈

栈：可以使用数组或链表来实现

- 每一个独立的栈帧中除了包含局部变量表以外，还包含一个**后进先出**(Last-In-First-Out)的操作数栈，也可以称之为**表达式栈**(Expression Stack)

- **操作数栈，在方法执行过程中，根据字节码指令，往栈中写入数据或提取数据，即入栈(push) /出栈(pop)。**
  - 某些字节码指令将值压入操作数栈，其佘的字节码指令将操作数取出栈。使用它们后再把结果压入栈。
  - 比如：执行复制、交换、求和等操作
  - ![image-20230112225419198](/../JVM.assets/image-20230112225419198.png)



- 操作数栈，**主要用于保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间。**
- 操作数栈就是JVM执行引擎的一个工作区，当一个方法刚开始执行的时候，一个新的栈帧也会随之被创建出来，**这个方法的操作数栈是空的。**
- 每一个操作数栈都会拥有一个明确的栈深度用于存储数值，其所需的最大深度在编译期就定义好了，保存在方法的Code属性中，为max_ stack的值。
- 栈中的任何一个元素都是可以任意的Java数据类型。
  - 32bit的类型占用一个栈单位深度
  - 64bit的类型占用两个栈单位深度
- 操作数栈**并非采用访问索引的方式来进行数据访问的**，而是只能通过标准的入栈(push)和出栈(pop) 操作来完成一次数据访问。

- **如果被调用的方法带有返回值的话，其返回值将会被压入当前栈帧的操作数栈中**，并更新PC寄存器中下一条需要执行的字节码指令。
- 操作数栈中元素的数据类型必须与字节码指令的序列严格匹配，这由编译器在编译器期间进行验证，同时在类加载过程中的类检验阶段的数据流分析阶段要再次验证。
- 另外，我们说Java虚拟机的**解释引擎是基于栈的执行引擎**，其中的栈指的就是操作数栈。

### 代码追踪

```java
public void testAddOperation() {
    byte i = 15;
    int j = 8;
    int var10000 = i + j;
}
```

![image-20230112232609529](/../JVM.assets/image-20230112232609529.png)

![image-20230112232735251](/../JVM.assets/image-20230112232735251.png)

### 栈顶缓存技术(了解)

前面提过，基于栈式架构的虚拟机所使用的零地址指令更加紧凑，但**完成一项操作的时候必然需要使用更多的入栈和出栈指令**，这同时也就**意味着将需要更多的指令分派( instruction dispatch)次数和内存读/写次数**。

由于操作数是存储在内存中的，因此频繁地执行内存读/写操作必然会影响执行速度。为了解决这个问题，HotSpot JVM的设计者们提出了栈顶缓存(ToS，Top-of-Stack Cashing)技术，<font color='red'>**将栈项元素全部缓存在物理CPU的寄存器中，以此降低对内存的读/写次数，提升执行引擎的执行效率。**</font>

### 动态链接（或指向运行时常量池的方法引用）

每一个栈帧内部都包含一个指向运行时常量池中**该栈帧所属方法的引用**。包含这个引用的目的就是为了支持当前方法的代码能够实现**动态链接(Dynamic Linking)** 。比如: invokedynamic指令

在Java源文件被编译到字节码文件中时，所有的变量和方法引用都作为符号引用(Symbolic Reference) 保存在class文件的常量池里。比如：描述一个方法调用了另外的其他方法时，就是通过常量池中指向方法的符号引用来表示的，那么**动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用。**

![image-20230113000258333](/../JVM.assets/image-20230113000258333.png)

实例：

![image-20230113000308404](/../JVM.assets/image-20230113000308404.png)

![image-20230113000321576](/../JVM.assets/image-20230113000321576.png)

每一个引用都有相对应的操作

![image-20230113000332200](/../JVM.assets/image-20230113000332200.png)

#3：符号引用

#38：直接引用

### 方法的调用

#### 绑定机制

##### 静态链接&动态链接

在JVM中，将符号引用转换为调用方法的直接引用与方法的绑定机制相关。

- 静态链接：
  - 当一个字节码文件被装载进JVM内部时，如果被调用的**目标方法在编译期可知，**且运行期保持不变时。这种情况下将调用方法的符号引用转换为直接引用的过程称之为静态链接。

- 动态链接：
  - 如果**被调用的方法在编译期无法被确定下来**，也就是说，只能够**在程序运行期将调用方法的符号引用转换为直接引用**，由于这种引用转换过程具备动态性，因此也就被称之为动态链接。

##### 早期绑定&晚期绑定

对应的方法的绑定机制为：早期绑定(Early Binding) 和晚期绑定(Late Binding) 。**绑定是一个字段、方法或者类在符号引用被替换为直接引用的过程，这仅仅发生一次。**

- 早期绑定：
  - 早期绑定就是指被调用的**目标方法如果在编译期可知，且运行期保持不变时，**即可将这个方法与所属的类型进行绑定，这样一来，由于明确了被调用的目标方法究竟是哪一个，因此也就可以使用静态链接的方式将符号引用转换为直接引用。

- 晚期绑定：
  - 如果**被调用的方法在编译期无法被确定下来，只能够在程序运行期根据实际的类型绑定相关的方法**，这种绑定方式也就被称之为晚期绑定。

##### 实例：

```java
class Animal{

    public void eat(){
        System.out.println("动物进食");
    }
}
interface Huntable{
    void hunt();
}
class Dog extends Animal implements Huntable{
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }

    @Override
    public void hunt() {
        System.out.println("捕食耗子，多管闲事");
    }
}

class Cat extends Animal implements Huntable{

    public Cat(){
        super();//表现为：早期绑定
    }

    public Cat(String name){
        this();//表现为：早期绑定
    }

    @Override
    public void eat() {
        super.eat();//表现为：早期绑定
        System.out.println("猫吃鱼");
    }

    @Override
    public void hunt() {
        System.out.println("捕食耗子，天经地义");
    }
}
public class AnimalTest {
    public void showAnimal(Animal animal){
        animal.eat();//表现为：晚期绑定
    }
    public void showHunt(Huntable h){
        h.hunt();//表现为：晚期绑定
    }
}
```

晚期绑定：

![image-20230113002656009](/../JVM.assets/image-20230113002656009.png)

![image-20230113002702799](/../JVM.assets/image-20230113002702799.png)

早期绑定：

![image-20230113002713039](/../JVM.assets/image-20230113002713039.png)

![image-20230113002721374](/../JVM.assets/image-20230113002721374.png)

##### 总结

- 随着高级语言的横空出世，类似于Java一样的基于面向对象的编程语言如今越来越多，尽管这类编程语言在语法风格上存在一定的差别，但是它们彼此之间始终保持着一个共性，那就是都支持封装、继承和多态等面向对象特性，既然**这一类的编程语言具备多态特性，那么自然也就具备早期绑定和晚期绑定两种绑定方式。**

- Java中任何一个普通的方法其实都具备虛函数的特征，它们相当于C++语言的虚函数(C++中则需要使用关键字virtual来显式定义)。如果在Java程序中不希望某个方法拥有虛函数的特征时，则可以使用关键字final来标记这个方法。

#### 虚方法和非虚方法

##### 概括

- 非虚方法:
  - 如果方法在编译期就确定了具体的调用版本，这个版本在运行时是不可变的。这样的方法称为非虚方法。
  - 静态方法、私有方法、final方法、实例构造器、父类方法都是非虚方法。
  - 其他方法称为虛方法。

虚拟机中提供了以下几条方法调用指令：

- 普通调用指令：

1. **invokestatic： 调用静态方法，解析阶段确定唯一方法版本**
2. **invokespecial：调用<init>方法、私有及父类方法，解析阶段确定唯一方法版本**
3. invokevirtual：调用所有虛方法
4. invokeinterface：调用接口方法

- 动态调用指令：

1. invokedynamic：动态解析出需要调用的方法，然后执行

前四条指令固化在虚拟机内部，方法的调用执行不可人为干预，而invokedynamic指令则支持由用户确定方法版本。其中**invokestatic指令和invokespecial指令调用的方法称为非虚方法，其余的(final修饰的除外)称为虚方法。**

##### 实例

```java
class Father {
    public Father() {
        System.out.println("father的构造器");
    }

    public static void showStatic(String str) {
        System.out.println("father " + str);
    }

    public final void showFinal() {
        System.out.println("father show final");
    }

    public void showCommon() {
        System.out.println("father 普通方法");
    }
}

public class Son extends Father {
    public Son() {
        //invokespecial
        super();
    }
    public Son(int age) {
        //invokespecial
        this();
    }
    //不是重写的父类的静态方法，因为静态方法不能被重写！
    public static void showStatic(String str) {
        System.out.println("son " + str);
    }
    private void showPrivate(String str) {
        System.out.println("son private" + str);
    }

    public void show() {
        //invokestatic
        showStatic("atguigu.com");
        
        //invokestatic
        super.showStatic("good!");
        
        //invokespecial
        showPrivate("hello!");
        
        //invokespecial
        super.showCommon();

        //invokevirtual
        showFinal();//因为此方法声明有final，不能被子类重写，所以也认为此方法是非虚方法。
        //虚方法如下：
        //invokevirtual
        showCommon();
        info();

        MethodInterface in = null;
        //invokeinterface
        in.methodA();
    }

    public void info(){

    }

    public void display(Father f){
        f.showCommon();
    }

    public static void main(String[] args) {
        Son so = new Son();
        so.show();
    }
}

interface MethodInterface{
    void methodA();
}
```

![image-20230113003200604](/../JVM.assets/image-20230113003200604.png)

#### invokedynamic指令

##### 概述

JVM字节码指令集一直比较稳定，一直到Java7中才增加了一个invokedynamic指令，这是**Java为了实现「动态类型语言」支持而做的一种改进**。

但是在Java7中并没有提供直接生成invokedynamic指令的方法，需要借助ASM这种底层字节码工具来产生invokedynamic指令。**直到Java8的Lambda表达式的出现，invokedynamic指令的生成，在Java中才有了直接的生成方式。**

Java7中增加的动态语言类型支持的本质是对Java虚拟机规范的修改，而不是对Java语言规则的修改，这一块相对来讲比较复杂，增加了虚拟机中的方法调用，最直接的受益者就是运行在Java平台的动态语言的编译器。

##### 动态语言&静态语言

**动态类型语言和静态类型语言两者的区别就在于对类型的检查是在编译期还是在运行期**，满足前者就是静态类型语言，反之是动态类型语言。

说的再直白一点就是，静态类型语言是判断变量自身的类型信息；动态类型语言是判断变量值的类型信息，变量没有类型信息，变量值才有类型信息，这是动态语言的一个重要特征。

##### 实例

```java
@FunctionalInterface
interface Func {
    public boolean func(String str);
}

public class Lambda {
    public void lambda(Func func) {
        return;
    }

    public static void main(String[] args) {
        Lambda lambda = new Lambda();

        Func func = s -> {
            return true;
        };

        lambda.lambda(func);

        lambda.lambda(s -> {
            return true;
        });
    }
}
```

![image-20230113003316936](/../JVM.assets/image-20230113003316936.png)

#### 方法重写

Java语言中方法重写的本质:

1. 找到操作数栈项的第一个元素所执行的对象的实际类型，记作C。
2. 如果在类型C中找到与常量中的描述符合简单名称都相符的方法，则进行访问权限校验，如果通过则返回这个方法的直接引用，查找过程结束；如果不通过，则返回java.lang. IllegalAccessError异常。
3. 否则，按照继承关系从下往上依次对C的各个父类进行第2步的搜索和验证过程。
4. 如果始终没有找到合适的方法，则抛出java.lang.AbstractMethodError异常。

IllegalAccessError介绍：

程序试图访问或修改一个属性或调用一个方法，这个属性或方法，你没有权限访问。一般的，这个会引起编译器异常。这个错误如果发生在运行时，就说明一个类发生了不兼容的改变。

##### 虚方法表

- 在面向对象的编程中，会很频繁的使用到动态分派，如果在每次动态分派的过程中都要重新在类的方法元数据中搜索合适的目标的话就可能影响到执行效率。因此，**为了提高性能**，JVM采用在类的方法区建立一个**虚方法表(virtual method table) (非虚方法不会出现在表中)来实现。使用索引表来代替查找。**

- 每个类中都有一个虚方法表，表中存放着各个方法的实际入口。

- 那么虚方法表什么时候被创建?
  - 虚方法表会在类加载的链接阶段被创建并开始初始化，类的变量初始值准备完成之后，JVM会把该类的方法表也初始化完毕。

重写方法的虚方法表指向重写的类，没有重写的方法指向父类

![image-20230113003723300](/../JVM.assets/image-20230113003723300.png)

##### 实例

```java
interface Friendly {
    void sayHello();
    void sayGoodbye();
}
class Dog {
    public void sayHello() {
    }
    public String toString() {
        return "Dog";
    }
}
class Cat implements Friendly {
    public void eat() {
    }
    public void sayHello() {
    }
    public void sayGoodbye() {
    }
    protected void finalize() {
    }
    public String toString(){
        return "Cat";
    }
}

class CockerSpaniel extends Dog implements Friendly {
    public void sayHello() {
        super.sayHello();
    }
    public void sayGoodbye() {
    }
}

public class VirtualMethodTable {
}
```

Dog虚方法表：

![image-20230113003754061](/../JVM.assets/image-20230113003754061.png)

CockerSpaniel虚方法表：

![image-20230113003804834](/../JVM.assets/image-20230113003804834.png)

Cat虚方法表：

![image-20230113003850107](/../JVM.assets/image-20230113003850107.png)

### 方法返回地址

- **<font color='red'>存放调用该方法的pc寄存器的值。</font>**

- 一个方法的结束，有两种方式:
  - 正常执行完成
  - 出现未处理的异常，非正常退出

- 无论通过哪种方式退出，在方法退出后都返回到该方法被调用的位置。方法正常退出时，**调用者的pc计数器的值作为返回地址，即调用该方法的指令的下一条指令的地址。**而通过异常退出的，返回地址是要通过异常表来确定，栈帧中一般不会保存这部分信息。

#### 退出方法的方式

当一个方法开始执行后，只有两种方式可以退出这个方法：

1、执行引擎遇到任意一个方法返回的字节码指令(return)，会有返回值传递给上层的方法调用者，简称正常完成出口;

- 一个方法在正常调用完成之后究竟需要使用哪-一个返回指令还需要根据方法返回值的实际数据类型而定。
- 在字节码指令中，返回指令包含ireturn (当返回值是boolean、 byte、 char、short和int类型时使用)、lreturn、 freturn、 dreturn以及areturn，另外还有一个return指令供声明为void的方法、实例初始化方法、类和接0的初始化方法使用。

2、在方法执行的过程中遇到了异常(Exception) ，并且这个异常没有在.方法内进行处理，也就是只要在本方法的异常表中没有搜索到匹配的异常处理器，就会导致方法退出。简称**异常完成出口。**

方法执行过程中抛出异常时的异常处理，存储在一个异常处理表，方便在发生异常的时候找到处理异常的代码。

![image-20230113003915396](/../JVM.assets/image-20230113003915396.png)

#### 实例

```java
public class ReturnAddressTest {
    public boolean methodBoolean() {
        return false;
    }

    public byte methodByte() {
        return 0;
    }

    public short methodShort() {
        return 0;
    }

    public char methodChar() {
        return 'a';
    }

    public int methodInt() {
        return 0;
    }

    public long methodLong() {
        return 0L;
    }

    public float methodFloat() {
        return 0.0f;
    }

    public double methodDouble() {
        return 0.0;
    }

    public String methodString() {
        return null;
    }

    public Date methodDate() {
        return null;
    }

    public void methodVoid() {

    }

    static {
        int i = 10;
    }
    
    public void method2() {

        methodVoid();

        try {
            method1();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void method1() throws IOException {
        FileReader fis = new FileReader("atguigu.txt");
        char[] cBuffer = new char[1024];
        int len;
        while ((len = fis.read(cBuffer)) != -1) {
            String str = new String(cBuffer, 0, len);
            System.out.println(str);
        }
        fis.close();
    }

}
```

method2中将异常解决

![image-20230113003953996](/../JVM.assets/image-20230113003953996.png)

method1中将异常上抛

![image-20230113004004058](/../JVM.assets/image-20230113004004058.png)

本质上，方法的退出就是当前栈帧出栈的过程。此时，需要恢复上层方法的局部变量表、操作数栈、将返回值压入调用者栈帧的操作数栈、设置PC寄存器值等，让调用者方法继续执行下去。

**正常完成出口和异常完成出口的区别在于：通过异常完成出口退出的不会给他的上层调用者产生任何的返回值。**

### 附加信息

栈帧中还允许携带与Java虛拟机实现相关的一些附加信息。例如，对程序调试提供支持的信息。

### 面试题

- 举例栈溢出的情况?（StackOverflowError）
  - 调用方法，栈帧把栈充满后了，就会造成栈溢出，典型案例：无限递归
  - 通过-Xss设置栈的大小；OOM

- 调整栈大小，就能保证不出现溢出吗?
  - 不能

- 分配的栈内存越大越好吗?
  - 不是，大小越适宜越好
  - 会挤占其他内存结构的空间

- 垃圾回收是否会涉及到虛拟机栈?
  - 不会，垃圾回收不在虚拟机栈内进行，GC主要在堆内存中进行

- 方法中定义的局部变量是否线程安全?
  - 具体问题具体分析
    - 如果只有一个线程才可以操作此数据，则必是线程安全的。
    - 如果有多个线程操作此数据，则此数据是共享数据。如果不考虑同步机制的话，会存在线程安全问题。

```java
public class StringBuilderTest {

    int num = 10;

    //s1的声明方式是线程安全的
    public static void method1(){
        //StringBuilder:线程不安全
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        /...
    }
    
    //sBuilder的操作过程：是线程不安全的
    public static void method2(StringBuilder sBuilder){
        sBuilder.append("a");
        sBuilder.append("b");
        /...
    }
    
    //s1的操作：是线程不安全的
    public static StringBuilder method3(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1;
    }
    
    //s1的操作：是线程安全的
    public static String method4(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1.toString();
    }

    public static void main(String[] args) {
        StringBuilder s = new StringBuilder();

        new Thread(() -> {
            s.append("a");
            s.append("b");
        }).start();

        method2(s);
    }

}
```

## 本地方法接口

![image-20230113004142452](/../JVM.assets/image-20230113004142452.png)

### 概述

简单地讲，一个Native Method就是一个Java调用非Java代码的接口。一个Native Method是这样一个Java方法: 该方法的实现由非Java语言实现，比如C。这个特征并非Java所特有，很多其它的编程语言都有这一机制，比如在C++中，你可以用extern "C" 告知C++编译器去调用一个C的函数。.

"A native method is a Java method whose implementation is provided by non-java code."

在定义一个native method时， 并不提供实现体(有些像定义一个Javainterface)，因为其实现体是由非java语言在外面实现的。

本地接口的作用是融合不同的编程语言为Java所用，它的初衷是融合C/C++程序。

#### 实例

```java
public class IHaveNatives {
	public native void methodNative1 ( int X ) ;
	public native static long methodNative2 () ;
	private native synchronized float methodNative3( Object o )
	native void methodNative4 ( int[] ary ) throws Exception ;
}
```

标识符native可以与所有其他的java标识符连用，但是abstract除外。

### 为什么要使用Native Method ?
Java使用起来非常方便，然而有些层次的任务用Java实现起来不容易，或者我们对程序的效率很在意时，问题就来了。

#### 与Java环境外交互:
**有时Java应用需要与Java外面的环境交互，这是本地方法存在的主要原因**。你可以想想Java需要与一些底层系统，如操作系统或某些硬件交换信息时的情况。本地方法正是这样一种交流机制：它为我们提供了一个非常简洁的接口，而且我们无需去了解Java应用之外的繁琐的细节。

#### 与操作系统交互

JVM支持着Java语言本身和运行时库，它是Java程序赖以生存的平台，它由一个解释器(解释字节码)和一些连接到本地代码的库组成。然而不管怎样，它毕竟不是一个完整的系统，它经常依赖于一些底层系统的支持。这些底层系统常常是强大的操作系统。**通过使用本地方法，我们得以用Java实现了jre的与底层系统的交互，甚至JVM的一些部分就是用C写的。**还有，如果我们要使用一些Java语言本身没有提供封装的操作系统的特性时，我们也需要使用本地方法。

#### Sun's Java

**Sun的解释器是用C实现的，这使得它能像一些普通的C一样与外部交互。**jre大部分是用Java实现的，它也通过一些本地方法与外界交互。例如：类java.lang. Thread的setPriority()方法是用Java实现的，但是它实现调用的是该类里的本地方法setPriority0()。这个本地方法是用C实现的，并被植入JVM内部，在windows 95的平台上，这个本地方法最终将调用win32 setPriority() API。 这是一个本地方法的具体实现由JVM直接提供，更多的情况是本地方法由外部的动态链接库(external dynamic link library) 提供，然后被JVM调用。

#### 现状

**目前该方法使用的越来越少了，除非是与硬件有关的应用，**比如通过Java程序驱动打印机或者Java系统管理生产设备，在企业级应用中已经比较少见。因为现在的异构领域间的通信很发达，比如可以使用Socket通信，也可以使用web service等等， 不多做介绍。

## 本地方法栈

![image-20230113004422333](/../JVM.assets/image-20230113004422333.png)

- **Java虚拟机栈用于管理Java方法的调用，而本地方法栈用于管理本地方法的调用。**

- 本地方法栈，也是线程私有的。

- 允许被实现成固定或者是可动态扩展的内存大小。(在内存溢出方面是相同的)
  - 如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Java虛拟机将会抛出一个StackoverflowError 异常。
  - 如果本地方法栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的本地方法栈，那么Java虚拟机将会抛出一一个OutofMemoryError异常。
- 本地方法是使用C语言实现的。
- 它的具体做法是Native Method stack中 登记native方法，在Execution Engine 执行时加载本地方法库。

![image-20230113004506230](/../JVM.assets/image-20230113004506230.png)

- **当某个线程调用一个本地方法时，它就进入了一个全新的并且不再受虚拟机限制的世界。它和虛拟机拥有同样的权限。**
  - 本地方法可以通过本地方法接口来**访问虚拟机内部的运行时数据区。**
  - 它甚至可以直接使用本地处理器中的寄存器
  - 直接从本地内存的堆中分配任意数量的内存。
- **并不是所有的JVM都支持本地方法。因为Java虚拟机规范并没有明确要求本地方法栈的使用语言、具体实现方式、数据结构等。如果JVM产品不打算支持native方法，也可以无需实现本地方法栈。**
- 在Hotspot JVM中， 直接将本地方法栈和虚拟机栈合二为一。

## 堆空间

![image-20230113004557390](/../JVM.assets/image-20230113004557390.png)

### 概述

- 一个JVM实例只存在一个堆内存，堆也是Java内存管理的核心区域。
- Java堆区在JVM启动的时候即被创建，其空间大小也就确定了。是JVM管理的最大一块内存空间。
  - 堆内存的大小是可以调节的。
- 《Java虚拟机规范》规定，堆可以处于**物理上不连续**的内存空间中，但在**逻辑上**它应该被视为**连续的**。
- **（逻辑连续实现起来简单，是一块连续的空间；存储也比较高效。物理内存和虚拟内存可以建立一个映射表，物理上不连续的表在虚拟内存的逻辑上可以视为连续）**
- 所有的线程共享Java堆，在这里还可以划分线程私有的缓冲区(ThreadLocal Allocation Buffer, TLAB) 。

### 堆的核心概述

![image-20230113135346408](/../JVM.assets/image-20230113135346408.png)

- 《Java虚拟机规范》中对Java堆的描述是：**所有的**对象实例以及数组都应当在运行时分配在堆上。(The heap is the run-time data area from which memory for all class instances and arrays is allocated )
  - 我要说的是：“几乎” 所有的对象实例都在这里分配内存。——从实际使用角度看的。

- **数组和对象可能永远不会存储在栈上**，因为栈帧中保存引用，这个引用指向对象或者数组在堆中的位置。

- **在方法结束后，堆中的对象不会马上被移除，仅仅在垃圾收集的时候才会被移除。**
  - 栈弹出后，推中的对象并不是立刻被移除，而是等到GC的时候，发现栈中没有指针指向该对象时，才被移除。（尽量减少GC）
  - new关键字代表带堆空间中创建对象
  - ![image-20230113135355590](/../JVM.assets/image-20230113135355590.png)

- 堆，是GC ( Garbage Collection,垃圾收集器)执行垃圾回收的重点区域。

#### 内存细分

现代垃圾回收器大部分都基于分代收集理论设计，堆空间细分为：

![image-20230113135408342](/../JVM.assets/image-20230113135408342.png)

约定：新生区<=>新生代<=>年轻代	养老区<=>老年区<=>老年代	永久区<=>永久代

JDK7和JDK8内存结构的变化之一就是元空间替换永久区，还有字符串常量池、静态的域……

### 设置堆内存大小与OOM

- Java堆区用于存储Java对象实例，那么堆的大小在JVM启动时就已经设定好了，大家可以通过选项”-Xmx"和”-Xms"来进行设置。
  - “-Xms"用于表示堆区的起始内存，等价于-XX：InitialHeapSize
  - “-Xmx"则用于表示堆区的最大内存，等价于-XX ：MaxHeapSize
-  **一旦堆区中的内存大小超过“-Xmx"所指定的最大内存时，将会抛出OutOfMemoryError异常。**
- 通常会将 -Xms 和 -Xmx 两个参数配置相同的值，其**目的是为了能够在java垃圾回收机制清理完堆区后不需要重新分隔计算堆区的大小，从而提高性能。**
- 默认情况下
  - 初始内存大小：物理电脑内存大小 / 64
  - 最大内存大小：物理电脑内存大小 / 4

#### 设置堆内存大小实例

1. 设置堆空间大小的参数
   - -Xms 用来设置堆空间（年轻代+老年代）的初始内存大小
     -  -X 	是jvm的运行参数
     -   ms  是memory start
   - -Xmx 用来设置堆空间（年轻代+老年代）的最大内存大小
2. 默认堆空间的大小
   - 初始内存大小：物理电脑内存大小 / 64（64分之1）
   - 最大内存大小：物理电脑内存大小 / 4（4分之1）
3. 手动设置：-Xms600m -Xmx600m
   - 开发中建议将初始堆内存和最大的堆内存设置成相同的值。
4. 查看设置的参数
   - 方式一： jps   /  jstat -gc 进程id
   - 方式二：-XX:+PrintGCDetails

```java
public class HeapSpaceInitial {
    public static void main(String[] args) {

        //Runtime指的是运行时数据区的实例
        //返回Java虚拟机中的堆内存总量
        long initialMemory = Runtime.getRuntime().totalMemory() / 1024 / 1024;
        //返回Java虚拟机试图使用的最大堆内存量
        long maxMemory = Runtime.getRuntime().maxMemory() / 1024 / 1024;

        System.out.println("-Xms : " + initialMemory + "M");
        System.out.println("-Xmx : " + maxMemory + "M");

//        System.out.println("系统内存大小为：" + initialMemory * 64.0 / 1024 + "G");
//        System.out.println("系统内存大小为：" + maxMemory * 4.0 / 1024 + "G");

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

运行结果（每个人可能不一样，根据自己电脑情况）：

![image-20230113135436103](/../JVM.assets/image-20230113135436103.png)

这是参数后的运行结果：

![image-20230113135445227](/../JVM.assets/image-20230113135445227.png)

![image-20230113135456799](/../JVM.assets/image-20230113135456799.png)

新生区分为伊甸园区、S0区、S1区，实际使用时，伊甸园区会和S0、S1中的其中一个使用，另一个用于复制，始终是空的所以总是差一点

#### OOM实例

```java
public class OOMTest {
    public static void main(String[] args) {
        ArrayList<Picture> list = new ArrayList<>();
        while(true){
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            list.add(new Picture(new Random().nextInt(1024 * 1024)));
        }
    }
}

class Picture{
    private byte[] pixels;

    public Picture(int length) {
        this.pixels = new byte[length];
    }
}
```

运行结果

![image-20230113135536744](/../JVM.assets/image-20230113135536744.png)

### 年轻代与老年代

- 存储在JVM中的Java对象可以被划分为两类:
  - 一类是生命周期较短的瞬时对象，这类对象的创建和消亡都非常迅速
  - 另外一类对象的生命周期却非常长，在某些极端的情况下还能够与JVM的生命周期保持一致。
- Java堆区进一步细分的话， 可以划分为年轻代(YoungGen)和老年代(OldGen)
- 其中年轻代又可以划分为Eden空间、Survivor0空间和Survivor1空间(有时也叫做from区、to区)。

![image-20230113135547353](/../JVM.assets/image-20230113135547353.png)

- 配置新生代与老年代在堆结构的占比。（不常用）
  - 默认-xx: NewRatio=2，表示新生代占1，老年代占2，新生代占整个堆的1/3
  - 可以修改-XX:NewRatio=4，表示新生代占1，老年代占4，新生代占整个堆的1/5

#### 实例

-Xms600m -Xmx600m

-XX:NewRatio ： 设置新生代与老年代的比例。默认值是2.

-XX:SurvivorRatio ：设置新生代中Eden区与Survivor区的比例。默认值是8

-XX:-UseAdaptiveSizePolicy ：关闭自适应的内存分配策略  （暂时用不到）

-Xmn:设置新生代的空间的大小。 （一般不设置）（优先级更高）

```java
public class EdenSurvivorTest {
    public static void main(String[] args) {
        System.out.println("我只是来打个酱油~");
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

运行结果

![image-20230113135821055](/../JVM.assets/image-20230113135821055.png)

- 在HotSpot中，Eden 空间和另外两个Survivor空间缺省所占的比例是8:1:1**（但是并不是，因为存在自适应机制，所以是6:1:1）**

![image-20230113135830613](/../JVM.assets/image-20230113135830613.png)

- 当然开发人员可以通过选项“-XX:SurvivorRatio”调整这个空间比例。比如-XX : SurvivorRatio=8
- **几乎所有**的Java对象都是在Eden区被new出来的。（放不下的就直接放老年代）
- 绝大部分的Java对象的销毁都在新生代进行了。
  - IBM公司的专门研究表明，新生代中80%的对象都是“朝生夕死”的。
- 可以使用选项"-Xmn"设置新生代最大内存大小
  - 这个参数一般使用默认值就可以了。

### 对象分配过程

为新对象分配内存是一件非常严谨和复杂的任务，JVM的设计者们不仅需要考虑内存如何分配、在哪里分配等问题，并且由于内存分配算法与内存回收算法密切相关，所以还需要考虑GC执行完内存回收后是否会在内存空间中产生内存碎片。
1. new的对象先放伊甸园区。此区有大小限制。
2. 当伊甸园的空间填满时，程序又需要创建对象，JVM的垃圾回收器将对伊甸园区进行垃圾回收(Minor GC)， 将伊甸园区中的不再被其他对象所引用的对象进行销毁。再加载新的对象放到伊甸园区
3. 然后将伊甸园中的剩余对象移动到幸存者0区。
4. 如果再次触发垃圾回收，此时上次幸存下来的放到幸存者0区的，如果没有回收，就会放到幸存者1区。
5. 如果再次经历垃圾回收，此时会重新放回幸存者0区，接着再去幸存者1区。
6. 啥时候能去养老区呢?可以设置次数。默认是15次。
- **可以设置参数: -XX: MaxTenuringThreshold=<N>进行设置。**

7. 在养老区，相对悠闲。当养老区内存不足时，再次触发GC: Major GC， 进行养老区的内存清理I
8. 若养老区执行了Major GC之 后发现依然无法进行对象的保存，就会产生00M异常

**java. lang . OutOfMemoryError: Java heap space**

#### 新生代对象分配与回收过程

![image-20230113140228742](/../JVM.assets/image-20230113140228742.png)

#### 总结

- 针对幸存者s0，s1区的总结:复制之后有交换，谁空谁是to。

- 关于垃圾回收：频繁在新生区收集，很少在养老区收集，几乎不在永久区/元空间收集。

#### 对象分配的特殊情况

![image-20230113140800643](/../JVM.assets/image-20230113140800643.png)

#### 实例

```java
public class HeapInstanceTest {

    byte[] buffer = new byte[new Random().nextInt(1024 * 200)];

    public static void main(String[] args) {
        ArrayList<HeapInstanceTest> list = new ArrayList<HeapInstanceTest>();
        while (true) {
            list.add(new HeapInstanceTest());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
}
```

![image-20230113140813302](/../JVM.assets/image-20230113140813302.png)

### Minor GC、Major GC、Full GC

#### 概括

JVM在进行GC时，并非每次都对上面三个内存（新生代、老年代；方法区）区域一起回收的，大部分时候回收的都是指新生代。

针对HotSpotVM的实现，它里面的GC按照回收区域又分为两大种类型:一种是部分收集(Partial GC)，一种是整堆收集(Full GC)

- 部分收集：不是完整收集整个Java堆的垃圾收集。其中又分为：
  - 新生代收集(Minor GC / Young GC) ：只是新生代的垃圾收集
  - 老年代收集(MajorGC/oldGC)：只是老年代的垃圾收集。
    - 目前，只有CMS GC会有单独收集老年代的行为。|
    - **注意，很多时候Major GC会和Full GC混淆使用，需要具体分辨是老年代回收还是整堆回收。**
  - 混合收集(Mixed GC)：收集整个新生代以及部分老年代的垃圾收集。
    - 目前，只有**G1 GC**会有这种行为

- 整堆收集(Full GC)：
  - 收集整个java堆和方法区的垃圾收集。

![image-20230113141513835](/../JVM.assets/image-20230113141513835.png)

- 年轻代GC(Minor GC)触发机制：
  - 当年轻代空间不足时，就会触发Minor GC，这里的年轻代满指的是Eden代满，Survivor满不会引发GC。(每次 Minor GC会清理年轻代的内存。)
  - 因为Java对象**大多都具备朝生夕灭**的特性，所以MinorGC非常频繁，一般回收速度也比较快。这一定义既清晰又易于理解。
  - Minor GC会引发STW， 暂停其它用户的线程，等垃圾回收结束，用线程才恢复运行。
- 老年代GC (Major GC/Full GC)触发机制:
  - 指发生在老年代的GC，对象从老年代消失时，我们说“Major GC”或“Full GC”发生了。
  - 出现了Major GC，经常会伴随至少一次的Minor GC (但非绝对的，在Parallel Scavenge收集器的收集策略里就有直接进行Major GC的策略选择过程)
    - 也就是在老年代空间不足时，会先尝试触发Minor GC。如果之后空间还不足，则触发Major GC
  - Major GC的速度一般会比Minor GC慢10倍以上，STW的时间更长。
  - 如果Major GC后，内存还不足，就报OOM了。
  
- Full GC触发机制:  (后面细讲)
  - 触发Full GC执行的情况有如下五种:
    1. 调用System. gc()时，系统建议执行Full GC，但是不必然执行
    2. 老年代空间不足
    3. 方法区空间不足
    4. 通过Minor GC后进入老年代的平均大小大于老年代的可用内存
    5. 由Eden区、survivor space0 (From Space) 区向survivor space1 (To Space)区复制时，对象大小大于To Space可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小

说明: **full gc是开发或调优中尽量要避免的。这样暂时时间会短一些。**

#### 实例

```java
public class GCTest {
    public static void main(String[] args) {
        int i = 0;
        try {
            List<String> list = new ArrayList<>();
            String a = "atguigu.com";
            while (true) {
                list.add(a);
                a = a + a;
                i++;
            }

        } catch (Throwable t) {
            t.printStackTrace();
            System.out.println("遍历次数为：" + i);
        }
    }
}
```

![image-20230113142057499](/../JVM.assets/image-20230113142057499.png)

运行结果

![image-20230113142108099](/../JVM.assets/image-20230113142108099.png)

### 堆空间分代思想

![image-20230113142118443](/../JVM.assets/image-20230113142118443.png)

**为什么需要把Java堆分代？不分代就不能正常工作了吗？**

- 经研究，不同对象的生命周期不同。70%-99%的对象是临时对象。
  - 新生代：有Eden、两块大小相同的Survivor (又称为from/to， s0/s1)构成，to总为空。
  - 老年代：存放新生代中经历多次GC仍然存活的对象。

- 其实不分代完全可以，分代的唯一理由就是**优化GC性能**。如果没有分代，那所有的对象都在一块，就如同把一个学校的人都关在一个教室。GC的时候要找到哪些对象没用这样就会对堆的所有区域进行扫描。而很多对象都是朝生夕死的，如果分代的话，把新创建的对象放到某一地方， 当GC的时候先把这块存储“朝生夕死”对象的区域进行回收，这样就会腾出很大的空间出来。

### 内存分配策略（或对象提升（Promotion）规则）

如果对象在Eden出生并经过第一次Minor GC后仍然存活，并且能被Survivor容纳的话，将被移动到Survivor空间中，并将对象年龄设为1。对象在Survivor区中每熬过一次Minor GC ，年龄就增加1 岁，当它的年龄增加到一定程度（默认为15岁，其实每个JVM、每个GC都有所不同）时，就会被晋升到老年代中。

对象晋升老年代的年龄阈值，可以通过选项**-XX :MaxTenuringThreshold来设置**。

针对不同年龄段的对象分配原则如下所示：

- 优先分配到Eden

- 大对象直接分配到老年代
  - 尽量避免程序中出现过多的大对象

- 长期存活的对象分配到老年代

- 动态对象年龄判断
  - 如果Survivor 区中相同年龄的所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象可以直接进入老年代，无须等到MaxTenuringThreshold中要求的年龄。

- 空间分配担保（大量的对象在GC之后依然存活，把Survivor中无法容纳的对象保存到老年区）
  - -XX : HandlePromotionFailures

### 为对象分配内存（与线程有关）：TLAB

#### 引言

**为什么有TLAB ( Thread Local Allocation Buffer ) ?**

- 堆区是线程共享区域，任何线程都可以访问到堆区中的共享数据

- 由于对象实例的创建在JVM中非常频繁，因此在并发环境下从堆区中划分内存空间是线程不安全的

- 为避免多个线程操作同一地址，需要使用加锁等机制，进而影响分配速度。

#### 概述

**什么是TLAB?**

- 从内存模型而不是垃圾收集的角度，对Eden区域继续进行划分，JVM为**每个线程分配了一个私有缓存区域**，它包含在Eden空间内。

![image-20230113143016618](/../JVM.assets/image-20230113143016618.png)

- 多线程同时分配内存时，使用TLAB可以避免一系列的非线程安全问题，同时还能够提升内存分配的吞吐量，因此我们可以将这种内存分配方式称；之为**快速分配策略。**
- 据我所知所有OpenJDK衍生出来的JVM都提供了TLAB的设计。
- **小对象会优先尝试分配在tlab上，如果太大分配失败则尝试分配在eden，如果eden也分配失败再尝试在老年代分配。——为了提高效率，减少GC**

#### 补充

- 尽管不是所有的对象实例都能够在TLAB中成功分配内存，但**JVM确实是TLAB作为内存分配的首选。**

- 在程序中，开发人员可以通过选项“-XX:UseTLAB”设置是否开启TLAB空间。**(默认是开启的)**

- 默认情况下，TLAB空间的内存非常小，仅占有整个Eden空间的1%，当然我们可以通过选项“-XX:TLABWasteTargetPercent”设置TLAB空间所占用Eden空间的百分比大小。

- 一旦对象在TLAB空间分配内存失败时，JVM就会尝试着通过**使用加锁机制**确保数据操作的原子性，从而直接在Eden空间中分配内存。

#### 总结

![image-20230113143030632](/../JVM.assets/image-20230113143030632.png)

1. **堆是共享的，访问需要加锁，而new 对象是件很频繁地事，为了提高效率，所以在Eden空间创建了TLAB区域**
2. **这个区域是线程独占的，就无须考虑线程并发问题**
3. **分配对象就像分配到TLAB区域。满了或对象过大，就会分配到老年代**
4. **这就要求我们创建对象时，尽量创建小对象，提高效率**

### 堆空间的参数设置(小结)

#### 常用参数

- -XX:+PrintFlagsInitial : 查看所有的参数的默认初始值
- -XX:+PrintFlagsFinal  ：查看所有的参数的最终值（可能会存在修改，不再是初始值）
  - 具体查看某个参数的指令
    - jps：查看当前运行中的进程
       jinfo -flag SurvivorRatio 进程id
- -Xms：初始堆空间内存 （默认为物理内存的1/64）
- -Xmx：最大堆空间内存（默认为物理内存的1/4）
- -Xmn：设置新生代的大小。(初始值及最大值)
- -XX:NewRatio：配置新生代与老年代在堆结构的占比
- -XX:SurvivorRatio：设置新生代中Eden和S0/S1空间的比例
- -XX:MaxTenuringThreshold：设置新生代垃圾的最大年龄
- -XX:+PrintGCDetails：输出详细的GC处理日志
  - 打印gc简要信息：
    - ① -XX:+PrintGC   
    - ② -verbose:gc
- -XX:HandlePromotionFailure：是否设置空间分配担保

#### Minor GC和Full GC的判定

在发生Minor GC之前，虚拟机会**检查老年代最大可用的连续空间是否大于新生代所有对象的总空间。**

- 如果大于，则此次Minor GC是安全的

- 如果小于，则虚拟机会查看-XX: HandlePromotionFailure设置值是否允许担保失败。
  - 如果HandlePromotionFailure=true， 那么会继续**检查老年代最大可用连续空间是否大于历次晋升到老年代的对象的平均大小。**
    - 如果大于，则尝试进行一次Minor GC，但这次Minor GC依然是有风险的;
    - 如果小于，则改为进行一次Full GC。
  - 如果HandlePromotionFailure=false，则改为进行一次Full GC。

#### 总结

在JDK6 Update24之 后，HandlePromotionFailure参数不会再影响到虚拟机的空间分配担保策略，观察OpenJDK中的源码变化，虽然源码中还定义了HandlePromotionFailure参数，但是在代码中已经不会再使用它。JDK6 Update24之后的规则变为<font color='red'>**只要老年代的连续空间大于新生代对象总大小或者历次晋升的平均大小就会进行Minor GC**，否则将进行Full GC。</font>

### 堆是分配对象的唯一选择吗？

在《深入理解Java虚拟机》中关于Java堆内存有这样一段描述:

随着JIT编译期的发展与**逃逸分析技术**逐渐成熟，**栈上分配、标量替换优化技术**将会导致一些微妙的变化，所有的对象都分配到堆上也渐渐变得不那么“绝对”了。

在Java虚拟机中，对象是在Java堆中分配内存的，这是一个普遍的常识。但是，有一种特殊情况，那就是**如果经过逃逸分析(Escape Analysis) 后发现，一个对象并没有逃逸出方法的话，那么就可能被优化成栈上分配**。这样就无需在堆上分配内存，也无须进行垃圾回收了。这也是最常见的堆外存储技术。.

此外，前面提到的基于openJDK深度定制的TaoBaoVM，其中创新的GCIH(GCinvisible heap) 技术实现off - heap，将生命周期较长的Java对象从heap中移至heap外，并且GC不能管理GCIH内部的Java对象，以此达到降低GC的回收频率和提升GC的回收效率的目的。

#### 逃逸分析

- 如何将堆上的对象分配到栈，需要使用逃逸分析手段。

- 这是一种可以有效减少Java程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。

- 通过逃逸分析，Java Hotspot编译器能够分析出一个新的对象的引用的使用范围从而决定是否要将这个对象分配到堆上。

- 逃逸分析的基本行为就是分析对象动态作用域:
  - 当一个对象在方法中被定义后，**对象只在方法内部使用**，则认为没有发生逃逸。
  - 当一个对象在方法中被定义后，**它被外部方法所引用**，则认为发生逃逸。例如作为调用参数传递到其他地方中。

**没有发生逃逸的对象，则可以分配到栈上，随着方法执行的结束，栈空间就被移除。**

#### 实例

没有逃逸

![image-20230113150746397](/../JVM.assets/image-20230113150746397.png)

发生逃逸

![image-20230113150808015](/../JVM.assets/image-20230113150808015.png)

```java
public class EscapeAnalysis {

    public EscapeAnalysis obj;

    /*
    方法返回EscapeAnalysis对象，发生逃逸
     */
    public EscapeAnalysis getInstance(){
        return obj == null? new EscapeAnalysis() : obj;
    }
    /*
    为成员属性赋值，发生逃逸
     */
    public void setObj(){
        this.obj = new EscapeAnalysis();
    }
    //思考：如果当前的obj引用声明为static的？仍然会发生逃逸。

    /*
    对象的作用域仅在当前方法中有效，没有发生逃逸
     */
    public void useEscapeAnalysis(){
        EscapeAnalysis e = new EscapeAnalysis();
    }
    /*
    引用成员变量的值，发生逃逸
     */
    public void useEscapeAnalysis1(){
        EscapeAnalysis e = getInstance();
        //getInstance().xxx()同样会发生逃逸
    }
}
```

**如何快速的判断是否发生了逃逸分析，大家就看new的对象实体是否有可能在方法外被调用。**

- 在JDK 6u23版本之后，HotSpot中默认就已经开启了逃逸分析。(也可以记JDK7以后)
- 如果使用的是较早的版本，开发人员则可以通过:
  - 选项“-XX: +DoEscapeAnalysis"显式开启逃逸分析
  - 通过选项“-XX: +PrintEscapeAnalysis"查看逃逸分析的筛选结果。

#### 结论

开发中能使用局部变量的，就不要使用在方法外定义。

### 逃逸分析：代码优化

使用逃逸分析，编译器可以对代码做如下优化:

一、**栈上分配**。将堆分配转化为栈分配。如果一个对象在子程序中被分配，要使指向该对象的指针永远不会逃逸，对象可能是栈分配的候选，而不是堆分配。

二、**同步省略**。如果一个对象被发现只能从一个线程被访问到，那么对于这个对象的操作可以不考虑同步。

三、**分离对象或标量替换**。有的对象可能不需要作为一个连续的内存结构存在也可以被访问到，那么对象的部分(或全部)可以不存储在内存，而是存储在CPU寄存器中。

#### 栈上分配

##### 概述

- JIT编译器在编译期间根据逃逸分析的结果，发现**如果一个对象并没有逃逸出方法的话，就可能被优化成栈上分配**。分配完成后，继续在调用栈内执行，最后线程结束，栈空间被回收，局部变量对象也被回收。这样就无须进行垃圾回收了。

- 常见的栈上分配的场景
  - 在逃逸分析中，已经说明了。分别是给**成员变量赋值、方法返回值、实例引用传递**。

##### 实例

###### 当堆空间足够大时

没有开启逃逸分析（在堆空间开辟空间）：

```java
public class StackAllocation {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();

        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        // 查看执行时间
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
        // 为了方便查看堆内存中对象个数，线程sleep
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e1) {
            e1.printStackTrace();
        }
    }

    private static void alloc() {
        User user = new User();//未发生逃逸
    }

    static class User {

    }
}
```

![image-20230113153638040](/../JVM.assets/image-20230113153638040.png)

![image-20230113153648122](/../JVM.assets/image-20230113153648122.png)

开启逃逸分析后：

![image-20230113153657749](/../JVM.assets/image-20230113153657749.png)

![image-20230113153708533](/../JVM.assets/image-20230113153708533.png)

###### 当堆空间变小后

没有开启逃逸分析：

![image-20230113153718549](/../JVM.assets/image-20230113153718549.png)

发生两次GC

![image-20230113153727547](/../JVM.assets/image-20230113153727547.png)

开启逃逸分析后：

![image-20230113153739224](/../JVM.assets/image-20230113153739224.png)

#### 同步省略

- 线程同步的代价是相当高的，同步的后果是降低并发性和性能。

- 在动态编译同步块的时候，JIT编译器可以借助逃逸分析来**判断同步块所使用的锁对象是否只能够被一个线程访问而没有被发布到其他线程**。如果没有，那么JIT编译器在编译这个同步块的时候就会取消对这部分代码的同步。这样就能大大提高并发性和性能。这个取消同步的过程就叫同步省略，也叫**锁消除**。

![image-20230113154440311](/../JVM.assets/image-20230113154440311.png)

#### 标量代替/分离对象

**标量(Scalar)**是指一个无法再分解成更小的数据的数据。Java中的原始数据类型就是标量。

相对的，那些还可以分解的数据叫做**聚合量(Aggregate)**，Java中的对象就是聚合量，因为他可以分解成其他聚合量和标量。

在JIT阶段，如果经过逃逸分析，发现一个对象不会被外界访问的话，那么经过JIT优化，就会把这个对象拆解成若干个其中包含的若干个成员变量来代替。这个过程就是**标量替换**。

![image-20230113154620699](/../JVM.assets/image-20230113154620699.png)

以上代码，经过标量替换后，就会变成：

![image-20211122105423252](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211122105423252.png)

可以看到，Point这个聚合量经过逃逸分析后，发现他并没有逃逸，就被替换成两个聚合量了。那么标量替换有什么好处呢？**就是可以大大减少堆内存的占用。因为一旦不需要创建对象了，那么就不再需要分配堆内存了。**

**标量替换为栈上分配提供了很好的基础。**

表明替换参数设置：参数-XX: +El iminateAllocationss:开启了标量替换(默认打开)，允许将对象打散分配在栈上。

#### 总结

逃逸分析并不成熟

其根本原因就是**无法保证逃逸分析的性能消耗一定能高于他的消耗虽然经过逃逸分析可以做标量替换、栈上分配、和锁消除。但是逃逸分析自身也是需要进行一系列复杂的分析的，这其实也是一个相对耗时的过程。**

虽然不成熟，但他也是即时编译器优化技术中心十分重要的手段

目前很多书籍还是基于JDK 7以前的版本，JDK已经发生了很大变化，intern字符串的缓存和静态变量曾经都被分配在永久代上，而永久代已经被元数据区取代。但是，intern字符串缓存和静态变量并不是被转移到元数据区，而是直接在堆上分配，所以这一点同样符合前面一点的结论：**对象实例都是分配在堆上。**

因为不成熟，所以对象实仍然都是分配在堆上的。

### 总结

**年轻代是对象的诞生、成长、消亡的区域，一个对象在这里产生、应用，最后被垃圾回收器收集、结束生命。**

**老年代放置长生命周期的对象，通常都是从Survivor区域筛选拷贝过来的Java对象。当然，也有特殊情况，我们知道普通的对象会被分配在TLAB上；如果对象较大，JVM会试图直接分配在Eden其他位置上；如果对象太大，完全无法在新生代找到足够长的连续空闲空间，JVM就会直接分配到老年代。**

**当GC只发生在年轻代中，回收年轻代对象的行为被称为MinorGC。当GC发生在老年代时则被称为MajorGC（只要回收的是老年代）或者FullGC（回收的是整个堆空间+方法区）。一般的，MinorGC的发生频率要比MajorGC高很多，即老年代中垃圾回收发生的频率将大大低于年轻代。**

## 方法区(元空间)

![image-20230113155103251](/../JVM.assets/image-20230113155103251.png)

从线程共享与否的角度划分

![image-20230113155122709](/../JVM.assets/image-20230113155122709.png)



### 栈、堆、方法区的交互关系

![image-20230113155132949](/../JVM.assets/image-20230113155132949.png)

![image-20230113155142444](/../JVM.assets/image-20230113155142444.png)

**person相当于方法中一个具体的线程中，线程中有一个栈，栈中的一个栈帧上的局部变量表中slot的一个引用类型（reference），指向堆空间中对象实体（对象实例数据），在对象实例数据中有一个到对象类型数据的指针，指针会指向这个对象究竟是哪个类new出来的，就指向了方法区中对象类型数据。**

### 方法区的理解

#### 概述

《Java虚拟机规范》中明确说明：“尽管所有的方法区在逻辑上是属于堆的一部分，但一些简单的实现可能不会选择去进行垃圾收集或者进行压缩。”但对于HotSpotJVM而言，方法区还有一个别名叫做Non- Heap (非堆)，目的就是要和堆分开。

所以，**方法区看作是一块独立于Java堆的内存空间。**

- 方法区(Method Area)与Java堆一样，是各个线程共享的内存区域。

- 方法区在JVM启动的时候被创建，并且它的实际的物理内存空间中和Java堆区一样都可以是不连续的。

- 方法区的大小，跟堆空间一样，可以选择固定大小或者可扩展。

- 方法区的大小决定了系统可以保存多少个类，如果系统定义了太多的类，导致方法区溢出，虛拟机同样会抛出内存溢出错误: java. lang .OutofMemoryError:**PermGen space** 或者java. lang. OutO fMemoryError: **Metaspace**
  - 加载大量的第三方jar包；
  - tomcat部署的工程过多（30-50个）；
  - 大量动态的生成反射类

- 关闭JVM就会释放这个区域的内存。

#### Hotspot中方法区的演进

- 在jdk7及以前，习惯上把方法区，称为永久代。jdk8开始，使用元空间取代了永久代。

![image-20230113155759266](/../JVM.assets/image-20230113155759266.png)

- 本质上，方法区和永久代并不等价。仅是对hotspot而言的。《Java 虛拟机规范》对如何实现方法区，不做统一要求。例如: BEA JRockit/ IBM J9中不存在永久代的概念。
  - 现在来看，当年使用永久代，不是好的idea。 导致Java程序更容易OOM (超过-XX : MaxPermSize上限)

![image-20230113155920884](/../JVM.assets/image-20230113155920884.png)

![image-20230113160005761](/../JVM.assets/image-20230113160005761.png)

- 而到了JDK 8，终于完全废弃了永久代的概念，改用与JRockit、J9一样在本地内存中实现的元空间(Metaspace)来代替

![image-20230113160103506](/../JVM.assets/image-20230113160103506.png)

- 元空间的本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代最大的区别在于：**元空间不在虚拟机设置的内存中，而是使用本地内存。**

- 永久代、元空间二者并不只是名字变了，内部结构也调整了。

- 根据《Java虚拟机规范》的规定，如果方法区无法满足新的内存分配需求时，将抛出OOM异常。

### 设置方法区大小与OOM

- 方法区的大小不必是固定的，jvm可以根据应用的需要动态调整。

- jdk7及以前:
  - 通过-XX: PermSize来设置永久代初始分配空间。默认值是20.75M
  - -XX:MaxPermSize来设定永久代最大可分配空间。32位机器默认是64M， 64位机器模式是82M
  - 当JVM加载的类信息容量超过了这个值，会报异常outOfMemoryError : PermGen space。

![image-20230113160250643](/../JVM.assets/image-20230113160250643.png)

- **jdk8及以后:**
  - 元数据区大小可以使用参数-XX :MetaspaceSize和-XX:MaxMetaspaceSize指定，替代上述原有的两个参数。
  - 默认值依赖于平台。**windows下，-XX:MetaspaceSize是21M， -XX :MaxMetaspaceSize的值是-1，即没有限制。**
  - 与永久代不同，如果不指定大小，默认情况下，虚拟机会耗尽所有的可用系统内存。如果元数据区发生溢出，虚拟机一样会抛出异常OutOfMemoryError: Metaspace
  - -XX:MetaspaceSize: 设置初始的元空间大小。对于一个64位的服务器端JVM来说，其默认的-XX:MetaspaceSize值为21MB。这就是初始的高水位线，一旦触及这个水位线，Full GC将会被触发并卸载没用的类(即这些类对应的类加载器不再存活) ，然后这个高水位线将会重置。新的高水位线的值取决于GC后释放了多少元空间。如果释放的空间不足，那么在不超过MaxMetaspaceSize时，适当提高该值。如果释放空间过多，则适当降低该值。
  - 如果初始化的高水位线设置过低，上述高水位线调整情况会发生很多次。通过垃圾回收器的日志可以观察到Full GC多次调用。为了避免频繁地GC，建议将-XX :MetaspaceSize设置为一个相对较高的值。

#### 实例

```java
public class OOMTest extends ClassLoader {
    public static void main(String[] args) {
        int j = 0;
        try {
            OOMTest test = new OOMTest();
            for (int i = 0; i < 10000; i++) {
                //创建ClassWriter对象，用于生成类的二进制字节码
                ClassWriter classWriter = new ClassWriter(0);
                //指明版本号，修饰符，类名，包名，父类，接口
                classWriter.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                //返回byte[]
                byte[] code = classWriter.toByteArray();
                //类的加载
                test.defineClass("Class" + i, code, 0, code.length);//Class对象
                j++;
            }
        } finally {
            System.out.println(/../J);
        }
    }
}
```

![image-20230113160609936](/../JVM.assets/image-20230113160609936.png)

![image-20230113160619103](/../JVM.assets/image-20230113160619103.png)

#### 如何解决OOM

1、要解决OOM异常或heap space的异常，一般的手段是首先通过内存映像分析工具(如Eclipse Memory Analyzer) 对dump出来的堆转储快照进行分析，重点是确认内存中的对象是否是必要的，也就是要先分清楚到底是出现了内存泄漏(Memory Leak)还是内存溢出(Memory Overflow)。

2、如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的引用链。于是就能找到泄漏对象是通过怎样的路径与GCRoots相关联并导致垃圾收集器无法自动回收它们的。掌握了泄漏对象的类型信息，以及GC Roots 引用链的信息，就可以比较准确地定位出泄漏代码的位置。

- 内存泄漏：有引用指向对象，这个对象之后也不会再用到，但是GC无法回收有引用指向的对象，这样的对象一旦累积很多，就会造成内存泄漏。

3、如果不存在内存泄漏，换句话说就是内存中的对象确实都还必须存活着，那就应当检查虚拟机的堆参数(-Xmx与-Xms) ，与机器物理内存对比看是否还可以调大，从代码上检查是否存在某些对象生命周期过长、持有状态时间过长的情况，尝试减少程序运行期的内存消耗。

### 方法区的内部结构

![image-20230113160853104](/../JVM.assets/image-20230113160853104.png)

#### 方法区存储内容

《深入理解Java虚拟机》书中对方法区(Method Area) 存储内容描述如下：它用于存储已被虚拟机加载的**类型信息、常量、静态变量、即时编译器编译后的码缓存**等。

![image-20230113160914449](/../JVM.assets/image-20230113160914449.png)

#### 类型信息

对每个加载的类型(类class、接口interface、枚举enum、注解annlotation)，JVM必须在方法区中存储以下类型信息:

- 这个类型的完整有效名称(全名=包名.类名)

- 这个类型直接父类的完整有效名(对于interface或是java. lang . Object，都没有父类)

- 这个类型的修饰符(public，abstract，final的某个子集)

- 这个类型直接接口的一个有序列表

#### 域信息

- JVM必须在方法区中保存类型的所有域的相关信息以及域的声明顺序。
- 域的相关信息包括：域名称、域类型、域修饰符(public, private,protected, static, final, volatile, transient的某个子集)

#### 方法信息

JVM必须保存所有方法的以下信息，同域信息一样包括声明顺序:

- 方法名称

- 方法的返回类型(或void)

- 方法参数的数量和类型(按顺序)

- 方法的修饰符(public， private， protected， static， final，synchronized， native，abstract的一个子集)

- 方法的字节码(bytecodes)、操作数栈、局部变量表及大小（abstract和native方法除外)

- 异常表(abstract和native方法除外)
  - 每个异常处理的开始位置、结束位置、代码处理在程序计数器中的偏移地址、被捕获的异常类的常量池索引

#### 实例

```java
public class MethodInnerStrucTest extends Object implements Comparable<String>,Serializable {
    //属性
    public int num = 10;
    private static String str = "测试方法的内部结构";
    //构造器
    //方法
    public void test1(){
        int count = 20;
        System.out.println("count = " + count);
    }
    public static int test2(int cal){
        int result = 0;
        try {
            int value = 30;
            result = value / cal;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }
}
```

#### non-final的类变量

- 静态变量和类关联在一起，随着类的加载而加载，它们成为类数据在逻辑上的一部分。

- 类变量被类的所有实例共享，即使没有类实例时你也可以访问它。

#### 全局常量：static final

- 被声明为final的类变量的处理法则不同，每个全局常量在变异的时候就会被分配了

#### 运行时常量池

##### 常量池

###### 概述

![image-20230113161131930](/../JVM.assets/image-20230113161131930.png)

- 方法区，内部包含了运行时常量池。

- 字节码文件，内部包含了常量池

- 要弄清楚方法区，需要理解清楚ClassFile，因为加载类的信息都在方法区

- 要弄清楚方法区的运行时常量池，需要理解清楚ClassFile中的常量池。

![image-20230113161318404](/../JVM.assets/image-20230113161318404.png)

一个有效的字节码文件中除了包含类的版本信息、字段、方法以及接口等描述信息外，还包含一项信息就是常量池表（Constant Pool Table）,包含各种字面量和对类型、域和方法的符号引用。

###### 为什么需要常量池

一个java源文件中的类、接口，编译后产生一个字节码文件。而Java中的字节码需要数据支持，通常这种数据会很大以至于不能直接存到字节码里，换另一种方式，可以存到常量池，这个字节码包含了指向常量池的引用。在动态链接的时候会用到运行时常量池，之前有介绍。

例如以下代码：

```java
public class SimpleClass{
    public void sayHello(){
        System.out.println("hello");
    }
}
```

虽然只有194字节，但是里面却使用了String、System、PrintStream及Object等结构。这里代码量其实已经很小了。如果代码多，引用到的结构会更多！这里就需要常量池了！

###### 常量池内容

几种在常量池内存储的数据类型包括:

- 数量值
- 字符串值
- 类引用
- 字段引用
- 方法引用

![image-20230113161525729](/../JVM.assets/image-20230113161525729.png)

###### 小结

**<FONT COLOR='RED'>常量池，可以看做是一张表，虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量等类型</FONT>**

##### 运行时常量池

- 运行时常量池(Runtime Constant Pool) 是方法区的一部分。

- 常量池表(Constant Pool Table)是Class文件的一部分，**用于存放编译期生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。**
- 运行时常量池，在加载类和接口到虛拟机后，就会创建对应的运行时常量池。
- JVM为每个已加载的类型(类或接口)都维护一个常量池。池中的数据项像数组项一样，是**通过索引访问**的。
- 运行时常量池中包含多种不同的常量，包括编译期就已经明确的数值字面量，也包括到运行期解析后才能够获得的方法或者字段引用。此时不再是常量池中的符号地址了，这里换为真实地址。
  - 运行时常量池，相对于Class文件常量池的另一重要特征是：**具备动态性。**

- 运行时常量池类似于传统编程语言中的符号表(symbol table) ，但是它所包含的数据却比符号表要更加丰富一些。

- 当创建类或接口的运行时常量池时，如果构造运行时常量池所需的内存空间超过了方法区所能提供的最大值，则JVM会 抛outOfMemoryError异常。

##### Class 文件格式

![image-20230113162455095](/../JVM.assets/image-20230113162455095.png)

- 本质就是二进制字节流
- 数据类型：u1 u2 u4 u8 和 _info（表类型）
  - u 表示字节
  - _info的来源是hotspot原码中的写法
- 查看16进制格式的ClassFile
  - subline\notepad\
  - IEDA插件：BinEd

![image-20230113162507031](/../JVM.assets/image-20230113162507031.png)



- 有很多可以观察ByteCode（从二进制字节流中获取信息）的方法：

  - javap（先cd到当前目录）

  ![image-20230113162515009](/../JVM.assets/image-20230113162515009.png)

  - JBE-可以直接修改
  - JClassLib-IDEA插件之一

  ![image-20230113162615268](/../JVM.assets/image-20230113162615268.png)

- classfile构成

  - ClassFile {
    	u4  magic;  
    	u2  minor_version; 	//class文件的版本号52.0（.0）
    	u2  major_version; 	//class文件的版本号52.0（52）
    	u2  constant_pool_count;	//常量池中有多少个常量（constant_pool_count-1个常量），是一个长度为constant_pool_count-1的表
    	cp_info constant_ pool [constant_pool_count - 1];
    	u2
    }

###### class文件每部分的含义(不会可以查)

![image-20230113162626808](/../JVM.assets/image-20230113162626808.png)

Magic Number

Minor_version（class文件的版本号52.0（.0））

Major_version（class文件的版本号52.0（52））

Constant pool count：常量池具体的实现（常量池类型有18种）表示常量池中的常量个数

![image-20230113162636017](/../JVM.assets/image-20230113162636017.png)

Access flags（访问标记类型）:

![image-20230113163051508](/../JVM.assets/image-20230113163051508.png)

This_class：自身在常量池中的位置

Super_class：父类在常量池中的位置

Interface count：实现了多少个接口

Fields count：多少个属性（27种）

Method count：多少个方法（21种）

![image-20230113163059396](/../JVM.assets/image-20230113163059396.png)

Attributes count：多少个附加属性（21种）

![image-20230113163106344](/../JVM.assets/image-20230113163106344.png)

###### Mehtods

```java
public static void testStatic() {
    new LocalVariablesTest();
    new Date();
    int count = 10;
    System.out.println(count);
}
```

字节码信息

![image-20230113163135080](/../JVM.assets/image-20230113163135080.png)

Misc

![image-20230113163142466](/../JVM.assets/image-20230113163142466.png)

代码和字节码文件的关系

![image-20230113163152199](/../JVM.assets/image-20230113163152199.png)

对局部变量的描述

按照执行顺序排序

![image-20230113163201404](/../JVM.assets/image-20230113163201404.png)

### 方法区使用举例

![image-20230113163210102](/../JVM.assets/image-20230113163210102.png)

步骤

![image-20230113163321151](/../JVM.assets/image-20230113163321151.png)

……

![image-20230113163340630](/../JVM.assets/image-20230113163340630.png)

### 方法区的演进细节

#### 概述

1.首先明确：**只有HotSpot才有永久代。**

BEA JRockit、 IBM J9等来说，是不存在永久代的概念的。原则上如何实现) 方法区属于虚拟机实现细节，不受《Java虚拟机规范》管束，并不要求统一。

2.Hotspot中方 法区的变化:

| 版本         | 特点                                                         |
| ------------ | :----------------------------------------------------------- |
| jdk1.7       | **有永久代，但已经逐步“去永久代”，字符串常量池、静态变量移除，保存在堆中** |
| jdk1.6及以前 | **有永久代（permanent generation）,静态变量存放在永久代上**  |
| jdk1.8及以后 | **无永久代，类型信息、字段、方法、常量保存在本地内存的元空间，但字符串常量池、静态变量仍在堆** |

![image-20230113164116093](/../JVM.assets/image-20230113164116093.png)

![image-20230113164124306](/../JVM.assets/image-20230113164124306.png)

![image-20230113164134177](/../JVM.assets/image-20230113164134177.png)

#### 永久代为什么要被元空间替换？

- 随着Java8的到来，HotSpot VM中**再也见不到永久代**了。但是这并不意味着类的元数据信息也消失了。这些数据被移到了一个与**堆不相连的本地内存区域，这个区域叫做元空间( Metaspace )。**
  
- 由于类的元数据分配在本地内存中，元空间的最大可分配空间就是系统可用内存空间。

- 这项改动是很有必要的，原因有:

  - 1)**为永久代设置空间大小是很难确定的。**
    在某些场景下，如果动态加载类过多，容易产生Perm区的OOM。比如某个实际Web工程中，因为功能点比较多，在运行过程中，要不断动态加载很多类，经常出现致命错误。

  ![image-20230113164711589](/../JVM.assets/image-20230113164711589.png)

  - 而元空间和永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。
  - 因此，默认情况下，元空间的大小仅受本地内存限制。
  - 2)**对永久代进行调优是很困难的。**

#### StringTable为什么要调整？

- jdk7中将StringTable放到了堆空间中。因为永久代的回收效率很低，在full gc的时候才会触发。而full gc是老年代的空间不足、永久代不足时才会触发。

- 这就导致StringTable回收效率不高。而我们开发中会有大量的字符串被创建，回收效率低，导致永久代内存不足。放到堆里，能及时回收内存。

#### 静态变量放在哪里

##### 实例1

```java
//-Xms200m -Xmx200m -XX:MetaspaceSize=300m -XX:MaxMetaspaceSize=300m -XX:+PrintGCDetails
public class StaticFieldTest {
    private static byte[] arr = new byte[1024 * 1024 * 100];//100MB

    public static void main(String[] args) {
        System.out.println(StaticFieldTest.arr);
    }
}
```

运行结果：

![image-20230113164953851](/../JVM.assets/image-20230113164953851.png)

##### 实例2

```java
//-Xms200m -Xmx200m -XX:MetaspaceSize=300m -XX:MaxMetaspaceSize=300m -XX:+PrintGCDetails
public class StaticObjTest {
    static class Test {
        static ObjectHolder staticObj = new ObjectHolder();
        ObjectHolder instanceObj = new ObjectHolder();

        void foo() {
            ObjectHolder localObj = new ObjectHolder();
            System.out.println("done");
        }
    }

    private static class ObjectHolder {
    }

    public static void main(String[] args) {
        Test test = new StaticObjTest.Test();
        test.foo();
    }
}
```

运行结果：

![image-20230113165004775](/../JVM.assets/image-20230113165004775.png)

**staticObj随着Test的类型信息存放在方法区，instance0bj随着Test的对象实例存放在Java堆，local0bject则是存放在foo( )方法栈帧的局部变量表中。**

![image-20230113165027783](/../JVM.assets/image-20230113165027783.png)

**测试发现：三个对象的数据在内存中的地址都落在Eden区范围内，所以结论：只要是对象实例必然会在Java堆中分配。**

接着，找到了一个引用该static0bj对象的地方，是在一个java.lang.Class的实例里，并且给出了这个实例的地址，通过Inspector查看该对象实例，可以清楚看到这矾实是一个java.lang.Class类型的对象实例，里面有一个名为staticObj的实例字段：

![image-20230113165055225](/../JVM.assets/image-20230113165055225.png)

从《Java虚拟机规范》所定义的概念模型来看，所有Class相关的信息都应该存放在方法区之中，但方法区该如何实现，《Java虚拟机规范》 并未做出规定，这就成了一件允许不同虚拟机自己灵活把握的事情。JDK7及其以后版本的HotSpot虚拟机选择把静态变量与类型在Java语言一端的映射Class对象存放在一起，存储于Java堆之中，从我们的实验中也明确验证了这一点。

结论：

**静态引用对应的对象实体始终都存在堆空间**

### 方法区的垃圾回收

有些人认为方法区(如HotSpot虚拟机中的**元空间**或者**永久代**)是没有垃圾收集行为的，其实不然。《Java虚拟机规范》对方法区的约束是非常宽松的，提到过可以不要求虚拟机在方法区中实现垃圾收集。事实上也确实有未实现或未能完整实现方法区类型卸载的收集器存在(如JDK11时期的ZGC收集器就不支持类卸载)

一般来说**这个区域的回收效果比较难令人满意，尤其是类型的卸载，条件相当苛刻。**但是这部分区域的回收**有时又确实是必要**的。以前Sun公司的Bug列表中，曾出现过的若干个严重的Bug就是由于低版本的HotSpot虚拟机对此区域未完全回收而导致内存泄漏。

**<font color='red'>方法区的垃圾收集主要回收两部分内容：常量池中废弃的常量和不再使用的类型。</font>**

#### 方法区的垃圾收集

- 先来说说方法区内常量池之中主要存放的两大类常量：字面量和符号引用。字面量比较接近Java语言层次的常量概念，如文本字符串、被声明为final的常量值等。而符号引用则属于编译原理方面的概念，包括下面三类常量：
  - 1、类和接口的全限定名
  - 2、字段的名称和描述符
  - 3、方法的名称和描述符

- HotSpot虚拟机对常量池的回收策略是很明确的，**只要常量池中的常量没有被任何地方引用，就可以被回收。**

- 回收废弃常量与回收Java堆中的对象非常类似。

- 判定一个常量是否“废弃”还是相对简单，而要判定一个类型是否属于“不再被使用的类”的条件就比较苛刻了。需要同时满足下面三个条件:
  - **该类所有的实例都已经被回收**，也就是Java堆中不存在该类及其任何派生子类的实例。
  - **加载该类的类加载器已经被回收**，这个条件除非是经过精心设计的可替换类加载器的场景，如OSGi、 JSP的重加载等，否则通常是很难达成的。
  - **该类对应的java.lang . Class对象没有在任何地方被引用**，无法在任何地方通过反射访问该类的方法。

- Java虚拟机被允许对足上述三个条件的无用类进行回收，这里说的仅仅是“被允许”，而并不是和对象一样，没有引用了就必然会回收。关于是否要对类型进行回收，HotSpot虚拟机提供了-Xnoclassgc参数进行控制，还可以使用-verbose:class以及 -XX:+TraceClass-Loading、-X:+TraceClassUnLoading查看类加载和卸载信息。

- 在大量使用反射、动态代理、CGLib等字节码框架，动态生成JSP以及OSGi这类频繁自定义类加载器的场景中，通常都**需要Java虚拟机具备类型卸载的能力，以保证不会对方法区造成过大的内存压力。**

### 总结

![image-20230113170139590](/../JVM.assets/image-20230113170139590.png)

#### 面试题

- 百度
  - 三面:说一下JVM内存模型吧，有哪些区？分别干什么的？

- 蚂蚁金服:
  - Java8的内存分代改进
  - JVM内存分哪几个区，每个区的作用是什么？
  - 一面: JVM内存分布/内存结构？栈和堆的区别？堆的结构？为什么两个survivor区？
  - 二面: Eden和Survior的比例分配

- 小米:
  - jvm内存分区，为什么要有新生代和老年代

- 字节跳动:
  - 二面: Java的内存分区
  - 二面:讲讲jvm运行时数据库区
  - 什么时候对象会进入老年代？

- 京东:
  - JVM的内存结构，Eden和Survivor比例。
  - JVM内存为什么要分成新生代，老年代，持久代。新生代中为什么要分为Eden和Survivor。
- 天猫:
  - 一面: Jvm内存模型以及分区，需要详细到每个区放什么。
  - 一面: JVM的内存模型，Java8做 了什么修改
- 拼多多:
  - JVM内存分哪几个区，每个区的作用是什么？
- 美团:
  - java内存分配
  - jvm的永久代中会发生垃圾回收吗？
  - 一面: jvm内存分区，为什么要有新生代和老年代？

## 对象的实例化内存布局与访问定位

### 对象实例化

![image-20230113172034155](/../JVM.assets/image-20230113172034155.png)

#### 创建对象的方式

![image-20230113172041153](/../JVM.assets/image-20230113172041153.png)

#### 创建对象的步骤

**1.加载类元信息**

> **判断对象对应的类是否加载、链接、初始化？**
>
> 虚拟机遇到一条new指令， 首先去检查这个指令的参数能否在Metaspace的常量池中定位到一个类的符号引用，并且检查这个符号引用代表的类是否已经被加载、解析和初始化。( 即判断类元信息是否存在)。如果没有，那么在双亲委派模式下，使用当前类加载器以ClassLoader+包名+类名为Key进行查找对应的.class文件。如果没有找到文件，则抛出ClassNotFoundException异常，如果找到，则进行类加载，并生成对应的Class类对象

**2.为对象分配内存**

> **为对象分配内存**
>
> 首先计算对象占用空间大小，接着在堆中划分一块内存给新对象。
>
> 如果实例成员变量是引用变量，仅分配引用变量空间即可，即4个字节大小。
>
> - 如果内存规整，使用指针碰撞
>
> 如果内存是规整的，那么虛拟机将采用的是指针碰撞法(Bump The Pointer) 来为对象分配内存。意思是所有用过的内存在一边，空闲的内存在另外一边，中间放着一个指针作为分界点的指示器，分配内存就仅仅是把指针向空闲那边挪动一段与对象大小相等的距离罢了。如果垃圾收集器选择的是Serial、ParNew这种基于压缩算法的，虚拟机采用这种分配方式。
>
> 一般使用带有**compact (整理)过程的收集器**时，使用指针碰撞。
>
> - 如果内存不规整，虚拟机需要维护一个列表，使用空闲列表分配
>
> 如果内存不是规整的，已使用的内存和未使用的内存相互交错，那么虚拟机将采用的是空闲列表法来为对象分配内存。意思是虚拟机维护了一个列表，记录上哪些内存块是可用的，再分配的时候从列表中找到一块足够大的空间划分给对象实例，并更新列表上的内容。这种分配方式成为“空闲列表(Free List)”。
>
> 说明：选择哪种分配方式由Java堆是否规整决定，而Java堆是否规整又由所采用的垃圾收集器**是否带有压缩整理功能决定**。

**3.处理并发问题**

> **处理并发安全问题**
>
> 在分配内存空间时，另外一个问题是及时保证new对象时候的线程安全性:创建对象是非常频繁的操作，虚拟机需要解决并发问题。虚拟机采用 了两种方式解决并发问题：
>
> - CAS(CompareAndSwap)失败重试、区域加锁：保证指针更新操作的原子性；
>
> - TLAB把内存分配的动作按照线程划分在不同的空间之中进行，即每个线程在Java堆中预先分配一小块内存，称为本地线程分配缓冲区，(TLAB ，Thread Local Allocation Buffer)虚拟机是否使用TLAB，可以通过-XX:+/ -UseTLAB参数来设定。

**4.属性的默认初始化（零值初始化）**

> **初始化分配到空间**
>
> 内存分配结束，虛拟机将分配到的内存空间都初始化为零值(不包括对象头)。这一步保证了对象的实例字段在Java代码中可以不用赋初始值就可以直接使用，程序能访问到这些字段的数据类型所对应的零值。

**5.设置对象头的信息**

> **设置对象的对象头**
>
> 将对象的所属类(即类的元数据信息)、对象的HashCode和对象的GC信息、锁信息等数据存储在对象的对象头中。这个过程的具体设置方式取决于JVM实现。

**6.属性的显式初始化、代码块中初始化、构造器中初始化**

> **执行init方法进行初始化**
>
> 在Java程序的视角看来，初始化才正式开始。初始化成员变量，执行实例化代码块，调用类的构造方法，并把堆内对象的首地址赋值给引用变量。
>
> 因此一般来说(由字节码中是否跟随有invokespecial指令所决定)，new指令之后会接着就是执行方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全创建出来。

### 对象的内存布局

![image-20230113172113763](/../JVM.assets/image-20230113172113763.png)

#### 实例

```java
public class Customer {
    int id = 1001;
    String name;
    Account acct;

    {
        name = "匿名客户";
    }

    public Customer() {
        acct = new Account();
    }

}

class Account {

}

public class CustomerTest {
    public static void main(String[] args) {
        Customer cust = new Customer();
    }
}
```

![image-20230113172125777](/../JVM.assets/image-20230113172125777.png)

### 对象的访问定位

JVM是如何通过栈帧中的对象引用访问到其他内部的对象实例的呢？

![image-20230116123704961](/../JVM.assets/image-20230116123704961.png)

 通过栈上reference访问定位到的

#### 对象访问方法主要有两种

##### 句柄访问

![image-20230113172143703](/../JVM.assets/image-20230113172143703.png)

**特点**

reference中存储稳定句柄地址,对象被移动(垃圾收集时移动对象很普遍)时只会改变句柄中实例数据指针即可，reference本身不需要被修改。

##### 直接指针（Hotspot采用的）

![image-20230113172151705](/../JVM.assets/image-20230113172151705.png)

特点

节省空间，速度快

## 直接内存

### 概述

- **不是虚拟机运行时数据区的一部分，也不是《Java虚拟机规范》中定义的内存区域。**

- 直接内存是在Java堆外的、直接向系统申请的内存区间。

- 来源于NIO，通过存在堆中的DirectByteBuf fer操作Native内存

```java
public class BufferTest {
    private static final int BUFFER = 1024 * 1024 * 1024;//1GB

    public static void main(String[] args){
        //直接分配本地内存空间
        ByteBuffer byteBuffer = ByteBuffer.allocateDirect(BUFFER);
        System.out.println("直接内存分配完毕，请求指示！");

        Scanner scanner = new Scanner(System.in);
        scanner.next();

        System.out.println("直接内存开始释放！");
        byteBuffer = null;
        System.gc();
        scanner.next();
    }
}
```

![image-20230113172203055](/../JVM.assets/image-20230113172203055.png)

释放后

![image-20230113172212694](/../JVM.assets/image-20230113172212694.png)

- 通常，访问直接内存的速度会优于Java堆。即读写性能高。
  - **因此出于性能考虑，读写频繁的场合可能会考虑使用直接内存。**
  - Java的NIO库允许Java程序使用直接内存，用于数据缓冲区

### 非直接缓冲区

读写文件，需要与磁盘交互，需要由用户态切换到内核态。在内核态时，需要内存如下图的操作。

使用IO，见右图。这里需要两份内存存储重复数据，效率低。

![image-20230113172227750](/../JVM.assets/image-20230113172227750.png)

### 直接缓冲区

使用NIO时，如下图。操作系统划出的直接缓存区可以被java代码直接访问，只有一份。NIO适合对大文件的读写操作。

![image-20230113172243795](/../JVM.assets/image-20230113172243795.png)

### 概述补充

- 也可能导致OutOfMemoryError异常

```
public class BufferTest2 {
    private static final int BUFFER = 1024 * 1024 * 20;//20MB

    public static void main(String[] args) {
        ArrayList<ByteBuffer> list = new ArrayList<>();

        int count = 0;
        try {
            while (true) {
                ByteBuffer byteBuffer = ByteBuffer.allocateDirect(BUFFER);
                list.add(byteBuffer);
                count++;
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        } finally {
            System.out.println(count);
        }
    }
}
```

![image-20230113172255559](/../JVM.assets/image-20230113172255559.png)

- 由于直接内存在Java堆外，因此它的大小不会直接受限于-Xmx指定的最大堆大小，但是系统内存是有限的，Java堆和直接内存的总和依然受限于操作系统能给出的最大内存。

- 缺点
  - **分配回收成本较高**
  - **不受JVM内存回收管理**

- 直接内存大小可以通过MaxDirectMemorySize设置

- 如果不指定，默认与堆的最大值-Xmx参数值一致

![image-20230113172308106](/../JVM.assets/image-20230113172308106.png)

**简单理解：java process memory = java heap + native memory**

# 执行引擎

![image-20230116133245308](/../JVM.assets/image-20230116133245308.png)

## 概述

- 执行引擎是Java虚拟机核心的组成部分之一。

- “虚拟机”是一个相对于“物理机”的概念，这两种机器都有代码执行能力，其区别是物理机的执行引擎是直接建立在处理器、缓存、指令集和操作系统层面上的，而**虚拟机的执行引擎则是由软件自行实现的**，因此可以不受物理条件制约地定制指令集与执行引擎的结构体系，**能够执行那些不被硬件直接支持的指令集格式。**

- JVM的主要任务是负责**装载字节码到其内部**，但字节码并不能够直接运行在操作系统之上，因为字节码指令并非等价于本地机器指令，它内部包含的仅仅只是一些能够被JVM所识别的字节码指令、符号表，以及其他辅助信息。

- 那么，如果想要让一个Java程序运行起来，执行引擎(Execution Engine)的任务就是**将字节码指令解释/编译为对应平台上的本地机器指令才可以**。简单来说，JVM中的执行引擎充当了将高级语言翻译为机器语言的译者。

### 工作过程

1) 执行引擎在执行的过程中究竟需要执行什么样的字节码指令完全依赖于PC寄存器。
1) 每当执行完一项指令操作后，PC寄存器就会更新下一条需要被执行的指令地址。
1) 当然方法在执行的过程中，执行引擎有可能会通过存储在局部变量表中的对象引用准确定位到存储在Java堆区中的对象实例信息，以及通过对象头中的元数据指针定位到目标对象的类型信息。

![image-20230116134128981](/../JVM.assets/image-20230116134128981.png)

从外观上来看，所有的Java虚拟机的执行引擎输入、输出都是一致的：**输入的是字节码二进制流，处理过程是字节码解析执行的等效过程，输出的是执行结果。**

## java代码编译和执行过程

### 概述

![image-20230116134140733](/../JVM.assets/image-20230116134140733.png)

大部分的程序代码转换成物理的目标代码或虚拟机能执行的指令集之前，都需要经过上图中给的各个步骤。

java代码编译是由java源码编译器来完成的，流程图如下（这个过程还没有JVM的参与）

![image-20230116134418325](/../JVM.assets/image-20230116134418325.png)

java字节码的执行是由JVM执行引擎来完成的，流程图如下

![image-20230116135104763](/../JVM.assets/image-20230116135104763.png)

### 解释器(Interpreter)

解释器：当Java虚拟机启动时会根据预定义的规范**对字节码采用逐行解释的方式执行**，将每条字节码文件中的内容“翻译”为对应平台的本地机器指令执行。

### JIT编译器

JIT (/../Just In Time Compiler) 编译器：就是虚拟机将源代码直接编译成和本地机器平台相关的机器语言。

### 为什么说java是半编译半解释型语言？

JDK1 . 0时代，将Java语言定位为“解释执行”还是比较准确的。再后来，Java也发展出可以直接生成本地代码的编译器。

现在JVM在执行Java代码的时候，通常都会将解释执行与编译执行二者结合起来进行。

![image-20230116135144264](/../JVM.assets/image-20230116135144264.png)

当执行引擎解释执行字节码文件时，既可以使用解释器、又可以使用即时编译器。

## 机器码、指令、汇编语言

![image-20230116135326175](/../JVM.assets/image-20230116135326175.png)

### 机器码

- 各种用二进制编码方式表示的指令，叫做**机器指令码**。开始，人们就用它采编写程序，这就是机器语言。

- 机器语言虽然能够被计算机理解和接受，但和人们的语言差别太大，不易被人们理解和记忆，并且用它编程容易出差错。

- 用它编写的程序一经输入计算机，CPU直接读取运行，因此和其他语言编的程序相比，执行速度最快。

- 机器指令与CPU紧密相关，所以不同种类的CPU所对应的机器指令也就不同。

### 指令

- 由于机器码是有0和1组成的二进制序列，可读性实在太差，于是人们发明了指令。

- 指令就是把机器码中特定的0和1序列，简化成对应的指令(一般为英文简写，如mov, inc等)，可读性稍好

- 由于不同的硬件平台，执行同一个操作，对应的机器码可能不同，所以不同的硬件平台的同一种指令(比如mov)，对应的机器码也可能不同。

#### 指令集

- 不同的硬件平台，各自支持的指令，是有差别的。因此每个平台所支持的指令，称之为对应平台的指令集。

- 如常见的
  - X86指令集，对应的是x86架构的平台
  - ARM指令集，对应的是ARM架构的平台

### 汇编语言

- 由于指令的可读性还是太差，于是人们又发明了汇编语言。

- 在汇编语言中，用**助记符**(Mnemonics)代替**机器指令的操作码，**用**地址符号(Symbol) 或标号(Iabel)** 代替**指令或操作数的地址**。

- 在不同的硬件平台，汇编语言对应着不同的机器语言指令集，通过汇编过程转换成机器指令。
  - 由于计算机只认识指令码，所以用**汇编语言编写的程序还必须翻译成机器指令码**，计算机才能识别和执行。

### 高级语言

- 为了使计算机用户编程序更容易些，后来就出现了各种高级计算机语言。高级语言比机器语言、汇编语言**更接近人的语言**

- 当计算机执行高级语言编写的程序时，**仍然需要把程序解释和编译成机器的指令码。**完成这个过程的程序就叫做解释程序或编译程序。

![image-20230116135705799](/../JVM.assets/image-20230116135705799.png)

#### 字节码

- 字节码是一种中间状态（中间码）的二进制代码（文件），它比机器码更抽象，需要直译器转译后才能成为机器码

- 字节码主要为了实现特定软件运行和软件环境、**与硬件环境无关。**

- 字节码的实现方式是通过编译器和虚拟机器。编译器将源码编译成字节码， 特定平台上的虚拟机器将字节码转译为可以直接执行的指令。
  - 字节码的典型应用为Java bytecode。

![image-20230116135715314](/../JVM.assets/image-20230116135715314.png)

#### 补充：C/C++源程序执行过程：

编译过程又可以分成两个阶段:编译和汇编。

- 编译过程：是读取源程序(字符流)，对之进行词法和语法的分析，将高级语言指令转换为功能等效的汇编代码

- 汇编过程：实际上指把汇编语言代码翻译成目标机器指令的过程。

![image-20230116135837470](/../JVM.assets/image-20230116135837470.png)

## 解释器

JVM设计者们的初衷仅仅只是单纯地**为了满足Java程序实现跨平台特性**，因此避免采用静态编译的方式直接生成本地机器指令，从而诞生了实现解释器在运行时采用逐行解释字节码执行程序的想法。

![image-20230116140028678](/../JVM.assets/image-20230116140028678.png)

- 解释器真正意义上所承担的角色就是一个运行时“翻译者”，将字节码文件中的内容“翻译”为对应平台的本地机器指令执行。

- 当一条字节码指令被解释执行完成后，接着再根据PC寄存器中记录的下一条需要被执行的字节码指令执行解释操作。

### 分类

在Java的发展历史里，一共有两套解释执行器，即古老的**字节码解释器**、现在普遍使用的**模板解释器**。

-  字节码解释器在执行时通过**纯软件代码**模拟字节码的执行，效率非常低下。

- 而模板解释器将**每一条字节码和一个模板函数相关联**，模板函数中直接产生这条字节码执行时的机器码，从而很大程度上提高了解释器的性能。
  - 在HotSpot VM中，解释器主要由Interpreter模块和Code模块构成。
    -  Interpreter模块: 实现了解释器的核心功能
    - Code模块:用于管理HotSpot VM在运行时生成的本地机器指令

### 现状

由于解释器在设计和实现上非常简单，因此除了Java语言之外，还有许多高级语言同样也是基于解释器执行的，比如Python、Perl、Ruby等。但是在今天，**基于解释器执行已经沦落为低效的代名词**，并且时常被一些C/C++程序员所调侃。

为了解决这个问题，JVM平台支持一种叫作即时编译的技术。即时编译的目的是避免函数被解释执行，而是**将整个函数体编译成为机器码，每次函数执行时，只执行编译后的机器码即可**，这种方式可以使执行效率大幅度提升。

不过无论如何，基于解释器的执行模式仍然为中间语言的发展做出了不可磨灭的贡献。

## JIT编译器

### 解释器和JIT编译器的关系

#### 概述

- 第一种是将源代码编译成字节码文件，然后在运行时通过解释器将字节码文件转为机器码执行

- 第二种是编译执行( 直接编译成机器码)。现代虚拟机为了提高执行效率，会使用即时编译技术(/../JIT，Just In Time) 将方法编译成机器码后再执行
- HotSpot VM是目前市面上高性能虚拟机的代表作之一。**它采用解释器与即时编译器并存的架构**。在Java虚拟机运行时，解释器和即时编译器能够相互协作，各自取长补短，尽力去选择最合适的方式来权衡编译本地代码的时间和直接解释执行代码的时间。
- 在今天，Java程序的运行性能早已脱胎换骨，已经达到了可以和C/C++程序一较高下的地步。

#### 疑问

有些开发人员会感觉到诧异，**既然HotSpot VM中已经内置JIT编译器了，那么为什么还需要再使用解释器来“拖累”程序的执行性能呢？**比如JRockit VM内部就不包含解释器，字节码全部都依靠即时编译器编译后执行。

- 首先明确:
  - 当程序启动后，解释器可以马上发挥作用，省去编译的时间，立即执行。
  - 编译器要想发挥作用，把代码编译成本地代码，需要一定的执行时间。但编译为本地代码后，执行效率高。

- 所以:
  - 尽管JRockitVM中程序的执行性能会非常高效，但程序在启动时必然需要花费更长的时间来进行编译。对于服务端应用来说，启动时间并非是关注重点，但对于那些看中启动时间的应用场景而言，或许就需要采用解释器与即时编译器并存的架构来换取一个平衡点。
  - 在此模式下，**当Java虛拟器启动时，解释器可以首先发挥作用，而不必等待即时编译器全部编译完成后再执行，这样可以省去许多不必要的编译时间。随着时间的推移，编译器发挥作用，把越来越多的代码编译成本地代码，获得更高的执行效率。**

**同时，解释执行在编译器进行激进优化不成立的时候，作为编译器的“逃生门”。**

#### Hotspot VM的执行方式

当虚拟机启动的时候，**解释器可以首先发挥作用**，而不必等待即时编译器全部编译完成再执行，这样可以**省去许多不必要的编译时间**。并且随着程序运行时间的推移，即时编译器逐渐发挥作用，根据热点探测功能，**将有价值的字节码编译为本地机器指令**，以换取更高的程序执行效率。

#### 实例

注意解释执行与编译执行在线上环境微妙的辩证关系。**机器在热机状态可以承受的负载要大于冷机状态**。如果以热机状态时的流量进行切流，可能使处于冷机状态的服务器因无法承载流量而假死。

在生产环境发布过程中，以分批的方式进行发布，根据机器数量划分成多个批次，每个批次的机器数至多占到整个集群的1/8。曾经有这样的故障案例：某程序员在发布平台进行分批发布，在输入发布总批数时，误填写成分为两批发布。如果是热机状态，在正常情况下一半的机器可以勉强承载流量，但由于刚启动的JVM均是解释执行，还没有进行热点代码统计和JIT动态编译，导致机器启动之后，当前1/2发布成功的服务器马上全部宕机，此故障说明了JIT的存在。——阿里团队

![image-20230116140433652](/../JVM.assets/image-20230116140433652.png)

### JIT编译器

#### 概念

- Java语言的“编译期” 其实是一 段“不确定”的操作过程，因为它可能是指一个**前端编译器**(其实叫“ 编译器的前端”更准确一些)把. java文件转变成.class文件的过程;
- 也可能是指虚拟机的**后端运行期编译器**(/../JIT编译器，Just In Time Compiler)把字节码转变成机器码的过程。
- 还可能是指使用**静态提前编译器**(AOT编译器，Ahead Of Time Compiler) 直接把. java文件编译成本地机器代码的过程。

![image-20230116141419694](/../JVM.assets/image-20230116141419694.png)

#### 热点代码及探测方式

当然是否需要启动JIT编译器将字节码直接编译为对应平台的本地机器指令，则需要根据代码被调用**执行的频率**而定。关于那些需要被编译为本地代码的字节码，也被称之为**“热点代码”**，JIT编译器在运行时会针对那些频繁被调用的“热点代码”做出**深度优化**，将其直接编译为对应平台的本地机器指令，以此提升Java程序的执行性能。

- **一个被多次调用的方法，或者是一个方法体内部循环次数较多的循环体都可以被称之为“热点代码”**，因此都可以通过JIT编译器编译为本地机器指令。由于这种编译方式发生在方法的执行过程中，因此也被称之为栈上替换，或简称为**OSR (On Stack Replacement)编译。**
- 一个方法究竟**要被调用多少次**，或者一个循环体究竟需要执行多少次循环才可以达到这个标准？必然需要一个明确的阈值，JIT编译器才会将这些“热点代码”编译为本地机器指令执行。这里主要依靠**热点探测功能。**
- **目前HotSpot VM所采用的热点探测方式是基于计数器的热点探测。**
- 采用基于计数器的热点探测，HotSpot VM将会为每一 个方法都建立2个不同类型的计数器，分别为**方法调用计数器**( Invocation Counter)和**回边计数器**(Back Edge Counter) 。
  - 方法调用计数器用于统计方法的调用次数
  - 回边计数器则用于统计循环体执行的循环次数

#### 方法调用计数器

- 这个计数器就用于统计方法被调用的次数，它的默认阀值在Client 模式下是1500 次，在Server模式下是10000次。超过这个阈值，就会触发JIT编译。

- 这个阈值可以通过虚拟机参数-XX :CompileThreshold来人为设定。

- 当一个方法被调用时，会先检查该方法是否存在被JIT编译过的版本，如果存在，则优先使用编译后的本地代码来执行。如果不存在已被编译过的版本，则将此方法的调用计数器值加1，然后判断**方法调用计数器与回边计数器值之和**是否超过方法调用计数器的阈值。如果已超过阈值，那么将会向即时编译器提交一个该方法的代码编译请求。

![image-20230116142056967](/../JVM.assets/image-20230116142056967.png)

#### 热度衰减

- 如果不做任何设置，方法调用计数器统计的并不是方法被调用的绝对次数，而是一个相对的执行频率，即**一段时间之内方法被调用的次数**。当超过**一定的时间限度**，如果方法的调用次数仍然不足以让它提交给即时编译器编译，那这个方法的调用计数器就会被**减少一半**，这个过程称为方法调用计数器热度的**衰减(Counter Decay)** ，而这段时间就称为此方法统计的**半衰周期(Counter Half Life Time)**。

- 进行热度衰减的动作是在虚拟机进行垃圾收集时顺便进行的，可以使用虚拟机参数**xx:-UseCounterDecay**来关闭热度衰减，让方法计数器统计方法调用的绝对次数，这样，只要系统运行时间足够长，绝大部分方法都会被编译成本地代码。

- 另外，可以使用**-xx: CounterHalfLifeTime**参数设置半衰周期的时间，单位是秒。

#### 回边计数器

它的作用是统计一个方法中**循环体代码执行的次数**，在字节码中遇到控制流向后跳转的指令称为“回边”(Back Edge) 。显然，建立回边计数器统计的目的就是为了触发OSR编译。

![image-20230116142209190](/../JVM.assets/image-20230116142209190.png)

#### Hotspot VM可以设置程序执行方式

缺省情况下HotSpot VM是采用解释器与即时编译器并存的架构，当然开发人员可以根据具体的应用场景，通过命令显式地为Java虚拟机指定在运行时到底是**完全采用解释器**执行，还是**完全采用即时编译器**执行。如下所示:

- **-Xint：**完全采用解释器模式执行程序;

- **-Xcomp：**完全采用即时编译器模式执行程序。如果即时编译出现问题，解释器会介入执行。

- **-Xmixed：**采用解释器+即时编译器的混合模式共同执行程序。

![image-20230116142247960](/../JVM.assets/image-20230116142247960.png)

#### Hotspot VM中JIT的分类

在HotSpot VM中内嵌有两个JIT编译器，分别为Client Compiler和Server Compiler，但大多数情况下我们简称为C1编译器和C2编译器。开发人员可以通过如下命令显式指定Java虚拟机在运行时到底使用哪一种即时编译器， 如下所示: 

- -client：指定Java虚拟机运行在Client模式下，并使用C1编译器：
  - C1编译器会对字节码进行**简单和可靠的优化，耗时短**。以达到更快的编译速度。

- -server：指定Java虚拟机运行在Server模式下，并使用C2编译器：
  - C2进行**耗时较长的优化，以及激进优化**。但优化的代码执行效率更高。


#### 分层编译

**分层编译(Tiered Compilation)策略**：程序解释执行(不开启性能监控)可以触发C1编译，将字节码编译成机器码，可以进行简单优化，也可以加上性能监控，C2编译会根据性能监控信息进行激进优化。

不过在Java7版本之后，一旦开发人员在程序中显式指定命令“-server" 时，默认将会开启分层编译策略，由C1编译器和C2编译器相互协作共同来执行编译任务。

#### C1和C2编译器不同的优化策略

- 在不同的编译器上有不同的优化策略，C1编译器上主要有**方法内联，去虚拟化、冗余消除。**
  - 方法内联：将引用的函数代码编译到引用点处，这样可以减少栈帧的生成，减少参数传递以及跳转过程
  - 去虚拟化：对唯一的实现类进行内联
  - 冗余消除：在运行期间把一些不会执行的代码折叠掉

- C2的优化主要是在全局层面，逃逸分析是优化的基础。基于**逃逸分析**在C2上有如下几种优化:
  - 标量替换：用标量值代替聚合对象的属性值
  - 栈上分配：对于未逃逸的对象分配对象在栈而不是堆
  - 同步消除：清除同步操作，通常指synchronize

#### 总结

- 一般来讲，JIT编译出来的机器码性能比解释器高。

- C2编译器启动时长比C1编译器慢，系统稳定执行以后，C2编译器执行速度远远快于C1编译器。

### 补充

#### Graal编译器

- 自JDK10起，HotSpot又加入一个全新的即时编译器: **Graal编译器**。

- 编译效果短短几年时间就追评了C2编译器。未来可期。

- 目前，带着“实验状态"标签，需要使用开关参数
  -XX: +UnlockExper imentalVMoptions -XX: +UseJVMCICompiler去激活，才可以使用。

#### 关于AOT编译器

- jdk9引入了AOT编译器(静态提前编译器，Ahead Of Time Compiler)

- Java 9引入了实验性AOT编译工具jaotc。它借助了Graal 编译器，将所输入的Java 类文件转换为机器码，并存放至生成的动态共享库之中。
- 所谓AOT编译，是与即时编译相对立的一个概念。我们知道，即时编译指的是在**程序的运行过程中**，将字节码转换为可在硬件上直接运行的机器码，并部署至托管环境中的过程。而AOT编译指的则是，在**程序运行之前**，便将字节码转换为机器码的过程。

.java->.class->.so

- 最大好处: Java 虚拟机加载已经预编译成二进制库，可以直接执行。不必等待即时编译器的预热，减少Java应用给人带来“第一次运行慢”的不良体验。
- 缺点:
  - 破坏了java"一次编译，到处运行”，必须为每个不同硬件、OS编译对应的发行包。
  - **降低了Java链接过程的动态性**，加载的代码在编译期就必须全部已知。
  - 还需要继续优化中，最初只支持Linux x64 java base

# StringTable

## String的基本特性

- String:字符串x使用一对""引起来表示。.
- String声明为final的，不可被继承
- String实现了Serializable接口:表示字符串是支持序列化的。
  - 实现了Comparable接口:表示String 可以比较大小
- String在jdk8及以前内部定义了final char[ ] value用于存储字符串数据。
  - jdk9时改为byte[ ]

jdk8里的String

![image-20230116142645239](/../JVM.assets/image-20230116142645239.png)

jdk9里面的String

![image-20230116142656166](/../JVM.assets/image-20230116142656166.png)

> ### 存储结构变更
>
> 大部分的String对象包含的都是拉丁字符，这些字符用一个byte就可以存储，而用char存储时，就会有一半的空间被浪费掉。
>
> 将内部的空间进行取代，从utf-16改为一个byte数组和字符编码集标识（拉丁字符，英语一个字符存储；汉字……一个存不下的，用两个）
>
> **结论：String再也不用cahr[]来存储了，改为byte[]加上编码标记，节约了一些空间。**
>
> Stringbuffer和Stringbuilder中一些基于String的类，包括HotSport VM的一些内部的类也做了一些调整

- String：代表不可变的字符序列。简称：**不可变性**。

  - 当对字符串重新赋值时，需要重写指定内存区域赋值，不能使用原有的value进行赋值。

  ```java
  @Test
  public void test1() {
      String s1 = "abc";//字面量定义的方式，"abc"存储在字符串常量池中
      String s2 = "abc";
      s1 = "hello";
  
      System.out.println(s1 == s2);//判断地址：true  --> false
  
      System.out.pr  intln(s1);//
      System.out.println(s2);//abc
  }
  ```

  ![image-20230116143617263](/../JVM.assets/image-20230116143617263.png)

  - 当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值。

  ```java
  @Test
  public void test2() {
      String s1 = "abc";
      String s2 = "abc";
      s2 += "def";
      System.out.println(s2);//abcdef
      System.out.println(s1);//abc
  }
  ```

  ![image-20230116143625789](/../JVM.assets/image-20230116143625789.png)

  - 当调用String的replace ()方法修改指定字符或字符串时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值。

  ```java
  @Test
  public void test3() {
      String s1 = "abc";
      String s2 = s1.replace('a', 'm');
      System.out.println(s1);//abc
      System.out.println(s2);//mbc
  }
  ```

  ![image-20230116143634324](/../JVM.assets/image-20230116143634324.png)

- 通过字面量的方式(区别于new)给一个字符串赋值，此时的字符串值声明在字符串常量池中。
  
- **字符串常量池中是不会存储相同内容的字符串的。**

  - String的String Pool是 一个固定大小的Hashtable，默认值大小长度是1009。**如果放进String Pool的String非常多， 就会造成Hash冲突严重，从而导致链表会很长，而链表长了后直接会造成的影响就是当调用string. intern时性能会大幅下降**。
  - 使用-XX : StringTableSize可设置StringTable的长度
  - 在jdk6中StringTable是固定的，就是1009的长度，所以如果常量池中的字符串过多就会导致效率下降很快。StringTableSize 设置没有要求 
  - 在jdk7中，StringTable的长度默认值是60013，StringTable的长度最小值没有要求
  - jdk8开始，StringTable的长度可设置的最小值是 1009。

## String的内存分配

- 在Java语言中有8种基本数据类型和一种比较特殊的类型String。这些类型为了使它们在运行过程中速度更快、更节省内存，都提供了一种常量池的概念。
- 常量池就类似一个Java系统级别提供的缓存。8种基本数据类型的常量池都是系统协调的，**String类型的常量池比较特殊。它的主要使用方法有两种。**
  - 直接使用双引号声明出来的String对象会直接存储在常量池中。
    - 比如: String info = "atguigu. com";
  - 如果不是用双引号声明的String对象，可以使用String提供的intern()方法。这个后面重点谈
- Java 6及以前，字符串常量池存放在永久代。
- Java 7中Oracle的工程师对字符串池的逻辑做了很大的改变，即**将字符串常量池的位置调整到Java堆内。**
  - ➢所有的字符串都保存在堆(Heap)中，和其他普通对象一样，这样可以让你在进行调优应用时仅需要调整堆大小就可以了。
  - ➢字符串常量池概念原本使用得比较多，但是这个改动使得我们有足够的理由让我们重新考虑在Java 7中使用String. intern()。
- Java8元空间，字符串常量在堆

![image-20230116143718453](/../JVM.assets/image-20230116143718453.png)

![image-20230116143726181](/../JVM.assets/image-20230116143726181.png)

### **为什么StringTable要调整？（从永久代调整到堆）**

**jdk7中将StringTable放到了堆空间中。因为永久代的回收效率很低，在full GC的时候才会触发。而Full GC是老年代空间不足、永久代空间不足时才会触发。这就导致StringTable回收效率不高。而我们开发中会有大量的字符串被创建，回收效率低，导致永久代内存不足。放到堆里，能及时回收内存。**

## 字符串基本操作

### 实例1

```java
public class StringTest4 {
    public static void main(String[] args) {
        System.out.println();//2293
        System.out.println("1");//2294
        System.out.println("2");
        System.out.println("3");
        System.out.println("4");
        System.out.println("5");
        System.out.println("6");
        System.out.println("7");
        System.out.println("8");
        System.out.println("9");
        System.out.println("10");//2303
        //如下的字符串"1" 到 "10"不会再次加载
        System.out.println("1");//2304
        System.out.println("2");//2304
        System.out.println("3");
        System.out.println("4");
        System.out.println("5");
        System.out.println("6");
        System.out.println("7");
        System.out.println("8");
        System.out.println("9");
        System.out.println("10");//2304
    }
}
```

Java语言规范里要求完全相同的字符串字面量，应该包含同样的Unicode字符序列(包含同一份码点序列的常量)，并且必须是指向同一个String类实例。

### 实例2

```java
class Memory {
    public static void main(String[] args) {//line 1
        int i = 1;//line 2
        Object obj = new Object();//line 3
        Memory mem = new Memory();//line 4
        mem.foo(obj);//line 5
    }//line 9

    private void foo(Object param) {//line 6
        String str = param.toString();//line 7
        System.out.println(str);
    }//line 8
}
```

![image-20230116143804801](/../JVM.assets/image-20230116143804801.png)

## 字符串的拼接操作

1. 常量与常量的拼接结果在常量池，原理是编译期优化

2. 常量池中不会存在相同内容的常量。

3. 只要其中有一个是变量，结果就在堆中。变量拼接的原理是StringBuilder

4. 如果拼接的结果调用intern()方法，则主动将常量池中还没有的字符串对象放入池中，并返回此对象地址

### 实例

```java
public class StringTest5 {
    @Test
    public void test1(){
        String s1 = "a" + "b" + "c";//编译期优化：等同于"abc"
        String s2 = "abc"; //"abc"一定是放在字符串常量池中，将此地址赋给s2
        /*
         * 最终.java编译成.class,再执行.class
         * String s1 = "abc";
         * String s2 = "abc"
         */
        System.out.println(s1 == s2); //true
        System.out.println(s1.equals(s2)); //true
    }

    @Test
    public void test2(){
        String s1 = "javaEE";
        String s2 = "hadoop";

        String s3 = "javaEEhadoop";
        String s4 = "javaEE" + "hadoop";//编译期优化
        //如果拼接符号的前后出现了变量，则相当于在堆空间中new String()，具体的内容为拼接的结果：javaEEhadoop
        String s5 = s1 + "hadoop";
        String s6 = "javaEE" + s2;
        String s7 = s1 + s2;

        System.out.println(s3 == s4);//true
        System.out.println(s3 == s5);//false
        System.out.println(s3 == s6);//false
        System.out.println(s3 == s7);//false
        System.out.println(s5 == s6);//false
        System.out.println(s5 == s7);//false
        System.out.println(s6 == s7);//false
        //intern():判断字符串常量池中是否存在javaEEhadoop值，如果存在，则返回常量池中javaEEhadoop的地址；
        //如果字符串常量池中不存在javaEEhadoop，则在常量池中加载一份javaEEhadoop，并返回次对象的地址。
        String s8 = s6.intern();
        System.out.println(s3 == s8);//true
    }

    @Test
    public void test3(){
        String s1 = "a";
        String s2 = "b";
        String s3 = "ab";
        /*
        如下的s1 + s2 的执行细节：(变量s是我临时定义的）
        ① StringBuilder s = new StringBuilder();
        ② s.append("a")
        ③ s.append("b")
        ④ s.toString()  --> 约等于 new String("ab")

        补充：在jdk5.0之后使用的是StringBuilder,在jdk5.0之前使用的是StringBuffer
         */
        String s4 = s1 + s2;//
        System.out.println(s3 == s4);//false
    }
    /*
    1. 字符串拼接操作不一定使用的是StringBuilder!
       如果拼接符号左右两边都是字符串常量或常量引用，则仍然使用编译期优化，即非StringBuilder的方式。
    2. 针对于final修饰类、方法、基本数据类型、引用数据类型的量的结构时，能使用上final的时候建议使用上。
     */
    
    @Test
    public void test4(){
        final String s1 = "a";
        final String s2 = "b";
        String s3 = "ab";
        String s4 = s1 + s2;
        System.out.println(s3 == s4);//true
    }
    
    //练习：
    @Test
    public void test5(){
        String s1 = "javaEEhadoop";
        String s2 = "javaEE";
        String s3 = s2 + "hadoop";
        System.out.println(s1 == s3);//false

        final String s4 = "javaEE";//s4:常量
        String s5 = s4 + "hadoop";
        System.out.println(s1 == s5);//true

    }

    /*
    体会执行效率：通过StringBuilder的append()的方式添加字符串的效率要远高于使用String的字符串拼接方式！
    详情：1.1.StringBuilder的append()的方式：自始至终中只创建过一个StringBuilder的对象
         1.2.使用String的字符串拼接方式：创建过多个StringBuilder和String的对象
         2.使用String的字符串拼接方式：内存中由于创建了较多的StringBuilder和String的对象，内存占用更大；如果进行GC，需要花费额外的时间。

    改进的空间：在实际开发中，如果基本确定要前前后后添加的字符串长度不高于某个限定值highLevel的情况下,建议使用构造器实例化：
               StringBuilder s = new StringBuilder(highLevel);//new char[highLevel]
     */
    
    @Test
    public void test6(){

        long start = System.currentTimeMillis();

//        method1(100000);//4014
        method2(100000);//7

        long end = System.currentTimeMillis();

        System.out.println("花费的时间为：" + (end - start));
    }

    public void method1(int highLevel){
        String src = "";
        for(int i = 0;i < highLevel;i++){
            src = src + "a";//每次循环都会创建一个StringBuilder、String
        }
//        System.out.println(src);

    }

    public void method2(int highLevel){
        //只需要创建一个StringBuilder
        StringBuilder src = new StringBuilder();
        for (int i = 0; i < highLevel; i++) {
            src.append("a");
        }
//        System.out.println(src);
    }
}
```

## intern()的使用

### intern()的理解

当intern方法被调用的时候，用equals方法判断，字符串常量池中是否包含新创建的对象，如果包含就将之前的字符串返回，否则的话新创建的对象添加到字符串常量池。

如果不是用双引号声明的String对象，可以使用String提供的intern方法：intern方法会从字符串常量池中查询当前字符串是否存在，若不存在就会将当前字符串放入常量池中。

比如: String myInfo = new String("I love j'a'va") .intern() ;

也就是说，如果在任意字符串上调用String. intern方法，那么其返回结果所指向的那个类实例，必须和直接以常量形式出现的字符串实例完全相同。因此，下列表达的值必定是true：("a" + "b" + "c") . intern() == "abc"

通俗点讲，Interned string就是**确保字符串在内存里只有一份拷贝，这样可以节约内存空间，加快字符串操作任务的执行速度**。注意，这个值会被存放在字符串内部池(String Intern Pool)。

### 实例

#### 实例1 

```java
public class StringNewTest {
    public static void main(String[] args) {
        String str = new String("ab");

        String str = new String("a") + new String("b");
    }
}
```

- 题目：
  - new String("ab")会创建几个对象？
    - 看字节码，就知道是两个。
    - 一个对象是：new关键字在堆空间创建的
    - 另一个对象是：字符串常量池中的对象"ab"。 字节码指令：ldc
    - ![image-20230116145428689](/../JVM.assets/image-20230116145428689.png)
- 思考：
  - new String("a") + new String("b")呢？
    -  对象1：new StringBuilder()
    -  对象2： new String("a")
    -  对象3： 常量池中的"a"
    -  对象4： new String("b")
    -  对象5： 常量池中的"b"
    - ![image-20230116145527444](/../JVM.assets/image-20230116145527444.png)
-  深入剖析：
  -  StringBuilder的toString():
    - 对象6 ：new String("ab")
  - 强调一下，toString()的调用，在字符串常量池中，没有生成"ab"

#### 实例2

如何保证变量s指向的是字符串常量池中的数据呢？

- 方式一：String s = "shkstart";//字面量定义的方式
- 方式二：调用intern()
  - String s = new String("shkstart").intern();
  -  String s = new StringBuilder("shkstart").toString().intern();

```java
public class StringIntern {
    public static void main(String[] args) {

        String s = new String("1");
        s.intern();//调用此方法之前，字符串常量池中已经存在了"1"
        String s2 = "1";
        System.out.println(s == s2);
        //jdk6：false
        //jdk7/8：false

        
        String s3 = new String("1") + new String("1");
        //s3变量记录的地址为：new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？不存在！！
        s3.intern();//在字符串常量池中生成"11"。
        // 如何理解： jdk6:创建了一个新的对象"11",也就有新的地址。
        //          jdk7:此时常量中并没有创建"11",而是创建一个指向堆空间中new String("11")的地址
        String s4 = "11";//s4变量记录的地址：使用的是上一行代码代码执行时，在常量池中生成的"11"的地址
        System.out.println(s3 == s4);
        //jdk6：false  
        //jdk7/8：true
    }
}
```

jdk6

![image-20230116145758724](/../JVM.assets/image-20230116145758724.png)

jdk7/8

![image-20230116145813135](/../JVM.assets/image-20230116145813135.png)

#### 实例3（扩展）

```java
public class StringIntern1 {
    public static void main(String[] args) {
        //StringIntern.java中练习的拓展：
        String s3 = new String("1") + new String("1");
        //new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！
        String s4 = "11";//在字符串常量池中生成对象"11"
        String s5 = s3.intern();
        System.out.println(s3 == s4);//false
        System.out.println(s5 == s4);//true
    }
}
```

### 总结

- jdk1.6中，将这个字符串对象尝试放入串池。
  - 如果串池中有，则并不会放入。返回已有的串池中的对象的地址
  - 如果没有，会把**此对象复制一份**，放入串池，并返回串池中的**对象地址**

- Jdk1.7起，将这个字符串对象尝试放入串池。
  - 如果串池中有，则并不会放入。返回已有的串池中的对象的地址
  - 如果没有，则会把**对象的引用地址复制一份**，放入串池，并返回串池中的**引用地址**

### intern()的效率测试（空间角度）

```java
public class StringIntern2 {
    static final int MAX_COUNT = 1000 * 10000;
    static final String[] arr = new String[MAX_COUNT];

    public static void main(String[] args) {
        Integer[] data = new Integer[]{1,2,3,4,5,6,7,8,9,10};

        long start = System.currentTimeMillis();
        for (int i = 0; i < MAX_COUNT; i++) {
		  //arr[i] = new String(String.valueOf(data[i % data.length]));
            arr[i] = new String(String.valueOf(data[i % data.length])).intern();

        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为：" + (end - start));

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.gc();
    }
}
```

使用intern()测试执行效率：空间使用上

结论：**对于程序中大量存在存在的字符串，尤其其中存在很多重复字符串时，使用intern()可以节省内存空间，而且可提高效率。**

大的网站平台，需要内存中存储大量的字符串。比如社交网站，很多)人都存储：北京市、海淀区等信息。这时候如果字符串都调用intern()方法，就会明显降低内存的大小。

## StringTable的垃圾回收

```java
public class StringGCTest {
    public static void main(String[] args) {
        for (int j = 0; j < 100000; j++) {
            String.valueOf(/../J).intern();
        }
    }
}
```

![image-20230116150106961](/../JVM.assets/image-20230116150106961.png)

发生垃圾回收行为

## G1中的String去重操作（了解）

- 背景:对许多Java应用( 有大的也有小的)做的测试得出以下结果:
  - 堆存活数据集合里面string对象占了25%
  - 堆存活数据集合里面重复的String对象有13.5%
  - String对象的平均长度是45
- 许多大规模的Java应用的瓶颈在于内存，测试表明，在这些类型的应用里面，**Java堆中存活的数据集合差不多25%是String对象**。更进一步，这里面差不多一半String对象是重复的，重复的意思是说:stringl. equals (string2)=true。**堆上存在重复的String对象必然是一种内存的浪费**。这个项目将在G1垃圾收集器中实现自动持续对重复的string对象进行去重，这样就能避免浪费内存。，
- 实现
  - 当垃圾收集器工作的时候，会访问堆上存活的对象。**对每一个访问的对象都会检查是否是候选的要去重的String对象。**
  - 如果是，把这个对象的一个引用插入到队列中等待后续的处理。一个去重的线程在后台运行，处理这个队列。处理队列的一个元素意味着从队列删除这个元素，然后尝试去重它引用的String对象。
  - 使用一个hashtable来记录所有的被String对象使用的不重复的char数组。当去重的时候，会查这个hashtable，来看堆上是否已经存在一个一模一样的char数组。
  - 如果存在，string对象会被调整引用那个数组，释放对原来的数组的引用，最终会被垃圾收集器回收掉。
  - 如果查找失败，char数组会被插入到hashtable，这样以后的时候就可以共享这个数组了。
- 命令行选项
  - UseStringDeduplication (bool) :开启String去重，**默认是不开启的，需要手动开启。**
  - PrintStr ingDedupl icationStatistics (bool) :打印详细的去重统计信息
  - StringDeduplicat ionAgeThreshold (uintx) :达到这个年龄的String对象被认为是去重的候选对象

# 垃圾回收

![image-20230116150957522](/../JVM.assets/image-20230116150957522.png)

## 垃圾回收概述

- 垃圾收集，不是Java语言的伴生产物。早在1960年，第一门开始使用内存动态分配和垃圾收集技术的Lisp语言诞生。

- 关于垃圾收集有三个经典问题:
  - 哪些内存需要回收？
  - 什么时候回收？
  - 如何回收？

- 垃圾收集机制是Java的招牌能力，**极大地提高了开发效率**。如今，垃圾收集几乎成为现代语言的标配，即使经过如此长时间的发展，Java的垃圾收集机制仍然在不断的演进中，不同大小的设备、不同特征的应用场景，对垃圾收集提出了新的挑战，这当然也是**面试的热点**。

**常见面试题**

- 蚂蚁金服:
  - 你知道哪几种垃圾回收器，各自的优缺点，重点讲一下cms和g1
  - 一面: JVM GC算法有哪些，目前的JDK版本采用什么回收算法
  - 一面:G1回收器讲下回收过程，
    - GC是什么？为什么要有GC？
  - 一面:GC的两种判定方法？CMS收集器与G1收集器的特点。
- 百度:
  - 说一下GC算法，分代回收说下
  - 垃圾收集策略和算法
- 天猫:
  - 一面: jvm GC原理，JVM怎么回收内存
  - 一面: CMS特点，垃圾回收算法有哪些？各自的优缺点，他们共同的缺点是什么？
- 滴滴:
  - 一面: java的垃圾回收器都有哪些，说下g1的应用场景，平时你是如何搭配使用垃圾回收器的

- 京东:
  - 你知道哪几种垃圾收集器，各自的优缺点，重点讲下cms和G1，包括原理流程，优缺点。
  - 垃圾回收算法的实现原理。
- 阿里:
  - 讲一讲垃圾回收算法。
  - 什么情况下触发垃圾回收?
  - 如何选择合适的垃圾收集算法?
  - JVM有哪三种垃圾回收器?
- 字节跳动:
  - 常见的垃圾回收器算法有哪些，各有什么优劣?
  - system.gc ()和runtime.gc()会做什么事情?
  - 一面: Java GC机制? GC Roots有哪些?
  - 二面: Java对象的回收方式，回收算法。
  - CMS和G1了解么，CMS解决什么问题，说一下回收的过程。
  - CMS回收停顿了几次，为什么要停顿两次。

### 什么是垃圾

- 什么是垃圾( Garbage) 呢?
- 垃圾是指在**运行程序中没有任何指针指向的对象**，这个对象就是需要被回收的垃圾。
- 外文: An object is considered garbage when it can no longer be reached from any pointer in the running progr am.
- 如果不及时对内存中的垃圾进行清理，那么，这些垃圾对象所占的内存空间会一直保留到应用程序结束，被保留的空间无法被其他对象使用。甚至可能**导致内存溢出**。

### 为什么需要GC

- 对于高级语言来说，一个基本认知是如果不进行垃圾回收，**内存迟早都会被消耗完**，因为不断地分配内存空间而不进行回收，就好像不停地生产生活垃圾而从来不打扫一样。

- 除了释放没用的对象，垃圾回收也可以清除内存里的记录碎片。碎片整理将所占用的堆内存移到堆的一端，以便**JVM将整理出的内存分配给新的对象**。

- 随着应用程序所应付的业务越来越庞大、复杂，用户越来越多，**没有GC就不能保证应用程序的正常进行**。而经常造成STW的GC又跟不上实际的需求，所以才会不断地尝试对GC进行优化。

### 早期垃圾回收

- 在早期的C/C+ +时代，垃圾回收基本上是手工进行的。开发人员可以使用new关键字进行内存申请，并使用de lete关键字进行内存释放。比如以下代码:

  ![image-20230118125519458](/../JVM.assets/image-20230118125519458.png)

- 这种方式可以灵活控制内存释放的时间，但是会给开发人员带来**频繁申请和释放内存的管理负担**。倘若有一处内存区间由于程序员编码的问题忘记被回收，那么就会产生**内存泄漏**，垃圾对象永远无法被清除，随着系统运行时间的不断增长，垃圾对象所耗内存可能持续_上升，直到出现内存溢出并造成**应用程序崩溃**。

- 在有了垃圾回收机制后，上述代码快极有可能变成这样：

  ![image-20230118133237312](/../JVM.assets/image-20230118133237312.png)

- 现在，除了Java以外，C#、Python、Ruby等语言都使用了自动垃圾回收的思想，也是未来发展趋势。可以说，这种自动化的内存分配和垃圾回收的方式己经成为现代开发语言必备的标准。

### java垃圾回收机制

- 自动内存管理，无需开发人员手动参与内存的分配与回收，这样**降低内存泄漏和内存溢出的风险**
  - 没有垃圾回收器，java也会和cpp一样，各种悬垂指针，野指针，泄露问题让你头疼不已。
- 自动内存管理机制，将程序员从繁重的内存管理中释放出来，可以**更专心地专注于业务开发**
- oracle官网关于垃圾回收的介绍
  - https://docs. oracle. com/javase/8/docs/technotes/guides/vm/gctuning/ toc.html

#### 担忧

- 对于Java开发人员而言，自动内存管理就像是一个黑匣子，如果过度依赖于“自动”，那么这将会是一场灾难，最严重的就会**弱化Java开发人员在程序出现内存溢出时定位问题和解决问题的能力。**
- 此时，了解JVM的自动内存分配和内存回收原理就显得非常重要，只有在真正了解JVM是如何管理内存后，我们才能够在遇见outOfMemoryError时，快速地根据错误异常日志定位问题和解决问题。
- 当需要排查各种内存溢出、内存泄漏问题时，当垃圾收集成为系统达到更高并发量的瓶颈时，我们就必须对这些“自动化”的技术**实施必要的监控和调节。**

#### 主要操作的是方法区和堆

#### 总结

- 垃圾回收器可以对年轻代回收，也可以对老年代回收，甚至是全堆和方法区的回收。
  - 其中，**Java堆是垃圾收集器的工作重点。**

- 从次数上讲:
  - **频繁收集Young区**
  - **较少收集old区**
  - **基本不动Perm区（或元空间）**

## 垃圾回收相关算法

### 标记阶段：引用计数算法

#### 概述

在堆里存放着几乎所有的Java对象实例，在GC执行垃圾回收之前，首先**需要区分出内存中哪些是存活对象，哪些是已经死亡的对象**。只有被标记为己经死亡的对象，GC才会在执行垃圾回收时，释放掉其所占用的内存空间，因此这个过程我们可以称为**垃圾标记阶段**。

那么在JVM中究竞是如何标记一个死亡对象呢?简单来说，当一个对象已经不再被任何的存活对象继续引用时，就可以宣判为已经死亡。

判断对象存活一般有两种方式:**引用计数算法**和**可达性分析算法**。

- 引用计数算法(Reference Counting) 比较简单，对每个对象保存一个整型的**引用计数器属性。用于记录对象被引用的情况。**
- 对于一个对象A，只要有任何一个对象引用了A，则A的引用计数器就加1；当引用失效时，引用计数器就减1。只要对象A的引用计数器的值为0，即表示对象A不可能再被使用，可进行回收。
- 优点：**实现简单，垃圾对象便于辨识；判定效率高，回收没有延迟性**。

- 缺点:
  - 它需要单独的字段存储计数器，这样的做法增加了**存储空间的开销**。
  - 每次赋值都需要更新计数器，伴随着加法和减法操作，这增加了**时间开销**。
  - 引用计数器有一个严重的问题，即**无法处理循环引用**的情况。这是一条致命缺陷，**导致在Java的垃圾回收器中没有使用这类算法**。
    - 循环引用
    - ![image-20230118133551683](/../JVM.assets/image-20230118133551683.png)

#### 实例

```java
public class RefCountGC {
    //这个成员属性唯一的作用就是占用一点内存
    private byte[] bigSize = new byte[5 * 1024 * 1024];//5MB

    Object reference = null;

    public static void main(String[] args) {
        RefCountGC obj1 = new RefCountGC();
        RefCountGC obj2 = new RefCountGC();

        obj1.reference = obj2;
        obj2.reference = obj1;

        obj1 = null;
        obj2 = null;
        //显式的执行垃圾回收行为
        //这里发生GC，obj1和obj2能否被回收？
        System.gc();

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

图示

![image-20230118133604846](/../JVM.assets/image-20230118133604846.png)

如果不下小心直接把obj1-reference和0bj2-reference置null。则在Java堆当中的两块内存依然保持着互相引用，无法回收。

运行结果：发生gc时，obj1和obj2都被回收了

结论：java使用的不是引用计数算法

#### 小结

- 引用计数算法，是很多语言的资源回收选择，例如因人工智能而更加火热的Python，它更是同时支持引用计数和垃圾收集机制。

- 具体哪种最优是要看场景的，业界有大规模实践中仅保留引用计数机制，以提高吞吐量的尝试。

- Java并没有选择引用计数，是因为其存在一个基本的难题，也就是很难处理循环引用关系。

- Python如何解决循环引用?
  - 手动解除：很好理解，就是在合适的时机，解除引用关系。
  - 使用弱引用weakref，weakref 是Python提供的标准库，旨在解决循环引用。

### 标记阶段：可达性分析算法

#### 概述

- 也可以称为**根搜索算法、追踪性垃圾收集**

- 相对于引用计数算法而言，可达性分析算法不仅同样具备实现简单和执行高效等特点，更重要的是该算法可以有效地**解决在引用计数算法中循环引用的问题，防止内存泄漏的发生**。

- 相较于引用计数算法，这里的可达性分析就是**Java、C#选择的**。这种类型的垃圾收集通常也叫作**追踪性垃圾收集**（Tracing Garbage Collection)。

- 所谓"GC Roots"根集 合就是一组必须活跃的引用。

- 基本思路：
  - 可达性分析算法是以根对象集合(GC Roots) 为起始点，按照从上至下的方式**搜索被根对象集合所连接的目标对象是否可达**。
  - 使用可达性分析算法后，内存中的存活对象都会被根对象集合直接或间接连接着，搜索所走过的路径称为**引用链(Reference Chain)**
  - 如果目标对象没有任何引用链相连，则是不可达的，就意味着该对象己经死亡，可以标记为垃圾对象。
  - 在可达性分析算法中，只有能够被根对象集合直接或者间接连接的对象才是存活对象。
  - ![image-20230118133626790](/../JVM.assets/image-20230118133626790.png)

 #### GC Roots

在Java语言中，GC Roots包括以下几类元素: 

- 虚拟机栈中引用的对象
  - 比如:各个线程被调用的方法中使用到的参数、局部变量等。
- 本地方法栈内JNI (通常说的本地方法)引用的对象
- 方法区中类静态属性引用的对象
  - 比如: Java类的引用类型静态变量
- 方法区中常量引用的对象
  - 比如:字符串常量池(String Table) 里的引用
- 所有被同步锁synchronized持有的对象
- Java虚拟机内部的引用。
  - 基本数据类型对应的Class对象，一些常驻的异常对象(如：NullPointerException、OutOfMemoryError)，系统类加载器。
- 反映java虚拟机内部情况的JMXBean、JVMTI中注册的回调、本地代码缓存等。.
- ![image-20230118133919815](/../JVM.assets/image-20230118133919815.png)
- 除了这些固定的GC Roots集合以外，根据用户所选用的垃圾收集器以及当前回收的内存区域不同，还可以有其他对象“临时性”地加入，共同构成完整GC Roots集合。比如:**分代收集和局部回收(Partial GC)。**
  - 如果只针对Java堆中的某一块区域进行垃圾回收(比如: 典型的只针对新生代)，必须考虑到内存区域是虚拟机自己的实现细节，更不是孤立封闭的，这个区域的对象完全有可能被其他区域的对象所引用，这时候就需要一并将关联的区域对象也加入GC Roots集合中去考虑，才能保证可达性分析的准确性。
- 小技巧:
  - 由于Root采用栈方式存放变量和指针，所以如果一个指针， 它保存了堆内存里面的对象，但是自己又不存放在堆内存里面，那它就是一个Root
- 如果要使用可达性分析算法来判断内存是否可回收，那么分析工作必须在一个能保障一致性的快照中进行。这点不满足的话分析结果的准确性就无法保证。
- 这点也是导致GC进行时必须“stop The World" 的一个重要原因。
  - 即使是号称(几乎)不会发生停顿的CMS收集器中，**枚举根节点时也是必须要停顿的**。

### 对象的finalization机制

#### 概述

- Java语言提供了对象终止( finalization)机制来允许开发人员提供**对象被销毁之前的自定义处理逻辑。**
- 当垃圾回收器发现没有引用指向一个对象，即：**垃圾回收此对象之前，总会先调用这个对象的finalize()方法。**
- finalize()方法允许在子类中被重写，**用于在对象被回收时进行资源释放**。通常在这个方法中进行一些资源释放和清理的工作，比如关闭文件、套接字和数据库连接等。
- 永远不要主动调用某个对象的finalize()方法，应该交给垃圾回收机制调用。理由包括下面三点:
  - 在finalize() 时可能会导致对象复活。
  - finalize()方法的执行时间是没有保障的，它完全由GC线程决定，极端情况下，若不发生GC，则finalize() 方法将没有执行机会。
  - .一个糟糕的finalize()会严重影响GC的性能。
- 从功能上来说，finalize() 方法与C++中的析构函数比较相似，但是Java采用的是基于垃圾回收器的自动内存管理机制，所以finalize() 方法在本质上不同于C+ +中的析构函数。
- 由于finalize()方法的存在，**虚拟机中的对象一般处于三种可能的状态。**

#### 生存还是死亡

- 如果从所有的根节点都无法访问到某个对象，说明对象己经不再使用了。一般来说，此对象需要被回收。但事实上，也并非是“非死不可”的，这时候它们暂时处于“缓刑”阶段。**一个无法触及的对象有可能在某一个条件下“复活”自己**，如果这样，那么对它的回收就是不合理的，为此，定义虚拟机中的对象可能的三种状态。如下:
  - **可触及的**：从根节点开始，可以到达这个对象。
  - **可复活的**：对象的所有引用都被释放，但是对象有可能在finalize()中复活。
  - **不可触及的**：对象的finalize()被调用，并且没有复活，那么就会进入不可触及状态。不可触及的对象不可能被复活，因为**finalize()只会被调用一次**。
- 以上3种状态中，是由于finalize()方法的存在，进行的区分。只有在对象不可触及时才可以被回收。

#### 具体过程

- 判定一个对象objA是否可回收，至少要经历两次标记过程：
  - 如果对象objA到GC Roots没有引用链，则进行第一次标记。
  - 进行筛选，判断此对象是否有必要执行finalize()方法
    - 如果对象objA没有重写finalize()方法，或者finalize()方法已经被虚拟机调用过，则虚拟机视为“没有必要执行”，objA被判定为不可触及的。
    - 如果对象objA重写了finalize()方法，且还未执行过，那么objA会被插入到F-Queue队列中，由一个虚拟机自动创建的、低优先级的Finalizer线程触发其finalize()方法执行。
    - ③**finalize() 方法是对象逃脱死亡的最后机会**，稍后GC会对F-Queue队列中的对象进行第二次标记。**如果objA在finalize()方法中与引用链上的任何一个对象建立了联系**，那么在第二次标记时，objA会被移出“即将回收”集合。之后，对象会再次出现没有引用存在的情况。在这个情况下，finalize方法不会被再次调用，对象会直接变成不可触及的状态，也就是说，一个对象的finalize方法只会被调用一次。

#### 复活实例

```java
public class CanReliveObj {
    public static CanReliveObj obj;//类变量，属于 GC Root


    //此方法只能被调用一次
    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("调用当前类重写的finalize()方法");
        obj = this;//当前待回收的对象在finalize()方法中与引用链上的一个对象obj建立了联系
    }
 
    public static void main(String[] args) {
        try {
            obj = new CanReliveObj();
            // 对象第一次成功拯救自己
            obj = null;
            System.gc();//调用垃圾回收器
            System.out.println("第1次 gc");
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
            System.out.println("第2次 gc");
            // 下面这段代码与上面的完全相同，但是这次自救却失败了
            obj = null;
            System.gc();
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### MAT与JProfiler的GC Roots溯源

#### 软件

MAT：[Eclipse Memory Analyzer Open Source Project |日食基金会](http://www.eclipse.org/mat/)

JProfiler：[ej-technologies - Java APM, Java Profiler, Java Installer Builder](https://www.ej-technologies.com/download/jprofiler/files)

### 清除阶段：标记-清除算法

当成功区分出内存中存活对象和死亡对象后，GC接下来的任务就是执行垃圾回收，释放掉无用对象所占用的内存空间，以便有足够的可用内存空间为新对象分配内存。

目前在JVM中比较常见的三种垃圾收集算法是：标记-清除算法( Mark-Sweep)、复制算法(Copying)、标记-压缩算法(Mark-Compact )

#### 标记-清除（Mark-Sweep算法）

- 背景：
  - 标记-清除算法( Mark-Sweep )是一种非常基础和常见的垃圾收集算法，该算法被J . McCarthy等人在1960年提出并并应用于Lisp语言。

- 执行过程:
  - 当堆中的有效内存空间(available memory)被耗尽的时候，就会停止整个程序(也被称为stop the world) ，然后进行两项工作，第一项则是标记，第二项则是清除。
    - **标记： Collector从引用根节点开始遍历，标记所有被引用的对象。一般是在对象的Header中记录为可达对象。**
    - **清除：Collector对堆内 存从头到尾进行线性的遍历，如果发现某个对象在其Header中没有标记为可达对象，则将其回收。**
- ![image-20230118135414140](/../JVM.assets/image-20230118135414140.png)

#### 不足

- 缺点
  - 效率不算高
  - 在进行GC的时候，需要停止整个应用程序，导致用户体验差
  - 这种方式清理出来的空闲内存是不连续的，产生内存碎片。需要维护一个空闲列表
- 注意：何为清除?
  - 这里所谓的清除并不是真的置空，而是把需要清除的对象地址保存在空闲的地址列表里。下次有新对象需要加载时，判断垃圾的位置空间是否够，如果够，就存放。

### 清除算法：复制算法

#### 概述

背景:

为了解决标记-清除算法在垃圾收集效率方面的缺陷，M.L.Minsky于1963年发表了著名的论文，“ 使用双存储区的Li sp语言垃圾收集器CALISP Garbage Collector Algorithm Using Serial Secondary Storage )M. L.Minsky在该论文中描述的算法被人们称为复制(Copying)算法，它也被M. L.Minsky本人成功地引入到了Lisp语言的一个实现版本中。

核心思想:

将活着的内存空间分为两块，每次只使用其中一块，在垃圾回收时将正在使用的内存中的存活对象复制到未被使用的内存块中，之后清除正在使用的内存块中的所有对象，交换两个内存的角色，最后完成垃圾回收。

#### 图示

![image-20230118140559103](/../JVM.assets/image-20230118140559103.png)

#### 优缺点

- 优点:
  - 没有标记和清除过程，实现简单，运行高效。
  - 复制过去以后保证空间的连续性，不会出现“碎片”问题。

- 缺点:
  - 此算法的缺点也是很明显的，就是需要两倍的内存空间。
  - 对于G1这种分拆成为大量region的GC，复制而不是移动，意味着GC需要维护region之间对象引用关系，不管是内存占用或者时间开销也不小。

- 特点:
  - **如果系统中的需要复制的对象很多时，复制算法就不会很理想； 复制算法需要复制的存活对象数量并不会太大，或者说非常低才行。**


### 清除算法：标记-压缩算法

#### 概述

背景:

复制算法的高效性是建立在存活对象少、垃圾对象多的前提下的。这种情况在新生代经常发生，但是在老年代，更常见的情况是大部分对象都是存活对象。如果依然使用复制算法，由于存活对象较多，复制的成本也将很高。因此，**基于老年代垃圾回收的特性，需要使用其他的算法。**

标记-清除算法的确可以应用在老年代中，但是该算法不仅执行效率低下，而且在执行完内存回收后还会产生内存碎片，所以JVM的设计者需要在此基础之上进行改进。标记-压缩(Mark - Compact) 算法由此诞生。

1970年前后，G. L. steele、c. J. Chene 和D.s. wise 等研究者发布标记-压缩算法。在许多现代的垃圾收集器中，人们都使用了标记-压缩算法或其改进版本。

 #### 执行过程

第一阶段和标记清除算法一样，从根节点开始标记所有被引用对象

第二阶段将所有的存活对象压缩到内存的一端，按顺序排放。

之后，清理边界外所有的空间

#### 图示

![image-20230118140630058](/../JVM.assets/image-20230118140630058.png)

标记-压缩算法的最终效果等同于标记-清除算法执行完成后，再进行一-次内存碎片整理，因此，也可以把它称为**标记-清除-压缩(Mark- sweep-Compact)算法。**

二者的本质差异在于标记-清除算法是一种**非移动式的回收算法**，标记-压缩是**移动式**的。是否移动回收后的存活对象是一项优缺点并存的风险决策。

可以看到，标记的存活对象将会被整理，按照内存地址依次排列，而未被标记的内存会被清理掉。如此一来， 当我们需要给新对象分配内存时，JVM只需要持有一个内存的起始地址即可，这比维护一个空闲列表显然少了许多开销。

#### 优缺点

- 优点:
  - 消除了复制算法当中，内存减半的高额代价。

- 缺点:
  - 从效率上来说，标记-整理算法要低于复制算法。
  - 移动对象的同时，如果对象被其他对象引用，则还需要调整引用的地址。
  - 移动过程中，需要全程暂停用户应用程序。即:STW

### 清除算法小结

|          | 标记-清除          | 标记-压缩          | 复制                             |
| -------- | ------------------ | ------------------ | -------------------------------- |
| 速度     | 中等               | 最慢               | 最快                             |
| 空间开销 | 少（但会堆积碎片） | 少（不会堆积碎片） | 通常需要对象的两倍（不堆积碎片） |
| 移动对象 | 否                 | 是                 | 是                               |

效率上来说，复制算法是当之无愧的老大，但是却浪费了太多内存。

而为了尽量兼顾上面提到的三个指标，标记-整理算法相对来说更平滑一些，但是效率上不尽如人意，它比复制算法多了一个标记的阶段，比标记-清除多了一个整理内存的阶段。

### 分代收集算法

前面所有这些算法中，并没有一种算法可以完全替代其他算法，它们都具有自己独特的优势和特点。分代收集算法应运而生。

分代收集算法，是基于这样一个事实:不同的对象的生命周期是不一样的。因此，**不同生命周期的对象可以采取不同的收集方式，以便提高回收效率。**一般是把Java堆分为新生代和老年代，这样就可以根据各个年代的特点使用不同的回收算法，以提高垃圾回收的效率。

在Java程序运行的过程中，会产生大量的对象，其中有些对象是与业务信息相关，比如Http**请求中的Session对象、线程、Socket连接**，这类对象跟业务直接挂钩，因此生命周期比较长。但是还有一些对象，主要是程序运行过程中生成的临时变量，这些对象生命周期会比较短，比如: **String对象**， 由于其不变类的特性，系统会产生大量的这些对象，有些对象甚至只用一次即可回收。



**目前几乎所有的GC都是采用分代收集( Generational Collecting) 算法执行垃圾回收的。**

在HotSpot中，基于分代的概念，GC所使用的内存回收算法必须结合年轻代和老年代各自的特点。

- 年轻代(Young Gen)
  - 年轻代特点:区域相对老年代较小，对象生命周期短、存活率低，回收频繁。
  - 这种情况复制算法的回收整理，速度是最快的。复制算法的效率只和当前存活对象大小有关，因此很适用于年轻代的回收。而复制算法内存利用率不高的问题，通过hotspot中的两个survivor的设计得到缓解。

- 老年代(Tenured Gen)
  - 老年代特点:区域较大，对象生命周期长、存活率高，回收不及年轻代频繁。
  - 这种情况存在大量存活率高的对象，复制算法明显变得不合适。一般是由**标记-清除**或者是标记-清除与标记-整理的混合实现。
    - **Mark阶段的开销与存活对象的数量成正比。**
    - **Sweep阶段的开销与所管理区域的大小成正相关。**
    - **Compact阶段的开销与存活对象的数据成正比。**

以HotSpot中的CMR回收器为例，CMS是基于Mark- Sweep实现的，对于对象的回收效率很高。而对于碎片问题，CMS采用基于Mark-Compact算法的Serial old回收器作为补偿措施：当内存回收不佳(碎片导致的Concurrent ModeFailure时)，将采用Serial old执行Full GC以达到对老年代内存的整理。

分代的思想被现有的虚拟机广泛使用。几乎所有的垃圾回收器都区分新生代和老年代。

### 增量收集算法、分区算法 

#### 增量收集算法

##### 概述

上述现有的算法，在垃圾回收过程中，应用软件将处于一种stop the World的状态。在**stop the world** 状态下，应用程序所有的线程都会挂起，暂停一切正常的工作，等待垃圾回收的完成。如果垃圾回收时间过长，应用程序会被挂起很久，**将严重影响用户体验或者系统的稳定性**。为了解决这个问题，即对实时垃圾收集算法的研究直接导致了增量收集( Incremental Collecting) 算法的诞生。

##### 基本思想

如果一次性将所有的垃圾进行处理，需要造成系统长时间的停顿，那么就可以让垃圾收集线程和应用程序线程交替执行。每次，**垃圾收集线程只收集一小片区域的内存空间，接着切换到应用程序线程。依次反复，直到垃圾收集完成。**

总的来说，增量收集算法的基础仍是传统的标记-清除和复制算法。增量收集算法通**过对线程间冲突的妥善处理，允许垃圾收集线程以分阶段的方式完成标记、清理或复制工作。**

##### 缺点 

使用这种方式，由于在垃圾回收过程中，间断性地还执行了应用程序代码, 所以能减少系统的停顿时间。但是，因为线程切换和上下文转换的消耗，会使得垃圾回收的总体成本上升，**造成系统吞吐量的下降。**

#### 分区算法 

##### 概述

一般来说，在相同条件下，堆空间越大，一次GC时所需要的时间就越长，有关GC产生的停顿也越长。为了更好地控制GC产生的停顿时间，将一块大的内存区域分割成多个小块，根据目标的停顿时间，每次合理地回收若干个小区间，而不是整个堆空间，从而减少一次GC所产生的停顿。

分代算法将按照对象的生命周期长短划分成两个部分，分区算法将整个堆空间划分成连续的不同小区间。

每一个小区间都独立使用，独立回收。这种算法的好处是可以控制一次回收多少个小区间。

##### 图示

![image-20230118141853524](/../JVM.assets/image-20230118141853524.png)

**注意，这些只是基本的算法思路，实际GC实现过程要复杂的多，目前还在发展中的前沿GC都是复合算法，并且并行和并发兼备。**

## 垃圾回收相关概念

### System.gc()的理解

#### 概述

- 在默认情况下，通过System. gc ()或者Runtime . getRuntime() .gc ()的调用，**会显式触发Full GC**，同时对老年代和新生代进行回收，尝试释放被丢弃对象占用的内存。
- 然而System. gc ()调用附带一个免责声明，无法保证对垃圾收集器的调用。
- JVM实现者可以通过system. gc ()调用来决定JVM的GC行为。而一般情况下，垃圾回收应该是自动进行的，**无须手动触发，否则就太过于麻烦了**。在一些特殊情况下，如我们正在编写一个性能基准，我们可以在运行之间调用System.gc()。

#### 实例1

```java
public class SystemGCTest {
    public static void main(String[] args) {
        new SystemGCTest();
        System.gc();//提醒jvm的垃圾回收器执行gc,但是不确定是否马上执行gc
        //与Runtime.getRuntime().gc();的作用一样。

        System.runFinalization();//强制调用使用引用的对象的finalize()方法
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("SystemGCTest 重写了finalize()");
    }
}
```

运行结果：

时有时没有，因为他只是负责声明，但是不能保证对垃圾收集器的调用

![image-20230118142007030](/../JVM.assets/image-20230118142007030.png)

![image-20230118142022687](/../JVM.assets/image-20230118142022687.png)

当加入了System.runFinalization();时会强制调用使用引用的对象的finalize()方法。

#### 实例2

```java
public class LocalVarGC {
    //没有回收
    public void localvarGC1() {
        byte[] buffer = new byte[10 * 1024 * 1024];//10MB
        System.gc();
    }

    //发生回收:重命名了
    public void localvarGC2() {
        byte[] buffer = new byte[10 * 1024 * 1024];
        buffer = null;
        System.gc();
    }

    //没有回收
    public void localvarGC3() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        System.gc();
    }

    //发生回收:原本存储buffer的地方被value占用，没有引用指向new byte[10 * 1024 * 1024]，所以就会被GC回收
    public void localvarGC4() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        int value = 10;
        System.gc();
    }

    //发生回收：第一次没有回收，第二次回收了
    public void localvarGC5() {
        localvarGC1();
        System.gc();
    }

    public static void main(String[] args) {
        LocalVarGC local = new LocalVarGC();
        local.localvarGC5();
    }
}
```

![image-20230118142215386](/../JVM.assets/image-20230118142215386.png)

![image-20230118142226428](/../JVM.assets/image-20230118142226428.png)

![image-20230118142237259](/../JVM.assets/image-20230118142237259.png)

![image-20230118142249129](/../JVM.assets/image-20230118142249129.png)

![image-20230118142258340](/../JVM.assets/image-20230118142258340.png)

### 内存溢出与内存泄漏

#### 内存溢出（OOM）

- 内存溢出相对于内存泄漏来说，尽管更容易被理解，但是同样的，内存溢出也是引发程序崩溃的罪魁祸首之一。

- 由于GC一直在发展，所有一般情况下，除非应用程序占用的内存增长速度非常快，造成**垃圾回收已经跟不上内存消耗的速度**，否则不太容易出现OOM的情况。

- 大多数情况下，GC会进行各种年龄段的垃圾回收，实在不行了就放大招，来一次独占式的Full GC操作，这时候会回收大量的内存，供应用程序继续使用。

- javadoc中对outofMemoryError的解释是，**没有空闲内存，并且垃圾收集器也无法提供更多内存。**
-  首先说没有空闲内存的情况：说明Java虚拟机的堆内存不够。原因有二：
- **(1) Java虚拟机的堆内存设置不够。**
  - 比如：可能存在内存泄漏问题；也很有可能就是堆的大小不合理，比如我们要处理比较可观的数据量，但是没有显式指定JVM堆大小或者指定数值偏小。我们可以通过参数-Xms、-Xmx来调整。
- **(2)代码中创建了大量大对象，并且长时间不能被垃圾收集器收集(存在被引用)**
  - 对于老版本的Oracle JDK， 因为永久代的大小是有限的，并且JVM对永久代垃圾回收（如，常量池回收、卸载不再需要的类型）非常不积极，所以当我们不断添加新类型的时候，永久代出现OutOfMemoryError也非常多见，尤其是在运行时存在大量动态类型生成的场合；类似intern字符串缓存占用太多空间，也会导致OOM问题。对应的异常信息，会标记出来和永久代相关: "**java. lang . OutOfMemoryError: PermGen space**"。
- 随着元数据区的引入，方法区内存已经不再那么窘迫，所以相应的OOM有所改观，出现OOM， 异常信息则变成了:“**java . lang . OutOfMemoryError: Metaspace**"。直接内存不足，也会导致OOM。
- 这里面隐含着一层意思是，在抛出OutOfMemoryError之前，通常垃圾收集器会被触发，尽其所能去清理出空间。
  - 例如:在引用机制分析中，涉及到JVM会去尝试回收**软引用指向的对象等。**
  - 在java. nio. BIts.reserveMemory()方法中，我们能清楚的看到，System. gc()会被调用，以清理空间。
- 当然，也不是在任何情况下垃圾收集器都会被触发的
  - 比如，我们去分配一个超大对象，类似一个超大数组超过堆的最大值，JVM可以判断出垃圾收集并不能解决这个问题，所以直接拋出OutOfMemoryError.

#### 内存泄漏

- 也称作“存储渗漏”。**严格来说，只有对象不会再被程序用到了，但是GC又不能回收他们的情况，才叫内存泄漏。.**

- 但实际情况很多时候一些不太好的实践(或疏忽)会导致对象的生命周期变得很长甚至导致0OM，也可以叫做**宽泛意义上的“内存泄漏”**

- 尽管内存泄漏并不会立刻引起程序崩溃，但是一旦发生内存泄漏，程序中的可用内存就会被逐步蚕食，直至耗尽所有内存，最终出现outOfMemory异常，导致程序崩溃。

- 注意，这里的存储空间并不是指物理内存，而是指虚拟内存大小，这个虚拟内存大小取决于磁盘交换区设定的大小。

##### 图示

![image-20230118143249393](/../JVM.assets/image-20230118143249393.png)

不要使用引用计数算法，如下：

![image-20230118143257894](/../JVM.assets/image-20230118143257894.png)

java没有使用引用计数算法

##### 实例

1、单例模式

单例的生命周期和应用程序是一样长的，所以单例程序中，如果持有对外部对象的引用的话，那么这个外部对象是不能被回收的，则会导致内存泄漏的产生。

2、一些提供**close()**的资源未关闭导致内存泄漏
数据库连接(dataSourse. getConnection())，网络连接(socket)和io连接必须手动close，否则是不能被回收的。

### Stop The World

- Stop-the-World，简称STW，指的是GC事件发生过程中，会产生应用程序的停顿。**停顿产生时整个应用程序线程都会被暂停，没有任何响应**，有点像卡死的感觉，这个停顿称为STW。
  - 可达性分析算法中枚举根节点(GC Roots)会导致所有Java执行线程停顿。
    - 分析工作必须在一个能确保一致性的快照中进行
    - 一致性指整个分析期间整个执行系统看起来像被冻结在某个时间点上
    - **如果出现分析过程中对象引用关系还在不断变化，则分析结果的准确性无法保证**

- 被STW中断的应用程序线程会在完成GC之后恢复，频繁中断会让用户感像是网速不快造成电影卡带-样，所以我们需要减少STW的发生。
- STW事件和采用哪款GC无关，所有的GC都有这个事件。
- 哪怕是G1也不能完全避免Stop-the-world情况发生，只能说垃圾回收器越来越优秀，回收效率越来越高，**尽可能地缩短了暂停时间**。
- STW是JVM在**后台自动发起和自动完成**的。在用户不可见的情况下，把用
  户正常的工作线程全部停掉。
- 开发中不要用System.gc() ；会导致Stop- the-world的发生。

#### 实例

```java
public class StopTheWorldDemo {
    public static class WorkThread extends Thread {
        List<byte[]> list = new ArrayList<byte[]>();

        public void run() {
            try {
                while (true) {
                    for(int i = 0;i < 1000;i++){
                        byte[] buffer = new byte[1024];
                        list.add(buffer);
                    }

                    if(list.size() > 10000){
                        list.clear();
                        System.gc();//会触发full gc，进而会出现STW事件
                    }
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static class PrintThread extends Thread {
        public final long startTime = System.currentTimeMillis();

        public void run() {
            try {
                while (true) {
                    // 每秒打印时间信息
                    long t = System.currentTimeMillis() - startTime;
                    System.out.println(t / 1000 + "." + t % 1000);
                    Thread.sleep(1000);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        WorkThread w = new WorkThread();
        PrintThread p = new PrintThread();
        w.start();
        p.start();
    }
}
```

开启WorkThread线程之前

![image-20230118143323799](/../JVM.assets/image-20230118143323799.png)

开启WorkThread线程之后

![image-20230118143338659](/../JVM.assets/image-20230118143338659.png)

### 垃圾回收的并行与并发

#### 并发

在操作系统中，是指**一个时间段**中有几个程序都处于已启动运行到运行完毕之间，且这几个程序都是在**同一个处理器上**运行。

并发不是真正意义上的“同时进行”，只是CPU把一个时间段划分成几个时间片段(时间区间)，然后在这几个时间区间之间来回切换，由于CPU处理的速度非常快，只要时间间隔处理得当，即可让用户感觉是多个应用程序同时在进行。

![image-20230118143723717](/../JVM.assets/image-20230118143723717.png)

#### 并行

当系统有一个以上CPU时，当一个CPU执行一个进程时，另一个CPU可以执行另一个进程，两个进程互不抢占CP附资源，可以同时进行，我们称之为并行(Parallel)。

其实决定并行的因素不是CPU的数量，而是CPU的核心数量，比如一个CPU多个核也可以并行。

适合科学计算，后台处理等弱交互场景

![image-20230118143732728](/../JVM.assets/image-20230118143732728.png)

#### 对比

并发，指的是多个事情，**在同一时间段内同时发生了**。

并行，指的是多个事情，**在同一时间点上同时发生了**。

并发的多个任务之间是互相抢占资源的。

并行的多个任务之间是不互相抢占资源的。.

只有在多CPU或者一个CPU多核的情况中，才会发生并行。

否则，看似同时发生的事情，其实都是并发执行的。

#### 垃圾回收中的应用

并发和并行，在谈论垃圾收集器的上下文语境中，它们可以解释如下:

- 并行(Parallel) ：指**多条垃圾收集线程并行工作**，但此时用户线程仍处于等待状态。
  - 如ParNew、 Parallel Scavenge、 Parallel old;

- 串行(Serial)
  - 相较于并行的概念，单线程执行。
  - 如果内存不够，则程序暂停，启动JVM垃圾回收器进行垃圾回收。回收完，再启动程序的线程。
- 并发(Concurrent) ：指**用户线程与垃圾收集线程同时执行**(但不一定是并行的，可能会交替执行)，垃圾回收线程在执行时不会停顿用户程序的运行。
  - 用户程序在继续运行，而垃圾收集程序线程运行于另一个CPU上；
  - 如: CMS、G1

![image-20230118144008722](/../JVM.assets/image-20230118144008722.png)

![image-20230118144017080](/../JVM.assets/image-20230118144017080.png)

![image-20230118144026676](/../JVM.assets/image-20230118144026676.png)

### 安全点与安全区域

#### 安全点

程序执行时并非在所有地方都能停顿下来开始GC， 只有在特定的位置才能停顿下来开始GC,这些位置称为“安全点(Safepoint )”

Safe Point的选择很重要，**如果太少可能导致GC等待的时间太长，如果太频繁可能导致运行时的性能问题**。大部分指令的执行时间都非常短暂，通常会根据“**是否具有让程序长时间执行的特征**”为标准。比如：选择些执行时间较长的指令作为Safe Point， 如**方法调用、循环跳转和异常跳转等**。

**如何在GC发生时，检查所有线程都跑到最近的安全点停顿下来呢?**

- **抢先式中断：(目前没有虚拟机采用了)**
  - 首知中断所有线程。如果还有线程不在安全点，就恢复线程，让线程跑到安全点。
- 主动式中断：
  - 设置一个中断标志，各个线程运行到Safe Point的时 候主动轮询这个标志，如果中断标志为真，则将自己进行中断挂起。

#### 安全区域

Safepoint机制保证了程序执行时，在不太长的时间内就会遇到可进入GC的Safepoint 。但是，程序“不执行”的时候呢？例如线程处于sleep状态或Blocked状态，这时候线程无法响应JVM的中断请求，“走” 到安全点去中断挂起，JVM也不太可能等待线程被唤醒。对于这种情况，就需要安全区域(Safe Region)来解决。

**安全区域是指在一段代码片段中，对象的引用关系不会发生变化，在这个区域中的任何位置开始GC都是安全的**。我们也可以把Safe Region 看做是被扩展了的Safepoint。

**开始运行时**

1、当线程运行到Safe Region的代码时， 首先标识已经进入了Safe Regior如果这段时间内发生GC，JVM会忽略标识为Safe Region状态的线程; 

2、当线程即将离开Safe Region时，会检查JVM是否已经完成GC，如果完成了，则继续运行，否则线程必须等待直到收到可以安全离开SafeRegion的信号为止;

### 再谈引用

我们希望能描述这样一类对象: 当内存空间还足够时，则能保留在内存中; t如果内存空间在进行垃圾收集后还是很紧张，则可以抛弃这些对象。

**[既偏门又非常高频的面试题]强引用、软引用、弱引用、虚引用有什么区别？具体使用场景是什么？**

在JDK 1. 2版之后，Java对引用的概念进行了扩充，将引用分为强引用(Strong Reference)、软引用(Soft Reference) 、弱引用(Weak Reference)和虛引用(Phantom Reference) 4种，**这4种引用强度依次逐渐减弱**。

除强引用外，其他3种引用均可以在java. lang. ref包中找到它们的身影。如下图，显示了这3种引用类型对应的类，开发人员可以在应用程序中直接使用它们。

Reference子类中只有终结器引用是包内可见的，其他3种引用类型均为public,可以在应用程序中直接使用

- **强引用(StrongReference) :** 最传统的“引用”的定义，是指在程序代码之中普遍存在的引用赋值，即类似“Object obj=new object()”这种引用关系。**无论任何情况下，只要强引用关系还存在，垃圾收集器就永远不会回收掉被引用的对象。**（死也不回收，99%的情况）
- **软引用(SoftReference)：** 在系统将要发生内存溢出之前，将会把这些对象列入回收范围之中进行第二次回收。如果这次回收后还没有足够的内存，才会抛出内存溢出异常。（内存不足才回收，缓存时使用较多）
- **弱引用(WeakReference) :**被弱引用关联的对象只能生存到下一次垃圾收集之前。当垃圾收集器工作时，无论内存空间是否足够，都会回收掉被弱引用关联的对象。（发现即被回收，缓存时使用较多）
- **虚引用(PhantomReference) :**一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来获得一个对象的实例。**为一个对象设置虚引用关联的唯一目的就是能在这个对象被收集器回收时收到一个系统通知**。（用于对象回收的跟踪）

#### 强引用——不回收

- 在Java程序中，最常见的引用类型是强引用**( 普通系统99%以上都是强引用)**，也就是我们最常见的普通对象引用，**也是默认的引用类型**。

- 当在Java语言中使用new操作符创建一个新的对象， 并将其赋值给一个变量的时候， 这个变量就成为指向该对象的一个强引用。

- **强引用的对象是可触及的，垃圾收集器就永远不会回收掉被引用的对象。**

- 对于一个普通的对象，如果没有其他的引用关系，只要超过了引用的作用域或者显式地将相应(强)引用赋值为null，就是可以当做垃圾被收集了，当然具体回收时机还是要看垃圾收集策略。

- 相对的，软引用、 弱引用和虚引用的对象是软可触及、弱可触及和虚可触及的，在一定条件下，都是可以被回收的。所以，**强引用是造成Java内存泄漏的主要原因之一。**

- 本例中的两个引用，都是强引用，强引用具备以下特点:
  - 强引用可以直接访问目标对象。
  - 强引用所指向的对象在任何时候都不会被系统回收，虚拟机宁愿抛出OOM异常，也不会回收强引用所指向对象。
  - 强引用可能导致内存泄漏。

##### 实例

```java
public class StrongReferenceTest {
    public static void main(String[] args) {
        StringBuffer str = new StringBuffer ("Hello,尚硅谷");
        StringBuffer str1 = str;

        str = null;
        System.gc();

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(str1);
    }
}
```

#### 软引用——内存不足即回收

- 软引用是用来描述一些还有用，但非必需的对象。**只被软引用关联着的对象，在系统将要发生内存溢出异常前，会把这些对象列进回收范围之中进行第二次回收**，如果这次回收还没有足够的内存，才会抛出内存溢出异常。

- 软引用通常用来实现内存敏感的缓存。比如：**高速缓存**就有用到软引用。如果还有空闲内存，就可以暂时保留缓存，当内存不足时清理掉，这样就保证了使用缓存的同时，不会耗尽内存。

- 垃圾回收器在某个时刻决定回收软可达的对象的时候，会清理软引用，并可选地把引用存放到一个引用队列(Reference Queue )

- 类似弱引用，只不过Java虚拟机会尽量让软引用的存活时间长一些，迫不得已才清理。

##### 实例

在JDK 1. 2版之后提供了java. lang. ref. SoftReference类来实现软引用。

![image-20230118145106830](/../JVM.assets/image-20230118145106830.png)

```java
public class SoftReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //创建对象，建立软引用
		//SoftReference<User> userSoftRef = new SoftReference<User>(new User(1, "songhk"));
        
        //上面的一行代码，等价于如下的三行代码
        User u1 = new User(1,"songhk");
        SoftReference<User> userSoftRef = new SoftReference<User>(u1);
        u1 = null;//取消强引用


        //从软引用中重新获得强引用对象
        System.out.println(userSoftRef.get());

        System.gc();
        System.out.println("After GC:");
        //垃圾回收之后获得软引用中的对象
        System.out.println(userSoftRef.get());//由于堆空间内存足够，所有不会回收软引用的可达对象。
        
        try {
            //让系统认为内存资源紧张、不够
			//byte[] b = new byte[1024 * 1024 * 7];
            byte[] b = new byte[1024 * 7168 - 635 * 1024];
        } catch (Throwable e) {
            e.printStackTrace();
        } finally {
            //再次从软引用中获取数据
            System.out.println(userSoftRef.get());//在报OOM之前，垃圾回收器会回收软引用的可达对象。
        }
    }
}
```

![image-20230118145123629](/../JVM.assets/image-20230118145123629.png)

#### 弱引用——发现即回收

- 弱引用也是用来描述那些非必需对象，**只被弱引用关联的对象只能生存到下一次垃圾收集发生为止**。在系统GC时，只要发现弱引用，不管系统堆空间使用是否充足，都会回收掉只被弱引用关联的对象。

- 但是，由于垃圾回收器的线程通常优先级很低，因此，并不一定能很快地发现持有弱引用的对象。在这种情况下，**弱引用对象可以存在较长的时间**。

- 弱引用和软引用一样，在构造弱引用时，也可以指定一个引用队列，当弱引用对象被回收时，就会加入指定的引用队列，通过这个队列可以跟踪对象的回收情况。

- **软引用、弱引用都非常适合来保存那些可有可无的缓存数据**。如果这么做，当系统内存不足时，这些缓存数据会被回收，不会导致内存溢出。而当内存资源充足时，这些缓存数据又可以存在相当长的时间，从而起到加速系统的作用。

- 在JDK 1. 2版之后提供了java. lang. ref . WeakReference类来实现弱引用。
- ![image-20230118145654330](/../JVM.assets/image-20230118145654330.png)
- **弱引用对象与软引用对象的最大不同**就在于，当GC在进行回收时，需要通过算法检查是否回收软引用对象，而对于弱引用对象，GC总是进行回收。**弱引用对象更容易、更快被GC回收**。
- 面试题：你开发中使用过WeakHashMap吗？

##### 实例

```java
public class WeakReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //构造了弱引用
        WeakReference<User> userWeakRef = new WeakReference<User>(new User(1, "songhk"));
        //从弱引用中重新获取对象
        System.out.println(userWeakRef.get());

        System.gc();
        // 不管当前内存空间足够与否，都会回收它的内存
        System.out.println("After GC:");
        //重新尝试从弱引用中获取对象
        System.out.println(userWeakRef.get());
    }
}
```

![image-20230118145704333](/../JVM.assets/image-20230118145704333.png)

#### 虚引用——对象回收跟踪

- 也称为“幽灵引用”或者“幻影引用”，是所有引用类型中最弱的一个。

- 一个对象是否有虚引用的存在，完全不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它和没有引用几乎是一样的，随时都可能被垃圾回收器回收。

- 它不能单独使用，也无法通过虚引用来获取被引用的对象。当试图通过虛引用的get()方法取得对象时，总是null。

- **为一个对象设置虚引用关联的唯一目的在于跟踪垃圾回收过程。比如:能在这个对象被收集器回收时收到一个系统通知。**

- 虚引用必须和引用队列一起使用。**虚引用在创建时必须提供一个引用队列作为参数**。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象后，将这个虚引用加入引用队列，以通知应用程序对象的回收情况。

- 由于虛引用可以跟踪对象的回收时间，因此，也可以将一些资源释放操作放置在虚引用中执行和记录。

- 在JDK 1.2版之后提供了PhantomReference类来实现虚引用。
- ![image-20230118145712712](/../JVM.assets/image-20230118145712712.png)

##### 实例

```java
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列

    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
        Thread t = new CheckRefQueue();
        t.setDaemon(true);//设置为守护线程：当程序中没有非守护线程时，守护线程也就执行结束。
        t.start();

        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);

        try {
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());

            //将强引用去除
            obj = null;
            //第一次进行GC,由于对象可复活，GC无法回收该对象
            System.gc();
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
            System.out.println("第 2 次 gc");
            obj = null;
            System.gc(); //一旦将obj对象回收，就会将此虚引用存放到引用队列中。
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![image-20230118145820447](/../JVM.assets/image-20230118145820447.png)

#### 终结器引用（了解）

- 它用以实现对象的finalize ()方法，也可以称为终结器引用。

- 无需手动编码，其内部配合引用队列使用。

- 在GC时，终结器引用入队。由Finalizer线 程通过终结器引用找到被引用对象并调用它的finalize()方法，第二次GC时才能回收被引用对象。

## 垃圾回收器

### GC分类与性能指标

#### 概述

- 垃圾收集器没有在规范中进行过多的规定，可以由不同的厂商、不同版本的JVM来实现。

- 由于JDK的版本处于高速迭代过程中，因此Java发展至今已经衍生了众多的GC版本。

- 从不同角度分析垃圾收集器，可以将GC分为不同的类型。

#### 垃圾回收器的分类

##### 按**线程数**分

按**线程数**分，可以分为串行垃圾回收器和并行垃圾回收器

![image-20230118151203764](/../JVM.assets/image-20230118151203764.png)

- 串行回收指的是在同一时间段内只允许有一个CPU用于执行垃圾回收操作，此时工作线程被暂停，直至垃圾收集工作结束。
  - 在诸如单CPU处理器或者较小的应用内存等硬件平台不是特别优越的场合，串行回收器的性能表现可以超过并行回收器和并发回收器。所以，**串行回收默认被应用在客户端的Client模式下的JVM中。**
  - 在并发能力比较强的CPU上，并行回收器产生的停顿时间要短于串行回收器。

- 和串行回收相反，并行收集可以运用多个CPU同时执行垃圾回收，因此提升了应用的吞吐量，不过并行回收仍然与串行回收一样，采用独占式，使用了“ Stop-the-world”机制。

##### 工作模式

按照**工作模式**分，可以分为并发式垃圾回收器和独占式垃圾回收器。

![image-20230118151255686](/../JVM.assets/image-20230118151255686.png)

- 独占式垃圾回收器(Stop the world)一旦运行，就停止应用程序中的所有用户线程，直到垃圾回收过程完全结束。
- 并发式垃圾回收器与应用程序线程交替工作，以尽可能减少应用程序的停顿时间。

##### 碎片处理方式

按**碎片处理方式**分，可分为压缩式垃圾回收器和非压缩式垃圾回收器。

- 压缩式垃圾回收器会在回收完成后，对存活对象进行压缩整理，消除回收后的碎片。
  - 再分配对象空间使用：指针碰撞
- 非压缩式的垃圾回收器不进行这步操作。
  - 再分配对象空间使用：空闲列表

##### 工作的内存区间

按**工作的内存区间**分，又可分为年轻代垃圾回收器和老年代垃圾回收器。

#### 评估GC的性能指标

- **吞吐量：运行用户代码的时间占总运行时间的比例**
  - (总运行时间:程序的运行时间+内存回收的时间)

- 垃圾收集开销：吞吐量的补数，垃圾收集所用时间与总运行时间的比例。

- **暂停时间：执行垃圾收集时，程序的工作线程被暂停的时间。**（STW）

- 收集频率：相对于应用程序的执行，收集操作发生的频率。

- **内存占用： Java堆区所占的内存大小。**

- 快速：一个对象从诞生到被回收所经历的时间。
- 这三者共同构成一个“不可能三角”。三者总体的表现会随着技术进步而越来越好。一款优秀的收集器通常最多同时满足其中的两项。
- 这三项里，暂停时间的重要性日益凸显。因为随着硬件发展，内存占用多些越来越能容忍，硬件性能的提升也有助于降低收集器运行时对应用程序的影响，即提高了吞吐量。而内存的扩大，对延迟反而带来负面效果。
- 简单来说，主要抓住两点:
  - 吞吐量
  - 暂停时间

#### 吞吐量

- 吞吐量就是CPU用于运行用户代码的时间与CPU总消耗时间的比值，即吞吐量=运行用户代码时间/ (运行用户代码时间+垃圾收集时间)。
  - 比如：虚拟机总共运行了100分钟，其中垃圾收集花掉1分钟，那吞吐量就是99%。

- 这种情况下，应用程序能容忍较高的暂停时间，因此，高吞吐量的应用程序有更长的时间基准，快速响应是不必考虑的。

- 吞吐量优先，意味着在单位时间内，STW的时间最短: 0.2 + 0.2 = 0.4
- ![image-20230118152003168](/../JVM.assets/image-20230118152003168.png)

#### 暂停时间

- “暂停时间”是指一个时间段内应用程序线程暂停，让GC线程执行的状态
  - 例如，GC期间100毫秒的暂停时间意味着在这100毫秒期间内没有应用程序线程是活动的。
- 暂停时间优先，意味着尽可能让单次STW的时间最短:0.1+0.1+0.1+0.1+0.1=0.5
- ![image-20230118152021826](/../JVM.assets/image-20230118152021826.png)

#### 比较

- 高吞吐量较好因为这会让应用程序的最终用户感觉只有应用程序线程在做“生产性”工作。直觉上，吞吐量越高程序运行越快。

- 低暂停时间(低延迟)较好因为从最终用户的角度来看不管是GC还是其他原因导致一个应用被挂起始终是不好的。这取决于应用程序的类型，**有时候甚至短暂的200毫秒暂停都可能打断终端用户体验**。因此，具有低的较大暂停时间是非常重要的，特别是对于一个**交互式应用程序**。

- 不幸的是”高吞吐量”和”低暂停时间”是一对相互竞争的目标(矛盾)。
  - 因为如果选择以吞吐量优先，那么**必然需要降低内存回收的执行频率**，但是这样会导致GC需要更长的暂停时间来执行内存回收。
  - 相反的，如果选择以低延迟优先为原则，那么为了降低每次执行内存回收时的暂停时间，也**只能频繁地执行内存回收**，但这又引起了年轻代内存的缩减和导致程序吞吐量的下降。
- 在设计(或使用) GC算法时，我们必须确定我们的目标:一个GC算法只可能针对两个目标之一(即只专注于较大吞吐量或最小暂停时间)，或尝试找到一个二者的折衷。
- 现在标准：**在最大吞吐量优先的情况下，降低停顿时间**。

### 不同的垃圾回收器概述

垃圾回收机制是java的招牌能力，极大地提升了开发效率。这当然也是面试的热点。

**那么，java常见的垃圾回收器有哪些？**

#### 发展史

有了虚拟机，就一定需要收集垃圾的机制，这就是Garbage Collection, 对应的产品我们称为Garbage Collector。 

- 1999年随JDK1.3.1-起来的是串行方式的Serial GC，它是第一款GC。ParNew垃圾收集器是Serial收集器的多线程版本
- 2002年2月26日，Parallel GC和Concurrent Mark Sweep GC跟随JDK1 .4.2一起发布
- Parallel GC在JDK6之后成为HotSpot默认GC。
- 2012年，在JDK1. 7u4版本中，G1可用。
- 2017年，JDK9中G1变成默认的垃圾收集器，以替代CMS。
- 2018年3月，JDK10中G1垃圾回收器的并行完整垃圾回收，实现并行性来改善最坏情况下的延迟。

***

- 2018年9月，JDK11发布。引入Epsilon垃圾回收器，又被称为"No-0p (无操作) "回收器。同时，引入**ZGC**：可伸缩的低延迟垃圾回收器(Experimental)。
- 2019年3月，JDK12发布。增强G1，自动返回未用堆内存给操作系统。同时，引入Shenandoah GC：低停顿时间的GC (Experimental)。
- 2019年9月，JDK13发布。增强ZGC，自动返回未用堆内存给操作系统。
- 2020年3月，JDK14发布。删除CMS垃圾回收器。扩展ZGC在macOS和Windows上的应用。

#### 7款经典的垃圾收集器

- 串行回收器: Serial、 Serial old
- 并行回收器: ParNew、 Parallel Scavenge、 Parallel Old
- 并发回收器: CMS、 G1

![image-20230118152515562](/../JVM.assets/image-20230118152515562.png)

#### 垃圾回收器的组合关系

![image-20230118152529467](/../JVM.assets/image-20230118152529467.png)

![image-20230118152551866](/../JVM.assets/image-20230118152551866.png)

- 两个收集器间有连线，表明它们可以搭配使用:
  - Serial/Serial 01d、Serial/CMS、 ParNew/Serial 01d、ParNew/CMS、Parallel Scavenge/Serial old、Paral1el Scavenge/Paral1e1 01d、G1;

- 其中Serial old作为CMS出现"Concurrent Mode Failure"失败的后备预案。

- (红色虚线)由于维护和兼容性测试的成本，在JDK 8时将Serial+CMS、ParNew+Serial old这两个组合声明为废弃(/../JEP 173) ，并在JDK 9中完全取消了这些组合的支持(/../JEP214)，即:移除。

- (绿色虚线)JDK 14中:弃用Paralle1 Scavenge和Serialold GC组合 (/../JEP366 )

- (青色虚线)JDK 14中: 删除CMS垃圾回收器 (/../JEP 363)

#### 垃圾回收器的选择

- **为什么要有很多收集器，一个不够吗？**因为Java的使用场景很多，移动端，服务器等。所以就需要针对不同的场景，提供不同的垃圾收集器，提高垃圾收集的性能。
- 虽然我们会对各个收集器进行比较，但并非为了挑选一个最好的收集器出来。没有一种放之四海皆准、任何场景下都适用的完美收集器存在，更加没有万能的收集器。所以**我们选择的只是对具体应用最合适的收集**

#### 指令

- **-xx: +PrintCommandLineFlags：**查看命令行相关参数(包含使用的垃圾收集器)
- 使用命令行指令: **jinfo -flag 相关垃圾回收器参数进程ID**
- ![image-20230118152702358](/../JVM.assets/image-20230118152702358.png)
- ![image-20230118152713583](/../JVM.assets/image-20230118152713583.png)

### Serial回收器：串行回收

#### 概述

![image-20230118160906875](/../JVM.assets/image-20230118160906875.png)

- Serial收集器是最基本、历史最悠久的垃圾收集器了。JDK1.3之前回收新生代唯一的选择。

- Serial收集器作为HotSpot中Client模式下的默认新生代垃圾收集器。

- **Serial 收集器采用复制算法、串行回收和"Stop-the-World"机制的方式执行内存回收。**

- 除了年轻代之外，Serial收集器还提供用于执行老年代垃圾收集的Serial old收集器。**Serial old收集器同样也采用了串行回收和"Stop the World"机制， 只不过内存回收算法使用的是标记-压缩算法。**
  - Serial old是运行在Client模式下默认的老年代的垃圾回收器
  - Serial old在Server模式下主要有两个用途:
    - ①与新生代的Parallel Scavenge配合使用
    - ②作为老年代CMS收集器的后备垃圾收集方案

#### 图示

![image-20230118161012078](../JVM.assets/image-20230118161012078.png)

这个收集器是一个单线程的收集器，但它的“单线程"的意义并不仅仅说明**它只会使用一个CPU或一条收集线程去完成垃圾收集工作**，更重要的是在它进行垃圾收集时，**必须暂停其他所有的工作线程**，直到它收集结束(Stop The World) 。

#### 优势

- **简单而高效**(与其他收集器的单线程比)，对于限定单个CPU的环境来说，Serial收集器由于没有线程交互的开销，专心做垃圾收集自然可以获得最高的单线程收集效率。
  - 运行在Client模式下的虚拟机是个不错的选择。
- 在用户的桌面应用场景中，可用内存一般不大(几十MB至一-两百MB) ,可以在较短时间内完成垃圾收集(几十ms至一百多ms) , 只要不频繁发生，使用串行回收器是可以接受的。
- 在HotSpot虚拟机中，使用-XX:+UseSerialGC参数可以指定年轻代和老年代都使用串行收集器。
  - 等价于新生代用Serial GC， 且老年代用Serial old GC

#### 实例

```java
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

开启前

![image-20230118161120668](/../JVM.assets/image-20230118161120668.png)

开启后

![image-20230118161127618](/../JVM.assets/image-20230118161127618.png)

#### 总结

这种垃圾收集器大家了解，现在已经不用串行的了。而且在**限定单核cpu**才可以用。现在都不是单核的了。

对于交互较强的应用而言，这种垃圾收集器是不能接受的。一般在Java web应用程序中是不会采用串行垃圾收集器的。

### ParNew回收器：并行回收

#### 概述

![image-20230118161426749](/../JVM.assets/image-20230118161426749.png)

- 如果说Serial GC是年轻代中的单线程垃圾收集器，那么ParNew收集器则是Serial收集器的多线程版本。
  - Par是Parallel的缩写，New:只能处理的是新生代

- ParNew收集器除了采用**并行回收**的方式执行内存回收外，两款垃圾收集器之间几乎没有任何区别。ParNew收集器在年轻代中同样也是**采用复制算法、"Stop-the-World"**机制。

- ParNew是很多JVM运行在Server模式下新生代的默认垃圾收集器。

#### 图示

![image-20230118161438112](/../JVM.assets/image-20230118161438112.png)

- 对于新生代，回收次数频繁，使用并行方式高效。

- 对于老年代，回收次数少，使用串行方式节省资源。(CPU并 行需要切换线程，串行可以省去切换线程的资源)

- 由于ParNew收集器是基于并行回收，那么是否可以断定ParNew收集器的回收效率在任何场景下都会比Seria1收集器更高效?
  - ParNew 收集器运行在多CPU的环境下，由于可以充分利用多CPU、多核心等物理硬件资源优势，可以更快速地完成垃圾收集，提升程序的吞吐量。
  - 但是在**单个CPU的环境下，ParNew收集器不比Serial收集器更高效**。虽然Serial收集器是基于串行回收，但是由于CPU不需要频繁地做任务切换，因此可以有效避免多线程交互过程中产生的一些额外开销。
- 因为除Serial外，目前只有ParNew GC能与CMS收集器配合工作

- 在程序中，开发人员可以通过选项"-XX: +UseParNewGC"手动指定使用ParNew收集器执行内存回收任务。它表示年轻代使用并行收集器，不影响老年代。
- -XX:ParallelGCThreads限制线程数量，默认开启和CPU数据相同的线程数。

#### 实例

代码同上

![image-20230118161526942](/../JVM.assets/image-20230118161526942.png)

### Parallel回收器：吞吐量优先

#### 概述

![image-20230118161537544](/../JVM.assets/image-20230118161537544.png)

- HotSpot的年轻代中除了拥有ParNew收集器是基于并行回收的以外，Parallel Scavenge收集器同样也采用了**复制算法、并行回收和"Stop the World" 机制**。

- 那么Parallel收集器的出现是否多此一举？
  - 和ParNew收集器不同，Parallel Scavenge收集器的目标则是达到一个**可控制的吞吐量**(Throughput)，它也被称为吞吐量优先的垃圾收集器。
  - 自适应调节策略也是Parallel Scavenge 与ParNew一个重要区别。

- 高吞吐量则可以高效率地利用CPU时间，尽快完成程序的运算任务，主要**适合在后台运算而不需要太多交互的任务**。因此，常见在服务器环境中使用。**例如，那些执行批量处理、订单处理、工资支付、科学计算的应用程序。**
- Parallel收集器在JDK1.6时提供了用于执行老年代垃圾收集的**Parallel old**收集器，用来代替老年代的Serial old收集器。
- Parallel old收集器采用了**标记-压缩算法**，但同样也是基于**并行回收“Stop-the-World"机制。**

#### 图示

![image-20230118161808266](/../JVM.assets/image-20230118161808266.png)

- 在程序吞吐量优先的应用场景中，Parallel 收集器和Parallel old收集器的组合，在Server模式下的内存回收性能很不错。
- 在Java8中，默认是此垃圾收集器。

#### 实例

jdk8中Parallel是默认的GC，JDK9中改为G1

![image-20230118161901370](/../JVM.assets/image-20230118161901370.png)

#### 参数配置

- **-XX: +UseParallelGC**手动指定 年轻代使用Parallel并行收集器执行内存回收任务。
- **-XX: +UseParalleloldGC** 手动指定老年代都是使用并行回收收集器。
  - 分别适用于新生代和老年代。默认jdk8是开启的。
  - 上面两个参数，默认开启一个，另一个也会被开启。 (互相激活)
- **-XX: ParallelGCThreads**设置年轻代并行收集器的线程数。一般地，最好与CPU数量相等，以避免过多的线程数影响垃圾收集性能。
  - 在默认情况下，当CPU数量小于8个，ParallelGCThreads 的值等于CPU数量。
  - 当CPU数量大于8个，ParallelGCThreads 的值等于3+ [5*CPU_Count] /8]。
- **-XX:MaxGCPauseMillis**设置垃圾收集器最大停顿时间(即STW的时间)。单位是毫秒。
  - 为了尽可能地把停顿时间控制在MaxGCPauseMills以内，收集器在工作时会调整Java堆大小或者其他一些参数。
  - 对于用户来讲，停顿时间越短体验越好。但是在服务器端，我们注重高并发，整体的吞吐量。所以服务器端适合Parallel,进行控制。
  - **该参数使用需谨慎。**
- **-XX: GCTimeRatio**垃圾收集时间占总时间的比例(= 1 / (N + 1))。用于衡量吞吐量的大小。
  - 取值范围(0,100)。默认值99，也就是垃圾回收时间不超过1%。
  - 与前一个-XX :MaxGCPauseMillis参数有一定矛盾性。暂停时间越长，Radio参 数就容易超过设定的比例。
- **-XX: +UseAdaptiveSizePolicy**设置Parallel Scavenge收集 器具有**自适应调节策略**
  - 在这种模式下，年轻代的大小、Eden和Survivor的比例、晋升老年代的对象年龄等参数会被自动调整，已达到在堆大小、吞吐量和停顿时间之间的平衡点。
  - 在手动调优比较困难的场合，可以直接使用这种自适应的方式，仅指定虚拟机的最大堆、目标的吞吐量( GCT imeRatio)和停顿时间( MaxGCPauseMills)，让虚拟机自己完成调优工作。

### CMS回收器：低延时

#### 概述

![image-20230118161919104](/../JVM.assets/image-20230118161919104.png)

- 在JDK 1.5时期，HotSpot推出了一款在强交互应用中几乎可认为有划时代意义的垃圾收集器: CMS (Concurrent -Mark- Sweep)收集器，**这款收集器是HotSpot虚拟机中第一款真正意义 上的并发收集器，它第一次实现了让垃圾收集线程与用户线程同时工作。**

- CMS收集器的关注点是尽可能缩短垃圾收集时用户线程的停顿时间。停顿时间越短(低延迟)就越适合与用户交互的程序，良好的响应速度能提升用户体验。
  - **目前很大一部分的Java应用集中在互联网站或者B/S系统的服务端上，这类应用尤其重视服务的响应速度，希望系统停顿时间最短**，以给用户带来较好的体验。CMs收集器就非常符合这类应用的需求。

- CMS的垃圾收集算法采用**标记-清除**算法，并且也会"Stop-the-world"
- 不幸的是，CMS作为老年代的收集器，却无法与JDK 1.4.0 中已经存在的新生代收集器Parallel Scavenge配合工作，所以在JDK 1.5中使用CMS来收集老年代的时候，新生代只能选择ParNew或者Serial收集器中的一个。
- 在G1出现之前，CMS使用还是非常广泛的。一直到今天，仍然有很多系统使用CMS GC。

#### 图示——工作原理

![image-20230118162632770](/../JVM.assets/image-20230118162632770.png)

CMS整个过程比之前的收集器要复杂，整个过程分为4个主要阶段，即初始标记阶段、并发标记阶段、重新标记阶段和并发清除阶段。

- 初始标记( Initial-Mark)阶段：在这个阶段中，程序中所有的工作线程都将会因为Stop-the -World"机制而出现短暂的暂停，这个阶段的主要任务**仅仅只是标记出GC Roots能直接关联到的对象**。一旦标记完成之后就会恢复之前被暂停的所有应用线程。由于直接关联对象比较小，所以这里的**速度非常快**。
- 并发标记(Concurrent-Mark) 阶段：从GC Roots的**直接关联对象开始遍历整个对象图的过程**，这个过程**耗时较长**但是**不需要停顿用户线程**，可以与垃圾收集线程一起并发运行。
- 重新标记(Remark) 阶段：由于在并发标记阶段中，程序的工作线程会和垃圾收集线程同时运行或者交叉运行，因此为了**修正并发标记期间，因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录**，这个阶段的STW停顿时间通常会比初始标记阶段稍长一些，但也远比并发标记阶段的时间短。
- 并发清除(Concurrent -Sweep)阶段：此阶段**清理删除掉标记阶段判断的已经死亡的对象，释放内存空间**。由于不需要移动存活对象，所以这个阶段也是可以与用户线程同时并发的

#### 特点

尽管CMS收集器采用的是并发回收(非独占式)，但是在**其初始化标记和再次标记这两个阶段中仍然需要执行“Stop-the-World"机制暂停程序中的工作线程**，不过暂停时间并不会太长，因此可以说明目前所有的垃圾收集器都做不到完全不需要“Stop-the-World”，只是尽可能地缩短暂停时间。

**由于最耗费时间的并发标记与并发清除阶段都不需要暂停工作，所以整体的回收是低停顿的。**

另外，由于在垃圾收集阶段用户线程没有中断，**所以在CMS回收过程中，还应该确保应用程序用户线程有足够的内存可用**。因此，CMS收集器不能像其他收集器那样等到老年代几乎完全被填满了再进行收集，而是**当堆内存使用率达到某一阈值时，便开始进行回收**，以确保应用程序在CMS工作过程中依然有足够的空间支持应用程序运行。要是CMS运行期间预留的内存无法满足程序需要，就会出现一次“**Concurrent Mode Failure**”失败，这时虚拟机将启动后备预案：临时启用Serial old收集器来重新进行老年代的垃圾收集，这样停顿时间就很长了。

#### 优缺点

CMS收集器的垃圾收集算法采用的是**标记-清除算法**，这意味着每次执行完内存回收后，由于被执行内存回收的无用对象所占用的内存空间极有可能是不连续的一些内存块，不可避免地将**会产生一些内存碎片**。那么CMS在为新对象分配内存空间时，将无法使用指针碰撞(Bump the Pointer) 技术，而只能够选择空闲列表(Free List) 执行内存分配。

![image-20230118164022820](/../JVM.assets/image-20230118164022820.png)

**有人会觉得既然Mark Sweep会造成内存碎片，那么为什么不把算法换成Mark Compact呢?**

答案其实很简答，因为当并发清除的时候，用Compact整理内存的话， 原来的用户线程使用的内存还怎么用呢？**要保证用户线程能继续执行（不能改变所使用的的对象的地址，但是标记-压缩时对象的地址会被修改）**，前提的它运行的资源不受影响。Mark Compact更适合 “Stop the World ”这种场景下使用。

- **CMS的优点:**
  - 并发收集
  - 低延迟
- **CMS的弊端:**
  - **会产生内存碎片**，导致并发清除后，用户线程可用的空间不足。在无法分配大对象的情况下，不得不提前触发Full GC。
  - **CMS收集器对CPU资源非常敏感**。在并发阶段，它虽然不会导致用户停顿，但是会因为占用了一部分线程而导致应用程序变慢，总吞吐量会降低。
  - **CMS收集器无法处理浮动垃圾**。可能出现“Concurrent Mode Failure" 失败而导致另一次Full GC的产生。在并发标记阶段由于程序的工作线程和垃圾收集线程是同时运行或者交叉运行的，那么**在并发标记阶段如果产生新的垃圾对象，CMS将无法对这些垃圾对象进行标记，最终会导致这些新产生的垃圾对象没有被及时回收**，从而只能在下一次执行GC时释放这些之前未被回收的内存空间。

#### 参数设置

- **-XX: +UseConcMarkSweepGC**手动指定使用CMS收集器执行内存回收任务。
  - 开启该参数后会自动将-XX:+UseParNewGC打开。即: ParNew (Young区用) +CMS (01d区用) +Serial old的组合。
- **-XX:CMSlnitiatingOccupanyFraction**设置堆内存使用率的阈值，一旦达到该阈值，便开始进行回收。
  - JDK5及以前版本的默认值为68,即当老年代的空间使用率达到68%时，会执行一次CMS回收。**JDK6及 以上版本默认值为92%。**
  - 如果内存增长缓慢，则可以设置一个稍大的值，大的阈值可以有效降低CMS的触发频率，减少老年代回收的次数可以较为明显地改善应用程序性能。反之，如果应用程序内存使用率增长很快，则应该降低这个阈值，以避免频繁触发老年代串行收集器。因此**通过该选项便可以有效降低Full GC的执行次数**。
- **-XX: +UseCMSCompactAtFullCollection**用于指定在执行完Full GC后对内存空间进行压缩整理，以此避免内存碎片的产生。不过由于内存压缩整理过程无法并发执行，所带来的问题就是停顿时间变得更长了。
- **-XX: CMSFullGCsBeforeCompaction**设置在执行多少次Full GC后对内存空间进行压缩整理。
- **-XX:Paralle1CMSThreads** 设置CMS的线程数量。
  - CMS默认启动的线程数是(ParallelGCThreads+3)/4，ParallelGCThreads是年轻代并行收集器的线程数。当CPU 资源比较紧张时，受到CMS收集器线程的影响，应用程序的性能在垃圾回收阶段可能会非常糟糕。

#### 小结

HotSpot有这么多的垃圾回收器，那么如果有人问，Serial GC、Parallel GC、Concurrent Mark Sweep GC这三个GC有什么不同呢?

请记住以下口令:

**如果你想要最小化地使用内存和并行开销，请选Serial GC；**

**如果你想要最大化应用程序的吞吐量，请选Parallel GC；**

**如果你想要最小化GC的中断或停顿时间，请选CMS GC。**

#### JDK后续版本中CMS的变化

- JDK9新特性: CMS 被标记为Deprecate了(/../JEP291)
  - 如果对JDK 9及以上版本的HotSpot虚拟机使用参数-XX:+UseConcMarkSweepGC来开启CMS收集器的话，用户会收到一个警告信息，提示CMS未来将会被废弃。
- JDK14新特性: 删除CMS垃圾回收器(/../JEP363)
  - 移除了CMS垃圾收集器，如果在JDK14中使用-XX: +UseConcMarkSweepGC的话，JVM不会报错，只是给出一个warning信息，但是不会exit。JVM会自动回退以默认GC方式启动JVM。

![image-20230118164041152](/../JVM.assets/image-20230118164041152.png)

### G1回收器：区域划分代式

![image-20230118164053891](/../JVM.assets/image-20230118164053891.png)

#### 概述

**既然我们已经有了前面几个强大的GC，为什么还要发布Garbage Firlst (G1 )GC？**

- 原因就在于应用程序所应对的**业务越来越庞大、复杂，用户越来越多**，没有GC就不能保证应用程序正常进行，而经常造成STw的GC又跟不上实际的需求，所以才会不断地尝试对GC进行优化。G1 (Garbage-First)垃圾回收器是在Java7 update 4之后引入的一个新的垃圾回收器，是当今收集器技术发展的最前沿成果之一。

- 与此同时，为了适应现在**不断扩大的内存和不断增加的处理器数量**，进一步降低暂停时间(pause time)，同时兼顾良好的吞吐量。

- **官方给G1设定的目标是在延迟可控的情况下获得尽可能高的吞吐量，所以才担当起“全功能收集器”的重任与期望。**

**为什么名字叫做Garbage First (G1)呢?**

- 因为G1是一个并行回收器，它把堆内存分割为很多不相关的区域(Region) (物理上不连续的)。使用不同的Region来表示Eden、幸存者0区，幸存者1区，老年代等。

- Region

  ![image-20230118164509344](/../JVM.assets/image-20230118164509344.png)

- G1 GC有计划地避免在整个Java堆中进行全区域的垃圾收集。G1跟踪各个Region里面的垃圾堆积的价值大小(回收所获得的空间大小以及回收所需时间的经验值)，在后台维护一个优先列表，**每次根据允许的收集时间，优先回收价值最大的Region。**

- 由于这种方式的侧重点在于回收垃圾最大量的区间(Region) ，所以我们给G1一个名字：垃圾优先(Garbage First) 。

+++

G1 (Garbage- First)是一款面向服务端应用的垃圾收集器，**主要针对配备多核CPU及大容量内存的机器**，以极高概率满足GC停顿时间的同时，还兼具高吞吐量的性能特征。

在JDK1.7版本正式启用，移除了Experimental的标识，**是JDK9以后的默认垃圾回收器**，取代了CMS回收器以及Parallel + Parallel old组合。被Oracle官方称为“**全功能的垃圾收集器**”

与此同时，CMS已经在JDK 9中被标记为废弃(deprecated)。在jdk8中还不是默认的垃圾回收器，需要使用-XX: +UseG1GC来启用。

#### 特点

##### 优势

与其他GC收集器相比，G1使用了全新的分区算法，其特点如下所示:

- **并行与并发**
  - 并行性：G1在回收期间，可以有多个GC线程同时工作，有效利用多核计算能力。此时用户线程STW。
  - 并发性：G1拥有与应用程序交替执行的能力，部分工作可以和应用程序同时执行，因此，一般来说，不会在整个回收阶段发生完全阻塞应用程序的情况。
- **分代收集**
  - 从分代上看，**G1依然属于分代型垃圾回收器**，它会区分年轻代和老年代，年轻代依然有Eden区和Survivor区。但从堆的结构，上看，它不要求整个Eden区、年轻代或者老年代都是连续的，也不再坚持固定大小和固定数量。
  - 将**堆空间分为若干个区域(Region) ,这些区域中包含了逻辑上的年轻代和老年代**。
  - ![image-20230118165932976](/../JVM.assets/image-20230118165932976.png)
  - 和之前的各类回收器不同，它同时**兼顾年轻代和老年代**。对比其他回收器，或者工作在年轻代，或者工作在老年代;

- **空间整合**

  - CMS:“标记-清除”算法、内存碎片、若干次GC后进行一次碎片整理。
  - G1将内存划分为一个个的region。内存的回收是以region作为基本单位的。**Region之间是复制算法（E->S->O）**，但整体上实际可看作是**标记-压缩(Mark-Compact )算法(整体)**，两种算法都可以避免内存碎片。这种特性有利于程序长时间运行，分配大对象时不会因为无法找到连续内存空间而提前触发下一次GC。尤其是**当Java堆非常大的时候，G1的优势更加明显。**

- **可预测的停顿时间模型**(即:软实时soft real-time )

  这是G1相对于CMS的另一大优势，G1除了追求低停顿外，还能建立可预测的停顿时间模型，能让使用者明确指定在一个长度为M毫秒的时间片段内，消耗在垃圾收集上的时间不得超过N毫秒。

  - 由于分区的原因，G1可以只选取部分区域进行内存回收，这样缩小了回收的范围，因此对于全局停顿情况的发生也能得到较好的控制。
  - G1跟踪各个Region里面的垃圾堆积的价值大小(回收所获得的空间大小以及回收所需时间的经验值)，在后台维护一个优先列表，**每次根据允许的收集时间，优先回收价值最大的Region**。保证了G1收集器在有限的时间内可以**获取尽可能高的收集效率**。
  - 相比于CMS GC，G1未必能做到CMS在最好情况下的延时停顿，但是最差情况要好很多。

##### 缺点

- 相较于CMS，G1还不具备全方位、压倒性优势。比如在用户程序运行过程中，G1无论是为了垃圾收集产生的内存占用(Footprint) 还是程序运行时的额外执行负载(Overload) 都要比CMS要高。

- 从经验上来说，在小内存应用上CMS的表现大概率会优于G1，而G1在大内存应用上则发挥其优势。平衡点在6- 8GB之间。

#### 参数设置

- **-XX: +UseG1GC**    手动指定使用G1收集器执行内存回收任务。
- **-XX: G1HeapRegionSize**    设置每个Region的大小。值是2的幂，范围是1MB到32MB之间，目标是根据最小的Java堆大小划分出约2048个区域。默认是堆内存的1 /2000。

- **-XX: MaxGCPauseMillis**    设置期望达到的最大GC停顿时间指标(/../JVM会尽力实现，但不保证达到)。默认值是200ms
- -XX: ParallelGCThread    设置STW时GC线程数的值。最多设置为8
- -XX: ConcGCThreads    设置并发标记的线程数。将n设置为并行垃圾回收线程数(ParallelGCThreads)的1/4左右。
- -XX: InitiatingHeapOccupancyPercent    设置触发并发GC周期的Java堆占用率阈值。超过此值，就触发GC。默认值是45。

#### 操作步骤

G1的设计原则就是简化JVM性能调优，开发人员只需要简单的三步即可完成调优:

**第一步：开启G1垃圾收集器**

**第二步：设置堆的最大内存**

**第三步：设置最大的停顿时间**

G1中提供了三种垃圾回收模式: YoungGC、 Mixed GC和Full GC，在不同的条件下被触发。

#### 适用场景

- 面向服务端应用，针对具有大内存、多处理器的机器。(在普通大小的堆里表现并不惊喜)
- 最主要的应用是需要低GC延迟，并具有大堆的应用程序提供解决方案;

- 如：在堆大小约6GB或更大时，可预测的暂停时间可以低于0.5秒; (G1通过 每次只清理一部分而不是全部的Region的增量式清理来保证每次GC停顿时间不会过长)。

- 用来替换掉JDK1.5中的CMS收集器;
  - 在下面的情况时，使用G1可能比CMS好:
    - ①超过50%的Java堆被活动数据占用;
    - ②对象分配频率或年代提升频率变化很大;
    - ③GC停顿时间过长(长于0.5至1秒)。

- HotSpot垃圾收集器里，除了G1以外，其他的垃圾收集器使用内置的JVM线程执行GC的多线程操作，而G1 GC可以采用应用线程承担后台运行的GC工作，即当JVM的GC线程处理速度慢时，系统会调用应用程序线程帮助加速垃圾回收过程。

#### Regin

##### 概述

使用G1收集器时，它将整个Java堆划分成约2048个大小相同的独立Region块，每个Region块 大小根据堆空间的实际大小而定，整体被控制在1MB到32MB之间，且为2的N次幂， 即1MB, 2MB, 4MB, 8MB, 16MB, 32MB。可以通过-XX:G1HeapRegionSize设定。**所有的Region大小相同，且在JVM生命周期内不会被改变。**

虽然还保留有新生代和老年代的概念，但新生代和老年代不再是物理隔离的了，它们都是一部分Region (不需要连续)的集合。**通过Region的动态分配方式实现逻辑上的连续。（物理上不在连续，逻辑上依旧连续着）**

##### 图示

![image-20230118170210192](/../JVM.assets/image-20230118170210192.png)

- 一个region 有可能属于Eden， Survivor 或者old/Tenured 内存区域。但是一个region只可能属于一个角色。图中的E表示该region属于Eden内存区域，s表示属于Survivor内存区域，o表示属于old内存区域。图中空白的表示未使用的内存空间。

- G1垃圾收集器还增加了一种新的内存区域，叫做Humongous内存区域，如图中的H块。主要用于存储大对象，**如果超过1.5个region，就放到H**。

##### 设置H的原因:

对于堆中的大对象，默认直接会被分配到老年代，但是如果它是一个短期存在的大对象就会对垃圾收集器造成负面影响。为了解决这个问题，G1划分了一个Humongous区，它用来专门存放大对象。**如果一个H区装不下一个大对象，那么G1会寻找连续的H区来存储**。为了能找到连续的H区，有时候不得不启动Full GC。**G1的大多数行为都把H区作为老年代的一部分来看待。**

![image-20230118170815390](/../JVM.assets/image-20230118170815390.png)

- Bump - the - pointer，即指针碰撞
- TLAB

#### 回收过程

##### 概述

G1 GC的垃圾回收过程主要包括如下三个环节:

- 年轻代GC (Young GC)

- 老年代并发标记过程(Concurrent Marking )

- 混合回收(Mixed GC)

- (如果需要，单线程、独占式、高强度的Full GC还是继续存在的。它
  针对GC的评估失败提供了一种失败保护机制，即强力回收。)

##### 图示

![image-20230118170957868](/../JVM.assets/image-20230118170957868.png)

（老年代并发标记过程中也会存在年轻代GC）

**顺时针，young gc -> young gc + concurrent mark-> Mixed GC顺序，进行垃圾回收。**

##### 过程

应用程序分配内存，**当年轻代的Eden区用尽时开始年轻代回收过程**； G1的年轻代收集阶段是一个**并行的独占式**收集器。在年轻代回收期，G1 GC暂停所有应用程序线程，启动多线程执行年轻代回收。然后**从年轻代区间移动存活对象到Survivor区间或者老年区间，也有可能是两个区间都会涉及**。

当堆内存使用达到一定值(默认45%)时，开始老年代并发标记过程。

标记完成马上开始混合回收过程。对于一个混合回收期，G1 GC从老年区间移动存活对象到空闲区间，这些空闲区间也就成为了老年代的一部分。和年轻代不同，老年代的G1回收器和其他GC不同，G1的老年代回收器不需要整个老年代被回收，一次只需要扫描/回收一小部分老年代的Region就可以了。同时，这个老年代Region是和年轻代一起 被回收的。

举个例子:一个Web服务器，Java进程最大堆内存为4G，每分钟响应1500个请求，每45秒钟会新分配大约2G的内存。G1会每45秒钟进行一次年轻代回收，每31 个小时整个堆的使用率会达到45%，会开始老年代并发标记过程，标记完成后开始四到五次的混合回收。

##### Rememered Set

###### 概述

- 一个对象被不同区域引用的问题
- 一个Region不可能是孤立的，一个Region中的对象可能被其他任意Region中对象引用，判断对象存活时，**是否需要扫描整个Java堆才能保证准确？**
- 在其他的分代收集器，也存在这样的问题(而G1更突出)
- **回收新生代也不得不同时扫描老年代？**
- 这样的话会降低MinorGC的效率；

+++

- **解决方法：**
  - 无论G1还是其他分代收集器，JVM都是使用RememberedSet来避免全局扫描；
  - 每个Region都有一个对应的Remembered Set；
  - 每次Reference类型数据写操作时，都会产生一个Write Barrier暂时中断操作；
  - 然后检查将要写入的引用指向的对象是否和该Reference类型数据在不同的Region (其他收集器:检查老年代对象是否引用了新生代对象)；
  - 如果不同，通过CardTable把相关引用信息记录到引用指向对象的所在Region对应的Remembered Set中；
  - 当进行垃圾收集时，在GC根节点的枚举范围加入Remembered Set；就可以保证不进行全局扫描，也不会有遗漏。

###### 图示

![image-20230118172512559](/../JVM.assets/image-20230118172512559.png)

1. Rememered Set记录的是incoming reference，为了在清理本区域的时候知道要扫描哪个region

2. G1每一个Region都会对应Remembered Set，它会记录这个Region清理时需要扫描哪几个Region，记录引用分区内对象的card索引。之所以这么做的原因是因为G1是分了Region，如果只使用Card Table，那么就可能不能发挥Region的作用，甚至会变成FULL GC。所以当CT更新后，会异步的去更新RS。这样标记时，仅仅需要扫描对应分区的对应card中的对象是否可达即可，极大的提升了GC效率。

##### 过程一：Young GC

###### 概述

JVM启动时，G1先准备好Eden区，程序在运行过程中不断创建对象到Eden区，当Eden空间耗尽时，G1会启动一次年轻代垃圾回收过程。

**年轻代垃圾回收只会回收Eden区和Survivor区。**

YGC时，首先G1停止应用程序的执行(Stop-The-World) ，G1 创建回收集合(Collection Set)，回收集合是指需要被回收的内存分段的集合，年轻代回收过程的回收集合包含年轻代Eden区和Survivor区所有的内存分段。

###### 图示

![image-20230118174802094](/../JVM.assets/image-20230118174802094.png)

###### 过程

然后开始如下回收过程:

**第一阶段，扫描根。**

根是指static变量指向的对象，正在执行的方法调用链条上的局部变量等。根引用连同RSet记录的外部引用作为扫描存活对象的入口。

**第二阶段，更新RSet。**

处理dirty card queue( 见备注)中的card，更新RSet。此阶段完成后，**RSet可以准确的反映老年代对所在的内存分段中对象的引用。**

**第三阶段，处理RSet。**

识别被老年代对象指向的Eden中的对象，这些被指向的Eden中的对象被认为是存活的对象。

**第四阶段，复制对象。**

此阶段，对象树被遍历，Eden区内存段中存活的对象会被复制到Survivor区中空的内存分段Survivor区内存段中存活的对象如果年龄未达阈值，年龄会加1，达到阀值会被会被复制到old区中空的内存分段。如果Survivor空间不够，Eden空间的部分数据会直接晋升到老年代空间。

**第五阶段，处理引用。**

处理Soft，Weak，Phantom， Final， JNI Weak等引用。最终Eden空间的数据为空，GC停止工作，而目标内存中的对象都是连续存储的，没有碎片，所以复制过程可以达到内存整理的效果，减少碎片。

> **dirty card queue**
>
> 对于应用程序的引用赋值语句object.field=object , JVM会在之前和之后执行特殊的操作以在dirty card queue中入队一个保存了对象引用信息的card。在年轻代回收的时候, G1会对Dirty Card Queue中所有的card进行处理，以更新RSet，保证RSet实时准确的反映引用关系。
>
> **那为什么不在引用赋值语句处直接更新RSet呢？**
>
> 这是为了性能的需要, RSet的处理需要线程同步，开销会很大，使用队列性能会好很多。

##### 过程二：并发标记过程

1. **初始标记阶段:**标记从根节点直接可达的对象。这个阶段是STW的，并且会触发一次年轻代GC。
2. **根区域扫描(Root Region Scanning)** : G1 GC扫描Survivor区直接可达的老年代区域对象，并标记被引用的对象。这一过程必须在young GC之前完成。
3. **并发标记(Concurrent Marking):**在整个堆中进行并发标记(和应用程序并发执行)，此过程可能被young GC中断。在并发标记阶段，**若发现区域对象中的所有对象都是垃圾，那这个区域会被立即回收**。同时，并发标记过程中，会计算每个区域的对象活性(区域中存活对象的比例)。
4. **再次标记(Remark):**由于应用程序持续进行，需要修正上一次的标记结果。是STW的。G1中采用了比CMS更快的初始快照算法:snapshot-at-the-beginning (SATB)。
5. **独占清理(cleanup, STW):** 计算各个区域的存活对象和GC回收比例，并进行排序，识别可以混合回收的区域。为下阶段做铺垫。是STW的。
   - 这个阶段并不会实际上去做垃圾的收集。
6. **并发清理阶段:**识别并清理完全空闲的区域。

##### 过程三：混合回收

###### 概述

当越来越多的对象晋升到老年代old region时，为了避免堆内存被耗尽，虚拟机会触发一个混合的垃圾收集器，即Mixed. GC，该算法并不是一个old GC，除了回收整个Young Region, 还会回收一部分的old Region。 这里需要注意：**是一部分老年代，而不是全部老年代**(前面有标记)。可以选择哪些old Region进行收集，从而可以对垃圾回收的耗时时间进行控制。也要注意的是Mixed GC并不是Full GC。

###### 图示

![image-20230118180410682](/../JVM.assets/image-20230118180410682.png)

- 并发标记结束以后，老年代中百分百为垃圾的内存分段被回收了，部分为垃圾的内存分段被计算了出来。默认情况下，这些老年代的内存分段会分8次(可以通过-XX: G1MixedGCCountTarget设置)被回收。

- 混合回收的回收集(Collection Set)包括八分之一的老年代内存分段，Eden区内存分段，Survivor区内存分段。混合回收的算法和年轻代回收的算法完全一样，只是回收集多了老年代的内存分段。具体过程请参考上面的年轻代回收过程。

- 由于老年代中的内存分段默认分8次回收，G1会优先回收垃圾多的内存分段。**垃圾占内存分段比例越高的，越会被先回收**。并且有一个阈值会决定内存分段是否被回收，-XX:G1MixedGCLiveThresholdPercent，默认为65%，意思是垃圾占内存分段比例要达到65%才会被回收。如果垃圾占比太低，意味着存活的对象占比高，在复制的时候会花费更多的时间。

- 混合回收并不一定要进行8次。有一个阈值-XX: G1HeapWastePercent，默认值为10%，意思是允许整个堆内存中有10%的空间被浪费，意味着如果发现可以回收的垃圾占堆内存的比例低于10%，则不再进行混合回收。因为GC会花费很多的时间但是回收到的内存却很少。

##### 过程四：Full GC

G1的初衷就是要避免Full GC的出现。但是如果上述方式不能正常工作，G1会**停止应用程序的执行**(stop-The-World) ，使用**单线程**的内存回收算法进行垃圾回收，性能会非常差，应用程序停顿时间会很长。

要避免Full GC的发生，一旦发生需要进行调整。什么时候会发生Full GC呢？比如**堆内存太小**，当G1在复制存活对象的时候没有空的内存分段可用，则会回退到full gc，这种情况可以通过增大内存解决。

导致G1Full GC的原因可能有两个:

1. Evacuation的时候没有足够的to-space来存放晋升的对象; 
2. 并发处理过程完成之前空间耗尽。

#### 补充

从Oracle官方透露出来的信息可获知，回收阶段( Evacuation)其实本也有想过设计成与用户程序一起并发执行，但这件事情做起来比较复杂，考虑到G1只是回收一部分Region，停顿时间是用户可控制的，所以并不迫切去实现，而**选择把这个特性放到了G1之后出现的低延迟垃圾收集器(即ZGC)中**。另外，还考虑到G1不是仅仅面向低延迟，停顿用户线程能够最大幅度提高垃圾收集效率，为了保证吞吐量所以才选择了完全暂停用户线程的实现方案。

##### 优化

- 年轻代大小
  - 避免使用-Xmn或-XX:NewRatio等相关选项显式设置年轻代大小
  - 固定年轻代的大小会覆盖暂停时间目标
- 暂停时间目标不要太过严苛
  - G1 GC的吞吐量目标是90%的应用程序时间和10%的垃圾回收时间
  - 评估G1 GC的吞吐量时，暂停时间目标不要太严苛。目标太过严苛表示你愿意承受更多的垃圾回收开销，而这些会直接影响到吞吐量。

### 垃圾回收器总结

##### 7大垃圾回收器的比较

截止JDK1.8，一共有7款不同的垃圾收集器。每一款不同的垃圾收集器都有不同的特点，在具体使用的时候，需要根据具体的情况选用不同的垃圾收集器。

| 垃圾收集器   | 作用               | 分类           | 作用位置             | 使用算法                | 特点         | 运用场景                             |
| ------------ | ------------------ | -------------- | -------------------- | ----------------------- | ------------ | ------------------------------------ |
| Serial       | 串行回收           | 串行运行       | 作用于新生代         | 复制算法                | 响应速度优先 | 适用于单CPU环境下的Client模式        |
| ParNew       | 并行回收           | 串行运行       | 作用于新生代         | 复制算法                | 响应速度优先 | 多CPU环境Server模式下与CMS配合使用   |
| Parallel     | 吞吐量优先         | 并行运行       | 作用于新生代         | 复制算法                | 吞吐量优先   | 适用于后台运算而不需要太多交互的场景 |
| Serial Old   | 老年代的串行回收   | 串行运行       | 作用于老年代         | 标记-压缩算法           | 响应速度优先 | 适用于单CPU环境下的Client模式        |
| Parallel Old | 老年代的吞吐量优先 | 并行运行       | 作用于老年代         | 标记-压缩算法           | 吞吐量优先   | 适用于后台运算而不需要太多交互的场景 |
| CMS          | 低延时             | 并发运行       | 作用于老年代         | 标记-清除算法           | 响应速度优先 | 适用于互联网或B/S业务                |
| G1           | 区域划分代式       | 并发、并行运行 | 作用于新生代、老年代 | 标记-压缩算法、复制算法 | 响应速度优先 | 面向服务端应用                       |

![image-20230118180755205](/../JVM.assets/image-20230118180755205.png)

#### GC发展阶段

**Serial=> Parallel (并行) => CMS (并发) => G1 => ZGC**

##### 垃圾回收器的选择

- Java垃圾收集器的配置对于JVM优化来说是一个很重要的选择，选择合适的垃圾收集器可以让JVM的性能有一个很大的提升。
- 怎么选择垃圾收集器?
  - 优先调整堆的大小让JVM自适应完成。
  - 如果内存小于100M，使用串行收集器
  - 如果是单核、单机程序，并且没有停顿时间的要求，串行收集器
  - 如果是多CPU、需要高吞吐量、允许停顿时间超过1秒，选择并行或者JVM自己选择
  - 如果是多CPU、追求低停顿时间，需快速响应(比如延迟不能超过1秒，如互联网应用)，使用并发收集器
  - 官方推荐G1，性能高。现在互联网的项目，基本都是使用G1。
  - 后续ZGC
- 没有最好的收集器，更没有万能的收集
- 调优永远是针对特定场景、特定需求，不存在一劳永逸的收集器

##### 面试题

- 对于垃圾收集，面试官可以循序渐进从理论、实践各种角度深入，也未必是要求面试者什么都懂。但如果你懂得原理，一定会成为面试中的加分项。这里较通用、基础性的部分如下:
  - 垃圾收集的算法有哪些？如何判断一个对象是否可以回收？
  - 垃圾收集器工作的基本流程。
- 另外，大家需要多关注垃圾回收器这一章的各种常用的参数。

### GC日志分析

#### 参数

通过阅读GC日志，我们可以了解Java虚拟机内存分配与回收策略。

内存分配与垃圾回收的参数列表：

| 参数                      | 功能                                                         |
| ------------------------- | ------------------------------------------------------------ |
| -XX: +PrintGC             | 输出GC日志。类似: -verbose:gc                                |
| -XX: +PrintGCDetails      | 输出GC的详细日志                                             |
| -XX: +Prin tGCTimeS tamps | 输出GC的时间戳(以基准时间的形式)                             |
| -XX:+PrintGCDateStamps    | 输出GC的时间戳(以日期的形式，如2013-05-04T21 :53:59.234+0800 ) |
| -XX: +PrintHeapAtGC       | 在进行Gc的前后打印出堆的信息                                 |
| -Xloggc: . . /logs/gc.log | 日志文件的输出路                                             |

#### 实例

![image-20230118180941688](/../JVM.assets/image-20230118180941688.png)

![image-20230118181007247](/../JVM.assets/image-20230118181007247.png)

![image-20230118181042396](/../JVM.assets/image-20230118181042396.png)

#### 补充

![image-20230118181110257](/../JVM.assets/image-20230118181110257.png)

![image-20230118181154976](/../JVM.assets/image-20230118181154976.png)

##### 日志文件分析

young GC

![image-20230118181231853](/../JVM.assets/image-20230118181231853.png)

Full GC

![image-20230118181242625](/../JVM.assets/image-20230118181242625.png)

堆区

![image-20230118181753614](/../JVM.assets/image-20230118181753614.png)

JDK7

jdk7中，当伊甸园区的剩余空间小于即将存入的空间大小，且幸存者区的空间也不足时，就会将原本伊甸园区的内容直接存储到老年代中

![image-20230118181806015](/../JVM.assets/image-20230118181806015.png)JDK8

jdk8中，当伊甸园区的剩余空间小于即将存入的空间大小，且幸存者区的空间也不足时，会直接将新的内容存到老年代中

![image-20230118181816649](/../JVM.assets/image-20230118181816649.png)

##### 日志工具

常用的日志分析工具有: GCViewer、 GCEasy、GCHisto、GCLogViewer、Hpj meter、garbagecat等。

### 垃圾回收器的新发展

GC仍然处于飞速发展之中，目前的默认选项**G1 GC在不断的进行改进**，很多我们原来认为的缺点，例如串行的Full GC、 Card Table扫描的低效等，都已经被大幅改进，例如，JDK 10以后，Full GC已经是并行运行，在很多场景下，其表现还略优于Parallel GC的并行Full GC实现。

即使是Serial GC， 虽然比较古老，但是简单的设计和实现未必就是过时的，它本身的开销，不管是GC相关数据结构的开销，还是线程的开销，都是非常小的，所以随着云计算的兴起，**在Serverless等 新的应用场景下，Serial GC找到了新的舞台。**

比较不幸的是CMS GC， 因为其算法的理论缺陷等原因，虽然现在还有非常大的用户群体，但在JDK9中已经被标记为废弃，并在JDK14版本中移除。

#### JDK11特性

![image-20230118181835465](/../JVM.assets/image-20230118181835465.png)

318：运用于：运行完程序后就退出的情况

333：未来主流

#### Open JDK12的Shenandoah GC

- 现在G1回收器已成为默认回收器好几年了。

- 我们还看到了引入了两个新的收集器：
  - ZGC( JDK11出现)和Shenandoah
    (Open JDK12 )
  - 主打特点：低停顿时间

##### 概述

**Shenandoah，无疑是众多GC中最孤独的一个**。是第一款不由Oracle公 司团队领导开发的HotSpot垃圾收集器。不可避免的**受到官方的排挤**。比如号称OpenJDK和OracleJDK没有区别的0racle公司仍拒绝在0rac1eJDK12中支持Shenandoah。


Shenandoah垃圾回收器最初由RedHat进行的一项垃圾收集器研究项目Pauseless GC的实现，**旨在针对JVM上的内存回收实现低停顿的需求**。在2014年贡献给OpenJDK。

Red Hat研 发Shenandoah团队对外宣称，S**henandoah垃圾 回收器的暂停时间与堆大小无关，这意味着无论将堆设置为200MB还是200GB，99.9%的目标都可以把，垃圾收集的停顿时间限制在十毫秒以内**。不过实际使用性能将取决于实际工作堆的大小和工作负载。

##### 总结

**Shenandoah GC的弱项：高运行负担下的吞吐量下降。**

**Shenandoah GC的强项：低延迟时间。**

**Shenandoah Gc的工作过程大致分为九个阶段。（Java12新特性）**

#### 其他的垃圾回收器

AliGC是阿里巴巴JVM团队基于G1算法，面 向大堆(LargeHeap)应用场景。

指定场景下的对比:

![image-20230118181906993](/../JVM.assets/image-20230118181906993.png)

### ZGC：JDK14的新特性

#### 概述

ZGC与Shenandoah目标高度相似，**在尽可能对吞吐量影响不大的前提下，实现在任意堆内存大小下都可以把垃圾收集的停顿时间限制在十毫秒以内的低延迟。**

《深入理解Java虚拟机》一书中这样定义ZGC: ZGC收集器是一款基于Region内存布局的， (暂时)不设分代的，使用了读屏障、染色指针和内存多重映射等技术来实现**可并发的标记-压缩算法**的，**以低延迟为首要目标的一款垃圾收集器**。

ZGC的工作过程可以分为4个阶段：**并发标记->并发预备重分配->并发重分配->并发重映射等。**

ZGC几乎在所有地方并发执行的，除了初始标记的是STW的。所以停顿时间几乎就耗费在初始标记上，这部分的实际时间是非常少的。

+++

虽然ZGC还在试验状态，没有完成所有特性，但此时性能已经相当亮眼，用“令人震惊、革命性”来形容，不为过。

未来将在服务端、大内存、低延迟应用的首选垃圾收集器。

《ZGC设计与实现》

# 性能监控与调优

## 概述

## JVM监控及诊断工具-命令行篇

## JVM监控及诊断工具-GUI篇

## JVM运行时参数

## 分析GC日志

# JMM

## JMM(/../JAVA内存模型)

![image-20230118181953082](/../JVM.assets/image-20230118181953082.png)

![image-20230118182005222](/../JVM.assets/image-20230118182005222.png)

### 数据一致性

协议很多，Intel用MESI协议

MESI Cache一致性协议

https://www.cnblogs.com/z00377750/p/9180644.html

![image-20230118182020143](/../JVM.assets/image-20230118182020143.png)

现代CPU的数据一致性实现=缓存锁（MESI）+总线锁

### 缓存行+伪共享

读取缓存以cache line为基本单位，目前为64bytes（读取时，会把连续的64个字节都读取进来，这64个字节被称为缓存行）

位于同一缓存行的两个不同数据，被两个不同CPU锁住，产生相互影响的伪共享问题

#### 什么是伪共享（不好！）

![image-20230118182052041](/../JVM.assets/image-20230118182052041.png)

此处的xy可以看为一个缓存行，读取时会被 一起读取。左边的L2只读取x时，会读取整个xy；右边的L2只读取y时，也会会读取整个xy。当左边读取完x后，并将x的值进行修改后，右边需要重新读取xy的值；当右边读取y的值，并将y的值修改后，左边也需要重新读取xy的值。两个互相无关的值在修改自身读取的数据时，会产生缓存行的相互影响问题，因为他们位于同一行。

#### 伪共享的解决方法

在同一缓存行内（用数组存储，保证两个T对象紧挨着）：

```java
private static class T{
	public volatile long x = 0L;
}

public static T[] orr = new τ[2];

static {
	orr[0] = new T();
	arr[1] = new T();
}
```

不在同一缓存行内（提前存储一个56为的值，用于缓存行对其，确保两个T对象不在统一缓存行内）：、

```java
private static class Padding {
	public volatile long p1, p2, p3，p4, p5, p6，p7;//缓存行对其.
}

private static class T extends Padding {
	pubtic volatile long x = 0L ;
}

public static T[] arr = new T[2];

static {
	arr[0] = new T() ;
	arr[1] = new T() ;
}
```

方法类：

```java
public static void main(String[] args) throws Exception {
	Thread t1 = new Thread(()->{
		for(longi=0;i<100_0000L;i++){
			arr[0].x = i;
		}
});
    
	Thread t2 = new Thread(()->{
		for(inti=0;i<100_0000;i++){
			arr[1].x =i;
        }
	});
    
	final long start = System. nanoTime();
	t1.start();
	t2.start();
	t1.join();
	t2.join();
	System.out.println(System.nanoTime() - start);
}
```

运行结果：

![image-20230118182741943](/../JVM.assets/image-20230118182741943.png)

![image-20230118182751510](/../JVM.assets/image-20230118182751510.png)

当两个对象不在同一缓存行内时，效率有明显的提升。270>115

**结论：利用空间换取效率**

### 乱序问题

CPU为了提高指令执行效率，会在一条指令执行过程汇总（比如去内存读取数据（慢100倍）），去同时执行另一条指令，前提是，两条指令没有依赖关系

![image-20230118182811020](/../JVM.assets/image-20230118182811020.png)

#### 乱序执行的根源

https://www.cnblogs.com/liushaodong/p/4777308.html

#### 合并写

写操作也可以进行合并。

当CPU计算完一个数之后，会优先缓存到L1里面，当L1中的缓冲没有命中，就会被缓存到L2中。但由于L2的速度非常慢，当CPU再次对该值进行计算修改时，就会将这些指令合并到一个合并缓存中（将多步合并为一步），作为一个最终的合并结果缓存到L2中。

##### 实例

没有用到合并写：

```java
public static long runCaseOne() {
	long start = System.nanoTime() ;
	int i = ITERATIONS ;
	
    while (--i != 0) {
		int slot = i & MASK;
		byte b = (byte) i;
		arrayA[slot] = b;
		arrayB[slot] = b;
		arrayC[slot] = b;
		arrayD[slot] = b;
		arrayE[slot] = b;
		arrayF[slot] = b;
    }
	return System.nanoTime() - start;
}
```

用到合并写：

```java
public static long runCaseTwo() {
	long start = System.nanoTime() ;
	int i = ITERATIONS ;
	while (--i != 0) {
		int slot = i & MASK;
		byte b = (byte) i;
		arrayA[slot] = b;
		arrayB[slot] = b;
		arrayC[slot] = b;
	}
	i = ITERATIONS ;
	while (--i != 0) {
		int slot = i & MASK;
		byte b = (byte) i;
		arrayD[slot] = b;
		arrayE[slot] = b;
		arrayF[slot] = b;
	}
	return System.nanoTime() - start;
}
```

方法类：

```java
public static void main(final String[] args) {
	for(inti=1;i<=3;i++){
		System.out.println(i + " SingleLoop duration (ns) = " + runCaseOne()) ;
		System.out.println(i + " SplitLoop duration (ns) = " + runCaseTwo()) ;
    }
}
```

运行结果：

![image-20230118183732334](/../JVM.assets/image-20230118183732334.png)

运合并写提高了效率

合并写的buffer只有4个，为了提高效率，每一个buffer满了才进行运算，因此第一种6个，4+2等2的效率就低于第二种4+4的效率。

#### 证明乱序执行

程序

```java
Thread one = new Thread(new Runnable() {
	public void run() {
		//由于线程one先启动，下面这句话让它等一 等线程two. 读着可根据自己电脑的实际性能适当调整等待时间。
	shortWait( interval: 100000) ;
	a=1;
	X=b;
	}
});

Thread other = new Thread(new Runnable() {
	public void run() { .
		b=1;
		y =a;
	}
});

one.start() ;
other.start();

one.join();
other.join();

String result= "第"+i + "次("+X+","+y+") ";
if(x==0&&y==0){
	System.err.println(result);
break;
```

当以上代码执行时，如果没有乱序的话，只可能出现1，0；0，1；1,1三种情况。但是如果出现了0,0这种情况就说明出现了乱序

![image-20211116163930108](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211116163930108.png)

#### 有序性保障（如何保证特定情况下不乱序）

##### CPU内存屏障

加一个屏障，让两条指令不能重排 

sfence：在sfence指令前的**写**操作当必须在sfence指令后的**写**操作前完成。

Ifence：在lfence指令前的**读**操作当必须在Ifence指令后的**读**操作前完成。

mfence：在mfence指令前的**读写**操作当必须在mfence指令后的读写操作前完成。

##### intel lock汇编指令

原子指令，如x86上的”lock ...“指令是一个Full Barrier,执行时会锁住内存子系统来确保执行顺序，甚至跨多个CPU。Software Locks通常使用了内存屏障或原子指令来实现变量可见性和保持程序顺序

##### JVM级别如何规范（JSR133）

- LoadLoad屏障:对于这样的语句oad1; LoadLoad;
  - Load2,在Load2及后续读取操作要读取的数据被访问前，保证Load1要读取的数据被读取完毕。
- StoreStore屏障:对于这样的语句Store1;
  - StoreStore; Store2,在Store2及后续写入操作执行前，保证Store1的写入操作对其它处理器可见。
- LoadStore屏障:对于这样的语句Load1;
  - LoadStore;Store2,在Store2及后续写入操作被刷出前，保uEoad1要读取的数据被读取完毕。

- StoreLoad屏障:对于这样的语句Store1;
  - StoreL oad; Load2，在Load2及 后续所有读取操作执行前，保证Store1的写入对所有处理器可见。它的开销是四种屏障中最大的。在大多数处理器的实现中，
  - 这个屏障是个万能屏障，兼具其它三种内存屏障的功能

##### Volatile有序

Java内存模型中volatile变量在写操作之后会插入个 store屏障，在读操作之前会插入一个load屏障。一个类的final字段会在初始化后插入个store屏障，来确保final字段在构造函数初始化完成并可被使用时可见

volatile的实现细节

1. 字节码层面
   ACC_VOLATILE

2. JVM层面
   volatile内存区的读写 都加屏障

   > StoreStoreBarrier
   >
   > volatile 写操作
   >
   > StoreLoadBarrier

   > LoadLoadBarrier
   >
   > volatile 读操作
   >
   > LoadStoreBarrier

3. OS和硬件层面
   https://blog.csdn.net/qq_26222859/article/details/52235930
   hsdis - HotSpot Dis Assembler
   windows lock 指令实现 | MESI实现

synchronized实现细节

1. 字节码层面
   ACC_SYNCHRONIZED
   monitorenter monitorexit
2. JVM层面
   C C++ 调用了操作系统提供的同步机制
3. OS和硬件层面
   X86 : lock cmpxchg / xxx
   [https](https://blog.csdn.net/21aspnet/article/details/88571740)[://blog.csdn.net/21aspnet/article/details/](https://blog.csdn.net/21aspnet/article/details/88571740)[88571740](https://blog.csdn.net/21aspnet/article/details/88571740)

