---
layout: post
title: 'MagicIndicator'
date: 2018-07-25
categories: Android
tags: Android

---

MagicIndicator是一个与Fragment和ViewPage组合使用的标题导航组件，引用方法自行查看 [github](https://github.com/hackware1993/MagicIndicator)，这里讲述下与Fragment和ViewPage组合使用的方法。

### 与ViewPage一起使用

```android

 @Bind(R.id.tl_titles)
 MagicIndicator tlTitles;

 private final String[] titles = {"标题1", "标题2", "标题3", "标题4"};


CommonNavigator commonNavigator = new CommonNavigator(this);
commonNavigator.setAdjustMode(true);  //ture 即标题平分屏幕宽度的模式
commonNavigator.setAdapter(new CommonNavigatorAdapter() {
    @Override
    public int getCount() {
        return titles.length;
    }

    @Override
    public IPagerTitleView getTitleView(Context context, final int i) {
        ColorTransitionPagerTitleView colorTransitionPagerTitleView = new ColorTransitionPagerTitleView(context);
        colorTransitionPagerTitleView.setText(titles[i]);
		//选中时字体颜色
        colorTransitionPagerTitleView.setSelectedColor(getResources().getColor(R.color.black_2826));
		//未选中时字体颜色
        colorTransitionPagerTitleView.setNormalColor(getResources().getColor(R.color.gray_71));
        colorTransitionPagerTitleView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {             
                mViewPager.setCurrentItem(i);
            }
        });
        return colorTransitionPagerTitleView;
    }

    @Override
    public IPagerIndicator getIndicator(Context context) {
        LinePagerIndicator indicator = new LinePagerIndicator(context);
        indicator.setMode(LinePagerIndicator.MODE_WRAP_CONTENT);
        indicator.setColors(black);//指示器的颜色
        return indicator;
    }
});
tlTitles.setNavigator(commonNavigator);
ViewPagerHelper.bind(magicIndicator, mViewPager);//绑定ViewPage
```

### 与Fragment一起使用

```android

 @Bind(R.id.tl_titles)
 MagicIndicator tlTitles;

 private final String[] titles = {"标题1", "标题2", "标题3", "标题4"};

FragmentContainerHelper containerHelper = new FragmentContainerHelper(tlTitles);
CommonNavigator commonNavigator = new CommonNavigator(this);
commonNavigator.setAdjustMode(true);  //ture 即标题平分屏幕宽度的模式
commonNavigator.setAdapter(new CommonNavigatorAdapter() {
    @Override
    public int getCount() {
        return titles.length;
    }

    @Override
    public IPagerTitleView getTitleView(Context context, final int i) {
        ColorTransitionPagerTitleView colorTransitionPagerTitleView = new ColorTransitionPagerTitleView(context);
        colorTransitionPagerTitleView.setText(titles[i]);
		//选中时字体颜色
        colorTransitionPagerTitleView.setSelectedColor(getResources().getColor(R.color.black_2826));
		//未选中时字体颜色
        colorTransitionPagerTitleView.setNormalColor(getResources().getColor(R.color.gray_71));
        colorTransitionPagerTitleView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {        
				containerHelper.handlePageSelected(i);     //移动指示器
                mViewPager.setCurrentItem(i);
            }
        });
        return colorTransitionPagerTitleView;
    }

    @Override
    public IPagerIndicator getIndicator(Context context) {
        LinePagerIndicator indicator = new LinePagerIndicator(context);
        indicator.setMode(LinePagerIndicator.MODE_WRAP_CONTENT);
        indicator.setColors(black);//指示器的颜色
        return indicator;
    }
});
tlTitles.setNavigator(commonNavigator);
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