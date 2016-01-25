---
layout: post
title: 持续集成(Continuous integration)
categories: linux
tags:
  - linux
---

### 持续集成简介
	持续集成是一种软件开发实践，即团队开发成员经常集成它们的工作，通过每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。

### 持续集成价值
	减少风险
	减少重复过程
	任何时间、任何地点生成可部署的软件
	增强项目的可见性
	建立团队对开发产品的信心

### 持续集成要素
	1.统一的代码库
	2.自动构建
	3.自动测试
	4.每个人每天都要向代码库主干提交代码
	5.每次代码递交后都会在持续集成服务器上触发一次构建
	6.保证快速构建
	7.模拟生产环境的自动测试
	8.每个人都可以很容易的获取最新可执行的应用程序
	9.每个人都清楚正在发生的状况
	10.自动化的部署

### 持续集成原则
	1. 所有的开发人员需要在本地机器上做本地构建，然后再提交的版本控制库中，从而确保他们的变更不会导致持续集成失败。
	2. 开发人员每天至少向版本控制库中提交一次代码。
	3. 开发人员每天至少需要从版本控制库中更新一次代码到本地机器。
	4. 需要有专门的集成服务器来执行集成构建,每天要执行多次构建。
	5. 每次构建都要100%通过。
	6. 每次构建都可以生成可发布的产品。
	7. 修复失败的构建是优先级最高的事情。
	8. 测试是未来，未来是测试

### 继续集成工具
* [Jenkins](https://jenkins-ci.org/)
* [Buildbot](http://buildbot.net/)
* [Travis CI](http://stridercd.com/)
* [Strider](http://stridercd.com/)
* [Go](http://www.go.cd/)
* [Integrity](http://integrity.github.io/)

### Jenkins 安装配置
	http://www.360doc.com/content/13/0412/09/10504424_277718090.shtml
	账号权限

	也可使用docker快速集成

### Jenkins 插件安装
	git
	subversion
	gradle
	maven
	ant
	cocoapods

### Android project 构建配置
	Android sdk
	ant
	gradle

### IOS project 构建配置
	xcodebuild
	xcrun
	问题：
		Code Sign error: No codesigning identities found:
		清除项目缓存：http://www.cnblogs.com/qingjoin/p/3929493.html

### Java web 构建配置
	maven
	ant
	gradle

### 单元测试及报告

### 质量测试