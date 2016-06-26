# 云平台注意事项

## 用户
TA： 使用测试帐号
学生： 通过学号登录

## 限制

1. 中级实训云平台用户不能访问外网，除了Matrix网站
2. 没有root权限，不可更改大部分系统配置

## 预装环境

1. 操作系统: ubuntu 12.04
2. open JDK 6,7,8 三个版本，在/usr/lib/jvm 下切换版本
3. eclipse环境，不带junit集成，不带sonar集成
4. sublime编辑器，不带插件
5. docker容器引擎，用于启动sonar的web服务
6. apache-ant

## 预先存放的资源安装包

存放目录 /opt/resources, 包括

1. 项目代码，包括实训3个阶段所有需要的框架资源
2. Junit的jar包
3. sonar-runner-2.4-dist 以及 sonar-scanner

提醒同学们把资源文件拷贝到自己主目录再进行操作

## Docker 镜像

sonarqube-5.6-community:latest