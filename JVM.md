JVM:====
1.JVM位置：
JVM可以理解为一个运行在操作系统之上的软件。使得java程序得以运行。
所谓JVM调优，就是在堆里面调

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201116160747021.png" alt="image-20201116160747021" style="zoom: 50%;" />

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201116163217492.png" alt="image-20201116163217492" style="zoom: 50%;" />

.java内存： （1）栈（Stack）存放的是方法中的局部变量，方法的运行一定要在栈中；
							(引用地址 通过地址 去堆里找数据)
                             局部变量： 方法的参数，或者是方法{}内部变量
                             作用域：一旦超出作用域，立刻从栈中消失
                      （2）堆（Heap）：凡是new出来的东西都在堆中。
                      （3）方法区（Method Area）：存放的class相关信息，包含方法信息。（成员方法的地址在方法区内？）
                      （4）本地方法区（Native Method Stack）与操作系统相关。
                      （5）与CPU相关
3.类加载器：
作用：加载Class文件，==由类加载器负责根据一个类的全限定名来读取此类的二进制字节流到JVM内部，并存储再运行时内存区的方法区，然后将其转换为一个与目标类型对应的java.lang.class对象实例==
分类：

1. 虚拟机自带的加载器
2.启动类(根)加载器(bootstrap classloader)
3.扩展类加载器(extension classloader)
4.应用程序加载器（向上为父类)(application classloader)

双亲委派机制:　双亲委派机制是指当一个类加载器收到一个类加载请求时，该类加载器首先会把请求委派给父类加载器。每个类加载器都是如此，只有在父类加载器在自己的搜索范围内找不到指定类时，子类加载器才会尝试自己去加载。

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201116220925560.png" alt="image-20201116220925560" style="zoom:50%;" />

#### 双亲委派模型工作工程：

　　1.当Application ClassLoader 收到一个类加载请求时，他首先不会自己去尝试加载这个类，而是将这个请求委派给父类加载器Extension ClassLoader去完成。 

　　2.当Extension ClassLoader收到一个类加载请求时，他首先也不会自己去尝试加载这个类，而是将请求委派给父类加载器Bootstrap ClassLoader去完成。  

　　3.如果Bootstrap ClassLoader加载失败(在<JAVA_HOME>\lib中未找到所需类)，就会让Extension ClassLoader尝试加载。 

　　4.如果Extension ClassLoader也加载失败，就会使用Application ClassLoader加载。 

　　5.如果Application ClassLoader也加载失败，就会使用自定义加载器去尝试加载。 

　　6.如果均加载失败，就会抛出ClassNotFoundException异常。

4.沙箱安全机制：
我们都知道，程序员编写一个Java程序，默认的情况下可以访问该机器的任意资源，比如读取，删除一些文件或者网络操作等。当你把程序部署到正式的服务器上，系统管理员要为服务器的安全承担责任，那么他可能不敢确定你的程序会不会访问不该访问的资源，为了消除潜在的安全隐患，他可能有两种办法：
（1）让你的程序在一个限定权限的帐号下运行.
（2）利用Java的沙箱机制来限定你的程序不能为非作歹。
以下用于介绍该机制：
Java安全模型的核心就是Java沙箱（sandbox），什么是沙箱？沙箱是一个==限制程序运行的环境==。沙箱机制就是将 Java 代码限定在虚拟机(JVM)特定的运行范围中，并且严格限制代码对本地系统资源访问，通过这样的措施来保证对代码的有效隔离，防止对本地系统造成破坏。沙箱**主要限制系统资源访问**，那系统资源包括什么？——`CPU、内存、文件系统、网络`。不同级别的沙箱对这些资源访问的限制也可以不一样。

**Native**:凡是带了native关键字的，说明Java的作用范围已经达不到了，回去调用底层c语言的库。会进入本地方法栈，之后会调用本地方法接口。JNI的作用，扩展Java的使用，融合不同的语言。
它在内存区域专门开辟了一块标记区域：Native Method Stack,登记native方法，在最终执行的时候，加载本地方法库中的方法通过JNI

**方法区**：所有定义的方法的信息都保存在该区域中，此区域属于共享区间：
静态变量(例如：private int a, private String name)、常量、类信息(构造方法、接口定义)，运行时的常量池在方法区中，但是==实例变量存在堆内存中==，和方法区无关。
static final Class 常量池

**栈**：主管程序的运行，生命周期和线程同步。线程结束，栈内存就会释放。
存放的是方法中的局部变量，方法的运行一定要在栈中( 局部变量： 方法的参数，或者是方法{}内部变量),和变量的引用
栈帧：栈帧就是存储在用户栈上的（当然内核栈同样适用）每一次函数调用涉及的相关信息的记录单元。

类的创建过程：

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201118200150963.png" alt="image-20201118200150963" style="zoom: 80%;" />

类在内存中的加载过程：
第一步，JVM去==方法区寻找Test类的代码信息==，如果有直接调用，没有的话使用类的加载机制把==类==加载进来。同时把静态变量、静态方法、常量加载进来。这里加载的是（“冯冬冬的IT技术栈”，“冯XX”）；这是因为字符串是常量，age中的18是基本类型。
第二步，jvm进入main方法，看到Person person=new Person()。首先分析Person这个类，同样的寻找Person类的代码信息，有就加载，没有的话类加载机制加载进来。同时也加载静态变量、静态方法、常量（“我正在走路。。。”）
第三步，jvm接下来看到了person，person在main方法内部，因而是局部变量，存放在栈空间中。
第四步，jvm接下来看到了new Person()。new出的对象（实例），存放在堆空间中。
第五步，jvm接下来看到了“=”，把new Person的地址告诉person变量，person通过四字节的地址（十六进制），引用该实例。
![image-20201118201517356](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201118201517356.png)

第六步，jvm看到person.name = “冯冬冬的IT技术栈”;person通过引用new Person实例的name属性，该name属性通过地址指向常量池的"冯冬冬的IT技术栈"。(这里是初始化的信息)
第七步，jvm看到person.age = 18; person的age属性是基本数据类型，直接赋值。（引用数据类型需要开辟内存地址空间进行存储）
第八步，jvm看到person.walk(); 调用实例的方法时，并不会在实例对象中生成一个新的方法，而是通过地址指向方法区中类信息的方法。
![image-20201118201706291](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201118201706291.png)

































