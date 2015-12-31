---
layout: post
title: 使用 gradle 构建 Android
categories: android
tags: gradle, android
---

### Gradle资源

* [yun pan](http://yun.baidu.com/s/1skv1JZn) 云盘下载

* [android-gradle-dsl](http://google.github.io/android-gradle-dsl/) Android Gradle Plugin DSL 在线文档 

* [appspot](http://gradleplease.appspot.com/) Gradle Dependencies Configuration Generator(需要梯子)

* [androiddevtools](http://www.androiddevtools.cn/#gradle) 国内Android开发资源


### Hello gradle


### 踩过的坑

#### 1、使用远程依赖时下载不了jar包
	
配置信息如下：

	compile 'com.android.support:appcompat-v7:23.1.1'
	compile 'com.android.support:design:23.1.1'

这样不用在jar包作者更新后再次手动更新jar包获取最新版本。

但是很多人包括我自己在不了解gradle使用的情况下，引用在线jar包时怎么都下载不下来，例如提示：

![gradle fail image](/img/build-application-with-gradle-1.png)

这是没翻墙么，不对，goagent更新AS都没问题，排除了墙的问题后一时想不到问题点在哪了。

之后查找到一个解决方法，异常的简单，设置一下远程仓库使用mavenCentral，在gradle里最外层加上：

	allprojects {  
	    repositories {  
	        mavenCentral()  
	    }  
	} 

然后Sync project with gradle files一下，就开始下载了，Btw，mavenCentral不需要翻墙。

如果jar包在别的仓库，比如jcenter，那就在里面再加个jcenter()就OK啦。

#### 2、多模块项目打包，打包特慢 
	
旧的项目用到了第三方开源库，配置如下：

	module/build.gradle  apply plugin: 'android-library'
	app/build.gradle apply plugin: 'com.android.application'

修改后的配置如下：

	module/build.gradle  apply plugin: 'com.android.library'
	app/build.gradle apply plugin: 'com.android.application'

修改之后问题解决。这是因为开源库用到了低版本的gradle插件，编译过程中加载不同版本的gradle插件，很容易出现兼容和错误，
而且不建议使用过低版本来编译 ，应配置为 1.3.0 以上：


	dependencies {
		classpath 'com.android.tools.build:gradle:1.3.0'
	}

#### 3、jni库找不着

这是因为 studio 默认就是自动打包所有.Jar文件：

	dependencies {
	    compile fileTree(dir: 'libs', include: '*.jar')
	}

打包时没有将so文件打包导致，修改 build.gradle 文件，添加如下内容即可：
	
	sourceSets {
	    main {
	        jniLibs.srcDirs = ['libs']
	    }
	}

#### 4、manifest多参数替换

	manifestPlaceholders = [key1: value1,key2: value2,...]





