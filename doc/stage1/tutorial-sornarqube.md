# Sonarqube 教程 2016

> by 叶嘉祺

## Sonarqube 简介

SonarQube（曾用名Sonar（声纳））是一个开源的代码质量管理系统。 支持超过25种编程语言：Java、C/C++、C#、PHP、Flex、Groovy、JavaScript、Python、PL/SQL、COBOL等。

本次实训使用这个系统来对同学们写的java代码进行质量分析,从而达到提高同学分编程质量的目的。

Sonar软件分为两个部分，一个是web服务端用于管理各个项目的检查报告，另外一个是扫描程序(sonar-runner 或者sonar-scanner)。

## Web 服务端部署

Web服务端通过容器引擎docker进行部署。docker是一个容器引擎，我们已经在云平台预先安装了由sornaqube官方提供的镜像，我们只需要一个命令，就可以运行整个服务器，这非常方便而且快捷。

### Quick Start

```shell
docker run -i -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

运行这个命令之后，我们在9000端口开启了sornarqube的web服务，我们可以通过浏览器访问9000端口，可以看到管理的web界面。

> 注意：为了简化实训的任务，我们使用了sonar的嵌入式数据库，这个在生产环境中是不建议的。通常可以使用mysql数据库或者postgre数据库等。

如果你需要关闭、或者重启这个web服务，可以通过容器引擎开关完成：

重启：

```
docker restart sonarqube
```

关闭：
```
docker stop sonarqube
```

删除容器：
```
docker stop sonarqube && docker rm sonarqube
```

注意：删除之后，数据可能会被清空。

关于数据安全：

对于sonarqube里面有两个重要的挂载点，可以通过命令：

```
docker inspect sonarqube
```
查看其中的mount信息

其中两个重要的数据文件在
```
/opt/sonarqube/data
/opt/sonarqube/extenstions
```

可以通过以下命令备份：

```
mkdir ~/sonarqube
docker cp sonarqube:/opt/sonarqube/data ~/sonarqube
docker cp sonarqube:/opt/extensions ~/sonarqube
```

也可以在运行容器时候就挂载数据文件到外部host

```
mkdir ~/sonarqube
mkdir ~/sonarqube/{data,extensions}
docker run -i -d --name sonarqube \
           -v ~/sonarqube/data:/opt/sonarqube/data \
           -v ~/sonarqube/extensions:/opt/sonarqube/extensions \
           -p 9000:9000 -p 9092:9092 sonarqube
```

不过只要你不进行容器删除操作，数据就会一直保存。

## 代码扫描程序

sonar-runner是一个已经预打包好了的程序，你可以设置一个环境变量达到方便使用的效果，也可以直接运行这个程序，这两种方法都是可以的。

注意：sonar-runner 必须要依赖于sonarqube的服务，没开启sonarqube服务会导致sonar-runner运行失败。

sornar资源放在云平台的/opt路径，首先把其复制到我们的主目录，并且解压之：

```
cp /opt/sonar-runner-dist-2.7.zip ~/sonar-runner.zip
cd ~/
unzip sonar-runner.zip
```

给sonar-runner运行脚本的运行权限
```
chmod +x ~/sonar-runner/bin/sonar-runner
```

测试运行
```
~/sonar-runner/bin/sonar-runner/bin/sonar-runner -v
```

输出版本信息：

```
WARN: sonar-runner script is deprecated. Please use sonar-scanner instead.
INFO: Scanner configuration file: path/conf/sonar-scanner.properties
INFO: Project root configuration file: NONE
INFO: SonarQube Scanner 2.6.1
INFO: Java 1.8.0_91 Oracle Corporation (64-bit)
INFO: Linux 4.4.0-24-generic amd64
```

如果想设置环境变量，可以修改~/.bashrc文件，这里不做介绍，有兴趣的同学可以做。

### 你可能会遇到的问题

Docker中的sonarqube依赖于JDK8环境，请修改sornar-runner的shell脚本，将`JAVA_CMD`替换为`/usr/lib/jvm/java-8-openjdk-amd64/bin/java`

### 配置你的项目

对与sonar-runner来说，在扫描项目的时候需要一个配置文件，名字是 sonar-project.properties

```
# 项目的唯一标识,必须不相同
sonar.projectKey=Part2-circleBug
# 项目名字
sonar.projectName=Part2-circleBug

sonar.projectVersion=1.0
sonar.sourceEncoding=UTF-8
sonar.modules=java-module

java-module.sonar.projectName=Java Module
java-module.sonar.language=java

# 这个是你项目的根目录
java-module.sonar.sources=.
java-module.sonar.projectBaseDir=./

```

写完这个配置文件之后，我们运行sonar-runner

```
~/sonar-runner/bin/sonar-runner /path/to/your/project 
```

注意/path/to/your/project是你的项目路径

## 查看结果

在web界面上查看 http://localhost:9000/

实训后期阶段会对代码质量有一定的量化要求，请查看wiki相关界面。

## 参考

[不同参数的意思](http://docs.codehaus.org/display/SONAR/Analysis+Parameters)

[不同项目的源码分析示例下载](https://github.com/SonarSource/sonar-examples/zipball/master)

[sonarQube 官网地址](http://www.sonarqube.org/)

[sonarQube 官方文档地址](http://docs.codehaus.org/display/SONAR/Documentation)