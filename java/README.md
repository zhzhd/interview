## 一、基础知识

### 1、Object中有哪些方法，作用是什么

#### （1）hashCode

提供此方法支持的原因是，为了支持一些 hash tables，在不修改 equals 方法的前提下，
在一次程序执行期间，多次调用同一个对象的 hashCode 方法，得到的结果始终应该是一个相同的整数。
如果两个对象的 equals 方法返回值一样，则调用它们各自的 hashCode 方法，返回值也必须保证一样。Object 对象的 hashCode 方法，不同的对象在调用的时候，确实返回了不同的结果，得益于本地实现，返回了对象的内存地址 。
#### （2）equals

equals方法具有以下特性：a：自反性，对于非 null 引用 x， x.equals(x) 应该返回 true。b：对称性，对于非 null 引用 x，y， x.equals(y) 的返回值应该和 y.equals(x) 的返回值保持一致；
#### （3）clone
Object中的clone方法是浅拷贝，当对象中含有可变的引用类型属性时，在复制得到的新对象对该引用类型属性内容进行修改，原始对象响应的属性内容也会发生变化，这就是"浅拷贝"的现象。
#### （4）toString

#### （5）notify、notifyAll
notify/notifyAll() 执行后会唤醒处于等待状态线程获取线程锁、只是notify()只会随机唤醒其中之一获取线程锁，notifyAll() 会唤醒所有处于等待状态的线程抢夺线程锁。
#### （6）wait()、wait(long timeout, int nanos)、wait(long timeout)
wait()执行后拥有当前锁的线程会释放该线程锁，并处于等待状态（等待重新获取锁）

### 2、Error和Exception有什么区别

Error表示系统级的错误和程序不必处理的异常，是恢复不是不可能但很困难的情况下的一种严重问题；比如内存溢出，不可能指望程序能处理这样的情况；Exception表示需要捕捉或者需要程序进行处理的异常，是一种设计或实现问题；也就是说，它表示如果程序运行正常，从不会发生的情况。

### 3、try{}里有一个return语句，那么紧跟在这个try后的finally{}里的代码会不会被执行，什么时候被执行，在return前还是后

会执行，在方法返回调用者前执行

### 4、列出一些你常见的运行时异常

ArithmeticException（算术异常）
ClassCastException （类转换异常）
IllegalArgumentException （非法参数异常）
IndexOutOfBoundsException （下标越界异常）
NullPointerException （空指针异常）
SecurityException （安全异常）

## 二、多线程

### 1、Java中锁的分类

#### （1）公平锁/非公平锁
公平锁是指多个线程按照申请锁的顺序来获取锁。非公平锁是指多个线程获取锁的顺序并不是按照申请锁的顺序，有可能后申请的线程比先申请的线程优先获取锁。有可能，会造成优先级反转或者饥饿现象。
对于Java ReentrantLock而言，通过构造函数指定该锁是否是公平锁，默认是非公平锁。非公平锁的优点在于吞吐量比公平锁大。对于Synchronized而言，也是一种非公平锁。
#### （2）可重入锁
可重入锁又名递归锁，是指在同一个线程在外层方法获取锁的时候，在进入内层方法会自动获取锁。说
#### （3）独享锁/共享锁
独享锁是指该锁一次只能被一个线程所持有。共享锁是指该锁可被多个线程所持有。
#### （4）互斥锁/读写锁
互斥锁在Java中的具体实现就是ReentrantLock，读写锁在Java中的具体实现就是ReadWriteLock
#### （5）乐观锁/悲观锁
悲观锁在Java中的使用，就是利用各种锁。乐观锁在Java中的使用，是无锁编程，常常采用的是CAS算法，典型的例子就是原子类，通过CAS自旋实现原子操作的更新。
#### （6）分段锁
分段锁其实是一种锁的设计，并不是具体的一种锁，对于ConcurrentHashMap而言，其并发的实现就是通过分段锁的形式来实现高效的并发操作。
#### （6）偏向锁/轻量级锁/重量级锁
这三种锁是指锁的状态，并且是针对Synchronized。
偏向锁是指一段同步代码一直被一个线程所访问，那么该线程会自动获取锁。降低获取锁的代价。
轻量级锁是指当锁是偏向锁的时候，被另一个线程所访问，偏向锁就会升级为轻量级锁，其他线程会通过自旋的形式尝试获取锁，不会阻塞，提高性能。
重量级锁是指当锁为轻量级锁的时候，另一个线程虽然是自旋，但自旋不会一直持续下去，当自旋一定次数的时候，还没有获取到锁，就会进入阻塞，该锁膨胀为重量级锁。重量级锁会让其他申请的线程进入阻塞，性能降低。
#### （7）自旋锁
在Java中，自旋锁是指尝试获取锁的线程不会立即阻塞，而是采用循环的方式去尝试获取锁，这样的好处是减少线程上下文切换的消耗，缺点是循环会消耗CPU。

### 2、Java内存模型
为了保证共享内存的正确性（可见性、有序性、原子性），内存模型定义了共享内存系统中多线程程序读写操作行为的规范。内存模型解决并发问题主要采用两种方式：限制处理器优化和使用内存屏障。
Java内存模型（Java Memory Model ,JMM）就是一种符合内存模型规范的，屏蔽了各种硬件和操作系统的访问差异的，保证了Java程序在各种平台下对内存的访问都能保证效果一致的机制及规范。
JMM是一种规范，目的是解决由于多线程通过共享内存进行通信时，存在的本地内存数据不一致、编译器会对代码指令重排序、处理器会对代码乱序执行等带来的问题。目的是保证并发编程场景中的原子性、可见性和有序性。
#### （1）Java内存模型的实现
在Java中提供了一系列和并发处理相关的关键字，比如volatile、synchronized、final、concurren包等。其实这些就是Java内存模型封装了底层的实现后提供给程序员使用的一些关键字。
#### （2）原子性
为了保证原子性，提供了两个高级的字节码指令monitorenter和monitorexit。在Java中可以使用synchronized来保证方法和代码块内的操作是原子性的。
#### （3）可见性
Java中的volatile关键字提供了一个功能，那就是被其修饰的变量在被修改后可以立即同步到主内存，被其修饰的变量在每次是用之前都从主内存刷新。因此，可以使用volatile来保证多线程操作时变量的可见性。synchronized和final两个关键字也可以实现可见性
#### （4）有序性
在Java中，可以使用synchronized和volatile来保证多线程之间操作的有序性。实现方式有所区别：volatile关键字会禁止指令重排。synchronized关键字保证同一时刻只允许一条线程操作。

### 3、synchronized关键字
#### （1）synchronized的用法
synchronized是Java提供的一个并发控制的关键字。主要有两种用法，分别是同步方法和同步代码块。
```
public class SynchronizedTest {
      //同步方法
     public synchronized void doSth1(){
         System.out.println("Hello World");
     }
     //同步代码块
     public void doSth2(){
         synchronized (SynchronizedTest.class){
             System.out.println("Hello World");
         }
     }
 }
```
#### （2）synchronized的实现原理
我们对上面的代码进行反编译，可以得到如下代码：
```
public synchronized void doSth();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_SYNCHRONIZED
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // String Hello World
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return

  public void doSth1();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=3, args_size=1
         0: ldc           #5                  // class com/hollis/SynchronizedTest
         2: dup
         3: astore_1
         4: monitorenter
         5: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         8: ldc           #3                  // String Hello World
        10: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        13: aload_1
        14: monitorexit
        15: goto          23
        18: astore_2
        19: aload_1
        20: monitorexit
        21: aload_2
        22: athrow
        23: return
```
对于同步方法，JVM采用ACC_SYNCHRONIZED标记符来实现同步。 
对于同步代码块。JVM采用monitorenter、monitorexit两个指令来实现同步。
自Java 6/Java 7开始，Java虚拟机对内部锁的实现进行了一些优化。这些优化主要包括锁消除（Lock Elision）、锁粗化（Lock Coarsening）、偏向锁（Biased Locking）以及适应性自旋锁（Adaptive Locking）。这些优化仅在Java虚拟机server模式下起作用.

### 4、volatile关键字解决了什么问题，实现原理是什么
Java提供了volatile关键字来保证可见性。当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。可以通过volatile关键字来保证一定的“有序性”。volatile不能保证原子性。
#### （1）happens-before原则（先行发生原则）：
	• 程序次序规则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作
	• 锁定规则：一个unLock操作先行发生于后面对同一个锁额lock操作
	• volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作
	• 传递规则：如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C
	• 线程启动规则：Thread对象的start()方法先行发生于此线程的每个一个动作
	• 线程中断规则：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生
	• 线程终结规则：线程中所有的操作都先行发生于线程的终止检测，我们可以通过Thread.join()方法结束、Thread.isAlive()的返回值手段检测到线程已经终止执行
	• 对象终结规则：一个对象的初始化完成先行发生于他的finalize()方法的开始
#### （2）volatile关键字的两层语义
	• 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
	• 禁止进行指令重排序
#### （3）volatile的原理和实现机制
观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：

	• 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；
	• 它会强制将对缓存的修改操作立即写入主存；
	• 如果是写操作，它会导致其他CPU中对应的缓存行无效。

### 5、Java多线程的5大状态，以及状态图流转过程

![thread](../img/thread.png "thread")

#### （1）新建状态(New): 
线程对象被创建后，就进入了新建状态。例如，Thread thread = new Thread()。
#### （2）就绪状态(Runnable): 
也被称为“可执行状态”。线程对象被创建后，其它线程调用了该对象的start()方法，从而来启动该线程。例如，thread.start()。处于就绪状态的线程，随时可能被CPU调度执行。
#### （3）运行状态(Running) : 
线程获取CPU权限进行执行。需要注意的是，线程只能从就绪状态进入到运行状态。
#### （4）阻塞状态(Blocked) : 
阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。阻塞的情况分三种：

	• 等待阻塞 -- 通过调用线程的wait()方法，让线程等待某工作的完成。
	• 同步阻塞 -- 线程在获取synchronized同步锁失败(因为锁被其它线程所占用)，它会进入同步阻塞状态。
	• 其他阻塞 -- 通过调用线程的sleep()或join()或发出了I/O请求时，线程会进入到阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。

#### （5）死亡状态(Dead): 
线程执行完了或者因异常退出了run()方法，该线程结束生命周期。

### 5、线程同步和通信方式
线程间的同步方式有四种
#### （1）临界区 
#### （2）互斥量
#### （3）信号量 
#### （4）事件
线程间的通信方式有三种
#### （1）使用全局变量
#### （2）使用消息实现通信
#### （3）使用事件CEvent类实现线程间通信

### 6、线程创建的方式
#### （1）继承Thread类
~~~
class Thread1Test extends Thread{
        @Override
        public void run() {

        }
 }
~~~
#### （2）实现Runnable接口
~~~
class RunnableTest implements Runnable{
        @Override
        public void run() {

        }
 }
~~~
#### （3）实现Callable接口
~~~
class CallableTest implements Callable<Integer>{
        @Override
        public Integer call() throws Exception {
            return null;
        }
}
~~~

### 7、线程

### 8、线程池
线程池的构造参数如下：

~~~
public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.acc = System.getSecurityManager() == null ?
                null :
                AccessController.getContext();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
~~~

线程池的实现原理如下：

![thread-pool](../img/thread-pool.png "thread-poll")

_图片来自：https://youzhixueyuan.com/_
#### （1）先判断线程池中的核心线程们是否空闲，如果空闲，就把这个新的任务指派给某一个空闲线程去执行。如果没有空闲，并且当前线程池中的核心线程数还小于 corePoolSize，那就再创建一个核心线程。
#### （2）如果线程池的线程数已经达到核心线程数，并且这些线程都繁忙，就把这个新来的任务放到等待队列中去。如果等待队列又满了，那么查看一下当前线程数是否到达maximumPoolSize，如果还未到达，就继续创建线程。
#### （3）如果已经到达了，就交给RejectedExecutionHandler(拒绝策略)来决定怎么处理这个任务。
#### （4）构造器中各个参数的含义：

	• corePoolSize（线程池的基本大小）

当提交一个任务到线程池时，线程池会创建一个线程来执行任务，即使其他空闲的基本线程能够执行新任务也会创建线程，等到需要执行的任务数大于线程池基本大小时就不再创建。如果调用了线程池的prestartAllCoreThreads方法，线程池会提前创建并启动所有基本线程。

	• runnableTaskQueue（任务队列）

用于保存等待执行的任务的阻塞队列。可以选择以下几个阻塞队列。
 ArrayBlockingQueue：是一个基于数组结构的有界阻塞队列，此队列按 FIFO（先进先出）原则对元素进行排序。
 LinkedBlockingQueue：一个基于链表结构的阻塞队列，此队列按FIFO （先进先出） 排序元素，吞吐量通常要高于ArrayBlockingQueue。
 SynchronousQueue：一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQueue。
 PriorityBlockingQueue：一个具有优先级得无限阻塞队列。

	• maximumPoolSize（线程池最大大小）

线程池允许创建的最大线程数。如果队列满了，并且已创建的线程数小于最大线程数，则线程池会再创建新的线程执行任务。值得注意的是如果使用了无界的任务队列这个参数就没什么效果。

	• ThreadFactory：用于设置创建线程的工厂

可以通过线程工厂给每个创建出来的线程设置更有意义的名字，Debug和定位问题时非常又帮助。

	• RejectedExecutionHandler（饱和策略）

当队列和线程池都满了，说明线程池处于饱和状态，那么必须采取一种策略处理提交的新任务。这个策略默认情况下是AbortPolicy，表示无法处理新任务时抛出异常。以下是JDK1.5提供的四种策略。
AbortPolicy：直接抛出异常。
CallerRunsPolicy：只用调用者所在线程来运行任务。
DiscardOldestPolicy：丢弃队列里最近的一个任务，并执行当前任务。
DiscardPolicy：不处理，丢弃掉。
当然也可以根据应用场景需要来实现RejectedExecutionHandler接口自定义策略。如记录日志或持久化不能处理的任务。

#### （5）Executors类
Executors类，提供了一系列工厂方法用于创建线程池，返回的线程池都实现了ExecutorService接口。
Executors提供四种线程池，newFixedThreadPool、newCachedThreadPool、newSingleThreadExecutor、newScheduledThreadPool。
a.newFixedThreadPool：创建一个可重用固定线程数的线程池，以共享的无界队列方式来运行这些线程。
~~~
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
}
~~~
b.newCachedThreadPool：创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程
~~~
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
}
~~~
c.newSingleThreadExecutor：创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行
~~~
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
}
~~~
d.newScheduledThreadPool：创建一个定长线程池，支持定时及周期性任务执行
~~~
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
        return new ScheduledThreadPoolExecutor(corePoolSize);
}
~~~
#### （6）重入锁ReentrantLock

~~~
public class ReentrantLockTest {
    public static ReentrantLock lock = new ReentrantLock();
    public static int i = 0;
    private static Runnable runnable = () -> IntStream.range(0, 10000).forEach((j)->{
        lock.lock();
        try {
            i++;
        } finally {
            lock.unlock();
        }
    });

    @Test
    public void reentrantLockTest() throws Exception{
        Thread thread1 = new Thread(runnable);
        Thread thread2 = new Thread(runnable);
        thread1.start();
        thread2.start();
        thread1.join();
        thread2.join();
        System.out.println(i);
    }
}
~~~

#### （7）重入锁的搭档Condition
#### （8）信号量Semaphore
#### （9）读写锁ReadWriteLock
#### （10）倒计数器CountDownLatch
#### （11）循环栅栏CyclicBarrier
#### （12）线程阻塞工具LockSupport

## 三、集合

## 四、IO

### 1、IO的分类
从数据传输方式或者说是运输方式角度看，可以将 IO 类分为字节流和字符流。字节流读取单个字节，字符流读取单个字符（一个字符根据编码的不同，对应的字节也不同，如 UTF-8 编码是 3 个字节，中文编码是 2 个字节。）字节流用来处理二进制文件（图片、MP3、视频文件），字符流用来处理文本文件（可以看做是特殊的二进制文件，使用了某种编码，人可以阅读）。简而言之，字节是个计算机看的，字符才是给人看的。
字节流和字符流的划分可以看下面这张图：

![io](../img/IO.png "io")

`图片来源：https://www.jianshu.com/p/715659e4775f`

下面介绍几个常用的IO类
### 2、常用IO类型 (同步、阻塞)
#### （1）FileInputStream与FileOutputStream
~~~
    @Test
    public void fileIOStreamTest() throws Exception{
        File file = new File(fileName);
        FileInputStream inputStream = new FileInputStream(file);
        byte[] b = new byte[1];
        File file2 = new File(fileName2);
        FileOutputStream outputStream = new FileOutputStream(file2);
        while (inputStream.read(b, 0, 1) != -1){
            outputStream.write(b);
        }
    }
~~~
#### （2）BufferedInputStream与BufferedOutputStream
~~~
    @Test
    public void bufferedIOStreamTest() throws Exception{
        File file1 = new File(fileName);
        InputStream inputStream = new FileInputStream(file1);
        BufferedInputStream bufferedInputStream = new BufferedInputStream(inputStream);
        byte[] b = new byte[2];
        File file2 = new File(fileName2);
        OutputStream outputStream = new FileOutputStream(file2);
        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(outputStream);
        while (bufferedInputStream.read(b) != -1){
            bufferedOutputStream.write(b, 0 , 2);
        }
        bufferedOutputStream.flush();
        bufferedOutputStream.close();
        outputStream.close();
        bufferedInputStream.close();
        inputStream.close();
    }
~~~
#### （3）FileReader和FileWriter
#### （4）InputStreamReader与OutputStreamWriter
#### （5）BufferedReader与BufferedWriter
~~~
    @Test
    public void fileReaderWriterTest() throws Exception{
        String fileName1 = "fileReader1.txt";
        String fileName2 = "fileReader2.txt";
        BufferedReader reader = new BufferedReader(new InputStreamReader(new FileInputStream(fileName1), "UTF-8"));
        char[] c = new char[5];
        BufferedWriter writer = new BufferedWriter (new OutputStreamWriter (new FileOutputStream (fileName2,true),"UTF-8"));
        while (reader.read(c) != -1){
            writer.write(c);
        }
        writer.flush();
        writer.close();
        reader.close();
    }
~~~

### 3、NIO(同步、非阻塞)
NIO之所以是同步，是因为它的accept/read/write方法的内核I/O操作都会阻塞当前线程
NIO的三个主要组成部分：Channel（通道）、Buffer（缓冲区）、Selector（选择器）

#### （1）Channel（通道）
Channel是一个对象，可以通过它读取和写入数据。可以把它看做是IO中的流，不同的是：Channel是双向的，既可以读又可以写，而流是单向的、
Channel可以进行异步的读写、对Channel的读写必须通过buffer对象。Channel主要有如下几种类型：

a.FileChannel：从文件读取数据的

b.DatagramChannel：读写UDP网络协议数据

c.SocketChannel：读写TCP网络协议数据

e.ServerSocketChannel：可以监听TCP连接

#### （2）Buffer
所有的数据都是用Buffer处理的，它是NIO读写数据的中转池。Buffer实质上是一个数组，通常是一个字节数据，但也可以是其他类型的数组。但一个缓冲区不仅仅是一个数组，重要的是它提供了对数据的结构化访问，而且还可以跟踪系统的读写进程。
Buffer 读写数据一般遵循以下四个步骤：

a.写入数据到 Buffer；

b.调用 flip() 方法；

c.从 Buffer 中读取数据；

d.调用 clear() 方法或者 compact() 方法。

有两种方式能清空缓冲区：调用 clear() 或 compact() 方法。clear() 方法会清空整个缓冲区。compact() 方法只会清除已经读过的数据。任何未读的数据都被移到缓冲区的起始处，新写入的数据将放到缓冲区未读数据的后面。
Buffer主要有如下几种：

a.ByteBuffer

b.CharBuffer

c.DoubleBuffer

d.FloatBuffer

e.IntBuffer

f.LongBuffer

g.ShortBuffer

~~~
public static void copyFile(String src, String dst) throws Exception{
        FileInputStream inputStream = new FileInputStream(new File(src));
        FileOutputStream outputStream = new FileOutputStream(new File(dst));
        /获得传输通道channel
        FileChannel inChannel = inputStream.getChannel();
        FileChannel outChannel = outputStream.getChannel();
        //获得容器buffer
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        while (true){
            //判断是否读完文件
            int eof = inChannel.read(byteBuffer);
            if (eof == -1){
                break;
            }
             //重设一下buffer的position=0，limit=position
            byteBuffer.flip();
            //开始写
            outChannel.write(byteBuffer);
            //写完要重置buffer，重设position=0,limit=capacity
            byteBuffer.clear();
        }
        inChannel.close();
        outChannel.close();
        inputStream.close();
        outputStream.close();
}
~~~
#### （3）Selector（选择器对象）
Selector是一个对象，它可以注册到很多个Channel上，监听各个Channel上发生的事件，并且能够根据事件情况决定Channel读写。这样，通过一个线程管理多个Channel，就可以处理大量网络连接了。
a.创建一个Selector
~~~
Selector selector = Selector.open();
~~~
b.注册Channel到Selector
~~~
//注册的Channel 必须设置成异步模式 才可以,否则异步IO就无法工作
channel.configureBlocking(false);
SelectionKey key =channel.register(selector,SelectionKey.OP_READ);
~~~
#### 4、NIO2(异步、非阻塞)
 在JDK1.7中，这部分内容被称作NIO.2，主要在Java.nio.channels包下增加了下面四个异步通道：
 a.AsynchronousSocketChannel
 
 b.AsynchronousServerSocketChannel
 
 c.AsynchronousFileChannel
 
 d.AsynchronousDatagramChannel
 
## 五、Java虚拟机
### 1、虚拟机运行时数据区域
#### （1）程序计数器：
当前线程所执行的字节码的行号指示器。如果是java方法记录字节码指令地址，如果是本地方法，则为空。是线程私有的，没有OutOfMemoryError
#### （2）java虚拟机栈：
每个方法在执行的同时都会创建一个栈帧（Stack Frame)用于存储局部变量表、操作数栈、动态链接、方法出口等信息。是线程私有的。有StackOverflowError和OutOfMemoryError

#### （3）本地方法栈：
本地方法栈则为虚拟机使用到的Native方法服务。本地方法栈区域也会抛出StackOverflowError和OutOfMemoryError异常
#### （4）java堆：
线程共享的。所有的对象实例以及数组都要在堆上分配，但是，并不是绝对的。Java堆中还可以细分为：新生代和老年代；再细致一点的有Eden空间、From Survivor空间、To Survivor空间等。
#### （5）方法区：
线程共享的。用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。也称为永久代
#### （6）运行时常量池：
用于存放编译期生成的各种字面量和符号引用，是方法区的一部分
#### （7）直接内存：
NIO中引入了一种基于通道（Channel）与缓冲区（Buffer）的I/O方式，它可以使用Native函数库直接分配堆外内存，然后通过一个存储在Java堆中的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在Java堆和Native堆中来回复制数据。
### 2、判断对象是否存活
#### （1）引用计数法：给对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加1；当引用失效时，计数器值就减1；任何时刻计数器为0的对象就是不可能再被使用的。
#### （2）可达性分析算法：当一个对象到GC Roots没有任何引用链相连（用图论的话来说，就是从GC Roots到这个对象不可达）时，则证明此对象是不可用的。
#### （3）在Java语言中，可作为GC Roots的对象包括下面几种：

虚拟机栈（栈帧中的本地变量表）中引用的对象。

方法区中类静态属性引用的对象。

方法区中常量引用的对象。

本地方法栈中JNI（即一般说的Native方法）引用的对象

### 3、引用类型
在JDK 1.2之后，Java对引用的概念进行了扩充，将引用分为强引用（Strong Reference）、软引用（Soft Reference）、弱引用（Weak Reference）、虚引用（Phantom Reference）4种
### 4、垃圾收集算法

#### （1）标记-清除算法

#### （2）复制算法

#### （3）标记整理算法
