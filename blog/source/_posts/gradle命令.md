---
title: gradle命令
date: 2018-12-06 14:17:24
tags:
---
#### 前言
项目中一直有用到gradle来构建项目,使用了以后,发现极为好用,但是又因为之前没有用过,很多东西不明白,所以有本文章用来记录gradle相关

#### 插件
在gradle.build中加入如下代码
```base
apply plugin: 'java'
```
表示 使用gradle中的java插件,java插件中有许多丰富的功能
#### 源集
java插件定义了两个源集,main和test.main为源代码,会被打成jar包,test为单元测试代码
两个源集在项目中可以用 sourceset来配置 例如
```base
sourceset{
	main{
		srcDir()	
	}
	test{

	}
}
```
