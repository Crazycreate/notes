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
静态变量(例如：private int a, private String name)、常量、类信息(构造方法、接口定义)，运行时的常量池在元空间中，但是==实例变量存在堆内存中==，和方法区无关。
static final Class 常量池

**栈**：主管程序的运行，生命周期和线程同步。线程结束，栈内存就会释放。
存放的是方法中的局部变量，方法的运行一定要在栈中( 局部变量： 方法的参数，或者是方法{}内部变量),和变量的引用
栈帧：栈帧就是存储在用户栈上的（当然内核栈同样适用）每一次函数调用涉及的相关信息的记录单元。

类的创建过程：![image-20201119195651649](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201119195651649.png)

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

三种JVM:
sun公司 Hotspot(主要学习)
BEA JRockit
IBM J9VM

**堆**：一个JVM只有一个堆内存，堆内存的大小可以调节。所有线程共享。存放new出来的数组和对象数据，以及类的静态变量。同时，包含一个常量池（final），是由1.7以前版本的方法区转移过来的。
元空间这个东西，是在JDK8以后才存在的，JDK7及以前，只有永久代这个区域，元空间的存储位置是在计算机的内存当中，而永久代的存储位置是在JVM的堆（Heap）中。![image-20201119195258439](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201119195258439.png)

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201119202117640.png" alt="image-20201119202117640" style="zoom:50%;"/>

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201119202206839.png" alt="image-20201119202206839" style="zoom:50%;" />

运行时==常量池和静态变量都存储到了堆==中，MetaSpace存储类的元数据，MetaSpace直接申请在本地内存中（Native memory）,这样类的元数据分配只受本地内存大小的限制,OOM问题就不存在了。
jdk1.8：仍然保留方法区的概念，只不过实现方式不同。取消永久代，方法存放于元空间(Metaspace)，*"元空间仍然与堆不相连，但与堆共享物理内存，逻辑上可认为在堆中"*。
内存快照分析工具，MAT，Jprofiler
MAT,Jprofile作用：
*分析Dump内存文件，快速定位内存泄漏
*获得堆的数据
*获得大的对象

GC
![image-20201121165738117](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201121165738117.png)

复制算法：将内存按容量划分为两块，每次只使用其中一块。当这一块内存用完了，就将存活的对象复制到另一块上，然后再把已使用的内存空间一次清理掉。这样使得每次都是对半个内存区回收，也不用==考虑内存碎片问题==，简单高效。缺点==需要两倍的内存空间==。
复制算法最佳使用场景：对象存活度较低的时候：新生区

标记清除：GC分为两个阶段，标记和清除。首先标记所有可回收的对象，在标记完成后统一回收所有被标记的对象。同时==会产生不连续的内存碎片==。碎片过多会导致以后程序运行时需要分配较大对象时，无法找到足够的连续内存，而不得已再次触发GC。

标记整理：分为两个阶段，首先标记可回收的对象，再将存活的对象都向一端移动，然后清理掉边界以外的内存。此方法避免标记-清除算法的碎片问题，同时也避免了复制算法的空间问题。
一般年轻代中执行GC后，会有少量的对象存活，就会选用复制算法，只要付出少量的存活对象复制成本就可以完成收集。而老年代中因为对象存活率高，没有额外过多内存空间分配，就需要使用标记-清理或者标记-整理算法来进行回收。

JVM调优：修改这些参数![image-20201121172334635](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201121172334635.png)

JMM: Java Memory Model
作用：缓存一致性协议，用于定义数据读写的规则

#### **类加载子系统的工作流程**：

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123095457199.png" alt="image-20201123095457199" style="zoom:50%;" />

1.ClassLoader只负责class文件的加载，至于它是否可以运行，则由Execution Engine决定
2.如果调用构造器实例化对象，则其实例存放在堆区
![image-20201123095754960](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123095754960.png)

**加载模块**

1.通过一个类的全限定明获取定义此类的二进制字节流；

2.将这个字节流所代表的的静态存储结构转化为方法区的运行时数据；

3.在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口

**链接模块分为三块，即验证、准备、解析**

**验证**

1.目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全。

2.主要包括四种验证，文件格式验证，源数据验证，字节码验证，符号引用验证。

**准备**

1.为类变量分配内存并且设置该类变量的默认初始值，即零值；

2.这里不包含用final修饰的sttic，因为final在编译的时候就会分配了，准备阶段会显式初始化；

3.之类不会为实例变量分配初始化，类变量会分配在方法去中，而实例变量是会随着对象一起分配到java堆中。

**解析**

1.将常量池内的符号引用转换为直接引用的过程。

2.事实上，解析操作网晚会伴随着jvm在执行完初始化之后再执行

3.符号引用就是一组符号来描述所引用的目标。符号应用的字面量形式明确定义在《java虚拟机规范》的class文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄

4.解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的CONSTANT_Class_info/CONSTANT_Fieldref_info、CONSTANT_Methodref_info等。

#### **类加载器分类：**

1.JVM支持两种类型的加载器，分别为引导类加载器C/C++实现（BootStrap ClassLoader）和自定义类加载器由Java实现
2.从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是java虚拟机规范却没有这么定义，而是将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器。
3.按照这样的加载器的类型划分，在程序中我们最常见的类加载器是：引导类加载器BootStrapClassLoader、自定义类加载器(Extension Class Loader、System Class Loader、User-Defined ClassLoader）




































































