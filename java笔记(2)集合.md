Object类：1.类Object是类层此结构的根，每个类都使用Object作为超（父）类；
因此自己定义的类，也可以使用Object中的方法；
（1）equal：对于基本类型来说比较的是值；对于引用数据类型来说是比较的两个对象的地址值
（2）toString：如果没有重写toString方法，那么打印的就是对象的地址值（默认）【因为子类中没有重写该方法，
就会去找父类的toString()方法】。

注意：多态的弊端：无法使用子类特有的属性和方法
解决：可以使用向下转型（强转）

2.null是不能调用方法的，会抛出空指针异常
3.总结：Objects对象工具类，

Data类：
1.System.currentTimeMillis()获取当前系统的毫秒值（从1970年1月1日0.0.0开始）
Date()；空参数构造方法
Data(Long date)：传递毫秒值，吧毫秒值转换成Data日期
Data.getTime();把日期转换成毫秒值

2.SimpleDateFormat("yyyy-MM-dd HH:mm:ss");按照指定的模式，输出日期
String text = simpleDateFormat.format(date);

按照指定的格式解析日期
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒");
        Date date2 = sdf.parse("2020年08月10日 11时39分56秒");
        System.out.println(date2);

4.总结：Calendar类
A.创建对象方式
Calendar c= Calendar.newlnstance();
B成员方法
int get(int n); 获取指定日历字段信息
void set(int n, int value); 将指定日历字段设置为指定的值 c.set(Calendar.YEAR,1997);
void add(int n, int value);将指定日历字段增加或减少为指定的值



**System类**
1.public static long currentTimeMills(); 返回以毫秒为单位的当前时间，可以用来计算程序执行的时间
2.public static void arraycopy(Object src, int srcPos, Objcet dest, int destPos, int length);
将src中的数组从第srcPos号位置开始，赋值到dest数组中的destPos位置，复制的个数为length;

StringBuilder类
1.String类字符串常量；他们的值在创建之后不能更改。字符串的底层是一个被final修饰的数组，不能改变，是一个
常量。【给一个已有字符串"abcd"第二次赋值成"abcedl"，不是在原内存地址上修改数据，而是重新指向一个新对象，新地址】
2.StringBuilder类 字符串缓冲区，可以提高字符串的操作效率(看成一个长度可变的字符串)，底层也是一个数组，但是
是没有被final修饰，可以改变长度
StringBuilder在内存中始终只有一个数组，占用空间少，效率高，如果超出了StringBuilder的容量，会自动扩容。
3.StringBuilder的构造方法
StringBuilder bu1 = new StringBuffer();
StringBuilder bu2 = new StringBuffer("aaaa");
4.(1)public StringBuilder append(); 添加任意类型数据的字符串形式，并返回当前对象自身。自身就会发生改变
，直接调用即可。
链式编程：bu1.append().append().append().append();
(2)String--->StringBuilder, StringBuilder(String str)构造一个字符串生成器，并初始化指定字符串中的内容；
StringBuilder--->String public String toString();将当前的StringBuilder对象转化成String对象。
bu.toString();

StringBuffer 和 StringBuilder 的 3 个区别:
*线程安全，StringBuilder：线程不安全。因为StringBuffer的所有公开方法都是+synchronized+修饰的，
而+StringBuilder+并没有+synchronized+修饰。
*StringBuffer+每次获取+toString+都会直接使用缓存区的+toStringCache+值来构造一个字符串。
+而+StringBuilder+则每次都需要复制一次字符数组，再构造一个字符串。
*StringBuilder+的性能要远大于+StringBuffer
所以，StringBuffer+适用于用在多线程操作同一个+StringBuffer+的场景，如果是单线程场合+StringBuilder+更适合。


java传递的参数：总结就是基本数据类型传递的是形参，形参不影响实参；引用数据类型传递的是地址，形参在方法内被改变，
实参也会改变，若在方法内实例化了同名对象，即产生了新的地址，对这个同名对象的改变，由于地址不一样，所以不影响原来的对象



包装类：
1.基本数据类型的数据，使用非常方便，但是没有对应方法进行操作这些数据，所以我们可以使用一个类，把基本数据
类型的数据包装起来，这个类叫做包装类，再包装类中可以定义一些方法，用来操作基本类型的数据
2.装箱：从基本数据类型转换成对应的包装类对象
拆箱：从包装类对象转换为对应的基本数据类型
Integer i = new Integer(4);
Integer ii = Integer.valueof(4);
int num = i.intVaule();

自动装箱：Integer in = 1；
自动拆箱：in = in + 3；

3.字符串转换成基本类型：使用包装类中的静态方法parseXx(String s) 例如Integer.parseInt(String s)
基本类型转换成字符串类型：1.直接用“+”连接字符串
2.toString(int t) 返回一个表示指定的整数的String对象 Integer.toString(100)；
3.使用String类中的静态方法，static String valueof(int i)返回int参数的字符串表示形式。
String.valueOf(100);


======================================
Collection 集合
1.集合：集合是java中提供的一种容器，可以用来存储多个数据
集合和数组的区别：（1）数组的长度是固定的，集合的长度是可变的
（2）数组中存储的是同一类型的元素，可以存储基本数据类型值，集合存储的都是对象，而且对象的类型可以不
一致，在开发中一般当多个对象的时候，使用集合进行存储。
2.Collection List   ArrayList   LinkedList    Vector
                    Set  HashSet   LinkedHashSet    TreeSet
Collection add() remove() clear() contains()(是否包含某个元素) isEmpty() size() toArray()(集合转换成数组)<定义在最顶层中，子类均继承>

Iterator接口 （Iterator是一个接口，需要实现类）
1（1）.Boolean hasNext() 如果仍有元素则返回true
（2）E next()返回迭代的下个元素
Collection接口中有一个方法，iterator(),这个方法返回的就是迭代器的实现类对象
Iterator<E> iterator() Iterator
2.当Iterator.next()每执行一次，Iterator变会自加1

3.增强for循环
        for(String c : coll){//集合或数组的数据类型 变量名 ： 集合名/数组名
            System.out.print(c);// 变量名
        }

泛型
泛型是一种未知的数据类型，
1.ArrayList集合定义的时候，不知道集合中都会存什么类型的数据，所以类型使用泛型
public class ArrayList<E>{
           public E get(int index)
}

2.含有泛型的方法：
public class Demo01Generic {
    public <E> void method(E e){
        System.out.println(e);
    }
}

3.含有泛型的接口：
格式：public interface GenericInterface<I> {}
实现方法：（1）在定义实现类时就，指定了泛型的数据类型：public class GenericInterfaceImpl implements GenericInterface<String>{}
（2）接口是什么泛型，实现类就是什么泛型，相当于定义了一个泛型类，【在创建对象的时候确定泛型的数据类型】
public class GenericInterfaceImpl2<I> implements GenericInterface<I>{}
GenericInterfaceImpl2<Integer> gi2 = new GenericInterfaceImpl2<>();

4.泛型通配符：?:代表任意的数据类型
使用方式：（1）不能创建对象使用，（2）**只能作为方法的参数使用**
public static void print(ArrayList<?> list)   用来接收

![image-20201204153841002](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201204153841002.png)
======================================
数据结构：
1.List : set() get() remove() add()
ArrayList 底层是一个数组 查询快增删慢
（2）.LinkedList 底层是一个哈希表（数组+链表/红黑树） 查询慢增删快 特有的getLast() addFirst()
2 .Set: 不允许重复的元素 没有索引，不能使用普通for遍历（用迭代器遍历，或是增强for）
HashSet：继承自Set，还有两个特点，存储元素与取出元素顺序不一致， 底层是一个哈希表（查询速度很快）
![image-20201204153209457](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201204153209457.png)

![image-20201204153603587](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201204153603587.png)
set.add() 因为map中的key无法重复，因此set无法重复

Object 中的一个方法 int hasCode();返回对象的哈希码值 （哈希值时十进制的数，由系统给出，就是对象的逻辑地址）

注意：Set元素不能重复，如果要存储自定义的元素，要重写hasCode(),和equals方法
LinkedHashSet特点存储是有序的，

可变参数：格式public static int adds(int...arr){} 修饰符 返回值类型 方法名（数据类型...变量名）{}
可以传递不确定数量的参数
注：（1）一个方法只能有一个可变参数，（2）如果方法参数有很多，那么可变参数必须写在参数列表末尾

**Collections（集合工具类）常用方法**：
（1）Collections.addAll()批量增加元素 Collections.addAll(list,"a","b","c","d");
（2）Collections.shuffle(list); 打乱顺序
（3）Collections.sort(list); 默认升序 
注：当排序的是自定义的对象时，需要在对象中重写compare方法（在对象中去实现Comparable<Person>接口）。
compareTo(Person o) 自己减去参数就是升序
第二种方法：Collections.sort(li, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o2.getAge() - o1.getAge();//当前时降序方法。
            }
        });


Map集合（HashMap、LinkedHashMap、TreeMap）
Map集合中的元素，key是不允许重复的，value是允许重复的
1.HashMap<K,V>底层是哈希表 是一个无序集合，是多线程的集合，线程不安全
LinkedHashMap集合底层是哈希表+链表
LinkedHashMap集合是一个有序集合，存储元素和取出元素的顺序一致

2.map.put("a","1");
System.out.println(map.remove("a")) ;key
System.out.println(map.get("c"));
map.containsKey();

Set<String> set = map.keySet();//返回key值的集合
Set<Map.Entry<String,String>> set =  map.entrySet();//把键值对存储在集合之中

Hahstable 单线程



### 集合源码：

#### 1.ArrayList:

（1）构造方法：

![image-20201216204401433](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201216204401433.png)

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201216205930085.png" alt="image-20201216205930085"  />

![image-20201216205808199](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201216205808199.png)

```
public ArrayList(Collection<? extends E> c) {
    Object[] a = c.toArray(); 
    if ((size = a.length) != 0) {
        if (c.getClass() == ArrayList.class) {//判断是否是数组
            elementData = a;
        } else {
            elementData = Arrays.copyOf(a, size, Object[].class);//不是数组则转成数组
        }
    } else {
        // replace with empty array.
        elementData = EMPTY_ELEMENTDATA;
    }
}
```

**总结：**
**1**.DEFAULTCAPACITY_EMPTY_ELEMENTDATA 该数组为空数组，用来记录ArrayList在生成时**没有**进行传参数，随后在操作过程中将elementData（底层数组）的大小设置为默认值10。
**2**.EMPTY_ELEMENTDATA用来记录ArrayList在生成时传参为0的情况。而为了与没传参数的情况分开所以重新用了一个空数组。

（2）add方法：（最核心的数组是 transient Object[] ==elementData==;）

```
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  //确定是否需要扩容
    elementData[size++] = e;
    return true;
}
```

```
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)//表示容量超过默认的需要扩容
        grow(minCapacity);
}
```

grow()对数组进行扩容

```
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);（除以二） 扩容到之前的1.5倍
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);//初始化时，把数组扩容，
    //把之前数组elementData的大小，copy到更大的里面
}
```

到目前为止，我们就可以知道`add(E e)`的基本实现了：
首先去检查一下数组的容量是否足够

- - **扩容到原来的1.5倍**
  - 第一次扩容后，如果容量还是小于minCapacity，就将容量扩充为minCapacity。
  - 足够：直接添加
  - 不足够：扩容

在指定位置添加 add(int index, E element)

```
public void add(int index, E element) {//在指定位置添加
    rangeCheckForAdd(index);//用来检测是否超过数组的范围

    ensureCapacityInternal(size + 1);  // Increments modCount!!
    System.arraycopy(elementData, index, elementData, index + 1,
                     size - index);
    elementData[index] = element;
    size++;
    
      /*public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
代码解释:
　　Object src : 原数组
   int srcPos : 从元数据的起始位置开始
　　Object dest : 目标数组
　　int destPos : 目标数组的开始起始位置
　　int length  : 要copy的数组的长度
　　*/
}
```

(3)remove方法：

指定下标删除

```
public E remove(int index) {
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);

    int numMoved = size - index - 1;//剩下需要copy的个数
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
}
```

指定元素删除：

```
public boolean remove(Object o) {
    if (o == null) {
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                fastRemove(index);
                return true;
            }
    } else {
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}

private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }
```



modCount字面意思就是修改次数,与线程安全相关，所有使用modCount属性的全是线程不安全的，发现这玩意只有在本数据结构对应迭代器中才使用

```
public void remove() {
    if (lastRet < 0)
        throw new IllegalStateException();
    checkForComodification();

    try {
        ArrayList.this.remove(lastRet);
        cursor = lastRet;
        lastRet = -1;
        expectedModCount = modCount;
    } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
    }
}
```

由以上代码可以看出，在一个迭代器初始的时候会赋予它调用这个迭代器的对象的mCount，如何在迭代器遍历的过程中，一旦发现这个对象的mcount和迭代器中存储的mcount不一样那就抛异常
好的，下面是这个的完整解释
**Fail-Fast 机制**
我们知道 java.util.HashMap 不是线程安全的，因此如果在使用迭代器的过程中有其他线程修改了map，那么将抛出ConcurrentModificationException，这就是所谓fail-fast策略。这一策略在源码中的实现是通过 modCount 域，modCount 顾名思义就是修改次数，对HashMap 内容的修改都将增加这个值，那么在迭代器初始化过程中会将这个值赋给迭代器的 expectedModCount。在迭代过程中，判断 modCount 跟 expectedModCount 是否相等，如果不相等就表示已经有其他线程修改了 Map：注意到 modCount 声明为 volatile，保证线程之间修改的可见性。



#### 2.LinkedList

链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成，每个结点包括两个部分：一个是存储数据元素地数据域，另一个是存储下一个结点地址地指针域。
双向链表是链表地一种，由结点组成，每个数据结点中都有两个指针，分别指向直接后继和直接前驱
从JDK1.7开始，LinkedList 由双向循环链表改为双向链表

![image-20201222094125356](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201222094125356.png)

```
transient int size = 0; // LinkedList节点个数

/**
 * Pointer to first node. 指向头结点
 * Invariant: (first == null && last == null) ||
 *            (first.prev == null && first.item != null)
 */
transient Node<E> first;

/**
 * Pointer to last node. 指向尾结点
 * Invariant: (first == null && last == null) ||
 *            (last.next == null && last.item != null)
 */
transient Node<E> last;
```

让我们来看LinkedList的最基础的存储单元——Node     **内部静态私有类Node**

```
private static class Node<E> {
    E item;      // 真正存储的数据
    Node<E> next;  // 前一个节点引用地址
    Node<E> prev;  // 后一个节点引用地址

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

(1)构造函数：

```
public LinkedList() {
}


public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}

public boolean addAll(Collection<? extends E> c) {
        return addAll(size, c); // size为LinkedList节点个数 
    }
    
public boolean addAll(int index, Collection<? extends E> c) {
        checkPositionIndex(index); // 1.添加位置的下标的合理性检查

        Object[] a = c.toArray();  // 2.将集合转换为Object[]数组对象
        int numNew = a.length;
        if (numNew == 0)
            return false;

        Node<E> pred, succ;// 3.找到插入位置的前继节点和后继节点
        if (index == size) {//如果需要插入到最后一个位置，或者链表为空
            succ = null;  // 从尾部添加的情况：前继节点是原来的last节点；后继节点是null
            pred = last;
        } else {
        // 从指定位置（非尾部）添加的情况: succ 指向 index 位置的节点，pred 指向它前一个
            succ = node(index); //把链表稍微遍历下，找到index这个位置      
            pred = succ.prev;
        }

        for (Object o : a) {
            @SuppressWarnings("unchecked") E e = (E) o;
            Node<E> newNode = new Node<>(pred, e, null);//把之前的链表，一个一个用new出来的新结点转移并且把当前这个结点前驱指向pred
            if (pred == null)//说明新建的结点是头节点
                first = newNode;
            else
                pred.next = newNode;//让pred结点指向它的后继就是new出来的结点
            pred = newNode;//把pred移向下一个位置
        }

        if (succ == null) {//说明要插入的位置就是尾部，现在pred即为last
            last = pred;
        } else {//当前的pred的后继为succ，同样succ的前驱为pred
            pred.next = succ;
            succ.prev = pred;
        }

        size += numNew;
        modCount++;
        return true;
    }
```

```
Node<E> node(int index) {//找到index处的这个结点
    // assert isElementIndex(index);

    if (index < (size >> 1)) {//比一半处小就从前向后找
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {//否则就从后向前找
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

（2） ① add(E e)方法

```
public boolean add(E e) {
    linkLast(e);
    return true;
}
```























































