title: Android 内存泄漏
date: 2016-01-04 18:16:41
categories: android
tags: 
	- android
---

<!-- toc -->
<!--more-->

### 1、Handler&内部类 (涉及知识点：静态和非静态内部类的区别)

```
public class SampleActivity extends Activity {

  private final Handler mLeakyHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
      // ... 
    }
  }
}
```
如果没有仔细观察，上面的代码可能导致严重的内存泄露。Android Lint会给出下面的警告：

```
In Android, Handler classes should be static or leaks might occur.
```

但是到底是泄漏,如何发生的？让我们确定问题的根源,先写下我们所知道的

> 在Java中，非静态的内部类和匿名类会隐式地持有一个他们外部类的引用。静态内部类则不会。
当Activity被finished后，当遇到延时发送的消息会继续在主线程的消息队列中存活，直到他们被处理。这个消息持有这个Activity的Handler引用，这个Handler有隐式地持有他的外部类。直到消息被处理前，这个引用都不会被释放。因此Activity不会被垃圾回收机制回收，泄露他所持有的应用程序资源。

如何解决

> 为了解决这个问题，Handler的子类应该定义在一个新文件中或使用静态内部类。静态内部类不会隐式持有外部类的引用。所以不会导致它的Activity泄露。如果你需要在Handle内部调用外部Activity的方法，那么让Handler持有一个Activity的弱引用（WeakReference）以便你不会意外导致context泄露。为了解决我们实例化匿名Runnable类可能导致的内存泄露，我们将用一个静态变量来引用他（因为匿名类的静态实例不会隐式持有他们外部类的引用）。

示例

```
private final MyHandler mHandler = new MyHandler(this);

/**
* Instances of anonymous classes do not hold an implicit
* reference to their outer class when they are "static".
*/
private static final Runnable sRunnable = new Runnable() {
  @Override
  public void run() { /* ... */ }
};
```





