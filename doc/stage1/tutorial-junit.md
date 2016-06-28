# Junit使用教程

> by 魏传柳

## Junit简介
  JUnit是一个Java语言的单元测试框架，Junit目前在一些大的公司或者相对规范的软件中实用较多。
测试在软件开发过程中是至关重要的一个环节，一般分为单元测试,集成测试（将各个模块拼接测试）
，确认测试，系统测试，验收测试和压力测试。
## Junit安装

 1. 验证java版本 java -version

  如果没有，按照下面步骤进行安装(命令行安装无需配置java环境)
  ```
   sudo add-apt-repository ppa:openjdk-r/ppa
   sudo apt-get update
   sudo apt-get install openjdk-7-jdk
  ```
2. 解压并导入junit-4.9.jar，（云平台已导入，忽略）
   (1)解压junit4.9.zip并找到junit-4.9.jar文件</br>
   (2)将junit-4.9.jar拷贝到java安装目录下的lib文件夹下
  ```
  sudo cp junit-4.9.jar /usr/lib/jvm/java-1.7.0-openjdk-amd64/jre/lib
  ```
  >注意上面/usr/lib/jvm/java-1.7.0-openjdk-amd64/jre/lib为java的安装目录，不同计算机
  安装java的路径可能不同。

 3. 设置环境变量
 在~/.bashrc文件中设置junit的环境变量，具体方法如下：
 ```
 gedit ~/.bashrc
 ```
 在上述文件中结尾加上以下内容
 ```
 export JUNIT_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre/lib
 export CLASSPATH=.:$CLASSPATH:$JUNIT_HOME/junit-4.9.jar
 ```
 >注意其中/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre/lib为java中的安装路径，不同计算机
 安装java的路径可能不同。

 4. source ~/.bashrc使环境变量生效
 具体步骤：
  ```
  source ~/.bashrc
  ```

## 测试Junit安装成功
 1.创建java类文件HelloWorld.java
 ```java
import java.util.*;

public class HelloWorld {
	String str;
	public void hello() {
		str="Hello World!";
	}
	public String getStr() {
		return str;
	}
}
 ```
 2.创建java类文件HelloWorldTest.java
```java
  import static org.junit.Assert.*;
import org.junit.Test;

public class HelloWorldTest {
	public  HelloWorld helloWorld = new HelloWorld();
	@Test
	public void testHelloWorld() {
		helloWorld.hello();
		assertEquals("Hello World!", helloWorld.getStr());
	}
}
```
3.验证结果
```
javac HelloWorldTest.java 
java -ea org.junit.runner.JUnitCore HelloWorldTest
```
设置成功运行结果
JUnit version 4.9
.
Time: 0.004

OK (1 test)

## Junit使用方法
> 先简单解释一下什么是 Annotation，这个单词一般是翻译成元数据。元数据是什么？元数据
就是描述数据的数据。也就是说，这个东西在 Java 里面可以用来和 public、static 等关键字
一样来修饰类名、方法名、变量名。修饰的作用描述这个数据是做什么用的，差不多和 public
描述这个数据是公有的一样。想具体了解可以看 Core Java2。
我们先看一下在 JUnit 3 中我们是怎样写一个单元测试的。比如下面一个类：

```java
public class AddOperation {
 public int add(int x,int y){
 return x+y;
 }
}
```

我们要测试 add 这个方法，我们写单元测试得这么写：
```java
import junit.framework.TestCase;
import static org.junit.Assert.*;
public class AddOperationTest extends TestCase{
 public void setUp() throws Exception {
 }
 public void tearDown() throws Exception {
 }
 public void testAdd() {
 System.out.println(\"add\");
 int x = 0;
 int y = 0;
 AddOperation instance = new AddOperation();
 int expResult = 0;
 int result = instance.add(x, y);
 assertEquals(expResult, result);
 }
}
```

可以看到上面的类使用了 JDK5 中的静态导入，这个相对来说就很简单，只要在 import 关键
字后面加上 static 关键字，就可以把后面的类的 static 的变量和方法导入到这个类中，调用
的时候和调用自己的方法没有任何区别。

> 我们可以看到上面那个单元测试有一些比较霸道的地方，表现在：
1.单元测试类必须继承自 TestCase。
2.要测试的方法必须以 test 开头。

```java
如果上面那个单元测试在 JUnit 4 中写就不会这么复杂。代码如下：
import junit.framework.TestCase;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;
/**
*
* @author bean
*/
public class AddOperationTest {

 @Before
 public void setUp() throws Exception {
 }
 @After
 public void tearDown() throws Exception {
 }
 @Test
 public void add() {
 System.out.println(\"add\");
 int x = 0;
 int y = 0;
 AddOperation instance = new AddOperation();
 int expResult = 0;
 int result = instance.add(x, y);
 assertEquals(expResult, result);
 }

}
```

我们可以看到，采用 Annotation 的 JUnit 已经不会霸道的要求你必须继承自 TestCase 了，而
且测试方法也不必以 test 开头了，只要以@Test 元数据来描述即可。
从上面的例子可以看到在 JUnit 4 中还引入了一些其他的元数据，下面一一介绍：

**@Before：**

使用了该元数据的方法在每个测试方法执行之前都要执行一次。

**@After：**

使用了该元数据的方法在每个测试方法执行之后要执行一次。

> 注意：@Before 和@After 标示的方法只能各有一个。这个相当于取代了 JUnit 以前版本中的
setUp 和 tearDown 方法，当然你还可以继续叫这个名字，不过 JUnit 不会霸道的要求你这么
做了。

**@Test(expected=*.class)**

在JUnit4.0之前，对错误的测试，我们只能通过fail来产生一个错误，并在try块里面assertTrue
（true）来测试。现在，通过@Test 元数据中的 expected 属性。expected 属性的值是一个异
常的类型

**@Test(timeout=xxx):**

该元数据传入了一个时间（毫秒）给测试方法，
如果测试方法在制定的时间之内没有运行完，则测试也失败。

**@ignore：**

该元数据标记的测试方法在测试中会被忽略。当测试的方法还没有实现，或者测试的方法已
经过时，或者在某种条件下才能测试该方法（比如需要一个数据库联接，而在本地测试的时
候，数据库并没有连接），那么使用该标签来标示这个方法。同时，你可以为该标签传递一
个 String 的参数，来表明为什么会忽略这个测试方法。比如：@lgnore(―该方法还没有实现‖)，
在执行的时候，仅会报告该方法没有实现，而不会运行测试方法。
