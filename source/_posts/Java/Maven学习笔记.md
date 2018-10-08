---
title: Maven学习笔记
date: 2018-10-08 10:53:44
categories: Java
tags: Maven
toc: true
---

> Maven 学习路上的点点滴滴

<!-- More -->

## Maven 安装

## Maven 配置

### 使用外部Maven
在 Eclipse 中设置外部 Maven，并使用自定义 Maven 配置文件 `settings.xml`。

### JRE 版本配置
默认情况下，右键项目 -> Maven -> Update Project
(check "Update project configuration from pom.xml")
项目属性将被设置成 JRE1.5，警告与当前所使用 JRE 不兼容。
修改 Maven 配置`D:\Program_Files\Java\Maven\conf`，如下：

``` md
<profile>
    <id>jdk-1.8</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
    </activation>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
</profile>
```
参见：[如何将maven的默认JRE1.5改成1.7修改方法](https://blog.csdn.net/wu2374633583/article/details/78949095)

## Maven 仓库

### (远程/本地)仓库
### 国内高速镜像站

``` md
/*----------- 阿里云 ----------*/
<mirror>
    <id>nexus-aliyun</id>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

``` md
/*--- Maven 官方运维的2号仓库 ---*/
<mirror>
    <id>repo2</id>
    <name>Mirror from Maven Repo2</name>
    <url>http://repo2.maven.org/maven2/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

``` md
/*-------- JBoss 的仓库 -------*/
<mirror>
    <id>jboss-public-repository-group</id>
    <mirrorOf>central</mirrorOf>
    <name>JBoss Public Repository Group</name>
    <url>http://repository.jboss.org/nexus/content/groups/public</url>
</mirror>
```

