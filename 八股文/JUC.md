1. Java中实现多线程有几种方法<p>
（1）ExecutorService线程池
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// 创建一个固定大小的线程池
ExecutorService executor = Executors.newFixedThreadPool(2);

executor.submit(new MyRunnable());
executor.submit(new MyCallable());

// 关闭线程池
executor.shutdown();
```

（2）Thread.start() + for循环
```java
class MyThread extends Thread {
public void run() {
// 执行任务
}
}

// 使用
MyThread t = new MyThread();
t.start();
```
(4)实现Runnable接口
```java
class MyRunnable implements Runnable {
    public void run() {
        // 执行任务
    }
}

// 使用
Thread t = new Thread(new MyRunnable());
t.start();
```
(5)实现Callable接口
```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

class MyCallable implements Callable<Integer> {
    public Integer call() throws Exception {
        // 执行任务并返回结果
        return 123;
    }
}

// 使用
FutureTask<Integer> task = new FutureTask<>(new MyCallable());
Thread t = new Thread(task);
t.start();

// 获取结果
Integer result = task.get();
```
2. 4种线程池的区别<br>
 (1) newFixedThreadPool<br>
 (2) 单线程化线程池（SingleThreadExecutor）<br>
 (3) 可缓存线程池（CachedThreadPool）<br>
    * 特点：<br>
   1. 线程数不固定，可以根据需要创建新线程。<br>
   2. 在先前构造的线程可用时将重用它们。<br>
   3. 若线程闲置超过60秒，则会被终止并移除线程池。<br>
 (4) 定时及周期性任务线程池（ScheduledThreadPool）<br>
    * 特点：<br>
   1. 用于延迟执行任务或定期执行任务。
   2. 核心线程数固定，非核心线程数（如果启用）没有限制。
   3. 任务可以周期性或单次执行。
（5）newWorkStealingPool<br>
3. 如何停止一个正在运行的线程<br>
（1）future.get()方法，停止主线程<br>
（2）thread.interrupt()方法，停止子线程<br>
 (3) thread.wait()方法，停止子线程<br>
 (4) thread.join()方法，停止主线程<br>
 (5) thread.stop()方法，停止子线程 `不建议使用`<br>
 (6) thread.suspend()方法，停止子线程 `不建议使用`<br>
4. notify()和notifyAll()的区别<br>
（1）notify()方法只能唤醒一个线程，notifyAll()方法可以唤醒所有等待的线程<br>
（2）notify()方法是将等待队列中的一个线程移动到同步队列中，notifyAll()方法是将等待队列中的所有线程移动到同步队列中<br>
（3）notify()方法是随机唤醒一个线程，notifyAll()方法是唤醒所有线程，但是只有一个线程能够获取到锁<br>
5. wait()和sleep()的区别<br>
（1）wait()方法是Object类的方法，sleep()方法是Thread类的方法<br>
（2）wait()方法会释放锁，sleep()方法不会释放锁<br>
（3）wait()方法需要被唤醒，sleep()方法不需要<br>
（4）wait()方法只能在同步方法或同步代码块中使用，sleep()方法可以在任何地方使用<br>
6. Thread类中的start()和run()方法的区别<br>
（1）start()方法用于启动线程，run()方法用于执行线程的任务<br>
（2）start()方法是Thread类的方法，run()方法是Runnable接口的方法<br>
（3）start()方法会创建一个新的线程，run()方法不会创建新的线程<br>
7. java中interrupted()和isInterrupted()方法的区别<br>
 (1) interrupted()方法是静态方法，isInterrupted()方法是实例方法<br>
 (2) interrupted()方法会清除中断标志，isInterrupted()方法不会清除中断标志<br> 
8. Java中的synchronized和RetentrantLock的区别<br>
 (1) synchronized是Java内置关键字，ReentrantLock是Java类<br>
 (2) synchronized无法判断是否获取到锁，ReentrantLock可以判断是否获取到锁<br>
 (3) synchronized会自动释放锁，ReentrantLock需要手动释放锁<br>
 (4) synchronized是非公平锁，ReentrantLock可以是公平锁也可以是非公平锁<br>
 (5) synchronized适合锁少量的代码同步问题，ReentrantLock适合锁大量的同步代码<br>
9. volatile是什么，可以保证有序性吗
 (1) volatile是Java关键字，用于修饰变量，保证变量的可见性<br>
 (2) volatile不能保证有序性<br>
10. SynchronizedMap和ConcurrentHashMap的区别<br>
 (1) SynchronizedMap是线程安全的，ConcurrentHashMap是线程安全的<br>
 (2) SynchronizedMap是对整个Map进行加锁，ConcurrentHashMap是对Map的部分进行加锁<br>
 (3) SynchronizedMap是悲观锁，ConcurrentHashMap是乐观锁<br>
 (4) SynchronizedMap是阻塞式的，ConcurrentHashMap是非阻塞式的<br>
11. Thread的yeild()方法是什么<br>
 (1) yeild()方法是Thread类的方法，用于让出CPU<br>
 (2) yeild()方法会让出CPU，但是不会释放锁<br>
 (3) yeild()方法只会让出CPU，但是不会释放锁<br>
12. Java线程池中submit()和execute()方法的区别<br>
 (1) submit()方法可以提交Callable和Runnable任务，execute()方法只能提交Runnable任务<br>
 (2) submit()方法可以获取任务的返回值，execute()方法不能获取任务的返回值<br>
 (3) submit()方法可以抛出异常，execute()方法不能抛出异常<br>
13. 什么是线程安全，Vector是线程安全的吗<br>
 (1) 线程安全是指多线程访问同一个资源时，不会出现不确定的结果<br>
 (2) Vector是线程安全的<br>
14. 简述一下你对线程池的理解
    (1) 线程池是一种多线程处理形式，它包含了一组线程，可以有效地控制线程的创建、运行和销毁<br>
    (2) 线程池的好处：<br>
        1. 降低资源消耗<br>
        2. 提高响应速度<br>
        3. 提高线程的可管理性<br>
15. 线程的生命周期
    (1) 新建状态（New）：线程对象被创建后，就进入了新建状态，此时它已经具备了运行的条件，但还没有开始运行<br>
    (2) 就绪状态（Runnable）：当调用线程对象的start()方法（t.start()）后，线程就进入了就绪状态。处于就绪状态的线程，只是说明此线程已经做好了准备，随时等待CPU调度执行，并不是说执行了t.start()此线程立即就会执行<br>
    (3) 运行状态（Running）：当CPU开始调度处于就绪状态的线程时，此时线程才得以真正执行，即进入到运行状态。注：就绪状态是进入到运行状态的唯一入口，也就是说，线程要想进入运行状态执行，首先必须处于就绪状态中<br>
    (4) 阻塞状态（Blocked）：处于运行状态中的线程由于某种原因，暂时放弃对CPU的使用权，停止执行，此时进入阻塞状态，直到其进入到就绪状态，才有机会再次被CPU调用以进入到运行状态。根据阻塞产生的原因不同，阻塞状态又可以分为三种：<br>
        1. 等待阻塞：运行状态中的线程执行wait()方法，使本线程进入到等待阻塞状态<br>
        2. 同步阻塞：线程在获取synchronized同步锁失败（因为锁被其它线程所占用），它会进入同步阻塞状态<br>
        3. 其他阻塞：通过调用线程的sleep()或join()或发出了I/O请求时，线程会进入到阻塞状态

