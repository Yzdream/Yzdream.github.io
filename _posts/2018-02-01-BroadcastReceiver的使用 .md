---
layout: post
title: 'BroadcastReceiver的使用'
date: 2018-02-01
categories: Android
tags: Android
---
### BroadcastReceiver概述

BroadcastReceiver（广播接收器），属于 Android 四大组件之一，监听和接收App发出的广播消息，并做出响应。如：Android不同组件间的通信（含 ：应用内 / 不同应用之间）、多线程通信、与 Android 系统在特定情况下的通信等。Android中的广播使用了设计模式中的观察者模式：基于消息的发布、订阅事件模型，因此在广播中有三个角色，消息订阅者（广播接收者）、消息发布者（广播发布者）、消息中心（AMS，即Activity Manager Service）。

### 广播接收器注册

##### 静态注册
静态注册的是一个全局的广播，当此 App首次启动时，系统会自动实例化注册的广播类，并注册到系统中。
注册方式：在AndroidManifest.xml里通过<receive>标签声明

- 注册广播
```android
 <receiver android:name="com.yz.listener.BgReceiver" >
    <intent-filter>
        <action android:name="1" />
        <action android:name="2" />
    </intent-filter>
 </receiver>
```
注意：这里注册的action一定要与接收者保持一致

- 广播接收者
继承我们的BroadcastReceiver就是一个广播类了，实现它的onReceive方法接受广播，实现回调。
```android
public class BgReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
       
    }
}
```

- 广播发布者
不带数据的，只是通知的广播
```android
this.sendBroadcast(new Intent("1"))//发布action为"1"的广播
```
带数据通知的广播
```android
Intent receiveIntent = new Intent();
receiveIntent.setAction("2");
receiveIntent.putExtra("name", "yz");
this.sendBroadcast(receiveIntent);
```
- 广播响应
```android
public class BgReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
         switch (intent.getAction().trim()) {
            case "1":
				//没有参数的通知广播
				break；
			case "2":
				//有参数的通知广播
				String name = intent.getStringExtra("name");
				break；
    }
}
```
注意：广播是根据注册了的action来区别的，想要发送一个广播就必须先注册它的action。

##### 动态注册
动态注册广播就是在你需要接收广播的地方去注册。对于动态广播，有注册就必然得有注销，否则会导致内存泄露，重复注册、重复注销也是不允许的。一般是在onCreate注册广播，在onDestory注销广播。

- 定义广播接收者
```android
 private BroadcastReceiver mReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            switch (intent.getAction().trim()) {
                case "1":                    
                    break;
                case "2":                   
                    break;
            }
        }
    };
```

- 注册广播
```android
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction("1");
intentFilter.addAction("2");
registerReceiver(mReceiver, intentFilter);
```
- 注销广播
```android
unregisterReceiver(mReceiver);
```
注意：注册广播的action不要重复，要保持唯一，否则会被其他广播接收。

广播最基本的使用到这里就完成了，至于有序广播和粘性广播我就不一一叙述了，粘性广播已经不推荐使用了，有序广播和普通广播使用差不多，就是在注册时添加了Priority的值，按照Priority属性值从大-小排序；Priority属性相同者，动态注册的广播优先。发送有序广播时使用sendOrderedBroadcast(intent);。现在EventBus出来后已经替代了Intent、Broadcast、Hander，不给过作为android四大组件之一我们还是需要了解的。




<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="MTAyMC8zMjU2Ny85MTI4">
<script type="text/javascript">
   (function(d, s) {
   var j, e = d.getElementsByTagName(s)[0];

   if (typeof LivereTower === 'function') { return; }

   j = d.createElement(s);
   j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
   j.async = true;

   e.parentNode.insertBefore(j, e);
   })(document, 'script');
</script>
<noscript> 为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->