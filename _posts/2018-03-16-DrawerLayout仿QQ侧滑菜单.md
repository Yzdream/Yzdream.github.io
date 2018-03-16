---
layout: post
title: 'DrawerLayout仿QQ侧滑菜单'
date: 2018-03-16
categories: Android
tags: Android
---
### 概述
DrawerLayout是Support Library包中实现了侧滑菜单效果的控件，drawerLayout分为侧边菜单和主内容区两部分，侧边菜单可以根据手势展开与隐藏（drawerLayout自身特性），主内容区的内容可以随着菜单的点击而变化（这需要使用者自己实现）。

效果图如下：
![](https://i.imgur.com/54UYDOE.png)

### 主界面布局
```android
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

		<android.support.v7.widget.RecyclerView
		    android:id="@+id/recycler_main"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content">
	
	</android.support.v7.widget.RecyclerView>

    <fragment
        android:name="com.yz.tool.view.fragment.MenuFragmrnt"
        android:layout_width="250dp"
        android:layout_height="match_parent"
        android:layout_gravity="left"
        android:tag="Left" />

</android.support.v4.widget.DrawerLayout>

```
说明：官网推荐将DrawerLayout作为根布局，否则可能会出现触摸事件被屏蔽的问题；RecyclerView主内容区的内容要放在侧滑菜单布局的前面, 因为 XML 顺序意味着按 z 序（层叠顺序）排序。这里菜单栏使用的fragment，name绑定name，MenuFragmrnt就是侧滑出来的菜单，在MenuFragmrnt的XML文件写侧滑菜单的布局。

### 初始化菜单栏
```android
 private void initMenu() {
        //屏幕保持高亮，不被侧滑菜单遮盖
        drawerLayout.setScrimColor(0x00ffffff);
        drawerLayout.addDrawerListener(new DrawerLayout.DrawerListener() {
            @Override
            public void onDrawerSlide(View drawerView, float slideOffset) {
                //滑动过程中不断回调 slideOffset:0~1
                View content = drawerLayout.getChildAt(0);
                float scale = 1 - slideOffset;//1~0
                content.setTranslationX(drawerView.getMeasuredWidth() * (1 - scale));//0~width
            }

            @Override
            public void onDrawerOpened(View drawerView) {

            }

            @Override
            public void onDrawerClosed(View drawerView) {

            }

            @Override
            public void onDrawerStateChanged(int newState) {

            }
        });
    }
```
说明：调用initMenu()之后可以使用手势滑出菜单栏了，要更改滑出效果在addDrawerListener中修改即可。

### 点击触发菜单
```android
drawerLayout.openDrawer(Gravity.LEFT);
```
在你需要点击触发的事件中加入以上代码即可点击触发侧滑效果。

 

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