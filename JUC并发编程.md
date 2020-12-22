JUC并发编程（java.util .concurrent）
java.util 工具包
java默认有两个线程:main gc
java无法开启线程
<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130114221339.png" alt="image-20201130114221339" style="zoom: 67%;" />

![image-20201130114254224](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130114254224.png)

并发：交替执行
并行：同时执行
<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130114927458.png" alt="image-20201130114927458" style="zoom: 67%;" />

并发编程的本质：充分利用CPU的资源
1.线程的状态
NEW,新生
RUNNABLE,运行
BLOCKED,阻塞
WAITING,等待
TIMED_WAITING,超时等待
TERMINATED;终止

2.wait与sleep的区别
（1）来自不同的类：wait ->Object sleep ->Thread
（2）锁的释放： wait 会释放锁 sleep 不会释放锁
（3）使用的范围不一样 ： wait 必须在同步代码块中使用 sleep 可以在任何地方使用
（4）是否需要捕获异常： wait不需要捕获异常 sleep需要捕获异常

3.Lock锁
Synchronized本质：队列，锁，直接在资源上加上
<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130145020703.png" alt="image-20201130145020703" style="zoom:67%;" />

线程是一个单独的资源类，没有任何的附属操作
并发：多个线程操作同一个资源类，把资源类丢入线程

lock锁
<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130145854804.png" alt="image-20201130145854804"  />

![image-20201130150302625](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130150302625.png)

公屏锁，先来后到
非公平锁，可以抢占（默认）
lock使用：new lock -> lock.lock -> try catch -> lock.unlock()

4.Synchronized与lock的区别
（1）Synchronized 内置的java关键字 ，Lock是一个java类
（2）Synchronized 无法判断获锁的状态，Lock可以判断是否获取到锁
（3）Synchronized 会自动释放锁 ，Lock必须要手动释放锁，如果不释放，会产生死锁
（4）Synchronized 线程1（获得锁，阻塞），线程2（一直等待）。Lock,不一定等待下去
（5）Synchronized 可重入锁，不可以中断，非公平。 Lock,可重入锁，可以判断锁，非公平锁（可以自己设置）
（6）Synchronized 适合锁少量的代码同步问题，Lock适合锁大量的同步代码块


![image-20201130161244942](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130161244942.png)

当线程变成四个后，会产生虚假唤醒的情况
解决方法：if 改成 while 
线程唤醒后，会从wait的地方继续执行，使用if的话唤醒之后，不会进行判断(因为wait之前已经判断过了)，使用循环的话唤醒之后会再次判断

6.JUC中的生产者，消费者问题
<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130174052477.png" alt="image-20201130174052477" style="zoom: 67%;" />

![image-20201130174434233](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130174434233.png)

Condition 精准的通知和唤醒功能
Condition因素出Object监视器方法（ wait ， notify和notifyAll ）成不同的对象，以得到具有多个等待集的每个对象，通过将它们与使用任意的组合的效果Lock个实现。==Lock替换synchronized方法和语句的使用==， ==Condition取代了对象监视器方法的使用==。

生产者消费者：判断-> 执行 -> 通知

5.锁是什么
![image-20201203145319477](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201203145319477.png)

![image-20201203145330314](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201203145330314.png)
(1)在标准状态下，send会先执行，call后执行
(2)在send延迟4s后，仍然是send先执行

synchronized锁的==对象是方法的调用者==
两个方法用的是同一个锁，谁先拿到谁执行，main自上而下执行

(3)
(4)![image-20201203151039166](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201203151039166.png)

两个对象两个同步方法，会先执行call,(两个对象便会有两把锁)

(5)增加两个静态的同步方法，

![image-20201203152018626](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201203152018626.png)

当synchronized前有static时，锁的对象变成了类，锁的是Class，(此时，即使==有两个对象，因为类只有一个==，send仍然先执行)

(6)当有一个静态同步锁，一个普通同步锁，一个对象，这两个锁是不同的锁，互不干扰，call先执行

(7))当有一个静态同步锁，一个普通同步锁，两个对象，这两个锁仍然是是不同的锁，互不干扰，call先执行

总结：
锁的种类分两种：一个是锁方法的，一个是锁类的

6.集合类不安全
并发下ArrayList不安全的
1.使用Vector解决，.List<String> list = new Vector();
2.List<String> list = Collections.synchronizedList(new ArrayList<>());
3.List<String> list = new CopyOnWriteArrayList<>();(CopyOnWriteArrayList 写入时复制，COW,计算机设计领域中的优化策略)
当有多个线程调用时，list,读取时是固定的，写入时，会产生一个复制品，写入后再给回去

Set
解决办法：
1.Set<String> set = Collections.synchronizedSet(new HashSet<>()); //使用Collections集合

HashMap


























