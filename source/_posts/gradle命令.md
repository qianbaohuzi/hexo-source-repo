---
title: gradle命令
date: 2018-12-06 14:17:24
tags:
description: 关于gradle构建Java项目的一些东西
---
#### 前言
项目中一直有用到gradle来构建项目,使用了以后,发现极为好用,因为之前没有用过,很多东西不熟悉,所以有本文章用来记录gradle相关知识

#### projects和tasks
一个project就是一个应用,可以是一个jar包或一个web应用.如果是多模块项目,则每个模块都会打包一个jar,每个模块都是一个project.
一个project本质上就是一组task的集合,每一个task就是可执行的最小的单元,例如编译源码(compileJava),编译测试类(compileTestJava).
如何构建一个自己的task
```groovy
task hello {
	println 'hello world'
}
```


#### 插件
__插件在gradle中是非常重要的一部分,也是我们用来快速构建项目的重要工具下面就来讲讲如何导入需要用到的插件,下面主要以java插件为主__
在gradle.build中加入如下代码
```groovy
apply plugin: 'java'
```
或者
```groovy
plugins {
	id 'java'
}
```
表示 使用gradle中的java插件,在这里推荐使用第二种方法.第二种写法的完整版写法,以springboot 2.1.5RELEASE版本插件为例:
```groovy
plugins {
	id 'org.springframework.boot' version '2.1.5.RELEASE'
}
```
这样就导入了springboot相关的插件,我们在build项目的时候就可以使用相关的task了,例如 bootJar

#### java插件
按照上面的步骤导入了Java插件，下面我们就要了解Java插件如何帮我们快速构建项目


###### 源集
java插件定义了两个源集,main和test.main为源代码,会被打成jar包,test为单元测试代码
两个源集在项目中可以用 sourceset来配置 例如
```groovy
sourceset{
	main{
		srcDir()	
	}
	test{
		srcDir()
	}
}
```
#### buildscript
经常会在gradle文件中见到如下buildscript代码片段
```
buildscript {
    ext {
        springbootVersion = '2.1.4.RELEASE'
        dependencymanagementPulginVersion = '0.6.0.RELEASE'
    }
    repositories {
        //spring提供的插件的仓库地址
        maven {
            url 'https://repo.spring.io/libs-milestone'
        }
    }
    dependencies {
		//使用springboot提供的插件
        classpath "org.springframework.boot:spring-boot-gradle-plugin:${springbootVersion}"
        classpath "io.spring.gradle:dependency-management-plugin:${dependencymanagementPulginVersion}"
    }
}
```
buildscript代码块中写入内容是gradle脚本自身需要使用的资源。build.gradle文档中的普通的代码是build项目的时候需要的依赖，而buildscript里面就是执行build.gradle时需要依赖的资源,包括依赖项、第三方插件、maven仓库等。
build脚本执行的时候会优先执行buildscript代码块中的内容
以之前讲过的导入springboot插件为例,就很明白看懂了
```groovy
buildscript {
	repositories {
		maven {
			url 'https://plugins.gradle.org/m2/'
		}
	}
	dependencies {
		classpath "org.springframework.boot:spring-boot-gradle-plugin:2.1.5.RELEASE"
	}
}
apply plugin : 'org.springframework.boot'
```
并且上面的这端代码等效于
```groovy
plugins {
	id 'org.springframework.boot' version '2.1.5.RELEASE'
}
```