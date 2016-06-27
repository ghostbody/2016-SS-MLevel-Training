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
2.解压并导入junit-4.9.jar，
   (1)解压junit4.9.zip并找到junit-4.9.jar文件</br>
   (2)将junit-4.9.jar拷贝到java安装目录下的lib文件夹下
  ```
  sudo cp junit-4.9.jar /usr/lib/jvm/java-1.7.0-openjdk-amd64/jre/lib
  ```
  >注意上面/usr/lib/jvm/java-1.7.0-openjdk-amd64/jre/lib为java的安装目录，不同计算机
  安装java的路径可能不同。

 3.设置环境变量
 在~/.bashrc文件中设置junit的环境变量，具体方法如下：
 ```
 sudo gedit ~/.bashrc
 ```
 在上述文件中结尾加上以下内容
 ```
 export JUNIT_HOME=/usr/lib/jvm/java-1.7.0-openjdk-amd64/jre/lib
 export CLASSPATH=.:$CLASSPATH:$JUNIT_HOME/junit-4.9.jar
 ```
 >注意其中/usr/lib/jvm/java-1.7.0-openjdk-amd64/jre/lib为java中的安装路径，不同计算机
 安装java的路径可能不同。

 4.source ~/.bashrc使环境变量生效
 具体步骤：
  ```
  source ~/.bashrc
  ```
  **重启或者注销计算机使其生效**

 ## 测试Junit安装成功
 1.创建java类文件TestJunit.java
 ```java
import org.junit.Test;
import static org.junit.Assert.assertEquals;
public class TestJunit {
@Test
public void testAdd() {
   String str= "Junit is working fine";
   assertEquals("Junit is working fine",str);
}
}
 ```
 2.创建java类文件TestRunner.java
```java
  import org.junit.runner.JUnitCore;
  import org.junit.runner.Result;
  import org.junit.runner.notification.Failure;

  public class TestRunner {
   public static void main(String[] args) {
      Result result = JUnitCore.runClasses(TestJunit.class);
      for (Failure failure : result.getFailures()) {
         System.out.println(failure.toString());
      }
      System.out.println(result.wasSuccessful());
   }
}
```
3.验证结果
```
javac TestJunit.java TestRunner.java
java TestRunner
```
若结果为true,则设置成功，否则失败
