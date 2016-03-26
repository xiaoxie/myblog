title: 通过xcodebuild和xcrun自动构建IOS项目
date: 2016-01-27 17:55:33
categories: ios
tags: ios
---

### 简介

通常打包采用xcodebuild和xcrun两个命令，xcodebuild负责编译，xcrun负责将app打成ipa。

<!--more-->

### 配置过程

清理工程

	xcodebuild -target targetName clean

 
编译工程

	xcodebuild -target targetName

生成IPA

	xcrun -sdk iphoneos PackageApplication -v path/To/xxx.app -o xxx.ipa

如果是含签名的包，上面两个命令需要增加一些参数。

	xcodebuild -target targetName CODE_SIGN_IDENTITY="iPhone Distribution:XXXXXX"

	xcrun -sdk iphoneos PackageApplication -v 源app路径 -o 输出的ipa路径 --sign "iPhone Distribution:XXXXXX"

创建钥匙链

	security create-keychain -p myapp myapp.keychain

解锁，否则回弹框等待输入密码

	security unlock-keychain -p myapp myapp.keychain

导入证书

	security import /opt/myapp.p12 -k myapp.keychain -P mypassword -T /usr/bin/codesign

### 自动构建步骤：

1、先用xocde手动编译好一个App，假设为MyApp.app

2、导入证书文件到MAC的钥匙链

3、以MyApp.app为模板，copy一个备份，然后进行各种资源的替换，比如替换了应用的图片文件等

4、替换对应的*.mobileprovision文件到MyApp.app目录

5、删除MyApp.app下的签名信息_CodeSignature

6、修改info.plist Bundle indentifier和*.mobileprovision一致

7、修改MyApp.xcent中application-identifiervalue值为对应证书名称，可以以一个xcent为模板，注意如果没有aps-environment关键字，打出来的ipa包将没有apns模块，格式如下：

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
		<dict>
		        <key>application-identifier</key>
		        <string>Z4LR7CBRUD.com.yesun.vic</string>
		        <key>aps-environment</key>
		        <string>production</string>
		        <key>get-task-allow</key>
		        <false/>
		</dict>
	</plist>


8、重签名codesign

	codesign --force --sign 9c8b212f6a2c2382847b104e387a01b246d4ce42 --resource-rules=MyApp.app/ResourceRules.plist --entitlements MyApp.xcent Mkey3G.app

9、生成ipa包

	xcrun -sdk iphoneos  PackageApplication -v MyApp.app -o MyApp.ipa  --sign  9c8b212f6a2c2382847b104e387a01b246d4ce42 --embed MyApp.app/embed.mobileprovision

10、删除钥匙链

	security delete-keychain myapp.keychain

### 踩过的坑

1、Code Sign error: No codesigning identities found:

	清除项目缓存：http://www.cnblogs.com/qingjoin/p/3929493.html

2、产品发布证书P12

3、自动登录	

	在launchpad里找到钥匙串访问，解锁登陆，在最左下角点击证书，进入，找到跟苹果相关的证书，一般很少，很容易找，在目录文件里找到apple ID 开头的，双击，改为第一个，可以接受所有访问

[iOS项目通过xcodebuild和xcrun自动发布](http://blog.csdn.net/offbye/article/details/39552911?utm_source=tuicool&utm_medium=referral)

4、经典依赖管理项目打包方式：

	xcodebuild clean
	xcodebuild
	xcrun -sdk iphoneos  PackageApplication -v build/Release-iphoneos/hello.app -o ~/hello.ipa

5、cocoapods依赖管理项目打包方式：

	DEVELOPER_DIR=~/Library/Developer/Xcode/DerivedData
	pod install --verbose --no-repo-update
	xcodebuild clean
	xcodebuild -workspace *.xcworkspace -scheme jlb-hello-oc
	xcrun -sdk iphoneos PackageApplication -v $DEVELOPER_DIR/jlb-hello-oc*/Build/Products/Debug-iphoneos/*.app -o /var/website/apks/ios/ios-hello-oc-$(date +%Y%m%d).ipa

6、继续完善

App的自动生成系统，命令行下实现iOS App编译，压缩，签名等生成企业inhouse应用的过程，比较坑人的一点是最后一行命令可以实现压缩应用的效果，不需要通过xcodebuild exportArchive命令到处压缩的应用， 这个是国外一个blog看到的。关键命令如下：

	xcodebuild -project "SalesApp.xcodeproj"  -target "SalesApp"  -configuration "Release Adhoc" clean

	xcodebuild -project SalesApp.xcodeproj -sdk iphoneos  -scheme "SalesApp" -configuration "Release Adhoc" CONFIGURATION_BUILD_DIR="XXXXXX/build" 

	xcrun -sdk iphoneos PackageApplication -v "XXXXX/SalesApp.app" -o "XXXXX/SalesApp-Release.ipa" --sign "iPhone Distribution: XXXXX."  --embed "XXXX.mobileprovision"

这样就可以编译成签名过的应用用于企业发布了， 然后按照模板生成plist文件，放到https服务器上， 在iphone或iPad上通过Safari浏览器访问下面的地址就可以安装应用了。

	itms-services://?action=download-manifest&url=https://www.XXX.com/XXX.plist

### 扩展

企业包：[发布到 fir.im 或者 蒲公英pgyer.com 进行分发](http://www.cocoachina.com/ios/20150814/13061.html)

个人包：[发布到AppStore进行审核](http://www.cocoachina.com/appstore)

