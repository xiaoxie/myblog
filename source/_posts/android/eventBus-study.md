title: EventBus 学习录
date: 2016-01-03 11:05:53
tags: EventBus,android
---

### EventBus 是神马？

> EventBus 直译过来就是事件总线，它使用发布订阅模式支持组件之间的通信，不需要显式地注册回调，比观察者模式更灵活，可用于替换Java中传统的事件监听模式

### EventBus 的作业
> 解耦，软件开发中永恒的话题，它不是通用的发布订阅系统，也不能用于进程间通信。

<!--more-->

### 开源库

> [guava](https://github.com/google/guava) Google出品的Guava，Guava是一个庞大的库，EventBus只是其中的一个小功能，因此实际项目中使用并不多
	 
> [EventBus](https://github.com/greenrobot/EventBus) 这个库的优点是接口简洁，集成方便，但是限定了方法名，不支持注解
	 
> [otto](https://github.com/square/otto) 修改自 Guava

### 原理

> ![EventBus原理图](/img/eventbus-1.png)

### UML

{% plantuml %}
	left to right direction
	skinparam packageStyle rect
	actor Produce
	actor Subscribe
	rectangle regitster {
	  Produce -- (onEvent)
	  (regitster) .> (onEvent) : include
	  (onEvent) .> (unRegitster) : include
	  (onEvent) -- Subscribe
	}
{% endplantuml %}

### 使用示例

> regitster
	
```
@Override protected void onResume() {
    super.onResume();

    // Register ourselves so that we can provide the initial value.
    BusProvider.getInstance().register(this);
}
```

> unRegitster

```
@Override protected void onPause() {
    super.onPause();

    // Always unregister when an object no longer should be on the bus.
    BusProvider.getInstance().unregister(this);
}
```

> post Event 

```
BusProvider.getInstance().post(produceLocationEvent());

```

> produce Event

```
@Produce public LocationChangedEvent produceLocationEvent() {
    // Provide an initial value for location based on the last known position.
    return new LocationChangedEvent(lastLatitude, lastLongitude);
}
```

