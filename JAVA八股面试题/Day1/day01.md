## 此文件为JAVA实习面试所用八股

该文件为bilibili地址：https://www.bilibili.com/video/BV1Ag4y1A7bg?buvid=XX6EF3B6E4F7132F9EB7C940551F4DB87DE12&from_spmid=playlist.playlist-detail.0.0&is_story_h5=false&mid=psB79T810OYGeAyXAiTpIQ%3D%3D&plat_id=116&share_from=ugc&share_medium=android&share_plat=android&share_session_id=0455e81e-f406-433b-baf7-b338fcefa7d6&share_source=WEIXIN&share_tag=s_i&spmid=main.ugc-video-detail.0.0&timestamp=1727512080&unique_k=hn1LRou&up_id=615412417

---
#### 1、JAVA中有哪几种方式创建线程执行任务

(1)继承Thread类

``` JAVA
public class Thread1 extends Thread {

  public static void main(String[] args){
    Thread1 thread = new Thread1();
    thread.start();
  }

  @Override
  public void run(){
    System.out.println("hello")
  }
}
```

**1/重写的是run()方法，而不是start方法，但是占用了继承的名额，JAVA中的类是单继承**

**2/但是接口可以多继承**

(2)实现Runnable接口

``` JAVA
public class Thread1 implements Runnable {
  public class void main(String[] args) {
    Thread  thread = new Thread(new Thread1()) ;
    thread.start;
  }
  public void run(){
    System.out.Println("hello");
  }
}

```
**1、实现了Runnable接口，实现run()方法，使用依然用到Thread，这种方法更加常用**

也可以采用匿名内部类的方式

``` JAVA
public class Thread1 {
  public class void main(String[] args) {
    Thread thread = new Thread(new Runnable(){
      public void run(){
        System.out.Println("1");
      }
    });
    thread.start();
  }
}
```
**lambda表达式**
```JAVA
public class Thread1 {
  public static void main(String[] args) {
    Thread thread = new Thread() -> System.out.Println("hello");
    thread.run();
  }
}
```
(3)实现Callable接口

```Java
import java.util.concurrent.*;

public class Thread1 implements Callable<String> {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建 FutureTask 对象，将 Callable 任务传入
        FutureTask<String> futureTask = new FutureTask<>(new Thread1());
        // 创建线程并将 FutureTask 对象作为任务传入
        Thread thread = new Thread(futureTask);
        // 启动线程
        thread.start();

        try {
            // 获取 Callable 任务的执行结果
            String result = futureTask.get();
            // 输出获取到的结果
            System.out.println(result);
        } catch (ExecutionException | InterruptedException e) {
            // 捕获并打印异常信息
            e.printStackTrace();
        }
    }

    @Override
    public String call() {
        // 定义 Callable 任务的执行逻辑，返回字符串 "hello"
        return "hello";
    }
}
```
**总结：实现Callable接口，实现call()方法，得使用Thread+FullTask配合，这种方法支持拿到异步执行任务的结果**

(4)利用线程池创建线程

**由ExcutorService来创建线程**
```Java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Thread1 implements Runnable {
    public static void main(String[] args) throws InterruptedException {
        // 创建一个固定大小为 10 的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        // 向线程池提交一个任务
        executorService.execute(new Thread1());
        // 关闭线程池，不再接受新任务，但会处理完已提交的任务
        executorService.shutdown();
    }

    @Override
    public void run() {
        // 线程执行的具体任务，这里只是简单打印 "hello"
        System.out.println("hello");
    }
}
```
**总结：所以创建线程执行任务都是Runnable**
---

### 2、为什么不建议采用Executors来创建线程池

1、资源耗尽风险：newCachedThreadPool 运行创建大量线程，newScheduledThreadPool和newFixedThreadPool队列无界，可能堆积大量请求，导致OOM内存溢出
``` JAVA
//当我们采用Executors创建FixThreadPool时，对应的构造方法：
public static ExecutorService newFixedThreadPool(int nThreads) {
  return new ThreadPoolExecutor(nThreads,nThreads,0L,TimeUnit.MILLISECONDS,new LinkedBlockingQueue<Runnable>());
}
```

发现创建的队列为LinkedBlockingQueue,是一个无界阻塞队列，如果采用该线程池执行任务，如果任务过多就会不断加入到队列中，任务越多占用的内存越多，最终耗尽内存，导致OOM

2、不利于排查问题

```Java
//当我们使用Executors船舰SingleThreadExecutor时，对应的构造方法为：
public static ExecutorService newSingleThreadExecutor() {
  return new FinalizableDelegatedExecutorService(new ThreadPoolExecutor(1,1,10L,TimeUnit.MILLISECONDS,new LinkBlockingQueue<Runnable>()));
}

```
也是LinkBlockingQueue，所以同样也会耗尽内存

**总结：除开可能造成OOM之外，我们采用Executors来创建线程不能自定义线程名字，不利于排查问题，所以建议直接采用ThreadPoolExecutors来创建线程池，可以灵活控制**

```java
import java.util.concurrent.*;

public class CustomThreadPoolDemo {
    public static void main(String[] args) {
        // 创建线程池
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2, // 核心线程数
                5, // 最大线程数
                60L, TimeUnit.SECONDS, // 线程空闲存活时间
                new ArrayBlockingQueue<>(10), // 任务队列
                Executors.defaultThreadFactory(), // 线程工厂
                new ThreadPoolExecutor.CallerRunsPolicy() // 拒绝策略
        );

        // 提交任务
        for (int i = 0; i < 15; i++) {
            final int taskId = i;
            executor.execute(() -> {
                System.out.println("Task " + taskId + " is running on " + Thread.currentThread().getName());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }

        // 关闭线程池
        executor.shutdown();
    }
}
```
---
### 3、线程池有哪几种状态？每种状态分别代表什么？

1、RUNNING

表示线程池正常运行，可以接受新任务也可正常处理队列中的任务

2、SHUTDOWM

当调用线程池的shutdown()方法，线程池就会进入shutdown状态，表示当前线程池处于关闭状态，此状态下线程池不会接受新任务，但是会把剩下的任务处理完成

3、STOP
进入STOP状态，表示处于停滞，不会接受新的任务，也不会处理剩下的任务，并且正在运行的任务也会被中断

4、TIDYING

当线程池中没有线程在运行后，线程池就会自动进入TIDYING，并且会调用terminated(),该方法是空方法，留给程序员进行拓展

5、TERMINATED

terminated()方法执行完成后，线程池就会进入terminated，既是生命周期结束，资源全部释放

---
### 4、Sychronized和ReentrantLock有哪些不同
Synchronized和ReentrantLock不同点如下：
- **语法层面**：Synchronized是关键字，自动释放锁；ReentrantLock是类，需手动加解锁。
- **功能特性**：ReentrantLock更丰富，有公平锁、可中断锁、超时锁等；Synchronized无这些特性。
- **性能表现**：低竞争时Synchronized简单高效；高竞争下ReentrantLock通过高级特性优化性能。 

---
 简单同步场景
- **Synchronized**：在电商库存操作等简单同步场景中，用它无需额外配置，能快速实现线程同步，代码简洁，自动加解锁，开发效率高。
- **ReentrantLock**：实现同样功能时，要手动加解锁，代码复杂些，但便于后续扩展功能。

 公平锁场景
- **Synchronized**：不支持公平锁，无法保证先请求锁的线程先获得锁。
- **ReentrantLock**：可创建公平锁，适用于银行排队系统等需按请求顺序处理的业务。

 可中断锁场景
- **Synchronized**：线程获取锁时若锁被占用只能阻塞等待，无法主动中断。
- **ReentrantLock**：可设置超时时间，线程等待超时后能放弃等待，适用于文件处理系统等避免长时间等待的场景。 

---

### 5、ThreadLocal有哪些运用场景和底层如何实现

（1）、ThreadLocal是JAVA中所提供的线程本地存储机制，可以利用该机制将数据缓存在某个线程内部，该线程ke'y在任意时刻、任意方法中获取缓存的数据
（2）、ThreadLocal底层是通过ThreaLocalMap来实现的，每个Thread对象（注意不是ThreadLocal对象）中都存在一个ThreadLocalMap，Map的key为ThreadLocal对象，Map的value为需要缓存 的值
（3）、如果在线程池中使用ThreadLocal会造成内存泄漏，因为当ThreadLocal对象用完之后，应该把要设置的key，value，也就是Entry对象进行回收，但线程池中的线程不会回收，而线程对象是通过强引用指向ThreadLocalMap，ThreadLocalMap也是通过强引用指向Entry对象，线程不被回收，Entry对象也就不会被回收，从而出现内存泄漏，解决办法是，在使用ThreadLocal对象之后，手动调用ThreadLocal的dremove方法，手动清除Entry对象

示例代码：
```JAVA
public class ThreadLocalExample {
    // 创建 ThreadLocal 对象
    private static ThreadLocal<String> threadLocal = new ThreadLocal<>();

    public static void main(String[] args) {
        // 创建线程 1
        Thread thread1 = new Thread(() -> {
            // 为线程 1 的 ThreadLocal 变量副本设置值
            threadLocal.set("Value from Thread 1");
            // 获取线程 1 的 ThreadLocal 变量副本的值
            System.out.println(Thread.currentThread().getName() + ": " + threadLocal.get());
            // 移除线程 1 的 ThreadLocal 变量副本的值
            threadLocal.remove();
        }, "Thread 1");

        // 创建线程 2
        Thread thread2 = new Thread(() -> {
            // 为线程 2 的 ThreadLocal 变量副本设置值
            threadLocal.set("Value from Thread 2");
            // 获取线程 2 的 ThreadLocal 变量副本的值
            System.out.println(Thread.currentThread().getName() + ": " + threadLocal.get());
            // 移除线程 2 的 ThreadLocal 变量副本的值
            threadLocal.remove();
        }, "Thread 2");

        // 启动线程 1
        thread1.start();
        // 启动线程 2
        thread2.start();
    }
}

```
（4）、ThreadLocal经典的应用场景就是连接管理（一个线程有一个连接，该连接对象可以在不同的方法之间进行传递，线程之间不共享同一个连接）

---
### 6、ReentrantLock分为公平锁和非公平锁，底层如何实现

`ReentrantLock` 公平锁和非公平锁底层基于 `AbstractQueuedSynchronizer`（AQS）实现：
- **公平锁**：线程请求锁时，会先检查 AQS 的同步队列，若队列中有等待线程，新线程会进入队列尾部等待，按先来先得原则获取锁。
- **非公平锁**：线程请求锁时，不管队列中是否有等待线程，会先尝试直接获取锁，获取失败才进入队列等待。 

---
### 7、Sychronized的锁升级过程是怎样的

```markdown
在 Java 6 之后，`Synchronized` 锁进行了优化，引入了锁升级机制，其升级过程如下：

### 无锁状态
对象刚被创建时，未被任何线程访问，处于无锁状态。对象头中的 Mark Word 存储对象的哈希码、分代年龄等信息。

### 偏向锁
- **原理**：当一个线程首次访问同步块并获取锁时，会在对象头的 Mark Word 中记录该线程的 ID，这个过程称为偏向锁的“偏向”。此后该线程再次进入这个同步块时，无需进行任何同步操作，直接获取锁，这样可以减少不必要的锁竞争开销。
- **升级条件**：当有其他线程尝试竞争该偏向锁时，偏向锁会升级为轻量级锁。

### 轻量级锁
- **原理**：线程在进入同步块时，会在栈帧中创建一个锁记录（Lock Record），并尝试通过 CAS（Compare - And - Swap）操作将对象头的 Mark Word 复制到锁记录中，同时将对象头的 Mark Word 指向锁记录的地址。如果 CAS 操作成功，线程就获得了轻量级锁；如果失败，说明有其他线程正在持有锁，该线程会进行自旋等待锁的释放。
- **升级条件**：如果自旋次数达到一定阈值（默认 10 次）或有多个线程同时竞争该锁，轻量级锁会升级为重量级锁。

### 重量级锁
- **原理**：重量级锁依赖于操作系统的互斥量（Mutex）实现，当线程竞争重量级锁失败时，会被挂起进入阻塞状态，等待锁释放后被唤醒。这种方式会涉及到用户态和内核态的切换，性能开销较大。
- **状态特点**：一旦锁升级为重量级锁，就不会再降级为轻量级锁或偏向锁。 

```
---

### 8、Tomcat中为什么要使用自定义类加载器

* 一个Tomcat中可以部署多个应用。而每个应用中都存在很多类，并且各个应用中的类是独立的，全类名是可以相同的，
* 比如一个订单系统中可能存在com.zhouyu.User类，一个库存系统中可能也存在com.zhouyu.User类，一个Tomcat，不管内部部署了多少应用，Tomcat启动之后就是一个Java进程，也就是一个JM，
* 所以如果Tomcat中只存在一个类加载器，比如默认的AppClassLoader，那么就只能加载一个com.zhouyu.User类，这是有问题的，
* 而在Tomcat中，会为部署的每个应用都生成一个类加载器实例，名字叫做WebAppClassLoader，这样Tomcat中每个应用就可以使用自己的类加载器去加载自己的类，从而达到应用之间的类隔离，不出现冲突。
* 另外Tomcat还利用自定义加载器实现了热加载功能。