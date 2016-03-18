title: Android Bundle 传递数据
date: 2015-12-30 13:59:08
categories: android
tags: Android
---

Android 开发中 bundle 再熟悉不过了，看了下 bundle 底层实现，涉及到的知识点不少，intent 的传递也是使用了内置的 bundle 来实现的，本文梳理对这些知识点做梳理整理：

* ArrayMap
* Parcel
* ClassLoader
* Java 序列化和反序列

<!--more-->

### 支持的数据类型

基本数据类型 

```
boolean value
byte value
char value
short value
int value
long value
String value
float value
double value
Size value
SizeF value
Parcelable value
Serializable value
ArrayList<? extends Parcelable> value
? extends Parcelable[] value
```

