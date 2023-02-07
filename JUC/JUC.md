---
typora-root-url: JUC.assets
---

# 1、进程与线程 

## 概述

在 Java 中，线程部分是一个重点，本篇文章说的 JUC 也是关于线程的。JUC就是 java.util .concurrent 工具包的简称。这是一个处理线程的工具包，JDK 1.5 开始出现的。

## 进程与线程

### 概述

**进程**：指在系统中正在运行的一个应用程序；程序一旦运行就是进程；进程——资源分配的最小单位。

**线程**：系统分配处理器时间资源的基本单元，或者说进程之内独立执行的一个单元执行流。线程——程序执行的最小单位。

**一个java程序对应一个进程**

**一个进程对应一个jvm实例**

**一个 jvm实例中只有一个运行时数据区**

**一个运行时数据区只有一个方法区和堆**

**一个进程中的多个线程需要共享同一个方法区，堆空间**



**每一个线程在创建时都会创建一个虚拟机栈**

**每一个线程拥有独立的一套程序计数器，本地方法栈，虚拟机栈**

**每一个线程内部保存的一个个的栈帧对应着一次次的Java方法调用**

### 补充

**提问？JAVA真的可以开启线程吗？ 开不了的！**

```java
public synchronized void start() {
    /**
     * This method is not invoked for the main method thread or "system"
     * group threads created/set up by the VM. Any new functionality added
     * to this method in the future may have to also be added to the VM.
     *
     * A zero status value corresponds to state "NEW".
     */
    if (threadStatus != 0)
        throw new IllegalThreadStateException();

    /* Notify the group that this thread is about to be started
     * so that it can be added to the group's list of threads
     * and the group's unstarted count can be decremented. */
    group.add(this);

    boolean started = false;
    try {
        start0();
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
            /* do nothing. If start0 threw a Throwable then
              it will be passed up the call stack */
        }
    }
}
//这是一个C++底层，Java是没有权限操作底层硬件的
private native void start0();
```
Java是没有权限去开启线程、操作硬件的，这是一个native的一个本地方法，它调用的底层的C++代码。

## 并发和并行

### 概述

**并发**：多线程操作同一个资源。

- CPU 只有一核，模拟出来多条线程，天下武功，唯快不破。那么我们就可以使用CPU快速交替，来模拟多线程。

**并发(concurrent)指的是多个程序可以同时运行的现象，更细化的是多进程可以同时运行或者多指令可以同时运行**

**要解决大并发问题，通常是将大任务分解成多个小任务**

**并行**：多项工作一起执行

- CPU多核，多个线程可以同时执行。 我们可以使用线程池！

```java
public class Test1 {
    public static void main(String[] args) {
        //获取cpu的核数
        System.out.println(Runtime.getRuntime().availableProcessors());
    }
}
```

**串行**

- 一个人干一件事

### 总结

并发编程的本质：**充分利用CPU的资源！**

## 线程的状态

```java
public enum State {
/**
* Thread state for a thread which has not yet started.
*/
NEW,(新建)
/**
* Thread state for a runnable thread. A thread in the runnable
* state is executing in the Java virtual machine but it may
* be waiting for other resources from the operating system
* such as processor.
*/
RUNNABLE,（运行）
/**
* Thread state for a thread blocked waiting for a monitor lock.
* A thread in the blocked state is waiting for a monitor lock
* to enter a synchronized block/method or
* reenter a synchronized block/method after calling
* {@link Object#wait() Object.wait}.
*/
BLOCKED,（阻塞）
/**
* Thread state for a waiting thread.
* A thread is in the waiting state due to calling one of the
* following methods:
* <ul>
* <li>{@link Object#wait() Object.wait} with no timeout</li>
* <li>{@link #join() Thread.join} with no timeout</li>
* <li>{@link LockSupport#park() LockSupport.park}</li>
* </ul>
** <p>A thread in the waiting state is waiting for another thread to
* perform a particular action.
*
* For example, a thread that has called <tt>Object.wait()</tt>
* on an object is waiting for another thread to call
* <tt>Object.notify()</tt> or <tt>Object.notifyAll()</tt> on
* that object. A thread that has called <tt>Thread.join()</tt>
* is waiting for a specified thread to terminate.
*/
WAITING,（不见不散等待）
/**
* Thread state for a waiting thread with a specified waiting time.
* A thread is in the timed waiting state due to calling one of
* the following methods with a specified positive waiting time:
* <ul>
* <li>{@link #sleep Thread.sleep}</li>
* <li>{@link Object#wait(long) Object.wait} with timeout</li>
* <li>{@link #join(long) Thread.join} with timeout</li>
* <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
* <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
* </ul>
*/
TIMED_WAITING,（过时不候——超时等待）
/**
* Thread state for a terminated thread.
* The thread has completed execution.
*/
TERMINATED;(终结) }
```

## wait 和sleep的区别

### 基础

**1、来自不同的类**

sleep 是 Thread 的静态方法

wait 是 Object 的方法，任何对象实例都能调用。

一般情况企业中使用休眠是：

```java
TimeUnit.DAYS.sleep(1); //休眠1天
TimeUnit.SECONDS.sleep(1); //休眠1s
```

**2、关于锁的释放**

sleep 不会释放锁，它也不需要占用锁。

wait 会释放锁，但调用它的前提是当前线程占有锁(即代码要在 synchronized 中)。 

**3、使用的范围是不同的**

sleep 可以在任何地方睡；

wait 必须在同步代码块中；

**4、是否需要捕获异常**

sleep必须要捕获异常；

wait是不需要捕获异常；

**5、它们都可以被 interrupted 方法中断**

### wait和sleep的区别与联系？

**区别：**

1. 由于sleep针对的是线程，所以他在thread类中，wait的源码中用到了sync锁，所以是和对象有关的，因此他在object类中
2. wait和锁相关，所以他需要释放锁；sleep是线程不涉及锁，所以不需要释放锁
3. sleep必须捕获异常，wait（）、notify、notifyAll不需要捕获异常

**联系：**

1. 两者都会造成线程阻塞（因为对象依附于线程，wait是对线程加锁，所以会阻塞；sleep原本就作用于线程，因此也会造成阻塞）
2. 两者均可被中断唤醒（）

**内容：**

1. sleep方法导致了程序暂停执行指定的时间，让出CPU给其他线程，但是他的监控状态依旧保持着，时间一到就会恢复运行
2. 调用sleep方法的时候，对象不会释放锁；而wait方法会放弃该对象的锁，将线程放入锁池，等待唤醒

## 管程

管程就是操作系统中进程同步的一种方式

管程(monitor)是保证了同一时刻只有一个进程在管程内活动，即管程内定义的操作在同一时刻只被一个进程调用(由编译器实现)。但是这样并不能保证进程以设计的顺序执行。

JVM 中同步是基于进入和退出管程(monitor)对象实现的，每个对象都会有一个管程(monitor)对象，管程(monitor)会随着java对象一同创建和销毁。

执行线程首先要持有管程对象，然后才能执行方法，当方法完成之后会释放管程，方法在执行时候会持有管程，其他线程无法再获取同一个管程。

**管程特点：**管程封装了同步操作，对进程隐蔽了同步细节，简化了同步功能的调用界面。用户编写并发程序如同编写顺序(串行)程序。

**引入管程机制的目的：**

1、把分散在各进程中的临界区集中起来进行管理；

2、防止进程有意或无意的违法同步操作；

3、便于用高级语言来书写程序，也便于程序正确性验证。

组成部分

1）局部于管程的共享变量；

2）对数据结构进行操作的一组过程；

3）对局部于管程的数据进行初始化的语句。

### 管程的属性

共享性：管程可被系统范围内的进程互斥访问，属于共享资源

安全性：管程的局部变量只能由管程的过程访问，不允许进程或其它管程直接访问，管程也不能访问非局部于它的变量。

互斥性：多个进程对管程的访问是互斥的。任一时刻，管程中只能有一个活跃进程。

封装性：管程内的数据结构是私有的，只能在管程内使用，管程内的过程也只能使用管程内的数据结构。进程通过调用管程的过程使用临界资源。管程在Java中已实现。

##  **用户线程和守护线程** 

**用户线程**：平时用到的普通线程， 自定义线程

**守护线程**：运行在后台，是一种特殊的线程，比如垃圾回收

**当主线程结束后，用户线程还在运行JVM存活**

**如果没有用户线程，都是守护线程，JVM结束**

# 2、Lock 接口

## Synchronized关键字

### 概述

synchronized 是 Java 中的关键字，是一种同步锁。它修饰的对象有以下几种：

- **修饰一个代码块**，被修饰的代码块称为同步语句块，其作用的范围是大括号{}括起来的代码，作用的对象是调用这个代码块的对象；

- **修饰一个方法**，被修饰的方法称为同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象；
- **修改一个静态的方法**，其作用的范围是整个静态方法，作用的对象是这个类的所有对象；
- **修改一个类**，其作用的范围是 synchronized 后面括号括起来的部分，作用主的对象是这个类的所有对象。

### 实例

```java 
/**
 * 真正的多线程开发
 * 线程就是一个单独的资源类，没有任何的附属操作！
 */
public class SaleTicketDemo01 {
    public static void main(String[] args) {
        //多线程操作
        //并发：多线程操作同一个资源类，把资源类丢入线程
        Ticket ticket = new Ticket();

        //@FunctionalInterface 函数式接口 jdk1.8之后 lambda表达式
        new Thread(()->{
            for(int i=0;i<40;i++){
                ticket.sale();
            }
        },"A").start();
        new Thread(()->{
            for(int i=0;i<40;i++){
                ticket.sale();
            }
        },"B").start();
        new Thread(()->{
            for(int i=0;i<40;i++){
                ticket.sale();
            }
        },"C").start();
    }
}
//资源类
//属性+方法
//oop
class Ticket{
    private int number=50;


    //卖票的方式
    // synchronized 本质：队列，锁
    public synchronized void sale(){
        if(number>0){
            System.out.println(Thread.currentThread().getName()+" 卖出了第"+number+" 张票,剩余："+number+" 张票");
            number--;
        }
    }
}

```

## Lock

### 概述

Lock 锁实现提供了比使用同步方法和语句可以获得的更广泛的锁操作。它们允许更灵活的结构，可能具有非常不同的属性，并且可能支持多个关联的条件对象。Lock 提供了比 synchronized 更多的功能。

![20200727195732117](/20200727195732117.png)

![2020080112041218](/2020080112041218.png)

### 实例

```java
public class SaleTicketDemo02 {
    public static void main(String[] args) {
        //多线程操作
        //并发：多线程操作同一个资源类，把资源类丢入线程
        Ticket2 ticket = new Ticket2();
        new Thread(()->{for(int i=0;i<40;i++) ticket.sale(); },"A").start();
        new Thread(()->{for(int i=0;i<40;i++) ticket.sale(); },"B").start();
        new Thread(()->{for(int i=0;i<40;i++) ticket.sale(); },"C").start();
    }
}

//lock三部曲
//1、    Lock lock=new ReentrantLock();
//2、    lock.lock() 加锁
//3、    finally=> 解锁：lock.unlock();
class Ticket2{
    private int number=50;

    Lock lock=new ReentrantLock();

    //卖票的方式
    // 使用Lock 锁
    public void sale(){
        //加锁
        lock.lock();
        try {
            //业务代码
            if(number>=0){
                System.out.println(Thread.currentThread().getName()+" 卖出了第"+number+" 张票,剩余："+number+" 张票");
                number--;
            }
        }catch (Exception e) {
            e.printStackTrace();
        }
        finally {
            //解锁
            lock.unlock();
        }
    }
}
```

## Synchronized 和 Lock区别

1、Synchronized 内置的Java关键字，Lock是一个Java类

2、Synchronized 无法判断获取锁的状态，Lock可以判断

3、Synchronized 会自动释放锁，lock必须要手动加锁和手动释放锁！可能会遇到死锁

4、Synchronized 线程1(获得锁->阻塞)、线程2(等待)；

lock就不一定会一直等待下去，lock会有一个trylock去尝试获取锁，不会造成长久的等待。

5、Synchronized 是可重入锁，不可以中断的，非公平的；Lock，可重入的，可以判断锁，可以自己设置公平锁和非公平锁；

6、Synchronized 适合锁少量的代码同步问题，Lock适合锁大量的同步代码；

**在性能上来说，如果竞争资源不激烈，两者的性能是差不多的，二当竞争资源非常激烈时（即有大量线程同时竞争），此时Lock的性能要远远由于Synchronized。**

### 补充

在jdk1.5以前sync对锁的处理十分的简单粗暴，为了优化性能在jdk1.5时，引入了lock锁。而且sync还有一个诟病就是无法同时使用wait和notify（原因是因为）

**sync的加锁过程**

在1.6之后，jdk对sync做了优化，引入了偏向锁、自旋锁等。加锁的过程也相对之前较为完善。为了让线程降低获取锁的成本，于是引入了偏向锁，偏向锁的锁头会从对象头的markwork区域获取并记录当前线程的id，他会将当前markwork信息复制到栈上。当别的线程来操作时，会首先对线程id进行判定是否当前线程在操作，在1.5之前如果获取失败会直接阻塞，在优化过后提出了自旋锁的概念，在获取失败后会通过cas自旋的方式再一次尝试获取到锁，在自旋超过10次以后该轻量级锁就会升级为重量级锁。

重量级锁相对于轻量级锁的区别就在于，重量级锁需要唤醒、阻塞、上下文切换等，主要针对于多个线程在不同时间请求同一把锁的情况。当升级为重量级锁时，会将该线程存放到等待队列中，等到被唤醒再一次去争抢锁

锁升级的原因：如果让当前线程阻塞，对性能的校会很大，如果使用自旋的话，当数据量小时自旋相当于就是让这些等待的线程去尝试获取锁。当线程很多在竞争时，就膨胀为重量级锁。因为不停地自旋也会影响性能。

**上下文切换**

线程运行时，会有自己的运行条件和状态，即上下文

- 主动让出 CPU，比如调用了 sleep(), wait() 等。
- 时间片用完，因为操作系统要防止一个线程或者进程长时间占用 CPU 导致其他线程或者进程饿死。
- 调用了阻塞类型的系统中断，比如请求 IO，线程被阻塞。
- 被终止或结束运行

发生线程切换时，线程切换意味着需要保存当前线程的上下文，留待线程下次占用 CPU 的时候恢复现场。并加载下一个将要占用 CPU 的线程上下文。这就是所谓的 **上下文切换**。

**lock的加锁过程**

reentrentLock是cas基于aqs实现的，为了实现公平锁和非公平锁

无论是公平锁还是非公平锁，在线程启用lock锁时，所有的线程都会尝试通过cas的方式修改state参数，最终那个线程成功修改了state参数哪个线程就抢到了锁。

- 公平锁

  1. 如果设置成功，设置当前线程为独占锁的线程；

  2. 如果设置失败，还会再获取一次锁数量
     1. 如果锁数量为0，再基于CAS尝试1一次将state（锁数量）从0设置为，如果设置成功，设置当前线程为独占锁的线程；
     2. 如果锁数量不为0或者上边的尝试又失败了，查看当前线程是不是已经是独占锁的线程了，如果是，则将当前的锁数量+1；如果不是，则将该线程封装在一个Node内，并加入到等待队列中去。等待被其前一个线程节点唤醒。

- 非公平锁

  1. 获取一次锁数量，

  2. 如果锁数量为0，如果当前线程是等待队列中的头节点，基于CAS尝试一次将state（锁数量）从0设置为1，如果设置成功，设置当前线程为独占锁的线程；

  3. 如果锁数量不为0或者当前线程不是等待队列中的头节点或者上边的尝试又失败了，查看当前线程是不是已经是独占锁的线程了，如果是，则将当前的锁数量+1；如果不是，则将该线程封装在一个Node内，并加入到等待队列中去。等待被其前一个线程节点唤醒。

     当A线程获取到锁之后进行执行中，将state+1，此时其他线程如果想要执行需要睡眠进行等待，等待A执行完毕后，释放锁后，获取到锁之后才可以执行。

reentrenLock实现公平锁和非公平锁的不同就是在加入队列后，当前节点是否有前驱节点的判断，如果不是头结点的下一个节点，就乖乖排队，去其他线程中查找符合条件的。**非公平锁直接cas，公平锁会就进行判断**

公平锁保证了锁的获取按照FIFO原则，而代价是进行大量的线程切换。非公平锁虽然可能造成线程“饥饿”，但极少的线程切换，保证了其更大的吞吐量。

## ReentrantReadWriteLock

### 基础

基于AQS实现的。

AQS就一个state属性，怎么基于一个state实现的互斥锁和共享锁。

但是它有读锁和写锁俩种状态，而AQS只有一个状态变量。所以高16位作为共享锁的实现   低16位作为互斥锁的实现

读写锁是可重入的，写锁因为是互斥，直接追加到低位即可。

但是读锁是共享的，读锁的重入是如何实现的？

因为读锁都会操作state的高16位，为了记录当前线程重入的次数，需要配合ThreadLocal实现。（自己线程的锁信息会保留在线程的ThreadLocal中。）

**因为读锁是共享的，所以多个读线程进来可以直接获取到读锁，如果在读线程中，有一个写线程在排队，是不是会造成写锁的饥饿呢？**

如果有写线程在AQS的队列中排队，那么读线程就不能直接获取读锁，要在写线程后面排队，避免写锁饥饿。

### 读写锁如何实现读锁的重入？

因为读锁都会操作state的高16位，为了记录当前线程重入的次数，需要配合ThreadLocal实现。（自己线程的锁信息会保留在线程的ThreadLocal中。）

### 对读锁的重入做了哪些优化？

每个线程如果要记录读锁的重入，需要配合threadlocal，threadlocal会初始化一个16位的entry数组

但是ThreadLocal有个特点，上来就初始化16长度Entry数组，每次记录读锁需要拿到ThreadLocal中的值，进行+ +或者--。同时还需要解决ThreadLocal中value的内存泄漏问题。

读锁针对第一个成功获取读锁的线程，不需要去基于ThreadLocal记录，直接使用读写锁内部提供的属性去记录读锁线程和读多重入次数。

```
private transient Thread firstReader = nu11;
private transient int firstReaderHoldCount ;
```

读锁还针对最后一个获取读锁的线程， 单路的将HoldCounter从ThreadLocal中取出， 放到属性中，以后就不需要反复的从ThreadLocal获取

### 锁降级

锁降级指的是写锁降级成为读锁。如果当前线程拥有写锁，然后将其释放，最后再获取读锁，这种分段完成的过程不能称之为锁降级。锁降级是指把持住（当前拥有的）写锁，再获取到读锁，随后释放（先前拥有的）写锁的过程。

**锁降级的必要性**

主要是为了保证数据的可见性，如果当前线程不获取读锁而是直接释放写锁， 假设此刻另一个线程（记作线程T）获取了写锁并修改了数据，那么当前线程无法感知线程T的数据更新。如果当前线程获取读锁，即遵循锁降级的步骤，则线程T将会被阻塞，直到当前线程使用数据并释放读锁之后，线程T才能获取写锁进行数据更新。

**实现**

使用ReentrantReadWriteLock实现锁降级

```java
package com.xz.thread.t12;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * @description: ReentrantReadWriteLock降级锁示例
 * @author: xz
 * @create: 2021-05-17 22:09
 */
public class Demo {
    /**
     *    1、ReentrantReadWriteLock实例实现了ReadWriteLock接口
     *    2、声明一个ReentrantReadWriteLock实例，指向父类ReadWriteLock的引用
     *       因为ReentrantReadWriteLock可以保证公平或者非公平
     *    3、ReadWriteLock类中只有readLock()和writeLock() 两个方法
     */
    private ReadWriteLock rwl =new ReentrantReadWriteLock();

    //读锁
    private Lock r =rwl.readLock();
    //写锁
    private Lock w =rwl.writeLock();

    //声明一个map,存储数据
    private Map<String,Object> map =new HashMap<>();
    //声明一个boolean类型的状态，用于判断是读操作还是写操作;volatile关键字保证可见性
    private volatile boolean isUpdateStatus;

    //定义一个读写方法
    public void readWriteMethod(){
        r.lock();//为了保证isUpdateStatus能够获取到最新的值，添加读锁
        if(isUpdateStatus){//如果isUpdateStatus是最新的值
            r.unlock();//因为要进行写操作，所以需要释放读锁
            w.lock();//在写操作之前，为了保证写的状态，需要加写锁
            map.put("key1","v1");//写操作
            //开始锁降级;
            //再写操作没有释放的时候，获取到读锁，再释放写锁
            r.lock();
            w.unlock();//写操作完成后释放写锁
            //锁降级完成;
        }
        Object obj = map.get("key1");//写操作
        r.unlock();//释放读锁
        System.out.println(Thread.currentThread().getName()+"读操作获取到的值："+obj);

    }

}
```

**总结**

首先你没理解读写锁的意义，读锁的存在意味着不允许其他写操作的存在。

按照你提供的例子，可能存在一个事务线程不希望自己的操作被别的线程中断，而这个事务操作可能分成多部分操作更新不同的数据（或表）甚至非常耗时。如果长时间用写锁独占，显然对于某些高响应的应用是不允许的，所以在完成部分写操作后，退而使用读锁降级，来允许响应其他进程的读操作。只有当全部事务完成后才真正释放锁。
按你的理解如果当中写锁被其他线程占用，那么这个事务线程将不得不中断等待别的写锁释放。

所以总结下锁降级的意义应该就是：在一边读一边写的情况下提高性能。

# 3、线程间通信

## 生产者和消费者问题

### Synchronized版本

```java 
public class A {
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(()->{for(int i=0;i<10;i++) {
            try {
                data.increment();//加一
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        },"A").start();
        new Thread(()->{for(int i=0;i<10;i++) {
            try {
                data.decrement();//减一
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }},"B").start();
    }
}
class Data{
    //数字  资源类
    private int number = 0;

    //+1
    public synchronized void increment() throws InterruptedException {
        //判断
        if(number!=0){
            //等待操作
            this.wait();
        }
        //干活
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        //通知
        //通知其他线程 我+1完毕了
        this.notifyAll();
    }

    //-1
    public synchronized void decrement() throws InterruptedException {
        if(number==0){
            //等待操作
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        //通知其他线程  我-1完毕了
        this.notifyAll();
    }

}
```

**问题存在，A线程B线程，现在如果我有四个线程A B C D！就会出现以下情况，即虚假唤醒：**

![image-20211206102831898](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211206102831898.png)

### 虚假唤醒

![image-20211206111051781](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211206111051781.png)

产生的主要原因是因为，wait方法在哪里睡在哪里醒，会跳过条件判断

![image-20211206102920561](/image-20211206102920561.png)

**解决方法**

把Data类中的if改为while即可，防止虚拟唤醒

```java
class Data{
    //数字  资源类
    private int number = 0;

    //+1
    public synchronized void increment() throws InterruptedException {
        while(number!=0){
            //等待操作
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        //通知其他线程 我+1完毕了
        this.notifyAll();
    }

    //-1
    public synchronized void decrement() throws InterruptedException {
        while(number==0){
            //等待操作
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        //通知其他线程  我-1完毕了
        this.notifyAll();
    }

}
```

### JUC版本

**await、signal 替换 wait、notify**

![image-20211206103135153](/image-20211206103135153.png)

通过Lock找到Condition

![image-20211206103336987](/image-20211206103336987.png)

```java
public class B {
    public static void main(String[] args) {
        Data2 data = new Data2();

        new Thread(()->{for(int i=0;i<10;i++) {
            data.increment();
        }
        },"A").start();
        new Thread(()->{for(int i=0;i<10;i++) {
            data.decrement();
        }},"B").start();
        new Thread(()->{for(int i=0;i<10;i++) {
            data.increment();
        }
        },"C").start();
        new Thread(()->{for(int i=0;i<10;i++) {
            data.decrement();
        }
        },"D").start();
    }
}
class Data2{
    //数字  资源类
    private int number = 0;

    //lock锁
    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();

    //+1
    public void increment()  {
        lock.lock();
        try{
            //业务
            while (number!=0){
                //等待操作
                condition.await();
            }
            number++;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
            //通知其他线程 我+1完毕了
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    //-1
    public void decrement()  {
        lock.lock();
        try{
            //业务
            while (number==0){
                //等待操作
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
            //通知其他线程 我+1完毕了
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

## Condition

**Condition的优势**：精准的通知和唤醒的线程！

**如果我们要指定通知的下一个进行顺序怎么办呢？ 我们可以使用Condition来指定通知进程~**

### 实例1 

```java
/**
 * A 执行完 调用B
 * B 执行完 调用C
 * C 执行完 调用A
 */

public class C {

    public static void main(String[] args) {
        Data3 data3 = new Data3();
        new Thread(()->{
            for(int i=0;i<10;i++){
                data3.printA();
            }
        },"A").start();
        new Thread(()->{
            for(int i=0;i<10;i++){
                data3.printB();
            }
        },"B").start();
        new Thread(()->{
            for(int i=0;i<10;i++){
                data3.printC();
            }
        },"C").start();
    }
}

class Data3{
    //资源类
    private Lock lock=new ReentrantLock();
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
    private int number = 1; //1A 2B 3C

    public void printA(){
        lock.lock();
        try {
            //业务 判断 -> 执行 -> 通知
            while(number!=1){
                //等待
                condition1.await();
            }
            //操作
            System.out.println(Thread.currentThread().getName()+",AAAAA");
            //唤醒指定的线程
            number=2;
            condition2.signal(); // 唤醒2

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    public void printB(){
        lock.lock();
        try {
            //业务 判断 -> 执行 -> 通知
            while (number!=2){
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName()+",BBBBB");
            //唤醒3
            number=3;
            condition3.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    public void printC(){
        lock.lock();
        try {
            //业务 判断 -> 执行 -> 通知
            while(number!=3){
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName()+",CCCCC");
            //唤醒1
            number=1;
            condition1.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

### 实例2

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class DemoClass {
    //通信对象:0--打印 A 1---打印 B 2----打印 C
    private int number = 0;
    //声明锁
    private Lock lock = new ReentrantLock();
    //声明钥匙 A
    private Condition conditionA = lock.newCondition();
    //声明钥匙 B
    private Condition conditionB = lock.newCondition();
    //声明钥匙 C
    private Condition conditionC = lock.newCondition();

    /**
     * A 打印 5 次
     */
    public void printA(int j) {
        try {
            lock.lock();
            while (number != 0) {
                conditionA.await();
            }
            System.out.println(Thread.currentThread().getName() + "输出 A,第" + j + " 轮开始");
            //输出 5 次 A
            for (int i = 0; i < 5; i++) {
                System.out.println("A");
            }
            //开始打印 B
            number = 1;
            //唤醒 B
            conditionB.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    /**
     * B 打印 10 次
     */
    public void printB(int j) {
        try {
            lock.lock();
            while (number != 1) {
                conditionB.await();
            }
            System.out.println(Thread.currentThread().getName() + "输出 B,第" + j + " 轮开始");
            //输出 10 次 B
            for (int i = 0; i < 10; i++) {
                System.out.println("B");
            }
            //开始打印 C
            number = 2;
            //唤醒 C
            conditionC.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    /**
     * C 打印 15 次
     */
    public void printC(int j) {
        try {
            lock.lock();
            while (number != 2) {
                conditionC.await();
            }
            System.out.println(Thread.currentThread().getName() + "输出 C,第" + j + " 轮开始");
            //输出 15 次 C
            for (int i = 0; i < 15; i++) {
                System.out.println("C");
            }
            System.out.println("-----------------------------------------");
            //开始打印 A
            number = 0;
            //唤醒 A
            conditionA.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

/**
 * volatile 关键字实现线程交替加减
 */
class TestVolatile {
    /**
     * 交替加减
     *
     * @param args
     */
    public static void main(String[] args) {
        DemoClass demoClass = new DemoClass();
        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                demoClass.printA(i);
            }
        }, "A 线程").start();
        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                demoClass.printB(i);
            }
        }, "B 线程").start();
        new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                demoClass.printC(i);
            }
        }, "C 线程").start();
    }
}
```

## 总结

Synchronized实现同步的基础：java中的每一个对象都可以作为锁，具体表现为以下三种形式。

对于普通同步方法，锁是当前实例对象。

对于静态同步方法，锁是当前类的Class对象

对于同步方法块，锁是Synchronized括号里的配置的对象

### 多线程之间是如何通信的？

为了实现可见性

操作系统：多进程：信号量、MQ、套接字、管道(有名管道、无名管道)、共享内存

由于多线程之间共享了进程的地址空间，所以原生就支持数据共享，当一个线程修改了进程变量，由于共享空间，另外一个线程自然能看到，所以原生支持通讯。由于线程之间并发必然会引起互斥操作，这是就需要同步机制：volitile、sync、lock...

1. 使用volitile关键字
   1. 多个线程同时监听一个变量，当这个变量发生变化的时候 ，线程能够感知并执行相应的业务
2. 使用object类的wait和notify方法
   1. 锁池、等待池
3. JUC的CountDownLatch
   1. `CountDownLatch` 基于 AQS 框架，相当于也是维护了一个线程间共享变量 state。
4. reentrantLock+Condition
   1. 类似于第二种object的
5. LockSupport
   1. 是一种非常灵活的实现线程间阻塞和唤醒的工具，使用它不用关注是等待线程先进行还是唤醒线程先运行，但是得知道线程的名字。

# 4、集合的线程安全

## List集合不安全

### 问题

我们来看一下List这个集合类：

```java 
//java.util.ConcurrentModificationException 并发修改异常！
public class ListTest {
    public static void main(String[] args) {

        List<Object> arrayList = new ArrayList<>();

        for(int i=1;i<=10;i++){
            new Thread(()->{
                arrayList.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(arrayList);
            },String.valueOf(i)).start();
        }

    }
}
```

会造成：

![image-20211206122120384](/image-20211206122120384.png)

**ArrayList 在并发情况下是不安全的！**

### 原因

查看 ArrayList 的 add 方法源码

```java
/**
* Appends the specified element to the end of this list.
*
* @param e element to be appended to this list
* @return <tt>true</tt> (as specified by {@link Collection#add})
*/
public boolean add(E e) {
ensureCapacityInternal(size + 1); // Increments modCount!!
elementData[size++] = e;
return true; 
}
```

没有Synchronized关键字，该方法线程不安全。

### 解决方法

#### Vector

**切换成Vector就是线程安全的啦！**

```java 
public class ListTest02 {
    public static void main(String[] args) {

        List<Object> arrayList = new Vector<>();

        for(int i=1;i<=100;i++){
            new Thread(()->{
                arrayList.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(arrayList);
            },String.valueOf(i)).start();
        }

    }
}
```

Vector 是**矢量队列**，它是 JDK1.0 版本添加的类。继承于 AbstractList，实现了 List, RandomAccess, Cloneable 这些接口。 Vector 继AbstractList，实现了 List；所以，**它是一个队列，支持相关的添加、删除、修改、遍历等功能**。 Vector 实现了 RandmoAccess 接口，即**提供了随机访问功能**。RandmoAccess 是 java 中用来被 List 实现，为 List 提供快速访问功能的。在Vector 中，我们即可以通过元素的序号快速获取元素对象；这就是快速随机访问。 Vector 实现了 Cloneable 接口，即实现 clone()函数。它能被克隆。

#### Collections

**2、使用Collections.synchronizedList(new ArrayList<>());**

```java
public class ListTest03 {
    public static void main(String[] args) {
        List<Object> arrayList = Collections.synchronizedList(new ArrayList<>());

        for (int i = 1; i <= 100; i++) {
            new Thread(() -> {
                arrayList.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(arrayList);
            }, String.valueOf(i)).start();
        }

    }
}
```

#### JUC

**3、使用JUC中的包：List arrayList = new CopyOnWriteArrayList<>();**

```java
public class ListTest04 {
    public static void main(String[] args) {

        List<Object> arrayList = new CopyOnWriteArrayList<>();

        for (int i = 1; i <= 10; i++) {
            new Thread(() -> {
                arrayList.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(arrayList);
            }, String.valueOf(i)).start();
        }

    }
}
```

#### CopyOnWriterArrayList和Vector对比

CopyOnWriterArrayList：

![image-20211206124330948](/image-20211206124330948.png)

Vetocr：

![image-20211206124413509](/image-20211206124413509.png)

Synchronized修饰的方法效率都很低，而CopyOnWriterArrayList中的方法都没有用到Synchronized修饰。

## Set集合不安全

### 概述

![image-20211206130152471](/image-20211206130152471.png)

和List、Set同级的还有一个BlockingQueue 阻塞队列；

**Set和List同理可得:** 多线程情况下，普通的Set集合是线程不安全的；

### 解决方法

- 使用Collections工具类的**synchronized**包装的Set类
- 使用CopyOnWriteArraySet写入复制的**JUC**解决方案

```java 
//同理：java.util.ConcurrentModificationException
// 解决方案：
public class SetTest {
    public static void main(String[] args) {
//        Set<String> hashSet = Collections.synchronizedSet(new HashSet<>()); //解决方案1
        Set<String> hashSet = new CopyOnWriteArraySet<>();//解决方案2
        for (int i = 1; i < 100; i++) {
            new Thread(()->{
                hashSet.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(hashSet);
            },String.valueOf(i)).start();
        }
    }
}
```

### HashSet底层是什么？

```java
public HashSet() {
        map = new HashMap<>();
}

//add 本质其实就是一个map的key，map的key是无法重复的，所以使用的就是map存储
//hashSet就是使用了hashmap key不能重复的原理
public boolean add(E e) {
        return map.put(e, PRESENT)==null;
}
//PRESENT是什么？ 是一个常量  不会改变的常量  无用的占位
private static final Object PRESENT = new Object();
```

hashSet底层就是一个**HashMap**；

## Map集合不安全

### 概述

回顾map的基本操作：

> //map 是这样用的吗？  不是，工作中不使用这个
> //默认等价什么？ new HashMap<>(16,0.75);
> Map<String, String> map = new HashMap<>();
> //加载因子、初始化容量

默认**加载因子是0.75**，默认的**初始容量是16**

![image-20211206131108713](/image-20211206131108713.png)

### 问题

同样的HashMap基础类也存在**并发修改异常**！

```java
public static void main(String[] args) {
        //map 是这样用的吗？  不是，工作中不使用这个
        //默认等价什么？ new HashMap<>(16,0.75);
        Map<String, String> map = new HashMap<>();
        //加载因子、初始化容量
        for (int i = 1; i < 100; i++) {
            new Thread(()->{
                map.put(Thread.currentThread().getName(),UUID.randomUUID().toString().substring(0,5));
                System.out.println(map);
            },String.valueOf(i)).start();
        }
}
```

### 解决方法

- **使用Collections.synchronizedMap(new HashMap<>());处理**；
- **使用ConcurrentHashMap进行并发处理**

### ConcurrentHashMap

#### 数据结构

1.8 数组+链表+红黑树

1.7数组+链表

**为什么 红黑树？**

链表的话，查询的时间复杂度为On，链表过长，查询速度慢；

当链表长度到达8的时候就会从链表编程红黑树，红黑树的复杂度是Ologn；

**链表长度为8时，一定要转换成红黑树吗？**

必须达到数组长度》=64，并且某一个桶下的链表长度到8，才回转化成红黑树，数组查询效率快

**为什么在8的时候才会转成红黑树？**

泊松分布

**什么时候转换回来？**

6个 

#### 计数器

计数器是用来记录原子个数的，put了一个，计数器+1，remove了一个，计数器-1

CHM要保证是线程安全的

为了保证+1、-1是原子性的，如果采用了Atmoic，在大并发的情况下，会不会造成CAS自选次数过多呢？

CHM采用的是LongAdder作为计数器的实现

并没有直接引用LongAdder，而是仿照LongAdder源码又实现了一份

**实现：**

在CHM底层有一个addCount方法，而CHM的计数就是对baseCount参数的调整。为了避免并发量特别大时，无法保证线程安全，CHM创建了一个CountCell数组，数组中也有一个value元素。

当并发不大的时候，直接对baseCount元素做操作；若并发量很大/cas操作失败时，就构建CountCell数组，对其中的value进行操作，最终的计数=baseCount+多个CountCell数组

![image-20230205141149211](/image-20230205141149211.png)

分段存储元素个数

#### 散列算法

chm存储数据的位置有三种：

+ 数组 O1（放在数组上的查询效率是最快的）
+ 链表 On
+ 红黑树 OlogN

chm本身就是为了让查询效率更快，因此当数据尽可能的放在数组上时，查询效率就是最快的。

chm是key基于hashCode值，决定存储在哪个位置的

假设k1的hash：01010101 01010101 01010101 01010101

假设k2的hash：11111111 11111111 11111111 01010101

长度 - 1：   		 00000000 00000000 00000000 00001111

计算的方式：将hash值，与数组长度-1做&运行

如果单纯按照上述方式计算，会导致只要hash值的低位一样造成数据放在一个索引位置。因为低位的运算不影响高位（前16位获得的数据是不一样的，后16位运算完之后是一样的）

**chm的处理方式**

高低位一起运算

```
(hash ^ ( hash >>> 16)) & (tableSize -1)
```

![image-20220416135354332](/image-20220416135354332.png)

对hashCode进行一个位运算。

![image-20220416140237442](/image-20220416140237442.png)

#### 会先查双向链表，再查红黑树

![image-20230205223832626](/image-20230205223832626.png)

**线程A在写入红黑树ing，线程B要查询红黑树中的数据？读线程阻塞吗？**

不阻塞

在链表转红黑树时，不单会将单向链表保留为红黑树，还会保留一个双向链表在红黑树中。如果有线程在写入红黑树时，其他直接去查询双向链表

chm还保留了双向链表，可以直接去查双向链表

![image-20230205222718966](/image-20230205222718966.png)

![image-20230205222958273](/image-20230205222958273.png)

整个过程是：先拼接双向链表，再转红黑树

**如果有读线程读取红黑树数据，此时写线程要写入红黑树数据，写线程阻塞吗？**

阻塞

#### 扩容

##### sizeCtl

用来控制初始化和扩容的参数

**线程扩容时，会使用sizeCtl记录现在扩容时的线程数量，那么为什么1个线程扩容时，低位数值为2, 2个线程扩容为3?**

**sizeCtl = -1:**代表当前ConcurrentHashMap的数组正在初始化

**sizeCtl < -1:**代表当前ConcurrentHashMap正在扩容,值为-2,代表有1个线程在扩容

**sizeCtl = 0:**代表当前还没初始化呢

**sizeCtl > 0**:如果还没初始化，代表初始数组长度。如果已经初始化了， 就代表扩容阈值

ConcurrentHashMap在第一次put操作时，才会初始化数组，默认16。

![image-20230205143225959](/image-20230205143225959.png)

![image-20230205143657066](/image-20230205143657066.png)

sizeCtl= -2时，代表有1个线程在扩容。-1 已经代表初始化状态了，而且在扩容时，-2也有妙用!(-2有一个在扩容，-3有两个人在扩容)

##### lastRun机制

ConcurrentHashMap当中在扩容操作时，涉及到oldTable中是一个链表。

需要将oldTable中链表的数据迁移到newTable中。

![image-20230205161226970](/image-20230205161226970.png)

右侧的情况无法使用lastRun机制

左侧的情况就可以使用lastRun机制

在扩容时，当旧的数组要向新的数组迁移时，会对链表从后往前计算，若发现可以放到新数组的同一位置上，此时尾部位置的头结点，就直接放到指定的索引位置。

##### ConcurrenHashMap如何保证数组初始化线程安全?

基于DCL做的

锁的实现是基于CAS的方式实现的

CHM在初始化数组时，sizeCtl=-1

要初始化数组的线程需要基于CAS成功的将sizeCtl改为-1

外层用while先判断一次是否初始化，基于cas加锁，内层再用if进行判断是否初始化

![image-20230206001400813](/image-20230206001400813.png)

##### ConcurrenHashMap在并发扩容时，如何保证安全?

在计算Node中key的hash值时，会特意的将hash值正常情况的数值定义为正数

负数有特殊的含义，如果hash值为-1,代表当前节点正在扩容
ConcurrenthashMap会在扩容时，每次将老数组中的数据table.size- 1 ~ table.size - 16索引的位置移动，然后再迁移其他索引位置的数据，如果有线程在插入数据时，发现正在扩容，找还没有被迁移数据的索引位置，帮助最开始扩容的线程进行扩容，

最开始扩容A: 31~16 

线程B插入数据，发现正在扩容，帮你迁移数据，15~0索引位置

每一个迁移完毕的数据，都会加上标识，代表扩容完毕，放上一-个ForwardingNode节点，代表扩容完毕，而且再扩容时，不会应用ConcurrentHashMap的遍历， 查询和添加(发现扩容，会帮忙~)

##### 扩容流程

**扩容条件**

addCount方法中，如果元素个数到达扩容的阈值

链表达到8，但是数组长度没有到达64（链表转红黑树前）

在执行putAll操作时，如果传入的map.size大于当前map，需要扩容

transfer方法

**扩容流程**

- 计算扩容戳（int）
  - 因为CHM，sizeCtl=-1时，才能表示在扩容（扩容戳左移16位，作为sizeCtl的高位）
  - **扩容戳二进制标识的第16位是1**
  - CHM允许协助扩容，从同样的oldCap到newCap
  - **低位是由oldCap计算出来的**
  - 扩容戳是一个负数，高16位标识当前old数组的长度，用来保证多线程扩容是从同样的长度开始扩容，到2倍长度。 低16位，用来标识当前(正在扩容的线程个数- 1)

- 第一个协助扩容线程，修改sizeCtl
  - 需要初始化型数组
  - 领取迁移数据的任务，去迁移数据
  - oldTable中迁移完的桶，会放到一个ForwardingNode中

- 第N个协助扩容线程，sizeCtil+1
  - 协助扩容前需要做判断
    - oldCap一致？
    - 扩容结束了？
    - 协助扩容的线程到最大值？
    - 新数组建立出来了？
    - 迁移的任务被领取完了？
  - 领取迁移数据的任务，去迁移数据
  - oldTable中迁移完的桶，会放到一个ForwardingNode中
- 最后一个完成迁移数据的线程，需要从头到尾再检查一次（单纯的检查，一般没问题）

**扩容方式**

多线程同时搬运数据

![image-20220416150950660](/image-20220416150950660.png)

**扩容戳**

要在扩容时保证多个线程扩容的长度都是一样的

![image-20220416153238963](/image-20220416153238963.png)

**扩容步骤**

ConcurrentHashMap在扩容时，会先指定每个线程每次扩容的长度，最小值为16 (根据数组长度和CPU内核去指定每次扩容长度)。

开始扩容，而开始扩容的线程只有一个，第一个扩容的线程需要把新数组new出来。

有了新数组之后，其他线程再执行transfer方法(可能从helpTransfer方法进来)，其他线程进来后，对扩容戳进行+ 1操作，也就是如果1个线程低位是-2，那么2个线程低位为-3

每次迁移时，会从后往前迁移数据，也就是说两个线程并发扩容:

线程A负责索引位置: 16~31

线程B负责索引位置: 15~0

是一个桶一个桶的去迁移数据，每次迁移完一个桶之后， 会将，会将ForwardingNode设置到老数组中

#### bug

1. 协助扩容的条件判断

![image-20230205145351243](/image-20230205145351243.png)

对正常操作，没有影响

rs是32位的，应该先左移16位，再+1

在jdk11修复了

2. 死锁问题

**computeIfAbsent方法**

编程中经常遇到这种数据结构，判断一个map中是否存在这个key，如果存在则处理value的数据，如果不存在，则创建一个满足value要求的数据结构放到value中。

**使用：**

hashMap.computeIfAbsent("china", key -> getValues(key)).add("liSi");

意思表示key为“China”的建值对是否存在，返回的是value的值。

如果存在则获取china的值，并操作值的set添加数据“lisi"。

如果不存在，则调用方法，新创建set结构，将"lisi"添加到set中，再存入到hashMap中。

**死锁问题**

![image-20230205145700273](/image-20230205145700273.png)

实例

![image-20230205151323633](/image-20230205151323633.png)

原因：源码中有sync，线程A锁住了，第二次调用时线程B无法解锁。如果是同一个线程就可重入了。

解决发慢换发

使用时尽量避免

或者用put方法替代

## 小结

### CopyOnWriteArrayList(Set/Map)

首先我们对 CopyOnWriteArraList 进行学习，其特点如下:

它相当于线程安全的ArrayList。和 ArrayList一样，它是个可变数组；但是和ArrayList 不同的时，它具有以下特性：

1. 它最适合于具有以下特征的应用程序：List 大小通常保持很小，只读操作远多于可变操作，需要在遍历期间防止线程间的冲突。

2. 它是线程安全的。

3. 因为通常需要复制整个基础数组，所以可变操作（add()、set() 和remove() 等等）的开销很大。
4. 迭代器支持 hasNext(), next()等不可变操作，但不支持可变 remove()等操作。

5. 使用迭代器进行遍历的速度很快，并且不会与其他线程发生冲突。在构造迭代器时，迭代器依赖于不变的数组快照

+++

**1. 独占锁效率低：采用读写分离思想解决**

**2. 写线程获取到锁，其他写线程阻塞**

**3. 复制思想：**当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行 Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器。

**这时候会抛出来一个新的问题，也就是数据不一致的问题。如果写线程还没来得及写会内存，其他的线程就会读到了脏数据。**

==**这就是 CopyOnWriteArrayList 的思想和原理。就是拷贝一份。**==

+++

**原因分析**(**重点**)：**动态数组与线程安全**

下面从“动态数组”和“线程安全”两个方面进一步对CopyOnWriteArrayList 的原理进行说明。

- **“动态数组“机制**
  - 它内部有个“volatile 数组”(array)来保持数据。在“添加/修改/删除”数据时，都会新建一个数组，并将更新后的数据拷贝到新建的数组中，最后再将该数组赋值给“volatile 数组”, 这就是它叫做 CopyOnWriteArrayList 的原因
  - **由于它在“添加/修改/删除”数据时，都会新建数组，所以涉及到修改数据的操作，CopyOnWriteArrayList 效率很低；但是单单只是进行遍历查找的话，效率比较高。**
- **“线程安全”机制**
  - 通过 volatile 和互斥锁来实现的。
  - 通过“volatile 数组”来保存数据的。一个线程读取 volatile 数组时，总能看到其它线程对该 volatile 变量最后的写入；就这样，通过 volatile 提供了“读取到的数据总是最新的”这个机制的保证。
  - 通过互斥锁来保护数据。在“添加/修改/删除”数据时，会先“获取互斥锁”，再修改完毕之后，先将数据更新到“volatile 数组”中，然后再“释放互斥锁”，就达到了保护数据的目的。

### 总结

**1.线程安全与线程不安全集合**

集合类型中存在线程安全与线程不安全的两种,常见例如:

ArrayList ---- Vector

HashMap ----HashTable

但是以上都是通过synchronized关键字实现效率较低

**2.Collections构建的线程安全集合**

**3.java.util.concurrent并发包下**

CopyOnWriteArrayList CopyOnWriteArraySet 类型，通过动态数组与线程安全各个方面保证线程安全。

# 5、多线程锁

## 八锁现象

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(()->{
            phone.sendSms();
        },"A").start();

        new Thread(()->{
            phone.call();
        },"B").start();
    }
}

class Phone{
    public synchronized void sendSms(){
        System.out.println("发短信");
    }
    public synchronized void call(){
        System.out.println("打电话");
    }
}
```

### 一、二

**正常执行，两个线程先打印，发短信还是打电话**

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(()->{
            phone.sendSms();
        },"A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone.call();
        },"B").start();
    }
}

class Phone{
    public synchronized void sendSms(){
        System.out.println("发短信");
    }
    public synchronized void call(){
        System.out.println("打电话");
    }
}
```

运行结果：

![image-20211206134528153](/image-20211206134528153.png)

**发短信延时四秒**

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(()->{
            phone.sendSms();
        },"A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone.call();
        },"B").start();
    }
}

class Phone{
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }
    public synchronized void call(){
        System.out.println("打电话");
    }
}
```

运行结果：

![image-20211206134825309](/image-20211206134825309.png)

**原因**

并不是顺序执行！是因为synchronized 锁的对象是方法的调用！对于两个方法用的是同一个锁，谁先拿到谁先执行！另外一个则等待！

### 三

**如果我们添加一个普通方法，那么先执行哪一个呢？**

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(() -> {
            phone.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone.Hello();
        }, "B").start();
    }
}

class Phone {
    public synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }

    public synchronized void call() {
        System.out.println("打电话");
    }

    //普通方法，不受锁的影响
    public void Hello() {
        System.out.println("Hello");
    }
}
```

运行结果：

![image-20211206135404827](/image-20211206135404827.png)

**先执行hello，然后再执行发短信！**

**原因是hello是一个普通方法**，**不受synchronized锁的影响**。

但是我发现，如果我把发短信里面的延迟4秒去掉，那么就会顺序执行，先执行发短信然后再执行hello，原因应该是顺序执行的原因吧，不是太理解。

![image-20211206135522258](/image-20211206135522258.png)

### 四

**如果我们使用的是两个对象，一个调用发短信，一个调用打电话，那么整个顺序是怎么样的呢？**

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            phone1.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone2.call();
            }, "B").start();
    }
}

class Phone {
    public synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }

    public synchronized void call() {
        System.out.println("打电话");
    }

    public void Hello() {
        System.out.println("Hello");
    }
}
```

运行结果

![image-20211206140043137](/image-20211206140043137.png)

原因：**在发短信方法中延迟了4s，又因为synchronized锁的是对象，但是我们这使用的是两个对象，所以每个对象都有一把锁，所以不会造成锁的等待。正常执行**

### 五、六

**如果我们把synchronized的方法加上static变成静态方法！那么顺序又是怎么样的呢？**

（1）我们先来使用一个对象调用两个方法！

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(() -> {
            phone.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone.call();
            }, "B").start();
    }
}

class Phone {
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }

    public static synchronized void call() {
        System.out.println("打电话");
    }
}
```

运行结果

![image-20211206140459978](/image-20211206140459978.png)

（2）如果我们使用两个对象调用两个方法！

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            phone1.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone2.call();
            }, "B").start();
    }
}

class Phone {
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }

    public static synchronized void call() {
        System.out.println("打电话");
    }
}
```

运行结果

![image-20211206140631974](/image-20211206140631974.png)

原因是什么呢？ 为什么加了static就始终前面一个对象先执行呢！为什么后面会等待呢？

**原因是：对于static静态方法来说，对于整个类Class来说只有一份，对于不同的对象使用的是同一份方法，相当于这个方法是属于这个类的，如果静态static方法使用synchronized锁定，那么这个synchronized锁会锁住整个对象！不管多少个对象，对于静态的锁都只有一把锁，谁先拿到这个锁就先执行，其他的进程都需要等待！**

### 七

**如果我们使用一个静态同步方法、一个同步方法、一个对象调用顺序是什么？**

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        new Thread(() -> {
            phone.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone.call();
            }, "B").start();
    }
}

class Phone {
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }

    public synchronized void call() {
        System.out.println("打电话");
    }
}
```

运行结果

![image-20211206140902422](/image-20211206140902422.png)

**因为一个锁的是Class类模板，一个锁的是对象调用者。后面那个打电话不需要等待发短信，直接运行就可以了。**

### 八

如果我们使用一个静态同步方法、一个同步方法、两个对象调用顺序是什么呢？

```java
public class Test01 {
    public static void main(String[] args) {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            phone1.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone2.call();
            }, "B").start();
    }
}

class Phone {
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("发短信");
    }

    public synchronized void call() {
        System.out.println("打电话");
    }
}
```

运行结果

![image-20211206141032030](/image-20211206141032030.png)

因为两个对象，一样的原因：**两把锁锁的不是同一个东西，所以后面的第二个对象不需要等待第一个对象的执行。**

### 小结

**如何判断锁的是谁？锁到底锁的是谁？**

锁会锁住：**对象、类**

**new** 出来的 this 是具体的一个对象

**static Class** 是唯一的一个模板

**Synchronized实现同步的基础：java中的每一个对象都可以作为锁，具体表现为以下三种形式。**

**对于普通同步方法，锁是当前实例对象。**

**对于静态同步方法，锁是当前类的Class对象**

**对于同步方法块，锁是Synchronized括号里的配置的对象**

## 公平锁和非公平锁

![image-20211206101713721](/image-20211206101713721.png)

**公平锁：**非常公平；不能插队的，必须先来后到；

```java 
/**
 * Creates an instance of {@code ReentrantLock}.
 * This is equivalent to using {@code ReentrantLock(false)}.
 */
public ReentrantLock() {
    sync = new NonfairSync();
}
```

**非公平锁：** 非常不公平，允许插队的，可以改变顺序。**(默认为非公平锁)**

```java
/**
 * Creates an instance of {@code ReentrantLock} with the
 * given fairness policy.
 *
 * @param fair {@code true} if this lock should use a fair ordering policy
 */
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

## 可重入锁（递归锁）

![image-20211206142656346](/image-20211206142656346.png)

![image-20211206145627101](/image-20211206145627101.png)

Synchronized锁

```java
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sms();
        },"A").start();
        new Thread(()->{
            phone.sms();
        },"B").start();
    }

}

class Phone{
    public synchronized void sms(){
        System.out.println(Thread.currentThread().getName()+"=> sms");
        call();//这里也有一把锁
    }
    public synchronized void call(){
        System.out.println(Thread.currentThread().getName()+"=> call");
    }
}
```

lock锁

```java 
//lock
public class Demo02 {

    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(()->{
            phone.sms();
        },"A").start();
        new Thread(()->{
            phone.sms();
        },"B").start();
    }

}
class Phone2{

    Lock lock=new ReentrantLock();

    public void sms(){
        lock.lock(); //细节：这个是两把锁，两个钥匙
        //lock锁必须配对，否则就会死锁在里面
        try {
            System.out.println(Thread.currentThread().getName()+"=> sms");
            call();//这里也有一把锁
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
    public void call(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "=> call");
        }catch (Exception e){
            e.printStackTrace();
        }
        finally {
            lock.unlock();
        }
    }
}
```

- lock锁必须配对，相当于lock和 unlock 必须数量相同；
- 在外面加的锁，也可以在里面解锁；在里面加的锁，在外面也可以解锁；

## 自旋锁

spinlock

```java 
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    return var5;
}
```

**自我设计自旋锁：**

```java 
public class SpinlockDemo {

    //int 0
    //thread null
    AtomicReference<Thread> atomicReference=new AtomicReference<>();

    //加锁
    public void myLock(){
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName()+"===> mylock");

        //自旋锁
        while (!atomicReference.compareAndSet(null,thread)){
            System.out.println(Thread.currentThread().getName()+" ==> 自旋中~");
        }
    }


    //解锁
    public void myunlock(){
        Thread thread=Thread.currentThread();
        System.out.println(thread.getName()+"===> myUnlock");
        atomicReference.compareAndSet(thread,null);
    }

}
```

```java 
public class TestSpinLock {
    public static void main(String[] args) throws InterruptedException {
        ReentrantLock reentrantLock = new ReentrantLock();
        reentrantLock.lock();
        reentrantLock.unlock();


        //使用CAS实现自旋锁
        SpinlockDemo spinlockDemo=new SpinlockDemo();
        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myunlock();
            }
        },"t1").start();

        TimeUnit.SECONDS.sleep(1);


        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myunlock();
            }
        },"t2").start();
    }
}
```

运行结果

![image-20211206143213182](/image-20211206143213182.png)

**t1获得锁后，一直不解锁，t2进程必须等待t1进程Unlock后，才能Unlock，在这之前进行自旋等待。。。。**

## 死锁排查

### 概述

**死锁是什么？**

死锁的产生：在资源有限的前提下，两个或者两个以上的线程相互占有对方的资源不释放，且相互不可剥夺，陷入循环等待的情景。

![image-20211206143543034](/image-20211206143543034-1675668113621.png)

**死锁条件：**

互斥

请求和保持

不可抢占

等待循环

**死锁原因：**

系统资源不足

进程运行推进顺序不合适

资源分配不足

### 实例

```java 
import java.util.concurrent.TimeUnit;

public class DeadLock {
    public static void main(String[] args) {
        String lockA= "lockA";
        String lockB= "lockB";

        new Thread(new MyThread(lockA,lockB),"t1").start();
        new Thread(new MyThread(lockB,lockA),"t2").start();
    }
}

class MyThread implements Runnable{

    private String lockA;
    private String lockB;

    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }

    @Override
    public void run() {
        synchronized (lockA){
            System.out.println(Thread.currentThread().getName()+" lock"+lockA+"===>get"+lockB);
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (lockB){
                System.out.println(Thread.currentThread().getName()+" lock"+lockB+"===>get"+lockA);
            }
        }
    }
}
```

### 解决方法

**1、使用jps定位进程号，jdk的bin目录下： 有一个jps**

命令：`jps -l`（linux ps-ef）

![image-20211206144731368](/image-20211206144731368.png)

**2、使用`jstack` 进程进程号 找到死锁信息**

![image-20211206144937385](/image-20211206144937385.png)

**一般情况信息在最后：**

![image-20211206145224466](/image-20211206145224466.png)

**面试，工作中！排查问题！**

**1、日志**

**2、堆栈信息**

# 6、Callable 接口

![image-20211206150542928](/image-20211206150542928.png)

**1、可以有返回值；**

**2、可以抛出异常；**

**3、方法不同，run()/call()**

## 创建线程的方法

1. runnable，执行run方法

   1. **Runable接口又实现类FutureTask**

      **FutureTask构造可以传递Classable**

2. callable，执行call方法

3. thread，执行run方法，本质是runable的实现类

4. 线程池

补充：如何将callable放到thread中？对于thread运行，他只能传入runable类型的参数，这是需要注意到runable中有一个futuretask的实现类，它里面是可以接受callable接口的。

## 代码测试

**传统使用线程方式：**

```java
public class CallableTest {
    public static void main(String[] args) {
        for (int i = 1; i < 10; i++) {
            new Thread(new MyThread()).start();
        }
    }
}

class MyThread implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

**使用Callable进行多线程操作：**

```java
public class CallablrTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        for (int i = 1; i < 10; i++) {
            //new Thread(new Runnable()).start();
			//new Thread(new FutureTask<>( Callable)).start();
            MyThread thread= new MyThread();
            //适配类：FutureTask
            FutureTask<String> futureTask = new FutureTask<>(thread);
            //放入Thread使用
            new Thread(futureTask,String.valueOf(i)).start();
            //获取返回值
            String s = futureTask.get();
            System.out.println("返回值："+ s);
        }
    }
}

class MyThread implements Callable<String> {

    @Override
    public String call() {
        return "123456"+Thread.currentThread().getName();
    }
}
```

这样我们就可以使用Callable来进行多线程编程了，并且我们发现可以有返回值，并且可以抛出异常。

![image-20211206152949846](/image-20211206152949846.png)

### 细节深挖

**Runable接口又实现类FutureTask**

**FutureTask构造可以传递Classable**

源码如下：

![image-20211206152045784](/image-20211206152045784.png)

Calleable 泛型V就是**call运行方法的返回值类型**；

但是**如何使用**呢？

**Callable怎么放入到Thread里面呢？**

源码分析：

![image-20211206152454150](/image-20211206152454150.png)

对于Thread运行，只能传入Runnable类型的参数；

我们这是Callable 怎么办呢？

看JDK api文档：

在Runnable里面有一个叫做FutureTask的实现类，我们进去看一下。

![image-20211206152615949](/image-20211206152615949.png)

FutureTask中可以接受Callable参数；

![image-20211206152636062](/image-20211206152636062.png)

这样我们就可以先把Callable 放入到FutureTask中， 再把FutureTask 放入到Thread就可以了。

## 注意

```JAVA 
public class CallablrTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {

        //new Thread(new Runnable()).start();
        //new Thread(new FutureTask<>( Callable)).start();
        MyThread thread = new MyThread();
        //适配类：FutureTask
        FutureTask<String> futureTask = new FutureTask<>(thread);
        //放入Thread使用

        new Thread(futureTask, "A").start();
        new Thread(futureTask, "B").start();
        //获取返回值
        String s = futureTask.get();
        //这个get方法可能会被阻塞，如果在call方法中是一个耗时的方法，所以一般情况我们会把这个放在最后，或者使用异步通信
        System.out.println("返回值：" + s);
    }
}

class MyThread implements Callable<String> {

    @Override
    public String call() {
        return "123456" + Thread.currentThread().getName();
    }
}
```

运行结果

![image-20211206154427645](/image-20211206154427645.png)

**此处并不是缓存，是由于JVM第二次再调用FutrueTask对象所持有的线程，此时FutrueTask中一个呗vloatile修饰的state此时已经是非NEW状态了，则此时会直接结束对应的线程，就会导致任务也不执行。只是在第一次调用时返回结果并保存。**

## 小结

在主线程中需要执行比较耗时的操作时，但又不想阻塞主线程时，可以把这些作业交给Future对象在后台完成当主线程将来需要时,就可以通过Future对象获得后台作业的计算结果或者执行状态。

一般FutureTask多用于耗时的计算，主线程可以在完成自己的任务后，再去获取结果。

仅在计算完成时才能检索结果；如果计算尚未完成，则阻塞get方法。一旦计算完成，就不能再重新开始或取消计算。get 方法而获取结果只有在计算完成时获取，否则会一直阻塞直到任务转入完成状态, 然后会返回结果或者抛出异常。

只计算一次。

# 7、JUC 三大辅助类

## CountDownLatch (减少计数)

### 基础

- CountDownLatch主要有两个方法，当一个或多个线程调用await方法时，这些线程会阻塞；

- 其它线程调用countDown方法会将计数器减1(调用countDown方法的线程不会阻塞)；

- **当计数器的值变为0时，因await方法阻塞的线程会被唤醒，继续执行**。

![image-20211206165512153](/image-20211206165512153.png)

**其实就是一个减法计数器，对于计数器归零之后再进行后面的操作，这是一个计数器！**

```java 
//这是一个计数器  减法
public class CountDownLatchDemo {

    public static void main(String[] args) throws InterruptedException {
        //总数是6
        CountDownLatch countDownLatch = new CountDownLatch(6);

        for (int i = 1; i <= 6 ; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+" Go out");
                countDownLatch.countDown(); //每个线程都数量-1
            },String.valueOf(i)).start();
        }
        countDownLatch.await();  //等待计数器归零  然后向下执行

        System.out.println("close door");

    }

}
```

主要方法：

- countDown 减一操作；
- await 等待计数器归零。

await等待计数器为0，就唤醒，再继续向下运行。

### CountDownLatch 的原理是什么？

`CountDownLatch` 是共享锁的一种实现,它默认构造 AQS 的 `state` 值为 `count`。当线程使用 `countDown()` 方法时,其实使用了`tryReleaseShared`方法以 CAS 的操作来减少 `state`,直至 `state` 为 0 。当调用 `await()` 方法的时候，如果 `state` 不为 0，那就证明任务还没有执行完毕，`await()` 方法就会一直阻塞，也就是说 `await()` 方法之后的语句不会被执行。然后，`CountDownLatch` 会自旋 CAS 判断 `state == 0`，如果 `state == 0` 的话，就会释放所有等待的线程，`await()` 方法之后的语句得到执行。

## CyclicBarrier（循环栅栏）

### 基础

`CyclicBarrier` 和 `CountDownLatch` 非常类似，它也可以实现线程间的技术等待，但是它的功能比 `CountDownLatch` 更加复杂和强大。主要应用场景和 `CountDownLatch` 类似。

> `CountDownLatch` 的实现是基于 AQS 的，而 `CycliBarrier` 是基于 `ReentrantLock`(`ReentrantLock` 也属于 AQS 同步器)和 `Condition` 的。

`CyclicBarrier` 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是：让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

- CyclicBarrier看英文单词可以看出大概就是循环阻塞的意思，在使用中CyclicBarrier的构造方法第一个参数是目标障碍数，每次执行CyclicBarrier一次障碍数会加一，**如果达到了目标障碍数，才会执行cyclicBarrier.await**(之后的语句。可以将CyclicBarrier理解为加1操作。

![image-20211206170156284](/image-20211206170156284.png)

其实就是一个加法计数器；

```java 
public class CyclicBarrierDemo {
    public static void main(String[] args) {

        //主线程
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7,()->{
            System.out.println("召唤神龙~");
        });

        for (int i = 1; i <= 7; i++) {
            //子线程
            int finalI = i;
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+" 收集了第 {"+ finalI+"} 颗龙珠");
                try {
                    cyclicBarrier.await(); //加法计数 等待
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }

    }
}
```

### CyclicBarrier 的原理是什么？

`CyclicBarrier` 内部通过一个 `count` 变量作为计数器，`count` 的初始值为 `parties` 属性的初始化值，每当一个线程到了栅栏这里了，那么就将计数器减 1。如果 count 值为 0 了，表示这是这一代最后一个线程到达栅栏，就尝试执行我们构造方法中输入的任务。

```java
//每次拦截的线程数
private final int parties;
//计数器
private int count;
```

下面我们结合源码来简单看看。

1、`CyclicBarrier` 默认的构造方法是 `CyclicBarrier(int parties)`，其参数表示屏障拦截的线程数量，每个线程调用 `await()` 方法告诉 `CyclicBarrier` 我已经到达了屏障，然后当前线程被阻塞。

```java
public CyclicBarrier(int parties) {
    this(parties, null);
}

public CyclicBarrier(int parties, Runnable barrierAction) {
    if (parties <= 0) throw new IllegalArgumentException();
    this.parties = parties;
    this.count = parties;
    this.barrierCommand = barrierAction;
}
```

其中，`parties` 就代表了有拦截的线程的数量，当拦截的线程数量达到这个值的时候就打开栅栏，让所有线程通过。

2、当调用 `CyclicBarrier` 对象调用 `await()` 方法时，实际上调用的是 `dowait(false, 0L)`方法。 `await()` 方法就像树立起一个栅栏的行为一样，将线程挡住了，当拦住的线程数量达到 `parties` 的值时，栅栏才会打开，线程才得以通过执行。

```java
public int await() throws InterruptedException, BrokenBarrierException {
  try {
    	return dowait(false, 0L);
  } catch (TimeoutException toe) {
   	 throw new Error(toe); // cannot happen
  }
}
```

`dowait(false, 0L)`方法源码分析如下：

```java
    // 当线程数量或者请求数量达到 count 时 await 之后的方法才会被执行。上面的示例中 count 的值就为 5。
    private int count;
    /**
     * Main barrier code, covering the various policies.
     */
    private int dowait(boolean timed, long nanos)
        throws InterruptedException, BrokenBarrierException,
               TimeoutException {
        final ReentrantLock lock = this.lock;
        // 锁住
        lock.lock();
        try {
            final Generation g = generation;

            if (g.broken)
                throw new BrokenBarrierException();

            // 如果线程中断了，抛出异常
            if (Thread.interrupted()) {
                breakBarrier();
                throw new InterruptedException();
            }
            // cout减1
            int index = --count;
            // 当 count 数量减为 0 之后说明最后一个线程已经到达栅栏了，也就是达到了可以执行await 方法之后的条件
            if (index == 0) {  // tripped
                boolean ranAction = false;
                try {
                    final Runnable command = barrierCommand;
                    if (command != null)
                        command.run();
                    ranAction = true;
                    // 将 count 重置为 parties 属性的初始化值
                    // 唤醒之前等待的线程
                    // 下一波执行开始
                    nextGeneration();
                    return 0;
                } finally {
                    if (!ranAction)
                        breakBarrier();
                }
            }

            // loop until tripped, broken, interrupted, or timed out
            for (;;) {
                try {
                    if (!timed)
                        trip.await();
                    else if (nanos > 0L)
                        nanos = trip.awaitNanos(nanos);
                } catch (InterruptedException ie) {
                    if (g == generation && ! g.broken) {
                        breakBarrier();
                        throw ie;
                    } else {
                        // We're about to finish waiting even if we had not
                        // been interrupted, so this interrupt is deemed to
                        // "belong" to subsequent execution.
                        Thread.currentThread().interrupt();
                    }
                }

                if (g.broken)
                    throw new BrokenBarrierException();

                if (g != generation)
                    return index;

                if (timed && nanos <= 0L) {
                    breakBarrier();
                    throw new TimeoutException();
                }
            }
        } finally {
            lock.unlock();
        }
    }
```

## Semaphore（信号灯）

### 基础

sync和lock一次只允许一个线程访问某个资源，而semaphore一次可以同时控制特定资源的线程数量。

Semaphore的构造方法中传入的第一个参数是最大信号量(可以看成最大线程池)，每个信号量初始化为一个最多只能分发一个许可证。使用acquire方法获得许可证，release 方法释放许可。

抢车位：

3个车位 6辆车：

```java 
public class SemaphoreDemo {
    public static void main(String[] args) {
        //停车位为3个
        Semaphore semaphore = new Semaphore(3);
        for (int i = 1; i <= 6; i++) {
            int finalI = i;
            new Thread(()->{
                try {
                    semaphore.acquire(); //得到
                    //抢到车位
                    System.out.println(Thread.currentThread().getName()+" 抢到了车位{"+ finalI +"}");
                    TimeUnit.SECONDS.sleep(2); //停车2s
                    System.out.println(Thread.currentThread().getName()+" 离开车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }finally {
                    semaphore.release();//释放
                }
            },String.valueOf(i)).start();
        }
    }
}
```

原理：

**semaphore.acquire()获得资源，如果资源已经使用完了，就等待资源释放后再进行使用！**

**semaphore.release()释放，会将当前的信号量释放+1，然后唤醒等待的线程！**

作用： 多个共享资源互斥的使用！ 并发限流，控制最大的线程数！

### 两种模式

- **公平模式：** 调用 `acquire()` 方法的顺序就是获取许可证的顺序，遵循 FIFO；
- **非公平模式：** 抢占式的。

`Semaphore` 对应的两个构造方法如下：

```java
public Semaphore(int permits) {
  	sync = new NonfairSync(permits);
}

public Semaphore(int permits, boolean fair) {
  	sync = fair ? new FairSync(permits) : new NonfairSync(permits);
}
```

**这两个构造方法，都必须提供许可的数量，第二个构造方法可以指定是公平模式还是非公平模式，默认非公平模式。**

`Semaphore` 通常用于那些资源有明确访问数量限制的场景比如限流（仅限于单机模式，实际项目中推荐使用 Redis +Lua 来做限流）。

### Semaphore 的原理是什么？

`Semaphore` 是共享锁的一种实现，它默认构造 AQS 的 `state` 值为 `permits`，你可以将 `permits` 的值理解为许可证的数量，只有拿到许可证的线程才能执行。

调用`semaphore.acquire()` ，线程尝试获取许可证，如果 `state >= 0` 的话，则表示可以获取成功。如果获取成功的话，使用 CAS 操作去修改 `state` 的值 `state=state-1`。如果 `state<0` 的话，则表示许可证数量不足。此时会创建一个 Node 节点加入阻塞队列，挂起当前线程。

```java
/**
 *  获取1个许可证
 */
public void acquire() throws InterruptedException {
 	 sync.acquireSharedInterruptibly(1);
}
/**
 * 共享模式下获取许可证，获取成功则返回，失败则加入阻塞队列，挂起线程
 */
public final void acquireSharedInterruptibly(int arg)
    throws InterruptedException {
    if (Thread.interrupted())
      throw new InterruptedException();
        // 尝试获取许可证，arg为获取许可证个数，当可用许可证数减当前获取的许可证数结果小于0,则创建一个节点加入阻塞队列，挂起当前线程。
    if (tryAcquireShared(arg) < 0)
      doAcquireSharedInterruptibly(arg);
}
```

调用`semaphore.release();` ，线程尝试释放许可证，并使用 CAS 操作去修改 `state` 的值 `state=state+1`。释放许可证成功之后，同时会唤醒同步队列中的一个线程。被唤醒的线程会重新尝试去修改 `state` 的值 `state=state-1` ，如果 `state>=0` 则获取令牌成功，否则重新进入阻塞队列，挂起线程。

```java
// 释放一个许可证
public void release() {
  	sync.releaseShared(1);
}

// 释放共享锁，同时会唤醒同步队列中的一个线程。
public final boolean releaseShared(int arg) {
    //释放共享锁
    if (tryReleaseShared(arg)) {
      //唤醒同步队列中的一个线程
      doReleaseShared();
      return true;
    }
    return false;
}
```

## Exchange(交换者)

Exchanger可以用于线程间交换信息。它提供一个同步点，当两个线程都到达这个同步点时，它们的信息交换。这两个线程通过exchange方法交换数据，如果第一个线程先执行exchange()方法，它会一直等待第二个线程也执行exchange方法，当两个线程都到达同步点时，这两个线程就可以交换数据，将本线程生产出来的数据传递给对方。

下面来看一下Exchanger的应用场景：

1. Exchanger可以用于遗传算法，遗传算法里需要选出两个人作为交配对象，这时候会交换两人的数据，并使用交叉规则得出2个交配结果。
2. Exchanger也可以用于校对工作，比如我们需要将纸制银行流水通过人工的方式录入成电子银行流水，为了避免错误，采用AB岗两人进行录入，录入到Excel之后，系统需要加载这两个Excel，并对两个Excel数据进行校对，看看是否录入一致。

```java
public class ExchangerDemo {
 
	private static final Exchanger<String> exgr = new Exchanger<String>();
	private static ExecutorService threadPool = Executors.newFixedThreadPool(2);
 
	public static void main(String[] args) {
		threadPool.execute(new Runnable() {
			@Override
			public void run() {
				try {
					String A = "银行流水100";// A录入银行流水数据
					String B=exgr.exchange(A);
					System.out.println("A的视角：A和B数据是否一致：" + A.equals(B) + "，A录入的是：" + A + "，B录入是：" + B);
				} catch (InterruptedException e) {
				}
			}
		});
		threadPool.execute(new Runnable() {
			@Override
			public void run() {
				try {
					String B = "银行流水200";// B录入银行流水数据
					String A = exgr.exchange(B);
					System.out.println("B的视角：A和B数据是否一致：" + A.equals(B) + "，A录入的是：" + A + "，B录入是：" + B);
				} catch (InterruptedException e) {
				}
			}
		});
		threadPool.shutdown();
	}
}
```

如果两个线程有一个没有执行exchange()方法，则会一直等待，如果担心有特殊情况发生，避免一直等待，可以使用exchange（V x，longtimeout，TimeUnit unit）设置最大等待时长。

## CyclicBarrier 和 CountDownLatch 的区别

CountDownLatch 是计数器，只能使用一次，而 CyclicBarrier 的计数器提供 reset 功能，可以多次使用。但是我不那么认为它们之间的区别仅仅就是这么简单的一点。我们来从 jdk 作者设计的目的来看，javadoc 是这么描述它们的：

![image-20230203222524104](/image-20230203222524104.png)

对于 CountDownLatch 来说，重点是“一个线程（多个线程）等待”，而其他的 N 个线程在完成“某件事情”之后，可以终止，也可以等待。而对于 CyclicBarrier，重点是多个线程，在任意一个线程没有完成，所有的线程都必须等待。

CountDownLatch 是计数器，线程完成一个记录一个，只不过计数不是递增而是递减，而

CyclicBarrier 更像是一个阀门，需要所有线程都到达，阀门才能打开，然后继续执行。

# 8、读写锁

## 悲观锁和乐观锁

### **悲观锁**

**概述**

优点：能解决并发中的各种问题

缺点：不支持并发操作

![image-20211206172705077](/image-20211206172705077.png)

之所以叫做悲观锁，是因为这是一种对数据的修改持有悲观态度的并发控制方式。总是假设最坏的情况，每次读取数据的时候都默认其他线程会更改数据，因此需要进行加锁操作，当其他线程想要访问数据时，都需要阻塞挂起。悲观锁的实现：

1. 传统的关系型数据库使用这种锁机制，比如行锁、表锁、读锁、写锁等，都是在操作之前先上锁。
2. Java 里面的同步 synchronized 关键字的实现。

**分类**

悲观锁主要分为共享锁和排他锁：

- 共享锁【shared locks】又称为读锁，简称 S 锁。顾名思义，共享锁就是多个事务对于同一数据可以共享一把锁，都能访问到数据，但是只能读不能修改。
- 排他锁【exclusive locks】又称为写锁，简称 X 锁。顾名思义，排他锁就是不能与其他锁并存，如果一个事务获取了一个数据行的排他锁，其他事务就不能再获取该行的其他锁，包括共享锁和排他锁。获取排他锁的事务可以对数据行读取和修改。

**说明**

悲观并发控制实际上是“先取锁再访问”的保守策略，为数据处理的安全提供了保证。但是在效率方面，处理加锁的机制会让数据库产生额外的开销，还有增加产生死锁的机会。另外还会降低并行性，一个事务如果锁定了某行数据，其他事务就必须等待该事务处理完才可以处理那行数据。

### 乐观锁

![image-20211206172746848](/image-20211206172746848.png)

**概述**

特点：支持并发

乐观锁采取了更加宽松的加锁机制。也是为了避免数据库幻读、业务处理时间过长等原因引起数据处理错误的一种机制，但乐观锁不会刻意使用数据库本身的锁机制，而是依据数据本身来保证数据的正确性。乐观锁的实现：

1. CAS 实现：Java 中java.util.concurrent.atomic包下面的原子变量使用了乐观锁的一种 CAS 实现方式。
2. 版本号控制：一般是在数据表中加上一个数据版本号 version 字段，表示数据被修改的次数。当数据被修改时，version 值会 +1。当线程 A 要更新数据时，在读取数据的同时也会读取 version 值，在提交更新时，若刚才读取到的 version 值与当前数据库中的 version 值相等时才更新，否则重试更新操作，直到更新成功。

**说明**

乐观并发控制相信事务之间的数据竞争(data race)的概率是比较小的，因此尽可能直接做下去，直到提交的时候才去锁定，所以不会产生任何锁和死锁。

## 实例

如果我们做一个我们自己的cache缓存。分别有写入操作、读取操作；

我们采用五个线程去写入，使用十个线程去读取。

### 不加锁实现

我们来看一下这个的效果，如果我们不加锁的情况！

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache_ReadWriteLock mycache = new MyCache_ReadWriteLock();
        //开启5个线程 写入数据
        for (int i = 1; i <=5 ; i++) {
            int finalI = i;
            new Thread(()->{
                mycache.put(String.valueOf(finalI),String.valueOf(finalI));
            }).start();
        }
        //开启10个线程去读取数据
        for (int i = 1; i <=10 ; i++) {
            int finalI = i;
            new Thread(()->{
                String o = mycache.get(String.valueOf(finalI));
            }).start();
        }
    }
}

class MyCache_ReadWriteLock{
    private volatile Map<String,String> map=new HashMap<>();

    //使用读写锁
    private ReadWriteLock readWriteLock=new ReentrantReadWriteLock();
    //普通锁
    private Lock lock=new ReentrantLock();

    public void put(String key,String value){
        //写入
        System.out.println(Thread.currentThread().getName()+" 线程 开始写入");
        map.put(key, value);
        System.out.println(Thread.currentThread().getName()+" 线程 写入OK");
    }

    public String get(String key){
        //得到
        System.out.println(Thread.currentThread().getName()+" 线程 开始读取");
        String o = map.get(key);
        System.out.println(Thread.currentThread().getName()+" 线程 读取OK");
        return o;
    }
}
```

运行结果

![image-20211206175108432](/image-20211206175108432.png)

由此可见，我们不加锁的情况，多线程的读写会造成数据不可靠的问题。

我们也可以采用**synchronized**这种重量锁和轻量锁 **lock**去保证数据的可靠。

### 读写锁实现

但是这次我们采用更细粒度的锁：**ReadWriteLock** 读写锁来保证

![image-20211206173947212](/image-20211206173947212.png)

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache_ReadWriteLock mycache = new MyCache_ReadWriteLock();
        //开启5个线程 写入数据
        for (int i = 1; i <=5 ; i++) {
            int finalI = i;
            new Thread(()->{
                mycache.put(String.valueOf(finalI),String.valueOf(finalI));
            }).start();
        }
        //开启10个线程去读取数据
        for (int i = 1; i <=10 ; i++) {
            int finalI = i;
            new Thread(()->{
                String o = mycache.get(String.valueOf(finalI));
            }).start();
        }
    }
}

class MyCache_ReadWriteLock{
    private volatile Map<String,String> map=new HashMap<>();

    //使用读写锁
    private ReadWriteLock readWriteLock=new ReentrantReadWriteLock();
    //普通锁
    private Lock lock=new ReentrantLock();

    public void put(String key,String value){
        //加锁
        readWriteLock.writeLock().lock();
        try {
            //写入
            //业务流程
            System.out.println(Thread.currentThread().getName()+" 线程 开始写入");
            map.put(key, value);
            System.out.println(Thread.currentThread().getName()+" 线程 写入OK");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            readWriteLock.writeLock().unlock(); //解锁
        }
    }

    public String get(String key){
        //加锁
        String o="";
        readWriteLock.readLock().lock();
        try {
            //得到
            System.out.println(Thread.currentThread().getName()+" 线程 开始读取");
            o = map.get(key);
            System.out.println(Thread.currentThread().getName()+" 线程 读取OK");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            readWriteLock.readLock().unlock();
        }
        return o;
    }
}
```

运行结果

![image-20211206175256926](/image-20211206175256926.png)

## 读写锁的演变

读写锁：一个资源可以被多个读线程访问，或者可以被一个写线程访问，但是不能同时存在读写线程，读写互斥，读读共享的。

- 无锁
  - 多线程抢占资源——乱
- 添加锁
  - 使用Synchronized和Reentrantlock都是独占的，每次只能来一个操作
- 读写锁
  - 读读可以共享，提升性能，同时可以多人进行读操作
  - 缺点
    - 造成锁饥饿，一直读，没有写操作，比如坐地铁
    - 读时候，不能写，只能读。完成之后，才可以写，写操作可以读

## 读写锁的降级

将写入锁降级为读锁

![image-20211206182243049](/image-20211206182243049.png)

# 9、阻塞队列

阻塞队列，顾名思义，首先它是一个队列，通过一个共享的队列，可以使得数据由队列的一端输入，从另外一端输出；

![image-20211206185844365](/image-20211206185844365.png)

当队列是空的，从队列中获取元素的操作将会被阻塞

当队列是满的，从队列中添加元素的操作将会被阻塞

试图从空的队列中获取元素的线程将会被阻塞，直到其他线程往空的队列插入新的元素

试图向已满的队列中添加新元素的线程将会被阻塞，直到其他线程从队列中移除一个或多个元素或者完全清空，使队列变得空闲起来并后续新增

在多线程领域：所谓阻塞，在某些情况下会挂起线程（即阻塞），一旦条件满足，被挂起的线程又会自动被唤起

**为什么需要 BlockingQueue**

好处是我们不需要关心什么时候需要阻塞线程，什么时候需要唤醒线程，因为这一切BlockingQueue 都给你一手包办了。

多线程环境中，通过队列可以很容易实现数据共享，比如经典的“生产者”和 “消费者”模型中，通过队列可以很便利地实现两者之间的数据共享。

当队列中没有数据的情况下，消费者端的所有线程都会被自动阻塞（挂起），直到有数据放入队列

当队列中填满数据的情况下，生产者端的所有线程都会被自动阻塞（挂起），直到队列中有空的位置，线程被自动唤醒

## BlockingQueue

### 概述

阻塞队列jdk1.8文档解释：

![image-20230206155330418](/image-20230206155330418.png)

blockingQueue 是Collection的一个子类；

什么情况我们会使用 阻塞队列呢？

**多线程并发处理、线程池！**

![image-20230206155353187](/image-20230206155353187.png)

整个阻塞队列的家族如下：Queue以下实现的有Deque、AbstaractQueue、BlockingQueue；

BlockingQueue以下有Link链表实现的阻塞队列、也有Array数组实现的阻塞队列

###  **BlockingQueue** 的使用

操作：添加、移除

但是实际我们要学的有这4组API

| 方式       | 抛出异常 | 不会抛出异常，有返回值 | 阻塞 等待 | 超时 等待                |
| ---------- | -------- | ---------------------- | --------- | ------------------------ |
| 添加       | add      | offer                  | put       | offer(timenum，timeUnit) |
| 移除       | remove   | poll                   | take      | poll(timenum，timeUnit)  |
| 判断队列首 | element  | peek                   | -         | -                        |

| BlockingQueue核心方法 | 概述                                                         |
| --------------------- | ------------------------------------------------------------ |
| 抛出异常              | 当阻塞队列满时，再往队列里ad d插入元素会抛llegalStateException:Queue full<br/>当阻塞队列空时,再往队列里remove移除元素会抛NoSuchElementException |
| 特殊值                | 插入方法,成功ture失败false<br/>移除方法，成功返回出队列的元素.队列里没有就返回null |
| 一直阻塞              | 当阻塞队列满时，生产者线程继续往队列里put元素，队列会直阻塞生产者线程直到put数据or响应中断退出<br/>当阻塞队列空时，消费者线程试图从队列里take元素，队列会一直阻塞消费者线程直到队列可用 |
| 超时退出              | 当阻塞队列满时，队列会阻塞生产者线程定时间，超过限时后生产者线程会退出 |

```java
/**
     * 抛出异常
     */
    public static void test1(){
        //需要初始化队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.add("a"));
        System.out.println(blockingQueue.add("b"));
        System.out.println(blockingQueue.add("c"));
        //抛出异常：java.lang.IllegalStateException: Queue full
//        System.out.println(blockingQueue.add("d"));
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        //如果多移除一个
        //这也会造成 java.util.NoSuchElementException 抛出异常
        System.out.println(blockingQueue.remove());
    }
=======================================================================================
/**
     * 不抛出异常，有返回值
     */
    public static void test2(){
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        //添加 一个不能添加的元素 使用offer只会返回false 不会抛出异常
        System.out.println(blockingQueue.offer("d"));

        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        //弹出 如果没有元素 只会返回null 不会抛出异常
        System.out.println(blockingQueue.poll());
    }
=======================================================================================
/**
     * 等待 一直阻塞
     */
    public static void test3() throws InterruptedException {
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        //一直阻塞 不会返回
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");

        //如果队列已经满了， 再进去一个元素  这种情况会一直等待这个队列 什么时候有了位置再进去，程序不会停止
//        blockingQueue.put("d");

        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        //如果我们再来一个  这种情况也会等待，程序会一直运行 阻塞
        System.out.println(blockingQueue.take());
    }
=======================================================================================
/**
     * 等待 超时阻塞
     *  这种情况也会等待队列有位置 或者有产品 但是会超时结束
     */
    public static void test4() throws InterruptedException {
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        blockingQueue.offer("a");
        blockingQueue.offer("b");
        blockingQueue.offer("c");
        System.out.println("开始等待");
        blockingQueue.offer("d",2, TimeUnit.SECONDS);  //超时时间2s 等待如果超过2s就结束等待
        System.out.println("结束等待");
        System.out.println("===========取值==================");
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println("开始等待");
        blockingQueue.poll(2,TimeUnit.SECONDS); //超过两秒 我们就不要等待了
        System.out.println("结束等待");
    }
```

### **常见的 BlockingQueue**

#### ArrayBlockingQueue

由数组结构组成的有界阻塞队列。

#### LinkedBlockingQueue

 由链表结构组成的有界（但大小默认值为integer.MAX_VALUE）阻塞队列

#### 对比

ArrayBlockingQueue 和 LinkedBlockingQueue 是两个最普通也是最常用的阻塞队列，一般情况下，在处理多线程间的生产者消费者问题，使用这两个类足以。

#### DelayQueue

 使用优先级队列实现的延迟无界阻塞队列。

#### PriorityBlockingQueue

一句话总结: 支持优先级排序的无界阻塞队列。

#### SynchronousQueue

 不存储元素的阻塞队列，也即单个元素的队列。

#### LinkedTransferQueue

由链表组成的无界阻塞队列

#### LinkedBlockingDeque

由链表组成的双向阻塞队列

## SynchronousQueue同步队列

同步队列没有容量，也可以视为容量为1的队列；进去一个元素，必须等待取出来之后，才能再往里面放入一个元素；

put方法 和 take方法；

Synchronized和其他的BlockingQueue不一样它不存储元素；put了一个元素，就必须从里面先take出来，否则不能再put进去值！

并且SynchronousQueue的take是使用了lock锁保证线程安全的。

```java
/**
 * 同步队列
 */
public class SynchronousQueueDemo {
    public static void main(String[] args) {
        BlockingQueue<String> synchronousQueue = new SynchronousQueue<>();
        //研究一下 如果判断这是一个同步队列

        //使用两个进程
        // 一个进程 放进去
        // 一个进程 拿出来
        new Thread(()->{
            try {
                System.out.println(Thread.currentThread().getName()+" Put 1");
                synchronousQueue.put("1");
                System.out.println(Thread.currentThread().getName()+" Put 2");
                synchronousQueue.put("2");
                System.out.println(Thread.currentThread().getName()+" Put 3");
                synchronousQueue.put("3");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T1").start();

        new Thread(()->{
            try {
                System.out.println(Thread.currentThread().getName()+" Take "+synchronousQueue.take());
//                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+" Take "+synchronousQueue.take());
//                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+" Take "+synchronousQueue.take());

            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T2").start();
    }
}
```

运行结果

![image-20211206184649554](/image-20211206184649554.png)

### 公平模式和非公平模式的区别：

**声明一个 SynchronousQueue 有两种不同的方式**

公平模式：SynchronousQueue 会采用公平锁，并配合一个 FIFO 队列来阻塞多余的生产者和消费者，从而体系整体的公平策略；

非公平模式（SynchronousQueue 默认）：SynchronousQueue 采用非公平锁，同时配合一个 LIFO 队列来管理多余的生产者和消费者，而后一种模式，如果生产者和消费者的处理速度有差距，则很容易出现饥渴的情况，即可能有某些生产者或者是消费者的数据永远都得不到处理。

## 小结

1. **在多线程领域：所谓阻塞，在某些情况下会挂起线程(即阻塞)，一旦条件满足，被挂起的线程又会自动被唤起**
2. **为什么需要BlockingQueue？**在concurrent包发布以前，在多线程环境下，我们每个程序员都必须去自己控制这些细节，尤其还要兼顾效率和线程安全，而这会给我们的程序带来不小的复杂度。使用后我们不需要关心什么时候需要阻塞线程，什么时候需要唤醒线程，因为这一切BlockingQueue都给你一手包办了

# 10、ThreadPool 线程池

==三大方法、7大参数、4种拒绝策略==

## 池化技术

程序的运行，本质：占用系统的资源！我们需要去优化资源的使用 ===> 池化技术

线程池、JDBC的连接池、内存池、对象池等等。。。。

资源的创建、销毁十分消耗资源

池化技术：**事先准备好一些资源，如果有人要用，就来我这里拿，用完之后还给我，以此来提高效率。**

线程池的优势：线程池做的工作只要是控制运行的线程数量，处理过程中将任务放入队列，然后在线程创建后启动这些任务，如果线程数量超过了最大数量，超出数量的线程排队等候，等其他线程执行完毕，再从队列中取出任务来执行。

线程池的好处：

1. 降低资源的消耗：通过重复利用已创建的线程降低线程创建和销毁造成的销耗。
2. 提高响应的速度：当任务到达时，任务可以不需要等待线程创建就能立即执行。
3. 提高线程的可管理性：线程是稀缺资源，如果无限制的创建,不仅会销耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一-的分配， 调优和监控。

线程复用、可以控制最大并发数、管理线程；

**Java中的线程池是通过Executor框架实现的，该框架中用到了Executor, Executors,ExecutorService, ThreadPoolExecutor 这几个类**

![image-20230206155549228](/image-20230206155549228.png)

## 三大方法

- ExecutorService threadPool = Executors.newSingleThreadExecutor();//单线程的线程池
- ExecutorService threadPool2 = Executors.newFixedThreadPool(5); //创建一个固定的线程池的大小
- ExecutorService threadPool3 = Executors.newCachedThreadPool(); //可伸缩的（可缓冲的线程池）
- newScheduledThreadPool();//不限制线程数量的线程池，可周期性的执行某个业务

```java 
//工具类 Executors 三大方法；
public class Demo01 {
    public static void main(String[] args) {

        ExecutorService threadPool = Executors.newSingleThreadExecutor();//单个线程
        ExecutorService threadPool2 = Executors.newFixedThreadPool(5); //创建一个固定的线程池的大小
        ExecutorService threadPool3 = Executors.newCachedThreadPool(); //可伸缩的

        //线程池用完必须要关闭线程池
        try {

            for (int i = 1; i <=100 ; i++) {
                //通过线程池创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+ " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

## 七大参数

- corePoolSize线程池的核心线程数
- maximumPoolSize能容纳的最大线程数
- keepAlive Time空闲线程存活时间
- unit存活的时间单位
- workQueue存放提交但未执行任务的队列
- threadFactory创建线程的工厂类
- handler等待队列满后的拒绝策略

源码分析

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```

本质：三种方法都是开启的**ThreadPoolExecutor**

```java
public ThreadPoolExecutor(int corePoolSize,  //核心线程池大小
                          int maximumPoolSize, //最大的线程池大小
                          long keepAliveTime,  //超时了没有人调用就会释放
                          TimeUnit unit, //超时单位
                          BlockingQueue<Runnable> workQueue, //阻塞队列
                          ThreadFactory threadFactory, //线程工厂 创建线程的 一般不用动
                          RejectedExecutionHandler handler //拒绝策略
                         ) {
    if (corePoolSize < 0 ||
        maximumPoolSize <= 0 ||
        maximumPoolSize < corePoolSize ||
        keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maximumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```

![image-20211206203423917](/image-20211206203423917.png)

阿里巴巴的Java操作手册中明确说明：对于Integer.MAX_VALUE初始值较大，所以一般情况我们要使用底层的**ThreadPoolExecutor**来创建线程池。

## 业务图

![image-20230206155658047](/image-20230206155658047.png)

## 四种拒绝策略

**当提交的任务数大于（workQueue.size() + maximumPoolSize ），就会触发线程池的拒绝策略**。

- CallerRunsPolicy: 当触发拒绝策略,只要线程池没有关闭的话，则使用调用线程直接运行任务。一般并发比较小,性能要求不高，不允许失败。但是，由于调用者自己运行任务，如果任务提交速度过快,可能导致程序阻塞，性能效率上必然的损失较大
- AbortPolicy:丢弃任务，并抛出拒绝执行RejectedExecutionException异常信息。线程池默认的拒绝策略。必须处理好抛出的异常，否则会打断当前的执行流程，影响后续的任务执行。
- DiscardPolicy:直接丢弃，其他啥都没有
- DiscardOldestPolicy:当触发拒绝策略，只要线程池没有关闭的话，丢弃阻塞队列workQueue中最老的一个任务，并将新任务加入

![image-20211206204738534](/image-20211206204738534.png)

**（1）new ThreadPoolExecutor.AbortPolicy()：** //该拒绝策略为：银行满了，还有人进来，不处理这个人的，并抛出异常

超出最大承载，就会抛出异常：队列容量大小+maxPoolSize

```java
public class Pool {
    public static void main(String[] args) {

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
        );

        //线程池用完必须要关闭线程池
        try {

            for (int i = 1; i <=100 ; i++) {
                //通过线程池创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+ " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

运行结果

![image-20211206205320200](/image-20211206205320200.png)

**（2）new ThreadPoolExecutor.CallerRunsPolicy()：** //该拒绝策略为：哪来的去哪里 main线程进行处理

```java
public class Pool {
    public static void main(String[] args) {

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.CallerRunsPolicy()
        );

        //线程池用完必须要关闭线程池
        try {

            for (int i = 1; i <=100 ; i++) {
                //通过线程池创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+ " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

运行结果

![image-20211206205443746](/image-20211206205443746.png)

**（3）new ThreadPoolExecutor.DiscardPolicy():** //该拒绝策略为：队列满了,丢掉异常，不会抛出异常。

```java
public class Pool {
    public static void main(String[] args) {

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardPolicy()
        );

        //线程池用完必须要关闭线程池
        try {

            for (int i = 1; i <=100 ; i++) {
                //通过线程池创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+ " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

运行结果

![image-20211206205516511](/image-20211206205516511.png)

**（4）new ThreadPoolExecutor.DiscardOldestPolicy()：** //该拒绝策略为：队列满了，尝试去和最早的进程竞争，不会抛出异常

```java
public class Pool {
    public static void main(String[] args) {

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy()
        );

        //线程池用完必须要关闭线程池
        try {

            for (int i = 1; i <=100 ; i++) {
                //通过线程池创建线程
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+ " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

运行结果

![image-20211206205608817](/image-20211206205608817.png)

## CTL属性

![image-20230203170102200](/image-20230203170102200.png)

是线程池中的一个属性，本质就是int类型的属性

高3位描述的是线程池的状态，低29位是描述工作线程的状态，来确认任务是否需要执行（以那种方式执行）

低29表述线程池中现存的工作线程数量

execute中的addwork方法就是添加工作线程的方法

## 线程池的状态

running——正常运行

shutdown——由running状态转换而成。不处理新任务，但是会处理现有的任务

stop——由running状态转换而成。不处理新任务，现有任务也停止

tidying——由shutdown、stop状态转换而成。作为停止前的中间态（马上就要停止了）

terminated——由tidying状态转换而成。在进入tidying状态后，源码中会自动化调用terminated方法。彻底结束

![image-20230206160744878](/image-20230206160744878.png)

## 常见问题

### 在线程池中什么是工作线程

在java的线程池中，工作线程指的是worker对象

```java
addwork(runnalbe，true/false);//true添加的是核心线程数，false添加的是最大线程数
```

worker对象其实是线程池中的一个内部类，继承了AQS，实现了runnable

```java
private final class Worker
extends AbstractQueuedSynchronizer
implements Runnable{}
```

线程池执行任务，实际调用的是worker类中的run方法内部的runworker方法

worker继承AQS的目的是为了添加标识来判断当前工作线程是否可以被打断

### 工作线程存储在哪个位置？

放在了hashset集合中——workers

### 如何在线程池执行任务前后做额外处理？

实现线程池，然后实现这两个方法

![image-20230203174253661](/image-20230203174253661.png)

### 如何合理的分配线程池大小？

hippo4j 运维用的多

在分配线程池容量大小时，必须要根据你的业务类型来决定。

**IO密集型？CPU密集型？混合型？**

CPI密集型：更多的CPU来计算，一直工作

IO密集型：更多的时候线程在等待响应

混合型：啥任务都有

CPU密集型：线程数少一点，CPU内核数+1

IO密集型：线程数多一点，CPU内核数 * 2 （线程等待时间与线程CPU时间之比+1）*CPU数目

**为了压测时，可以动态的监控线程池运行情况以及动态修改线程池内部的核心属性。可以基于线程池提供配置参数get、set方法，动态的调整入参进行判别**

基于set修改核心属性时，不需要额外的操作，线程池支持动态修改

### 如果存在临界资源，如何保证线程的安全性？

1. 加互斥锁
   1. sync、lock
2. 非阻塞锁
   1. cas（真的无锁吗？java层面没锁，c++层面有）
3. 不采用锁
   1. threadlocal，适当的情况下采用volatile也行！
   2. 让多个线程将共享资源copy到本地，就没有多线程操作共享资源的问题
   3. volatile：只要不包含并发对共享数据进行运算（写），基本没问题

### 线程池线程复用的原理是什么？

线程会包装成worker对象执行任务，执行结束后通过while调用getTask方法继续获取任务，获取不到任务就会跳出循环

getTask方法中会对allowCoreThreadTimeOut判定，若设置了超时时间，超时就会被回收；若没有设置超时时间从工作队列中获取，获取不到就会等待。

## 小结

### 工作原理

![image-20211206213255464](/image-20211206213255464.png)

1. 在创建了线程池后，线程池中的线程数为零
2. 当调用execute()方法添加一个请求任务时，线程池会做出如下判断：
   1. 如果正在运行的线程数量小于corePoolSize，那么马上创建线程运行这个任务；
   2. 如果正在运行的线程数量大于或等于corePoolSize，那么将这个任务放入队列；
   3. 如果这个时候队列满了且正在运行的线程数量还小于
      maximumPoolSize，那么还是要创建非核心线程立刻运行这个任务；
   4. 如果队列满了且正在运行的线程数量大于或等于maximumPoolSize，那么线程池会启动饱和拒绝策略来执行。
3. 当一个线程完成任务时，它会从队列中取下一个任务来执行
4. 当一个线程无事可做超过一定的时间(keepAliveTime) 时，线程会判断: 
   1. 如果当前运行的线程数大于corePoolSize，那么这个线程就被停掉。
   2. 所以线程池的所有任务完成后，它最终会收缩到corePoolSize的大小。

### CPU密集型和IO密集型

**如何去设置线程池的最大大小如何去设置？**

#### CPU密集型

**电脑的核数是几核就选择几；选择maximunPoolSize的大小**

我们可以使用代码来来获取逻辑处理器数量。

于是**cpu密集型**的写法如下：

```java
ExecutorService threadPool = new ThreadPoolExecutor(
        2,
        Runtime.getRuntime().availableProcessors(),
        3,
        TimeUnit.SECONDS,
        new LinkedBlockingDeque<>(3),
        Executors.defaultThreadFactory(),
        new ThreadPoolExecutor.AbortPolicy()
);
```

#### IO密集型

在程序中有15个大型任务，io十分占用资源；I/O密集型就是判断我们程序中十分耗I/O的线程数量，大约是最大I/O数的一倍到两倍之间。

# 11、四大函数式接口 

新时代的程序员：**lambda表达式、链式编程、函数式接口、Stream流式计算**

## 函数式接口：只有一个方法的接口

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
//超级多的@FunctionalInterface
//简化编程模型，在新版本的框架底层大量应用
//foreach()的参数也是一个函数式接口，消费者类的函数式接口
```

![image-20211206224915900](/image-20211206224915900.png)

函数型接口可以使用lambda表达式；

## Function函数型接口

```java
public class Demo1 {
    public static void main(String[] args) {
        Function<String, String> function = (str) -> {
            return str;
        };
        System.out.println(function.apply("starasdas"));
    }
}
```

![image-20211206225231581](/image-20211206225231581.png)

## Predicate断定型接口

```java
/**
 * 断定型接口：有一个输入参数，返回值只能是 布尔值！
 */
public class Demo2 {
    public static void main(String[] args) {
        //判断字符串是否为空
        Predicate<String> predicate = (str)->{return str.isEmpty();};
        System.out.println(predicate.test("11"));
        System.out.println(predicate.test(""));
    }
}

```

![image-20211206225329801](/image-20211206225329801.png)

## Consummer 消费型接口

```java
public class Demo3 {
    public static void main(String[] args) {
        Consumer<String> consumer = (str)->{
            System.out.println(str);
        };
        consumer.accept("abc");
    }
}
```

![image-20211206225506361](/image-20211206225506361.png)

## Supplier供给型接口

```java
/**
 * 供给型接口，只返回，不输入
 */
public class Demo4 {
    public static void main(String[] args) {
        Supplier<String> supplier = ()->{return "1024";};
        System.out.println(supplier.get());
    }
}
```

![image-20211206225618024](/image-20211206225618024.png)

# 12、Stream流计算

## 概述

**存储**：集合、MySQL

**计算**：流式计算~

## 链式编程

题目要求

一分钟内完成此题，只能用一行代码实现！

现在有5个用户，筛选：

1、ID必须是偶数

2、年龄必须大于23岁

3、用户名转为大写字母

4、用户名字母倒着排序

5、只输出一个用户！

```java 
public class Test {
    public static void main(String[] args) {
        User user1 = new User(1,"a",21);
        User user2 = new User(2,"b",22);
        User user3 = new User(3,"c",23);
        User user4 = new User(4,"d",24);
        User user5 = new User(5,"e",25);
        User user6 = new User(6,"f",26);
        List<User> list = Arrays.asList(user1, user2, user3, user4, user5, user6);

        //计算交给流
        //链式编程！！！！
        list.stream()
                .filter((u)->{ return u.getId()%2==0; })
                .filter((u)->{return u.getAge()>23;})
                .map((u)->{return u.getName().toUpperCase();})
                .sorted((uu1,uu2)->{
                    return uu2.compareTo(uu1);
                })
                .limit(1)
                .forEach(System.out::println);
    }
}
```

# 13、Fork/Join 框架

## 概述

Fork/Join 它可以将一个大的任务拆分成多个子任务进行并行处理，最后将子任务结果合并成最后的计算结果，并进行输出。Fork/Join 框架要完成两件事情：

Fork：把一个复杂任务进行分析，大事化小

Join：把拆分任务的结果进行合并

大数据中：**MapReduce 核心思想->把大任务拆分为小任务！**

## ForkJoin 特点

工作窃取！

实现原理是：**双端队列**！从上面和下面都可以去拿到任务进行执行！

![image-20211206215441558](/image-20211206215441558.png)

## ForkJoin的使用

- 通过**ForkJoinPool**来执行
- 计算任务 **execute(ForkJoinTask<?> task)**

![image-20211206215533661](/image-20211206215533661.png)

- 计算类要去继承ForkJoinTask；
- ForkJoinTask:我们要使用Fork/Join 框架，首先需要创建一个ForkJoin任务。该类提供了在任务中执行fork和join的机制。通常情况下我们不需要直接集成ForkJoinTask类，只需要继承它的子类，Fork/Join 框架提供了两个子类：
  - a.RecursiveAction：用于没有返回结果的任务
  - b.Recursive Task：用于有返回结果的任务
- ForkJoinPool：ForkJoinTask需要通过ForkJoinPool来执行
- RecursiveTask：继承后可以实现递归(自己调自己)调用的任务
- Fork/Join框架的实现原理
  - ForkJoinPool由ForkJoinTask数组和ForkJoinWorkerThread数组组成，ForkJoinTask数组负责将存放以及将程序提交给ForkJoinPool，而ForkJoinWorkerThread负责执行这些任务。

### 实例

```java 
import java.util.concurrent.RecursiveTask;

public class ForkJoinDemo extends RecursiveTask<Long> {

    private long star;
    private long end;

    //临界值
    private long temp=1000000L;

    public ForkJoinDemo(long star, long end) {
        this.star = star;
        this.end = end;
    }

    /**
     * 计算方法
     * @return Long
     */
    @Override
    protected Long compute() {
        if((end-star)<temp){
            Long sum = 0L;
            for (Long i = star; i < end; i++) {
                sum+=i;
            }
//            System.out.println(sum);
            return sum;
        }else {
            //使用forkJoin 分而治之 计算
            //计算平均值
            long middle = (star+ end)/2;
            ForkJoinDemo forkJoinDemoTask1 = new ForkJoinDemo(star, middle);
            forkJoinDemoTask1.fork();  //拆分任务，把线程任务压入线程队列
            ForkJoinDemo forkJoinDemoTask2 = new ForkJoinDemo(middle, end);
            forkJoinDemoTask2.fork();  //拆分任务，把线程任务压入线程队列
            long taskSum = forkJoinDemoTask1.join() + forkJoinDemoTask2.join();
            return taskSum;
        }
    }
}
```

**测试类**

```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.stream.LongStream;

public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        test1();
        test2();
        test3();
    }

    /**
     * 普通计算
     */
    public static void test1(){
        long star = System.currentTimeMillis();
        long sum = 0L;
        for (long i = 1; i < 20_0000_0000; i++) {
            sum+=i;
        }
        long end = System.currentTimeMillis();
        System.out.println(sum);
        System.out.println("普通计算,sum="+"时间："+(end-star));
    }

    /**
     * 使用ForkJoin
     */
    public static void test2() throws ExecutionException, InterruptedException {
        long star = System.currentTimeMillis();
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinDemo(0L, 20_0000_0000L);
        ForkJoinTask<Long> submit = forkJoinPool.submit(task);
        Long aLong = submit.get();
        System.out.println(aLong);
        long end = System.currentTimeMillis();
        System.out.println("使用ForkJoin,sum="+"时间："+(end-star));
    }

    /**
     * 使用Stream 并行流
     */
    public static void test3(){
        long star = System.currentTimeMillis();
        //Stream并行流()
        long sum = LongStream.range(0L, 20_0000_0000L).parallel().reduce(0, Long::sum);
        System.out.println(sum);
        long end = System.currentTimeMillis();
        System.out.println("使用Stream 并行流,sum="+"时间："+(end-star));
    }
}
```

运行结果

![image-20211206221557266](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211206221557266.png)

### Fork/Join的异常处理

ForkJoinTask在执行的时候可能会抛出异常，但是我们没办法在主线程里直接捕获异常，所以ForkJoinTask提供了isCompletedAbnormally()方法来检查任务是否已经抛出异常或已经被取消了，并且可以通过ForkJoinTask的getException方法获取异常。

getException方法返回Throwable对象，如果任务被取消了则返回CancellationException。如果任务没有完成或者没有抛出异常则返回null。

**.parallel().reduce(0, Long::sum)使用一个并行流去计算整个计算，提高效率。**

![image-20211206215831641](/image-20211206215831641.png)

**reduce方法的优点：**

![image-20211206215917543](/image-20211206215917543.png)

# 14、异步回调

## Future

Future 设计的初衷：对将来的某个事件结果进行建模！

![image-20211206222130880](/image-20211206222130880.png)

### 主要缺点

- 不支持手动完成
  - 我提交了一个任务，但是执行太慢了，我通过其他路径已经获取到了任务结果，现在没法把这个任务结果通知到正在执行的线程，所以必须主动取消或者一直待它执行完成
- 不支持进一步的非阻塞调用
  - 通过 Future 的 get 方法会一直阻塞到任务完成，但是想在获取任务之后执行额外的任务，因为 Future 不支持回调函数，所以无法实现这个功能
- 不支持链式调用
  - 对于 Future 的执行结果，我们想继续传到下一个Future 处理使用，从而形成一个链式的pipline调用，这在Future中是没法实现的。
- 不支持多个Future合并
  - 比如我们有 10 个 Future 并行执行，我们想在所有的 Future 运行完毕之后，执行某些函数，是没法通过 Future 实现的。
- 不支持异常处理
  - Future的API没有任何的异常处理的api，所以在异步运行时，如果出了问题是不好定位的。

## CompletableFuture

但是我们平时都使用**CompletableFuture**

CompletableFuture在Java里面被用于异步编程，异步通常意味着非阻塞，可以使得我们的任务单独运行在与主线程分离的其他线程中,并且通过回调可以在主线程中得到异步任务的执行状态，是否完成，和是否异常等信息。

CompletableFuture实现了Future, CompletionStage接口，实现了Future接日就可以兼容现在有线程池框架，而CompletionStage接口才是异步编程的接口抽象，里面定义多种异步方法，通过这两者集合，从而打造出了强大的CompletableFuture类。

### 常用方法

`CompletableFuture` 同时实现了 `Future` 和 `CompletionStage` 接口。

```java
public class CompletableFuture<T> implements Future<T>, CompletionStage<T> {
}
```

`CompletableFuture` 除了提供了更为好用和强大的 `Future` 特性之外，还提供了函数式编程的能力。

![img](/image-20210902092441434.png)

`Future` 接口有 5 个方法：

- `boolean cancel(boolean mayInterruptIfRunning)` ：尝试取消执行任务。
- `boolean isCancelled()` ：判断任务是否被取消。
- `boolean isDone()` ： 判断任务是否已经被执行完成。
- `get()` ：等待任务执行完成并获取运算结果。
- `get(long timeout, TimeUnit unit)` ：多了一个超时时间。

`CompletionStage<T>` 接口中的方法比较多，`CompletableFuture` 的函数式能力就是这个接口赋予的。从这个接口的方法参数你就可以发现其大量使用了 Java8 引入的函数式编程。



### CompletableFuture入门

#### 使用CompletableFuture

主线程里面创建一个CompletableFuture，然后主线程调用get方法会阻塞，最后我们在一个子线程中使其终止。

```java
public class Test01 {
    public static void main(String[] args) throws Exception {
        CompletableFuture<String> future = new CompletableFuture<>();
        new Thread(() -> {
            try {
                System.out.println(Thread.currentThread().getName() + "子线程开始干活");
                //子线程睡 5 秒
                Thread.sleep(5000);
                //在子线程中完成主线程
                future.complete("success");
            } catch (Exception e) {
                e.printStackTrace();
            }
        }, "A").start();
        //主线程调用 get 方法阻塞
        System.out.println("主线程调用 get 方法获取结果为: " + future.get());
        System.out.println("主线程完成,阻塞结束!!!!!!");
    }
}
```

![image-20211206223351228](/image-20211206223351228.png)

#### 没有返回值的异步回调——runAsync

##### test01

```java
public static void main(String[] args) throws ExecutionException, InterruptedException 
{
        // 发起 一个 请求

        System.out.println(System.currentTimeMillis());
        System.out.println("---------------------");
        CompletableFuture<Void> future = CompletableFuture.runAsync(()->{
            //发起一个异步任务
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+".....");
        });
        System.out.println(System.currentTimeMillis());
        System.out.println("------------------------------");
        //输出执行结果
        System.out.println(future.get());  //获取执行结果
 }
```

![image-20211206223253288](/image-20211206223253288.png)

##### test02

```java
public static void main(String[] args) throws ExecutionException, InterruptedException 
{
        // 发起 一个 请求

        System.out.println(System.currentTimeMillis());
        System.out.println("---------------------");
        CompletableFuture<Void> future = CompletableFuture.runAsync(()->{
            //发起一个异步任务
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+".....");
        });
        System.out.println(System.currentTimeMillis());
        System.out.println("------------------------------");
        //输出执行结果
        System.out.println(future.get());  //获取执行结果
 }
```

#### 有返回值的异步回调supplyAsync

##### test01

```java
public class Test03 {
    public static void main(String[] args) throws Exception {
        System.out.println("主线程开始");
        //运行一个有返回值的异步任务
        CompletableFuture<String> future =
                CompletableFuture.supplyAsync(() -> {
                    try {
                        System.out.println("子线程开始任务");
                        Thread.sleep(5000);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    return "子线程完成了!";
                });
        //主线程阻塞
        String s = future.get();
        System.out.println("主线程结束, 子线程的结果为:" + s);
    }
}
```

![image-20211206223541793](/image-20211206223541793.png)

##### test02

```java
//有返回值的异步回调
CompletableFuture<Integer> completableFuture=CompletableFuture.supplyAsync(()->{
    System.out.println(Thread.currentThread().getName());
    try {
        TimeUnit.SECONDS.sleep(2);
        int i=1/0;
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return 1024;
});
System.out.println(completableFuture.whenComplete((t, u) -> {
    //success 回调
    System.out.println("t=>" + t); //正常的返回结果
    System.out.println("u=>" + u); //抛出异常的 错误信息
}).exceptionally((e) -> {
    //error回调
    System.out.println(e.getMessage());
    return 404;
}).get());
```

**whenComplete**: 有两个参数，一个是t 一个是u

T：是代表的 **正常返回的结果**；

U：是代表的 **抛出异常的错误信息**；

如果发生了异常，get可以获取到**exceptionally**返回的值；

#### **线程依赖**

当一个线程依赖另一个线程时，可以使用 thenApply 方法来把这两个线程串行化。

```java
public class Test04 {
    private static Integer num = 10;

    /**
     * 先对一个数加 10,然后取平方
     *
     * @param args
     */
    public static void main(String[] args) throws Exception {
        System.out.println("主线程开始");
        CompletableFuture<Integer> future =
                CompletableFuture.supplyAsync(() -> {
                    try {
                        System.out.println("加 10 任务开始");
                        num += 10;
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    return num;
                }).thenApply(integer -> {
                    return num * num;
                });
        Integer integer = future.get();
        System.out.println("主线程结束, 子线程的结果为:" + integer);
    }
}
```

![image-20211206224148327](/image-20211206224148327.png)

#### **消费处理结果**

thenAccept 消费处理结果，接收任务的处理结果，并消费处理，无返回结果。

```java
public class Test05 {
    private static Integer num = 10;

    public static void main(String[] args) throws Exception {
        System.out.println("主线程开始");
        CompletableFuture.supplyAsync(() -> {
            try {
                System.out.println("加 10 任务开始");
                num += 10;
            } catch (Exception e) {
                e.printStackTrace();
            }
            return num;
        }).thenApply(integer -> {
            return num * num;
        }).thenAccept(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {
                System.out.println("子线程全部处理完成,最后调用了 accept,结果为:" + integer);
            }
        });
    }
}
```

![image-20211206224409398](/image-20211206224409398.png)

#### 处理异步结算的结果

当我们获取到异步计算的结果之后，还可以对其进行进一步的处理，比较常用的方法有下面几个：

- `thenApply()`
- `thenAccept()`
- `thenRun()`
- `whenComplete()`

`thenApply()` 方法接受一个 `Function` 实例，用它来处理结果。

```java
// 沿用上一个任务的线程池
public <U> CompletableFuture<U> thenApply(
    Function<? super T,? extends U> fn) {
    return uniApplyStage(null, fn);
}

//使用默认的 ForkJoinPool 线程池（不推荐）
public <U> CompletableFuture<U> thenApplyAsync(
    Function<? super T,? extends U> fn) {
    return uniApplyStage(defaultExecutor(), fn);
}
// 使用自定义线程池(推荐)
public <U> CompletableFuture<U> thenApplyAsync(
    Function<? super T,? extends U> fn, Executor executor) {
    return uniApplyStage(screenExecutor(executor), fn);
}
```

`thenApply()` 方法使用示例如下：

```java
CompletableFuture<String> future = CompletableFuture.completedFuture("hello!")
        .thenApply(s -> s + "world!");
assertEquals("hello!world!", future.get());
// 这次调用将被忽略。
future.thenApply(s -> s + "nice!");
assertEquals("hello!world!", future.get());
```

你还可以进行 **流式调用**：

```java
CompletableFuture<String> future = CompletableFuture.completedFuture("hello!")
        .thenApply(s -> s + "world!").thenApply(s -> s + "nice!");
assertEquals("hello!world!nice!", future.get());
```

**如果你不需要从回调函数中获取返回结果，可以使用 `thenAccept()` 或者 `thenRun()`。这两个方法的区别在于 `thenRun()` 不能访问异步计算的结果。**

`thenAccept()` 方法的参数是 `Consumer<? super T>` 。

```java
public CompletableFuture<Void> thenAccept(Consumer<? super T> action) {
    return uniAcceptStage(null, action);
}

public CompletableFuture<Void> thenAcceptAsync(Consumer<? super T> action) {
    return uniAcceptStage(defaultExecutor(), action);
}

public CompletableFuture<Void> thenAcceptAsync(Consumer<? super T> action,
                                               Executor executor) {
    return uniAcceptStage(screenExecutor(executor), action);
}
```

顾名思义，`Consumer` 属于消费型接口，它可以接收 1 个输入对象然后进行“消费”。

```java
@FunctionalInterface
public interface Consumer<T> {

    void accept(T t);

    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```

`thenRun()` 的方法是的参数是 `Runnable` 。

```java
public CompletableFuture<Void> thenRun(Runnable action) {
    return uniRunStage(null, action);
}

public CompletableFuture<Void> thenRunAsync(Runnable action) {
    return uniRunStage(defaultExecutor(), action);
}

public CompletableFuture<Void> thenRunAsync(Runnable action,
                                            Executor executor) {
    return uniRunStage(screenExecutor(executor), action);
}
```

`thenAccept()` 和 `thenRun()` 使用示例如下：

```java
CompletableFuture.completedFuture("hello!")
        .thenApply(s -> s + "world!").thenApply(s -> s + "nice!").thenAccept(System.out::println);//hello!world!nice!

CompletableFuture.completedFuture("hello!")
        .thenApply(s -> s + "world!").thenApply(s -> s + "nice!").thenRun(() -> System.out.println("hello!"));//hello!
```

`whenComplete()` 的方法的参数是 `BiConsumer<? super T, ? super Throwable>` 。

```java
public CompletableFuture<T> whenComplete(
    BiConsumer<? super T, ? super Throwable> action) {
    return uniWhenCompleteStage(null, action);
}


public CompletableFuture<T> whenCompleteAsync(
    BiConsumer<? super T, ? super Throwable> action) {
    return uniWhenCompleteStage(defaultExecutor(), action);
}
// 使用自定义线程池(推荐)
public CompletableFuture<T> whenCompleteAsync(
    BiConsumer<? super T, ? super Throwable> action, Executor executor) {
    return uniWhenCompleteStage(screenExecutor(executor), action);
}
```

相对于 `Consumer` ， `BiConsumer` 可以接收 2 个输入对象然后进行“消费”。

```java
@FunctionalInterface
public interface BiConsumer<T, U> {
    void accept(T t, U u);

    default BiConsumer<T, U> andThen(BiConsumer<? super T, ? super U> after) {
        Objects.requireNonNull(after);

        return (l, r) -> {
            accept(l, r);
            after.accept(l, r);
        };
    }
}
```

`whenComplete()` 使用示例如下：

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "hello!")
        .whenComplete((res, ex) -> {
            // res 代表返回的结果
            // ex 的类型为 Throwable ，代表抛出的异常
            System.out.println(res);
            // 这里没有抛出异常所有为 null
            assertNull(ex);
        });
assertEquals("hello!", future.get());
```

#### 异常处理

你可以通过 `handle()` 方法来处理任务执行过程中可能出现的抛出异常的情况。

```java
public <U> CompletableFuture<U> handle(
    BiFunction<? super T, Throwable, ? extends U> fn) {
    return uniHandleStage(null, fn);
}

public <U> CompletableFuture<U> handleAsync(
    BiFunction<? super T, Throwable, ? extends U> fn) {
    return uniHandleStage(defaultExecutor(), fn);
}

public <U> CompletableFuture<U> handleAsync(
    BiFunction<? super T, Throwable, ? extends U> fn, Executor executor) {
    return uniHandleStage(screenExecutor(executor), fn);
}
```

示例代码如下：

```java
CompletableFuture<String> future
        = CompletableFuture.supplyAsync(() -> {
    if (true) {
        throw new RuntimeException("Computation error!");
    }
    return "hello!";
}).handle((res, ex) -> {
    // res 代表返回的结果
    // ex 的类型为 Throwable ，代表抛出的异常
    return res != null ? res : "world!";
});
assertEquals("world!", future.get());
```

你还可以通过 `exceptionally()` 方法来处理异常情况。

```java
CompletableFuture<String> future
        = CompletableFuture.supplyAsync(() -> {
    if (true) {
        throw new RuntimeException("Computation error!");
    }
    return "hello!";
}).exceptionally(ex -> {
    System.out.println(ex.toString());// CompletionException
    return "world!";
});
assertEquals("world!", future.get());
```

如果你想让 `CompletableFuture` 的结果就是异常的话，可以使用 `completeExceptionally()` 方法为其赋值。

```java
CompletableFuture<String> completableFuture = new CompletableFuture<>();
// ...
completableFuture.completeExceptionally(
  new RuntimeException("Calculation failed!"));
// ...
completableFuture.get(); // ExecutionException
```

#### 组合 CompletableFuture

你可以使用 `thenCompose()` 按顺序链接两个 `CompletableFuture` 对象。

```java
public <U> CompletableFuture<U> thenCompose(
    Function<? super T, ? extends CompletionStage<U>> fn) {
    return uniComposeStage(null, fn);
}

public <U> CompletableFuture<U> thenComposeAsync(
    Function<? super T, ? extends CompletionStage<U>> fn) {
    return uniComposeStage(defaultExecutor(), fn);
}

public <U> CompletableFuture<U> thenComposeAsync(
    Function<? super T, ? extends CompletionStage<U>> fn,
    Executor executor) {
    return uniComposeStage(screenExecutor(executor), fn);
}
```

`thenCompose()` 方法会使用示例如下：

```java
CompletableFuture<String> future
        = CompletableFuture.supplyAsync(() -> "hello!")
        .thenCompose(s -> CompletableFuture.supplyAsync(() -> s + "world!"));
assertEquals("hello!world!", future.get());
```

在实际开发中，这个方法还是非常有用的。比如说，我们先要获取用户信息然后再用用户信息去做其他事情。

和 `thenCompose()` 方法类似的还有 `thenCombine()` 方法， `thenCombine()` 同样可以组合两个 `CompletableFuture` 对象。

```java
CompletableFuture<String> completableFuture
        = CompletableFuture.supplyAsync(() -> "hello!")
        .thenCombine(CompletableFuture.supplyAsync(
                () -> "world!"), (s1, s2) -> s1 + s2)
        .thenCompose(s -> CompletableFuture.supplyAsync(() -> s + "nice!"));
assertEquals("hello!world!nice!", completableFuture.get());
```

**那 `thenCompose()` 和 `thenCombine()` 有什么区别呢？**

- `thenCompose()` 可以两个 `CompletableFuture` 对象，并将前一个任务的返回结果作为下一个任务的参数，它们之间存在着先后顺序。
- `thenCombine()` 会在两个任务都执行完成后，把两个任务的结果合并。两个任务是并行执行的，它们之间并没有先后依赖顺序。

#### 并行运行多个 CompletableFuture

你可以通过 `CompletableFuture` 的 `allOf()`这个静态方法来并行运行多个 `CompletableFuture` 。

实际项目中，我们经常需要并行运行多个互不相关的任务，这些任务之间没有依赖关系，可以互相独立地运行。

比说我们要读取处理 6 个文件，这 6 个任务都是没有执行顺序依赖的任务，但是我们需要返回给用户的时候将这几个文件的处理的结果进行统计整理。像这种情况我们就可以使用并行运行多个 `CompletableFuture` 来处理。

示例代码如下：

```java
CompletableFuture<Void> task1 =
  CompletableFuture.supplyAsync(()->{
    //自定义业务操作
  });
......
CompletableFuture<Void> task6 =
  CompletableFuture.supplyAsync(()->{
    //自定义业务操作
  });
......
 CompletableFuture<Void> headerFuture=CompletableFuture.allOf(task1,.....,task6);

  try {
    headerFuture.join();
  } catch (Exception ex) {
    ......
  }
System.out.println("all done. ");
```

经常和 `allOf()` 方法拿来对比的是 `anyOf()` 方法。

**`allOf()` 方法会等到所有的 `CompletableFuture` 都运行完成之后再返回**

```java
Random rand = new Random();
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
    try {
        Thread.sleep(1000 + rand.nextInt(1000));
    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        System.out.println("future1 done...");
    }
    return "abc";
});
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
    try {
        Thread.sleep(1000 + rand.nextInt(1000));
    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        System.out.println("future2 done...");
    }
    return "efg";
});
```

调用 `join()` 可以让程序等`future1` 和 `future2` 都运行完了之后再继续执行。

```java
CompletableFuture<Void> completableFuture = CompletableFuture.allOf(future1, future2);
completableFuture.join();
assertTrue(completableFuture.isDone());
System.out.println("all futures done...");
```

输出：

```java
future1 done...
future2 done...
all futures done...
```

**`anyOf()` 方法不会等待所有的 `CompletableFuture` 都运行完成之后再返回，只要有一个执行完成即可！**

```java
CompletableFuture<Object> f = CompletableFuture.anyOf(future1, future2);
System.out.println(f.get());
```

输出结果可能是：

```java
future2 done...
efg
```

也可能是：

```text
future1 done...
abc
```

# 15、JMM

## 概述

> 请你谈谈你对Volatile 的理解

**Volatile**是 Java 虚拟机提供**轻量级的同步机制**

**1、保证可见性**

**2、不保证原子性**

**3、禁止指令重排**

> 什么是JMM？

**JMM：JAVA内存模型，不存在的东西，是一个概念，也是一个约定！**

**关于JMM的一些同步的约定：**

1、线程解锁前，必须把共享变量**立刻**刷回主存；

2、线程加锁前，必须**读取主存**中的最新值到工作内存中；

3、加锁和解锁是同一把锁；

线程中分为 **工作内存、主内存**

- Read（读取）：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用；
- load（载入）：作用于工作内存的变量，它把read操作从主存中变量放入工作内存中；
- Use（使用）：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令；
- assign（赋值）：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中；
- store（存储）：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用；
- write（写入）：作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中；
- lock（锁定）：作用于主内存的变量，把一个变量标识为线程独占状态；
- unlock（解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定；

![image-20211206232219901](/image-20211206232219901.png)

![image-20211206232544261](/image-20211206232544261.png)

JMM对这8种操作给了相应的规定：

- 不允许read和load、store和write操作之一单独出现。即使用了read必须load，使用了store必须write
- 不允许线程丢弃他最近的assign操作，即工作变量的数据改变了之后，必须告知主存
- 不允许一个线程将没有assign的数据从工作内存同步回主内存
- 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是对变量实施use、store操作之前，必须经过assign和load操作
- 一个变量同一时间只有一个线程能对其进行lock。多次lock后，必须执行相同次数的unlock才能解锁
- 如果对一个变量进行lock操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新load或assign操作初始化变量的值
- 如果一个变量没有被lock，就不能对其进行unlock操作。也不能unlock一个被其他线程锁住的变量
- 对一个变量进行unlock操作之前，必须把此变量同步回主内存

![image-20211206232753577](/image-20211206232753577.png)

遇到问题：**程序不知道主存中的值已经被修改过了！；**

## 补充

![image-20211116134016324](/image-20211116134016324.png)

![image-20211116135316591](/image-20211116135316591.png)

### 数据一致性

协议很多，Intel用MESI协议

MESI Cache一致性协议

https://www.cnblogs.com/z00377750/p/9180644.html

![image-20211116140559272](/image-20211116140559272.png)

现代CPU的数据一致性实现=缓存锁（MESI）+总线锁

### 缓存行+伪共享

读取缓存以cache line为基本单位，目前为64bytes（读取时，会把连续的64个字节都读取进来，这64个字节被称为缓存行）

位于同一缓存行的两个不同数据，被两个不同CPU锁住，产生相互影响的伪共享问题

#### 什么是伪共享（不好！）

![image-20211116150510719](/image-20211116150510719.png)

此处的xy可以看为一个缓存行，读取时会被 一起读取。左边的L2只读取x时，会读取整个xy；右边的L2只读取y时，也会会读取整个xy。当左边读取完x后，并将x的值进行修改后，右边需要重新读取xy的值；当右边读取y的值，并将y的值修改后，左边也需要重新读取xy的值。两个互相无关的值在修改自身读取的数据时，会产生缓存行的相互影响问题，因为他们位于同一行。

#### 伪共享的解决方法

在同一缓存行内（用数组存储，保证两个T对象紧挨着）：

![image-20211116153241512](/image-20211116153241512.png)

不在同一缓存行内（提前存储一个56为的值，用于缓存行对其，确保两个T对象不在统一缓存行内）：

![image-20211116153712138](/image-20211116153712138.png)

方法类：

![image-20211116153205807](/image-20211116153205807.png)

运行结果：

![image-20211116154114888](/image-20211116154114888.png)

![image-20211116154127493](/image-20211116154127493.png)

当两个对象不在同一缓存行内时，效率有明显的提升。270>115

**结论：利用空间换取效率**

### 乱序问题

CPU为了提高指令执行效率，会在一条指令执行过程汇总（比如去内存读取数据（慢100倍）），去同时执行另一条指令，前提是，两条指令没有依赖关系

![image-20211116155530678](/image-20211116155530678.png)

#### 乱序执行的根源

https://www.cnblogs.com/liushaodong/p/4777308.html

#### 合并写

写操作也可以进行合并。

当CPU计算完一个数之后，会优先缓存到L1里面，当L1中的缓冲没有命中，就会被缓存到L2中。但由于L2的速度非常慢，当CPU再次对该值进行计算修改时，就会将这些指令合并到一个合并缓存中（将多步合并为一步），作为一个最终的合并结果缓存到L2中。

##### 实例

没有用到合并写：

![image-20211116161007668](/image-20211116161007668.png)

用到合并写：

![image-20211116161100445](/image-20211116161100445.png)

方法类：

![image-20211116161415689](/image-20211116161415689.png)

运行结果：

![image-20211116161502156](/image-20211116161502156.png)

运合并写提高了效率

合并写的buffer只有4个，为了提高效率，每一个buffer满了才进行运算，因此第一种6个，4+2等2的效率就低于第二种4+4的效率。

#### 证明乱序执行

程序

![image-20211116163716285](/image-20211116163716285.png)

当以上代码执行时，如果没有乱序的话，只可能出现1，0；0，1；1,1三种情况。但是如果出现了0,0这种情况就说明出现了乱序

![image-20211116163930108](/image-20211116163930108.png)

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
   [https](https://blog.csdn.net/21aspnet/article/details/88571740)[://blog.csdn.net/21aspnet/article/details/](https://blog.csdn.net/21aspnet/article/details/88571740)[88571740](

## 补充2

### happens-before

JSR 133 引入了 happens-before 这个概念来描述两个操作之间的内存可见性。

**为什么需要 happens-before 原则？** happens-before 原则的诞生是为了程序员和编译器、处理器之间的平衡。程序员追求的是易于理解和编程的强内存模型，遵守既定规则编码即可。编译器和处理器追求的是较少约束的弱内存模型，让它们尽己所能地去优化性能，让性能最大化。happens-before 原则的设计思想其实非常简单：

- 为了对编译器和处理器的约束尽可能少，只要不改变程序的执行结果（单线程程序和正确执行的多线程程序），编译器和处理器怎么进行重排序优化都行。
- 对于会改变程序执行结果的重排序，JMM 要求编译器和处理器必须禁止这种重排序。

![image-20230206223955926](/image-20230206223955926.png)

了解了 happens-before 原则的设计思想，我们再来看看 JSR-133 对 happens-before 原则的定义：

- 如果一个操作 happens-before 另一个操作，那么第一个操作的执行结果将对第二个操作可见，并且第一个操作的执行顺序排在第二个操作之前。
- 两个操作之间存在 happens-before 关系，并不意味着 Java 平台的具体实现必须要按照 happens-before 关系指定的顺序来执行。如果重排序之后的执行结果，与按 happens-before 关系来执行的结果一致，那么 JMM 也允许这样的重排序。

### happens-before 常见规则有哪些？谈谈你的理解？

happens-before 的规则就 8 条，说多不多，重点了解下面列举的 5 条即可。全记是不可能的，很快就忘记了，意义不大，随时查阅即可。

1. **程序顺序规则** ：一个线程内，按照代码顺序，书写在前面的操作 happens-before 于书写在后面的操作；
2. **解锁规则** ：解锁 happens-before 于加锁；
3. **volatile 变量规则** ：对一个 volatile 变量的写操作 happens-before 于后面对这个 volatile 变量的读操作。说白了就是对 volatile 变量的写操作的结果对于发生于其后的任何操作都是可见的。
4. **传递规则** ：如果 A happens-before B，且 B happens-before C，那么 A happens-before C；
5. **线程启动规则** ：Thread 对象的 `start()`方法 happens-before 于此线程的每一个动作。

如果两个操作不满足上述任意一个 happens-before 规则，那么这两个操作就没有顺序的保障，JVM 可以对这两个操作进行重排序。

### happens-before 和 JMM 什么关系？

happens-before 与 JMM 的关系用《Java 并发编程的艺术》这本书中的一张图就可以非常好的解释清楚。

![happens-before 与 JMM 的关系](/image-20220731084604667.png)





# 16、Volatile

## 1、保证可见性

```java
public class JMMDemo01 {

    // 如果不加volatile 程序会死循环
    // 加了volatile是可以保证可见性的
    private volatile static Integer number = 0;

    public static void main(String[] args) {
        //main线程
        //子线程1
        new Thread(()->{
            while (number==0){
            }
        }).start();
        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //子线程2
        new Thread(()->{
            while (number==0){
            }

        }).start();
        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        number=1;
        System.out.println(number);
    }
}
```

## 2、不保证原子性

原子性：不可分割；

线程A在执行任务的时候，不能被打扰的，也不能被分割的，要么同时成功，要么同时失败。

```java
/**
 * 不保证原子性
 * number <=2w
 * 
 */
public class VDemo02 {

    private static volatile int number = 0;

    public static void add(){
        number++; 
        //++ 不是一个原子性操作，是两个~3个操作
        //
    }

    public static void main(String[] args) {
        //理论上number  === 20000

        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 1; j <= 1000 ; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount()>2){
            //main  gc
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName()+",num="+number);
    }
}
```

**如果不加lock和synchronized ，怎么样保证原子性？**

![image-20211206233618942](/image-20211206233618942.png)

解决方法：使用JUC下的原子包下的class——atomic；

![image-20211206233720770](/image-20211206233720770.png)

代码如下：

```java
public class VDemo02 {

    private static volatile AtomicInteger number = new AtomicInteger();

    public static void add(){
//        number++;
        number.incrementAndGet();  //底层是CAS保证的原子性
    }

    public static void main(String[] args) {
        //理论上number  === 20000

        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 1; j <= 1000 ; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount()>2){
            //main  gc
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName()+",num="+number);
    }
}
```

这些类的底层都直接和操作系统挂钩！是在内存中修改值。

Unsafe类是一个很特殊的存在；

## 3、禁止指令重排

我们写的程序，计算机并不是按照我们自己写的那样去执行的

源代码–>编译器优化重排–>指令并行也可能会重排–>内存系统也会重排–>执行

**处理器在进行指令重排的时候，会考虑数据之间的依赖性！**

```java
int x=1; //1
int y=2; //2
x=x+5;   //3
y=x*x;   //4

//我们期望的执行顺序是 1_2_3_4  可能执行的顺序会变成2134 1324
//可不可能是 4123？ 不可能的
```

可能造成的影响结果：前提：a b x y这四个值 默认都是0

线程A	线程B
x=a	y=b
b=1	a=2
正常的结果： x = 0; y =0;

线程A	线程B
x=a	y=b
b=1	a=2
可能在线程A中会出现，先执行b=1,然后再执行x=a；

在B线程中可能会出现，先执行a=2，然后执行y=b；

那么就有可能结果如下：x=2; y=1.

volatile可以避免指令重排：

volatile中会加一道内存的屏障，这个内存屏障可以保证在这个屏障中的指令顺序。

内存屏障：CPU指令。作用：

1、保证特定的操作的执行顺序；

2、可以保证某些变量的内存可见性（利用这些特性，就可以保证volatile实现的可见性）

![image-20211206234007029](/image-20211206234007029.png)

## 总结

- **volatile可以保证可见性；**
- **不能保证原子性**
- **由于内存屏障，可以保证避免指令重排的现象产生**

面试官：那么你知道在哪里用这个内存屏障用得最多呢？**单例模式**

### synchronized 和 volatile 有什么区别？

`synchronized` 关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！

- `volatile` 关键字是线程同步的轻量级实现，所以 `volatile`性能肯定比`synchronized`关键字要好 。但是 `volatile` 关键字只能用于变量而 `synchronized` 关键字可以修饰方法以及代码块 。
- `volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。
- `volatile`关键字主要用于解决变量在多个线程之间的可见性，而 `synchronized` 关键字解决的是多个线程之间访问资源的同步性。

# 17、玩转单例模式

## 饿汉式

```java
/**
 * 饿汉式单例
 */
public class Hungry {

    /**
     * 可能会浪费空间
     */
    private byte[] data1=new byte[1024*1024];
    private byte[] data2=new byte[1024*1024];
    private byte[] data3=new byte[1024*1024];
    private byte[] data4=new byte[1024*1024];



    private Hungry(){

    }
    private final static Hungry hungry = new Hungry();

    public static Hungry getInstance(){
        return hungry;
    }

}
```

DCL懒汉式

```java
//懒汉式单例模式
public class LazyMan {

    private static boolean key = false;

    private LazyMan(){
        synchronized (LazyMan.class){
            if (key==false){
                key=true;
            }
            else{
                throw new RuntimeException("不要试图使用反射破坏异常");
            }
        }
        System.out.println(Thread.currentThread().getName()+" ok");
    }
    private volatile static LazyMan lazyMan;

    //双重检测锁模式 简称DCL懒汉式
    public static LazyMan getInstance(){
        //需要加锁
        if(lazyMan==null){
            synchronized (LazyMan.class){
                if(lazyMan==null){
                    lazyMan=new LazyMan();
                    /**
                     * 1、分配内存空间
                     * 2、执行构造方法，初始化对象
                     * 3、把这个对象指向这个空间
                     *
                     *  就有可能出现指令重排问题
                     *  比如执行的顺序是1 3 2 等
                     *  我们就可以添加volatile保证指令重排问题
                     */
                }
            }
        }
        return lazyMan;
    }
    //单线程下 是ok的
    //但是如果是并发的
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException, NoSuchFieldException {
        //Java中有反射
//        LazyMan instance = LazyMan.getInstance();
        Field key = LazyMan.class.getDeclaredField("key");
        key.setAccessible(true);
        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
        declaredConstructor.setAccessible(true); //无视了私有的构造器
        LazyMan lazyMan1 = declaredConstructor.newInstance();
        key.set(lazyMan1,false);
        LazyMan instance = declaredConstructor.newInstance();

        System.out.println(instance);
        System.out.println(lazyMan1);
        System.out.println(instance == lazyMan1);
    }
}
```

静态内部类

```java
//静态内部类
public class Holder {
    private Holder(){

    }
    public static Holder getInstance(){
        return InnerClass.holder;
    }
    public static class InnerClass{
        private static final Holder holder = new Holder();
    }
}
```

单例不安全, 因为反射

所以可以引用枚举类

![image-20211206235707782](/image-20211206235707782.png)

当你尝试使用反射获取枚举类型时，就会抛出错误。

尝试：

```java
public enum EnumSingle {
    INSTANCE;
    public EnumSingle getInstance(){
        return INSTANCE;
    }
}

class Test{
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor();
        declaredConstructor.setAccessible(true);
        //java.lang.NoSuchMethodException: com.ogj.single.EnumSingle.<init>()

        EnumSingle instance2 = declaredConstructor.newInstance();
        System.out.println(instance1);
        System.out.println(instance2);
    }
}
```

![image-20211206235141155](/image-20211206235141155.png)

枚举类型使用JAD最终反编译后源码：

如果我们看idea 的文件：会发现idea骗了我们，居然告诉我们是有有参构造的，我们使用jad进行反编译。

![image-20211206235409381](/image-20211206235409381.png)

class文件

```java
public final class EnumSingle extends Enum
{

    public static EnumSingle[] values()
    {
        return (EnumSingle[])$VALUES.clone();
    }

    public static EnumSingle valueOf(String name)
    {
        return (EnumSingle)Enum.valueOf(com/ogj/single/EnumSingle, name);
    }

    private EnumSingle(String s, int i)
    {
        super(s, i);
    }

    public EnumSingle getInstance()
    {
        return INSTANCE;
    }

    public static final EnumSingle INSTANCE;
    private static final EnumSingle $VALUES[];

    static 
    {
        INSTANCE = new EnumSingle("INSTANCE", 0);
        $VALUES = (new EnumSingle[] {
            INSTANCE
        });
    }
}
```

修改后

```java
//enum 是什么？ enum本身就是一个Class 类
public enum EnumSingle {
    INSTANCE;
    public EnumSingle getInstance(){
        return INSTANCE;
    }
}

class Test{
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(String.class,int.class);
        declaredConstructor.setAccessible(true);
        //java.lang.NoSuchMethodException: com.ogj.single.EnumSingle.<init>()

        EnumSingle instance2 = declaredConstructor.newInstance();
        System.out.println(instance1);
        System.out.println(instance2);
    }
}
```

使用枚举，我们就可以防止反射破坏了。

![image-20211206234421492](/image-20211206234421492.png)

# 18、深入理解CAS

什么是CAS？

CAS 全称 compare and swap —— 比较并替换，它是并发条件下修改数据的一种机制，包含三个操作数：

- 需要修改的数据的内存地址（V）；
- 对这个数据的旧预期值（A）；
- 需要将它修改为的值（B）；

  CAS的操作步骤如下：

1. 修改前记录数据的内存地址V；
2. 读取数据的当前的值，记录为A；
3. 修改数据的值变为B；
4. 查看地址V下的值是否仍然为A，若为A，则用B替换它；若地址V下的值不为A，表示在自己修改的过程中，其他的线程对数据进行了修改，则不更新变量的值，而是重新从步骤2开始执行，这被称为**自旋**；

通过以上四个步骤对内存中的数据进行修改，就可以保证数据修改的原子性。`CAS`是乐观锁的一种实现，所以这里介绍的步骤和乐观锁的定义差不多，还是很好理解的。

## CAS的使用

```java
public class casDemo {
    //CAS : compareAndSet 比较并交换
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);

        //boolean compareAndSet(int expect, int update)
        //期望值、更新值
        //如果实际值 和 我的期望值相同，那么就更新
        //如果实际值 和 我的期望值不同，那么就不更新
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

        //因为期望值是2020  实际值却变成了2021  所以会修改失败
        //CAS 是CPU的并发原语
        atomicInteger.getAndIncrement(); //++操作
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());
    }
}
```

## Unsafe类

![image-20211207000435875](/image-20211207000435875.png)

![image-20211207000728626](/image-20211207000728626.png)

![image-20211207000923557](/image-20211207000923557.png)

**总结：**

CAS：比较当前工作内存中的值和主内存中的值，如果这个值是期望的，那么则执行操作！如果不是就一直循环，使用的是自旋锁。

**缺点：**

- 循环会耗时；
- 一次性只能保证一个共享变量的原子性；
- 它会存在ABA问题

## CAS：ABA问题？(狸猫换太子)

![image-20211207001102321](/image-20211207001102321.png)

线程1：期望值是1，要变成2；

线程2：两个操作：

- 1、期望值是1，变成3
- 2、期望是3，变成1

所以对于线程1来说，A的值还是1，所以就出现了问题，骗过了线程1；

```java
public class casDemo {
    //CAS : compareAndSet 比较并交换
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);

        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

        //boolean compareAndSet(int expect, int update)
        //期望值、更新值
        //如果实际值 和 我的期望值相同，那么就更新
        //如果实际值 和 我的期望值不同，那么就不更新
        System.out.println(atomicInteger.compareAndSet(2021, 2020));
        System.out.println(atomicInteger.get());

        //因为期望值是2020  实际值却变成了2021  所以会修改失败
        //CAS 是CPU的并发原语
//        atomicInteger.getAndIncrement(); //++操作
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());
    }
}
```

![image-20211207001256778](C:\Users\幻想家懒得想了\AppData\Roaming\Typora\typora-user-images\image-20211207001256778.png)

# 19、原子引用

解决ABA问题，对应的思想：就是使用了**乐观锁~**

带版本号的 原子操作！

**Integer 使用了对象缓存机制，默认范围是-128~127，推荐使用静态工厂方法valueOf获取对象实例，而不是new，因为valueOf使用缓存，而new一定会创建新的对象分配新的内存空间。**

![image-20211207001429042](/image-20211207001429042.png)

所以如果遇到，使用大于128的时候，**使用原子引用的时候，如果超过了这个值，那么就不会进行版本上升！**

```java
public class casDemo02 {
    //CAS : compareAndSet 比较并交换
    public static void main(String[] args) {
        AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference(2020, 1);
        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp();
            System.out.println("a1->" + stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicStampedReference.compareAndSet(2020, 2022, atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));

            System.out.println("a2->" + stamp);

            System.out.println(atomicStampedReference.compareAndSet(2022, 2020, atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));

            System.out.println("a3->" + stamp);

        }, "a").start();

        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp();
            System.out.println("b1->" + stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicStampedReference.compareAndSet(2020, 6666, stamp, atomicStampedReference.getStamp() + 1));

            System.out.println("b2->" + stamp);

        }, "b").start();

    }
}
```

运行结果：

版本号没有升级（原因看阿里管理手册）

![image-20211207003855556](/image-20211207003855556.png)

那么如果我们使用小于128的时候：

```java

public class casDemo02 {
    //CAS : compareAndSet 比较并交换
    public static void main(String[] args) {
        AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference(1, 1);
        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp();
            System.out.println("a1->" + stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicStampedReference.compareAndSet(1, 2, atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));

            System.out.println("a2->" + stamp);

            System.out.println(atomicStampedReference.compareAndSet(2, 1, atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));

            System.out.println("a3->" + stamp);

        }, "a").start();

        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp();
            System.out.println("b1->" + stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicStampedReference.compareAndSet(2, 6, stamp, atomicStampedReference.getStamp() + 1));

            System.out.println("b2->" + stamp);

        }, "b").start();

    }
}
```

运行结果：

![image-20211207003802868](/image-20211207003802868.png)

正常业务操作中，我们一般使用的是一个个对象，一般情况不会遇到这种情况。

# 20、AQS

## AQS核心思想

AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒 时锁分配的机制，这个机制 AQS 是用 CLH 队列锁实现的，即将暂时获取不到锁的线程加入到队列中。

## AQS原理

状态变量state

AQS中定义了一个状态变量state,它有以下两种使用方法:

(1)**互斥锁**

当AQS只实现为互斥锁的时候，每次只要原子更新state的值从0变为1成功了就获取了锁，**可重入是通过不断把state原子更新加1实现的**。

(2)**互斥锁+共享锁**

当AQS需要同时实现为互斥锁+共享锁的时候，**低1 6位存储互斥锁的状态，高1 6位存储共享锁的状态,主要用于实现读写锁。**

互斥锁是一种独占锁， 每次只允许一个线程独占，且当一个线程独占时，其它线程将无法再获取互斥锁及共享锁，但是它自己可以获取共享锁。

共享锁同时允许多个线程占有，只要有一个线程占有 了共享锁，所有线程(包括自己)都将无法再获取互斥锁，但是可以获取共享锁。

**AQS队列**

AQS中维护了一个队列，**获取锁失败(非tryLock()) 的线程都将进入这个队列中排队，等待锁释放后唤醒下一个排队的线程**(互斥锁模式下)。

**condition队列**(等待队列)

AQS中还有另一个非常重要的内部类ConditionObject,它实现了Condition接口,主要用于实现条件锁。

ConditionObject中也维护了一个队列,这个队列主要用于等待条件的成立，当条件成立时，其庀线程将signal这个队列中的元素，将其移动到AQS的队列中，等待占有锁的线程释放锁后被唤醒。

Condition典型的运用场景是在BlockingQueue中的实现，当队列为空时，获取元素的线程阻塞在notEmpty条件上一旦队列中添加了一个元素，将通知notEmpty条件，将其队列中的元素移动到AQS队列中等待被唤醒。

**模板方法**

AQS这个抽象类把模板方法设计模式运用地炉火纯青，它里面定义了一系列的模板方法

互斥尝试获取锁

互斥尝试释放锁

共享尝试获取锁

共享按时释放锁

当前线程是否独占锁

### AQS资源共享方式

AQS 定义两种资源共享方式：`Exclusive`（独占，只有一个线程能执行，如`ReentrantLock`）和`Share`（共享，多个线程可同时执行，如`Semaphore`/`CountDownLatch`）。

一般来说，自定义同步器的共享方式要么是独占，要么是共享，他们也只需实现`tryAcquire-tryRelease`、`tryAcquireShared-tryReleaseShared`中的一种即可。但 AQS 也支持自定义同步器同时实现独占和共享两种方式，如`ReentrantReadWriteLock`。

### 自定义同步器

AQS 定义两种资源共享方式：`Exclusive`（独占，只有一个线程能执行，如`ReentrantLock`）和`Share`（共享，多个线程可同时执行，如`Semaphore`/`CountDownLatch`）。

一般来说，自定义同步器的共享方式要么是独占，要么是共享，他们也只需实现`tryAcquire-tryRelease`、`tryAcquireShared-tryReleaseShared`中的一种即可。但 AQS 也支持自定义同步器同时实现独占和共享两种方式，如`ReentrantReadWriteLock`。

### [#](#自定义同步器) 自定义同步器

同步器的设计是基于模板方法模式的，如果需要自定义同步器一般的方式是这样（模板方法模式很经典的一个应用）：

1. 使用者继承 `AbstractQueuedSynchronizer` 并重写指定的方法。
2. 将 AQS 组合在自定义同步组件的实现中，并调用其模板方法，而这些模板方法会调用使用者重写的方法。

这和我们以往通过实现接口的方式有很大区别，这是模板方法模式很经典的一个运用。

**AQS 使用了模板方法模式，自定义同步器时需要重写下面几个 AQS 提供的钩子方法：**

```java
//独占方式。尝试获取资源，成功则返回true，失败则返回false。
protected boolean tryAcquire(int)
//独占方式。尝试释放资源，成功则返回true，失败则返回false。
protected boolean tryRelease(int)
//共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
protected int tryAcquireShared(int)
//共享方式。尝试释放资源，成功则返回true，失败则返回false。
protected boolean tryReleaseShared(int)
//该线程是否正在独占资源。只有用到condition才需要去实现它。
protected boolean isHeldExclusively()
```

**什么是钩子方法呢？** 钩子方法是一种被声明在抽象类中的方法，一般使用 `protected` 关键字修饰，它可以是空方法（由子类实现），也可以是默认实现的方法。模板设计模式通过钩子方法控制固定步骤的实现。

除了上面提到的钩子方法之外，AQS 类中的其他方法都是 `final` ，所以无法被其他类重写

## 常见问题

### AQS是什么？

AQS是一个抽象类

AQS只是一个JUC包下的基础类，他提供了一些公共的功能，内部并没有什么实际的功能

rentrantLock、Semaphone、CountDownLatch...

### AQS结构？

由volitile修饰的基于CAS修改的state

​	假设从0=》1说明当前线程获取到了资源；从2=》1说明拿到了一个资源

AQS还提供了一个双向链表，结点为node，每一个node内部还会存储一个线程信息

A、B争抢资源，A获取到了，B没有获取到，B使用CAS再尝试几次都没获取到就会被封装成Node存储在双向链表中。

Node可以是互斥也可以是共享

Node还会存储状态，比如结点不排队了，查看后继线程是否挂起，默认状态，是否await，是否需要释放后继结点

CLH（双向队列）+state（int类型的变量）

基于双向队列和CAS的方式操作state，实现了各个JUC下常用的并发内容

![image-20230203205701427](/image-20230203205701427.png)

### 公平锁和非公平锁

公平锁: AQS队列有Node,就直接排队，不竞争锁资源

非公平锁:啥也不管，上来直接先竞争锁资源，然后再走上面套路

### 为什么AQS有一个虚拟的head结点？

当竞争资源时，竞争失败就会被封装为node，存放到双向队列中，再经过几次cas都无法获取到state之后，该线程就会挂起，挂起前会对前一个结点进行调整。waitdtatus由0变为-1

可以基于虚拟的head结点，判断出后续线程是否挂起。

当占用资源的线程释放资源时，就回去双向链表中查看虚拟的head结点中的参数，来判断该队列中是否挂起的线程，有的话，就将离head最近的非虚拟结点唤醒。

如果排队的线程特别多，需要通过从尾部往前查离head最近的节点唤醒

还有一种可能node在挂起的过程中，线程终止了，waitdtatus就会被改为1。

### reentrantLock释放锁时为什么要从后往前找有效节点？

addwirter方法中，新的节点在插入链表时，先先将tail指向自己（只有这个操作是cas操作），再将自己的pre指向上一个节点，再将上一个节点next指向自己，但是因为时间片轮转问题，会导致第三步还没来得及进行。因此需要从后往前，而不是从前往后。

当资源被唤醒后，head直接指向被唤醒的node对象，ws改为0，thread改为null

### 为什么用双向链表？

线程在排队期间是可以被取消的，取消后，需要将前集结点的next指向当前后继结点，如果是单向链表，稚嫩刚找到后继结点或者前级结点，只能找到其中一个，另一个需要遍历整个链表才能找到

# atomic类

## Atomic 原子类介绍

Atomic 翻译成中文是原子的意思。在化学上，我们知道原子是构成一般物质的最小单位，在化学反应中是不可分割的。在我们这里 Atomic 是指一个操作是不可中断的。即使是在多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程干扰。

所以，所谓原子类说简单点就是具有原子/原子操作特征的类。

并发包 `java.util.concurrent` 的原子类都存放在`java.util.concurrent.atomic`下,如下图所示。

![JUC原子类概览](/JUC%E5%8E%9F%E5%AD%90%E7%B1%BB%E6%A6%82%E8%A7%88.png)

根据操作的数据类型，可以将 JUC 包中的原子类分为 4 类

**基本类型**

使用原子的方式更新基本类型

- `AtomicInteger`：整型原子类
- `AtomicLong`：长整型原子类
- `AtomicBoolean` ：布尔型原子类

**数组类型**

使用原子的方式更新数组里的某个元素

- `AtomicIntegerArray`：整型数组原子类
- `AtomicLongArray`：长整型数组原子类
- `AtomicReferenceArray` ：引用类型数组原子类

**引用类型**

- `AtomicReference`：引用类型原子类
- `AtomicMarkableReference`：原子更新带有标记的引用类型。该类将 boolean 标记与引用关联起来，也可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。
- `AtomicStampedReference` ：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

**🐛 修正（参见：[issue#626open in new window](https://github.com/Snailclimb/JavaGuide/issues/626)）** : `AtomicMarkableReference` 不能解决 ABA 问题。

**对象的属性修改类型**

- `AtomicIntegerFieldUpdater`:原子更新整型字段的更新器
- `AtomicLongFieldUpdater`：原子更新长整型字段的更新器
- `AtomicReferenceFieldUpdater`：原子更新引用类型里的字段

## 基本类型原子类

使用原子的方式更新基本类型

- `AtomicInteger`：整型原子类
- `AtomicLong`：长整型原子类
- `AtomicBoolean` ：布尔型原子类

上面三个类提供的方法几乎相同，所以我们这里以 `AtomicInteger` 为例子来介绍。

**AtomicInteger 类常用方法**

```java
public final int get() //获取当前的值
public final int getAndSet(int newValue)//获取当前的值，并设置新的值
public final int getAndIncrement()//获取当前的值，并自增
public final int getAndDecrement() //获取当前的值，并自减
public final int getAndAdd(int delta) //获取当前的值，并加上预期的值
boolean compareAndSet(int expect, int update) //如果输入的数值等于预期值，则以原子方式将该值设置为输入值（update）
public final void lazySet(int newValue)//最终设置为newValue,使用 lazySet 设置之后可能导致其他线程在之后的一小段时间内还是可以读到旧的值。
```

**`AtomicInteger` 类使用示例** :

```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicIntegerTest {

    public static void main(String[] args) {
        int temvalue = 0;
        AtomicInteger i = new AtomicInteger(0);
        temvalue = i.getAndSet(3);
        System.out.println("temvalue:" + temvalue + ";  i:" + i); //temvalue:0;  i:3
        temvalue = i.getAndIncrement();
        System.out.println("temvalue:" + temvalue + ";  i:" + i); //temvalue:3;  i:4
        temvalue = i.getAndAdd(5);
        System.out.println("temvalue:" + temvalue + ";  i:" + i); //temvalue:4;  i:9
    }

}
```

### 基本数据类型原子类的优势

通过一个简单例子带大家看一下基本数据类型原子类的优势

**1、多线程环境不使用原子类保证线程安全（基本数据类型）**

```java
class Test {
        private volatile int count = 0;
        //若要线程安全执行执行count++，需要加锁
        public synchronized void increment() {
                  count++;
        }

        public int getCount() {
                  return count;
        }
}
```

**2、多线程环境使用原子类保证线程安全（基本数据类型）**

```java
class Test2 {
        private AtomicInteger count = new AtomicInteger();

        public void increment() {
                  count.incrementAndGet();
        }
      //使用AtomicInteger之后，不需要加锁，也可以实现线程安全。
       public int getCount() {
                return count.get();
        }
}
```

### AtomicInteger 线程安全原理简单分析

`AtomicInteger` 类的部分源码：

```java
    // setup to use Unsafe.compareAndSwapInt for updates（更新操作时提供“比较并替换”的作用）
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;

    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }

    private volatile int value;
```

`AtomicInteger` 类主要利用 CAS (compare and swap) + volatile 和 native 方法来保证原子操作，从而避免 synchronized 的高开销，执行效率大为提升。

CAS 的原理是拿期望的值和原本的一个值作比较，如果相同则更新成新的值。UnSafe 类的 `objectFieldOffset()` 方法是一个本地方法，这个方法是用来拿到“原来的值”的内存地址。另外 value 是一个 volatile 变量，在内存中可见，因此 JVM 可以保证任何时刻任何线程总能拿到该变量的最新值。

## 数组类型原子类

使用原子的方式更新数组里的某个元素

- `AtomicIntegerArray`：整形数组原子类
- `AtomicLongArray`：长整形数组原子类
- `AtomicReferenceArray` ：引用类型数组原子类

上面三个类提供的方法几乎相同，所以我们这里以 `AtomicIntegerArray` 为例子来介绍。

**`AtomicIntegerArray` 类常用方法** ：

```java
public final int get(int i) //获取 index=i 位置元素的值
public final int getAndSet(int i, int newValue)//返回 index=i 位置的当前的值，并将其设置为新值：newValue
public final int getAndIncrement(int i)//获取 index=i 位置元素的值，并让该位置的元素自增
public final int getAndDecrement(int i) //获取 index=i 位置元素的值，并让该位置的元素自减
public final int getAndAdd(int i, int delta) //获取 index=i 位置元素的值，并加上预期的值
boolean compareAndSet(int i, int expect, int update) //如果输入的数值等于预期值，则以原子方式将 index=i 位置的元素值设置为输入值（update）
public final void lazySet(int i, int newValue)//最终 将index=i 位置的元素设置为newValue,使用 lazySet 设置之后可能导致其他线程在之后的一小段时间内还是可以读到旧的值。
```

**`AtomicIntegerArray` 类使用示例** :

```java
import java.util.concurrent.atomic.AtomicIntegerArray;

public class AtomicIntegerArrayTest {

    public static void main(String[] args) {
        int temvalue = 0;
        int[] nums = { 1, 2, 3, 4, 5, 6 };
        AtomicIntegerArray i = new AtomicIntegerArray(nums);
        for (int j = 0; j < nums.length; j++) {
            System.out.println(i.get(j));
        }
        temvalue = i.getAndSet(0, 2);
        System.out.println("temvalue:" + temvalue + ";  i:" + i);
        temvalue = i.getAndIncrement(0);
        System.out.println("temvalue:" + temvalue + ";  i:" + i);
        temvalue = i.getAndAdd(0, 5);
        System.out.println("temvalue:" + temvalue + ";  i:" + i);
    }

}
```

## 引用类型原子类

基本类型原子类只能更新一个变量，如果需要原子更新多个变量，需要使用 引用类型原子类。

- `AtomicReference`：引用类型原子类
- `AtomicStampedReference`：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。
- `AtomicMarkableReference` ：原子更新带有标记的引用类型。该类将 boolean 标记与引用关联起来，也可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

上面三个类提供的方法几乎相同，所以我们这里以 `AtomicReference` 为例子来介绍。

**`AtomicReference` 类使用示例** :

```java
import java.util.concurrent.atomic.AtomicReference;

public class AtomicReferenceTest {

    public static void main(String[] args) {
        AtomicReference < Person > ar = new AtomicReference < Person > ();
        Person person = new Person("SnailClimb", 22);
        ar.set(person);
        Person updatePerson = new Person("Daisy", 20);
        ar.compareAndSet(person, updatePerson);

        System.out.println(ar.get().getName());
        System.out.println(ar.get().getAge());
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        super();
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

}
```

上述代码首先创建了一个 `Person` 对象，然后把 `Person` 对象设置进 `AtomicReference` 对象中，然后调用 `compareAndSet` 方法，该方法就是通过 CAS 操作设置 ar。如果 ar 的值为 `person` 的话，则将其设置为 `updatePerson`。实现原理与 `AtomicInteger` 类中的 `compareAndSet` 方法相同。运行上面的代码后的输出结果如下：

```text
Daisy
20
```

**`AtomicStampedReference` 类使用示例** :

```java
import java.util.concurrent.atomic.AtomicStampedReference;

public class AtomicStampedReferenceDemo {
    public static void main(String[] args) {
        // 实例化、取当前值和 stamp 值
        final Integer initialRef = 0, initialStamp = 0;
        final AtomicStampedReference<Integer> asr = new AtomicStampedReference<>(initialRef, initialStamp);
        System.out.println("currentValue=" + asr.getReference() + ", currentStamp=" + asr.getStamp());

        // compare and set
        final Integer newReference = 666, newStamp = 999;
        final boolean casResult = asr.compareAndSet(initialRef, newReference, initialStamp, newStamp);
        System.out.println("currentValue=" + asr.getReference()
                + ", currentStamp=" + asr.getStamp()
                + ", casResult=" + casResult);

        // 获取当前的值和当前的 stamp 值
        int[] arr = new int[1];
        final Integer currentValue = asr.get(arr);
        final int currentStamp = arr[0];
        System.out.println("currentValue=" + currentValue + ", currentStamp=" + currentStamp);

        // 单独设置 stamp 值
        final boolean attemptStampResult = asr.attemptStamp(newReference, 88);
        System.out.println("currentValue=" + asr.getReference()
                + ", currentStamp=" + asr.getStamp()
                + ", attemptStampResult=" + attemptStampResult);

        // 重新设置当前值和 stamp 值
        asr.set(initialRef, initialStamp);
        System.out.println("currentValue=" + asr.getReference() + ", currentStamp=" + asr.getStamp());

        // [不推荐使用，除非搞清楚注释的意思了] weak compare and set
        // 困惑！weakCompareAndSet 这个方法最终还是调用 compareAndSet 方法。[版本: jdk-8u191]
        // 但是注释上写着 "May fail spuriously and does not provide ordering guarantees,
        // so is only rarely an appropriate alternative to compareAndSet."
        // todo 感觉有可能是 jvm 通过方法名在 native 方法里面做了转发
        final boolean wCasResult = asr.weakCompareAndSet(initialRef, newReference, initialStamp, newStamp);
        System.out.println("currentValue=" + asr.getReference()
                + ", currentStamp=" + asr.getStamp()
                + ", wCasResult=" + wCasResult);
    }
}
```

输出结果如下：

```text
currentValue=0, currentStamp=0
currentValue=666, currentStamp=999, casResult=true
currentValue=666, currentStamp=999
currentValue=666, currentStamp=88, attemptStampResult=true
currentValue=0, currentStamp=0
currentValue=666, currentStamp=999, wCasResult=true
```

**`AtomicMarkableReference` 类使用示例** :

```java
import java.util.concurrent.atomic.AtomicMarkableReference;

public class AtomicMarkableReferenceDemo {
    public static void main(String[] args) {
        // 实例化、取当前值和 mark 值
        final Boolean initialRef = null, initialMark = false;
        final AtomicMarkableReference<Boolean> amr = new AtomicMarkableReference<>(initialRef, initialMark);
        System.out.println("currentValue=" + amr.getReference() + ", currentMark=" + amr.isMarked());

        // compare and set
        final Boolean newReference1 = true, newMark1 = true;
        final boolean casResult = amr.compareAndSet(initialRef, newReference1, initialMark, newMark1);
        System.out.println("currentValue=" + amr.getReference()
                + ", currentMark=" + amr.isMarked()
                + ", casResult=" + casResult);

        // 获取当前的值和当前的 mark 值
        boolean[] arr = new boolean[1];
        final Boolean currentValue = amr.get(arr);
        final boolean currentMark = arr[0];
        System.out.println("currentValue=" + currentValue + ", currentMark=" + currentMark);

        // 单独设置 mark 值
        final boolean attemptMarkResult = amr.attemptMark(newReference1, false);
        System.out.println("currentValue=" + amr.getReference()
                + ", currentMark=" + amr.isMarked()
                + ", attemptMarkResult=" + attemptMarkResult);

        // 重新设置当前值和 mark 值
        amr.set(initialRef, initialMark);
        System.out.println("currentValue=" + amr.getReference() + ", currentMark=" + amr.isMarked());

        // [不推荐使用，除非搞清楚注释的意思了] weak compare and set
        // 困惑！weakCompareAndSet 这个方法最终还是调用 compareAndSet 方法。[版本: jdk-8u191]
        // 但是注释上写着 "May fail spuriously and does not provide ordering guarantees,
        // so is only rarely an appropriate alternative to compareAndSet."
        // todo 感觉有可能是 jvm 通过方法名在 native 方法里面做了转发
        final boolean wCasResult = amr.weakCompareAndSet(initialRef, newReference1, initialMark, newMark1);
        System.out.println("currentValue=" + amr.getReference()
                + ", currentMark=" + amr.isMarked()
                + ", wCasResult=" + wCasResult);
    }
}
```

输出结果如下：

```text
currentValue=null, currentMark=false
currentValue=true, currentMark=true, casResult=true
currentValue=true, currentMark=true
currentValue=true, currentMark=false, attemptMarkResult=true
currentValue=null, currentMark=false
currentValue=true, currentMark=true, wCasResult=true
```

## 对象的属性修改类型原子类

如果需要原子更新某个类里的某个字段时，需要用到对象的属性修改类型原子类。

- `AtomicIntegerFieldUpdater`:原子更新整形字段的更新器
- `AtomicLongFieldUpdater`：原子更新长整形字段的更新器
- `AtomicReferenceFieldUpdater` ：原子更新引用类型里的字段的更新器

要想原子地更新对象的属性需要两步。第一步，因为对象的属性修改类型原子类都是抽象类，所以每次使用都必须使用静态方法 newUpdater()创建一个更新器，并且需要设置想要更新的类和属性。第二步，更新的对象属性必须使用 public volatile 修饰符。

上面三个类提供的方法几乎相同，所以我们这里以 `AtomicIntegerFieldUpdater`为例子来介绍。

**`AtomicIntegerFieldUpdater` 类使用示例** :

```java
import java.util.concurrent.atomic.AtomicIntegerFieldUpdater;

public class AtomicIntegerFieldUpdaterTest {
	public static void main(String[] args) {
		AtomicIntegerFieldUpdater<User> a = AtomicIntegerFieldUpdater.newUpdater(User.class, "age");

		User user = new User("Java", 22);
		System.out.println(a.getAndIncrement(user));// 22
		System.out.println(a.get(user));// 23
	}
}

class User {
	private String name;
	public volatile int age;

	public User(String name, int age) {
		super();
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

}
```

输出结果：

```text
22
23
```

## LongAdder和AtomicLong

**AtomicLong特点：**

AtomicLong底层是基于CAS自旋操作的，如果在高并发的情况下，CPU会花费大量的时间和性能浪费到CAS自旋上面，非常影响CPU的性能（当然并发量小可以忽略不计）。

**LongAdder特点：**

LongAdder的特点就是分散热点，把value分成若干个cell段，底层类似于ConcurrentHashMap中的分段锁，每个线程只对属于自己的“那段”数据进行CAS操作，也就是说，在最理想的情况下，每个线程都有独立的锁，这样可以大量减少并发操作。

总结：

一些高并发的场景，比如限流计数器，建议使用LongAdder替换AtomicLong，性能可以提升不少。

在低并发的时候通过对base的直接更新可以很好的保障和AtomicLong的性能基本保持一致，而在高并发的时候通过分散提高了性能。缺点是LongAdder在统计的时候如果有并发更新，可能导致统计的数据有误差。

# ThreadLocal

ThreadLocal就是一个工具真正存储数据的是ThreadLocal中的一个Map——ThreadLocalMap

ThreadLocal可以将一个数据和本地线程绑定在一起

## hash算法

既然是`Map`结构，那么`ThreadLocalMap`当然也要实现自己的`hash`算法来解决散列表数组冲突问题。

```java
int i = key.threadLocalHashCode & (len-1);
```

`ThreadLocalMap`中`hash`算法很简单，这里`i`就是当前 key 在散列表中对应的数组下标位置。

这里最关键的就是`threadLocalHashCode`值的计算，`ThreadLocal`中有一个属性为`HASH_INCREMENT = 0x61c88647`

```java
public class ThreadLocal<T> {
    private final int threadLocalHashCode = nextHashCode();

    private static AtomicInteger nextHashCode = new AtomicInteger();

    private static final int HASH_INCREMENT = 0x61c88647;

    private static int nextHashCode() {
        return nextHashCode.getAndAdd(HASH_INCREMENT);
    }

    static class ThreadLocalMap {
        ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
            table = new Entry[INITIAL_CAPACITY];
            int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);

            table[i] = new Entry(firstKey, firstValue);
            size = 1;
            setThreshold(INITIAL_CAPACITY);
        }
    }
}
```

每当创建一个`ThreadLocal`对象，这个`ThreadLocal.nextHashCode` 这个值就会增长 `0x61c88647` 。

这个值很特殊，它是**斐波那契数** 也叫 **黄金分割数**。`hash`增量为 这个数字，带来的好处就是 `hash` **分布非常均匀**。

我们自己可以尝试下：

![image-20230206233022872](/image-20230206233022872.png)

可以看到产生的哈希码分布很均匀，这里不去细纠**斐波那契**具体算法，感兴趣的可以自行查阅相关资料

## hash冲突

> **注明：** 下面所有示例图中，**绿色块**`Entry`代表**正常数据**，**灰色块**代表`Entry`的`key`值为`null`，**已被垃圾回收**。**白色块**表示`Entry`为`null`。

虽然`ThreadLocalMap`中使用了**黄金分割数**来作为`hash`计算因子，大大减少了`Hash`冲突的概率，但是仍然会存在冲突。

`HashMap`中解决冲突的方法是在数组上构造一个**链表**结构，冲突的数据挂载到链表上，如果链表长度超过一定数量则会转化成**红黑树**。

而 `ThreadLocalMap` 中并没有链表结构，所以这里不能使用 `HashMap` 解决冲突的方式了。

![image-20230207002547671](/image-20230207002547671.png)

如上图所示，如果我们插入一个`value=27`的数据，通过 `hash` 计算后应该落入槽位 4 中，而槽位 4 已经有了 `Entry` 数据。

此时就会线性向后查找，一直找到 `Entry` 为 `null` 的槽位才会停止查找，将当前元素放入此槽位中。当然迭代过程中还有其他的情况，比如遇到了 `Entry` 不为 `null` 且 `key` 值相等的情况，还有 `Entry` 中的 `key` 值为 `null` 的情况等等都会有不同的处理，后面会一一详细讲解。

这里还画了一个`Entry`中的`key`为`null`的数据（**Entry=2 的灰色块数据**），因为`key`值是**弱引用**类型，所以会有这种数据存在。在`set`过程中，如果遇到了`key`过期的`Entry`数据，实际上是会进行一轮**探测式清理**操作的，具体操作方式后面会讲到。

## 常见问题

### 不使用声明式事务，如何实现事务

https://blog.csdn.net/wei_gg/article/details/115575391

spring的TransactionSynchronizationManager类底层就是使用threadlocal来实现的

简单的理解：首先在Thread类中，都会维护一个ThreadLocalMap映射表，这个映射表存储的key是ThreadLocal本身，value则是我们存储的局部变量object。

ThreadLocal类中set方法的实现，主要就是依赖于getMap方法。

```java 
    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }
 
    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }
```

每个线程Thread都会维护自己的ThreadLocalMap，这个map的key则是ThreadLocal类本身，而value则是我们保存的数据。ThreadLocal在多线程中是被公共持有的，被隔离的数据实际是存放在每个线程的ThreadLocalMap中的，只不过是通过ThreadLocal的引用得到每个线程维护的ThreadLocalMap中的value。

### ThreadLocal的内存泄漏问题

#### key的内存泄漏

当tl弹栈走了之后，就没有指向threadLocal的在指针的，而key的引用依旧指向tl，就造成了key的内存泄漏

**解决方法**

jdk将key指向threadlocal的引用设置为弱引用，只要GC就会被回收

#### value的内存泄漏

![image-20230203190217699](/image-20230203190217699.png)

这种情况只存在于**线程池中**：在线程池中，线程用完不一定会被回收。因为entry中的val指向ThreadLocal引用是软引用，只要GC，key就会被回收，但是value并没有被清除，因此就造成了内存泄漏。

**解决方法**

在ThreadLocal使用完毕后，进行remove操作

### 作用

不能用于解决共享变量的问题。也不是为了协调线程同步而存在的，是为了方便每个线程处理自己的状态而引入的一个机制，这点至关重要

key是threadLocal实例，value是数据

不是为线程保存对象的副本，它仅仅是只是起到一个索引的作用。主要目的是为每一个线程隔离一个类的实例，这个实例的作用范围仅限线程内部

## 更多

https://javaguide.cn/java/concurrent/threadlocal.html#threadlocal%E9%A1%B9%E7%9B%AE%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%AE%9E%E6%88%98
