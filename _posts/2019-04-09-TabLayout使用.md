---
layout: post
title: 'TabLayout的使用'
date: 2019-04-09
categories: Android
tags: TabLayout

---


## TabLayout 
TabLayout实现的是一个选项卡的效果，是安卓6.0之后出现的，在android.support.design.widget包下。

### TabLayout属性

```android
//设置字体选中颜色
app:tabSelectedTextColor
//设置字体默认颜色
app:tabTextColor
//设置指示器高度
app:tabIndicatorHeight
//设置指示器颜色
app:tabIndicatorColor
//设置指示器是否填满宽度
app:tabIndicatorFullWidth
//设置Tab滚动方式 scrollable（滚动）  fixed（固定）
app:tabMode
```

### 使用
```android
//设置选项卡文字和图标
mytab.addTab(mytab.newTab().setText("选项卡一").setIcon(R.mipmap.ic_launcher));
mytab.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
//                添加选中Tab的逻辑
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {
//                添加未选中Tab的逻辑
            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {
//                再次选中tab的逻辑
            }
        });
```

### 自定义tab
```android
 for (int i = 0;i <tab.size(); i++){
            TabLayout.Tab item = tlState.getTabAt(i);
            if (item!=null){
                @SuppressLint("InflateParams")
                View view = LayoutInflater.from(getContext()).inflate(R.layout.item_tab, null);
                ((TextView) view.findViewById(R.id.tv_count)).setText(tab.get(i));
                ((TextView) view.findViewById(R.id.tv_title)).setText(title[i]);
                //最后添加view到Tab上面
                item.setCustomView(view);
            }
        }

mytab.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
//                添加选中Tab的逻辑
				if (tab!=null){
	                View view = tab.getCustomView();
	               
	                ((TextView) Objects.requireNonNull(view).findViewById(R.id.tv_count)).setTextColor(getResources().getColor(R.color.green_7e));
	                ((TextView) view.findViewById(R.id.tv_title)).setTextColor(getResources().getColor(R.color.green_7e));	             	            
	            }
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {
//                添加未选中Tab的逻辑
 					if (tab!=null){
                        View view = tab.getCustomView();
                        ((TextView) Objects.requireNonNull(view).findViewById(R.id.tv_count)).setTextColor(black_6a);
                        ((TextView) view.findViewById(R.id.tv_title)).setTextColor(gray_80);
                    }
            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {
//                再次选中tab的逻辑
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