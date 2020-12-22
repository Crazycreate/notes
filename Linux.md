Linux
（文件：读写执行（查看。创建，删除，移动，复制，编辑）权限（用户，用户组）。系统（磁盘，进程））

没有报错即成功

1.sync 将数据同步到硬盘上
2.一切皆文件
3.根目录 / ，所有文件都在该节点下
![image-20201123160952274](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123160952274.png)

/bin：bin 是 Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令。
/boot：这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。
/dev ：dev 是 Device(设备) 的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。
/**etc**：==这个目录用来存放所有的系统管理所需要的**配置文件**和子目录==
/**home**：用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的，如上图中的 alice、bob 和 eve。
/lib：lib 是 Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库
lost+found：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
/media：linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux 会把识别的设备挂载到这个目录下。
/mnt：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了。
**/opt**：opt 是 optional(可选) 的缩写，这是给主机==额外安装软件==所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。
/root：该目录为系统管理员，也称作超级权限者的用户主目录。
**/usr**：usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。
**/tmp**：tmp 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的。（安装一些用完即丢弃的文件，如安装包）
**/var**：var 是 variable(变量) 的缩写，这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。
/www：存放服务器网站相关的资源，环境，网站的项目



### 常用命令：

sudo 命令
![image-20201130194911123](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201130194911123.png)

sudo:
-V 显示版本编号
-h 会显示版本编号及指令的使用方式说明
-l 显示出自己（执行 sudo 的使用者）的权限
-v 因为 sudo 在第一次执行时或是在 N 分钟内没有执行（N 预设为五）会问密码，这个参数是重新做一次确认，如果超过 N 分钟，也会问密码
-k 将会强迫使用者在下一次执行 sudo 时问密码（不论有没有超过 N 分钟）
-b 将要执行的指令放在背景执行
-p prompt 可以更改问密码的提示语，其中 %u 会代换为使用者的帐号名称， %h 会显示主机名称
-u username/#uid 不加此参数，代表要以 root 的身份执行指令，而加了此参数，可以以 username 的身份执行指令（#uid 为该 username 的使用者号码）
==-s 执行环境变数中的 SHELL 所指定的 shell ，或是 /etc/passwd 里所指定的 shell==
-H 将环境变数中的 HOME （家目录）指定为要变更身份的使用者家目录（如不加 -u 参数就是系统管理者 root ）
command 要以系统管理者身份（或以 -u 更改为其他人）执行的指令



目录管理：
绝对路径的全称：C:\Users\Crazycreate\Desktop\Java
假如在Desktop目录下，对于Java文件，相对路径为/Java
绝对路径以 / 开头
cd :切换目录
./：当前目录
cd ..     :/返回上级目录
cd ../..  返回上两级目录 
cd java//进入文件夹
cd ~ 回到当前用户目录

ls ：查看目录中的文件 
-a 参数：all ，查看全部文件，包括隐藏文件
-l 列出所有文件，包含文件的属性和权限，没有隐藏文件
所有linux可以组合使用

pwd//显示当前目录

mkdir test //建立文件
mkdir -p test1/test2/test3 // 递归创建文件夹
mkdir dir1 dir2 同时创建两个目录 

rm -r test //删除文件
rmdir dir1 删除一个叫做 'dir1' 的文件// 只能删除空的目录，如果下面存在文件，需要删除文件，递归删除多个目录使用-p参数
rm -rf dir1 删除一个叫做 'dir1' 的目录并同时删除其内容 
-f 强制删除
-r 递归删除目录
-i 互动，删除询问

cp复制文件和目录
cp file1 file2 复制file1到file2中
cp -r dir1 dir2 复制一个目录及子目录（文件夹）

mv 移动文件或者目录
-f 强制
-u 只替换已经更新过的文件

### 基本属性：

![image-20201123195015592](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123195015592.png)

实例中，bin 文件的第一个属性用 d 表示。d 在 Linux 中代表该文件是一个目录文件。
在 Linux 中第一个字符代表这个文件是目录、文件或链接文件等等。
当为 d 则是目录
当为 - 则是文件；
若是 l 则表示为链接文档(link file)；
若是 b 则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；
若是 c 则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。
接下来的字符中，以三个为一组，且均为 **rwx** 的三个参数的组合。其中， **r** 代表可读(read)、 **w** 代表可写(write)、 **x** 代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号 **-** 而已。

![image-20201123195528041](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123195528041.png)
从左至右用 0-9 这些数字来表示。
第 0 位确定文件类型，第 1-3 位确定属主（该文件的所有者）拥有该文件的权限。
第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。
其中，第 1、4、7 位表示读权限，如果用 r 字符表示，则有读权限，如果用 - 字符表示，则没有读权限；
第 2、5、8 位表示写权限，如果用 w 字符表示，则有写权限，如果用 - 字符表示没有写权限；第 3、6、9 位表示可执行权限，如果用 x 字符表示，则有执行权限，如果用 - 字符表示，则没有执行权限。

![image-20201123200341158](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123200341158.png)

对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。
同时，在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。
文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。
因此，Linux系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。
在以上实例中，mysql 文件是一个目录文件，属主和属组都为 mysql，属主有可读、可写、可执行的权限；与属主同组的其他用户有可读和可执行的权限；其他用户也有可读和可执行的权限。
对于 root 用户来说，一般情况下，文件的权限对其不起作用。

#### 更改文件属性

1、chgrp：更改文件属组 
chgrp [-R] 属组名 文件名
2、chown：更改文件属主，也可以同时更改文件属组 
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
3、chmod：更改文件9个属性
Linux文件属性有两种设置方法，一种是数字，一种是符号。
Linux 文件的基本权限就有九个，分别是 owner/group/others(拥有者/组/其他) 三种身份各有自己的 read/write/execute 权限。
先复习一下刚刚上面提到的数据：文件的权限字符为： -rwxrwxrwx ， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：
r:4 w:2 x:1
每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： -rwxrwx--- 分数则是：
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0
所以等一下我们设定权限的变更时，该文件的权限数字就是 **770**。变更权限的指令 chmod 的语法是这样的：
chmod [-R] xyz 文件或目录
选项与参数：
xyz : 就是刚刚提到的数字类型的权限属性，为 rwx 属性数值的相加。
-R : 进行递归(recursive)的持续变更，亦即连同次目录下的所有文件都会变更
举例来说，如果要将 .bashrc 这个文件所有的权限都设定启用，那么命令如下：
![image-20201123203352644](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201123203352644.png)

那如果要将权限变成 *-rwxr-xr--* 呢？那么权限的分数就成为 [4+2+1][4+0+1][4+0+0]=754。

#### Linux文件内容查看：

==cat==  由第一行开始显示==文件==内容
tac  从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
nl   显示的时候，顺道输出行号！常用

more 一页一页的显示文件内容（空格表示翻页,enter 代表向下看一行）
空白键 (space)：代表向下翻一页；
Enter         ：代表向下翻『一行』；
/字串         ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
:f            ：立刻显示出档名以及目前显示的行数；
q             ：代表立刻离开 more ，不再显示该文件内容。
b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。

less 与 more 类似，但是比 more 更好的是，他可以往前翻页！（上下键可以往上或往下走）
空白键    ：向下翻动一页；
[pagedown]：向下翻动一页；
[pageup]  ：向上翻动一页；
/字串     ：向下搜寻『字串』的功能；
?字串     ：向上搜寻『字串』的功能；
n         ：重复前一个搜寻 (与 / 或 ? 有关！)
N         ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
q         ：离开 less 这个程序；

head 只看头几行
tail 只看尾巴几行

### Linux的链接

Linux 链接分两种，一种被称为硬链接（Hard Link），另一种被称为符号链接（Symbolic Link）。默认情况下，**ln** 命令产生硬链接。

**硬连接**
硬连接指通过索引节点来进行连接。在 Linux 的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。在 Linux 中，多个文件名指向同一索引节点是存在的。比如：A 是 B 的硬链接（A 和 B 都是文件名），则 A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号相同，即一个 inode 节点对应两个不同的文件名，两个文件名指向同一个文件，A 和 B 对文件系统来说是完全平等的。删除其中任何一个都不会影响另外一个的访问。
硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删除。

**软连接**
另外一种连接称之为符号连接（Symbolic Link），也叫软连接。软链接文件有类似于 Windows 的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。比如：A 是 B 的软链接（A 和 B 都是文件名），A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号不相同，A 和 B 指向的是两个不同的 inode，继而==指向两块不同的数据块==。但是 A 的数据块中存放的只是 B 的路径名（可以根据这个找到 B 的目录项）。A 和 B 之间是“主从”关系，如果 B 被删除了，A 仍然存在（因为两个是不同的文件），但指向的是一个无效的链接。

建立连接命令：ln f1 f2 建立一个硬链接 由f2 到f1
ln -s f1 f3 //建立一个软连接 由 f3 到 f1
touch 命令创建文件
vim a.txt
echo 输入字符串
echo "ssss" >> a.txt(文件名)

### VIM编辑器

Vim是从 vi 发展出来的一个文本编辑器。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。 vim 则可以说是程序开发者的一项很好用的工具。

vi/vim 的使用
基本上 vi/vim 共分为三种模式，分别是命令模式（Command mode），输入模式（Insert mode）和底线命令模式（Last line mode）。 这三种模式的作用分别是：

命令模式：
用户刚刚启动 vi/vim，便进入了命令模式。
此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。比如我们此时按下i，并不会输入一个字符，i被当作了一个命令。
以下是常用的几个命令：
i 切换到输入模式，以输入字符。
x 删除当前光标所在处的字符。
: 切换到底线命令模式，以在最底一行输入命令。
若想要编辑文本：启动Vim，进入了命令模式，按下i，切换到输入模式。
命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。
![image-20201125185811957](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125185811957.png)

![image-20201125185844664](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125185844664.png)
![image-20201125185920021](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125185920021.png)

输入模式:
在命令模式下按下i就进入了输入模式。
在输入模式中，可以使用以下按键：
字符按键以及Shift组合，输入字符
ENTER，回车键，换行
BACK SPACE，退格键，删除光标前一个字符
DEL，删除键，删除光标后一个字符
方向键，在文本中移动光标
HOME/END，移动光标到行首/行尾
Page Up/Page Down，上/下翻页
Insert，切换光标为输入/替换模式，光标将变成竖线/下划线
ESC，退出输入模式，切换到命令模式

底线命令模式
在命令模式下按下:（英文冒号）就进入了底线命令模式。
底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。
在底线命令模式中，基本的命令有（已经省略了冒号）：
q 退出程序
w 保存文件
按ESC键可随时退出底线命令模式。
![image-20201125190200584](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125190200584.png)

![image-20201125190246537](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125190246537.png)特别注意，在 vi/vim 中，数字是很有意义的！数字通常代表重复做几次的意思！ 也有可能是代表去到第几个什么什么的意思。举例来说，要删除 50 行，则是用 『50dd』 对吧！ 数字加在动作之前，如我要向下移动 20 行呢？那就是『20j』或者是『20↓』即可。

### Linux 用户和用户组管理

![image-20201125192114320](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125192114320.png)

![image-20201125192143439](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125192143439.png)

![image-20201125195623120](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125195623120.png)

![image-20201125200851182](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125200851182.png)

![image-20201125200906010](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201125200906010.png)

![image-20201126154247205](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201126154247205.png)

![image-20201126154510961](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201126154510961.png)

![image-20201126154546163](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201126154546163.png)



### 磁盘管理

![image-20201126160412609](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201126160412609.png)
![image-20201126160500747](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201126160500747.png)

![image-20201126162329615](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201126162329615.png)

### 进程管理

基本概念
1.在Linux中，每一个程序都有自己的一个进程，每一个进程都有一个id号
2.每一个进程，都会有父进程
3.进程可以有两种存在方式：前台，后台运行
4.一般的话服务都是在后台运行，基本的程序都是前台运行的

命令
ps查看当前系统中正在执行的各种进程的信息
ps-xx:
	-a 显示当前终端运行的所有进程信息
	-u 以用户的信息显示进程
	-x 显示后台运行进程的参数

例子： ps -a|b     ps -aux|grep mysql
| 是管道符，表示可以把a命令的输出作为b命令的输入
grep 查找文件中符合条件的字符串
只需掌握 ps-xx|grep 进程名字！过滤进程信息

ps -ef:可以查看父进程信息
ps -ef|grep mysql 看父进程我们一般可以通过目录树结构来查看
pstree -pu
	-p 显示父id
	-u 显示用户组
kill -9 进程id //强制结束进程

### 环境安装：

![image-20201128150932212](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201128150932212.png)

























