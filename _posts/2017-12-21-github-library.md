---
layout: post
title: '发布github开源库'
date: 2017-12-21
categories: Github
tags:  Github Android
---

## AS 第三方库的使用 
这里先说一下AS 第三方库的使用，再说github开源库的使用，相信很多人都和我一样不太明白这里面具体的细节，在Android Studio中引入一个library到你的项目，你只需添加如下的一行代码到模块的build.gradle文件中即可：

    dependencies {
            compile 'com.github.Yzdream:InitFramework:v1.0'
    }


就加这么一行代码就可以使用library库了，非常简单，那么Android Studio又是怎么引用我们导入的库的呢！

### Android Studio使用github上的第三方库
我们新建一个项目之后，在Project的build.gradle中有这么一段代码：


    allprojects {
            repositories {
            google()
            jcenter()  
        }
    }



Android Studio正是从这里配置的仓库服务器上下载library的，Apache Maven是Apache开发的一个工具，提供了用于贡献library的文件服务器。我查看资料发现一般都是两个标准的Android library文件服务器：jcenter 和 Maven Central，而AS 3.0之后 都有了google()，不过我们都是使用一个免费的仓库服务器,JitPack,这里就需要定义jitpack的服务器地址：

    allprojects {
        repositories {
            google()
            jcenter()
            maven { url 'https://jitpack.io' }
        }
    }

## 发布GitHub开源库

 - 首先你得将你的项目变成Libaray类型的,然后发布到GitHub上.这里我就不多说怎么发布到github上了.
 - 打开你的GitHub项目,生成Release文件
 
 ![这里写图片描述](http://img.blog.csdn.net/20171221144523551?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzI5Mzg0ODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
点击之后就可以创建提交了

![创建Release](http://img.blog.csdn.net/20171221144722282?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzI5Mzg0ODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

输入版本信息之后提交即可

![输入版本信息](http://img.blog.csdn.net/20171221144846468?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzI5Mzg0ODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


完成之后打开 [JitPack](https://jitpack.io/) ,然后输入你上传的github项目地址

![这里写图片描述](http://img.blog.csdn.net/20171221145311363?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzI5Mzg0ODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171221145419747?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzI5Mzg0ODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

到这里一个GitHub开源库就发布成功了，只需要在我们的项目中添加依赖就能使用了，当然也可以到github上将项目下载下来作为Module添加到项目使用，如果你不需要改源码的话建议直接添加依赖使用。[CSDN博客 版本](http://blog.csdn.net/qq_32938483/article/details/78863862)


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

