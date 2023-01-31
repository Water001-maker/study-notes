# 基础内容及语法

## 1.java基础

由硬件和软件组成

##### 应用领域:

​		1)科学计算
​		2)人工智能(数据分析)
​		3)数据的处理
​		4)辅助设计
​		5)计算机网络...

##### 软件的划分:	

​		1)系统软件:dos,windows,Linux(centos7.8,红帽,deepin:国产),Mac 
​			移动端:android,ios, 鸿蒙(华为)
​		2)应用软件: 客户端软件必须更新才能使用服务器高版本的功能！
​			QQ客户端
​			office办公软件
​			vnc-view(客户端)
​			vnc-service(服务端)

##### 软件结构的划分:

C/S结构  :Client/Server 客户端和服务端
			举例:QQ客户端
			特点:客户端软件必须升级才能使用服务器高版本的功能

B/S架构:Browser/Server:浏览器和服务器端的交互	 (以后的方向)

​			服务器:开源的服务器:tomcat web容器   理论并发访问量:500个；http服务器:nginx(负载均衡,反向代理):5万个

​			特点:只要能够满足计算机中有浏览器即可
(IE,firfox,谷歌,浏览器端不需要特定的升级,就能够访问服务器中的一些内容!

##### 软件开发:						

notepad++  高级记事本

idea(2019/2020) :联想很精确(不适合前两周用)	

##### DOS命令：

| 命令                             | 用法                                      |
| -------------------------------- | ----------------------------------------- |
| exit                             | 退出当前DOS命令窗口                       |
| DOS复制：                        | 窗口中任意位置点击鼠标右键                |
| dir                              | 列出当前目录下的所有子文件/子目录         |
| cd                               | （change directory【改变目录】）          |
| cd ..                            | 回到上级目录                              |
| cd \                             | 直接回到根部路径                          |
| c:回车  d:回车   e:回车   f:回车 | 切换盘符                                  |
| Ipconfig                         | 查看IP地址                                |
| Xxx -version                     | 查看版本                                  |
| Del *                            | 删除                                      |
| md                               | 创造一个文件夹                            |
| rd                               | 删除文件夹（必须为空）                    |
| rd /s /q                         | 强制删除                                  |
| rd /s                            | 带内容的目录(文件夹):询问是否删除这个目录 |

##### java的基本含义：

​	JDK：（Java Development Kit）java开发工具包
​	JRE：（Java Runtime Envirment）java运行环境
​	JVM:（Java Virtual Machine）java虚拟机

下载地址：https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html

安装jdk完成之后:进入dos，输入java -version:查看当前本地的jdk的版本信息

安装好jdk之后:需要在安装目录的bin目录去使用的编译命令和执行命令
javac.exe	编译命令
java.exe	运行命令

##### path环境（系统配置）：

配置path环境变量的原因:
		我们需要在任何目录下使用javac和java对Java源文件进行编译和运行

###### 方式1:(存在弊端)

​		win10/win8---->此电脑---->右键属性---->高级系统设置---->环境变量---->系统变量  找到path将jdk安装目录的bin路径复制到path里面即可!

​		path: jdk\bin的路径

**弊端:**	
		1)当jdk版本升级了,开发者需要手动更改path:可能造成的path其他变量更改了,所以不好!		
		2)二阶段使用tomcat服务器(使用java编写的),启动的时候会寻找计算机中系统变量中的path是否存在
		JAVA_HOME,如果找不到,服务器启动时候出现"闪屏";		

###### 使用方式2:（推荐)：

​		win10/win8---->此电脑---->右键属性---->高级系统设置---->环境变量---->系统变量

​		新建变量名:
​				JAVA_HOME
​		变量值:
​				jdk路径		

**优点:**

​		1)不需要手动更改path
​		2)tomcat服务器启动的时候,找到JAVA_HOME变量,就不会出现闪屏

**path环境变量的执行流程:**

​		写好了java文件,对java文件进行编译和运行;

​				1)首先需要在dos中找到java文件的目录,如果找不到

​				2)就会在path环境变量中找%JAVA_HOME%\bin

​				3)如果path中都没有这个文件,那么久会提示 找不到文件...

###### classpath:临时环境变量(了解)：

​		classpath执行流程:先在系统变量中寻找是否存在classpath的路径
​		如果存在优先的是这里面的xxx.class文件

## 2.注释

##### 单行注释： 

```java
//只注释当前行
```

##### 多行注释：

```java
 /*

注释内容

注释内容

...

​      */
```

##### javadoc注释：

（这种注释是比较专业的注释，该注释信息会被javadoc.exe工具解析提取并生成帮助文档）

```java
/**

\* javadoc注释

\* javadoc注释

\* javadoc注释

\* javadoc注释

\* javadoc注释

*/
```

## 3.关键字和标识符

### 3.1关键字

什么是关键字:
			被Java语言赋予特定含义的单词!
			而且在高级记事本以及开发工具中,针对"关键字"有一种特殊颜色标记!
					notepad++		eclipse/idea
					editplus
			注意事项:
			goto和const这两个作为保留字使用,不是关键字!
			
关键字:			
	class	:表示创建一个类(是Java最基本的单元)	
	public  :权限修饰符的一种 :公共的,公开的,访问权限足够大
	static  :后面面向对象部分讲: 静态修饰符号
	void    :和Java中的方法有关系 (讲Java方法的时候去说)	

![image-20210710104832102](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20210710104832102.png)



### 3.2标识符

##### 什么是标识符

​		在Java原程序当中凡是**程序员有权利自己命名**的单词都是标识符（**黑色字体**）

​		可以标识的元素：**类名、方法名（main）、变量名、接口名、常量名（srgs）...**

##### 标识符的组成:

​		**一个合法的标识符只能由“数字、字母、下划线_、$”组成，不能含有其他符号**

​		**开头不能是数字**

​		**严格区分大小写**

​		**关键字（蓝色的）不能做标识符**

​		**理论上无长度限制，最好不要太长**

**如果是单个单词:**

字母全部小写
举例:

```java
main()
		int price  = 100 ;
```

**如果是多个单词:**

第一个单词字母全部小写,从第二个单词开始每个单词首字母大写,其余小写(小驼峰命名法)
举例:

```java
checkUserName()
			int applePrice  = 100 ;
```

**给常量命名的规则:**

如果是单个单词:字母全部大写(推荐多个单词)
举例:

```java
HELLO=100；
```

如果是多个单词:每个单词字母大写,单词和单词使用下划线隔开
举例:

```java
HELLO_WORLD=100；
```

后期学习:Thread:线程类

```java
public final static int MIN_PRIORITY = 1; 最小优先级

public final static int NORM_PRIORITY = 5; 默认优先级

public final static int MAX_PRIORITY = 10;最多优先级
```



## 4.常量

在程序的执行过程中,其值不发生改变的量!

##### 1)字面值常量

![image-20210710105015203](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20210710105015203.png)

**字符串常量:**使用双引号包裹起来的内容:称为"字符串常量"
					举例:
						"我爱高圆圆"
						"HelloWorld"
**字符常量:**使用单引号包裹起来的内容
				举例:
					'a','A','0'
					'我'
					'abc' 非法数据(只能单引号括起来的单个内容)
**整数常量:**
				100			包含"进制"    整数----默认十进制
				64
				...
**小数常量:**
				3.14
				12.56
**布尔常量:**true false

**空常量:**null		
					引用数据类型默认值null

##### 2)自定义常量

(面向对象后面讲:关键字:final:状态修饰符)

```java
public final static int MIN_PRIORITY = 1; 最小优先级
```

## 5.变量

**什么是变量？**

​		**变量本质上来说是内存中的一块空间，这块空间数据类型、有名字、有字面值**

​		**变量包括：数据类型、名称、字面值**

​		**变量是内存中存储数据的最基本的单元**

##### **变量要求：**

​	**变量存储的具体的“数据”，必须和变量的“数据类型”一致，当不一致的时候，编译报错。**

 

##### 变量的声明/定义变量的语法格式：

​		数据类型 变量名（合法标识符）；

 

##### 变量的赋值:

​		语法格式：变量名 = 字面值（“=”是赋值运算符）

​		PS.字面值的数据类型必须和变量名的数据类型一致

eg.

```java
Int i=10;//声明和赋值可以放在一起完成/*变量赋值之后，可以重新赋值（变量值可变）有了变量的概念之后，内存空间得到后可重复使用*/int i=10;System.out.println(i);//10int i=100;System.out.println(i);//100......System.out.println(i)
```



##### 访问变量的访问方法：

​		1.读取变量中保存的具体数据 get/获取

​		2.修改变量中保存的具体数据 set/设置

eg.

```java
i=20;//set

System.out.println(i);//get

//变量在一行上可以声明多个。先声明，再赋值，才能访问。

int a,b,c;
int a=1;
int b=2;
int c=3;
```

Java代码是遵循自上而下的顺序依次执行的，前一行的代码必须完整的结束之后，下一行的程序才能执行

在同一个“作用域”当中，变量名不能重名，但是变量可以重新赋值

 

##### 变量的作用域：

作用域：变量的有效范围，在什么范围之内是可以被访问的，只要出了这个范围，变量就不会被访问了。出了{}就不认识了。

 

##### 变量的分类：

根据变量声明的位置来分类：

局部变量：在方法体中声明的变量

成员变量：在方法体外，类体之内声明的变量

（不同作用域中，变量名可以重名；同一作用域中，变量名不能重名）



## 6.数据类型

数据类型的作用？

​		不同的数据有不同的类型，不同的数据类型底层会分配不同的空间

​		数据类型是指导程序在运行阶段应该分配多大的内存空间

### 6.1基本数据类型

| 基本数据类型 | 取值范围          | 默认值     | 占用空间大小(字节) |
| ------------ | ----------------- | ---------- | ------------------ |
| byte         | -128~127          | 0          | 1                  |
| short        | -32768~32767      | 0          | 2                  |
| int          | -2^31~2……2^31-1   | 0          | 4                  |
| long         | -2^63~2^63-1      | 0          | 8                  |
| float        |                   | 0.0f       | 4                  |
| double       |                   | 0.0d       | 8                  |
| char         | 0~2^16-1（65535） | false（0） | 2                  |
| boolean      | true/flase        | /u0000     | 1                  |

#### 字节

1 byte = 8 bit（1bit表示一个二进制位）

1 KB = 1024 byte

1 MB = 1024 KB

1 GB = 1024 MB

1 TB = 1024 GB

整数型当中的byte类型，占用1个字节，所以byte类型的数据占用8个比特位。

byte类型的取值范围是多少呢？

关于java中的数字类型，数字都是由正负之分的，所以在数字的二进制当中有一个二进制位被称为“符号位”，并且这个“符号位”在所有二进制位的最左边，0表示正数，1表示负数。

max：01111111（10000000（二进制）-1=127） min:11111111（-127）

具体的二进制表示还包括：原码、补码、反码

#### 进制转换

二换十 十换二....

计算机只认识二进制，那么计算机是如何表示现实世界当中的文字呢？

八种基本数据类型中char类型表示的是现实生活中的文字，文字和计算机二进制之间默认情况下是不存在任何转换关系的。

为了让计算机可以也表示先生世界当中的文字，我们需要进行人为的干涉，需要人负责提前制定好“文字”和“二进制”之间的对照关系。这种对照关系被称为：字符编码。

计算机最初只支持英文，最先出现的字符编码是：ASCII码

‘a’=97【01100001】

‘A’=65

‘0’=48

编码和解码时采用同一套对照表，不会出现乱码。

当解码和编码时采用的部室同一套对照表时，会出现乱码

#### 数据类型转换

```java
public class 练习{    public static void main(String[] args){        /*        100L是long类型字面值        x是long类型变量        不存在类型转换，直接赋值        */        long x=100L;        /*        x是long类型，8个字节        y是int类型，4个字节        编译报错，大容量不能直接变一个小容量        int y=x;        大容量转换小容量，需要进行强制类型转换        强制类型转换需要加“强制类型转换符”        加上强制类型转换符之后编译通过了，但是运行阶段可能损失精度        随意强制类型转换谨慎使用，因为损失精度之后损失很严重        强制转换原理        原始数据 00000000 000000000 00000000 00000000 0000000 0000000 00000000 01100000        强制转换之后：00000000 0000000 00000000 01100100        将左边的二进制砍掉！        */        int y=(int)x;        System.out.println(y);        /*        原始数据：00000000 000000000 00000000 00000000 10000000 00000000 00000000 000000000        强制转换之后的数据：10000000 00000000 00000000 00000000(补码形式）        以上补码转换到原码就是最终结果        */        long k=2147483648L;        int e=(int)k;        System.out.println(e);//损失精度非常严重，结果是负数【-2147483648】        /*        依据目前内容，以下程序无法编译        理由：50是int类型的字面值，b是byte类型的变量        显然是大容量int转换成的小容量byte        大容量转换成小容量是需要添加强制转换符的，一下程序没有添加强制转换符号，所以编译报错        但是，实际编译时，以下代码通过了，盖子面值可以直接赋值给byte类型的变量        */        byte b=50;//可以        byte c=127;//可以        //编译报错，128这个int类型的字面值超出了byte类型的取值范围，不能直接赋值给byte类型的变量        //byte b1=128;        byte b1=(byte)128;//-125        /*        纠正错误,需要使用强制类型转换符，但是一定会损失精度        原始数据：00000000 00000000 00000000 10000000        强制转换之后10000000【这是储存在计算机内部的，这是一个补码，】        System.out.println(b1);        */    }}
```

**补码：**

​		正数：与原码相同

​		负数：0变1，1变0，后加1

当一个整数字面值没有超出byte、short、char的取值范围，这个字面值可以直接赋值给byte、short、char类型的变量。（为了方便程序员的编程）

### 6.2引用数据类型

数组、类、接口

## 7.运算符

##### 7.1算术运算符

​	运算符有优先级，一个表达式当中有过多个运算符，不确定的加（），优先级得到提升。

​	没有必要专门去记忆运算符优先级

​	++出现在变量后，先做赋值运算，在对变量中保存的值进行自加1

![img](C:\Users\幻想家懒得想了\Desktop\wps1.jpg)	![img](C:\Users\幻想家懒得想了\Desktop\wps2.jpg) 

​	++出现在变量前，先进性自加1，在进行赋值

 ![img](C:\Users\幻想家懒得想了\Desktop\wps3.jpg)![img](C:\Users\幻想家懒得想了\Desktop\wps4.jpg)

<img src="C:\Users\幻想家懒得想了\Desktop\wps5.jpg" alt="img"  /> 

##### 7.2关系运算符

​	关系运算符的运算结果一定是布尔类型：true/false

##### 7.3逻辑运算符

​	& 逻辑与 （两边的算子都是真，结果才为真）

​	| 逻辑或 （两边算子只有一个是真，结果就是真）

​	! 逻辑非 （取反）

​	^ 逻辑异或 （两边算子不一样（真假/假真），结果就是真）

​	&& 短路与 （与逻辑与运算结果相同，不过短路与存在短路现象）

​	前半被判为false，整句便是false，后半便不再执行（比逻辑与更智能）

​	|| 短路或 （与逻辑或运算结果相同，不过短路或存在短路现象）

​	前半为真，整句为真，后半不再执行

​	逻辑运算符要求两边的算子都是布尔类型，并且逻辑运算符最终的运算结果也是一个布尔类型

##### 7.4赋值类运算符

###### 	基本的赋值运算符：

​	=

###### 	扩展的赋值运算符：

​	+=  -=  *=  /=  %=

eg.

```java
byte b=10;b=b+5;//报错
```

纠正：

```java
b = (byte)(b+5);b + = 5;//b+=5等同于b=(byte)(b+5)
```

扩展类的赋值运算符，不改变运算结果的数据类型（假设最初是byte，无论怎么追加、追减，最终结果还是byte类型）

##### 7.5字符串连接运算符

关于java中的“+”运算符

1加法运算（数字+数字）  

2字符串的连接运算（字符串+字符串）

3在一个表达式当中可以出现多个“+”，在没有添加小括号的前提下，遵循自左向右依次运算。

eg.

```java
int a=1;int b=2;System.out.println(a+“+”+b+“=”+(a+b));//1+2=3
```

##### 7.6三元（三目/条件）运算符

语法规则：

布尔表达式？表达式1：表达式2

执行原理：

当布尔表达式结果为真时，选择表达式1作为整个表达式的执行结果

当布尔表达式结果为假时，选择表达式2作为整个表达式的执行结果

eg1.

```java
boolean sex=true;

Char c =sex ? ‘男’:‘女’;//男
```

eg2.

```java
Sex=true;

System.out.println(sex ?‘男’:“女”);
```

##### 7.7位运算符

| 位运算符 |                                                 |
| -------- | ----------------------------------------------- |
| <<       | 乘2的n次幂                                      |
| >>       | 除2的n次幂                                      |
| >>>      | 被移位二进制最高位无论是0还是1，空缺位都用0补。 |

# 流程控制语句

## 8.流程控制语句

#### 8.1顺序结构语句

#### 8.2选择结构语句

##### 8.2.1 if语句

**if语句的语法结构：四种编写方式**

```java
/*PS.对于java中的if语句来说，只要有一个分支执行，整个if语句全部结束。*/

//第一种：
if（布尔表达式）{
/*
java语句；
java语句；
java语句；
java语句；
...
*/
}

//第二种：
if(布尔表达式){
/*java语句；
java语句；
...
*/
}else{
/*
java语句；
java语句；
...
*/
}

//第三种：
if(布尔表达式){
/*
java语句；
java语句；
...
*/
}else if(布尔表达式）{
/*
java语句；
java语句；
...
*/
}else if(布尔表达式）{
/*
java语句；
java语句；
...
*/
}.....

//第四种：
if(布尔表达式){
/*
ava语句；
java语句；
...
*/
}else if(布尔表达式）{
/**
java语句；
java语句；
...
*/
}else if(布尔表达式）{
/*
java语句；
java语句；
*/
...
}else{
/*
java语句；
java语句；
...
*/
}   
```

> **第二种编写方式和第四种编写方式都带有else分支，这两种方式可以保证100%会有分支执行。**
>
> **所有的控制语句是可以相互嵌套使用的。只要河里嵌套就行。**
>
> **嵌套使用时，代码格式保持完美。【该缩进时，必须缩进】**
>
> **if语句的分支中只有一条java语句时，大括号可以不写。**

eg.

```java
import java.util.Sacnner;

public class 天气问题 {
    public static void main(String[] args) {
        
        System.out.println("闲的没事干的寒假编程练习");
        System.out.println("天气分为“sun”和“rain”两种");
        System.out.println("性别分为“man”和“rain”两种");
        java.util.Scanner s = new java.util.Scanner(System.in);
        
        //接收天气状况
        System.out.print("请输入当前天气状况：");
        String weather = s.next();
        System.out.print("请输入性别：");
        String sex =s.next();
        
        if("sun".equals(weather)){
            System.out.print("请输入今天的温度：");
            int wendu =s.nextInt();
            System.out.println("大晴天");
            if(wendu>30){
                if("man".equals(sex)){
                System.out.println("戴墨镜");
                }else if("woman".equals(sex)){
                System.out.println("撒防晒霜");
                }else{
            System.out.println("妈的！看清楚再输入！");
            }
            }else{
            System.out.println("不出门了");
            }
        }else if ("rain".equals(weather)){
            System.out.println("大雨天");
            if("man".equals(sex)){
            System.out.println("大黑伞");
            }else if("woman".equals(sex)){
            System.out.println("小花伞");
            }else{
            System.out.println("妈的！看清楚再输入！");
            }
        }else {
            System.out.println("妈的！看清楚再输入！");
        }
    }
}
```



##### 8.2.2swtich语句

```java
switch语句的语法结构：

    switch(int或String类型的字面值或变量){
        case int或string类型的字面值或变量：
        	java语句；
        	...
        break；
        case int或string类型的字面值或变量：
        	java语句；
        	...
        break；
        case int或string类型的字面值或变量：
        	java语句；
        	...
        break；
        Default：
        	java语句;
        	....
    }
```

> **执行原理：**
>
> 1 switch后面小括号当中的“数据”和case后面的“数据”进行一一匹配，匹配成功的分支执行。
>
> 2 按照自上而下的顺序依次匹配。
>
> 3 匹配成功的分支执行，分支当中最后有“break”语句的话，整个switch语句终止。
>
> 4 匹配成功的分支执行，分支当中没“break”语句的话，直接进入下一个分支执行（不进行匹配），这种现象被称为case穿透现象（提供break语句可以避免穿透现象）
>
> 5 所有分支都没有匹配成功的话，当有default语句时，会执行default语句中的
>
> 6 switch后面和case后面只能是int或string类型的数据，不能是其他类型
>
> 当然byte short char也可以直接写到switch和case后面，因为他们可以进行自动类型转换转换成int类型。
>
> 7 case可以合并：
>
> ```java
> int i=10;switch(i){	case1:case2:case3:		System.out.println(“test code!”);}
> ```

eg.

```java
import java.util.Sacnner;public class Switch今天星期几 {    public static void main(String[] args) {                java.util.Scanner s = new java.util.Scanner(System.in);        System.out.print("猜猜今天星期几：");        int num = s.nextInt();        switch(num){            case 1:                System.out.println("星期一");                break;            case 2:                System.out.println("星期二");                break;            case 3:                System.out.println("星期三");                break;                case 4:                System.out.println("星期四");                break;            case 5:                System.out.println("星期五");                break;            case 6:                System.out.println("星期六");                break;            case 7:                System.out.println("星期天");                break;            default:                System.out.println("请输入一到七中的数字");        }    }}
```

#### 8.3循环结构语句

##### 8.3.1 for语句

```java
switch语句的语法结构：    	for(初始化表达式；布尔表达式；更新表达式）{		//是需要重复执行的代码片段【循环体：由java语句构成】	}
```

eg.1

```java
public class JavaApplication5 {
    public static void main(String[] args) {
    
   for(int i=1;i<=10;i+=2 ){
             System.out.println("i="+i);
    }
    
    for(int j=2;j<=10;j+=2 ){
             System.out.println("j="+j);
    }
    
    for(int k=10;k>0;k-- ){
             System.out.println("k="+k);
    }
    
    for(int z=100;z>0; ){
             System.out.println("z="+z);
             z-=10;
    }
    }
}
```

**循环语句和条件判断语句嵌套使用**

最好不要想太多，即使循环体当中是一个for循环，不要将这个for特殊化，

【for和if的嵌套】

eg.2

```java
//100以内的所有质数
public class ForFor嵌套求质数 {
    public static void main(String[] args) {
        //给范围
        for(int i=2;i<=100;i++){
           //求素数
           boolean sushu = true;
           for(int j=2;j<i;j++){
               if(i%j==0){
                  sushu = false;
                  break;
               }      
           }
           //输出
           if(sushu){
                System.out.print(i+"\t");
           }
    }   
    }
}
```

eg.3

```java
//10000以内的所有质数，每八个数字换一行//方法①public class ForFor嵌套求质数 {    public static void main(String[] args) {        int count=0;        //给范围        for(int i=2;i<=10000;i++){           //求素数           boolean sushu = true;           for(int j=2;j<i;j++){               if(i%j==0){                  sushu = false;                  break;               }                 }           //输出           if(sushu){               count++;               System.out.print(i+"\t");                if(count%8==0){                   System.out.println();               }           }        }       }}//方法②public class 求质数升级版 {     public static void main(String[] args){         int count=0;         for(int i=1;i<10000;i++){             boolean sushu = true;             for(int j=2;j<i;j++){                 if(i%j==0){                  sushu = false;                  break;               }                 }           //输出             if(sushu){               count++;               System.out.print(i+"\t");                 if(count==8){                   System.out.println();                   count=0;                }            }        }      }   }
```

##### 8.3.2 while语句

```java
while循环的语法结构：    while（布尔表达式）{    	循环体；    }//表达式为true时，执行循环体
```

> while循环的循环次数：
>
> 0到N次，while循环的循环体可能一次都不执行

eg.

```java
//输出0-10：public class ArrayOutPut {	public static void main(String[] args){		int i=0;		while (i<10){			System.out.print(i+"\t");			i++;        }	}}
```

eg.2

```java
class WhileTest{
	public static void main(String[] args){
		//1-100之间的和
		//1-100偶数和
		int x = 1 ; 
		int sum = 0 ;
		while(x<=100){
			if(x % 2 ==0){
				sum += x ;
			}
			
			x ++;
		}
		System.out.println("1-100之间的偶数和:"+sum) ;
		System.out.println("----------------------");
		int jc = 1 ;
		int j = 1 ;
		while(j<=5) {
			jc*= j ;
			j++ ;
		}
		System.out.println("5的阶乘是:"+jc) ;
		System.out.println("----------------------");
		
		//初始化
		int i = 100 ;
		while(i<=999){
			//定义三个变量
			int ge = i % 10 ;
			int shi = i /10 % 10 ;
			int bai = i/10 /10 % 10 ;
			
			if(i==(ge*ge*ge+shi*shi*shi+bai*bai*bai)){
				System.out.println(i) ;
			}
			i ++ ;
		}
		
	}
}
```

##### 8.3.3 do-while语句

```java
\\do...while循环的语法结构：do{	循环体；}while（布尔表达式）；
```

> do...whlie循环体代码片段执行次数是：1-N次（至少一次）
>
> do...while循环的注意事项：do...while循环语句最终有一个分号

##### 8.3.4几种循环结构的比较

###### 1.for和while循环的区别?

1)从格式上来讲:格式不同
	for(初始化语句;条件表达式;控制体语句){
		循环体语句;
	}
	

	初始化语句;while(条件表达式){	循环体语句;	控制体语句;}for循环结束之后,不能在访问for中变量,for循环结束;变量需要被释放掉了!while循环可以访问

2)内存:for循环节省内存空间
		堆,栈内存(存储局部变量:在方法定义中/方法声明上的变量),
		方法区..(main())
		

		for循环结束,变量随着被释放掉,节省内存空间;(不能访问这个变量了.)	while循环结束,依然可以访问这个变量,比较消耗内存空间...

3)使用场景:
	for循环:明确循环次数使用  水仙花数:100-999 (开发中:优先考虑for)
	while循环:不明确循环次数,使用
	举例:
		猜数字游戏:
			用户输入的数据和已经存在随机进行比较,用户不断录入数据!
			

		一般情况:都会采用	while(true){				当达到某种条件时结束;break;(不能单独使用,只能在switch和循环)	}

###### **2.for,while,dowhile的区别?**

dowhile:优先循环体语句,即使条件不成立,循环体至少执行一次!
	

​	在jdk提供的某些原码中可以看到do-while循环
​	
开发中:
​	优先for,其次while,再次do-while

##### 8.3.5死循环

两种格式:

```java
		for(;;){}			循环体语句;		}				while(true){					循环体语句;		}	
```

> 灵活使用死循环:在开发中使用,当前满足某个条件,跳转死循环!(结束掉/中断掉)
> 							break; 关键字

死循环的应用：

```java
import java.util.Scanner;
class GuessNumGame{
	public static void main(String[] args){
		

		//定义一个统计变量
		int count = 0 ;
		System.out.println("游戏开始了...") ;
		//1)产生一个1-100之间随机数
		
		int num = (int)(Math.random()*100+1) ;
		
		while(true){//不断录入
			//2)创建键盘录入对象
			Scanner sc = new Scanner(System.in) ;
			
			count ++ ;//统计变量++

			//提示并录入数据
			System.out.println("请您输入一个数据:") ;
			int guessNumber = sc.nextInt() ;
			
			//进行判断
			if(guessNumber>num){
				System.out.println("您要猜的数字大了...") ;
			}else if(guessNumber<num){
				System.out.println("您要猜的数字小了...") ;
			}else{
				System.out.println("恭喜您,第"+count+"次猜中了...") ;
				break ;//结束
			}
		}
		
	}

}
```

#### 8.4.跳转语句

##### 8.4.1break

> break是Java语句中的关键字，翻译为“中断/折断”
>
> break+“；”可以成为以单独的完整java语句：break；
>
> break语句使用在switch语句当中，用来终止switch的语句执行。
>
> break语句同样可以使用在循环语句当中，用来终止循环的执行
>
> break语句使用在for、while、do...while循环语句当中用来跳出循环，终止玄幻的执行。因为当程序循环到某个条件的时候，后续循环没必要执行，在执行也是耗费资源，所以可以终止循环，这样可以提高程序的执行效率。
>
> 在默认情况下，break终止的是离他最近的循环语句：

##### 8.4.2continue

> continue表示是：继续/下一个
>
> continue的java语句：continue;

**break和continue的区别：**

**break：跳出循环**

**continue:进入下一循环**

# 方法

## 9.方法

### 9.1什么是方法？

> 功能代码只需要输入一遍
>
> 要使用这个功能，只需要给这个功能传递具体的数据
>
> 这个功能完成之后返回一个最终结果
>
> 这样的代码就可以重复使用，提高代码复用性 
>
> 方法定义在类体当中，在一个类体当中可以定义多个方法，方法编写的位置没有先后顺序，可以随意。
>
> 方法体由java语句构成，方法体当中的代码遵守自上而下的顺序一次执行。
>
> **方法体当中不能再定义方法。**
>
> **方法就是一个代码片段，并且这段代码片段可以完成某个指定的功能，并且可以被重复的使用。**

### 9.2方法的定义

#### 9.2.1有具体返回值类型的方法的定义

```java
[修饰符列表] 返回值类型 方法名（形式参数列表）{
	方法体；
}
```

对以上的语法结构进行解释说明：

> 修饰符列表：
>
> ​	public: 访问权限足够大,公共的
>
> ​	static: 静态修饰符(面向对象中讲)
>
> 返回值类型：
>
> ​	就是数据类型(目前使用的基本数据类型...)
>
> 方法名: 
>
> ​	见名知意 (小驼峰命名法)
>
> 形参列表：
>
> ​	形参中起决定性作用的是形参的数据类型，形参的名字就是局部变量的名字
>
> ​	方法在调用的时候，实际给这个方法传递的真实数据被称为：实参
>
> ​	**实参和形参列表必须数量相同，类型对应相同**
>
> 方法体：
>
> ​	方法体必须由大括号括起来，方法体中你的代码有顺序，遵循自上而下的顺序依次执行。
>
> ​	并且方法由java语句构成，每一个java语句以“；”结尾。

#### 9.2.2有具体返回值类型的方法的定义:

```java
[修饰符列表] void 方法名（形式参数列表）{
	方法体；
}	
```

#### 9.2.3定义一个方法需要明确的两点：

1)明确返回值类型:没有具体返回值类型

2)明确参数类型以及参数个数

### 9.3方法的调用

```java
类名.方法名（实参列表）；//这是一条java语句，表示调用某个类的某个方法，传递这样的实参
```

> 方法只有在调用的时候才会执行，只定义不调用是不会执行的。
>
> **方法的修饰符列表当中有static**
>
> 方法调用的时候实参和形参要求能够进行相应的自动个数对应相同，数据类型对应相同
>
> 类型不同的时候要求能够进行相应的自动类型转换
>
> **调用本类中static修饰的方法时，类名可以省略（只有这种情况可以不加**）

### 9.4方法在执行过程当中，JVM中内存的分配和变化

#### 9.4.1.内存的分配

方法只定义不调用，是不会执行的，并且在JVM中也不会给该方法分配“运行所属”的内存空间。只有在调用这个方法的时候，才会动态的给这个方法分配所属的内存空间。

#### 9.4.2.内存的分配

在JVM内存划分上有这样三块主要的内存空间：

方法区内存

堆内存

栈内存

#### 9.4.3关于栈数据结构：

数据结构反映的是数据的存储形态

常见的数据结构：

数组、队列、栈、链表、二叉树、哈希表...

##### 概念：

stack，是一种数据结构

栈帧永远指向栈顶元素

栈顶元素处于活跃状态，其他元素静止

##### 术语：

压栈/入栈/push

弹栈/出栈/pop

##### 特点：

先进后出

后进先出

#### 9.4.4方法执行的时候代码片段存在哪里？方法执行的时候执行过程的内存在哪里分配？

方法代码片段属于.class的一部分，字节码文件在类加载的时候，将其放在方法区中。所以JVM中的三块主要的内存空间中方法区内存最先有数据。存放了代码片段。

代码片段虽然在方法区内存当中只有一份，但是可以被重复调用。每一次调用这个方法的时候，需要给该方法分配单独的活动场所，在栈内存中分配。【栈内存中分配方法运行的所属内存空间】

方法在调用的瞬间，会给该方法分配的内存空间，会在栈中发生压栈（给方法分配内存）动作，方法执行结束之后，给该方法分配的内存空间全部释放，此时发生弹栈（释放该方法的内存空间）动作。

局部变量在方法体中声明，局部变量运行阶段内存在栈中分配。

### 9.5方法的重载

没有使用重载时：

```java
package 重载;
public class 重载 {

        public static void main(String[] args) {
          int a = sumInt(1,2);
          System.out.println(a);
        
          double b = sumDouble(1.0,2.0);
          System.out.println(b);
        
          long c = sumLong(1L,2L);
          System.out.println(c);
        }
       //定义一个方法，计算int类型的求和
      public static int sumInt(int a, int b){
      return a+b;
}
       //定义一个方法，计算double类型的求和
       public static double sumDouble(double a, double b){
       return a+b;
       }
       //定义一个方法，计算long类型的求和
       public static long sumLong(long a, long b){
       return a+b;
}
}
```

以上内容方法虽然功能不同，但是功能是相似的，都是求和。在以下程序当中功能相似的方法，分别起了三个不同步名字，对程序员来说，调用方法的时候不方便，程序员需要记忆更多的方法，才能完成调用。【不方便且不美观】

重载：虽然功能不同，但是“功能相似”时，可以让程序员使用这些方法的时候就像在使用同一种方法一样，这样程序员以后编写代码比较方便，也不需要记忆更多的方法名，代码也会很美观。

使用重载后：

```java
重载2：
public class 重载2 {

      public static void main(String[] args) {
          a.b(true);
          a.b(2.0);
          a.b(null);
      }
}
class a{
       public static void b(byte c){
          System.out.println(c);
  }
    
   public static void b(short c){
          System.out.println(c);
      }
     
       public static void b(char c){
          System.out.println(c);
      }
      
       public static void b(boolean c){
          System.out.println(c);
      }
       
       public static void b(long c){
          System.out.println(c);
      }
        
       public static void b(double c){
          System.out.println(c);
      }
         
       public static void b(String c){
          System.out.println(c);
      }
           
       public static void b(int c){
          System.out.println(c);
      }
}
```

# 数组

## 10.数组

### 10.1概念

> 1.数组是一种引用类型
>
> 2.数组是一种简单的数据结构
>
> 3.数组是一个容器，可以用来存储其他元素（数组是可以存储任意类型的元素）
>
> 4.数组分为：一维数组，二维数组，三维数组，多维数组...
>
> 5.数组中存储的元素是统一的
>
> 6.数组长度不可改变，数组一旦创建长度是不可变的

数组中存储元素的类型是统一的，每一个元素在内存中所占的空间大小是相同的，知道数组的首元素的内存地址，要查找的元素只要知道下标就可以快速的计算出偏移量，通过首元素内存地址加上偏移量快速计算出要查找元素的内存地址，通过内存地址快速定位该元素，所以数组查找元素的效率提高。

随机的对数组进行增删元素，当增加元素的时候，为了保证数组中元素在空间存储上是有序的，所以被添加元素位置后面的所有元素都要向后移动。删除元素时，后面所有的元素要向前移动。所以数组的增删元素的效率很低。

### 10.2数组的定义

##### 初始化一维数组有两种方式：

> 1.静态初始化
>
> （数组 静态初始化）
>
> ```java
> 		数据类型[] 数组名称 = new 数据类型[]{元素1,元素2,元素3....} ;
> 		数据类型 数组名称[] = new 数据类型[]{元素1,元素2,元素3....} ;
> 简化格式:
> 		数据类型[] 数组名称  = {元素1,元素2,元素3....} ;
> 		数据类型 数组名称[]  = {元素1,元素2,元素3....} ;
> ```
>
> 2.动态初始化
>
> （数组 动态初始化）
>
> ```java
> 数据类型[]  数组名称   = new 数据类型[数组长度] ;
> 数据类型   数组名称[]  = new 数据类型[数组长度] ;
> ```

动态初始化一维数组会现在堆内存中分配这个数组，并且数组中每一个元素都采用默认值

##### 静态初始化和动态初始化的选择：

> 1.无论是动态初始化还是静态初始化，最终的内存分布都是一样的
>
> 2.在创建数组时，知道数组中应该存储什么数据，这个时候应该采用静态初始化

在创建数组时，无法预测数组中存储什么数据，只是先开辟空间，则使用动态初始化

### 10.3数组的应用

#### 排序

冒泡排序

eg.

```java
public class bubbleSort {

    public static void main(String[] args) {

        int arr[] = {56, 72, 94, 37, 25};

        text(arr);

    }

    public static void text(int arr[]) {

        for (int j = 0; j < arr.length; j++) {
            for (int i = 1; i < arr.length - j; i++) {
                if (arr[i - 1] > arr[i]) {
                    int temp = arr[i - 1];
                    arr[i - 1] = arr[i];
                    arr[i] = temp;
                }
            }
            System.out.print("第" + (j + 1) + "次循环的结果：[");
            for (int i = 0; i < arr.length; i++) {
                if (i == arr.length - 1) {
                    System.out.println(arr[i] + "]");
                } else {
                    System.out.print(arr[i] + ",");
                }
            }
        }

        System.out.println("—————————————————");

        System.out.print("最终的结果：[");
        for (int i = 0; i < arr.length; i++) {
            if (i == arr.length - 1) {
                System.out.print(arr[i] + "]");
            } else {
                System.out.print(arr[i] + ",");
            }
        }
    }

}
```

选择排序

eg.

```java
import java.util.Arrays;

public class selectSort {

    public static void main(String[] args) {

        int arr[] = {56, 72, 94, 37, 25};
        sort1(arr);

    }

    public static void sort1(int arr[]) {

        for (int i = 0; i < arr.length - 1; i++) {
            int min = i;

            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < arr[min]) {
                    min = j;
                }
            }

            int temp = arr[i];
            arr[i] = arr[min];
            arr[min] = temp;

            System.out.print("第" + (i + 1) + "次循环的结果：[");
            for (int k = 0; k < arr.length; k++) {
                if (k == arr.length - 1) {
                    System.out.println(arr[k] + "]");
                } else {
                    System.out.print(arr[k] + ",");
                }
            }

        }
        System.out.print("最终的结果：[");
        for (int i = 0; i < arr.length; i++) {
            if (i == arr.length - 1) {
                System.out.print(arr[i] + "]");
            } else {
                System.out.print(arr[i] + ",");
            }
        }

    }

}
```

#### 查找

常规查找

eg.

```java
import java.util.Scanner;

public class text10 {

    public static void main(String[] args) {

        int[] arr = {10, 55, 30, 24, 17,60,18};

        num(arr);

    }

    public static void num(int[] arr) {

        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入你要查询的数字");
        int n = scanner.nextInt();
        int count = 0;

        for (int i = 0; i < arr.length; i++) {
            count++;
            if (arr[i] == n) {
                System.out.println("这个数字在数组的第" + count + "个");
                break;
            } else if (i == arr.length-1 && arr[i] != n){
                System.out.println("您查找的数字没找到");
            }
        }
    }
}
```



# 面向对象

## 面向对象

### 面向对象和面向过程的区别

#### 面向过程

> 主要关注点是：实现的具体过程，因果关系【集成显卡的开发思路】
>
> 优点：对于业务逻辑比较简单的程序，可以达到快速开发，前期投入成本较低
>
> 缺点：采用面向过程的方式开发很难解决非常复杂的业务逻辑，另外面向过程的方法导致软件元素之间的“耦合度”很高，只要其中一环出问题，整个系统受到影响，导致最终的软件“扩展力”差。另外，由于没有独立体的概念，所以无法达到组件复用。

#### 面向对象

> 主要关注点是：主要关注对象【独立体】能完成哪些功能。【独立显卡的开发思路】
>
> 优点：耦合度低，扩展力强。更容易解决现实世界当中更复杂的业务逻辑。组件复用性强。
>
> 缺点：前期投入成本高，需要进行独立体的抽取，大量的系统分析与设计。

**C语言是纯面向对象的，C++半面向对象，java纯面向对象**

面向对象的三大特征：封装、继承、多态

采用面向对象的方式开发一个软件，生命周期当中：

面向对象的分析：OOA

面向对象的设计：OOD

面向对象的编程：OOP

#### 面向对象的思想概述和特点

面向对象是基于面向过程的编程思想

特点：

1.是一种更符合我们思想习惯的思想

2.可以将复杂的事物简单化

3.将我们从执行者编程指挥者（角色发生了转换）

### 类与对象的关系

> 类描述的是对象的共同特征
>
> 共同特征例如：身高特征
>
> 这个身高特征在访问的时候，必须先创建对象，通过对象去访问这个特征。
>
> 因为这个特征具体的某个对象上之后，值不同。有的对象身高180，有的280

#### 什么是类？

##### 类的概念

> 是一组相关的属性和行为的集合。
>
> 在现实世界当中是不存在的，是一个模板，是一个概念。是人类大脑思考抽象的结果。（在现实世界当中，对象A与对象B之间具有共同特征，进行抽象总结出一个模板，这个模板被称为类。）
>
> 类=属性+方法
>
> 属性：描述对象的状态信息
>
> 方法：描述对象的动作信息

**注意：状态和动作当具体到某个对象上之后，发现最终的结果可能不一样。**

**对象和对象之间有共同特征，但是具体到对象之后有数据的差异。**

##### 类的定义

```java
语法结构：
[修饰符] class 类名{
}
```

eg.

```java
public class Student{

//属性【描述的是对象的状态信息】
//属性通常采用变量的方式来定义
//在类体当中，方法体之外定义的变量被称为“成员变量”
//成员变量没有赋值，系统赋默认值：一切向0看齐

	int no;//学号
	String name;//名字
	boolean sex;//性别
	int age//年龄
	String address;//住址

//方法
//方法描述的是对象的动作信息
//当前例子就只描述属性，没有描述方法

}
```

基本数据类型：

​		byte、short、int、long、double、float、char、boolean

引用数据类型(SUN提供的，或者程序员自己写的}

​		String.class、System.class、User,class...

Java语言中所有的 .class都属于引用数据类型

#### 什么是对象？

##### 对象的概念

> 对象是实际存在个体。现实世界当中实际存在。

##### 对象的创建和使用

> 属性【存储数据采用变量的形式】
>
> 由于变量定义在类体中，方法体之外，这种变量称为成员变量
>
> 对象又被称为实例，实例变量又被称为对象变量。【对象级别的变量】
>
> 不创建对象，变量的内存空间时不存在的，只有创建了对象，这个变量的内存空间才会创建

eg.

```java
public class Student1{
	public static void main（String[] args）{


		int        i       =         10;
	 //数据类型    变量名         一个int类型的字面值

	//通过一个类可以实例N个对象
	//实例化对象的语法：new 类名（）；
	//new 是java语言当中 的一个运算符
	//new运算符的作用是创建对象，在JVM堆内存当中开辟新的内存空间

	//方法区内存：在类加载的时候，class字节码代码片段被加载到该内存空间当中
	//堆内存：new的对象在堆内存中存储

	//Student是一个引用数据类型
	//s是一个变量名
	//new Student（）是一个学生对象
	//s是一个局部变量【在栈内存中存储】

	//什么是对象：new运算符在堆内存中开辟的内存空间称为对象
	//什么是引用：引用是一个变量，只不过这个变量中保存了另一个java对象的内存地址
	//java语言当中，程序员只能通过“引用”去访问堆内存当中对象内部的实例变量。不能直接操作堆内存，java中没有指针。

		Student s = new Student();

		//访问实例变量的语法格式：
		//读取数据：引用.变量名
		//修改数据：引用.变量名=值

		int stNo = s.no;//学号
		String stName = s.name;//名字
		boolean  = s.sex;//性别
		int stAge = s.age//年龄
		String stAddr = s.addr;//住址

		System.out.println(“学号=”+stNo);
		System.out.println(“姓名=”+stName);
		System.out.println(“年龄=”+stSex);
		System.out.println(“性别=”+stAge);
		System.out.println(“住址=”+stAddr);

		Student stu = new Student();

		System.out.println(“学号=”+stu.no);
		System.out.println(“姓名=”+stu.name);
		System.out.println(“年龄=”+stu.sex);
		System.out.println(“性别=”+stu.age);
		System.out.println(“住址=”+stu.addr);

}
}
```



### 成员变量和局部变量的区别

**在类中的位置不同**

成员变量 类中方法外

局部变量 方法内或者方法声明上

**在内存中的位置不同**

成员变量 堆内存

局部变量 栈内存

**生命周期不同**

成员变量 随着对象的存在而存在，随着对象的消失而消失

局部变量 随着方法的调用而存在，随着方法的调用完毕而消失

**初始化值不同**

成员变量 有默认的初始化值

局部变量 没有默认的初始化值，必须先定义，赋值，才能使用。

成员变量 局部变量的区别

#### 相关知识点总结

1.JVM主要包括三块内存空间：栈内存、堆内存、方法区内存。

2.堆内存和方法区内存各有一个。一个线程一个栈内存。

3.方法调用的时候，该方法需要的内存空间在栈内存中分配，被称为压栈。方法执行结束之后，该方法所属的内存空间释放，被称为弹栈。

4.战中主要存储的是方法体中的局部变量。

5.方法的代码片段以及整个类的代码片段都被存储到方法区内存当中，在类加载的时候，这些代码片段会被载入

6.在程序执行过程中使用new运算符创建的java对象，出巡在堆内存当中。对象内部有实例变量，随意实例变量在堆内存当中

7.变量分类：

​		局部变量【方法体中声明】

​		成员变量【方法体外声明】

​				实例变量【修饰符前面没有static】

​				静态变量【修饰符中有static】

8.静态变量存储在方法区内存当中

9.三块内存当中变化最频繁的是栈内存，最先有数据的是方法区内存，垃圾回收器主要针对的是堆内存

10.垃圾回收器【自动垃圾回收机制，GC机制】什么时候会考虑将某个java对象的内存回收呢？

​		当堆内存当中的java对象成为垃圾数据的时候，会被垃圾回收器回收

​		什么时候堆内存中的java对象会变成垃圾呢？

​				没有更多的引用指向它的时候

​				这个对象无法被访问，因为访问对象只能通过引用的方式访问。

### 对象和引用

#### 对象和引用的概念

对象：目前在使用new运算符在堆内存中开辟的内存空间被称为对象

引用：是一个变量，不一定是局部变量，还可能是成员变量。引用保存了内存地址，指向了堆内存当中的对象。

所有访问实例相关的数据，都需要通过“引用”的方式访问，因为只有通过引用才能找到对象。

只有一个空的对象，访问对象的实例相关的数据会出现空指针异常。

### 参数的传递

主要研究和学习的是方法在调用的时候，涉及到参数传递的问题，到底是怎么传递数据的呢？

 eg.1

```java
int i=10;

int j=i;//i传递给j，实际上只是将i变量中的保存的10传递给j，j实际上是一块全新的内存空间。
```

 eg.2

```java
User u=new User;

User u2=u;//u传递给u2，实际上是将内存地址赋值给u2，u和u2实际上是两个不同的局部变量。
```

eg.3

```java
package 参数传递;
public class 参数传递 {
    public static void main(String[] args) {
      User u=new User(20);
//传递u给add方法的时候，实际上传递的是u变量中保存的值，只不过这个值是一个java对象的内存地址。
       add(u);
       System.out.println(u.age);
    }
   
    public static void add(User u){
        u.age++;
        System.out.println(u.age);
    }
}
class User{
    int age;
    public User(int age){
    	this.age=age;
    }
}
```

![image-20210716202326615](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20210716202326615.png)

#### 形参问题

当基本参数为形参时，其实际参数不会受到影响

当引用类型为形参时，其实际参数也会受到影响

```java
class Demo { 
	public int getSum(int a,int b) { 
		return a + b; 
	} 
}
class Test { 
	public static void main(String[] args) { 
		Demo d = new Demo(); 
		System.out.println(d.getSum(10,20)); 
	} 
}

//讲解的时候，先说我们去吃饭，调用服务员的记菜功能，然后服务员调用厨师的做菜功能。 完毕后，厨师调用服务员的端菜功能，最后，我们吃就OK了。 
class Student { 
	public void show() { 
		System.out.println(“show”); 
	} 
}
class StudentDemo { 
	//如果参数是一个类名，那么实际需要的是一个具体的对象 
	public void method(Student s) { 
		s.show(); 
	} 
}
class StudentTest { 
	public static void main(String[] args) { 
		StudentDemo sd = new StudentDemo(); 
		Student s = new Student(); 
		sd.method(s); //多个匿名对象的写法 
		new StudentDemo.method(new Student()); 
	} 
}
```

#### 最终结论

方法调用的时候，涉及到参数传递的问题，传递的时候，java只遵循一种语法机制，就是将变量中保存的“值”传递过去了，只不过有的时候这个值是一个字面值10，有时候这个值是另一个对象的内存地址。

### 匿名对象

匿名对象：

​		就是没有名字的对象。

​		是对象的一种简化表示形式

匿名对象的两种使用情况

​		作为实际参数传递

​		对象调用方法仅仅一次的时候

​		(因为没有栈内存变量指向堆内存地址,直接是在堆内存开辟空间,使用完毕,立即被回收!)

```java
1:new Student().show();
2:new StudentTest().method(new StudentDemo());
```

### 封装

#### 封装的好处

> 2.1封装之后，对于那个事物来说，看不到这个失误比较复杂的那一面，只能看待该事物简单地那一面。复杂性封装，对外提供简单地操作入口。（照相机：原理复杂，操作简单）
>
> 2.2封装之后才会形成真正的“对象”，真正的“独立体”
>
> 2.3封装就意味着以后的程序可以重复使用。并且这个事物应该适应性比较强，在任何场合都可以使用。
>
> 2.4封装之后，对于事物本身是安全的，提高了安全性【安全级别提高】

#### private关键字的特点:

> 1)可以修饰成员变量,也可以修饰成员方法,但是都只能在本类访问,外界类不能够访问
> 2)这些被私有修饰的成员变量,或者成员方法,可以间接通过公共方法来访问!

#### 封装的步骤

1.所有属性私有化，使用private关键字进行修饰，private表示私有的，修饰的所有数据只能在本类中访问

2.对外提供简单地操作入口，也就是说以后外部程序要想访问age属性，必须通过这些简单的入口访问：

​		对外提供两个公开的方法：get和set

​		修改——set

​		读取——get

3.set方法的命名规范：

```java
public void set+属性名首字母大写（形参）{
}
```

eg.

```java
public void setAge（int age）{
	this.age=age;
}
```

4.get方法的命名规范：

```java
public void set+属性名首字母大写（形参）{
}
```

eg.

```java
public int getAge（）{
	return age;
}
```

一个属性通常访问时的两种访问形式：

​		get 读取  set 修改

**setter和getter方法没有static关键字**

**有static关键字修饰的方法怎么调用：类名.方法名（实参）；**

**没有static关键字修饰的方法怎么调用：引用.方法名（实参）；**

### this关键字

#### 关于java的this关键字：

1 this是一个关键字，意为：这个

2 this是一个引用，this是一个变量，this变量中保存了内存地址指向了自身，this存储在JVM堆内存java对象内部。

3创建100个java对象，每一个对象都有this，也就说100个不同的this

4this可以出现在实例方法当中，this指向当前正在执行的这个动作的对象（this代表当前对象）

5多数情况下this可以省略不写

6this不能使用在带有static的方法中

7this.在区分局部变量和实例变量的时候不能省略

```java
private int ID;

public void setId(int id){

	this.id=id;

}

public int getId(){

	return id;

}
```

#### this本质

![image-20210716204158187](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20210716204158187.png)

Ps. 没有static关键字的方法被称为“实例方法”，实例方法怎么访问？“引用.”

没有static关键字的变量被称为“实例变量”

注意：当一个行为/动作执行的过程当中是需要对象参与的，那么这个方法一定要定义为“实例方法”，不要带static关键字

#### this的调用

实例方法：

```java
public class NewClass {
    public static void main(String[] args) {
    
        NewClass.dosome();
        
        dosome();
        
        NewClass a=new NewClass();
        a.dosome2();
        
        a.sagd();
}
    public static void dosome(){
        System.out.println("1");
    }
    
    //实例方法
    public void dosome2(){
        System.out.println("2");
    }
    
    
    //实例方法，调用方法一定是有对象存在的
    //一定实现创建对象，才能调用方法
    public void sagd(){
        
        //在大括号中的代码执行过程当中一定是存在“当前对象”的
        //也就是说这里一定有“this”
        System.out.println("3");
        
        /*
        dosome2是一个实例方法，实例方法调用必须有对象的存在
        以下代码的含义就是：调用当前对象的dosome2方法
        */
        this.dosome2();
        dosome2();//this一般可省略
    }
}
```

实例变量：

```java
public class This20 {
int num=1;//实例变量
    public static void main(String[] args) {
        This20 a=new This20();
        System.out.println(a.num);
    }
}
```

#### this的应用范围

可以使用在实例方法当中，代表当前对象【this.】

可以使用在构造方法当中，通过当前的构造方法来调用其他构造方法【this（实参）】

**重点：this（）只能出现在构造函数的第一行**

eg.1

```java
public class This日期 {
    public static void main(String[] args) {
        //创建日期对象1
        Date t1=new Date();
        t1.print();
        
        //创建日期对象2
        Date t2=new Date(2000,8,24);
        t2.print();
    }
}

public class Date {
    private int year;
    private int month;
    private int day;
    
    //构造方法
    public Date(int year,int month,int day){
        this.year=year;
        this.month=month;
        this.day=day;
    }
    
    /*
    当程序员调用一下无参数方法时，默认设置日期是“1970-1-1”
    */
    public Date(){
        /*
        this.year=1970;
        this.month=1;
        this.day=1;
        */
        //以上代码可以通过调用另一个构造方法来完成
        
        //需要采用以下语法来完成构造方法的调用
        //这种方法不用创建新的java对象，但同时又可以达到调用其他的构造方法。
        this(1970,1,1);                
    }
    //set get
    public int getYear(){
        return year;
    }
    public int getMonth(){
        return month;
    }
    public int getDay(){
        return day;
    }
    
    public void setYear(int year){
        this.year=year;
    }
    public void setMonth(int month){
        this.month=month;
    }
    public void setDay(int day){
        this.day=day;
    }
    
    //对外提供一个方法，可以把日期打印到控制台
    //实例方法
    public void print(){
        System.out.println(this.year+"年"+this.month+"月"+this.day+"日");
    }
}
```



### 构造方法

#### 关于java中的构造方法：

1.构造方法又被称为构造函数/构造器/Constructor

2.构造方法的语法结构：

```java
[修饰符列表] 构造方法名（形式参数列表）{
	构造方法体；
}
```

3.普通方法的语法结构：

```java
[修饰符列表] 返回值类型 方法名 （形式参数列表）{
	方法体；
}
```

4.对于构造方法来说返回值类型不需要指定。并且也不能写void，写上void，那么这个方法就变成普通方法了。

5.对于构造方法来说，构造方法的方法名必须和类名保持一致。

6.构造方法的作用？

构造方法存在的意义是，通过构造方法的调用，可以创建对象。

7.构造方法怎么调用？

普通方法是这样调用的：类名.方法名（实参列表）或者 引用.方法名（实参列表）

new 构造方法名(实参列表)

8.构造方法调用执行之后，有返回值吗?

每一个构造方法实际上执行结束之后都会有返回值，但是这个return值这样的语句不用写，java程序自动返回值。并且返回值类型是构造方法的所在的类的类型。由于构造方法的返回值类型就是本身，所以返回值类型不需要缩写。

9.当一个类中没有定义任何构造方法时，系统默认给该类提供一个无参数的构造方法，这个构造方法被称为缺省构造器

10.当一个类显示的将构造方法定义出来了，那么系统则不再默认为这个类提供缺省构造器了。建议开发中手动的为当前类提供无参数构造方法。因为无参数构造方法太常用了。

11.构造方法支持重载机制，在一个类当中编写多个构造方法，这多个构造方法显然已经构成方法重载机制。

#### 构造方法的作用：

1创建对象

2创建对象的同时，初始化实例变量的内存空间。【给实例变量赋值】

成员变量值实例变量，属于对象级别的变量，这种变量必须现有对象才能有实例变量

> 实例变量没有手动赋值时，系统默认赋值，那么这个系统默认赋值是在什么时候完成的呢？是在类加载的时候吗？
>
> 不是，因为雷佳在的时候只加载了代码片段，还没来得及创建对象。所以此时实例对象并没有初始化。
>
> 实际上，实例对象的内存空间是在构造方法执行过程当中完成开辟的。完成初始化的。
>
> 系统在默认赋值的时候，也是在构造方法执行过程当中完成的赋值。
>
> 实例变量是存储在JVM的堆内存的 

### 标准类

类

**成员变量**

**构造方法**

无参构造方法

带参构造方法

**成员方法**

 getXxx()

 setXxx()

**给成员变量赋值的方式**

无参构造方法+setXxx()

带参构造方法

eg.

```java
class Student{
	//属性私有化
	private String name ;//姓名
	private int age ; //年龄
	private String gender ;//性别
	private String sid ;  //学生学号
	
	//无参构造方法
	public Student(){
		
	}
	
	//有参构造方法
	public Student(String name,int age,String gender,String sid) {
		//赋值
		this.name = name ;
		this.age = age ;
		this.gender = gender ;
		this.sid = sid ;
	}
	
	//提供setXXX()/getXXX()方法
	//给姓名赋值
	public void setName(String name){
		this.name = name ;
	}
	//获取姓名
	public String getName(){
		return name ;
	}
	
	//给年龄赋值
	public void setAge(int age){
		this.age = age ;
	}
	//获取年龄
	public int getAge(){
		return age ;
	}
	
	//给性别赋值
	public void setGender(String gender){
		this.gender = gender ;
	}
	//获取性别
	public String getGender(){
		return gender ;
	}
	
	//给学号赋值
	public void setSid(String sid){
		this.sid = sid ;
	}
	//获取学号
	public String getSid(){
		return sid ;
	}
	
	//吃
	public void eat(){
		System.out.println("学生吃营养餐...") ;
	}
	//睡
	public void sleep(){
		System.out.println("学习累了就需要休息") ;
	}
	//学习
	public void study(){
		System.out.println("学习JavaSE...") ;
	}
}


//测试类
class StudentTest{
	public static void main(String[] args){
		
		//测试学生类
		//方式1:通过无参构造方法 + setXXX()/getXXX()
		Student s = new Student() ;
		s.setName("亚索") ;
		s.setAge(23) ;
		s.setGender("男") ;
		s.setSid("s007") ;
		//获取内容
		System.out.println("姓名为:"+s.getName()+",年龄是:"+s.getAge()+",性别是:"+s.getGender()+",学号是:"+s.getSid()) ;
		s.eat() ;
		s.sleep() ;
		s.study() ;
		
		System.out.println("----------------------------------------") ;
		
		//方式2:通过有参构造方法赋值+getXXX()获取内容
		Student s2 = new Student("李青",45,"男","s008") ;
		System.out.println("姓名为:"+s2.getName()+",年龄是:"+s2.getAge()+",性别是:"+s2.getGender()+
		",学号是:"+s2.getSid()) ;
		s2.eat() ;
		s2.sleep() ;
		s2.study() ;
		
		
	}
	
}
```

### static关键字

#### 静态static关键字的特点:

**1）随着类的加载而加载**

**2）优先于对象存在:** 

它不能和this共存(this:代表当期类对象的地址值引用)

对象还没有new的时候,当前被static修饰的成员就已经内存了

**3）被静态修饰**

可以被多个对象共享:有共享共用的意思

举例:饮水机中的水(适合) <==> 水杯共享(不适合!)

**4）被静态修饰的变量,方法=>静态变量或者静态方法**

我们所说的成员变量和成员方法:都指的是非静态

静态的成员的访问方式:类名.变量\类名.方法名()

**5）关于static关键字的使用注意事项:**

①非静态的方法既可以访问静态变量,也可以访问非静态的变量；既可以调用静态方法,也可以调用非静态方法

②静态的方法:只能访问静态变量；只能调用静态方法

**简单记:静态只能访问静态**

#### 静态变量和成员变量的区别

**所属不同**

静态变量属于类，所以也称为为类变量 

成员变量属于对象，所以也称为实例变量(对象变量) 

**内存中位置不同** 

静态变量存储于方法区的静态区 

成员变量存储于堆内存 

**内存出现时间不同** 

静态变量随着类的加载而加载，随着类的消失而消失 

成员变量随着对象的创建而存在，随着对象的消失而消失 

**调用不同**

静态变量可以通过类名调用，也可以通过对象调用 

成员变量只能通过对象名调用 

### 代码块

**在Java中，使用{}包裹棋起来的内容，成为代码块!**

#### 代码块的分类

局部代码块:在方法定义中使用，作用:限定局部变量的生命周期

构造代码块:在类的成员位置(类中，方法外)，使用{}包裹起来作用:给 类中的一些成员进行数据初始化

#### 代码块的特点

每次在执行构造方法之前，如果存在构造代码块，先执行构造代码块中的内容

#### 静态代码块

在类的成员位置，直接使用static{},

**特点:**

随着类的加载而加载，优先于对象存在!

**作用:**

也可以通过static代码块，对一些操作 (后期I0流创建文件.JDBC (Java链接数据库注册驱动))

**静态代码块就加载一次!**

#### 优先级:

静态代码块 > 构造代码块 > 构造方法

eg.

```java
package HemoWork.day10;

public class Test01 {

    public static void main(String[] args) {
        new Leaf();

    }
}

class Root {
    static {
        System.out.println("Root的静态初始化块");
    }

    {
        System.out.println("Root的普通初始化块");
    }

    public Root() {
        System.out.println("Root的无参数的构造器");
    }
}

class Mid extends Root {
    static {
        System.out.println("Mid的静态初始化块");
    }

    {
        System.out.println("Mid的普通初始化块");
    }

    public Mid() {
        System.out.println("Mid的无参数的构造器");
    }

    public Mid(String msg) {
        this();
        System.out.println("Mid的带参数构造器，其参数值：" + msg);
    }
}

class Leaf extends Mid {
    static {
        System.out.println("Leaf的静态初始化块");
    }

    {
        System.out.println("Leaf的普通初始化块");
    }

    public Leaf() {
        super("hello");
        System.out.println("Leaf的构造器");
    }

}
```

运行结果：

```
Root的静态初始化块
Mid的静态初始化块
Leaf的静态初始化块
Root的普通初始化块
Root的无参数的构造器
Mid的普通初始化块
Mid的无参数的构造器
Mid的带参数构造器，其参数值：hello
Leaf的普通初始化块
Leaf的构造器
```

### 继承

#### 继承的概念：

将多个类的共性内容抽取到独立的类中，然后这多各类和独立的类产生一种关系：继承关系

####  继承的好处：

1）提高代码的维护性

2）提高代码的复用性

3）让类和类之间产生了关系，是“多态的前提条件”

#### 什么时候使用继承？

1）不要为了实现部分功能而使用继承关系

 2）A is a B的时候可以使用继承

#### 继承的特点：

1）继承只支持单继承，有些语言中支持多继承，但是Java中不支持

2）不支持多继承，但支持多层继承

#### 注意事项：

1）子类继承父类：可以继承父类的非私有的成员，私有的成员外界不能访问，只能在本类中访问。但是可以通过公共访问间接方法

2）构造方法是不能被继承的，但是子类可以简介通过super关键字访问父类的构造方法

#### 成员变量的关系问题：

**a)子类继承父类，如果子类中的成员变量名称和父类的成员变量名称不一致，分别访问即可!**

 **b)子类继承父类，如果子类的成员变量名称和父类的成员变量名称致: 如何访问呢?** 

> 1)首先在子类的局部位置找，是否存在局部变量名称，如果有，就使用
>
> 2)如果没有，就在子类的成员位置找，是否存在这个变量，如果存在，就使用
>
> 3)如果在子类的成员位置中没有找到，直接在父类的成员位置中找，如果有，就是使用!
>
> 4)如果父类的成员位置都没有，就没有这个变量，报错!

**遵循一个原则:就近原则!**

#### 继承中无参构造方法的访问：

##### 1)子类继承父类，子类的所有的构造方法都会默认的访问父类的无参方法

##### 2)如果父类中没有无参构造方法子类会怎么样?

子类的所有的构造都会报错! (因为子类所有构造方法默认父类的无参构造方法!)

##### 如何解决呢?

方式1:手动给出父类的无参构造方法(推荐)

方式2:在子类的构造方法中的第一句话:通过super (xxx),间接的访问父类的有参构造方法

方式3:只要子类的所有构造方法中一个能够让父类初始化即可!

#### 方法重载和方法重写的区别：

##### 方法重载：

在一个类中，提供n多个接口，这些功能，方法名相同，参数列表不同，与返回值无关(目的:提高某个功能的扩展性)

参数列表不同：①类型不同，②个数不同，③考虑参数类型的顺序

构造方法也可以重载

##### 方法重写：

在继承关系中，子类出现了和父类一模一样方法声明，重写的目的：子类有自己的功能，需要将父亲的该功能覆盖掉

**方法重写时需要注意：**

当前子类继承父类时，如果存在方法重写，那么子类重写该方法访问权限不能更低

要么跟父类方法保持一致，要么加上public，最好和父类的修饰一样

#### 什么时候使用继承？、

如果A类是B类的一种，或者B类是A类的一种，这个使用extends完成两者继续关系，体现的是一种"is a"的关系。不要为了部分功能而去使用继承!

eg.

```java
public class People {

    private String name;
    private int age;
    private String sex;

    public People() {
    }

    public People(String name, int age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
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

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public void eat() {    }

}

class PeopleNan extends People {

    public PeopleNan() {
    }

    public PeopleNan(String name, int age, String sex) {
        super(name, age, sex);
    }

    @Override
    public void eat() {
        System.out.println("南方人吃米饭");
    }

    public String work() {
        return "南方人经商";
    }

}

class PeopleBei extends People {

    public PeopleBei() {
    }

    public PeopleBei(String name, int age, String sex) {
        super(name, age, sex);
    }

    @Override
    public void eat() {
        System.out.println("北方人吃面");
    }

    public String work() {
        return "北方人考学";
    }

}

class PeopleTest {

    public static void main(String[] args) {

        People people = new People();
        PeopleBei peopleBei = new PeopleBei();
        PeopleNan peopleNan = new PeopleNan();

        peopleBei.setName("老北");
        peopleBei.setAge(40);
        peopleBei.setSex("男");

        System.out.println("我是" + peopleBei.getName() + "，今年" + peopleBei.getAge() + ",是个" + peopleBei.getSex()+"的");
        peopleBei.eat();
        System.out.println(peopleBei.work());


        peopleNan.setName("南姐");
        peopleNan.setAge(40);
        peopleNan.setSex("女");
        System.out.println("我是" + peopleNan.getName() + "，今年" + peopleNan.getAge() + ",是个" + peopleNan.getSex()+"的");
        peopleNan.eat();
        System.out.println(peopleNan.work());

    }

}
```

运行结果：

```java
我是老北，今年40,是个男的
北方人吃面
北方人考学
我是南姐，今年40,是个女的
南方人吃米饭
南方人经商
```

eg2

```java
public class Animal {

    private String name;
    private int age;
    private String color;

    public Animal() {}

    public Animal(String name, int age, String color) {
        this.name = name;
        this.age = age;
        this.color = color;
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

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public void eat(){}

    public void play(){}

    public void introduce(){
        System.out.println("我是一只"+getName()+"，我是"+getColor()+"色的，今年"+getAge()+"岁");
    }

}

class Cat extends Animal{

    public Cat() {}

    public Cat(String name, int age, String color) {
        super(name, age, color);
    }

    public void eat(){
        System.out.println("猫咪吃饭");
    }

    public void play(){
        System.out.println("猫咪舔自己");
    }

    public void introduce(){
        System.out.println("我是一只"+getName()+"猫，我是"+getColor()+"色的，今年"+getAge()+"岁");
    }

}

class Dog extends Animal{

    public Dog() {
    }

    public Dog(String name, int age, String color) {
        super(name, age, color);
    }

    public void eat(){
        System.out.println("狗狗吃饭");
    }

    public void play(){
        System.out.println("狗狗吃骨头");
    }

    public void introduce(){
        System.out.println("我是一只"+getName()+"狗，我是"+getColor()+"色的，今年"+getAge()+"岁");
    }

}

class AnimalTest{

    public static void main(String[] args) {

        Animal animal = new Animal();
        Cat cat = new Cat("大黄",3,"黄");
        Dog dog = new Dog("大毛",5,"白");

        cat.eat();
        dog.eat();

        cat.play();
        dog.play();

        cat.introduce();
        dog.introduce();

    }

}
```

运行结果：

```
猫咪吃饭
狗狗吃饭
猫咪舔自己
狗狗吃骨头
我是一只大黄猫，我是黄色的，今年3岁
我是一只大毛狗，我是白色的，今年5岁
```

#### super和this关键字

this代表本类对应的引用。

super代表父类存储空间的标识(可以理解为父类引用)

#### final关键字

##### 关于final关键字的特点:

  1)可以修饰类，该类不能被继承!

  2)可以修饰符成员方法，成员方法不能重写! (根据具体的题意要求!)

  3)可以修饰的变量，这个变量此时是一个常量! (自定义常量)

##### 面试题:

final , finally, finalize(的区别?

> final的关键字描述以及的三个特点
>
> finally:处理异常用到try... catch... finally(后面讲)
>
> finalize() :跟垃圾回收器有关系:回收内存中没有更多引用的对象(后面在讲常用类）

##### final修饰的变量：

普通变量：

final int a = 10;

实例变量：

final Student student = new Student();

##### fina1修饰基本数据类型和引用类型的区别?

final修饰基本数据类型:基本数据类型的对应的数据值不能在被赋值了，只能赋值一次!

final修饰引用类型:引用数据类型对应的地址值不能被改变

```java
final Student s = new Student() ;//修饰实例变量s, s的堆内存地址值永远是固定值!
new Student() ;//重新开辟 空间(报错)
```

##### 开发中书写常量:

都使用public static final int a = 100 ;

> public:访问权限足够大
>
> static:可以被类名直接访问
>
> final:这个变量是一个常虽

### 多态

#### 多态的概念

一个事物在不同时刻不同形态

#### 多态的前提条件

1）必须存在继承关系extends

2）必须存在方法重写【子类需要覆盖父类的功能】

3）必须有父类引用指向子类对象（向上转型）

```java
F f = new Z();
```

#### 多态的访问特点

**1）针对成员变量的访问问题**

​    编译看左(看Fu类是否存在变量，存在，编译不会报错!)

​    运行看左(使用Fu类的东西)

**2）针对成员方法的访问问题（非静态）**

​    编译看左(看Fu类是否存在这个方法，存在，编译不会报错!)

   运行看右(存在方法重写，所以最终子类的功能将父类的该功能进行覆盖!)

**3）如果成员方法是静态方法（静态成员方法算不上方法重写，直接可以类名来访问，跟类相关的方法）**

​    编译看左(看Fu类是否存在这个静态方法，存在，编译不会报错!),

​    运行看左(静态方法:子类出现了父类一模一样的静态方法，不能算重写，和类相关)

**4）针对构造方法的访问问题:**

​    存在继承关系：还需要让父类先初始化，然后再子类进行数据初始化!（分层初始化!）

#### 多态的好处

1）提高了代码的复用性（有继承保证）

2）提高了代码的扩展性（有多态保证）

#### 多态的弊端

不能访问子类的特有功能

##### 如何解决？（如何访问子类独有的功能）

方案一：具体的子类创建具体的子类对象

```java
Zi z = new Zi() ;
z.成员方法名()
本身Fu f = new Zi();//已经在堆内存中开辟空间了
Zi z = new Zi();//在堆内存中又开辟空间，从内存角度考虑，这种比较消耗内存空间
```

方案二：将父类引用转换为子类引用（向下转型）

```java
Fu fu = new Zi();
Zi zi = (Zi)fu;
```

##### 需要注意：

向下转型使用不当，就出现java. lang. ClassCastException；类转换异常；属于运行时异常。

当前堆内存中的实例不是该类型时，就会出现问题!

eg

```java
class Animal{

    static String name = "狗";

    int age = 5;

    public Animal() {
    }

    public void eat(){
        System.out.println("饿了就吃");
    }

}

class Dog1 extends Animal{

    String name;

    int age = 2;

    public Dog1() {
    }

    public Dog1(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }

    public void sleep() {
        System.out.println("狗打呼噜");
    }
}

public class DuoTaiTest {

    public static void main(String[] args) {


        Animal animal = new Dog1("大黄");
        //成员方法
        animal.eat();//子类

        //强制类型转换（向下转型）
        Dog1 dog2 = (Dog1)animal;
        dog2.sleep();

        //成员变量
        int age = animal .age;
        System.out.println(age);//父类

        //构造方法
        //赋值需要向下转型
        System.out.println(((Dog1) animal).getName());

        Animal dog = new Dog1("aa");

        System.out.println(dog.name);

    }

}
```

运行结果

```
狗吃骨头
狗打呼噜
5
大黄
狗
```

eg2

```java
public class JuiceMachineTest {

    public static void main(String[] args) {

        JuiceMachine juiceMachine = new JuiceMachine();
        juiceMachine.makeJuice(new Apple());
        juiceMachine.makeJuice(new Banana());
        juiceMachine.makeJuice(new Orange());

    }

}

class JuiceMachine {
    public void makeJuice(Fruit fruit) {
        fruit.Juice();
    }
}

class Fruit {
    public void Juice() {
        System.out.println("出汁！");
    }
}

class Apple extends Fruit {
    public void Juice() {
        System.out.println("流出苹果汁");
    }
}

class Banana extends Fruit {
    public void Juice() {
        System.out.println("流出香蕉酱");
    }
}

class Orange extends Fruit {
    public void Juice() {
        System.out.println("流出橙汁");
    }
}
```

运行结果

```
流出苹果汁
流出香蕉酱
流出橙汁
```

### 抽象类

#### 什么是抽象类？

现实世界事物中，某个事物是比较概括性，描述为抽象事物

将某个事务中的一些功能仅仅给出声明即可，没有方法体。

抽象类关键字：abstract关键字

#### 抽象方法的格式:

```java
权限修饰符(eg.pub1ic) abstract 返回值类型方法名 (形式参数列表);
abstract class 类名{}
```

#### 抽象类的特点:

**1)有抽象方法的类一定是抽象类；**

**2)抽象类中不一定只有抽象方法，还可以有非抽象方法(有方法体**) ；

**3）抽象类不能实例化=>不能创建对象**

 通过具体的子类进行实例化（创建对象），抽象类多态Fu fu = new Zi();

**4）抽象类的子类有两种情况：**

 ①目前来说:如果抽象类的子类都是抽象类---毫无意义。因为子类也不能new，除非再有具体的子类；

②抽象类的子类是具体类---才能new:抽象 多态的形式Fu fu = new Zi()

#### 抽象类成员的特点：

**成员方法**：
          既可以定义为抽象方法，也可以定义为非抽象方法
**成员变量：**
          既可以定义变量，也可以定义常量
**构造方法：**
          既可以有参狗仔，也可以无参构造

#### abstract关键字和那些关键字冲突：

final：abstract修饰的类继承后，该类的方法需要重写，而final修饰的类不能继承，也不能有子类，方法更不能重写，相互冲突。

private：私有方法子类的方法不能被继承，就不能被重写，而abstract就是要方法重写。相互冲突。

static：static能被实例化可直接调用，abstract是不能被实例化，相互冲突。

####   面试题:

##### 一个类中没有抽象方法，那么将这个类定义为抽象类的意义何在?

为了不让它直接实例化!

##### 如何实例化？

1）直接具有具体的子类

2）间接具有具体的子类

可能:某个功能的返回值是它本身---->功能里面的逻辑可能就是在new 最具体的子类!

eg

```java
//创建一个名称为 Vehicle类并将它声明为抽象类,在Vehicle类中声明一个NoOfWheels方法,使它返回一个字符串值.
public abstract class Vehicle {
    public abstract String NoOfWheels();
}

//Motorbike类
public class Motorbike extends Vehicle{
    @Override
    public String NoOfWheels() {
        return "双轮车";
    }
}

//Car类
public class Car extends Vehicle {
    @Override
    public String NoOfWheels() {
        return "四轮车";
    }
}

//创建一个带main方法的类,在该类中创建 Car和Motorbike的实例,
public class VehicleTest {

    public static void main(String[] args) {

        Vehicle vehicle1 = new Car();
        System.out.println(vehicle1.NoOfWheels());

        Vehicle vehicle2 = new Motorbike();
        System.out.println(vehicle2.NoOfWheels());

    }

}
```

运行结果

```java
四轮车
双轮车
```

eg.2

```java
public abstract class Animal{

    String name;
    int age;
    String sex;

    public Animal() {
    }

    public Animal(String name, int age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
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

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public abstract void eat();

    public abstract void sleep();

    public abstract void hobit();

    public void introduce(){
        System.out.println("我是"+getName()+"，我今年"+getAge()+"岁，我是"+getSex()+"的");
    }
}

//猫类
public class Cat extends Animal {

    public Cat() {
    }

    public Cat(String name, int age, String sex) {
        super(name, age, sex);
    }

    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }

    @Override
    public void sleep() {
        System.out.println("猫睡觉");
    }

    @Override
    public void hobit() {
        System.out.println("猫玩毛线");
    }
}

//狗类
public class Dog extends Animal {

    public Dog() {
    }

    public Dog(String name, int age, String sex) {
        super(name, age, sex);
    }

    @Override
    public void eat() {
        System.out.println("狗吃饭");
    }

    @Override
    public void sleep() {
        System.out.println("狗睡觉");
    }

    @Override
    public void hobit() {
        System.out.println("狗打滚");
    }
}


//一个测试类
public class AnimalTest {

    public static void main(String[] args) {

        Animal animal1 = new Dog("大黄",5,"公");
        animal1.introduce();
        animal1.eat();
        animal1.hobit();
        animal1.sleep();

        Animal animal2 = new Cat("nono",4,"母");
        animal2.introduce();
        animal2.eat();
        animal2.hobit();
        animal2.sleep();

    }

}
```

运行结果

```
我是大黄，我今年5岁，我是公的
狗吃饭
狗打滚
狗睡觉
我是nono，我今年4岁，我是母的
猫吃鱼
猫玩毛线
猫睡觉
```

### 接口

本质是体现一个现实世界事物所具有的额外的扩展抽象

#### 接口的特点:

​      1）不能实例化(不能创建对象）

​          如何实例化？通过接口的子实现类(一定是具体类)进行实例化----接口多态(使用时最!)

​      2）子类实现和接口的关系: implements实现关系

​      3）接口的子类，要么是抽象类，要么重写接口中的所有抽象方法

####   接口的成员特点：

​      成员变量：只能常量，默认用public static final修饰

​      构造方法：接口没有构造方法

​      成员方法：只有抽象方法，默认用public abstract修饰

#### 抽象类和接口之间的区别：

##### 1）成员的区别

抽象类：

成员变量：既可以是常量，也可以是变量；

成员方法：既可以是抽象方法，也可以是非抽象方法；

构造方法：既存在有参构造，也存在无参构造；

接口：

成员变量：稚嫩是常量，默认修饰符public static final；

成员方法：只能是抽象方法：默认修饰符 public abstract；

构造方法：接口中没有构造方法

##### 2）关系的区别

类与类之间的关系：extends 继承关系

类与接口之间的关系：implements 实现关系

接口与接口之间的关系：extends 继承关系，支持多继承

##### 3）设计理念的区别：

抽象类不能实例化，需要通过具体的子类实例化=>is a

接口不能实例化，需要通过具体的子实现类实例化=>like a

eg.

```java
public class EatingOut {

    public static void main(String[] args) {
        //创建厨师对象
        FoodMenu cooker1 = new ChinaCooker();
        FoodMenu cooker2 = new AmricanCooker();
        //创建顾客对象
        Customer customer = new Customer(cooker1);

        //顾客点菜
        customer.order();
    }

}

//顾客
class Customer {

    /*
    Cat is a Animal,但凡满足isa的表示都可以设置为继承。
    Customer has a FoodMenu,但凡是满足hasa的表示都以属性的形式存在。
    */
    private FoodMenu foodMenu;

    public Customer() {
    }

    public Customer(FoodMenu foodMenu) {
        this.foodMenu = foodMenu;

    }

    public FoodMenu getFoodMenu() {
        return foodMenu;
    }

    public void setFoodMenu(FoodMenu foodMenu) {
        this.foodMenu = foodMenu;
    }

    public void order(){
       foodMenu.shiZiCchaoJiDan();
       foodMenu.yuXiangRouSi();
    }

}

//菜单
interface FoodMenu {

    void shiZiCchaoJiDan();

    void yuXiangRouSi();
}

//中国厨师
class ChinaCooker implements FoodMenu {

    @Override
    public void shiZiCchaoJiDan() {
        System.out.println("中餐厨师做的西红柿炒鸡蛋，得劲！");
    }

    @Override
    public void yuXiangRouSi() {
        System.out.println("中餐厨师做的鱼香肉丝，得劲！");
    }
}


//外国厨师
class AmricanCooker implements FoodMenu {

    @Override
    public void shiZiCchaoJiDan() {
        System.out.println("西餐厨师做的西红柿炒鸡蛋，蒂丽舍丝");

    }

    @Override
    public void yuXiangRouSi() {
        System.out.println("西餐厨师做的鱼香肉丝，蒂丽舍丝");
    }

}
```

运行结果

```
中餐厨师做的西红柿炒鸡蛋，得劲！
中餐厨师做的鱼香肉丝，得劲！
```

### 类型与类型之间的关系

**is a（继承）、has a（关联）、like a（实现）**

#### is a：

Cat is a Animal（猫是一个动物）

凡是能够满足is a的表示“继承关系”

A extends B

#### has a：

I has a Pen（我有一支笔）

凡是能够满足has a关系的表示“关联关系”

关联关系通常以“属性”的形式存在。

#### like a:

Cooker like a FoodMenu（厨师像一个菜单一样）

凡是能够满足like a关系的表示“实现关系”

实现关系通常是：类实现接口。

### 形式参数和返回值问题

#### 如果方法的形式参数是类调用方法的时候,如何传递实际参数?

具体类,调用该方法,实际参数需要传递当前具体类的对象

抽象类,调用该方法实际参数需要传递的抽象类的子类对象 (抽象类多态)

接口  调用该方式,实际参数需要传递的是当前接口的子实现类对象(接口多态)

#### 如果一个方法的返回值是引用类型,最终方法结束,如何返回?

具体类  :方法返回的就是当前具体类对象!

抽象类 :需要返回的是抽象类的子类对象

接口:需要返回的是该接口的子实现类对象

### 包

#### 编译和运行

##### 同一个包下的编译和运行:

###### 第一种方式（手动方式）

1)需要在当前文件夹下将com文件夹以及子文件夹qf手动创建出来
2)进入dos控制台:进入当前java文件所处的目录下

```
javac  源文件名称(HelloWorld.java)---->HelloWorld.class 字节码文件				
```

3)将2)步产生的字节码文件放在1)qf子文件夹下

4)运行:带上包运行 :java 包名.类名

```
java com.qf.HelloWorld	
```

###### 第二种方式（自动方式）

1)直接进入dos控制台,进入到指定目录下:

```
javac -d . java源文件(HelloWorld.java)
```

2)自动将包名创建出来以及包下面的字节码文件	

3)运行:带上包运行 :java 包名.类名

```
java com.qf.HelloWorld
```

##### 不同包下的编译和运行

1)将net.hsbc.Demo类导入进来

2)通过自动方式将net.hsbc.Demo---->先编译成

3)然后再通过自动方式对Test.java文件进行编译

4)运行即可java com.qf.Test				

#### package

第一：package出现在java源文件第一行。

第二：带有包名怎么编译？javac -d . xxx.java

第三：怎么运行？java 完整类名

补充：以后说类名的时候，如果带着包名描述，表示完整类名。

如果没有带包，描述的话，表示简类名。

#### import

import什么时候不需要？
java.lang不需要。
同包下不需要。 
其它一律都需要。

eg

```java
	import 完整类名;
	import 包名.*;

	import java.util.Scanner; // 完整类名。

// 同学的疑问：这样是不是效率比较低。
// 这个效率不低，因为编译器在编译的时候，会自动把*变成具体的类名。
	import java.util.*;

// 想省懒劲你不能太省了。
	import java.*; 这是不允许的，因为在java语言中规定，这里的*只代表某些类的名
```

### 访问控制权限

#### 访问控制权限都有哪些？

| private   | 私有   |
| --------- | ------ |
| public    | 公开   |
|           | 默认   |
| protected | 受保护 |

#### 以上的4个访问控制权限:控制的范围是什么?

private表示私有的，只能在本类中访问
public表示公开的，在任何位置都可以访问
"默认"表示只能在本类，以及同包下访问
protected表示只能在本类、同包、子类中访问.

| 访问控制修饰符 | 本类 | 同包 | 子类 | 任何位置 |
| -------------- | ---- | ---- | ---- | -------- |
| public         | 可以 | 可以 | 可以 | 可以     |
| protected      | 可以 | 可以 | 可以 | 不行     |
| 默认           | 可以 | 可以 | 不行 | 不行     |
| private        | 可以 | 不行 | 不行 | 不行     |

### **内部类**

#### 格式

**内部类**:
    在一个类中可以定义另一个:
    在类A 中定义了类B,将类B就称为类A的内部类,类A就是外部类!
**成员内部类:**
    在一个类的成员位置中定义了另一个类
    内部类可以访问外部类的成员,包括私有!

```java
//外部类
class Outer{
    //成员变量
    public int num = 100 ;
    private int num2 = 200 ;
    class Inner{ //成员内部类
        //成员内部类的一个成员方法
        public void method(){
            System.out.println("method Inner");
            System.out.println();
            System.out.println(num2);
        }
    }
    //外部类的成员方法
    public void show(){
        //访问的成员内部类的method--->通过创建内部类对象来访问
        //method() ;错误的----访问的本类的method方法
        Inner inner = new Inner() ;
        inner.method();
    }
}
//测试
public class InnerClassDemo {
    public static void main(String[] args) {
        //创建外部类对象
        Outer outer = new Outer() ;
        outer.show();
    }
}
```

#### 外部类访问内部类

**1）外部类如何直接访问内部类的成员方法?**

格式:

外部类名.内部类名  对象名 =  new 外部类对象().new 内部类对象() ;

```java
class Outer2{
    private int num = 20 ;
    //成员内部类
    class Inner2{
        public void show(){
            System.out.println(num);
        }
    }
    //外部类的成员方法
    public void method(){
        // 创建内部类对象访问内部类的成员方法
    }
}
//测试类
public class InnerClassDemo2 {
    public static void main(String[] args) {
        //外部类名.内部类名  对象名 =  外部类对象.内部类对象;
        //适用于:通过外部类直接访问成员内部类的成员(前提条件:当前成员内部类是一个非静态类)
        Outer2.Inner2 oi = new Outer2().new Inner2() ;
        oi.show() ;
    }
}
```

**2）关于我们成员内部类的修饰符:**

在成员内部类上面---加入private修饰:为了数据的安全性,它的访问---就要外部类的公共访问间接访问...

非static内部类不能有static修饰的变量或方法

非static的内部类，在外部类加载的时候，并不会加载它，所以它里面不能有静态变量或者静态方法。

​		1、static类型的属性和方法，在类加载的时候就会存在于内存中。

​		2、要使用某个类的static属性或者方法，那么这个类必须要加载到jvm中。

```java
伪代码
     举例:
     人首先身体---身体内有心脏
           class Body{
               //内部类:心脏
             private  class Heart{  //加入private:保证数据的安全性
                      //手术
                    public void operator(){
                        System.out.println("心脏搭桥手术....") ;
                    }
               }
               //外部类提供一些公共访问
               public void method(){
                  if("如果你是外科医生"){
                       Heart heart = new Heart() ;
                       heart.operator() ;
                   }
               }
           }
           //外部类直接访问成员内部类的成员
```

**3）如果当前成员内部类是静态的,  里面的方法无论是静态的还是非静态的,都只能访问外部类的静态成员,包括私有!**
如何直接访问静态成员内部类的成员呢?
将静态的成员内部类看成是外部类的静态成员访问
直接访问方式
外部类名.内部类名 对象名 = new 外部类名.内部类名() ;

```java
class Outer3{
    //定义非静态的成员变量
    public int num = 50 ;
    private static int num2 = 20 ;
    //定义成员内部类:静态的           ---->静态的成员内部类可以看成是外部类的静态成员
    static class Inner3{//此时类都是静态
        public void show(){
           // System.out.println(num);
            System.out.println(num2);
        }
        public static void show2(){
           // System.out.println(num);
            System.out.println(num2);
        }
    }
}
//测试类
public class InnerClassDemo3 {
    public static void main(String[] args) {
        // 外部类名.内部类名 对象名  = 外部类对象.内部类对象;
        //Outer3.Inner3 oi = new Outer3().new Inner3() ;  适用不了了
        //   外部类名.内部类名 对象名 = new 外部类名.内部类名() ;
        Outer3.Inner3 oi = new Outer3.Inner3() ;
        oi.show();
        oi.show2() ; //静态---不推荐对象名访问
        System.out.println("------------------------------");
        //show2()的另一种方式
        Outer3.Inner3.show2(); //show2()静态方法
    }
}
```

#### 局部内部类

##### 概念

在外部类的成员方法中的定义的类，局部内部类可以访问外部类的成员变量包括私有!

在外部类的局部位置,访问内部类的成员方法,创建当前局部内部类对象来访问!

##### 面试题

> 局部内部类访问局部变量的时候,此时局部变量应该注意什么?(JDK7/JDK8),为什么要加入final关键字呢?
>

```java
/*
如何此时Java环境是Jdk7,局部内部类访问局部变量时,此时该变量必须显示加入final修饰；
目前环境是JDK8环境,做了什么优化?
*/
//原码
class Outer2 {
    public void method() {
        int num = 20; //局部变量

        class Inner2 {
            public void show() {
                System.out.println(num);
            }
        }

        //创建Inner2类对象
        Inner2 inner2 = new Inner2();
        inner2.show();
    }
}

//测试类
public class OuterDemo2 {
    public static void main(String[] args) {
        //创建Outer2对象
        Outer2 outer2 = new Outer2();
        outer2.method();
    }
}

//通过反编译查看
class Outer2$1Inner2{
	final int val$num;  //num已经加入了final修饰

	final Outer2 this$0;

	public void show(){
		System.out.println(val$num);
	}

	Outer2$1Inner2(){
		this.this$0 = this$0;
		val$num = I.this;
		super();
	}
}
```

原因:

> 局部变量的生命周期是随着方法调用而存在，随着方法调用结束而消失
>
> 而当前外部类对象调用method 方法的时候，此时num进入栈内存,在局部位置创建了局部内部类对象
>
> 而局部内部类对象调用它的成员方法访问该变量，方法method方法结束之后，内部类对象不会立即消失,
>
> 它里面的成员方法在访问局部变量，局部变量必须变成常量，常驻内存，否则如果当前变量消失了，局部内部类的成员依然在访问
>
> 就会出现冲突！所以 jdk7 收到必须加入final修饰，jdk8通过jvm已经做了优化了，无需手动加入final修饰

#### 匿名内部类

##### 概念

没有名字的内部类，一般在我们局部位置使用!

格式:

```java
//匿名内部类它是内部类的一种简化格式
new 类名(可以是抽象类,也可以具体类)或者是接口名(){
	重写功能
};
```

##### 本质:

继承了该类或者是实现了该接口的子类对象

##### 匿名内部类在开发中的使用:

###### （一）方法的形式参数如果是一个抽象类，需要接口的子类对象

> 1）将子类定义出来 继承自抽象类
>
> 2）直接使用抽象类的匿名内部类

```java
//抽象类
abstract  class Person{
    public  abstract  void work() ;
}

//PersonDemo类
class PersonDemo{
    public void method(Person p){//方法的形式参数是一个抽象类
        p.work();
    }
}

//定义一个子类继承自Person类
class Student extends  Person{
    @Override
    public void work() {
        System.out.println("good good study ,day day up!!");
    }
}

//测试类
public class PersonTest {
    public static void main(String[] args) {
        //如何调用PersonDemo类中的method方法?
        //创建PersonDemo类对象 /或者匿名对象
        PersonDemo pd = new PersonDemo() ;
        //抽象类多态:创建子类对象
        Person p = new Student() ; //定义一个子类Student
        pd.method(p) ;
        System.out.println("----------------------------------------");

        //使用匿名内部类去完成:
        //调用PersonDemo类中的method方法
        PersonDemo pd2 = new PersonDemo() ;
        pd2.method(new Person(){ //实际参数也是传递抽象类的子类对象(抽象类的匿名内部类)
            @Override
            public void work() {
                System.out.println("good good Study,day day Up!!");
            }
        });
    }
}
```

######  （二）方法的形式参数如果是一个接口，需要传递的接口子实现类对象

> 方式1:将接口的子实现类定义出来
>
> 方式2;使用接口的匿名内部类

```java
//接口
interface Mary{
    void mary() ;
}

//定义一个LoveDemo类
class LoveDemo{
    public void funciton(Mary mary){//形式参数是一个接口
        mary.mary();
    }
}

//定义一个子类实现类
class You implements  Mary{

    @Override
    public void mary() {
        System.out.println("要结婚了,很开心...");
    }
}

//测试类
public class LoveTest {
    public static void main(String[] args) {
        //方式1:需要调用LoveDemo类中的function方法
        LoveDemo loveDemo  = new LoveDemo() ; //或者使用匿名对象
        //接口多态
        Mary mary = new You() ;
        loveDemo.funciton(mary);

        System.out.println("----------------------------------------");

        //方式2:接口的匿名内部类
        //创建LoveDemo类对象
        LoveDemo ld = new LoveDemo() ;
        ld.funciton(new Mary() {

            @Override
            public void mary() {
                System.out.println("要结婚了,很开心...");
            }
        });
        
    }
}
```

###### （三）方法的返回值是接口类型

> 需要返回的当前接口的子实现类对象

```java
//接口
interface Love{
    void love() ;
}

class LoveDemo{
    public Love function(){
        return new Love(){
            @Override
            public void love() {
                System.out.println("爱Java,爱学习...");
            }
        } ;
    }
}

//定义接口的子实现类
class Student implements  Love{
    @Override
    public void love() {
        System.out.println("爱Java,爱学习...");
    }
}

//测试类
public class LoveTest {
    public static void main(String[] args) {
        //要访问LoveDemo类中的function方法
        LoveDemo ld = new LoveDemo() ;
        Love love = ld.function();//Love love  =  new Student() ;
        love.love();

        System.out.println("------------------");
        //方式2:接口匿名内部类的方式
        LoveDemo ld2 = new LoveDemo() ;
        Love l = ld2.function();
        l.love();
    }
}
```

###### （四）方法的返回值是一个抽象类

> 需要返回的是当前抽象类的子类对象

```java
//抽象类
abstract  class Person{
    public abstract  void work() ;
}

class PersonDemo{
    public Person method(){
         
        //直接抽象类的匿名内部类
        return new Person(){//相当于Person类的子类对象(匿名内部类的本质)
            @Override
            public void work() {
                System.out.println("程序员日日夜夜敲代码");
            }
        } ;
    }
}

//定义一个子类
class Programmer extends  Person{
    @Override
    public void work() {
        System.out.println("程序员日日夜夜敲代码...");
    }
}

//测试类
public class PersonTest {
    public static void main(String[] args) {
        //调用PersonDemo类中的method 方法
        PersonDemo pd = new PersonDemo() ;
        Person person = pd.method(); //Person pserson = new Programmer() ;
        person.work();

        System.out.println("--------------------------");
        //方式2:匿名内部类
        PersonDemo pd2 = new PersonDemo() ;
        Person p = pd2.method();
        p.work();
    }
}
```

例题：

```java
/*要求:需要在控制台分别打印30,20,10

考点:
 外部类直接访问非静态的成员内部类的格式
 外部类的成员变量的方法方式,(在成员内部类的成员方法中)
 成员变量,局部变量名称都一样(就近原则)
 外部类和内部类没有继承关系
*/
 class Outer{
    int num = 10 ;

    //成员内部类
    class Inner{
        int num = 20 ;
        public void show(){
            int num = 30;
            //补全代码
            System.out.println(num);
            System.out.println(this.num);//this限定 :this.变量名:当前类的成员变量
           // System.out.println(new Outer().num) ; //方式1:new Outer().num

            System.out.println(Outer.this.num) ; //方式2:外部类的this限定
        }
    }
}
public class OuterTest {
    public static void main(String[] args) {
            Outer.Inner oi = new Outer().new Inner() ;
            oi.show();
    }
}
```

# 常用类

## Object类

是类结构层次的根类(超类--->父类)，所有的类都默认继承自Object

使用JDK提供的API文档学习常用类中的常用功能

API：Application Programming Interface:应用程序接口开发文档

### getClass方法

Object功能类的getClass()方法

```java
public final Class getClass();//表示正在运行的类  (就是字节码文件对象)
```

先去使用Class类（反射的时候去使用）:

功能：public String getName():获取当前类的全限定名称(包名.类名)

> Object类中的getClass()/finalize()/hashCode()以及以后常用类。功能中如果native关键字:本地方法,非java语言底层实现

##### 面试题:

获取一个类的字节码文件对象有几种方式? 

> 三种
> 第一种：通过Object类的getClass()--->Class       :正在运行的java类: class  包名.类名
> 第二种：任意Java类型的class属性----获取当前类的字节码文件对象Class
> 第三种：Class里面forName("类的全限定名称(包名.类名)") ; (使用最多)

```java
public class ObjectTest01 {

    public static void main(String[] args) throws ClassNotFoundException {
        //创建一个学生类对象
        Student s = new Student();
        Class c1 = s.getClass();
        System.out.println(c1);//HemoWork.day15.ObjectTest.Student    class 包.类名
        Class c2 = s.getClass();
        System.out.println(c2);//HemoWork.day15.ObjectTest.Student   class 包名.类名
        System.out.println(c1 == c2);
        //==在基本数据类型里面:比较的是数据值相同,在引用类型:比较的是两个对象的地址值是否相同
        
        // ①Class ---->HemoWork.day15.ObjectTest.Student
        String name1 = c1.getName();
        String name2 = c2.getName();
        System.out.println(name1);
        System.out.println(name2);

        //Class类中public static Class forName(String classname): 后期反射中使用
        Class c3 = Class.forName("HemoWork.day15.ObjectTest.Student");//③Class里面forName("类的全限定名称(包名.类名)") ;
        System.out.println(c1 == c3);
        System.out.println(c2 == c3);
        
        Class c4 = Student.class; //②任意Java类型的class属性----获取当前类的字节码文件对象Class
        System.out.println(c4);
        System.out.println(c1 == c4);
        System.out.println(c2 == c4);
        System.out.println(c3 == c4);
    }
}

class Student {
    public static void get() {
        System.out.println("get student");
    }
}
```

### toString方法

Object的public String toString()；

返回对象的字符串表示形式。结果应该是一个简明扼要的表达，容易让人阅读。建议所有子类覆盖此方法。

大部分的常用类或者后面的集合都会重写Object类的toString()

```java
public class ObjectDemo {
    public static void main(String[] args) {

        //通过有参构造方法创建一个学生对象
        Student s = new Student("高圆圆",42) ;
        //直接输出对象名称 --->会默认调用它的父类:Object的toString方法
        System.out.println(s);//com.qf.object_06.Student@1540e19d
        System.out.println("-----------------------------------");

		/*
          Object类的toString方法的原码
          public String toString() {
                  return getClass().getName() + "@" + Integer.toHexString(hashCode());
          }
         
          Integer类:int类型的包装类类型
          public static String toHexString(int i) :将整数转换成16进制数据结果以String类型展示
         */
        						        /*System.out.println(s.getClass().getName()+"@"+Integer.toHexString(s.hashCode()));
        System.out.println(s.toString());*/
        System.out.println(s.toString());
        
    }
}
```

### equals方法&hashCode方法

Object类的equals方法

```java
public boolean equals(Object obj){}//判断当前obj对象是否和当前对象想等
```

Object类中的equals的源码:

```java
public boolean equals(Object obj) {
         return (this == obj);  return s1 == s2 ;  // ==:引用类型:比较的是地址值是否相同
}
```

####  面试题

 equals和==的区别?

>  ==:   连接的基本数据类型,比较的是数据值否相同
>
>  ==:   连接的是引用类型,比较的是地址值是否相同
>
>  equals方法:如果使用Object默认的:底层用==,默认比较的还是两个对象的地址值是否相同,
>
> ```java
>          Student s1  = new Student("文章",35) ;
>          Student s2  = new Student("文章",35) ;
> ```
>
> s1和s2虽然地址值不同,他们的成员的内容相同,认为他是同一个人,但是如何让s1.equals(s2)为true:针对equals来说比较的是成员信息内容是否相同;
>
> 重写Object的equals方法同时还需要重写hashCode，内容相同，还需要比较哈希码值相同
>
> 重写之后，就比较的是成员信息的内容是否相同!

Object类的hashCode方法

```java
public int hashCode()://(了解)获取对象的一个哈希码值 (本质不是地址值,可以把它理解为地址值)----跟哈希表有关系(HashMap)
```

一般情况：不同的对象获取的哈希码值是不同的 ,(但是中文字符,可能内容不一样,但是哈希码值不同!)

```java
public class ObjectDemo {
    public static void main(String[] args) {
        int a = 10 ;
        int b = 15 ;
        System.out.println(a==b);//==链接的基本数据类型:比较的是数据值否相同
        System.out.println("--------------------------------------------");

        Student s1 = new Student("文章",35) ;
        System.out.println(s1);
        Student s2 = new Student("文章",35) ;
        System.out.println(s2);
        System.out.println(s1 == s2);

        System.out.println("--------------------------------------");

        //public boolean equals(Object obj)//Object obj = new Student() ;多态
        System.out.println(s1.equals(s2));
        //没有重写之前:执行的是Object类的equals()
        //重写之后,就比较的是对象的成员信息是否一致!

        System.out.println(s1.hashCode());
        System.out.println(s2.hashCode());
        System.out.println("文章".hashCode());
        System.out.println("---------------------------------------------");

//      String类型(大部分常用类都会重写equals)重写Object的equals方法,所以比较内容是否相同
        String str1 = "hello" ;
        String str2 = "hello" ;
        System.out.println(str1.equals(str2));
        String str3 = "world" ;
        String str4  = new String("world") ;

        System.out.println(str3.equals(str4));

    }
}
```

### clone方法

```java
protected Object clone() throws CloneNotSupportedException;
//创建对象并返回该对象的副本,这个方法会抛出一个异常,throws:表示的是可能出现异常,针对调用者必须进行处理
```

要使用clone方法,当前某个对象所在的类必须实现"标记接口"Cloneable(没有字段(成员变量),也没有成员方法)

实现这个接口,那么就可以使用Object的clone()方法

```java
public class ObjectDemo {
    public static void main(String[] args) throws CloneNotSupportedException{
        
        //创建学生对象
        Student s1 = new Student("高圆圆",42) ;
        System.out.println(s1);
        System.out.println(s1.getName()+"---"+s1.getAge());

        System.out.println("-----------------------------------");
        //调用克隆方法clone()
        Object obj = s1.clone(); //浅克隆:将s1地址值赋值给Objet
        //向下转型
        Student s2 = (Student) obj;
        System.out.println(s2);
        System.out.println(s2.getName()+"---"+s2.getAge());

        System.out.println("-----------------------------------");
        //传统方式
        Student s3 = s1 ;
        System.out.println(s3.getName()+"---"+s3.getAge());

    }
}
```

### Scanner类

Scanner类:文本扫描器    java.util.Scaner ;

构造方法:

```java
public Scanner(InputStream source);//创建一个文本扫描器

//形式参数是一个抽象类，在System类中
public final static InputStream in = null;

//本地方法(非Java语言实现)
private static native void setIn0(InputStream in);//底层方法一定会创建系统资源---->读取用户输入的字符(整数,字符串...)
```

  Scanner类提供判断功能:防止出现输入的类型和结果类型不匹配!

```java
	public boolean hasNextXXX():判断下一个录入的是否为指定的XXX类型
	XXX nextXXX() 获取功能
eg.
	public boolean hasNextInt()
	int nextInt()
```

 如果先录入int，在录入String，录入的字符串数据被漏掉

> 解决方案;
> 1)直接使用next()；
> 2)在使用nextLine()之前,在创建Scanner对象即可。

eg.拥有登录功能的猜数字游戏：

```java
import java.util.Scanner;

public class Game {
    public static void main(String[] args) {

        Game game = new Game();
        game.Start();

    }

    //登录
    public void Start() {

        String Username = "admin";
        String Password = "admin";

        int count = 3;

        while (count > 0) {

            Scanner scanner = new Scanner(System.in);
            System.out.println("请输入账号：");
            String name = scanner.nextLine();
            System.out.println("请输入密码");
            String pwd = scanner.nextLine();

            if (name.equals(Username) && pwd.equals(Password)) {
                System.out.println("登录成功！");
                guess();
                break;
            } else {
                System.out.println("您输入的账号或密码有误，请重新输入。");
                System.out.println("您还有" + count + "次机会");
                count--;
            }

        }

    }

    //猜数字
    public void guess() {
        int b = (int) (Math.random() * 100 + 1);

        Scanner scanner = new Scanner(System.in);

        int count = 0;

        while (true) {

            System.out.println("请输入一个数字：");
            int a = scanner.nextInt();

            if (a > b) {
                System.out.println("猜大了");
            } else if (a < b) {
                System.out.println("猜小了");
            } else {
                System.out.println("猜对了！");
                System.out.println("用了"+(count+1)+"次就猜对啦！");
                break;
            }
            count ++;

        }
    }

}
```

## String类

### 概念

字符串是一个常量,一旦被赋值了,其值(地址值)不能被更改

推荐的使用方式:

```java
String 变量名 = "xxxx" ;//xxxx代表 的当前String的实例
```

String类常用的功能:

```java
int length();//获取字符串长度
public String(byte[] bytes,字符集);//使用指定的字符集,将字节数组构造成一个字符串
public String(byte[] bytes,int offset,int length);//将指定的部分字节数组转换成字符串	参数1:字节数组对象,参数2;  指定的角标值;  参数3:指定长度
public String(char[] value);//将字符数组构造成一字符串
public String(char[] value,int offset,int count);//将部分字符数组转换成字符串
public String(String original)//构造一个字符串,参数为字符串常量
```

#### 面试题1:

> 在数组中有没有length方法？在String类中有没有length方法？在集合中有没有length方法？
>
> 数组中没有length方法，有length属性
>
> ```java
> int[] arr = new int[3] ;
> arr.length;
> ```
>
> String类中有length()
>
> 集合中没有length()，有size()获取元素数

  构造方法:

```java
public String();//空参构造:空字符序列
public String(byte[] bytes);//将一个字节数组构造成一个字符串,使用平台默认的字符集(utf-8:一个中文对应三个字节) 解码
//编码和解码---保证字符集统一
//编码:将一个能看懂的字符串--->字节               
//解码:将看不懂的字节---->字符串                  
```

eg.

```java
public class StringTest01 {
    public static void main(String[] args) throws UnsupportedEncodingException {

        String country = "中国";
        byte[] bytes = country.getBytes();
        System.out.println(Arrays.toString(bytes));

        String s = new String(country);
        System.out.println(s);

    }
}
```

运行结果

> [-28, -72, -83, -27, -101, -67]
> 中国

#### 面试题2：

> String s1 = "hello" ;
> String s2 = new String("hello") ;
> 在内存中分别创建了几个对象?
>
> 第一个创建了一个对象,直接在常量池创建,开辟常量池空间
> 第二个:创建了两个对象,一个堆内存中开辟空间,一个指向常量池(不推荐)

```java
public class StringTest02 {
    public static void main(String[] args) {

        String s1 = "hello" ;
        String s2 = new String("hello") ;
        System.out.println(s1 == s2);//false
        System.out.println(s1.equals(s2));  //true   只管内容相同

    }
}
```

####  == 和equals

 **==:连接引用类型比较的是地址值**

 **equals:默认比较地址值,但是String类重写equals方法,所以内容是否相同**

>  字符串变量相加,常量池中先开空间,再拼接
>
>  字符串常量相加,先相加,然后看结果是否在常量池中存在；如果存在,直接返回当前地址值,如果不存在,在常量池开辟空间!

### 转换功能（重点）

```java
byte[] getBytes() ;//将字符串转换成字节数组 (编码)
                   //如果方法为空参，使用平台默认的编码集进行编码(utf-8:一个中文对应三个字节)
byte[] getBytes(String charset);//使用指定的字符集进行编码
解码的过程:将看不懂的字节数----->String
String(byte[] bytes);//使用默认字符集进行解码
String(byte[] bytes,指定字符集)
```

编码和解码必须要保证字符集统一

字符集:

> ​         gbk :一个中文两个字节(中国中文编码表)
> ​         gb2312:gbk升级版(含义有一个中文字符:中国的中文编码表)
> ​         iso-8859-1:拉丁文码表
> ​         utf-8:任何的浏览器--->都支持 utf-8格式 (后期统一个)
> ​         unicode:国际编码表
> ​         JS:日本国际 电脑系统 一个字符集

Arrays静态功能:

```java
public static String toString(int/byte/float/double...[] a);//将任意类型的数组---->String
public char[] toCharArray();//将字符串转换成字符数组
public String toString();//返回自己本身---"当前字符串的内容"
public String toUpperCase();//将字符串转换成大写
public String toLowerCase();//将字符串转换成小写
```

#### 将字符串反转

```java
//键盘录入字符串,将字符串进行反转(使用功能改进)  String转换功能
import java.util.Scanner;

public class Test02_2 {
    public static void main(String[] args) {

        //创建键盘录入对象
        Scanner sc = new Scanner(System.in);

        //提示并录入数据
        System.out.println("请您输入一个字符串:");
        String line = sc.nextLine();

        //调用功能
        String result = reverse(line);
        System.out.println(result);
    }

    //定义字符串反转功能

    public static String reverse(String s) {

        //定义一个结果变量
        String result = "";
        //将字符串转换成char[]
        char[] chs = s.toCharArray(); //转换功能
        for (int x = chs.length - 1; x >= 0; x--) {
            result += chs[x];
        }
        return result;
    }
    
}
```

运行结果：

> 请您输入一个字符串:
> aeqwe]
> ]ewqea

### 判断功能

常用方法：

```java
public boolean equals(Object anObject):比较两个字符的内容是否相同 (区分大小写)
public boolean equalsIgnoreCase(String anotherString):比较两个字符串是否相同(不区分大小写)
public boolean startsWith(String prefix):判断字符串是否以指定的内容开头
public boolean endsWith(String suffix):判断字符串是否以指定的内容结尾

boolean isEmpty()  判断字符串是否为空 :若为空,则返回true;否则返回false

String s = "" ;// 空字符串 ,存在String对象 ""
String s = null ; 空值 (空对象) null:引用类型的默认值
```

### 获取功能(重点)

常用方法：

```java
int length();///获取字符串长度
public char charAt(int index);//获取指定索引处的字符
public String concat(String str);//将指定的字符串和当前字符串进行拼接,获取一个新的字符串
public int indexOf(int ch);//返回指定字符第一次出现的索引值
public int lastIndexOf(int ch);//返回值指定字符最后一次出现的索引值
public String[] split(String regex);//拆分功能:通过指定的格式将字符串---拆分字符串数组
public String substring(int beginIndex);//从指定位置开始默认截取到末尾，角标从0开始
public String substring(int beginIndex,int endIndex);//从指定位置开始,截取到位置结束(包前不包右)，只能取到endIndex-1处
public static String valueOf(boolean/int/long/float/double/char...Object b);//万能方法,将任意类型转换String类型
```

#### String数组的遍历

```java
public class StringTest {
    public static void main(String[] args) {
        
        //创建一个字符串
        String str = "helloJavaEE" ;
        //最原始的做法
        System.out.println(str.charAt(0));
        System.out.println(str.charAt(1));
        System.out.println(str.charAt(2));
        System.out.println(str.charAt(3));
        System.out.println(str.charAt(4));
        System.out.println(str.charAt(5));
        System.out.println(str.charAt(6));
        System.out.println(str.charAt(7));
        System.out.println(str.charAt(8));
        System.out.println(str.charAt(9));
        System.out.println(str.charAt(10));
        System.out.println("------------------------------");

        //使用String类的转换功能
        char[] chs = str.toCharArray(); //String---->字符数组toCharArray()--->char[]
        for(int x = 0 ; x < chs.length ; x ++){
            char ch = chs[x] ;
            System.out.println(ch);
        }
        
    }
}
```

## StringBuffer类

字符串缓冲区 ---->类似于String,但是不一样 (可变的字符序列)

StringBuffer的构造方法:

```java
public StringBuffer();//空参构造,创建一个空字符序列的字符串缓冲去  (推荐)
public StringBuffer(int capacity);//构造一个字符串缓冲区对象,指定容量大小
public StringBuffer(String str);//指定字符序列,长度加上初始容量16(总容量)
```

获取功能:

```java
public int length();//获取字符数(长度)
public int capacity();//获取字符串缓冲区容量
```

### 添加功能

```java
StringBuffer  append(任何类型) ;//将内容追加到字符串缓冲区中 (在字符串缓冲区的最后一个字符序列的末尾追加),返回值是字符串缓冲区本身...
public StringBuffer insert(int offset,String str);//插入:在指定位置处插入指定的内容
```

### 删除功能

```java
public StringBuffer deleteCharAt(int index);//删除指定索引处的缓冲区的字符序列,返回字符串缓冲区本身
public StringBuffer delete(int start,int end);//删除从指定位置到指定位置结束的字符序列(包含end-1处的字符),返回字符串缓冲区本身
```

### 类型的相互的转换（String和StringBuffer的转换）

```java
//String---->StringBuffer
//StringBuffer---->String
public class StringBufferDemo3 {
    public static void main(String[] args) {

        //String---->StringBuffer
        String s = "hello" ;
      
        //方式1:StringBuffer有参构造方法
        StringBuffer sb = new StringBuffer(s) ;
        System.out.println(sb);

        System.out.println("---------------------------");

        //方式2:StringBuffer的无参构造方法 结合append(String str)
        StringBuffer sb2 = new StringBuffer() ;
        sb2.append(s) ;//追加功能
        System.out.println(sb2);

        //StringBuffer---->String
        StringBuffer buffer = new StringBuffer("helloJavaee") ;//创建字符串缓冲区对象
        
        //方式1:String类型的构造方法
        //public String(StringBuffer buffer)
        String  str = new String(buffer) ;
        System.out.println(str);
        
        System.out.println("-------------------------------------------");

        //方式2:StringBuffer的public String toString()方法
        String str2 = buffer.toString();
        System.out.println(str2);

    }
}
```

### 字符反转

```java
import java.util.Scanner;

public class Test02 {

    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入一个字符串：");
        String s = scanner.nextLine();
        //转成StringBuffer类型
        StringBuilder s1 = new StringBuilder(s);
       
        StringBuilder s2 = s1.reverse();//反转
        System.out.println("反转后为：");
        System.out.println(s2);

    }

}
```

运行结果：

> 请输入一个字符串：
> sadhjghjkasds
> 反转后为：
> sdsakjhgjhdas

### 截取功能

```java
public String substring(int start):从指定位置开始,默认截取到末尾,返回值是新的字符串
public String substring(int start,int end):从指定位置开始到指定end-1结束进行截取,返回的新的字符串
```

### 替换功能

```java
StringBuffer的替换功能
public StringBuffer replace(int start,       //起始索引
                            int end,         //结束索引(end-1)
                            String str)      //替换的内容
```

### 面试题

#### 1、StringBuffer和数组的区别?

数组:

> ①只能存储同一种数据类型容器
> ②数组可以存储基本类型,也可以存储引用类型
>
> ③数组的最大特点:长度固定
> 静态初始化int[] arr = {元素1,元素2,元素3...} ;/动态初始化 int[] arr = new int[长度];
> String[] strArray = {"xx","xx","xx"} ;

StringBuffer:

> ①支持可变的字符序列
> ②里面存储可以存储任意类型的元素
>
> append(int/char/double/float/Obejct/String)
> isnert(int offert,int/char/double/float/Obejct/String)
>
> 一般情况:开发中 将StringBuffer----->String

#### 2、集合/数组的区别?(等会说)

 主体不同：

> 1、数组：是有序的元素序列。将有限个类型相同的变量的集合命名。
> 2、集合：具有某种特定性质的具体的或抽象的对象汇总而成的集体。

特点不同：

> 1、数组：数组中的所有元素都具有相同类型。数组中的元素存储在一个连续性的内存块中，并通过索引来访问。
> 2、集合：给定一个集合，任给一个元素，该元素或者属于或者不属于该集合，二者必居其一，不允许有模棱两可的情况出现。

规则不同：

> 1、数组：不给可初始化的数组赋初值，则全部元素均为0值。只能给元素逐个赋值，不能给数组整体赋值。
> 2、集合：集合中，每个元素的地位都是相同的，元素之间是无序的。集合上可以定义序关系，定义了序关系后，元素之间就可以按照序关系排序。

#### 3、StringBuffer,StringBuilder和String的区别?

> String：字符串是一个常量，一旦被赋值，其值不能更改/作为形式参数属于特殊的引用类型，形式参数的改变不会实际参数
>
> StringBuffer：可变的字符序列，线程安全的类，但执行效率低(线程角度)
>
> StringBuilder：可变的字符序列，和StringBuffer具有相互兼容的api，单线程程序中(只考虑执行效率，不考虑安全问题)

## Integer类

int类型的包装类类型(引用类型),包含了int类型的原始数据值

基本类型四类八种都会 对应的各自的引用类型

需要基本类型和String类型之间转换:需要中间桥梁(基本类型对应的包装类类型)

| 整数类型 | 引用类型引用类型(默认值都是null) |
| -------- | -------------------------------- |
| byte     | Byte                             |
| short    | Short                            |
| int      | Integer                          |
| long     | Long                             |
| 浮点类型 |                                  |
| float    | Float                            |
| double   | Double                           |
| 字符类型 |                                  |
| char     | Character                        |
| 布尔类型 |                                  |
| boolean  | Boolean                          |

需求:

```java
     public static String toBinaryString(int i);//将整数---->二进制 的字符串
     public static String toOctalString(int i);//将整数---->八进制的字符串
     public static String toHexString(int i);//将整数---->十六进制数据


     public static final int MAX_VALUE;//int的最大值
     public static final int MIN_VALUE;//int的最小值
```

### 构造方法

```java
Integer(int value):可以将int类型保证为Integer类型
Integer(String s) throws NumberForamtException:  抛出一个数字格式化异常
```

注意事项：当前字符串如果不是能够解析的整数的,就会出现数字格式化异常,s必须为 数字字符串

```java
public class IntegerDemo2 {
    public static void main(String[] args) {

        //创建Integer类对象
        int i = 100 ;
        Integer integer = new Integer(i) ;
        System.out.println(integer);
        
        //创建一个字符串
        String s  = "50" ;
        Integer integer2 = new Integer(s) ;
        System.out.println(integer2);
    }
}
```

### 自动拆装箱

```java
Integer x = 100; // x里面并不是保存100，保存的是100这个对象的内存地址。
Integer y = 100;
System.out.println(x == y); // true
 
Integer x = 128;
Integer y = 128;
System.out.println(x == y); // false
```
### int和String的转换

**int---->String**

```java
int i = 10;
//方法一
// Integer类的静态功能
String s1= Integer.toString(i);
//方法二
//底层使用的方法public static String toString(int i)
Integer integer1 = new Integer(i);
String s2 = integer1.toString();
```

**String--->int(使用居多)**

```java
String s3 = "10";
//方法一
//public static int parseInt(String s)throws NumberFormatException:数字字符串 (使用最多)
int i1 = Integer.parseInt(s3);//通过Integer的方法将s3转为int类型
//方法二
//Integer 的成员方法:int intValue()--->int
Integer integer2 = new Integer(s3);
int i2 = integer2.intValue();
```

### Integer的内部缓存区

Integer在内存中有一个内存缓存区，存放-128~127之间的数字

```java
Integer i1 = new Integer(127);
Integer i2 = new Integer(127);
System.out.println(i1 == i2);//false
System.out.println(i1.equals(i2));//true

Integer i3 = 127;
Integer i4 = new Integer(127);
System.out.println(i3 == i4);//false
System.out.println(i3.equals(i4));//true

Integer i5 = 128;
Integer i6 = new Integer(128);
System.out.println(i5 == i6);//false
System.out.println(i5.equals(i6));//true

Integer i7 = 127;
Integer i8 = 127;
System.out.println(i7 == i8);//true  IntegerCache:缓存区取出数据
System.out.println(i7.equals(i8));//true

Integer i9 = 128;
Integer i10 = 128;
System.out.println(i9 == i10);//false  return new Integer(128)
System.out.println(i9.equals(i10));//true
```

## Character类

Charcater :char类型的包装类类型,

### 构造方法

```java
public Character(char value)
```

主要功能

```java
public static boolean isUpperCase(char ch):判断当前字符是否大写字母字符
public static boolean isLowerCAse(char ch):是否为小写字母字符
public static boolean isDigit(char ch):是否为数字字符

//String转换的功能很类似
public static char toLowerCase(char ch):将字符转换成小写
public static char toUpperCase(char ch):将字符转换成大写
```

eg.键盘录入一个字符串，有大写字母字符，数字字母字符，小写字母字符，不考虑其他字符，分别统计大写字母字符，小写字母字符,数字字符的个数！

```java
public class Test01 {

    public static void main(String[] args) {

        String string = "Hello123World";

        Method1(string);
        Method(string);

    }

	//用哈希码
    public static void Method1(String string) {
        byte arr[] = string.getBytes();

        int a = 0;
        int b = 0;
        int c = 0;

        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > 65 && arr[i] < 90) {
                a++;
            } else if (arr[i] > 97 && arr[i] < 122) {
                b++;
            } else {
                c++;
            }
        }

        System.out.println("大写字符：" + a + ",小写字符：" + b + ",数字：" + c);

    }

	//用方法
    public static void Method(String string) {

        char[] arr = string.toCharArray();

        int a = 0;
        int b = 0;
        int c = 0;

        for (int i = 0; i < arr.length; i++) {
            Character character = new Character(arr[i]);
            // Character 的方法
            if (Character.isUpperCase(character)) {//isUpperCase判断是否是大写字母
                a++;
            } else if (Character.isLowerCase(character)) {//isLowerCase判断是否是小写字母
                b++;
            } else {//isDigit判断是否是数字
                c++;
            }
        }

        System.out.println("大写字符：" + a + ",小写字符：" + b + ",数字：" + c);

    }

}
```

运行结果：

> 大写字符：2,小写字符：8,数字：3
> 大写字符：2,小写字符：8,数字：3

## Calendar类

提供一些诸如 获取年,月,月中的日期 等等字段值。属于抽象类，不能实例化

> 如果一个类没有抽象方法，这个类可不可以定义为抽象类？
>
> 可以，为了不能实例化，通过子类间接实例化

不能实例化：

```java
public static Calendar getInstance();//     静态功能,返回值是它自己本身
```

###  成员方法:

```java
public int get(int field):根据给定日历字段----获取日历字段的值(系统的日历)
public abstract void add(int field,int amount):给指定的日历字段,添加或者减去时间偏移量
                     参数1:日历字段
                     参数2:偏移量
```

eg.

```java
import java.util.Calendar;
import java.util.Scanner;

public class CalenderTest {
    public static void main(String[] args) {

        //创建日历类对象
        //获取系统的年月日
        Calendar calendar = Calendar.getInstance();

        //获取年
        //public static final int YEAR
        int year = calendar.get(Calendar.YEAR);

        //获取月:
        //public static final int MONTH:0-11之间
        int month = calendar.get(Calendar.MONTH);

        //获取月中的日期
        //public static final int DATE
        //public static final int DAY OF MONTH : 每个月的第一天1
        int date = calendar.get(Calendar.DATE);

        System.out.println("当前日历为:" + year + "年" + (month + 1) + "月" + date + "日");

        System.out.println("------------------------------------------------");
        
        //获取3年前的今天
        //给year设置偏移量
        //public abstract void add(int field,int amount);
        calendar.add(Calendar.YEAR, -3);
        //获取
        year = calendar.get(Calendar.YEAR);
        System.out.println("当前日历为:" + year + "年" + (month + 1) + "月" + date + "日");

        
        //5年后的十天前
        calendar.add(Calendar.YEAR, +5);
        calendar.add(Calendar.DAY_OF_YEAR, -10);

        year = calendar.get(Calendar.YEAR);
        month = calendar.get(Calendar.MONTH);
        date = calendar.get(Calendar.DATE);
        System.out.println("当前日历为:" + year + "年" + (month + 1) + "月" + date + "日");

        
        //需求:键盘录入一个年份,算出任意年份的2月份有多少天 (不考虑润月)
        Scanner sc =new Scanner(System.in);
        System.out.println("请输入年份");
        int year1 =sc.nextInt();

        //设置日历对象
        Calendar c = Calendar.getInstance();
        c.set(year1,2,1);//三月一号
        //把时间往前推移一天就是二月的最后一天
        c.add(Calendar.DATE,-1);
        System.out.println(c.get(Calendar.DATE));

    }
}
```

运行结果：

> 当前日历为:2021年7月30日
>
> 当前日历为:2018年7月30日
>
> 当前日历为:2023年7月20日
>
> 请输入年份
>
> 2000
>
> 29

## Date类

表示特定瞬间,精确到毫秒!
它还允许格式化和解析日期字符串

### 构造方法:

```java
public Date():当前系统时间格式
public Date(long date):参数为 时间毫秒值---->Date对象  (1970年1月1日...)
```

### Date<==>String  格式化过程

DateForamt：抽象类，提供具体的日期/格式化的子类:SimpleDateFormat
format(Date对象)--->String

#### SimpleDateFormat:构造函数

```java
public SimpleDateFormat():使用默认模式
public SimpleDateFormat(String pattern):使用指定的模式进行解析或者格式 (推荐)
```

参数:

| 模式 | 代码表示 |
| ---- | -------- |
| 年   | "yyyy"   |
| 月   | "MM"     |
| 日   | "dd"     |
| 小时 | "HH"     |
| 分   | "mm"     |
| 秒   | "ss"     |
| 毫秒 | “SSSS”   |

String：日期文本----->Date

解析过程

```java
public Date parse(String source)throws ParseException;
//如果解析的字符串的格式和 public SimpleDateFormat(String pattern)的参数模式不匹配的话,就会出现解析异常!
```

封装成功工具类DateUtils: 构造方法私有,功能都是静态
eg.

```java
public class DateTest {

    public static void main(String[] args) throws ParseException {

        Date date = new Date();

        System.out.println(date.toString());

        //负责日期格式化（把时间转成字符串）
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");

        String nowTimeStr = sdf.format(date);
        System.out.println(nowTimeStr);

        //把字符串转成时间
        String time = "2020-01-01 10:00:00 888";
        //注意：指定的日期和给的格式必须一致，不然会有异常
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        Date date1 = sdf2.parse(time);
        System.out.println(date1);

        //返回1970.1.1到当前时间的毫秒数
        long beginTime = System.currentTimeMillis();
        print();
        long endTime = System.currentTimeMillis();
        //计算以上方法运行所需的时间
        System.out.println("以上方法耗时"+(endTime-beginTime)+"毫秒");

    }

    //统计一个方法执行所需的时长
    public static void print(){
        for (int i = 0; i < 1000; i++) {
        }
    }

}
```

运行结果：

> Fri Jul 30 21:15:02 CST 2021
> 2021-07-30 21:15:02 240
> Wed Jan 01 10:00:00 CST 2020
> 以上方法耗时0毫秒

## Random类

伪随机数生成器

### 构造方法

```java
public Random(); //产生一个随机生成器对象,通过成员方法随机数每次没不一样的(推荐)
public Random(long seed) ;//参数为long类型的值(随机数流:种子),每次通过成员方法获取随机数产生的随机数相同的

获取随机数的成员方法
public int nextInt();//获取的值的范围是int类型的取值范围(-2的31次方到2的31次方-1)
public int nextInt(int n);//获取的0-n之间的数据 (不包含n)
```

产生随机数:

```java
/*Math类的random方法*/
public static double random();

/*Random类:也能够去使用*/
//无参构造方法  + 成员方法
public Random():+ public int nextInt(int n)
```

eg.

```java
public class RandomTest01 {

    public static void main(String[] args) {
        Random();
    }

    //生成五个不重复的随机数，重复的话重新生成，最终生成的5个随机数放到数组中
    public static void Random() {

        //建立List集合
        ArrayList<Integer> arrayList = new ArrayList<>(15);

        while (arrayList.size() < 15) {
            //创建随机数对象，且获取随机数
            int num = (new Random()).nextInt(31);

            //判断此随机数是否为偶数，且大于0
            if (num % 2 == 0 && num != 0) {
                //判断集合中有没有这个数字
                if (!arrayList.contains(num)) {
                    //将数字赋值到列表中
                    arrayList.add(num);
                }
            }

        }
        //遍历
        System.out.println(arrayList);

    }
}
```

运行结果：

[14, 10, 24, 2, 16, 6, 18, 22, 26, 8, 30, 4, 28, 20, 12]

## Math类

针对数学运算的工具类,提供了很多方法

```java
public static int abs(int  a):绝对值方法
public static double ceil(double a):向上取整
public static double floor(double a):向下取整
public static int max(int a,int b):获取最大值
public static int min(int a,int b):获取最小值
public static double pow(double a,double b):a的b次幂
public static double random():[0.0,1.0):随机数
public static long round(double a):四舍五入
public static double sqrt(double a):开平方根
```

Math类中的功能都是静态的,里面构造方法私有了！

> 一般情况:工具类中构造方法都是会私有化(自定义的工具类),提供对外静态的公共访问方法
> (Java设计模式:单例模式)

JDK5的静态导入特性,必须方法静态的(导入到方法的级别)

Math类的功能都是静态的,就可以使用静态导入

```java
import static 包名.类名.方法名;
```

```java
public class MathDemo {
    public static void main(String[] args) {
        //abs()
        System.out.println(Math.abs(-100));
        // public static double ceil(double a):向上取整
        System.out.println(Math.ceil(13.56));
       // public static double floor(double a):向下取整
        System.out.println(Math.floor(12.45));
        // public static int max(int a,int b):获取最大值
        System.out.println(Math.max(Math.max(10,30),50));//方法嵌套
        //public static double pow(double a,double b):a的b次幂
        System.out.println(Math.pow(2,3));
        //ublic static long round(double a):四舍五入
        System.out.println(Math.round(13.78));

        //  public static double sqrt(double a):开平方根
        System.out.println(Math.sqrt(4));

    }
}
```

运行结果：

> 100
> 14.0
> 12.0
> 50
> 8.0
> 14
> 2.0

## BigDecimal类

小数要进行精确计算，还可以计算的同时保留小数点后的有效位数

### 构造方法

```java
public BigDecimal(String value):数字字符串
```

### 成员方法:

```java
public BigDecimal add(BigDecimal augend)加
public BigDecimal subtract(BigDecimal subtrahend)减
public BigDecimal multiply(BigDecimal multiplicand)乘
public BigDecimal divide(BigDecimal divisor):除
public BigDecimal divide(BigDecimal divisor,int scale,int roundingMode)
          参数1:商
          参数2:小数点后保留的有效位数
          参数3:舍入模式 :四舍五入
```

eg.

```java
public class Test{
    public static void main(String[] args) {
        //System.out.println(1.01 / 0.36);

        //创建BigDecimal对象
        BigDecimal bg1 = new BigDecimal("1.01") ;
        BigDecimal bg2 = new BigDecimal("0.36") ;

        BigDecimal bg3 = new BigDecimal("10.0") ;
        BigDecimal bg4 = new BigDecimal("5.05") ;

        System.out.println(bg1.add(bg2));
        System.out.println(bg1.subtract(bg2));
        System.out.println(bg1.multiply(bg2));
        // System.out.println(bg3.divide(bg4));//不保留(整除)
        System.out.println(bg3.divide(bg4,3,BigDecimal.ROUND_HALF_UP));//四十五入

    }
}
```

运行结果：

> 1.37
> 0.65
> 0.3636
> 1.980

# 集合

数组其实就是一个集合。集合实际上就是一个容器。可以来容纳其它类型的数据。

集合和数组有什么区别?

> 1)长度区别
>      数组:长度固定
>      集合:长度可变
>
> 2)存储数据类型的区别
>      数组:可以存储基本数据类型,也可以存储引用数据类型
>      集合:前提条件:集合中加入泛型<>
>      		 也是在模拟数组的特点:只能存储引用类型  Collection<E> :泛型<引用类型>
>
> 3)存储元素的区别:
>       数组:存储的元素必须为同一种数据类型
>       集合:如果没有加入泛型     :就出任意类型的元素(必须引用类型)

## 集合框架

### 基本功能:

```java
添加
boolean add(Object e):添加元素  E(Element)

删除:
void clear() 暴力删除(将集合的素有元素全部干掉)
boolean remove(Object o):从集合中删除指定的元素
获取集合的元素数 :int size()

判断功能:
boolean isEmpty():判断集合是否为空,为空元素,则返回true
boolean contains(Object o):判断集合中是否包含指定元素,包含则返回true
```

### Collection接口：

![图片1](C:\Users\幻想家懒得想了\Desktop\图片1.jpg)

### Map接口

![图片2](C:\Users\幻想家懒得想了\Desktop\图片2.jpg)

## Collection集合的高级功能

```java
boolean addAll(Collection c);//添加一个集合中的所有元素
boolean containsAll(Collection c);//包含一个集合中的所有元素
boolean removeAll(Collection c);//删除集合中的所有元素, (删除一个算删除,还是删除所有)
boolean retainAll(Collection c);//A集合对B集合求交集, boolean的返回值是什么意思,交集的元素是保存在A中还是B中

//Collection最基本的遍历功能,不属于集合的专有遍历
Object[] toArray();//将集合转换成了对象数组
```

### 迭代器

Collection的迭代器:集合的专有遍历方式

```java
Iterator iterator();//返回值类型接口类型,需要返回的子实现类对象

//Iterator接口:
boolean hasNext();//判断迭代器中是否存在下一个元素
Object next();//获取下一个可以遍历的元素
//给Collection中存储String类型,遍历出来
```

eg,

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
import java.util.Scanner;

public class Test01 {

    public static void main(String[] args) {

        Collection<Student> collection = new ArrayList<Student>();

        collection.add(new Student("老王",34));
        collection.add(new Student("老张",41));
        collection.add(new Student("老梁",52));
        collection.add(new Student("老刘",35));
        collection.add(new Student("老李",61));

        for (Object object : collection) {
            Student s = (Student) object;
            System.out.println(s.toString() + "学生");
            System.out.println(object.toString() + "toString");

            /*
            Iterator<Student> iterator = collection.iterator();
            while(iterator.hasNext()){
            Object object = iterator.next();
            Student s = (Student)object;
            System.out.println(s.toString()+"学生");
            System.out.println(object.toString()+"toString");
             */
        }

    }

}

class Student{

    String name;
    int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

运行结果：

> Student{name='老王', age=34}学生
> Student{name='老王', age=34}toString
> Student{name='老张', age=41}学生
> Student{name='老张', age=41}toString
> Student{name='老梁', age=52}学生
> Student{name='老梁', age=52}toString
> Student{name='老刘', age=35}学生
> Student{name='老刘', age=35}toString
> Student{name='老李', age=61}学生
> Student{name='老李', age=61}toString

#### 迭代器原理

> 内部具备一个指针，当在第一次获取元素的时候,指针处在第一个元素前面的位置，不指向任何元素，当通过hasNext()判断，如果存在下一个可以迭代的元素，那么指针就会向下一个索引进行移动，并且next()获取前面的元素，依次这样操作，直到hasNext()为false的时候，没有元素了，指针就不会在移动；Iterator是一个接口，具体的移动的操作是通过ArrayList的成员内部类Itr来进行实现;

## 泛型

格式：

```javjavaa
集合类型<引用数据类型>  集合对象名 = new  子实现类<引用数据类型>() ;
```

泛型的好处:

> 1)将运行时期异常提前了编译时期
>
> 2)避免了强制类型转换
>
> 3)提高了程序安全性

eg.使用Collection集合存储5个学生,加入泛型 Collection<Student>,使用迭代器遍历并获取

```java
public class GenericDemo {
    public static void main(String[] args) {

        //创建Collection集合对象
        Collection<String> c = new ArrayList<String>() ; //new XXX<数据类型>: jdk7以后泛型推断

        c.add("hello") ;
        c.add("高圆圆") ;
        c.add("你好吗") ;

        //获取迭代器Iteratr<E>是集合中存储的泛型是一致的
        Iterator<String> it = c.iterator();
        while(it.hasNext()){
            //获取String字符串的同时,还要获取长度
            String str = it.next();
            System.out.println(str+"---"+str.length());
        }
    }
}
```

## Collection集合

###  特点

> 有序(存储元素和取出元素一致)
> 允许元素重复

###   具备Collection相关的功能

```java
Object [] toArray()
Iterator iterator()
```

###  特有功能

```java
void add(int index,Object element):在指定的索引处插 入元素
Object get(int index):获取指定位置处的元素  + int size():一种新的集合遍历方式
Object remove(int index):删除指定位置处的元素
Object set(int index,E element):修改指定位置处的元素(替换)

ListIterator<E> listIterator():列表迭代器
ListIterator接口:
void add(E e)有添加
remove():有删除
```

### 集合的遍历

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.ListIterator;

public class Test01 {

    public static void main(String[] args) {

        List<Person> list = new ArrayList<Person>();

        list.add(new Person("老大", 12));
        list.add(new Person("老二", 11));
        list.add(new Person("老三", 10));

        //①toArray
        Object[] objects = list.toArray();
        for (int i = 0; i < objects.length; i++) {
            Person person = (Person) objects[i];
            System.out.println(person.toString());
        }

        System.out.println("————————");

        //②get/set
        Object[] objects1 = list.toArray();
        for (int i = 0; i < objects1.length; i++) {
            Person person = (Person) objects[i];
            System.out.println(person.getName()+"----"+person.getAge());
        }

        System.out.println("————————");

        //③迭代器
        Iterator<Person> iterator = list.iterator();
        while (iterator.hasNext()) {
            Person p = iterator.next();
            System.out.println(p.toString());
        }

        System.out.println("————————");

        //④list迭代器
        ListIterator<Person> iterator1 = list.listIterator();
        while (iterator1.hasNext()) {
            Person p = iterator1.next();
            System.out.println(p.toString());
        }

        System.out.println("————————");

        while (iterator1.hasPrevious()) {
            Person p = iterator1.previous();
            System.out.println(p);
        }

        System.out.println("————————");

        //⑤增强for
        for (Person p : list) {
            System.out.println(p);
        }

    }

}

class Person {

    String name;
    int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

运行结果：

> Person{name='老大', age=12}
> Person{name='老二', age=11}
> Person{name='老三', age=10}
> ————————
> 老大----12
> 老二----11
> 老三----10
> ————————
> Person{name='老大', age=12}
> Person{name='老二', age=11}
> Person{name='老三', age=10}
> ————————
> Person{name='老大', age=12}
> Person{name='老二', age=11}
> Person{name='老三', age=10}
> ————————
> Person{name='老三', age=10}
> Person{name='老二', age=11}
> Person{name='老大', age=12}
> ————————
> Person{name='老大', age=12}
> Person{name='老二', age=11}
> Person{name='老三', age=10}

### 并发修改异常

通过List集合ArrayList集合对象,如果当前集合中存在"world"元素,那么就给集合中添加一个"javaEE"元素,最后将集合中的所有元素进行遍历！

#### 问题描述

当集合的元素正在被迭代器进行遍历,那么集合对象是不能够对元素进行增加或者删除 (一个线程正在遍历,一个线程在修改元素)

#### 解决方案

> 1)要么就是迭代器去遍历集合的元素,迭代器去添加元素 :列表迭代器才具备添加的动作
> 2)要么集合遍历,集合添加

eg.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;

public class Test02 {

    public static void main(String[] args) {

        List<String> list = new ArrayList<String>();

        list.add("helo");
        list.add("world");
        list.add("java");

        /*
        Iterator<String> iterator = list.iterator();
        while(iterator.hasNext()){
            String s = iterator.next();//java.util.ConcurrentModificationException并发修改异常
            if("world".equals(s)){
                list.add("javaee");
            }
            System.out.println(list);
        }
        会报错
        [helo, world, java]
        [helo, world, java, javaee]
        Exception in thread "main" java.util.ConcurrentModificationException
       at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:901)
       at java.util.ArrayList$Itr.next(ArrayList.java:851)
       at HemoWork.day19.ClassTest.ListTest.Test02.main(Test02.java:19)
         */

        //解决方法1
        //用迭代器添加
        ListIterator<String> it = list.listIterator();
        while (it.hasNext()) {
            String s = it.next();
            if ("world".equals(s)) {
                //迭代器添加
                it.add("javaee");
            }

        }
        System.out.println(list);

        System.out.println("-------------");

        //解决方法2
        //用集合添加（只能加到末尾
        for (int i = 0; i < list.size(); i++) {
            String string = list.get(i);
            if("world".equals(string)){
                list.add("javaee");
            }

        }
        System.out.println(list);
    }

}
```

运行结果：

> [helo, world, javaee, java]
>
> [helo, world, javaee, java, javaee]

#### foreach

JDK5以后 提供了增强for循环,替代集合中迭代器去遍历集合使用的(优先在集合中使用)

##### 格式:

```java
 for(存储的引用数据类型 变量名: 集合/数组对象){ //集合使用居多,数组一般都是使用普通for
    使用变量名即可
 }
```

##### 注意事项:

当前集合对象不能为空 null :foreach语句;

增强for它本身就是获取迭代器了，就会出现空指针异常

### cantions方法

List集合如何去重?

eg.1

```java
import java.util.ArrayList;
import java.util.List;

public class Test03 {

    public static void main(String[] args) {

        List<String> list = new ArrayList();

        list.add("hello");
        list.add("hello");
        list.add("hello");
        list.add("hello1");
        list.add("hello");
        list.add("hello3");
        list.add("hello");
        list.add("hello");
        list.add("hello5");
        list.add("hello");

        //去重
        //方法一：新建一个空集合，
        List<String> newlist = new ArrayList<>();

        for (String s : list) {
            if (!newlist.contains(s)) {
                newlist.add(s);
            }
        }

        for (String s2 : newlist) {
            System.out.println(s2);
        }

        list.add("hello");
        list.add("hello");
        list.add("hello");
        list.add("hello1");
        list.add("hello");
        list.add("hello3");
        list.add("hello");
        list.add("hello");
        list.add("hello5");
        list.add("hello");

        System.out.println("——————————");

        //方法二：利用选择排序的思想
        //两辆比较
        for (int i = 0; i < list.size(); i++) {
            for (int j = i + 1; j < list.size(); j++) {
                if (list.get(i).contains(list.get(j))) {
                    list.remove(j);
                    j--;
                }
            }
        }

        for (String s2 : list) {
            System.out.println(s2);
        }

    }

}
```

运算结果：

> hello
> hello1
> hello3
> hello5
> ——————————
> hello
> hello1
> hello3
> hello5

eg.2

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Objects;

public class Test04 {

    public static void main(String[] args) {

        List<Student> list = new ArrayList();

        list.add(new Student("老大"));
        list.add(new Student("老大"));
        list.add(new Student("老二"));
        list.add(new Student("老大"));
        list.add(new Student("老三"));
        list.add(new Student("老大"));

        //新建集合
        List<Student> newList = new ArrayList<>();

        for (Student student : list) {
            if (!newList.contains(student)) {
                newList.add(student);
            }
        }

        for (Student s : newList) {
            System.out.println(s);
        }

        System.out.println("————————————");

        //选择排序思想
        list.add(new Student("老大"));
        list.add(new Student("老大"));
        list.add(new Student("老二"));
        list.add(new Student("老大"));
        list.add(new Student("老三"));
        list.add(new Student("老大"));

        for (int i = 0; i < list.size()-1; i++) {
            for (int j = i+1; j < list.size(); j++) {
                if(list.get(i).getName().contains(list.get(j).getName())){
                    list.remove(j);
                    j--;
                }
            }
        }

        for (Student s : list){
            System.out.println(s);
        }

    }

}

class Student {

    String name;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Student student = (Student) o;

        return name.equals(student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

运行结果：

> Student{name='老大'}
> Student{name='老二'}
> Student{name='老三'}
> ————————————
> Student{name='老大'}
> Student{name='老二'}
> Student{name='老三'}

### List集合子实现类的特点

#### ArrayList

> ArrayList集合初始化容量10
>
> 扩容为原容量1.5倍。
>
> 底层是数组，查询快,增删慢
>
> 另外要注意：ArrayList集合末尾增删元素效率还是可以的。
>
> 线程角度:  线程不安全的类,实现不同步的 ----->执行效率高

#### Vector

> Vector初始化容量是10.
>
> 扩容为原容量的2倍。
>
> 底层是数组，查询快,增删慢
>
> Vector底层是线程安全的。
>
> 线程角度:线程安全的类----同步的方法---->执行效率低
>
> 怎么得到一个线程安全的List：
> Collections.synchronizedList(list);
>
> 单线程程序中.考虑集合默认都会使用 ArrayList,多线程环境集合---->Vector集合

#### LinkedList

> 底层数据结构是链表,查询慢,增删快
>
> 线程角度:线程不安全的类---->不同步---->执行效率高
>
> 特有功能:
>
> ```java
> addFirst()
> removeFirst()
> getFirst()
> ```
>
> xxxLast()
> 应用场景：模拟栈结构特点，先进后出

     如果没有明确要求使用什么(List)集合的时候 ,默认都是用ArrayList

### Vector类

#### 特有功能:

```java
public void addElement(Object obj);//在vector对象的末尾添加元素  ------> 一直使用的add(Object e)
删除
public boolean removeElement(Object obj);//删除元素
获取功能
public Object elementAt(int index);//获取指定位置的元素---->类似于  public Object get(int index)
public Enumeration<E> elements();//Vector集合的专有遍历方式---->类似于   Iterator literator();接口
boolean hasMoreElements();//判断是否有更多的元素可以迭代
Object nextElement();//获取元素
```

eg.

```java
import java.util.ArrayList;
import java.util.List;

public class Test01 {

    public static void main(String[] args) {
        //创建List集合对象
        List<String> list = new ArrayList<String >();

        //添加元素
        list.add("hello") ;
        list.add("hello") ;
        list.add("world") ;
        list.add("world") ;
        list.add("world") ;
        list.add("javaEE") ;
        list.add("javaEE") ;

        // void add(int index,Object element):在指定的索引处插 入元素
        list.add(1,"高圆圆");
        
        //Object get(int index):获取指定位置处的元素 :返回的被获取到的元素内容
        System.out.println(list.get(1));

        //  Object remove(int index):删除指定位置处的元素,返回被删除的元素
        System.out.println(list.remove(2));

        //Object set(int index,E element):修改指定位置处的元素(替换)
        System.out.println(list.set(1,"赵又廷"));
        System.out.println(list);
    }

}
```

### 直接插入排序

核心思想：使用1角标对应的元素进行和0角标比较，如果前面元素大，向右移动,确定角标1对应的元素的位置，再次使用2角标对应的元素依次和1和0都元素比较，依次这样比较。

```java
/*
Integer类:包含了int类型的值:
Integer实现自然排序(默认升序排序)
*/

public int compareTo(Integer anotherInteger) {
     return compare(this.value, anotherInteger.value);
}

public static int compare(int x, int y) {  //提高程序的执行效率
     return (x < y) ? -1 : ((x == y) ? 0 : 1);
}
```

eg.

```java
public class InsertSort {

    public static void main(String[] args) {

        int[] arr = {34, 8, 64, 51, 32, 21};

        System.out.println("排序前：");
        printArr(arr);

        System.out.println("排序后：");
        insertSort(arr);


    }

    private static void printArr(int[] arr) {

        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            if (i == arr.length - 1) {
                System.out.println(arr[i] + "]");
            } else {
                System.out.print(arr[i] + ",");
            }
        }

    }

    private static void insertSort(int[] arr) {

        //定义一个变量，记录当前角标的变化
        int index;

        //定义变量：P表示一个比较次数
        for (int p = 0; p < arr.length; p++) {

            //定义临时变量temp
            Integer temp = arr[p];//temp = 8;temp = 64

            //开始比较
            for (index = p; index > 0 && temp.compareTo(arr[index - 1]) < 0; index--) {
                // j=1 ; 1>0 &&  8 < 34   j-- : j= 0
                // j=2 ; 2>0 && 64 < 32
                //数据移动
                arr[index] = arr[index - 1];
            }

            //确定temp的位置
            arr[index] = temp;

        }

        printArr(arr);
    }

}
```

运行结果：

> 排序前：
> [34,8,64,51,32,21]
> 排序后：
> [8,21,32,34,51,64]

### LinkedList类

#### 集合特点：

线程不安全，执行效率较高，是个列表结构

#### 特有功能：

```java
public void addFirst(0bject e):在列表开头插入元素
pub1ic void addLast(0bject e):将元素追加到列表的末尾
public 0bject getFirst(): 获取列表的第一个元 素
public 0bject getLast():获取 列表的最后一 个元素
public 0bject removeFirst(): 删除列表的第个元素
public 0bject removeLast() :删除列表的最后一个元素

push压栈
pop弹栈
```

#### 利用Linkedlist模拟栈数据结构：

```java
import java.util.LinkedList;
//栈方法类
public class MyStack {

    private LinkedList<Object> link;

    //空参构造
    public MyStack() {
        link = new LinkedList<Object>();
    }

    public MyStack(LinkedList<Object> link) {
        this.link = link;
    }

    //添加方法
    public void add(Object o) {
        link.addFirst(o);
    }

    //获取获取元素
    public void get(){
        System.out.println(link.removeFirst());
    }

    //判断是否为空
    public void isEmpty(){
        if (link.isEmpty()){
            System.out.println("栈为空");
        }else {
            System.out.println("栈不为空");
        }

    }

    public LinkedList getLink() {
        return link;
    }

    public void setLink(LinkedList link) {
        this.link = link;
    }
}
```

```java
//工具类
public class Test01 {
    public static void main(String[] args) {

        //创建对象
        MyStack stack = new MyStack();

       	//压栈
        stack.add("sad");
        stack.add(123);
        stack.add(1.2234);
        stack.add(true);

        //判断是否为空
        stack.isEmpty();

        //弹栈
        stack.get();
        stack.get();
        stack.get();
        stack.get();

        //判断是否为空
        stack.isEmpty();

    }
}
```

运行结果：

> 栈不为空
> true
> 1.2234
> 123
> sad
> 栈为空

### HashSet类

底层数据结构是一个哈希表

线程不安全的类---->不同步---->执行效率高

#### Set集合特点：

无序(存储和取出不一致),能够保证元素唯一

#### JUC（java.util.concurrent)

JDK8以后，提供了juc(并发包:java.util.concurrent)：
ConcurrentHashMap<K,V>和HashMap的区别 ?

前者线程安全，后者线程不安全

#### hashCode方法及equals方法

String类型:String类型本身已经重写了hashCode()和equals,如果hashCode和equals()都相同,那么认为同一个元素,存储以前的值

#### 如果现在存储是自定义对象,如何保证元素唯一?

```java
HashSet<Student>

    Student s1 = new Student("高圆圆"42) ;
    Student s2 = new Student("高圆圆"42) ;

/*
    HashSet集合依赖于add方法---->HashMap的put方法

    首先要比较元素的哈希码值相同----->hash()就相同
    还要比较成员信息是否相同,对应存储自定的类必须要重写Object的equals方法

    Student的这个类,必须手动给出hashCode()和equals

    Hashset集合不能保证顺序迭代恒久不变!
*/
```

#### 应用场景:

在一些需求中,如果没有明确要求元素重复,那就可以使用hashSet,保证元素唯一!
类型:String,Integer,Long,....常用类都已经重写了hashCode和equals方法

### TreeSet类：

#### 集合特点：

无序性,元素唯一

底层依赖于TreeMap集合,  红黑树结构(也称为 "自平衡的二叉树结构"),可以实现Map的自然排序以及比较器排序取决于使用的构造方法

#### 构造方法:

```java
public TreeSet();//构造一个空的树,实现元素自然排序 (取决于存储的元素类型能否实现Comparable接口)
```

#### TreeSet能够实现两种排序:

自然排序:

```java
TreeSet<E>();//E类型必须实现Comparable接口,实现自然排序(实现的compareTo(T t))
```

比较强排序:

```java
public TreeSet(Comparator<? super E> comparator)
```

Comparator是一个接口类型

1)自定义一个类实现Comparator接口,重写compare方法

eg.

```java
import com.sun.istack.internal.NotNull;

import java.util.Objects;
import java.util.TreeSet;

public class TreeSetTest01 {

    public static void main(String[] args) {

        TreeSet<Student> set = new TreeSet<>();

        set.add(new Student("laoAAAAA", 14));
        set.add(new Student("laoA", 10));
        set.add(new Student("laoAA", 11));
        set.add(new Student("laoA", 11));
        set.add(new Student("laoAAAA", 13));
        set.add(new Student("laoAA", 12));
        set.add(new Student("laoAAA", 12));


        for (Student s : set) {
            System.out.println(s.toString());
        }

    }

}

class Student implements Comparable<Student> {

    String name;
    int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age &&
                name.equals(student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    //利用接口的子实现类完成比较
    public int compareTo(@NotNull Student s) {

        int num = this.age - s.age;
        int num2 = this.name.compareTo(s.name);
        int num3 = num2 == 0 ? num : num2;
        return num3;
    }
}
```

2)使用接口的匿名内部类(推荐)

eg.

```java
public class TreeSetTest02 {

    public static void main(String[] args) {

        //匿名内部类完成比较
        TreeSet<Student2> set = new TreeSet<>(new Comparator<Student2>() {
            @Override
            public int compare(Student2 s1, Student2 s2) {
                int num = s1.age - s2.age;
                int num2 = s1.name.compareTo(s2.name);
                int num3 = num2 == 0 ? num : num2;
                return num3;
            }
        });

        set.add(new Student2("laoAAAAA", 14));
        set.add(new Student2("laoA", 10));
        set.add(new Student2("laoAA", 11));
        set.add(new Student2("laoA", 11));
        set.add(new Student2("laoAAAA", 13));
        set.add(new Student2("laoAA", 12));
        set.add(new Student2("laoAAA", 12));


        for (Student2 s : set) {
            System.out.println(s.toString());
        }

    }

}


class Student2 {

    String name;
    int age;

    public Student2() {
    }

    public Student2(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student2 student = (Student2) o;
        return age == student.age &&
                name.equals(student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

}
```

####  泛型高级通配符(了解)

  <?>    :任意Java类型,包括Object

  <? super E> : 向上限定:E类型以及E父类

  <? extends E>: 向下限定:E以及它的子类

## Map集合

键映射到值的对象,Map集合可以多个值,但键必须唯一!

### Map和Collection集合的区别:

Collection:只能存储一种类型 Collection<E>

Map集合:可以两种类型的,键的类型,值的类型 Map<K,V> 

##### 遍历方式不同

Collection：5种方式

> 详见集合的遍历

Map：两种方式:

> 方式1:获取所有的K的集合(键的集合)
>            通过键获取值
>
> 方式2: 获取所有的键值对对象Map.Entry<K,V> ("结婚证")
>             通过键值对对象获取所有的键("结婚证男方")
>             通过键值对对象获取所有的值("结婚证女方")

##### 有内在联系:

TreeSet集合：Collection---->间接的使用到了TreeMap集合的put方法

HashSet阶：Collection---->间接使用到了HashMap的put方法

#### Map集合的功能:

```java
V put(K key,V value);//添加键值对元素

/*注意事项:
如果key是第一次添加,那么返回的结果为null
如果key是否重复添加,第二次添加,返回的上一次添加的键对应的值
*/
V remove(Object key);//删除指定的键,返回被删除键对应的值
void clear()

boolean containsKey(Object key);//是否包含指定的键 (使用居多)
boolean containsValue(Object value);//是否包含指定的值
```

### 高级功能(遍历):

方法1：

```java
Set<K> keySet();//获取当前Map集合中的所有的键的集合  (将所有的丈夫集中起来,找对应的妻子)
V get(Object key);//通过键获取值
```

方式2:

```java
//获取所有的结婚证  (键值对对象)
Set<Map.Entry<K,V>> entrySet();

//通过键值对象 获取键 /获取值(通过结婚证找男方/女方)
K getKey();
V getValue();
```

eg.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class MapTest {

    public static void main(String[] args) {

        //创建Map集合对象
        Map<String, String> map = new HashMap<>();

        //添加元素
        map.put("令狐冲", "东方不败");
        map.put("杨过", "小龙女");
        map.put("陈玄风", "梅超风");
        map.put("郭靖", "黄蓉");

        //方法1：
        // Set<K> keySet()  :获取当前Map集合中的所有的键的集合
        Set<String> keySet = map.keySet();
        //增强for遍历
        for (String key : keySet) {
            String value = map.get(key);
            System.out.println(key + "-" + value);
        }

        System.out.println("————————————————");

        //方式2:
        //Set<Map.Entry<K,V>> entrySet()
        Set<Map.Entry<String, String>> entrySet = map.entrySet();
        //增强for:遍历键值对对象获取到
        for (Map.Entry<String, String> entry : entrySet) {
            System.out.println(entry.getKey()+"-"+entry.getValue());
        }

    }

}
```

运行结果：

```java
令狐冲-东方不败
陈玄风-梅超风
杨过-小龙女
郭靖-黄蓉
————————————————
令狐冲-东方不败
陈玄风-梅超风
杨过-小龙女
郭靖-黄蓉
```

### Properties类

目前只需要掌握Properties属性类对象的相关方法即可。

Properties是一个Map集合，继承Hashtable，Properties的key和value都是String类型。

Properties被称为属性类对象。

Properties是线程安全的。

```java
public class PropertiesTest01 {
    public static void main(String[] args) {

        // 创建一个Properties对象
        Properties pro = new Properties();

        // 需要掌握Properties的两个方法，一个存，一个取。
        pro.setProperty("url", "jdbc:mysql://localhost:3306/bjpowernode");
        pro.setProperty("driver","com.mysql.jdbc.Driver");
        pro.setProperty("username", "root");
        pro.setProperty("password", "123");

        // 通过key获取value
        String url = pro.getProperty("url");
        String driver = pro.getProperty("driver");
        String username = pro.getProperty("username");
        String password = pro.getProperty("password");

        System.out.println(url);
        System.out.println(driver);
        System.out.println(username);
        System.out.println(password);

    }
}
```

## Collections工具类

针对集合操作工具类

### 提供静态功能

```java
public static <T extends Comparable<? super T>> void sort(List<T> list);//按照自然升序排序（针对List集合排序）
public static <T> void sort(List<T> list,Comparator<? super T> c);//按照比较器排序针对List集合
public static <T extends Object & Comparable<? super T>> T max(Collection<? extends T>;//获取当前自然顺序中List的最大值
public static <T extends Object & Comparable<? super T>> T min(Collection<? extends T>;//最小值
public static void reverse(List<?> list);//对List集合顺序反转
public static void shuffle(List<?> list);//随机置换
```

eg.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test01 {

    public static void main(String[] args) {

        //创建List集合
        List<Integer> list = new ArrayList<>() ;

        //添加元素
        list.add(10) ;
        list.add(50) ;
        list.add(15) ;
        list.add(25) ;
        list.add(5) ;
        list.add(12) ;

        System.out.println(list);//[10, 50, 15, 25, 5, 12]

        System.out.println("---------------------------------");

        //排序
        Collections.sort(list);
        System.out.println(list);//[5, 10, 12, 15, 25, 50]
        
        System.out.println("----------------------------------");

        //最大值
        Integer max = Collections.max(list);
        System.out.println(max);//50

        System.out.println("----------------------------------");

        //最小值
        System.out.println(Collections.min(list));//5

        System.out.println("-----------------------------------");

        //反转
        Collections.reverse(list);
        System.out.println(list);//[50, 25, 15, 12, 10, 5]

        System.out.println("------------------------------------");

        //乱序
        Collections.shuffle(list);
        System.out.println(list);//[10, 25, 15, 12, 50, 5]

    }

}
```

### 集合排序

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)：按照自然升序排序（针对List集合排序）
public static <T> void sort(List<T> list,Comparator<? super T> c)：按照比较器排序针对List集合

List<Student>：针对List进行自然升序排序，
```

eg.

> 主要条件：按照学生的年龄从小到大排
>
> 次要条件：年龄相同，比较姓名内容是否相同！

```java
public class CollectionsTest {
    public static void main(String[] args) {

        //创建List集合对象
        List<Student> list = new ArrayList<>() ;
        
        //添加
        list.add(new Student("gaogaoyuan",42)) ;
        list.add(new Student("gaogaoyuan",40)) ;
        list.add(new Student("liushishi",42)) ;
        list.add(new Student("wanglihong",45)) ;
        list.add(new Student("wenzhang",38)) ;
        list.add(new Student("huazi",35)) ;
        list.add(new Student("huazi",32)) ;
        list.add(new Student("zhangjunjie",20)) ;

        //排序
        //Collections.sort(list); //自然升序排序：针对集合当前存储的类型必须实现Comparable

        //使用比较器排序：针对List集合
        //public static <T> void sort(List<T> list,Comparator<? super T> c)：按照比较器排序针对List集合
        Collections.sort(list, new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int num  = s1.getAge() - s2.getAge() ;
                int num2 = (num==0)?(s1.getName().compareTo(s2.getName())):num ;
                return num2;
            }
        });

        for(Student s:list){
            System.out.println(s.getName()+"---"+s.getAge());
        }

    }
}
```

```java
import java.util.Comparator;

//实现接口
//public class Student implements Comparable<Student> {
public class Student {
    private String name ;
    private int age ;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

   /* @Override
    public int compareTo(Student s) {

        //主要条件：按照学生的年龄从小到大排序
        int num = this.age - s.age ;
        //如果年龄相同，姓名比（字典顺序）
        int num2 = (num==0)?(this.name.compareTo(s.name)):num ;
        return num2;
    }*/
    
}
```

## 集合总结

### ArrayList集合：

> 1、默认初始化容量10（底层先创建了一个长度为0的数组，当添加第一个元素的时候，初始化容量10。）
>
> 2、集合底层是一个Object[]数组。
>
> 3、构造方法：
>         new ArrayList();
>         new ArrayList(20);
>
> 4、ArrayList集合的扩容：
>         增长到原容量的1.5倍。
>         ArrayList集合底层是数组，怎么优化？
>             尽可能少的扩容。因为数组扩容效率比较低，建议在使用ArrayList集合
>             的时候预估计元素的个数，给定一个初始化容量。
>
> 5、数组优点：
>         检索效率比较高。（每个元素占用空间大小相同，内存地址是连续的，知道首元素内存地址，
>         然后知道下标，通过数学表达式计算出元素的内存地址，所以检索效率最高。）
>
> 6、数组缺点：
>         随机增删元素效率比较低。
>         另外数组无法存储大数据量。（很难找到一块非常巨大的连续的内存空间。）
>
> 7、向数组末尾添加元素，效率很高，不受影响。
>
> 8、面试官经常问的一个问题？
>         这么多的集合中，你用哪个集合最多？
>             答：ArrayList集合。
>             因为往数组末尾添加元素，效率不受影响。
>             另外，我们检索/查找某个元素的操作比较多。
>
> 9、ArrayList集合是非线程安全的。（不是线程安全的集合。）

### 泛型

> 1、JDK5.0之后推出的新特性：泛型
>
> 2、泛型这种语法机制，只在程序编译阶段起作用，只是给编译器参考的。（运行阶段泛型没用！）
>
> 3、使用了泛型好处是什么？
>     第一：集合中存储的元素类型统一了。
>     第二：从集合中取出的元素类型是泛型指定的类型，不需要进行大量的“向下转型”！
>
> 4、泛型的缺点是什么？
>     导致集合中存储的元素缺乏多样性！
>     大多数业务中，集合中元素的类型还是统一的。所以这种泛型特性被大家所认可。

### HashMap集合：

> 1、HashMap集合底层是哈希表/散列表的数据结构。
>
> 2、哈希表是一个怎样的数据结构呢？
>
> ​    	哈希表是一个数组和单向链表的结合体。
> ​    	数组：在查询方面效率很高，随机增删方面效率很低。
> ​    	单向链表：在随机增删方面效率较高，在查询方面效率很低。
> ​    	哈希表将以上的两种数据结构融合在一起，充分发挥它们各自的优点。
>
> 3、HashMap集合底层的源代码：
>
> ```java
>     public class HashMap{
>         // HashMap底层实际上就是一个数组。（一维数组）
>         Node<K,V>[] table;
>         // 静态的内部类HashMap.Node
>         static class Node<K,V> {
>             final int hash; // 哈希值（哈希值是key的hashCode()方法的执行结果。hash值通过哈希函数/算法，可以转换存储成数组的下标。）
>             final K key; // 存储到Map集合中的那个key
>             V value; // 存储到Map集合中的那个value
>             Node<K,V> next; // 下一个节点的内存地址。
>         }
>     }
> ```
>
>
> ​    哈希表/散列表：一维数组，这个数组中每一个元素是一个单向链表。（数组和链表的结合体。）
>
> 4、最主要掌握的是：
>
> ​		map.put(k,v)
> ​		v = map.get(k)
> ​		以上这两个方法的实现原理，是必须掌握的。
>
> ​		map.put(k,v)的实现原理：
> ​    		第一步:先将k,v封装到Node对象当中。
> ​    		第二步:底层会调用k的hashCode0方法得出hash值,然后通过哈希函数/哈希算法,将hash值转换成数组的下标，下标位置上如果没有任何元素,就把Node添加到这个位置上了。如果说下标对应的位置上有链表,此时会拿着k和链表上每一个节点中的k进行equals ,如果所有的equals方法返回都是false ,那么这个新节点将会被添加到链表的末尾。如果其中有一个equals返回了true，那么这个节点的value将会被覆盖。
>
> ​		v = map.get(k)的实现原理：
> ​    		先调用k的hashCode0方法得出哈希值,通过哈希算法转换成数组下标,通过数组下标快速定位到某个位置上,如果这个位置上什么也没有,返回null.如果这个位置上有单向链表,那么会拿着参数k和单向链表上的每个节点中的k进行equals ,如果所有equals方法返回false ,那么get方法返回null ,只要其中有一个节点的k和参数k equals的时候返回true ,那么此时这个节点的value就是我们要找的value , get方法最终返回这个要找的value。
>
> 5、HashMap集合的key部分特点：
>
> ​    无序，不可重复。
> ​    为什么无序？ 因为不一定挂到哪个单向链表上。
> ​    不可重复是怎么保证的？ equals方法来保证HashMap集合的key不可重复。
> ​    如果key重复了，value会
> ​    放在HashMap集合key部分的元素其实就是放到HashSet集合中了。
> ​    所以HashSet集合中的元素也需要同时重写hashCode()+equals()
>
> 6、哈希表HashMap使用不当时无法发挥性能！
>
> ​    假设将所有的hashCode()方法返回值固定为某个值，那么会导致底层哈希表变成了纯单向链表。这种情况我们成为：散列分布不均匀。
>
> ​    什么是散列分布均匀？
> ​        假设有100个元素，10个单向链表，那么每个单向链表上有10个节点，这是最好的，是散列分布均匀的。
>
> ​    假设将所有的hashCode()方法返回值都设定为不一样的值，可以吗，有什么问题？
> ​        不行，因为这样的话导致底层哈希表就成为一维数组了，没有链表的概念了。也是散列分布不均匀。
>
> ​    散列分布均匀需要你重写hashCode()方法时有一定的技巧。
>
> 7、重点：放在HashMap集合key部分的元素，以及放在HashSet集合中的元素，需要同时重写hashCode和equals方法。
>
> 8、HashMap集合的默认初始化容量是16，默认加载因子是0.75
>
> ​    这个默认加载因子是当HashMap集合底层数组的容量达到75%的时候，数组开始
> ​    重点，记住：HashMap集合初始化容量必须是2的倍数，这也是官方推荐的，
> ​    这是因为达到散列均匀，为了提高HashMap集合的存取效率，所必须的。

### java.util.Map接口中常用的方法

> ​    1、Map和Collection没有继承关系。
>
> ​    2、Map集合以key和value的方式存储数据：键值对
>
> ​        key和value都是引用数据类型。
> ​        key和value都是存储对象的内存地址。
> ​        key起到主导的地位，value是key的一个附属品。
>
> ​    3、Map接口中常用方法：
>
> ​        V put(K key, V value) 向Map集合中添加键值对
> ​        V get(Object key) 通过key获取value
> ​        void clear()    清空Map集合
> ​        boolean containsKey(Object key) 判断Map中是否包含某个key
> ​        boolean containsValue(Object value) 判断Map中是否包含某个value
> ​        boolean isEmpty()   判断Map集合中元素个数是否为0
> ​        V remove(Object key) 通过key删除键值对
> ​        int size() 获取Map集合中键值对的个数。
> ​        Collection<V> values() 获取Map集合中所有的value，返回一个Collection
>
> ​        Set<K> keySet() 获取Map集合所有的key（所有的键是一个set集合）

### HashMap集合补充

> 1、向Map集合中存，以及从Map集合中取，都是先调用key的hashCode方法，然后再调用equals方法！
>
> equals方法有可能调用，也有可能不调用。
>
> ​    拿put(k,v)举例，什么时候equals不会调用？
> ​        k.hashCode()方法返回哈希值，
> ​        哈希值经过哈希算法转换成数组下标。
> ​        数组下标位置上如果是null，equals不需要执行。
>
> ​    拿get(k)举例，什么时候equals不会调用？
>
> ​        k.hashCode()方法返回哈希值，
> ​        哈希值经过哈希算法转换成数组下标。
> ​        数组下标位置上如果是null，equals不需要执行。
>
> 2、注意：如果一个类的equals方法重写了，那么hashCode()方法必须重写。
>
> 并且equals方法返回如果是true，hashCode()方法返回的值必须一样。
>     equals方法返回true表示两个对象相同，在同一个单向链表上比较。
>     那么对于同一个单向链表上的节点来说，他们的哈希值都是相同的。
>     所以hashCode()方法的返回值也应该相同。
>
> 3、hashCode()方法和equals()方法不用研究了，直接使用IDEA工具生成，但是这两个方法需要同时生成。
>
> 4、终极结论：
>     放在HashMap集合key部分的，以及放在HashSet集合中的元素，需要同时重写hashCode方法和equals方法。
>
> 5、对于哈希表数据结构来说：
>
> ​    如果o1和o2的hash值相同，一定是放到同一个单向链表上。
> ​    当然如果o1和o2的hash值不同，但由于哈希算法执行结束之后转换的数组下标可能相同，此时会发生“哈希碰撞”。

### 扑克牌

eg.1

```
import java.util.ArrayList;
import java.util.Collections;

public class Test06 {

    public static void main(String[] args) {

        ArrayList<String> arrayList = new ArrayList<>();

        String[] colors = {"♥", "♦", "♠", "♣"};

        String[] numbers = {"A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"};

        for (String number : numbers) {
            for (String color : colors) {
                arrayList.add(color + number);

            }
        }
        arrayList.add("大王");
        arrayList.add("小王");

        //洗牌
        Collections.shuffle(arrayList);

        ArrayList<String> player1 = new ArrayList<>();
        ArrayList<String> player2 = new ArrayList<>();
        ArrayList<String> player3 = new ArrayList<>();
        ArrayList<String> dipai = new ArrayList<>();

        for (int i = 0; i < arrayList.size(); i++) {
            if (i <arrayList.size()-3) {
                if ((i) % 3 == 0) {
                    player1.add(arrayList.get(i));
                } else if ((i) % 3 == 1) {
                    player2.add(arrayList.get(i));
                } else{
                    player3.add(arrayList.get(i));
                }
            } else {
                dipai.add(arrayList.get(i));
            }
        }

        System.out.print("player1:");
        Sort(player1);

        System.out.print("player2:");
        Sort(player2);

        System.out.print("player3:");
        Sort(player3);

        System.out.print("dipai:");
        Sort(dipai);

    }

    public static void Sort(ArrayList<String> arrayList){

        for (String s:arrayList) {
            System.out.print(s+"\t");
        }
        System.out.println();
    }

}
```

运行结果：

> player1:♥6	♣10	♣J	♦5	♥2	♠J	♠3	♣7	♣6	♠8	♠Q	♥9	♣4	♠5	♦K	♥7	♦7	
> player2:♥5	♣8	♣3	♦4	♦3	♥10	♦J	♥3	♣A	♠K	♥4	♦6	大王	♠7	小王	♦2	♠A	
> player3:♦A	♣2	♣Q	♣9	♥A	♥8	♣K	♠6	♦Q	♦8	♠2	♠9	♠10	♦10	♣5	♥Q	♦9
> dipai:♥K	♠4	♥J	

eg.2

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.TreeSet;

public class Test06_2 {

    public static void main(String[] args) {

        HashMap<Integer,String> map = new HashMap<>();

        ArrayList<Integer> list = new ArrayList<>();

        String[] colors = {"♥", "♦", "♠", "♣"};

        String[] numbers = {"A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"};

        int index = 0;

        for (String number:numbers) {
            for (String color:colors) {
                map.put(index,color+number);
                list.add(index);
                index++;
            }
        }


        list.add(index);
        map.put(index++,"大王");
        list.add(index);
        map.put(index,"小王");

        //洗牌
        Collections.shuffle(list);

        TreeSet<Integer> player1= new TreeSet<>();
        TreeSet<Integer> player2= new TreeSet<>();
        TreeSet<Integer> player3= new TreeSet<>();
        TreeSet<Integer> dipai= new TreeSet<>();

        for (int i = 0; i < list.size(); i++) {
            if (i <list.size()-3) {
                if ((i) % 3 == 0) {
                    player1.add(list .get(i));
                } else if ((i) % 3 == 1) {
                    player2.add(list .get(i));
                } else{
                    player3.add(list .get(i));
                }
            } else {
                dipai.add(list .get(i));
            }
        }

        Sort("palyer1",player1,map);

        Sort("palyer2",player2,map);

        Sort("palyer3",player3,map);

        Sort("底牌",dipai,map);

    }

    public static void Sort(String name,TreeSet<Integer> set,HashMap<Integer,String> map ){

        System.out.print(name+"的牌是：");

        for (Integer i:set) {
            System.out.print(map.get(i)+"\t");
        }

        System.out.println();
    }

}
```

运行结果：

> palyer1的牌是：♦2	♠2	♦3	♠3	♣3	♦6	♣6	♥7	♦7	♥8	♠9	♦10	♥Q	♦Q	♣Q	♦K	♠K	
> palyer2的牌是：♥A	♠A	♥3	♠4	♣4	♦5	♠5	♣5	♣7	♦9	♣9	♠10	♦J	♠J	♣J	♣K	大王	
> palyer3的牌是：♦A	♣A	♥2	♣2	♥4	♦4	♥5	♥6	♠7	♠8	♥9	♥10	♣10	♥J	♠Q	♥K	小王	
> 底牌的牌是：♠6	♦8	♣8	

# System类

不能实例化

## 成员变量

```java
public static final InputStream in:标准输入流
public static final PrintStream out:标准输出流
public static final PrintStream err:错误输出流(打印错误信息/一些信息需要用户引起注意:相关的日志)
```

后期使用日志文件:log4j.properites

## 级别

> info:详情信息
> debug:断点调试模式
> error:错误信息
> warning:警告信息

```java
System.exit(0) :jvm退出 ,0表示正常终止
public static void gc():手动开启垃圾回收器,会去回收内存中没有更多引用的对象!
```

```java
public class SystemDemo {
    public static void main(String[] args) throws IOException {
        System.err.println("这是错误输出流");//这是错误输出流

		//字节输出流
        InputStream in = System.in ;
        //键盘录入的就可以用到Scanner     
        //BufferedRead 字符缓冲输入流:读数据
        //里面传递参数InputStream参数
        Scanner sc = new Scanner(in) ;
        System.out.println("helloworld");//helloworld

         //字节打印流
        PrintStream ps = System.out ;
        //println(任何数据类型):PrintStream流中的一些工具类
        ps.println(10);//10
        ps.println(12.56);//12.56

        //创建Person对象
        Person p = new Person("大毛",20) ;
        System.out.println(p.getName()+"-"+p.getAge());//大毛-20
        p = null ;

        //手动垃圾回收器(不需要手动开启垃圾回收器,自动回收)!
        System.gc() ;//gc正在去回收没有更多引用的对象Person{name='高圆圆', age=41}

        //protected void finalize() throws Throwable:需要通过jvm启动垃圾回收器回收没有更多引用的对象

       Runtime.getRuntime().exec("notepad") ;//打开记事本
       Runtime.getRuntime().exec("calc") ;//打开计算机

    }
}

//在Person类中重写
    @Override
    protected void finalize() throws Throwable {
        System.out.println("gc正在去回收没有更多引用的对象"+this);
        super.finalize();
    }
```



# 异常

## 概念

Throwable:包含所有的错误以及异常! 它是一个超类(父类)

### error:

非常严重问题  (跟代码没有太大有关系)

OOM Out Of Memory:内存溢出 (严重问题)

### Exception:

编译时期异常和运行时期异常(RuntimeException):程序在运行过程中出现问题(代码书写不严谨)

只要不是RuntimeException的子类都是属于编译时期异常

#### 运行时期异常:

> 一般程序员逻辑结构不严谨导致的问题,调用者可以进行显示处理(try...catch.../throws)
>
> 也可以不进行显示处理,通过逻辑语句进行处理!

#### 编译时期异常:

> 调用者必须显示处理,不处理,编译通过不了,程序运行不了

如果在当前方法中已经去捕获了try...catch...,调用者无序进行处理,
但是如果在方法中抛出异常的,调用者必须处理(捕获/抛出throws)

开发中:尽量优先使用try...catch异常,其次再是throws

> 编译时期异常:  在程序,运行前需要检查的!     在生活中   "长途旅行之前,检查你的车胎情况"...
>
> 运行时期异常:在程序.程序代码逻辑问题(代码不严谨)  在生活中  "no zuo no die"

## 处理异常的方法

#### 处理异常的第一种方式：

> ​    在方法声明的位置上使用throws关键字抛出，谁调用我这个方法，我就抛给谁。抛给调用者来处理。
>
> ​    这种处理异常的态度：上报。

#### 处理异常的第二种方式：

> ​    使用try..catch语句对异常进行捕捉。
>
> ​    这个异常不会上报，自己把这个事儿处理了。
>
> ​    异常抛到此处为止，不再上抛了。

#### 注意：

> 只要异常没有捕捉，采用上报的方式，此方法的后续代码不会执行。
>
> 另外需要注意，try语句块中的某一行出现异常，该行后面的代码不会执行。
>
> try..catch捕捉异常之后，后续代码可以执行。

#### 如何选择？

> 如果希望调用者来处理，选择throws上报。
>
> 其它情况使用捕捉的方式。
>
> ##### 1)有的时候没有办法去抛出,继承关系中,如果子类继承父类,父类的该方法没有异常,子类重写该方法的时候,只能try...catch
>
> ##### 2)子类继承父类,如果父类的该方法本身抛出异常了,那么子类重写该方法的时候,要么跟父类的方法的异常类名一致,要么是该异常的类子类!
>
> 一般不建议在main方法上使用throws，因为这个异常如果真正的发生了，一定会抛给JVM。JVM只有终止。
>
> 异常处理机制的作用就是增强程序的健壮性。怎么能做到，异常发生了也不影响程序的执行。所以一般main方法中的异常建议使用try..catch进行捕捉。main就不要继续上抛了。

## 面试题:

throws和throw的区别?

### 共同点:

都是抛出

### 用法不同: 

#### 1)使用位置不同

> throws:
>
> a)将异常抛出在方法声明上
> b)在方法名的后面可以跟多个异常类名,中间逗号隔开!
>
> throw
>
> a)在方法的语句体中某个逻辑语句中
> b)它后面只能跟异常对象,而不是类名

#### 2)调用者是否处理不同

> throws:调用者必须进行显示处理(try...catch/throws),否则报错
>
> throw:调用者无须显示处理,一般情况都是在逻辑语句进行处理

#### 3)出现异常是否肯定性

> throws:在方法上的,执行某个方法的代码中,可能有问题(表示出现异常的一种可能性)
>
> throw:执行某段代码一定会执行这个异常(表示出现异常的一种肯定性)

#### 4)

> throws---->将具体的处理交给jvm---通过jvm吧异常信息打印控制台上显示的底层源码而且会显示当前错误消息字符串
>
> throw---->程序 中某段代码有问题:只是打印异常类名(jvm处理)

## finally

不能单独使用,它是结合try...catch....finally:异常的标准格式

###  特点:

释放相关的资源,代码一定执行的

除非在执行finally,jvm退出了!、

### 关于try..catch中的finally子句：

> **1、在finally子句中的代码是最后执行的，并且是一定会执行的，即使try语句块中的代码出现了异常。**
>
> ​    finally子句必须和try一起出现，不能单独编写。
>
> **2、finally语句通常使用在哪些情况下呢？**
>
>    通常在finally语句块中完成资源的释放/关闭。
>
>    因为finally中的代码比较有保障。
>
>    即使try语句块中的代码出现异常，finally中代码也会正常执行。
>
> **3、退出JVM之后，finally语句中的代码就不执行了！**

eg.

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ExceptionFinally_Test01 {
    public static void main(String[] args) {
        FileInputStream fis = null; // 声明位置放到try外面。这样在finally中才能用。
        try {
            // 创建输入流对象
            fis = new FileInputStream("随便一个路径"); // 开始读文件....
           
            String s = null;
            // 这里一定会出现空指针异常！
            s.toString();
            System.out.println("hello world!");

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch(IOException e){
            e.printStackTrace();
        } catch(NullPointerException e) {
            e.printStackTrace();
        } finally {
            System.out.println("hello 浩克！");
            // 流的关闭放在这里比较保险。
            // finally中的代码是一定会执行的。
            // 即使try中出现了异常！
            if (fis != null) { // 避免空指针异常！
                try {
                    // close()方法有异常，采用捕捉的方式。
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        System.out.println("hello kitty!");

        try {
            System.out.println("try...");
            System.exit(0); // 退出JVM之后，finally语句中的代码就不执行了！
        } finally {
            System.out.println("finally...");
        }

    }

}
```

```
java.io.FileNotFoundException: 你输入的路径(系统找不到指定的路径。)
	at java.io.FileInputStream.open0(Native Method)
	at java.io.FileInputStream.open(FileInputStream.java:195)
	at java.io.FileInputStream.<init>(FileInputStream.java:138)
	at java.io.FileInputStream.<init>(FileInputStream.java:93)
	at com.xm.ExceptionTest.ExceptionFinally_Test01.main(ExceptionFinally_Test01.java:25)
hello 浩克！
hello kitty!
try...
```



## 异常对象的两个方法：

```java
String msg = e.getMessage();
e.printStackTrace(); // 一般都是使用这个。
```

如何查看异常的追踪信息，我们应该怎么看可以快速的调试程序呢？

> 异常信息追踪信息，从上往下一行一行看。
> 但是需要注意的是：SUN写的代码就不用看了(看包名就知道是自己的还是SUN的。)。
> **主要的问题是出现在自己编写的代码上。**

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class ExceptionTest02 {
    public static void main(String[] args) {
        try {
            m1();
        } catch (FileNotFoundException e) {
            // 获取异常的简单描述信息
            System.out.println(e.getMessage()); //你输入的路径 (系统找不到指定的文件。)

            //打印异常堆栈追踪信息！！！
            //在实际的开发中，建议使用这个。养成好习惯！
            // 这行代码要写上，不然出问题你也不知道！
            e.printStackTrace();
            /*
            java.io.FileNotFoundException:你输入的路径 (系统找不到指定的文件。)
                at java.base/java.io.FileInputStream.open0(Native Method)
                at java.base/java.io.FileInputStream.open(FileInputStream.java:213)
                at java.base/java.io.FileInputStream.<init>(FileInputStream.java:155)
                at java.base/java.io.FileInputStream.<init>(FileInputStream.java:110)
                at com.bjpowernode.javase.exception.ExceptionTest09.m3(ExceptionTest09.java:31)
                at com.bjpowernode.javase.exception.ExceptionTest09.m2(ExceptionTest09.java:27)
                at com.bjpowernode.javase.exception.ExceptionTest09.m1(ExceptionTest09.java:23)
                at com.bjpowernode.javase.exception.ExceptionTest09.main(ExceptionTest09.java:14)
                因为31行出问题导致了27行，27行出问题导致23行，23行出问题导致14行。
                应该先查看31行的代码。31行是代码错误的根源。
             */
        }

        // 这里程序不耽误执行，很健壮。《服务器不会因为遇到异常而宕机。》
        System.out.println("Hello World!");
    }

    private static void m1() throws FileNotFoundException {m2();}
    private static void m2() throws FileNotFoundException {m3();}
    private static void m3() throws FileNotFoundException {
        new FileInputStream("随便一个地址");
    }
}
```

## 自定义异常

SUN提供的JDK内置的异常肯定是不够的用的。在实际的开发中，有很多业务，这些业务出现异常之后，JDK中都是没有的。和业务挂钩的。

那么异常类我们程序员可以自己定义吗？

可以。

### 如何自定义异常

第一步：编写一个类继承Exception或者RuntimeException.

第二步：提供两个构造方法，一个无参数的，一个带有String参数的。

eg.

```java
class MyExceptionTest {
    public static void main(String[] args) {

        MyException exception = new MyException("用户名不能为空！");
        /*
        ExceptionTest.MyException: 用户名不能为空！
			at com.xm.ExceptionTest.MyExceptionTest.main(MyException.java:20)
        */

        exception.printStackTrace();
        exception.getMessage();

    }
}

public class MyException extends Exception { // 编译时异常
    public MyException() {

    }

    public MyException(String s) {
        super(s);
    }
}
```

eg.2登录实例

用户类

```java
public class UserService {

    public UserService(String name, String password) {
        this.name = name;
        this.password = password;
    }

    private String name;
    private String password;

    public UserService() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    //登录
    public void register(String username, String password) throws IllegalNameException {

        if(username == null || username.length()<6 || username.length()>14 ){
           throw  new IllegalNameException("用户名不合法，长度必须在6-14个字符之间");
        }
        System.out.println("注册成功！");
    }

}
```

自定义的异常类

```java
public class IllegalNameException extends Exception {

    public IllegalNameException() {
    }

    public IllegalNameException(String s){
        super(s);
    }

}
```

测试类

```java
public class Test {

    public static void main(String[] args) {

        UserService user = new UserService();

        try {
            user.register("","123456");
            /*
            用户名不合法，长度必须在6-14个字符之间
			com.xm.ExceptionTest.UserServiceTest.IllegalNameException: 用户名不合法，长度必须在6-14个字符之间
				at com.xm.ExceptionTest.UserServiceTest.UserService.register(UserService.java:35)
				at com.xm.ExceptionTest.UserServiceTest.Test.main(Test.java:10)
            */ 
        } catch (IllegalNameException e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }

    }

}
```



# 线程

## 概念

### 进程和线程

进程是一个应用程序（1个进程是一个软件）。

线程是一个进程中的执行场景/执行单元。

一个进程可以启动多个线程。

线程是依赖于进程的

### 多线程的意义

多线程的特点:具有随机性

多个线程在抢占CPU的执行权

### 面试题  

#### jvm是多线程吗?

是多线程，至少有两条线程

用户线程main,以及创建对象的时候,当对象使用完毕,需要被垃圾回收器回收;

jvm针对没有更多引用对象,开启一条垃圾回收线程!

#### java语言能够开启多线程?

开启线程---->开启进程----Java语言不能够开启进程---借助于底层语言C语言开启进程
封装成本地方法 ----->jdk提供类:Thread 里面封装好的方法
开启线程:start()

### Thread类的构造方法

```java
Thread(String name):创建线程类对象,设置名称
```

### Thread类的成员方法

```java
public final String getName():获取线程名称
public final void setName(String name):设置线程名称
```

###  线程的优先级

    Thread类中静态常量字段(成员变量field)
    public static final int MAX_PRIORITY 10     最大优先级
    public static final int MIN_PRIORITY 1      最小优先级
    public static final int NORM_PRIORITY 5     默认优先级
    public final void setPriority(int newPriority):设置线程的优先级
    public final int getPriority():获取优先级
优先级越大的:抢占CPU的执行权越大

优先级小的:抢占CPU的执行权越小

默认优先级:随机性大一些

## 实现线程有两种方式

java支持多线程机制。并且java已经将多线程实现了，我们只需要继承就行了。

### 第一种方式：

```java
编写一个类，直接继承java.lang.Thread，重写run方法。
	// 定义线程类
	public class MyThread extends Thread{
		public void run(){
		
		}
	}
	// 创建线程对象
	MyThread t = new MyThread();
	// 启动线程。
	t.start();
```

### 第二种方式：

```java
编写一个类，实现java.lang.Runnable接口，实现run方法。
	// 定义一个可运行的类
	public class MyRunnable implements Runnable {
		public void run(){
		
		}
	}
	// 创建线程对象
	Thread t = new Thread(new MyRunnable());
	// 启动线程
	t.start();

```

注意：第二种方式实现接口比较常用，因为一个类实现了接口，它还可以去继承
其它的类，更灵活。

## 线程对象的生命周期

```
新建状态
就绪状态
运行状态
阻塞状态
死亡状态
```

## 方法

### Join线程强制执行

```java
public final void join() throws InterruptedException;//等待该线程终止!
```

底层依赖于线程安全的方法join(0):永远等待(当前执行完毕结束!)，又依赖于wait(long time)

eg.

考虑安全问题:

三个线程---优先级 都是默认(5)

都去设置join(): 三个线程谁先抢占到谁先执行

join方法，m1调用之后，执行完m1才会执行join方法后的start方法

```java
public class ThreadTest01 {

    public static void main(String[] args) {

        MyThread m1 = new MyThread();
        MyThread m2 = new MyThread();
        MyThread m3 = new MyThread();

        m1.setName("老大");
        m2.setName("老二");
        m3.setName("老三");

        m1.setPriority(10);
        m2.setPriority(5);
        m3.setPriority(1);


        m1.start();
        try {
            m1.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        m2.start();
        try {
            m2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        m3.start();

    }

}

class MyThread extends Thread {

    public void run() {
        for (int i = 0; i < 3; i++) {
            System.out.println(getName() + ":" + i);
        }
    }

}
```

```
老大:0
老大:1
老大:2
老二:0
老二:1
老二:2
老三:0
老三:1
老三:2
```

### yield线程礼让

```java
public static void yield();//暂停当前正在执行的线程,执行对方线程
```

eg.

```java
public class ThreadTest02 {

    public static void main(String[] args) {
        //创建两条线程对象
        YieldThread yt1 = new YieldThread() ;
        YieldThread yt2 = new YieldThread() ;

        //设置名称
        yt1.setName("老大") ;
        yt2.setName("老二") ;

        //启动线程
        yt1.start();
        yt2.start();
    }
}

class YieldThread  extends Thread{

    @Override
    public void run() {
        for(int x = 0 ; x <100 ; x ++){
            System.out.println(getName()+":"+x);
            Thread.yield(); //暂停当前线程,执行对方线程
        }
    }

}
```

### sleep线程休眠

```java
public class ThreadTest04 {

    public static void main(String[] args) {

        Movies m1 = new Movies();
        Movies m2 = new Movies();
        Movies m3 = new Movies();

        m1.setName("窗口1");
        m2.setName("窗口2");
        m3.setName("窗口3");

        m1.start();
        m2.start();
        m3.start();

    }

}

class Movies extends Thread {

    public static int tickets = 100;


    @Override
    public void run() {
        while (true) {

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            if (tickets > 0) {
                System.out.println(getName() + "正在出售第" + tickets-- + "张票");
            }
        }
    }

}
```

### setDaemon

```java
public final void setDaemon(boolean on);
   //参数为true,表示标记当前线程为守护线程,当正在运行的线程如果都是守护线程,则jvm自动退出。这个方法必须在启动线程之前调用(start()之前)
```

 举例:
           玩游戏:坦克大战
           这个两个坦克----守护线程

eg.

```
public class ThreadTest05 {

    public static void main(String[] args) {

        //创建两个线程
        ThreadDaemon td1 = new ThreadDaemon() ;
        ThreadDaemon td2 = new ThreadDaemon() ;

        //设置名称
        td1.setName("张飞");
        td2.setName("关羽");

        //设置为守护线程
        td1.setDaemon(true) ;
        td2.setDaemon(true) ;

        //启动线程
        td1.start();
        td2.start();

        //public static Thread currentThread():获取正在运行的线程执行对象的引用
        Thread.currentThread().setName("刘备");
        //提供for循环:
        for(int x = 0 ; x < 5 ; x ++){
            System.out.println(Thread.currentThread().getName()+":"+x);
        }

    }

}

class ThreadDaemon extends Thread {
    @Override
    public void run() {
        for (int x = 0 ; x < 100; x ++){
            System.out.println(getName()+":"+x);
        }
    }
}
```

```
刘备:0
刘备:1
刘备:2
刘备:3
刘备:4
张飞:0
张飞:1
张飞:2
张飞:3
张飞:4
张飞:5
张飞:6
关羽:0
关羽:1
//一会自己就停了
```

## 线程创建实例

多线程的实现方式2步骤

> 1)自定义类实现Runnable接口
> 2)重写Runnable接口的run方法
> 3)在main用户线程中
>        可以分配类的实例(创建类的实例)
> 4)创建当前类对象,然后创建Thread类对象,将当前类对象作为参数来传递
>        当前类---->"资源共享类"
>        Thread(Runnable target, String name)
> 5)分别启动线程即可!

## 静态代理

###  特点

真实角色和代理角色必须实现同一个接口

### 角色

真实角色:专注于自己的功能

代理角色:完成对真实角色功能的"增强"，可以去做很多真实对象做不了的事情

eg.结婚

真实角色:You  你自己

代理角色:WeddingCompany 婚庆公司

```java
public class ThreadTest01 {

    public static void main(String[] args) {

        You you = new You();
        you.marry();

        WeddingCompany weddingCompany = new WeddingCompany(you);
        weddingCompany.marry();
        
        
        new WeddingCompany(new You()).marry();
       
        new Thread(()->System.out.println("我爱你")).start();
       	//()->System.out.println("我爱你") ，实现于Runnable接口

    }

}

interface Marry{
    void marry();
}

//结婚的人
class You implements  Marry{

    @Override
    public void marry() {
        System.out.println("结婚了,很开心...");
    }
}

//代理角色:婚庆公司 在你结婚之前,它可以给你布置婚礼线程, 结婚之后,开开心心吃席
class WeddingCompany implements Marry{

    //将真实角色作为参数传递
    private You you ;

    public WeddingCompany() {
    }

    public WeddingCompany(You you){
        this.you = you ;
    }

    @Override
    public void marry() {
        System.out.println("给你布置婚礼现场...");
        you.marry();  //只要专注于自己的事情!
        System.out.println("婚礼线程布置完毕,吃席...");
    }
}
```

```
结婚了,很开心...
给你布置婚礼现场...
结婚了,很开心...
婚礼线程布置完毕,吃席...
```

## Lamda表达式

函数式接口的定义： 任何接口，如果只包含唯一一个抽象方法，那么他就是一个函数接口

对于函数式接口，我们可以通过lamda表达式来创建接口对象

```java
 public interface Runnable{
        public abstract void run();
    }
```

```java
public class LamdaTest {

    //3.静态内部类
    static class Like2 implements lLike {

        @Override
        public void lamda() {
            System.out.println("我喜欢2");
        }
    }

    public static void main(String[] args) {
        lLike like = new Like();
        like.lamda();//1

        like = new Like2();
        like.lamda();//2


        //4.局部内部类
        class Like3 implements lLike {
            @Override
            public void lamda() {
                System.out.println("我喜欢3");
            }
        }

        like = new Like3();
        like.lamda();//3

        //5.匿名内部类
        like = new lLike() {
            @Override
            public void lamda() {
                System.out.println("我喜欢4");
            }
        };
        like.lamda();//4

        //6.lamda表达式
        like = () -> {
            System.out.println("我喜欢5");
        };
        like.lamda();//5

        //化简
        like = () -> System.out.println("我喜欢6");
        like.lamda();

        /*总结：

        */

    }
}

//1.定义一个接口
interface lLike {
    void lamda();
}

//2.子实现类
class Like implements lLike {

    @Override
    public void lamda() {
        System.out.println("我喜欢1");
    }
}
```

```jaav 
我喜欢1
我喜欢2
我喜欢3
我喜欢4
我喜欢5
我喜欢6
```

lambda 表达式C只能有一行代码的情况下才能简化成为一行， 如果有多行，那么就用代码块包安。

前提是接口必须为函数型接口

多个参数也可以去掉参数类型，要去掉就都去掉

## 多线程安全

###  检验多线程安全问题的标准;

1)是否是多线程环境，不能更改,使用多线程实现

2)是否存在共享数据，必须要有共享数据

3)是否存在多条语句对共享数据的操作

### 解决

#### Java提供同步机制:

同步代码块 将多条对共享数据包裹起来

```java
synchronized(锁对象){
    将多条对共享数据包裹起来
}
```

 锁对象:必须要多个线程使用的同一个锁对象,而不是分别自己的锁对象!

#### 出现同票的原因:

线程的原子性操作(++,--:最简单的操作)

原子性:记录原始数据值,然后再对数据自增或者自减

eg.

```java
class Movie implements Runnable {
    private int tickets = 1000;
    private Object obj = new Object();

    @Override
    public void run() {
        while (true) {
            synchronized (obj) {
                if (tickets > 0) {
                    System.out.println(Thread.currentThread().getName() + "正在出售第" + tickets + "张票");
                    tickets--;
                } else {
                    return;
                }
            }
        }
    }
}

public class ThreadTest02 {
    public static void main(String[] args) {
        Movie m = new Movie();

        Thread t1 = new Thread(m, "窗口1");
        Thread t2 = new Thread(m, "窗口2");
        Thread t3 = new Thread(m, "窗口3");

        t1.start();
        t2.start();
        t3.start();
    }

}
```

窗口2正在出售第100张票
窗口2正在出售第99张票
窗口1正在出售第98张票
窗口1正在出售第97张票
窗口1正在出售第96张票
窗口1正在出售第95张票
窗口1正在出售第94张票
窗口1正在出售第93张票
窗口1正在出售第92张票
窗口3正在出售第91张票
窗口3正在出售第90张票
窗口3正在出售第89张票
窗口3正在出售第88张票
窗口3正在出售第87张票
窗口3正在出售第86张票

……

## 对象锁synchronized

锁对象:必须为多个线程的同一把锁;锁对象可以是任意Java的类对象

```
synchronized(锁对象){
    多条语句对共享数据的操作
}
```

### 面试题:

 wait()方法/notify()方法    ---- 也可以称为"同步"  --->等待唤醒机制

#### 线程等待/线程唤醒，这些方法为什么不定义在Thread类中呢,而是定义Object类中呢?

它和锁对象有关系,而锁对象，可以任何的java类对象,-----Object(顶层父类)

```java
syncronized(锁对象){
     锁对象.wait()
   	 //锁对象.notify()
}
```

#### 什么是同步方法?

如果一个方法的方法体的第一句话就是同步代码块

可以将synchronized关键字提取到方法声明上,跟在权限修饰符的后面

```java
权限修饰符 synchronized 返回值类型 方法名(形式列表){   //非静态的同步方法
     业务逻辑...
}
```

### 关于synchronized的使用

#### 面试题：

doother方法执行的时候需不需要等待dosome方法的结束？

eg.1

```java
public class synchronizeExam01 {

    public static void main(String[] args) throws InterruptedException {

        MyClass ms = new MyClass();

        Thread t1 = new MyThread(ms);
        Thread t2 = new MyThread(ms);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000);//为了确保t1先执行
        t2.start();

    }

}

class MyThread extends Thread {

    private MyClass ms;

    public MyThread(MyClass ms) {
        this.ms = ms;
    }

    @Override
    public void run() {
        if(Thread.currentThread().getName().equals("t1")){
            ms.dosome();
        }else if(Thread .currentThread().getName().equals("t2")){
            ms.doother();
        }
    }
}

class MyClass {

    //synchronize出现在实例方法上,表this
    public synchronized void dosome() {
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }

    //没有synchronize
    public void doother() {
        System.out.println("doother begin");
        System.out.println("doother over");
    }

}
```

```
//不需要，因为doother没有synchronize
/*
doSome begin
doother begin
doother over
（十秒后）
doSome over
 */
```

eg.2

```java
public class synchronizeExam02 {

    public static void main(String[] args) throws InterruptedException {

        MyClass ms = new MyClass();

        Thread t1 = new MyThread(ms);
        Thread t2 = new MyThread(ms);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000);//为了确保t1先执行
        t2.start();

    }

}

class MyThread extends Thread {

    private MyClass ms;

    public MyThread(MyClass ms) {
        this.ms = ms;
    }

    @Override
    public void run() {
        if(Thread.currentThread().getName().equals("t1")){
            ms.dosome();
        }else if(Thread .currentThread().getName().equals("t2")){
            ms.doother();
        }
    }
}

class MyClass {

    //synchronize出现在实例方法上,表this
    public synchronized void dosome() {
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }

    public synchronized void doother() {
        System.out.println("doother begin");
        System.out.println("doother over");
    }

}
```

```
//需要等待，因为在一个所里面
/*
doSome begin
（十秒）
doSome over
doother begin
doother over
 */
```

eg.3

```java
public class synchronizeExam03 {

    public static void main(String[] args) throws InterruptedException {

        MyClass ms1 = new MyClass();
        MyClass ms2 = new MyClass();

        Thread t1 = new MyThread(ms1);
        Thread t2 = new MyThread(ms2);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000);//为了确保t1先执行
        t2.start();

    }

}

class MyThread extends Thread {

    private MyClass ms;

    public MyThread(MyClass ms) {
        this.ms = ms;
    }

    @Override
    public void run() {
        if(Thread.currentThread().getName().equals("t1")){
            ms.dosome();
        }else if(Thread .currentThread().getName().equals("t2")){
            ms.doother();
        }
    }
}

class MyClass {

    //synchronize出现在实例方法上,表this
    public synchronized void dosome() {
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }

    public synchronized void doother() {
        System.out.println("doother begin");
        System.out.println("doother over");
    }

}
```

```
//不需要，因为不是一个锁里，有两把锁
/*
doSome begin
doother begin
doother over
（十秒）
doSome over
 */
```

eg.4

```java
public class synchronizeExam04 {

    public static void main(String[] args) throws InterruptedException {

        MyClass ms = new MyClass();

        Thread t1 = new MyThread(ms);
        Thread t2 = new MyThread(ms);

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        Thread.sleep(1000);//为了确保t1先执行
        t2.start();

    }

}

class MyThread extends Thread {

    private MyClass ms;

    public MyThread(MyClass ms) {
        this.ms = ms;
    }

    @Override
    public void run() {
        if(Thread.currentThread().getName().equals("t1")){
            ms.dosome();
        }else if(Thread .currentThread().getName().equals("t2")){
            ms.doother();
        }
    }
}

class MyClass {
    //synchronize出现在静态方法上,是类锁
    public synchronized static void dosome() {
        System.out.println("doSome begin");
        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("doSome over");
    }

    public synchronized static void doother() {
        System.out.println("doother begin");
        System.out.println("doother over");
    }

}
```

```
//需要等待，因为静态方法是类锁，不管创建了几个对象，类锁只有一把
/*
doSome begin
（十秒）
doSome over
doother begin
doother over
 */
```

## 死锁

### 线程安全问题:

可以通过同步方法或者是同步代码块去解决,但是执行过程中就可能出现死锁问题

### 死锁问题:

(使用同步机制解决线程安全) 线程和线程之间出现了互相等待的情况!

### 解决方案:

多个线程之间的通信:必须使用的是一个资源类对象,而不能是每一个线程在使用自己的资源类对象!

使用生成者和消费者模式思想去解决,前提条件:生成者线程和消费者线程 必须操作的同一个资源类对象!
           
synchronize不要嵌套使用，容易死锁!!!!!

### 产生死锁的四个必要条件:

1.互斥条件:一个资源每次只能被一个进程使用。

2.请求与保持条件:一个进程因请求资源而阻塞时,对已获得的资源保持不放。

3.不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。

4.循环等待条件:若干进程之间形成-种头尾相接的循环等待资源关系。

```java
public class DieLockTest {
    public static void main(String[] args) {

        Object o1 = new Object();
        Object o2 = new Object();

        Thread t1 = new MyThread1(o1, o2);
        Thread t2 = new MyThread2(o1, o2);

        t1.start();
        t2.start();

    }
}

class MyThread1 extends Thread {

    Object o1;
    Object o2;

    public MyThread1(Object o1, Object o2) {
        this.o1 = o1;
        this.o2 = o2;
    }

    @Override
    public void run() {
        synchronized (o1) {
            System.out.println("T1O1");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o2) {
                System.out.println("T1O2");
            }
        }
    }
}

class MyThread2 extends Thread {

    Object o1;
    Object o2;

    public MyThread2(Object o1, Object o2) {
        this.o1 = o1;
        this.o2 = o2;
    }

    @Override
    public void run() {
        synchronized (o2) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("T2O2");
            synchronized (o1) {
                System.out.println("T2O1");
            }
        }
    }
}
```

```
T1O1
T2O2
```

## 生产者消费者问题

### 概念

1、使用wait万法和notify方法实现“生产者和消费者模式”

2、什么是“生产者和消费者模式?
生产线程负责生产，消费线程负责消费。
生产线程和消费线程要达到均衡。
这是一种特殊的业务需求,在这种特殊的情况下需要使用wait方法和notify方法。

3、wait称notify方法不是线程对象的方法，是普通java对象都有的方法。

4、wait方法和notify方法建立在线程同步的基础之上。因为多线程要同时操作一个仓库。有线程安全问题。

5、wait方法作用:o.wait()让正在o对象上活动的线程进入等待状态,并且释放掉t线程之前占有的o对象的锁。

6、notify方法作用:o.notify()让正在o对象上等待的线程晚醒，只是通知，不会释放o对象上之前占有的锁。

### 实例

eg.1信号灯法

```
import java.util.ArrayList;
import java.util.List;

public class ThreadTest {
    public static void main(String[] args) {

        List list = new ArrayList();

        Thread t1 = new Thread(new Proudcer(list));
        Thread t2 = new Thread(new Consumer(list));

        t1.setName("生产者线程");
        t2.setName("消费者线程");

        t1.start();
        t2.start();

    }
}

class Proudcer implements Runnable {

    private List list;

    public Proudcer(List list) {
        this.list = list;
    }

    @Override
    public void run() {
        //一直生产
        while (true) {
            //给仓库list上锁
            synchronized (list) {
                if (list.size() > 0) {//大于0，仓库以及有东西了
                    try {
                        list.wait();//当前线程处于等待状态，并且释放producer之前占有的list集合的锁。
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                //程序到这里说明仓库是空的，可以生产
                Object o = new Object();
                list.add(o);
                System.out.println(Thread.currentThread().getName()+"="+o);
                //唤醒消费者进行消费
                list.notifyAll();//唤醒全部也没关系，因为没有唤醒锁
            }
        }
    }
}

class Consumer implements Runnable {

    private List list;

    public Consumer(List list) {
        this.list = list;
    }

    @Override
    public void run() {
        //一直消费
        while (true) {
            synchronized (list){
                if (list.size() == 0){
                    //仓库已空
                    //消费者线程等待，释放掉list的锁
                    try {
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                //程序能够执行到此处说明仓库中有数据，进行消费
                Object o = list.remove(0);
                System.out.println(Thread.currentThread().getName()+"="+o);
                //唤醒生产者生产
                list.notifyAll();//唤醒全部也没关系，因为没有唤醒锁
            }
        }
    }
}
```

```
生产者线程=java.lang.Object@23f6a0e3
消费者线程=java.lang.Object@23f6a0e3
生产者线程=java.lang.Object@e660ca1
消费者线程=java.lang.Object@e660ca1
生产者线程=java.lang.Object@3b8ad26c
消费者线程=java.lang.Object@3b8ad26c
```

eg.2信号灯法

> 1)StuffBun包子类属性
>         包含包子的名称name
>         包子的大小type
> 2)生产者资源类      SetBun   产生包子
> 3)消费者资源类      GetBun   使用包子
> 4)ThreadDemo:main 用户线程
>
> 按照上面的方式:模拟生产者产生数据,消费者使用数据出现问题 null---null
>
> 生产资源类中和消费者资源类中所操作的包子对象不是同一个对象!
>
> 可以将包子通过生产资源类或者消费者资源类 通过构造方法传递
>
>
> 优化1:
>         加入while循环,模拟包子一直生产和一直消费!
>
> ​        出现问题:数据紊乱:加入同步代码块给每一个资源类中都加入解决! 将多条语句对共享数据的操作包起来!

```java
//包子类
public class StuffedBun {

     String type;
     String stuffing;

}
//吃包子
public class getBun implements Runnable {

    private StuffedBun gtb;

    public getBun(StuffedBun gtb) {
        this.gtb = gtb;
    }

    @Override
    public void run() {
        while (true) {
            synchronized (gtb) {
                System.out.println(gtb.type + gtb.stuffing);
            }
        }
    }
}
//生产包子
public class setBun implements Runnable {

    private StuffedBun stb;

    public setBun(StuffedBun stb) {
        this.stb = stb;
    }

    static int i = 1;

    @Override
    public void run() {
        while (true) {
            synchronized (stb) {
                if (i % 2 == 0) {
                    stb.stuffing = "肉馅";
                    stb.type = "大包子";
                } else  {
                    stb.stuffing = "韭菜馅";
                    stb.type = "小包子";
                }
                i++;
            }
        }
    }
}
public class Test {

    public static void main(String[] args) {

        //生成包子
        StuffedBun bun = new StuffedBun();

        //生成生产者
        setBun sb = new setBun(bun);
        //生成消费者
        getBun gb = new getBun(bun);

        Thread t1 = new Thread(sb);
        Thread t2 = new Thread(gb);

        t1.start();
        t2.start();

    }

}
```

```
……
小包子韭菜馅
小包子韭菜馅
小包子韭菜馅
大包子肉馅
大包子肉馅
……
```

eg.3信号灯法

> 问题:
>
> 消费者资源类所在的消费者线程中,每次输出一大片的内容:
> 线程的执行随机性----线程抢占CPU的执行权,一点点时间片执行很多次
>
> 优化2:
>    想出现依次打印
>    肉包子---大包子
>    菜包子---小包子
>    ...
> wait()+notify()--->实现同步机制(并且同时信号法:将死锁问题解决!)

```java
public class Bun {

    String name;
    String type;
    boolean flag = false;//没包子

}
public class getBun implements Runnable {

    private Bun gb;

    public getBun(Bun gb) {
        this.gb = gb;
    }

    @Override
    public void run() {
        while (true) {
            synchronized (gb) {
                if (!gb.flag) {//没包子
                    try {
                        gb.wait();//锁住，不能吃包子
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                } else {//有包子
                    System.out.println(Thread.currentThread().getName() + "：吃了一个包子" + gb.name + "," + gb.type);
                    System.out.println("没包子了！！！");
                    gb.flag = false;
                    gb.notify();//等待，开始做包子
                }
            }
        }
    }
}
public class setBun implements Runnable {

    private Bun sb;

    public setBun(Bun sb) {
        this.sb = sb;
    }


    @Override
    public void run() {
        int i = 0;

        while (true) {
            synchronized (sb) {
                if (sb.flag) {//有包子
                    try {
                        sb.wait();//等待，不能做包子
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                //没包子
                System.out.println("开始做包子！！！");
                if (i % 2 == 0) {
                    sb.name = "菜包子";
                    sb.type = "大包子";
                } else {
                    sb.name = "肉包子";
                    sb.type = "小笼包";
                }
                i++;

                sb.flag = true;//有包子了
                System.out.println(Thread.currentThread().getName() + "：产出一个包子" + sb.name + "," + sb.type);
                sb.notify();//释放，让吃包子
            }
        }
    }
}
public class Test {
    public static void main(String[] args) {

        Bun bun = new Bun();

        setBun setBun = new setBun(bun);
        getBun getBun = new getBun(bun);

        Thread t1 = new Thread(setBun);
        Thread t2 = new Thread(getBun);

        t1.setName("生产包子");
        t2.setName("吃包子");

        t1.start();
        t2.start();

    }
}
```

```
开始做包子！！！
生产包子：产出一个包子菜包子,大包子
吃包子：吃了一个包子菜包子,大包子
没包子了！！！
开始做包子！！！
生产包子：产出一个包子肉包子,小笼包
吃包子：吃了一个包子肉包子,小笼包
没包子了！！！
```

eg.4管程法

```java
public class SynchronizeTest {
    public static void main(String[] args) {

        Synchronizer s1 = new Synchronizer();

        new Thread(new Productor(s1)).start();
        new Thread(new Consumer(s1)).start();

    }
}


//生产者
class Productor implements Runnable{

    Synchronizer container;

    public Productor(Synchronizer container) {
        this.container = container;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("生产了第"+i+"只鸡");
            container.setChickens(new Chicken(i));
        }
    }
}

//消费者
class Consumer implements Runnable{

    Synchronizer container;

    public Consumer(Synchronizer container) {
        this.container = container;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("吃了第"+container.getChickens().id+"只鸡");
        }
    }
}

class Chicken{

    int id;

    public Chicken() {
    }

    public Chicken(int id) {
        this.id = id;
    }
}

//缓冲区
class Synchronizer{

    Chicken[] chickens = new Chicken[10];

    int i = 0;

    //生产
    public synchronized void setChickens(Chicken chicken){

        //如果容器满了，通知消费者消费，生产者等待
        if(i==chickens.length){
            System.out.println("容器已满，生产者等待，消费者请消费");
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //如果容器没满，生产者生产
        System.out.println("容器未满，放入容器");
        chickens[i] = chicken;
        i++;

        //消费者开始消费
        notify();

    }

    //消费者
    public synchronized Chicken getChickens(){

        //容器空了，消费者等待，生产者生产
        if(i==0){
            System.out.println("容器已空，消费者等待，生产者请生产");
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //如果可以消费
        System.out.println("开始消费");
        i--;
        Chicken chicken = chickens[i];

        //没鸡了，通知生产着生产
        notify();
        return chicken;

    }

}
```

```
容器未满，放入容器
生产了第1只鸡
容器未满，放入容器
生产了第2只鸡
容器未满，放入容器
生产了第3只鸡
容器未满，放入容器
生产了第4只鸡
容器未满，放入容器
生产了第5只鸡
容器未满，放入容器
生产了第6只鸡
容器未满，放入容器
生产了第7只鸡
容器未满，放入容器
生产了第8只鸡
容器未满，放入容器
生产了第9只鸡
容器未满，放入容器
生产了第10只鸡
容器已满，生产者等待，消费者请消费
开始消费
吃了第9只鸡
……
```

## lock锁

### synchronize和lock的对比

> Lock是显式锁(手动开启和关闭锁，别忘记关闭锁) synchronized是隐式锁， 出了
> 作用域自动释放
>
> Lock只有代码块锁，synchronized有代码块锁和方法锁
>
> 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的扩展
> 性(提供更多的子类)

### 优先使用顺序:

Lock >同步代码块(已经进入了方法体，分配了相应资源) >同步方法(在方法体之外)

### 格式：

```java
class A{
	private final ReentrantLock lock = new ReenTrantL ock();
	public void m(){
	lock.lock();
		try{
			//保证线程安全的代码;
			}
		
		finally{
			lock.unlock);
			//如果同步代码有异常，要将unlock()写入inally语句块
		}
	}
}
```

eg.实现电影卖票:三个窗口出售100张票 (线程的创建方式2)

```java
public class LockDemo {


    public static void main(String[] args) {
            //创建共享资源类对象
        SellTicket st = new SellTicket() ;
        //创建三个线程类对象
        Thread t1 = new Thread(st,"窗口1") ;
        Thread t2 = new Thread(st,"窗口2") ;
        Thread t3 = new Thread(st,"窗口3") ;

        //启动线程
        t1.start();
        t2.start();
        t3.start();
    }
}

public class SellTicket implements  Runnable {

    //定义100张票
    private static int tickets = 100 ;

    //创建一个锁对象
    Lock lock = new ReentrantLock() ;

    @Override
    public void run() {
        //模拟一只有票
        while(true){

            //try...catch...finaly:捕获异常
            //使用try..finally
            try{
                 //通过锁对象--->获取锁
                lock.lock();
                //判断
                if(tickets>0){

                    //睡眠100毫秒
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    System.out.println(Thread.currentThread().getName()+"正在出售第"+(tickets--)+"张票");
                }else{
                    break ;
                }
            }finally {
                //释放锁
                lock.unlock();
            }
        }
    }
}
```

窗口2正在出售第100张票
窗口2正在出售第99张票
窗口2正在出售第98张票
窗口2正在出售第97张票
窗口2正在出售第96张票

……

## 线程组

### 概念

将线程可以都添加一组中,方便管理，线程启动完毕之后，线程终止之后,不会将这个线程对象在内存中重复利用

线程组代表一组线程；还可以包括其他线程组

```java
Thread：
public final ThreadGroup getThreadGroup() {//获取线程组
	return group;
}

//给线程设置默认的线程组名称
//public Thread(ThreadGroup group, Runnable target) {

ThreadGroup:
public final String getName() {//获取默认的线程组名称
  	return name;
}
```

### 构造方法;

```java
public ThreadGroup(String name) {}
```

 任务队列--------一个任务称为"一个线程" , Quene

```java
public class ThreadGroupDemo {
    public static void main(String[] args) { //jvm调用main方法

      //  method1();
        method2() ;
    }

    //设置一个新的线程组名称
    private static void method2() {
        //创建一个线程组对象--同时设置线程组名称
        ThreadGroup tg = new ThreadGroup("myMain") ;
        //创建两条线程对象
        MyThread my = new MyThread() ;
        Thread t1 = new Thread(tg,my) ;
        Thread t2 = new Thread(tg,my) ;
        //获取线程组对象并同时线程组名称
        String name1 = t1.getThreadGroup().getName();
        String name2 = t2.getThreadGroup().getName();
        System.out.println(name1+"---"+name2);
    }

    private static void method1() {

        //创建两个线程
        MyThread my  = new MyThread() ;
        Thread t1 = new Thread(my) ;
        Thread t2 = new Thread(my) ;
        ThreadGroup tg1 = t1.getThreadGroup();
        ThreadGroup tg2 = t2.getThreadGroup();
        String name1 = tg1.getName();
        String name2 = tg2.getName();
        System.out.println(name1+"---"+name2);  //默认线程组名称都是main


    }
}

public class MyThread implements Runnable {
    @Override
    public void run() {
        for(int x = 0 ;x < 100 ;  x ++){
            System.out.println(Thread.currentThread().getName()+":"+x);
        }
    }
}
```

```
myMain---myMain
```

## 线程池

### 概念

JDK5.0起提供了线程池相关API: ExecutorService和Executors

ExecutorService: 真正的线程池接口。

### 常见子类

ThreadPoolExecutor

​	void execute(Runnable command) :执行任务/命令,没有返回值，一般用来执行Runnable
​    异步计算的结果,如果不做计算,无须返回结果!

​	<T> Future<T> submit(Callable<T> task):执行任务,有返回值，一般又来执行Callable
​    提交值返回任务以执行，并返回代表任务待处理结果的Future。

​    void shutdown() :关闭连接池

### Executors:

工具类、线程池的工厂类,用于创建并返回不同类型的线程池
通过Executors工具类，创建一个固定的可重用的线程数,返回值就线程池对象
public static ExecutorService newFixedThreadPool(int nThreads)

### 特点

在内存中创建一个固定可重用的线程数,当前线程执行完毕终止了,不会被回收掉,再次回到线程池中,等待下一次利用!

### 弊端:

维护成本大

eg.1

```java
public class ThreadPoolDemo {
    public static void main(String[] args) {

        //通过工厂类创建线程池对象
        ExecutorService threadPool = Executors.newFixedThreadPool(2);

        //提交异步方法
        //MyRunnable:打印x的值0-99之间的数据,不需要返回结果
      	//threadPool.submit(new MyRunnable()) ;
      	//threadPool.submit(new MyRunnable()) ;
        //<T> Future<T> submit(Callable<T> task)
        
        //Callable:提交异步计算---需要重写Callable的call来计算结果;如果没有结果,直接在call无须返回
        threadPool.submit(new MyCallable()) ;
        threadPool.submit(new MyCallable()) ;

        //void shutdown()关闭线程池
        threadPool.shutdown();

    }
}
public class MyCallable implements Callable {
    @Override
    public Object call() throws Exception {
        for(int x = 0 ; x < 100 ; x++){
            System.out.println(Thread.currentThread().getName()+":"+x);
        }

        return null;
    }
}
public class MyRunnable implements  Runnable {
    @Override
    public void run() {
        for(int x = 0 ; x < 100 ; x ++){
            System.out.println(Thread.currentThread().getName()+":"+x);
        }
    }
}
```

```
pool-1-thread-1:0
pool-1-thread-1:1
pool-1-thread-1:2
pool-1-thread-1:3
pool-1-thread-2:0
pool-1-thread-1:4
pool-1-thread-1:5

……

pool-1-thread-2:97
pool-1-thread-2:98
pool-1-thread-2:99
```

eg.2

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class PoolTest {
    public static void main(String[] args) {

        ExecutorService service = Executors.newFixedThreadPool(10);

        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());

        service.shutdown();

    }
}

class MyThread implements  Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

```
pool-1-thread-1
pool-1-thread-2
pool-1-thread-3
pool-1-thread-4
```

# 设计模式——简单工厂

设计模式是一种思想,前人总结出来,不是技术！！

## 设计原则

### 开闭原则

对现有代码修改关闭,对扩展代码开放

举例：项目开发完毕，进行更新，不能够修改现有代码，在现有代码的基础上提供扩展！

### 接口分离原则

一个接口中定义一个功能，接口和接口之间独立的,不能相互影响

实际开发中：某个接口中,可能将相关的功能都定义在这一个接口中

### 按模块划分

用户模块	UserDao  接口
login()/register()/logout()/checkUserName()

商品模块	ProductDao 接口
Product findById(Integer id) ;
List<Product> findAll() ;
void update(Product product);

订单模块	OrderDao 接口
List<Order> findPage(int pageSize,int currentPage);

### 里氏替换原则

```java
任何父类出现的地方都可以子类替代!
class Father{
	public void show(){
            // ...
    	Class<Son> clazz  = Son.class ;
           //反射方式---->字节码文件对象就调用method----->将所有的成员方法---->Method
	}
}
class Son extends Father{
	public void method(){

    }
}
```

### "低耦合,高内聚"

23种设计模式都需要遵循 原则："低耦合,高内聚"

## 类型

### 创建型:

对象的创建 (使用最多)

### 结构型:

整个结构的组成：

```
代理模式
		静态代理
        	代理角色
            真实角色
```

### 行为型:

具备功能性的

创建型设计模式:

## 简单工厂（静态工厂方法模式）

> 优点:利用多态创建子类对象,能够灵活去创建对象(提供的静态功能)
>
> 弊端:代码量大,一旦有新的类型增加,工厂类的静态功能就需要改动...

eg.1

```java
public class Animal {
    public void eat(){
        System.out.println("吃");
    }
    public void sleep(){
        System.out.println("睡");
    }
}

public class Cat extends Animal {
    @Override
    public void eat() {
        System.out.println("猫吃鱼...");
    }

    @Override
    public void sleep() {
        System.out.println("猫趴着睡觉");
    }
}

public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗吃骨头");

    }

    @Override
    public void sleep() {
        System.out.println("狗躺着睡");
    }
}

public class Pig extends Animal {
    @Override
    public void eat() {
        System.out.println("猪吃白菜");
    }

    @Override
    public void sleep() {
        System.out.println("猪侧着睡...");

    }
}

public class AnimalFactory {
    //多态方式
    public static Animal createAnimal(String type){
        if("cat".equals(type)){
            return new Cat() ;
        }else if("dog" .equals(type)){
            return new Dog() ;
        }else if("pig".equals(type)){
            return new Pig() ;
        }
        return null ;
    }
}

public class PatternDemo1 {
    public static void main(String[] args) {

        //没有提供任何式设计模式
        //创建具体类对象
        //使用多态
        //Cat c = new Cat() ;
        Animal a = new Cat() ;
        a.eat();
        a.sleep();
        a = new Dog() ;
        a.eat();
        a.sleep();
        System.out.println("----------------------------------------------");

        //利用多态完成
        Animal animal = AnimalFactory.createAnimal("cat");
        animal.eat();
        animal.sleep();
        animal = AnimalFactory.createAnimal("dog");
        animal.eat();
        animal.sleep();
        animal = AnimalFactory.createAnimal("pig");
        animal.eat();
        animal.sleep();
        animal = AnimalFactory.createAnimal("monkey");
        if(animal!=null){
            animal.eat();
            animal.sleep();
        }else{
            System.out.println("没有提供该动物的实例创建");
        }

    }
}
```

```
猫吃鱼...
猫趴着睡觉
狗吃骨头
狗躺着睡
----------------------------------------------
猫吃鱼...
猫趴着睡觉
狗吃骨头
狗躺着睡
猪吃白菜
猪侧着睡...
没有提供该动物的实例创建
```

## 设计模式——工厂方法模式

### 特点

>  1)抽象类:Animal 动物类 (基类)
>  2)提供一些子类,完成方法重写:eat(){} sleep(){}
>  3)提供一个接口

优点:具体的动物创建,交给类工厂类来实现 (面向接口编程),便于功能维护(代码维护)
弊端:代码量非常大
如果有新的类中增加,那么还必须提供对应具体的工厂类创建当前类实例

eg.

```java
// 抽象的动物类
public abstract class Animal {
    public abstract  void eat() ;
    public abstract  void sleep() ;
}

//工厂接口，具体的对象的创建工作----交给工厂接口(抽象类)的子实现类完成!
public interface Factory {
    public Animal createAnimal() ;
}

//具体的工厂类 创建狗这个类型的实例
public class DogFactory  implements Factory{
    @Override
    public Animal createAnimal() { //接口多态中---->抽象类多态
        return new Dog();
    }
}

//具体的工厂类 创建猫这个类型的实例
public class CatFactory  implements  Factory{
    @Override
    public Animal createAnimal() {
        return new Cat();
    }
}

public class Cat extends Animal {
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }

    @Override
    public void sleep() {
        System.out.println("猫趴着睡觉...");
    }
}

public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗吃骨头...");
    }

    @Override
    public void sleep() {
        System.out.println("狗躺着睡觉...");
    }
}

public class FactoryPattern {
    public static void main(String[] args) {

        //没有工厂方法之前:
        //使用抽象类多态
        Animal a = new Dog() ;
        a.eat();
        a.sleep();
        System.out.println("-------------------------------");
        a = new Cat() ;
        a.eat();
        a.sleep();

        //使用工厂方式模式来操作
        System.out.println("-------------------------------------");
        Factory factory = new DogFactory() ;
        Animal animal = factory.createAnimal();
        animal.eat();
        animal.sleep();

        System.out.println("---------------------------------------");
        factory = new CatFactory() ;
        Animal a2 = factory.createAnimal();
        a2.eat();
        a2.sleep();
    }
}
```

```
狗吃骨头...
狗躺着睡觉...
-------------------------------
猫吃鱼
猫趴着睡觉...
-------------------------------------
狗吃骨头...
狗躺着睡觉...
---------------------------------------
猫吃鱼
猫趴着睡觉...
```

# 设计模式——单例模式

## 概念

始终在内存中创建一个实例

## 分类

分为两种

饿汉式:永远不会出现的单例模式(最简单的一种单例)

懒汉式:可能出现问题的一种单例模式

### 饿汉式

1)构造方法私有:保证外界不能直接创建当前类对象

2)在当前类的成员位置:创建当前类实例

3)提供一个静态的功能,返回值就是当前类本身(需要当前类的实例)

```java
public class Student {

    //成员变量
    private  static Student s = new Student() ; //当前类实例 (静态实例变量)
    private Student(){}

    //静态功能
    //返回是当前类本身
    public static Student getStudentInstance(){
        return s;
    }
}

public class Single_Pattern_01 {
    public static void main(String[] args) {

		//调用公共方法
        Student s1 = Student.getStudentInstance();
        Student s2 = Student.getStudentInstance();
        Student s3 = Student.getStudentInstance();
        Student s4 = Student.getStudentInstance();
        System.out.println(s1==s2);
        System.out.println(s1==s3);
        System.out.println(s1==s4);
      
    }
}
```

```
true
true
true
```

### 懒汉式

可能出现问题的一种单例模式

1)构造方法私有化

2)在当前成员变量的位置声明变量:数据类型就是当期类 (私有的,静态的)

3)提供静态功能,返回值还是当前类本身,判断如果当前没有给当前类型变量为null,直接new 当期类的实例；如果不为null,就返回当前类的变量!

延迟加载或者懒加载----出现安全问题

面试中,问的最多就是懒汉式----->如何解决线程安全问题: 想到同步方法解决

```java
public class Worker {

    //成员变量位置声明变量w
    private static Worker w;//默认值null
    //构造方法私有化
    private Worker() {} //外界不能直接new 对象

   //改进:同步代码块---->静态的同步方法
    public synchronized static Worker getWorkerIntance(){ //锁对象:Worker.class
      
        if(w == null){ //当为null
        	w = new Worker() ; //如果当前w变量为null,创建一个新的对象,返回
        	return w;
       	}
        return w ;
    }
}

public class Single_pattern_02 {
    public static void main(String[] args) {

        //使用Worker第一次调用getInstance方法
        Worker w1 = Worker.getWorkerIntance();//第一个用户操作这个方法----属于一个线程
        //第二次
        Worker w2 = Worker.getWorkerIntance();//第二个用户操作---->属于一个线程
        System.out.println(w1==w2);
        
        Worker w3 = Worker.getWorkerIntance();//第三个用户操作--->属于一个线程
        System.out.println(w1==w3);
    }
```

```
true
true
```

# IO流

## File类

File 文件和目录(文件夹)路径名的抽象表示。

### 构造方法

```java
File(String pathname);//参数就是指定的路径/如果没有指定路径(默认是在当前项目下)
通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例
File(File parent, String child);//从父抽象路径名和子路径名字符串创建新的 File实例。
File(String parent, String child);//参数1:父目录地址    参数2:具体的子文件地址
```

### 成员方法

#### 创建/删除/重名

```java
public boolean createNewFile()   throws IOException :表示创建文件 :如果不存在,则创建
public boolean mkdir():创建文件夹,如果不存在,则创建;否则就返回false
public boolean mkdirs():创建多个文件,如果父目录不存在,则创建
public boolean delete():删除文件或者文件夹(如果删除文件夹,文件夹必须为空目录)
public boolean renameTo(File dest):重命名
参数传递的修改的File对象
```

#### 判断功能

```java
public boolean canRead()是否可读
public boolean canWrite()是否可写
public boolean exists():是否存在
public boolean isFile():是否是文件
public boolean isDirectory():是否是文件夹
public boolean isHidden():是否隐藏
```

#### 高级获取功能

```java
public long length()
public String getName():获取抽象路径 名所表示的文件或者目录的名称
public File[] listFiles():获取某个目录下的所有的文件以及文件夹的File数组
public String[] list():获取某个抽象路径名所表示的文件以及目录的字符串数组
```

需求:获取
D盘下的所有的文件夹以及文件的名称....

```java
public class FileDemo2 {
    public static void main(String[] args) {

        //创建File对象,描述当前项目下的aaa.txt文件
        File file = new File("aaa.txt") ;
        System.out.println(file.canRead());
        System.out.println(file.canWrite());
        System.out.println(file.exists());
        System.out.println(file.isDirectory());//false
        System.out.println(file.isFile());
        System.out.println(file.isHidden());
        System.out.println(file.length());
        System.out.println(file.getName());
        System.out.println("------------------------");

        // public File[] listFiles():获取某个目录下的所有的文件以及文件夹的File数组
        //描述D盘
        File file2 = new File("d://") ;
        File[] fileArray = file2.listFiles();
        //防止空指针异常
        if(fileArray!=null){
            for(File f :fileArray){
                System.out.println(f.getName());
            }
        }
        System.out.println("----------------------------------");
        //public String[] list():获取某个抽象路径名所表示的文件以及目录的字符串数组
        String[] strArray = file2.list();
        if(strArray!=null){
            for(String s:strArray){
                System.out.println(s);
            }
        }


    }
}
```

```
eg.1
需求:
将D:\EE_2106\day25\code路径下logo.jpg---->改名为  D:\EE_2106\day25\code路径下 mv.jpg
如果两个地址一样:只是改名
如果地址不一样,剪切并改名!
```

```java
public class FileDemo {
    public static void main(String[] args) throws IOException {
        //表示:E盘下的demo文件夹中的a.txt文件
        //File(String pathname) 方式1 (推荐)
       // File file = new File("e://demo//a.txt") ;只是表示这个路径,如果创建a.txt文件,系统找不到指定路径

        File file = new File("D:\\EE_2106\\day25\\code\\a.txt"); //绝对路径
        File file2 = new File("aaa.txt");//没有带路径,就默认在当前项目下(相对路径)
        File file3 = new File("D:\\EE_2106\\day25\\code\\demo") ;
        File file4 = new File("aaa\\bbb\\ccc\\ddd") ;

        //File(File parent, String child) 方式2
      /*  File file2 = new File("E://demo") ;
        File file3 = new File(file2,"a.txt") ;

        //File(String parent, String child):方式3
        File file4 = new File("E://demo","a.txt") ;*/
       // public boolean createNewFile()
        System.out.println(file.createNewFile());
        System.out.println(file2.createNewFile());
        System.out.println(file3.mkdir());
        System.out.println(file4.mkdirs());
        System.out.println(file3.delete());
        System.out.println(file.delete());
        System.out.println("------------------------");
        //D:\EE_2106\day25\code路径下logo.jpg :描述下这个地址File
       // File srcFile  = new File("D:\\EE_2106\\day25\\code\\logo.jpg") ;
        File srcFile = new File("D:\\EE_2106\\day25\\code\\mv.jpg") ;
        File destFile = new File("高圆圆.jpg") ;//当前项目路径下了
        System.out.println(srcFile.renameTo(destFile)) ;



    }
}
```

eg.2
需求2:
获取D盘下所有的以.jpg结尾的文件

分析:

> 1)描述下D盘
>
> 2)  public File[] listFiles():获取D盘下的所有的文件以及文件夹的File数组
> 2.1)对获取到的数组进行非判断，如果不为null,再去判断
> 2.2)判断File是一个文件
> 2.3)判断:文件必须以.jpg结尾。String类 endsWith(".jpg")
>
> 提供了另一个重载功能
>
> ```java
> public File[] listFiles(FilenameFilter filter)
> String[] list(FilenameFilter filter)
> //参数为:文件名称过滤器FilenameFilter:接口
> ```
>
> 成员方法
>
> ```java
> boolean accept(File dir,String name);//测试指定文件是否包含在文件列表中
> 返回如果true,将文件添加到文件列表中
> ```
>
> 1)描述下D盘
>
> 2)public File[] listFiles(FilenameFilter filenamefilter):
>
> 获取D盘下的File数组的时候,就已经指定文件进行过滤...

```java
import java.io.File;
import java.io.FilenameFilter;

public class IOtest01 {

    public static void main(String[] args) {

        File file = new File("e://");

        File[] files1 = file.listFiles();

        //方法一：
        if (files1 != null) {
            for (File f : files1) {
                if (f.isFile()) {
                    if (f.getName().endsWith(".txt")) {
                        System.out.println(f);
                    }
                }
            }
        }

        System.out.println("————————————————");

        //方法二：
        File srcfile = new File("e://");

        //获取当前D盘下的所有文件以及文件夹File数组,并进行文件名过滤
        //public File[] listFiles(FilenameFilter filter)
        File[] files2 = srcfile.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                //返回true:表示将指定文件添加列表文件中
                //描述文件所表示抽象路径File
                File file1 = new File(dir, name);

                //两个条件:file是文件并且file的文件名称以".jpg结尾"
                return file1.isFile() && file1.getName().endsWith(".txt");
            }
        });

        if (files2 != null) {
            for (File f : files2) {
                System.out.println(f.getName());
            }
        }
    }
}
```

## 递归

#### 方法递归

方法调用方法本身的一种现象,并非是方法嵌套方法

#### 前提条件

1)必须有一个成员方法

2)必须有方法递归的出口条件(结束条件),如果没有出口条件,就是死递归...

3)还存在一定的规律

#### 注意事项

构造方法不存在递归

eg.1 5的阶乘

```java
public class DiGuiDemo {

   public static void main(String[] args) {
       System.out.println("5的阶乘是:"+jieCheng(5));
   }

    //递归:需要考虑(一定要有规律)
    private static int jieCheng(int i) {
       if(i==1){
           return 1 ;
       }else{
           return i * jieCheng(i-1) ;
       }
    }

}
```

eg.2 需求:使用递归方式去删除D盘某个demo文件夹中有很多个目录。

```java
public class DiGuiTest {
    public static void main(String[] args) {

         //描述D盘的demo文件夹
         File srcFloder = new File("d://demo") ;//想删的文件夹地址
         //调用递归删除的方法
        deleteFloder(srcFloder) ;
    }
    
    public static void deleteFloder(File srcFloder) {
        //获取srcFloder下的所有的文件以及文件的File数组
        File[] fileArray = srcFloder.listFiles();
        if(fileArray!=null){
            for(File file:fileArray){
                //获取到每一个file对象
                //如果file是文件夹,继续回到deleteFloder(srcFloder) ; 删除文件夹
                if(file.isDirectory()){
                    deleteFloder(file) ;
                }else{
                    //判断是文件,必须以.java结尾 文件
                    if(file.getName().endsWith(".java")){
                        //删除的同时----获取名称
                        System.out.println(file.getName()+"------------"+file.delete());
                    }
                }

            }
            //删除文件夹
            System.out.println(srcFloder.getName()+"----"+srcFloder.delete());
        }
    }
}
```

## InputStream&OutputStream

### IO流的分类

#### 按流的方向

输入和输出流

#### 按类型分

字节和字符流：字节先出现，后面在有字符流

##### 再次流的方向划分

字节输入流:InputStream:表示输入字节流的所有类的超类(父类)

字节输出流:OutputStream:表示字节输出流的所有类的超类

字符输入流:Reader表示输入字符流的抽象类

字符输出流:Writer表示输出字符流的抽象类

##### 字节/字符流都很多子类

XXXInputStream

XXXOutputStream

XXXReader

XXXWriter

##### 字节输出流

OutputStream抽象类

子类进行实例化FileOutputStream:将指定的内容写到文件中

实现步骤:

1)创建文件输出流对象 :

```java
FileOutputStream(String name) :推荐:可以指定参数地址
FileOutputStream(File file)
```

2)写数据

```java
public void write(int b) throws IOException 写一个字节
public void write(byte[] bytes) throws IOException 写一个字节数组
public void write(byte[] bytes,int off,int len) throws IOException:写一部分字节数组
```


​	3)关闭资源

```java
public class FileOutputStreamDemo {
    public static void main(String[] args) throws IOException {
        
        //创建文件输出流对象
        //指向某个盘符下的文件中(或者当前项目下)
        FileOutputStream fos = new FileOutputStream("my.txt");//文件需要被创建当前项目下

        //2)写数据
        for (int x = 0; x < 10; x++) {
            fos.write(("hello" + x).getBytes());

            //windows操作系统  "\r\n"代表换换行
            fos.write("\r\n".getBytes());
        }

        //3)关闭资源
        fos.close(); //释放fos流对象所指向的my.txt的系统资源

    }
}
```

### 在字节输出流中加入异常处理(捕获异常)

```java
public class FileOutputStreamDemo2 {

    public static void main(String[] args) {

       // method1() ;
        method2() ;//
    }

    //标准方式:try...catch...finally
    //统一处理
    private static void method2() {
        FileOutputStream fos = null ;
        //alt+ctrl+t--->
        try {
           fos = new FileOutputStream("fos2.txt") ;
            //写数据
            fos.write("hello,我来了".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //释放资源

                if(fos!=null){
                    try {
                        fos.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }

        }
    }

    //分别try...catch...(不用,这种方式阅读性很差)
    private static void method1() {
        //在当前项目下输出fos.txt
        //创建字节输出流对象
        FileOutputStream fos = null ;
        try {
             fos = new FileOutputStream("fos.txt") ;
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        //写数据
        try {
            fos.write("hello,IO".getBytes()) ;
        } catch (IOException e) {
            e.printStackTrace();
        }

        //关闭资源
        try {
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 流的拷贝

```
字节输入流:InputStream
子类:FileInputStream

实现步骤:
	需要读取当前项目下的fis.txt文件
	1)创建文件字节输入流对象,指向 fis.txt
		public FileInputStream(String name) throws FileNotFoundException
	2)读内容
		read(byte[] bytes):一次读取一个字节数组
		read():一次读取一个字节
	3)释放资源

	读取文件的时候,一次读取一个字节,存在中文乱码, 在最终输出的结果 (char)by   (场景:将某一个文件内容打印在控制台上了)

	"abc" 英文 读一个字节  a--->97   b --98
	针对中文字符: 现在idea环境: 编码格式:utf-8格式: 一个中文对应三个字节 ,和前面的英文拼接出现问题,只要中文都会乱码
	
	因此才出现了字符流(加入编码和解码的格式)
```

```java
public class FileInputStreamDemo {
    public static void main(String[] args) {
        //创建一个字节输入流对象
        FileInputStream fis = null ;
        try {
          // fis  = new FileInputStream("fis.txt") ;
           fis  = new FileInputStream("DiGuiTest.java") ; //读取当前项目下的DiGuiTest.java
            //读取内容
          /*  //第一次读取
            int by = fis.read(); //字节数
            System.out.println(by);
            System.out.println((char)by); //获取的字符
            //第二次读取
            by  = fis.read();
            System.out.println((char)by);
            //第三次读取
            by = fis.read() ;
            System.out.println((char)by);
            //第四次读取
            by = fis.read() ;
            System.out.println(by);//字节数
            System.out.println((char)by);*/
          //使用循环优化:  结束条件:获取的字节数为-1

            //当前不知道循环多少次:while循环
           //将判断,赋值一块去使用 (模板代码)
           int by = 0 ; //字节数为0
           while((by=fis.read())!=-1) {
               System.out.print((char)by);
           }


        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //释放资源
            if(fis!=null){
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

> 读写复制操作:一次读取一个字节的方式
> 一次读取一个字节数组
>
> 需求:
>
> 将当前项目下的DiGuiTest.java 的内容 复制到D盘下的Copy.java文件中
>
> 分析:
>
> 源文件:  当前项目下 "DiGuiTest.java"
>
> 封装源文件:FileInputStraem(String pathname)一次读取一个字节
>
> 目的地文件:  D://Copy.java
>
> 封装目的地文件:FileOutputStream(String pathname)一次写一个字节

```java
public class CopyFileDemo {
    public static void main(String[] args) {

        long start = System.currentTimeMillis() ;//时间毫秒值

       // method("DiGuiTest.java","D://Copy.java") ;
        method2("DiGuiTest.java","D://Copy.java") ;

        long end  = System.currentTimeMillis() ;
        System.out.println("共耗时:"+(end-start)+"毫秒");
    }

    private static void method2(String srcFile, String destFile) {
        FileInputStream fis  = null ;
        FileOutputStream fos = null ;
        try {
            //  封装源文件:FileInputStraem(String pathname)
            //字节输入流
            fis = new FileInputStream(srcFile) ;
            //字节输出流
            // 封装目的地文件:FileOutputStream(String pathname)
            fos = new FileOutputStream(destFile) ;

            //读写复制操作
            //一次读取一个字节数组
            byte[] bytes = new byte[1024] ;
            int len = 0 ;
            while((len=fis.read(bytes))!=-1){
                //赋值
                //fos流对象中写
                //带上len的使用
                fos.write(bytes,0,len);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }finally {

            try {
                fos.close();
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }


    /**
     * 一次读取一个字节:读写复制
     * @param srcFile  源文件
     * @param destFile 目的地文件
     */
    private static void method(String srcFile, String destFile) {
        FileInputStream fis  = null ;
        FileOutputStream fos = null ;
        try {
            //  封装源文件:FileInputStraem(String pathname)
            //字节输入流
            fis = new FileInputStream(srcFile) ;
            //字节输出流
            // 封装目的地文件:FileOutputStream(String pathname)
            fos = new FileOutputStream(destFile) ;

            //读写复制操作
            //一次读取一个字节
            int by = 0 ;
            while((by=fis.read())!=-1){
                //没有读完,继续复制 :写一个字节
                fos.write(by);

            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {

            try {
                fos.close();
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public int read(byte[] b)throws IOException//一次读取一个字节数组，返回值值的是每次读取的实际字节数
需求：将fis2.txt读取出来,并展示在控制台上
```

```java
public class FileInputStreamDemo2 {
    public static void main(String[] args) {
        //创建字节输入流对象
        FileInputStream fis = null ;
        try {
            fis = new FileInputStream("fis2.txt") ;

         //创建一个数组:长度:1024或者1024的整数倍
          byte[] buffer = new byte[1024] ;   //长度虽然1024个长度
          int len = 0 ;
          while((len=fis.read(buffer))!=-1){
              //每次获取的从0开始获取实际字节长度
              System.out.println(new String(buffer,0,len)) ;
          }

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

## 字节流

### 字符流和字节流读取文件的方式

```java
import java.io.*;

public class BufferedTest {

    public static void main(String[] args) {

        long start = System.currentTimeMillis();

		Copy1("srcFile","destFile");
        Copy2("srcFile","destFile");
        Copy3("srcFile","destFile");
        Copy4("srcFile","destFile");

        long end = System.currentTimeMillis();

        System.out.println("本次共运行了"+(end-start)+"毫秒");

    }

    //基本的字节流一次读取一个字节
    public static void Copy1(String srcFile,String destFile)throws IOException{

        FileInputStream fis = null;
        FileOutputStream fos = null;

        fis = new FileInputStream(srcFile);
        fos = new FileOutputStream(destFile);

        int by = 0;
        while((by=fis.read())!=-1){
            fos.write(by);
        }

        fos.close();
        fis.close();

    }

    //基本的字节流一次读取一个字节数组
    public static void Copy2(String srcFile,String destFile)throws IOException{

        FileInputStream fis = null;
        FileOutputStream fos = null;

        fis = new FileInputStream(srcFile);
        fos = new FileOutputStream(destFile);

        byte[] bytes = new byte[1024];
        int len = 0;
        while((len=fis.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }

        fos.close();
        fis.close();

    }

    //缓冲流一次读取一个字节
    public static void Copy3(String srcFile,String destFile)throws IOException{

        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        bis = new BufferedInputStream(new FileInputStream(srcFile));
        bos = new BufferedOutputStream(new FileOutputStream(destFile));

        int by = 0;
        while((by=bis.read())!=-1){
            bos.write(by);
        }

        bos.close();
        bis.close();

    }

    //缓冲流一次读取一个字节数组
    public static void Copy4(String srcFile,String destFile)throws IOException{

        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        bis = new BufferedInputStream(new FileInputStream(srcFile));
        bos = new BufferedOutputStream(new FileOutputStream(destFile));

        byte[] bytes = new byte[1024];
        int len = 0;
        while((len=bis.read(bytes))!=-1){
            bos.write(bytes,0,len);
        }

        bos.close();
        bis.close();

    }

}
```

## 字符流

### Reader/Writer子类:转换流

```java
InputStreamReader(InputStream in)
OutputStreamWriter(OutputStream out)

//他们不能直接去操作文件,jdk提供了这两种类型的便捷类,可以直接操作文件
FileReader(File file)
FileReader(String pathname)

FileWriter(File file)
FileWriter(String filename)

//这两个类:使用的平台的默认编码和解码 (utf-8)
```
#### Reader:抽象类

具体的子类:字符转换输入流 InputStreamReader

##### 构造方法

```java
InputStreamReader(InputStream in) ;//使用平台默认解码集进行读
InputStreamReader(InputStream in,String charset) ;//使用指定的解码集进行读
```

字符流的出现是在字节流的后面,可以解决中文乱码问题

#### Writer:抽象类

提供子类:字符转换输出流: 字节输出流通向字符输出流的桥梁!

##### 构造方法

```java
OutputStreamWriter(OutputStream out) ;//使用平台默认编码集写入数据
OutputStreamWriter(OutputStream out, String charsetName) ;//使用指定的字符集进行编码 写入数据
```

```java
import java.io.*;

public class Reader_WriterTest01 {

    public static void main(String[] args) throws IOException {

        InputStreamReader isr = new InputStreamReader(new FileInputStream("D:\\IO操作\\c.txt"));
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("D:\\IO\\c3.txt"));

        char[] chars = new char[1024];
        int len = 0;
        while((len = isr.read(chars))!=-1){
            osw.write(chars,0,len);
            System.out.println(new String(chars,0,len));
            osw.flush();
        }

        osw.close();
        isr.close();

    }

}
```

```
asdasdas
asdasd
sfdsdg
adas
as11223
aassa
asda
s2121
```

```
public class Reader_WriterTest02 {
    public static void main(String[] args) throws IOException {

        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("D:\\千锋\\IO\\c4.txt"));

        String s = "12345643213224";
        char[] chars =s.toCharArray();

        osw.write(chars);
        osw.flush();

        osw.close();

    }
}
```



```
BufferedReaer/BufferedWriter  :读写复制操作
	一次读取一个字符
	一次读取一个字符数组

特有功能
	利用BufferedReader的readLine()读取一行
	利用BufferedWriter写一行,然后换行

一个文本文件读写复制:
阻塞流 (传统的IO流)
    当一个线程如果操作的是读的动作,read(byte[] byte/char[] ..)/readLine():都属于阻塞式方法
    另一个线程如果操作的是写的动作,读的线程如果开始读,这边写的线程才能开始进行写的复制操作!

基本的字节流:一次读取一个字节/一次读取一个字节数组
字节缓冲流:一次读取一个字节/一次读取一个字节数组

字符转换流:
    InputStreamReader(InputStream in)
    OutputStreamWriter(OutputStream out)
    一次读取一个字符/一次读取一个字符数组

转换流的便捷类
    FileReader(String name)
    FileWriter(String writer)
    一次读取一个字符/一次读取一个字符数组

    BufferedReader
    BufferedWriter
    一次读取一个字符/一次读取一个字符数组
特有功能:
    一次读取一行内容


```

#### BufferedReader：字符缓冲输入流

> BufferedReader键盘录入数据 
> Scanner(InputSteram in)、String nextLine()
> BufferedReader(Reader in);
>
> InputStreamReader(InputStream in);//转换流
>
> 如果直接进行读的操作(直接操作文件)FileReader(String pathName)
>
> 创建使用默认大小的输入缓冲区的缓冲字符输入流。
>
> BufferedReader(Reader in, int size):指定缓冲区大小

##### 特有功能:

public String readLine() :一次读取一行内容

#### BufferedWriter:字符缓冲输出流

> BufferedWriter(Writer out) :创建默认的缓冲区大小:  默认大小:defaultcharbuffersize:8192(默认值足够大)
>
> BufferedWriter(Writer out, int sz)  :指定的大小
>
> public void newLine() throws IOException:写入行的分隔符号
>
> BufferedReaer/BufferedWriter  :读写复制操作
> 一次读取一个字符
> 一次读取一个字符数组

##### 特有功能:

利用BufferedReader的readLine()读取一行
利用BufferedWriter写一行,然后换行

eg.将当前项目下的ReaderDemo.java复制到当前项目下:copy.java

```java
import java.io.*;

//一次读取一行
public class Reader_WriterTest03 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = null;
        BufferedWriter bw = null;

        br = new BufferedReader(new FileReader("D:\\IO操作\\c.txt"));
        bw = new BufferedWriter(new FileWriter("D:\\IO\\c5.txt"));

        String line = null;
        while((line = br.readLine())!=null){
            bw.write(line);
            bw.newLine();
            bw.flush();
        }

        bw.close();
        br.close();

    }

}
```

## 合并流

### SequenceInputStream:

#### 构造方法

```java
public SequenceInputStream(InputStream s1,InputStream s2)//参数1和参数2:分别要读取的字节输入流对象
public SequenceInputStream(Enumeration<? extends InputStream> e) ;//将两个以上的文件进行读取
Vector<InputStream>add//(添加多个字节输入流对象)
```

#### eg.

```java
public class SequenceTest {

    public static void main(String[] args) throws IOException{

        InputStream isr1 = new FileInputStream("D:\\IO\\c1.txt");
        InputStream isr2 = new FileInputStream("D:\\IO\\c2.txt");
        InputStream isr3 = new FileInputStream("D:\\IO\\c3.txt");
        InputStream isr4 = new FileInputStream("D:\\IO\\c4.txt");
        InputStream isr5 = new FileInputStream("D:\\IO\\c5.txt");

        Vector<InputStream> vector = new Vector();


        vector.add(isr1);
        vector.add(isr2);
        vector.add(isr3);
        vector.add(isr4);
        vector.add(isr5);

        //public Enumeration<E> elements() ---- >类似于Collection的Iterator迭代器
        Enumeration<InputStream> enumeration = vector.elements();

        //创建合并流对象  封装源文件
        SequenceInputStream sis = new SequenceInputStream(enumeration);

        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("D:\\IO\\c.txt"));

        int b = 0;
        while((b = sis.read())!=-1){
            bos.write(b);
            bos.flush();
        }

        bos.close();
        sis.close();

    }

}
```

## 序列化&反序列化

### 序列化

ObjectOutputStream

将某个实体对象写入到流对象中---->将一个对象变成流数据

### 构造方法

```java
protected ObjectOutputStream()
protected ObjectOutputStream(OutputStream out) :将某个对象通过底层的基本字节输出进行写的动作
public final void writeObject(Object obj) throws IOException
```

### 反序列化

ObjectInputStream

将流数据----还原成 "对象"

### 构造方法

```java
public ObjectInputStream(InputStream in)
public final Object readObject()
throws IOException, ClassNotFoundException
```

> java.io.NotSerializableException: com.qf.serailizable_05.Person
>
> NotSerializableException:未实现序列化接口的异常
> 一个类在进行序列化的时候,(将类的对象写入序列化流中),标记接口Serializable 它有个特点为当前这个Person进行编码(为类以及类的成员----->序列化化版本ID(签名):SerialversonUID)
>
> java.io.InvalidClassException: com.qf.serailizable_05.Person; local class incompatible:stream classdesc serialVersionUID = 2588921225545403990,local class serialVersionUID = -862808041229244683
>
> 在进行序列化的时候:当前的serialVersionUID:序列化版本id号 (假设100):内存中生成一个id值
> 在进行反序列化的时候:将流指向对象:Person类的字段信息更改了(类的签名信息就会被更改),那么直接进行反序列化产生的serialVersionUID=(假设200)
> 两个序列ID不一样
> 序列化版本id保证，在idea中生成一个固定的序列版本id号 (一般都针对实体类)
>

一个实体类:

​    1)当前类必须为具体类 class 类名{}

​    2)必须存在私有字段(私有成员变量)

​    3)必须提供公共的setXXX()/getXXX()

​    4)如果当前类需要在网络中进行传输(分布式系统中)必须implements Serializable

idea--->file--->setting---->editor---->Inspections----->java---->序列化serializion issue--->serializable class  打上对钩即可!

需求:将Person p = new Person("高圆圆",42) ;---->变成流对象 进行传输

```java
import java.io.Serializable;

public class Person implements Serializable {

    //产生一个固定的序列化版本Id
    private static final long serialVersionUID = 8988325735017562383L; //常量值

    String name;
    int age; //transient:想让某个字段不参与序列化:这个字段(成员变量就使用transient)

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException, ClassNotFoundException {

        myWriter();

        myReader();

    }

    //序列化
    public static void myWriter() throws IOException {
        //创建Person对象
        Person person = new Person("老大", 12);

        //protected ObjectOutputStream(OutputStream out):创建一个序列化流对象
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:\\IO操作\\a.txt"));

        //将p对象写入到oos流对象中
        //public final void writeObject(Object obj)throws IOException
        oos.writeObject(person);

        //释放资源
        oos.close();
    }

    //反序列化
    public static void myReader() throws IOException, ClassNotFoundException {
        //创建反序列化流对象
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:\\IO操作\\a.txt"));

        //读
        //public final Object readObject()
        Object o = ois.readObject();

        //关闭流
        ois.close();
        System.out.println(o);
    }

}
```

```
Person{name='老大', age=12}
```

## Properties

### 概念

> Properties extends Hashtable<K,V> ,它没有泛型,Key和Value都是String
>
> 表示一组持久的属性。 Properties可以保存到流中或从流中加载。
>
> 属性列表中的每个键及其对应的值都是一个字符串。

1)可以使用Map的功能

```
put(K ,V)
遍历:keySet()通用
```

2)有自己特有功能添加元素和遍历

```
public Object setProperty(String key,String value):给属性列表中添加属性描述(key和value)
public Set<String> stringPropertyNames():获取属性列表中的所有的键
public String getProperty(String key):在属性列表中通过键获取值

Propeties:键和值都是字符串类型:可能需要在src(类所在的路径:类路径)
```

> 作为一个配置文件
>
> 将字节输入流或者字符输入中所在的文件中的内容加载属性列表中
>
> ```java
> void load(Reader reader)
> void load(InputSteram in)
> ```
>
>
> 将Properties里面存储的内容，保存到某个目录的xxx配置文件中保存
>
> ```java
> public void store(Writer writer,String comments)
> public void store(OutputStream out,String comments)
> ```
>
>
> 将属性列表中的内容保存到指定字节输出流/字符输出流所指向那个文件中

eg.1

```java
import java.util.Properties;
import java.util.Set;

public class PropertiesTest01 {
    public static void main(String[] args) {
        //Properties() :空参构造
        Properties properties = new Properties();

        // System.out.println(properties);
        //利用Map集合的功能去添加元素
        properties.put("高圆圆", "赵又廷");
        properties.put("文章", "姚笛");
        properties.put("文章", "马伊琍");
        properties.put("王宝强", "马蓉");
        // System.out.println(properties);

        //遍历
        Set<Object> keySet = properties.keySet();
        for (Object key : keySet) {
            //通过key获取value
            Object value = properties.get(key);
            System.out.println(key + "---" + value);
        }

        System.out.println("---------------------------------------");

        //创建一个空的属性列表
        Properties prop = new Properties();
        prop.setProperty("张三", "30");
        prop.setProperty("李四", "40");
        prop.setProperty("王五", "35");
        prop.setProperty("赵六", "45");


        //遍历:
        Set<String> set = prop.stringPropertyNames();//获取所有键
        for (String key : set) {
            String value = prop.getProperty(key);
            System.out.println(key + "---" + value);

        }

    }
}
```

```
高圆圆---赵又廷
王宝强---马蓉
文章---马伊琍
---------------------------------------
赵六---45
王五---35
张三---30
李四---40
```

eg.2

```java
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;
import java.util.Set;

public class PropertiesTest02 {
    public static void main(String[] args) throws IOException{

        myStore();

        myLoad();

    }

    //将属性集合类中的内容--保存到指定文件汇中
    public static void myStore()throws IOException{
        Properties prop = new Properties();

        //添加key和value
        prop.setProperty("张三丰","56");
        prop.setProperty("吴奇隆","60");
        prop.setProperty("成龙","65");
        prop.setProperty("刘德华","70");

        //public void store(Writer writer,String comments):
        //参数2:给当前属性列表中加入一个描述
        //保存指定的文件中
        prop.store(new FileWriter("D:\\IO操作\\d.txt"),"text01");

        System.out.println("保存完毕！");
    }

    //加载：需要将names.txt文件加载到属性列表中
    public static void myLoad()throws IOException{
        //创建一个空的属性列表
        Properties prop = new Properties();
        System.out.println(prop);

        //读取src路径下的names.properties
        //1.获取当前类所在的字节码文件对象
        Class clazz = PropertiesTest02.class;

        //2.获取当前类所在的类加载器Class:public ClassLoader getClassLoader()
        ClassLoader classLoader = clazz.getClassLoader();

        //3.在类加载器中，获取当前资源文件（配置文件names.proprites）所在的输入流对象
        //public InputStream getResourceAsStream(String name)
        //text01.properties必须在src目录下
        InputStream ism = classLoader.getResourceAsStream("text01.properties");
        
        //将inputStream加载到属性集合类中 void load(InputStream in)
        prop.load(ism);

        System.out.println(prop);
        
        //遍历
        Set<String> set = prop.stringPropertyNames();
        for(String s: set){
            String value = prop.getProperty(s);
            System.out.println(s+"--"+value);
        }

    }

}
```

```
保存完毕！
{}
{liso=70, zhangsan=65, liuwu=56, dada=60}
liso--70
zhangsan--65
liuwu--56
dada--60
```

## IO流总结

> ### I0流原理及流的分类
>
> #### Java IO流原理
>
> ​        1.I/O是Input/Output的缩写，I/O技术是非常实用的技术， 用于处理数据传输。如读/写文件，网络通讯等。
>
> ​        2.Java程序中， 对于数据的输入/输出操作以"流(stream)”的方式进行。
>
> ​        3.java.io包 下提供了各种"流”类和接口，用以获取不同种类的数据，并通过方法输入或输出数据。
>
> ​        4.输入input:读取外部数据(磁盘、光盘等存储设备的数据)到程序(内存)中。
>
> ​        5.输出output:将程序(内存)数据输出到磁盘、光盘等存储设备中。
>
> #### 流的分类
>
> ​        按操作数据单位不同分为:字节流(8 bit)二进制文件，字符流(按字符)文本文件
>
> ​        按数据流的流向不同分为:输入流，输出流
>
> ​        按流的角色的不同分为:节点流，处理流/包装流
>
> | 抽象基类 | 字节流       | 字符流 |
> | -------- | ------------ | ------ |
> | 输入流   | InputStream  | Reader |
> | 输出流   | OutputStream | Writer |
>
> ### FileReader 和FileWriter介绍
>
> ​        FileReader和FileWriter是字符流，即按照字符来操作io
>
> ####     FileReader相关方法:
>
> ​        1) new FileReader(File/String)
>
> ​        2) read:每次读取单个字符，返回该字符，如果到文件末尾返回-1
>
> ​        3) read(char[]):批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1
>
> ​    相关API:
>
> ​        1) new String(char[):将char[]转换成String
>
> ​        2) new String(charD.off,len):将char[]的指定部分转换成String
>
> ####     FileWriter常用方法
>
> ​        1) new FileWriter(File/String):覆盖模式，相当于流的指针在首端
>
> ​        2) new FileWriter(File/String.true):追加模式，相当于流的指针在尾端
>
> ​        3) write(int):写入单个字符
>
> ​        4) write(char[]):写入指定数组
>
> ​        5) write(char[],off,len):写入指定数组的指定部分
>
> ​        6) write (string) :写入整个字符串
>
> ​        7) write(string,off,len):写入字符串的指定部分
>
> ​    相关API:
>
> ​        String类: toCharArray:将String转换成char[]
>
> ​    注意:
>
> ​        FileWriter使用后，必须要关闭(close)或刷新(flush),否则写入不到指定的文件!
>
> ### 节点流和处理流
>
> ####     节点流和处理流的区别和联系
>
> ​        1.节点流是底层流/低级流，直接跟数据源相接。
>
> ​        2.处理流包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。[源码理解]。
>
> ​        3.处理流(也叫包装流)对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连[模拟修饰器设计模式]。
>
> ####     处理流的功能主要体现在以下两个方面:
>
> ​        1.性能的提高:主要以增加缓冲的方式来提高输入输出的效率。
>
> ​        2.操作的便捷:处理流可能提供了一系列便捷的方法来次输入输出大批量的数据， 使用更加灵活方便。
>
> ​    对象流-ObjectlnputStream和ObjectOutputStream
>
> ​        1.将int num = 100这个int数据保存到文件中，注意不是100数字，而是int 100,并且，能够从文件中直接恢复int 100
>
> ​        2.将Dog dog = new Dog(”小黄”, 3)这个dog对象保存到文件中，并且能够从文件恢复.
>
> ​        3.上面的要求， 就是能够将基本数据类型或者对象进行序列化和反序列化操作
>
> ​    序列化和反序列化
> ​           1. 序列化就是在保存数据时，保存数据的值和数据类型
> ​           2. 列化就是在恢复数据时，恢复数据的值和数据类型
> ​           3. 需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一:
> ​                 Serializable / 这是一个标记接口
> ​                 Externalizable
>
> ​    注意事项和细节说明
>
> ​        1)读写顺序要一致
>
> ​        2)要求实现序列化或反序列化对象,需要实现Serializable
>
> ​        3)序列化的类中建议添加SerialVersionUID,为了提高版本的兼容性
>
> ​        4)序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient修饰的成员
>
> ​        5)序列化对象时，要求里面属性的类型也需要实现序列化接口
>
> ​        6)序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化
>
> ​    标准输入输出流
>
> ​        System.in标准输入   InputStream     键盘
> ​        System.out标准输出  OutPutStream    显示器
>
> ​    转换流
>
> ​        InputStreamReader
> ​        OutputStreamWriter

# 网络编程

## IP

```
  ip地址：InetAddress

  1.唯一定位一台网络上计算机
  2.127.0.0.1： 本机localhost
  3.ip地址的分类
    IPV4/IPv6
        IPV4 127.0.0.1 4个字节组成，0-255 42亿个 30亿都在北美，亚洲4亿。2011年就用尽
        IPV6 ；128位。8个无符号整数！

    公网-私网
```

```java
public class InetAddressTest {

    public static void main(String[] args) {

        try {
            //获取网站IP地址
            InetAddress inetAddress1 = InetAddress.getByName("www.baidu.com");
            System.out.println(inetAddress1);

            //获取本机地址
            //方法一
            InetAddress inetAddress2 = InetAddress.getByName("127.0.0.1");
            System.out.println(inetAddress2);
            //方法二
            InetAddress inetAddress3 = InetAddress.getLocalHost();
            System.out.println(inetAddress3);

            InetAddress inetAddress4 = InetAddress.getByName("localhost");
            System.out.println(inetAddress4);

            //常用方法
            System.out.println(inetAddress2.getAddress());//可以不记
            System.out.println(inetAddress2.getCanonicalHostName());//规范的名字
            System.out.println(inetAddress2.getHostName());//ip
            System.out.println(inetAddress2.getHostAddress());//域名，或者自己电脑的名字

        } catch (UnknownHostException e) {
            e.printStackTrace();
        }


    }

}
```

```
www.baidu.com/14.215.177.38
/127.0.0.1
LAPTOP-UPUUVGDR/10.13.156.80
localhost/127.0.0.1
[B@1540e19d
127.0.0.1
127.0.0.1
127.0.0.1
```

## Port

```
    端口表示计算机上的一个程序的进程。

    1.一栋楼表示一个ip ，这栋楼里面的 门牌号 就是端口号。
    2.不同的进程有不同的端口号！用来区分软件的。
    3.端口被规定为：0-65535
    4.TCP ，UDP： 每个都有 0-65535 * 2 ，单个协议下，端口号不能冲突。
    5.端口分类
        共有端口0-1023
        HTTP ： 80
        HTTPS ：443
        FTP ： 21
        Telet ： 23
    程序注册端口：1024-49151，分配给用户或者程序
        Tomcat：8080
        Mysql：3306
        Oracle：1521
    动态、私有：49152-65535

        netstat -ano #查看所有端口
        netstat -ano  | findstr "5900" #查看指定的端口
        tasklist | findstr "8696" #查看指定端口的进程
        Ctrl + Shift + ESC
```

```java
import java.net.InetSocketAddress;

public class InetSocketAddressTest {
    public static void main(String[] args) {

        InetSocketAddress socketAddress = new InetSocketAddress("localhost",8080);
        System.out.println(socketAddress);

        System.out.println(socketAddress.getAddress());
        System.out.println(socketAddress.getHostName());//地址
        System.out.println(socketAddress.getPort());//端口


    }
}
```

```
localhost/127.0.0.1:8080
localhost/127.0.0.1
localhost
8080
```

## 网络协议

```
协议：约定，就好比我们现在说的是普通话。

    网络通信协议：
        1.速率
        2.传输码率
        3.代码结构
        4.传输控制

    问题：非常的复杂

    TCP/IP协议簇：实际上是一组协议
    重要：
        TCP：用户传输协议
        UDP：用户数据报协议

    出名的协议：
        TCP
        IP
```

## TCP&UDP

### TCP和UDP的对比

```
是否可靠连接
		UDP 不可靠连接
		TCP 可靠连接
是否建立连接通道
		UDP不需要建立连接通道,实际发送:DatagramPacket(数据报包)
		TCP需要建立连接通道,将数据的传输委派给通道内的最基本的字节流
是否安全
		UDP,不同步,不安全的---执行效率高
		TCP,安全---同步------执行效率低
```

### TCP

```
TCP：打电话
    连接：稳定
    
    三次握手
      A：你愁啥？
      B：瞅你咋地？
      A：干一次！

    四次挥手
      A：我要断开了 （我要走了）
      B：我知道你要断开了（你真的要走了吗？）
      B：你真的断开了吗？（你真的真的要走了吗？）
      A：我真的断开了 （我真的要走了）

    客户端，服务端
    传输完成，释放连接、效率低

    第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。

    第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

    第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。
```

eg.1接收String

```java
/*
 TCP特点:需要建立连接通道(就是以一种字节流的方式写入,读取)
           什么时候建立连接(服务器端如果监听到端口,客户端就立即和服务器端建立连接!)

  TCP客户端写数据
       1)创建TCP客户端的Socket对象
       2)写数据
       3)释放资源
 */
public class SocketDemo {
    public static void main(String[] args) throws IOException {
        //1)创建TCP客户端的Socket对象
        //public Socket(String host,int port)throws UnknownHostException,IOException
        Socket socket = new Socket("127.0.0.1",8888);

        //2)写数据
        //public OutputStream getOutputStream()throws IOException
        OutputStream osm = socket.getOutputStream();//获取通道内的输出流对象
        osm.write("你好，我是客户端".getBytes());

        //3)释放资源
        socket.close();
    }
}
```

```java
/*
 TCP服务器端的实现步骤:
        1)创建服务器端的Socket对象
            public ServerSocket(int port)throws IOException
        2)监听客户端连接
            public Socket accept()throws IOException 返回值就是当前监听到的客户端的Socket对象
        3)获取通道内的输入流
            public InputStream getInputStream() throws IOException
        4)读取数据:一次读取一个字节数组
        5)展示数据 而且获取ip地址
            public InetAddress getInetAddress()
 */
public class ServerDemo {

    public static void main(String[] args) throws IOException {
        //1)创建服务器端的Socket对象
        //public ServerSocket(int port)throws IOException
        ServerSocket socket = new ServerSocket(8888);
        System.out.println("服务器等待链接 ……");

        //2)监听客户端连接
        //public Socket accept()throws IOException 返回值就是当前监听到的客户端的Socket对象
        Socket s = socket.accept();//阻塞式方法，返回值就是当前监听到的客户端的Socket对象

        //3)获取通道内的输入流
        //public InputStream getInputStream() throws IOException
        InputStream ism = s.getInputStream();

        //4)读取数据:一次读取一个字节数组
        byte[] bytes = new byte[1024];
        int len = ism.read(bytes);
        String str = new String(bytes,0,len);

        //5)展示数据 而且获取ip地址
        //public InetAddress getInetAddress()
        String ip = s.getInetAddress().getHostAddress();
        System.out.println(ip+":"+str);

        //关闭
        socket.close();
    }

}
```

```
服务器等待链接 ……
127.0.0.1:你好，我是客户端
```

eg.2接收图片

```java
//客户端
public class ClientTest {
    public static void main(String[] args) throws IOException {

        //创建一个Socket连接
        InetAddress inetAddress = InetAddress.getByName("127.0.0.1");
        int port = 8888;

        Socket socket = new Socket(inetAddress,port);

        //创建一个流,读取文件
        OutputStream osm = socket.getOutputStream();
        //读文件
        FileInputStream fis = new FileInputStream(new File("test.jpg"));
        //写文件
        byte[] bytes = new byte[1024];
        int len;
        while ((len = fis.read(bytes))!=-1){
            osm.write(bytes,0,len);
        }

        socket.shutdownOutput();//我已经输出完了

        //确定服务器接收完毕，才能断开链接
        InputStream inputStream = socket.getInputStream();

        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        byte[] bytes2 = new byte[1024];
        int len2;
        while ((len2 = inputStream.read(bytes2)) != -1) {
            baos.write(bytes2, 0, len2);
        }
        System.out.println(baos.toString());

        //关闭
        baos.close();
        inputStream.close();
        fis.close();
        osm.close();
        socket.close();
    }
}
```

```java
//服务器
public class ServerTest {
    public static void main(String[] args) throws IOException {

        ServerSocket serverSocket = new ServerSocket(8888);

        Socket socket = serverSocket.accept();//阻塞式监听，监听不到，程序不走

        InputStream ism = socket.getInputStream();

        //文件输出
        FileOutputStream fos = new FileOutputStream(new File("new.jpg"));

        byte[] bytes = new byte[1024];
        int len;
        while ((len = ism.read(bytes)) != -1) {
            fos.write(bytes, 0, len);
        }

        //通过客户端接收完毕
        OutputStream os = socket.getOutputStream();
        os.write("我接受完毕了~".getBytes());

        fos.close();
        ism.close();
        socket.close();
        serverSocket.close();

    }
}
```

### UDP

```
UDP：发短信
    1.不连接，不稳定
    2.客户端、服务端：没有明确的界限
    3.不管有没有准备好，都可以发给你…
    4.导弹
    5.DDOS：洪水攻击！（饱和攻击）
    //不需要连接服务器
```

eg.1发送String

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
/*
  Udp编程的发送端：
   1）创建Socket对象
   2)发送内容  :内容数据一种数据报文(报包)DatagramPacket
   3)释放资源
   10.12.156.50
 */
public class UdpSend {
    public static void main(String[] args) throws IOException {

        //1）创建Socket对象
        //DatagramSocket:发送和接收数据报数据包的套接字。
        //public DatagramSocket() throws SocketException
        DatagramSocket ds = new DatagramSocket();

        //2)创建一个数据报包对象DatagramPacket
        //参数1:当前数据的字节数组
        byte[] bytes = "hello,马三奇".getBytes();
        //参数2:当前数据的长度
        int length = bytes.length;
        //参数3:InetAddress:ip地址对象
        //public static InetAddress getByName(String host)
        InetAddress inetAddress = InetAddress.getByName("127.0.0.1");
        //参数4:绑定的端口号
        int port = 12306;
        //DatagramPacket(byte[] buf, int length, InetAddress address, int port)
        DatagramPacket dp = new DatagramPacket(bytes, length, inetAddress, port);

        //3)发送数据报包
        //public void send(DatagramPacket p)
        ds.send(dp);

        //4)释放资源
        ds.close();

    }
}
```

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
/*
 Udp接收端
 1)创建Socket对象
 2)创建一个接收容器:数据报包:DatagramPacket
 3)接收
 4)解析容器的的实际数据大小
 5)展示发送端发送的数据
 */
public class UdpReceive {
    public static void main(String[] args) throws IOException {

        //1)创建Socket对象
        //public DatagramSocket(int port)
        DatagramSocket ds = new DatagramSocket(12306);

        //2)创建一个接收容器:数据报包:DatagramPacket  实际数据没有这么大
        //public DatagramPacket(byte[] buf, int length)
        //自定义字节缓冲区
        byte[] bytes = new byte[1024];
        int lentgth = bytes.length;
        DatagramPacket dp = new DatagramPacket(bytes, lentgth);

        //3)public void receive(DatagramPacket p)
        ds.receive(dp);

        //4)解析实际数据
        byte[] bytes2 = dp.getData();//byte[] getData()   获取实际字节数
        //返回数据缓冲区。
        int length2 = dp.getLength();//int getLength()   获取实际长度

        String receiverStr = new String(bytes2, 0, length2);
        //获取ip地址
        //InetAddress getAddress()
        //InetAddress
        //public String getHostAddress():
        String ip = dp.getAddress().getHostAddress();

        //展示数据
        System.out.println("data from " + ip + " ,content is :" + receiverStr);

    }
}
```

eg.2接收键盘的字符串输入

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;

public class Sender {
    public static void main(String[] args) throws Exception {

        //发送端
        DatagramSocket socket = new DatagramSocket(8888);
        //接收键盘录入
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        while (true) {
            //按行将键盘录入对象以字符串的形式存储
            String data = reader.readLine();
            //再将字符串内容以转换为byte存放在byte数组中
            byte[] bytes = data.getBytes();
            //打包
            DatagramPacket packet = new DatagramPacket(bytes, 0, bytes.length, new InetSocketAddress("127.0.0.1", 6666));
            //发包
            socket.send(packet);
            //结束条件
            if(data.equals("bye")){
                break;
            }

        }
        socket.close();

    }

}
```

```java
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class Receive {
    public static void main(String[] args) throws Exception {

        DatagramSocket socket = new DatagramSocket(6666);

        while (true) {
            //准备接收包裹
            byte[] bytes = new byte[1024];
            //打一个空包，来接收包裹
            DatagramPacket packet = new DatagramPacket(bytes, 0, bytes.length);
            //阻塞式接收包裹
            socket.receive(packet);

            //用byte数组存储接收到的内容
            byte[] data = packet.getData();
            //再将接收到的文件，转成字符串
            String datas = new String(data, 0, data.length);
            //输出接受到的内容
            System.out.println(datas);

            //断开连接
            if (datas.equals("bye")) {
                break;
            }

        }
        socket.close();
    }

}
```

eg.3实现简单的聊天对话

```java
//用户一
public class Student {
    public static void main(String[] args) {
        new Thread(new Sender(6666,"localhost",8888)).start();
        new Thread(new Receive("老大哥",7777)).start();
    }
}
```

```java
//用户二
public class Teacher {
    public static void main(String[] args) {
        new Thread(new Sender(5555,"localhost",7777)).start();
        new Thread(new Receive("黑社会",8888)).start();
    }
}
```

```java
import java.net.DatagramPacket;
import java.net.DatagramSocket;

//接收端

public class Receive implements Runnable {

    DatagramSocket socket = null;
    private String msgFrom;//发送信息的人
    private int Port;

    public Receive(String msgFrom, int toPort) {
        this.Port = toPort;
        this.msgFrom = msgFrom;
        try {
            socket = new DatagramSocket(this.Port);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    @Override
    public void run() {

        while (true) {
            try {
                byte[] bytes = new byte[1024];
                DatagramPacket packet = new DatagramPacket(bytes, 0, bytes.length);
                socket.receive(packet);

                byte[] data = packet.getData();
                String datas = new String(data, 0, data.length);
                System.out.println(msgFrom+":"+datas);

                if (datas.equals( "bye")) {
                    System.out.println("对方已下线");
                    break;
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        socket.close();
    }
}
```

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;

//发送端

public class Sender implements Runnable {

    DatagramSocket socket = null;
    BufferedReader reader = null;

    private int fromPort;
    private String toIP;
    private int toPort;

    public Sender(int fromPort, String toIP, int toPort) {
        this.fromPort = fromPort;
        this.toIP = toIP;
        this.toPort = toPort;

        try {
            socket = new DatagramSocket(fromPort);
            reader = new BufferedReader(new InputStreamReader(System.in));
        } catch (Exception e) {
            e.printStackTrace();
        }

    }


    @Override
    public void run() {

        while (true) {
            try {
                String line = reader.readLine();
                byte[] bytes = line.getBytes();
                DatagramPacket packet = new DatagramPacket(bytes, 0, bytes.length, new InetSocketAddress(this.toIP, this.toPort));

                socket.send(packet);

                if (line.equals("bye")) {
                    System.out.println("您已下线");
                    break;
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        socket.close();
    }

}
```

## URL

eg.1

```java
import java.net.MalformedURLException;
import java.net.URL;

public class Test {
    public static void main(String[] args) throws MalformedURLException {
        URL url = new URL("https://blog.csdn.net/m0_59537084/article/details/119567722?spm=1001.2014.3001.5501");

        System.out.println(url.getProtocol());//协议名
        System.out.println(url.getHost());//主机IP
        System.out.println(url.getPort());//端口
        System.out.println(url.getPath());//文件
        System.out.println(url.getFile());//全路径
        System.out.println(url.getQuery());//参数

    }
}
```

eg.2将网上的照片下载下来

```java
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URL;

public class Test02 {
    public static void main(String[] args) throws Exception {
        //1.下载地址
        URL url = new URL("http://img-arch.pconline.com.cn/images/upload/upc/tx/photoblog/1210/26/c2/14676275_14676275_1351218242935_mthumb.jpg");
        
        //2.链接到这个资源
        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();

        InputStream ism = urlConnection.getInputStream();

        FileOutputStream fos = new FileOutputStream("url.jpg");

        byte[] bytes = new byte[1024];
        int len ;
        while((len = ism.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }

        fos.close();
        ism.close();

    }
}
```

## 网络多线程

TCP多线程

# 反射

```
什么是反射?
     能够获取当前某个类的字节码文件对象Class,那么就可以获取当前类的构造器并且创建当前类实例,
     还可以获取当前类的成员变量并去赋值,或者获取当前类的成员方法并去调用!

如何获取一个类的字节码文件对象?

     Object类的getClass()获取  获取当前某个类的实例(正在运行的类)
     任意Java类型的class属性

     Class类的静态功能
     public static Class<?> forName(String className)
```

eg.Person类

```java
public class Person {

    private  String name;
    int age;
    String Address;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name, int age, String address) {
        this.name = name;
        this.age = age;
        Address = address;
    }

    public void show(){
        System.out.println("show Person");
    }

    private String function(String str){
        return str;
    }

    void method(String message,int num){
        System.out.println(message+num);
    }

    public int method2(){
        return 100;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", Address='" + Address + '\'' +
                '}';
    }
}
```

## 使用反射获取字节码文件对象

```java
public class ReflectTest01 {
    public static void main(String[] args) throws ClassNotFoundException {

        //获取Person类的字节码文件对象:Class
        Person p = new Person() ;
        Class c1 = p.getClass();
        System.out.println(c1);//class 全限定名称
        Person p2  = new Person() ;
        Class c2 = p2.getClass() ;
        System.out.println(c2);
        System.out.println(c1==c2);
        System.out.println(p==p2);//两个对象

        System.out.println("----------------------------");

        Class c3 = Person.class ;
        System.out.println(c3);
        System.out.println(c1==c3);

        System.out.println("-----------------------------");
        //知道某个一类的全限定名称了,也可以获取当前类的字节码文件对象
        // Class c4 = Class.forName("Person"); //参数字符串:全限定名称
        Class c4 = Class.forName("HemoWork.day28.ClassTest.ReflectTest.Person");//参数字符串:全限定名称
        System.out.println(c4);
        System.out.println(c1==c4);

        //第二种方式和第三种使用
        // 推荐第三种
        // 因为参数为String------>就可以存储在配置文件中

    }
}
```

```
class HemoWork.day28.ClassTest.ReflectTest.Person
class HemoWork.day28.ClassTest.ReflectTest.Person
true
false
----------------------------
class HemoWork.day28.ClassTest.ReflectTest.Person
true
-----------------------------
class HemoWork.day28.ClassTest.ReflectTest.Person
true
```

## 使用反射建立对象

```java
/*
使用反射(底层)如何创建p对象的!
 */
public class ReflectTest02 {
    public static void main(String[] args) throws Exception {
        //使用反射的方式创建当前类的实例
        //1、获取Person类的字节码文件对象
        Class personClass = Class.forName("HemoWork.day28.ClassTest.ReflectTest.Person");

        //2、获取构造器对象
        //public Constructor<?>[] getConstructors():获取当前字节码文件对象中(正在运行的这个类)里面所有的公共的构造方法所在的对象
        //public Constructor<?>[] getDeclaredConstructors():获取所有的构造器对象Constructor,返回构造器对象数组  包含私有化构造/默认的
        Constructor constructor = personClass.getConstructor();

        //通过构造器对象创建当前类的实例
        //public T newInstance(Object... initargs): 参数为最终赋值的实际参数 (可变参数:实际参数未知)
        Object obj = constructor.newInstance();
        System.out.println(obj);

    }
}
```

```
Person{name='null', age=0, Address='null'}
```

## 使用反射录入属性内容

### 为普通对象赋值

eg.1

```
import java.lang.reflect.Constructor;

public class ReflectTest03 {
    public static void main(String[] args) throws Exception {

        //1、获取类的字节码文件对象
        Class clazz = Class.forName("HemoWork.day28.ClassTest.ReflectTest.Person");

        //2、获取构造Constructor(构造方法所在对象）
        //public Constructor<T> getDeclaredConstructor(类<?>... parameterTypes) :参数都是参数类型的字节码文件对象
        // 公共的构造函数对象:public Constructor<T> getConstructor(Class<?>... parameterTypes)
        Constructor constructor = clazz.getConstructor(String.class,int.class);//通过构造器创建当前类的实例，getDeclaredConstructor

        //取消Java语言访问检查(反射中使用到)
        //public void setAccessible(boolean flag)参数为true,取消Java语言访问检查
        constructor.setAccessible(true);

        Object obj = constructor.newInstance("高圆圆", 20);
        System.out.println(obj);//Person{name='高圆圆', age=20,
    }
}
```

```
Person{name='老大哥', age=20, Address='null'}
```

### 为私有属性赋值

```java
import java.lang.reflect.Constructor;

public class ReflectTest04 {
    public static void main(String[] args) throws Exception {

        Class clazz = Class.forName("HemoWork.day28.ClassTest.ReflectTest.Person");

        Constructor constructor = clazz.getConstructor(String.class,int.class,String.class);

        //参数为true,取消Java语言访问检查
        constructor.setAccessible(true);

        Object object = constructor.newInstance("老大哥",28,"辽宁");

        System.out.println(object);
    }
}
```

```
Person{name='老大哥', age=28, Address='辽宁'}
```

## Field方法

### 给属性赋值

```java
import java.lang.reflect.Field;

public class ReflectTest05 {
    public static void main(String[] args) throws Exception {

        Class clazz = Class.forName("HemoWork.day28.ClassTest.ReflectTest.Person");

        //方式1:直接获取当前类的实例
        //public T newInstance()
        Object object = clazz.newInstance();

        //方式2:通过获取构造函数对象,创建当前类实例
        /*
        1、获取构造器对象
        Constructor constructor = clazz.getConstructor();
        Object obj = constructor.newInstance();
        System.out.println(obj);
        */

        /*
        2、Class类
            获取所有的公共的成员变量Field
            public Field[] getFields()throws SecurityException
            public Field[] getDeclaredFields():所有的字段所在的Field对象
            这包括公共，受保护，默认（包）访问和私有字段，但不包括继承的字段。
            Field[] fields = clazz.getFields() ;
            Field[] fields = clazz.getDeclaredFields() ;
         */

        //现在访问name   private String name ;
        Field nameField = clazz.getDeclaredField("name");
        nameField.setAccessible(true);
        nameField.set(object,"老大哥");
        System.out.println(object);

        //int age
        Field ageField = clazz.getDeclaredField("age");
        ageField.set(object,28);
        System.out.println(object);

        //String Address
        Field AddressField = clazz.getDeclaredField("Address");
        AddressField.setAccessible(true);
        AddressField.set(object,"东北");
        System.out.println(object);

    }
}
```

```
Person{name='老大哥', age=0, Address='null'}
Person{name='老大哥', age=28, Address='null'}
Person{name='老大哥', age=28, Address='东北'}
```

### 调用方法

```java
import java.lang.reflect.Method;

public class ReflectTest06 {
    public static void main(String[] args) throws Exception {
        Class clazz = Class.forName("HemoWork.day28.ClassTest.ReflectTest.Person");

        //获取实例
        Object object = clazz.newInstance();

        /*
        2)获取所有的成员方法:公共的(包含他父类的所有的公共的方法)
            public Method[] getMethods()
            public 方法[] getDeclaredMethods():通过此表示类对象，包括公共，保护，默认（包）访问和私有方法，但不包括继承的方法。
            Method[] methods = clazz.getMethods();
         */

        Method showMethod = clazz.getMethod("show");
        showMethod.invoke(object);

        //调用Person类中function
        //获取function方法的Method对象
        //public Method getDeclaredMetho(String name,Class<?> ..parameterTypes),可访问各种方法
        Method functionMethod = clazz.getDeclaredMethod("function", String.class);
        functionMethod.setAccessible(true);
        Object o = functionMethod.invoke(object,"工作");
        System.out.println(o);

        Method methodMethod = clazz.getDeclaredMethod("method", String.class, int.class);
        Object o1 = methodMethod.invoke(object,"工作",12);

    }
}
```

```
show Person
工作
工作12
```

eg.现在有一个ArrayList<Integer>,里面有一些元素,如何给ArrayList添加String类型的元素呢?

```java
import java.lang.reflect.Method;
import java.util.ArrayList;

public class ReflectTest07 {
    public static void main(String[] args) throws Exception {

        ArrayList<Integer> arrayList = new ArrayList<>();
        arrayList.add(100);
        arrayList.add(200);
        arrayList.add(300);
        arrayList.add(400);
        System.out.println(arrayList);

        //1)获取当前集合实例所在的字节码文件对象
        //通过Object的getClass()获取当前实例所在的类的字节码对象
        Class clazz = arrayList.getClass();

        //System.out.println(obj);
        //System.out.println(clazz);//class java.util.ArrayList
        //获取当前类中 add方法所在的Method
        //public boolean add(E e)  ://参数就是任意Java类型 (Element)
        Method method = clazz.getMethod("add",Object.class);

        //调用方法
        method.invoke(arrayList,"hello");
        method.invoke(arrayList,"adc");
        method.invoke(arrayList,"top");

        System.out.println(arrayList);

    }
}
```

```
[100, 200, 300, 400]
[100, 200, 300, 400, hello, adc, top]
```

## 读取properties文件，利用反射调用方法

myClass.Properties文件

```properties
className=HemoWork.day28.ClassTest.ReflectTest.RefledtTest08.Student
methodName=work
```

```java
//Worker类
public class Worker {
    public void work(){
        System.out.println("工人爱打工");
    }

    //HemoWork.day28.ClassTest.ReflectTest.RefledtTest08.Worker
}
```

```java
//学生类
public class Student {
    public void work(){
        System.out.println("学生爱学习");
    }

//HemoWork.day28.ClassTest.ReflectTest.RefledtTest08.Student
}
```

```java
import java.io.InputStream;
import java.lang.reflect.Method;
import java.util.Properties;

public class ReflectTest {
    public static void main(String[] args) throws Exception {

        InputStream ism = ReflectTest.class.getClassLoader().getResourceAsStream("myClass.properties");//放在src目录下

        Properties prop = new Properties();
        prop.load(ism);

        String classname = prop.getProperty("className");
        String methodname = prop.getProperty("methodName");

        Class clazz = Class.forName(classname);

        Object object = clazz.newInstance();

        Method method = clazz.getMethod(methodname);

        method.invoke(object);


    }
}
```

```
学生爱学习//HemoWork.day28.ClassTest.ReflectTest.RefledtTest08.Student
```

```
工人爱工作//HemoWork.day28.ClassTest.ReflectTest.RefledtTest08.Worker
```
