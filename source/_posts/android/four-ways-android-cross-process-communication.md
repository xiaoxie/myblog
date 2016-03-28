---
title: Android跨进程通信的四种方式
date: 2016-03-28 16:56:30
categories: android
tags: android
---

<!-- toc -->
<!--more-->

### 简述

Android 系统中应用程序之间不能共享内存。因此，在不同应用程序之间交互数据（跨进程通讯）就稍微麻烦一些。在android SDK中提供了4种用于跨进程通讯的方式。这4种方式正好对应于android系统中4种应用程序组件：Activity、Content Provider、Broadcast和Service。其中Activity可以跨进程调用其他应用程序的Activity；Content Provider可以跨进程访问其他应用程序中的数据（以Cursor对象形式返回），当然，也可以对其他应用程序的数据进行增、删、改操 作；Broadcast可以向android系统中所有应用程序发送广播，而需要跨进程通讯的应用程序可以监听这些广播；Service和Content Provider类似，也可以访问其他应用程序中的数据，但不同的是，Content Provider返回的是Cursor对象，而Service返回的是Java对象，这种可以跨进程通讯的服务叫AIDL服务。

### 访问其他应用程序的Activity

#### Activity既可以在进程内（同一个应用程序）访问，也可以跨进程访问。如果想在同一个应用程序中访问Activity，需要指定Context对象和Activity的Class对象

代码如下：

	Intent intent = new  Intent(this , Test.class );  
	startActivity(intent);  

#### Activity的跨进程访问与进程内访问略有不同。

虽然它们都需要Intent对象，但跨进程访问并不需要指定Context对象和Activity的Class对象，而需要指定的是要访问的Activity所对应的Action（一个字符串）。有些Activity还需要指定一个Uri（通过 Intent构造方法的第2个参数指定）。在android系统中有很多应用程序提供了可以跨进程访问的Activity，例如，下面的代码可以直接调用拨打电话的Activity。

	Intent callIntent = new  Intent(Intent.ACTION_CALL, Uri.parse("tel:12345678" );  
	startActivity(callIntent);

这个常量是一个字符串常量，也是我们在这节要介绍的跨进程调用Activity的关键。如果在应用程序中要共享某个Activity，需要为这个 Activity指定一个字符串ID，也就是Action。也可以将这个Action看做这个Activity的key。在其他的应用程序中只要通过这个 Action就可以找到与Action对应的Activity，并通过startActivity方法来启动这个Activity。

#### 也可以使用startActivityForResult方法来启动其他应用程序的Activity，以便获得Activity的返回值。


### Content Provider 

Android应用程序可以使用文件或SqlLite数据库来存储数据。Content Provider提供了一种在多个应用程序之间数据共享的方式（跨进程共享数据）。应用程序可以利用Content Provider完成下面的工作:

1. 查询数据
2. 修改数据
3. 添加数据
4. 删除数据

虽然Content Provider也可以在同一个应用程序中被访问，但这么做并没有什么意义。Content Provider存在的目的向其他应用程序共享数据和允许其他应用程序对数据进行增、删、改操作。
Android系统本身提供了很多Content Provider，例如，音频、视频、联系人信息等等。我们可以通过这些Content Provider获得相关信息的列表。这些列表数据将以Cursor对象返回。因此，从Content Provider返回的数据是二维表的形式。

Content Provider 的创建枯燥无味，可以使用生成器来辅助开发 [android-contentprovider-generator](https://github.com/BoD/android-contentprovider-generator)

### 广播（Broadcast）

广播是一种被动跨进程通讯的方式。当某个程序向系统发送广播时，其他的应用程序只能被动地接收广播数据。这就象电台进行广播一样，听众只能被动地收听，而不能主动与电台进行沟通。
在应用程序中发送广播比较简单。只需要调用sendBroadcast方法即可。该方法需要一个Intent对象。通过Intent对象可以发送需要广播的数据。

### AIDL服务

服务（Service）是android系统中非常重要的组件。Service可以脱离应用程序运行。也就是说，应用程序只起到一个启动Service的作用。一但Service被启动，就算应用程序关闭，Service仍然会在后台运行。

android系统中的Service主要有两个作用：后台运行和跨进程通讯。后台运行就不用说了，当Service启动后，就可以在Service对象中运行相应的业务代码，而这一切用户并不会察觉。而跨进程通讯是这一节的主题。如果想让应用程序可以跨进程通讯，就要使用我们这节讲的AIDL服 务，AIDL的全称是Android Interface Definition Language，也就是说，AIDL实际上是一种接口定义语言。通过这种语言定义接口后，Eclipse插件（ODT）会自动生成相应的Java代码接 口代码。

### 总结 
      
四种跨进程通讯的方式：Activity、ContentProvider、Broadcast、AIDL、Service。
其中Activity可以跨进程调用其他应用程序的Activity；
ContentProvider可以访问其他应用程序返回的 Cursor对象；Broadcast采用的是被动接收的方法，也就是说，客户端只能接收广播数据，而不能向发送广播的程序发送信息。
AIDL Service可以将程序中的某个接口公开，这样在其他的应用程序中就可以象访问本地对象一样访问AIDL服务对象了。




