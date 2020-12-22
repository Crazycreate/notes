Maven
1.依赖管理：就是对jar包的统一管理
Maven项目中需要某一个jar包，只需要在Maven项目中配置需要jar包坐标信息，Maven程序根据jar包坐标的信息去jar包仓库去查找jar包。
jar包的坐标信息【Apache（公司名称）+struts2(项目名称)+2.3.24（版本信息）】
2.项目构建：在项目编码完成后，对项目进行编译、测试、打包、部署。

3.Maven常用的命令
（1）clean: mvn clean 将项目根目录下的target目录清理掉
（2）compile: mvn compile 将项目中的.java文件编译为.class文件
（3）test：单元测试 将项目根目录下的src/test/java目录下的单元测试类都会执行。
（4）package: 打包 web project -----> war包； java project -----> jar包
将项目打包，打包项目根目录下target目录；
（5）install:安装 解决本地多个项目公用一个jar包问题，打包到本地仓库。

4.生命周期：
在Maven中存在 三套 生命周期，每一套生命周期相互独立，互不影响。【在一套生命周期内，执行后面的命令前面的操作会自动执行。】
CleanLifeCycle:清理生命周期
  Clean
defaultLifeCycle 默认生命周期
   【compile,test,package,install,deploy】
siteLifeCycle:站点生命周期
  site

5.依赖范围：
添加依赖范围：默认是compile
Provided：运行部署到tomcat不在需要

如果将servlet-api.jar，设置为compile，打包后包含servlet-api.jar，war包部署到tomcat时，存在servlet-api.jar包冲突。导致运行失败
总结：如果使用到tomcat自带的jar包，将项目中的依赖作用范围设置为：provided

![image-20201218213647898](C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201218213647898.png)

我们手工创建了三个目录：

1. src/main/java
2. src/test/java
3. src/test/resources

为什么自动生成的目录不完备？确实挺无语的，我们就不要去纠结了。不过有必要稍微解释一下这个 Maven 目录规范：

- main 目录下是项目的主要代码，test 目录下存放测试相关的代码。
- ==编译输出后的代码会放在target 目录下（该目录与 src 目录在同一级别下，这里没有显示出来）==。
- java 目录下存放 Java 代码，resources 目录下存放配置文件。
- webapp 目录下存放 Web 应用相关代码。
- pom.xml 是 Maven 项目的配置文件。

其中 pom.xml 称为 **Project Object Model（项目对象模型）**，它用于描述整个 Maven 项目，所以也称为 Maven 描述文件。

理解pom.xml
<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201218171459676.png" alt="image-20201218171459676"  />

- modelVersion：这个是 POM 的版本号，现在都是 4.0.0 的，必须得有，但不需要修改。
- groupId、artifactId、version：分别表示 Maven 项目的组织名、构件名、版本号，它们三个合起来就是 **Maven** **坐标**，根据这个坐标可以在 Maven 仓库中对应唯一的 **Maven 构件**。
- packaging：表示该项目的打包方式，war 表示打包为 war 文件，默认为 jar，表示打包为 jar 文件。
- name、url：表示该项目的名称与 URL 地址，意义不大，可以省略。
- dependencies：定义该项目的依赖关系，其中每一个 dependency 对应一个 Maven 项目，可见 Maven 坐标再次出现，还多了一个 scope，表示作用域（下面会描述）。
- build：表示与构建相关的配置，这里的 finalName 表示最终构建后的名称 smart-demo.war，这里的 finalName 还可以使用另一种方式来定义（下面会描述）。



如果用树形图来表达 pom.xml，那么会更加清晰：

<img src="C:\Users\Crazycreate\AppData\Roaming\Typora\typora-user-images\image-20201218172214231.png" alt="image-20201218172214231" style="zoom:80%;" />

除了项目的基本信息（Maven 坐标、打包方式等）以外，每个 pom.xml 都应该包括：

1. Lifecycle（生命周期）
2. Plugins（插件）
3. Dependencies（依赖）

Lifecycle 是项目构建的生命周期，它包括 9 个 Phase（阶段）。

大家知道，==Maven 是一个核心加上多个插件的架构==，而这些插件提供了一系列非常重要的功能，这些插件会在许多阶段里发挥重要作用。

| **阶段** | **插件**                      | **作用**                                         |
| -------- | ----------------------------- | ------------------------------------------------ |
| clean    | clean                         | 清理自动生成的文件，也就是 target 目录           |
| validate | 由 Maven 核心负责             | 验证 Maven 描述文件是否有效                      |
| compile  | compiler、resources           | 编译 Java 源码                                   |
| test     | compiler、surefire、resources | 运行测试代码                                     |
| package  | war                           | 项目打包，就是生成构件包，也就是打 war 包        |
| verify   | 由 Maven 核心负责             | 验证构件包是否有效                               |
| install  | install                       | 将构件包安装到本地仓库                           |
| site     | site                          | 生成项目站点，就是一堆静态网页文件，包括 JavaDoc |
| deploy   | deploy                        | 将构件包部署到远程仓库                           |

以上表格中所出现的插件名称实际上是插件的别名（或称为前缀），比如：**compiler 实际上是 org.apache.maven.plugins:maven-compiler-plugin:2.3.2，这个才是 Maven 插件的完全名称。**

每个插件又包括了一些列的 **Goal（目标）**，以 compiler 插件为例，它包括以下目标：

- compiler:help：用于显示 compiler 插件的使用帮助。
- compiler:compile：用于编译 main 目录下的 Java 代码。
- compiler:testCompile：用于编译 test 目录下的 Java 代码。

可见，插件目标才是具体干活的人，一个插件包括了一个多个目标，一个阶段可由零个或多个插件来提供支持。

我们可以在 pom.xml 中定义一些列的项目依赖（构件包），每个构件包都会有一个 **Scope（作用域）**，它表示该构件包在什么时候起作用，包括以下五种：

1. compile：默认作用域，在编译、测试、运行时有效
2. test：对于测试时有效
3. runtime：对于测试、运行时有效
4. provided：对于编译、测试时有效，但在运行时无效
5. system：与 provided 类似，但依赖于系统资源

| 作用域   | 编译时有效 | 测试时有效  | 运行时有效 | 示例                     |
| -------- | ---------- | ----------- | ---------- | ------------------------ |
| compile  | **√**      | ***\*√\**** | **√**      | smart-framework.jar      |
| test     |            | ***\*√\**** |            | junit.jar                |
| runtime  |            | **√**       | **√**      | mysql-connector-java.jar |
| provided | **√**      | **√**       |            | servlet-api.jar          |
| system   | **√**      | **√**       |            | JDK 的 rt.jar            |



例如：如果您想开发一个 Smart 应用，可参考如下 p om.xml：

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <smart.version>1.0</smart.version>
    </properties>

    <groupId>com.smart</groupId>
    <artifactId>smart-demo</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>

    <dependencies>
        <!-- JUnit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <!-- MySQL -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.25</version>
            <scope>runtime</scope>
        </dependency>
        <!-- Servlet -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <!-- JSTL -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
            <scope>runtime</scope>
        </dependency>
        <!-- Smart -->
        <dependency>
            <groupId>com.smart</groupId>
            <artifactId>smart-framework</artifactId>
            <version>${smart.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Compile -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.5.1</version>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                </configuration>
            </plugin>
            <!-- Test -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.15</version>
                <configuration>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
            <!-- Package -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <warName>${project.artifactId}</warName>
                </configuration>
            </plugin>
            <!-- Tomcat -->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
            </plugin>
        </plugins>
    </build>

</project>
```

- 我们可使用 properties 来定义一些配置属性，例如：project.build.sourceEncoding（项目构建源码编码方式），可设置为 UTF-8，可防止中文乱码。也可定义相关构件包版本号，例如：smart.version，便于日后统一升级。
- 建议使用最新版本的 JUnit，通过 Archetype 自动生成的 JUnit 太老了（3.8.1），可改为最新版（4.11）。
- 因为没必要使用 MySQL 客户端的 API，它仅仅在运行时有效，所以我们将 MySQL 构件包的作用域设置为 runtime。
- 因为我们只想在代码中使用 Servlet API，而不想将它所对应的 jar 包放入 WEB-INF 的 lib 目录下，所以我们可设置 Servlet 构件包的作用域为 provided。
- ==为了保证在 JDK 1.6 运行，我们可配置 maven-compiler-plugin 插件，设置输入源码为 1.6，编译输出的字节码也为 1.6。==
- 如果想跳过测试，可配置 maven-surefire-plugin 插件，将 skipTests 设置为 true。
- 如果想配置生成的 war 包为 artifactId，可修改 maven-war-plugin 插件，将 warName 修改为 ${project.artifactId}，这样就无需再配置 finalName 了。
- 如果想通过 Maven 将应用部署到 Tomcat 中，可使用 tomcat7-maven-plugin 插件，可使用 mvn tomcat7:run-war 命令来运行 war 包。

**使用命令：**

前面我们已经使用了几个 Maven 命令，例如：mvn archetype:generate，mvn tomcat7:run-war 等。其实，可使用两种不同的方式来执行 Maven 命令：

方式一：**mvn <插件>:<目标> [参数]**

方式二：**mvn <阶段>**

现在我们接触到的都是第一种方式，而第二种方式才是我们日常中使用最频繁的，例如：

- **mvn clean**：清空输出目录（即 target 目录）
- **mvn compile**：编译源代码
- **mvn package**：生成构件包（一般为 jar 包或 war 包）
- **mvn install**：将构件包安装到本地仓库
- **mvn deploy**：将构件包部署到远程仓库

执行 Maven 命令需要注意的是：==必须在 Maven 项目的根目录处执行，也就是当前目录下一定存在一个名为 pom.xml 的文件==。





































