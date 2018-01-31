---
layout: post
title: 'EditText的drawable监听'
date: 2018-01-31
categories: Android
tags: Android
---

在我们的输入框中最右边一般有一个小图标，特别是密码框，有的是一个删除图标，有的是一个眼睛，那么点击这个小图标的事件是怎么监听的呢，先看看我们的效果图

![](https://i.imgur.com/eZqjEc8.png)


实现效果代码如下：

```android
  <EditText
            android:id="@+id/edit_name"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginLeft="5dp"
            android:background="@drawable/bg_login_user"
            android:drawableRight="@mipmap/cha"
            android:gravity="center_vertical"
            android:hint="最多十个字(不能修改)"
            android:maxLength="10"
            android:paddingLeft="10dp"
            android:inputType="text"
            android:paddingRight="10dp"
            android:singleLine="true"
            android:textSize="15sp" />
```
使用drawableRight属性既可以实现右边的小图标了，当然了，还有drawableLeft左侧图标的属性。下面就是事件的监听了：

```Android
    EditText.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {

                // et.getCompoundDrawables()得到一个长度为4的数组，分别表示左右上下四张图片
                Drawable drawable = et.getCompoundDrawables()[2];
                //如果右边没有图片，不再处理
                if (drawable == null)
                    return false;
                //如果不是按下事件，不再处理
                if (motionEvent.getAction() != MotionEvent.ACTION_UP)
                    return false;
                if (motionEvent.getX() > et.getWidth()
                        - et.getPaddingRight()
                        - drawable.getIntrinsicWidth()) {
                    et.setText("");//点击事件
                }
                return false;
            }
        });
```
 


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